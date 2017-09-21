# 简介

在现有的 Web 应用中，除了一些静态网站外，大部分都会经常涉及到与数据库的交互：Web 网站在后台连接数据库服务器，从中取出页面需要的数据，然后在页面上展示；或者通过页面提交用户数据，修改数据库数据。

在 Django 中，模型层（Models）定义了和数据库打交道的逻辑。模型是 Django 项目中唯一的数据源，它包含了你所存储数据的必要字段和行为。通过 Django Model，我们可以很方便的操作数据库。为了了解 Django Model 的强大，我们先看下在不使用 Django Model 的时候怎么进行数据库操作。

# 原生数据库查询

在教程中我们使用 MySQL作为 数据库。Python 中一般使用 [MySQL-python](https://pypi.python.org/pypi/MySQL-python/1.2.5) 连接数据库，但是 MySQL-python 并不支持 Python 3.x 版本，所以我们使用 [pymysql](https://pypi.python.org/pypi/PyMySQL/0.7.11) 替换，使用方法和 MySQL-python 基本一致。假如目前我们需要数据库中所有的文章信息，具体代码如下：

    import pymysql
    # 如果想兼容之前 MySQLdb 代码，可以通过下面的代码设置，之后就可以直接使用 import MySQLdb
    # pymysql.install_as_MySQLdb()

    # 连接数据库
    conn = pymysql.connect(host='localhost', user='root', password='123456', db='geekblog')

    try:
        with conn.cursor() as cursor:
            sql = "INSERT INTO `users` (`email`, `password`) VALUES (%s, %s)"
            cursor.execute('SELECT * from blog_article order by created_time desc;')
            for article in cursor.fetchall():
                print(article)
    finally:
        conn.close()

这种方法完全可以实现所有的数据库操作，但是很快你会发现原生 SQL 的局限性和问题：

* **数据库配置**：数据库连接参数放在了具体的视图中，而非 Django settings 文件中，后期变更连接参数需要修改多个地方；
* **代码重复**：每一处需要数据库操作的地方都需要你编写数据库连接，获取游标，执行 SQL，关闭连接的代码，很不优雅；
* **数据库限制**：上面的代码是基于 MySQL 数据库的，如果后期需要更改数据库，那么所有的数据库操作代码基本都要重写。

Django Model 层主要致力于解决这些问题，封装底层的数据库操作，让你通过几步简单的配置实现数据库连接和操作。使用 Django Model 上面的代码可以简化为：

```
from blog.models import Article

articles = Article.objects.all().order_by('-created_time')
for article in articles:
    print(article)
```

下面具体介绍下怎么配置 Django 数据库连接。

# Django 操作数据库

Django 内置支持多种数据库，包括MySQL，SQLite，Oracle 和 PostgreSQL。除此之外，也可以通过第三方 Lib 连接其他类型数据库。这些内容后面会具体介绍，这里主要先以 MySQL 数据库为例，介绍下如何通过 Django 配置 & 连接数据库。

### 设置数据库参数

首先，最基础的一点，你需要在 Django settings 中设置 DATABASES 参数：

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',  # 数据库backend，这里使用 MySQL
        'USER': 'root',  # 数据库用户名
        'PASSWORD': '123456',  # 数据库密码
        'HOST': '127.0.0.1',  # 数据库地址
        'PORT': '3306',  # 数据库端口
        'NAME': 'geekblog',  # 数据库名称
    },
}
```

配置好之后我们需要测试下是否可以正常连接数据库，首先输入 `python manage.py shell` 进入 Django 控制台，然后通过下面的代码测试是否连接成功：

```
>>> from django.db import connections
>>> from django.db.utils import OperationalError
>>> db_conn = connections['default']
>>> try:
...     c = db_conn.cursor()
... except OperationalError:
...     connected = False
... else:
...     connected = True
...
>>> connected
True
```

`connected = True` 表示数据库连接成功。

### 数据库 Model

配置好 Django DATABASES 后可以正常连接数据库，为了能操作数据库中的表，我们需要创建相应的 Django Model，所有创建的 Model 类都继承自 `django.db.models.Model`，对应数据库中的一张表。我们这里以文章（Article）为例：

```
from django.db import models

class Article(models.Model):
    title = models.CharField(max_length=100, unique=True)
    slug = models.CharField(max_length=100, unique=True)
    category = models.ForeignKey(Category, related_name='total_articles', limit_choices_to={'parent__isnull': False})
    status = models.IntegerField(default=0, choices=ARTICLE_STATUS.to_choices())
    enable_comment = models.BooleanField(default=True)
    description = UEditorField(height=200, width=690, verbose_name=_('description'), null=True, blank=True,
                               toolbars='full', image_path="upload/images/", file_path='upload/files/')
    content = UEditorField(height=400, width=690, verbose_name=_('content'), toolbars='full',
                           image_path="upload/images/", file_path='upload/files/')
    mark = models.IntegerField(default=0, choices=ARTICLE_MARKS.to_choices())
    tags = models.ManyToManyField(Tag, blank=True, related_name='total_articles')
    publish_date = models.DateTimeField(auto_now_add=True)
    login_required = models.BooleanField(blank=True, default=False)
    thumbnail = models.ForeignKey(Photo, null=True, blank=True)
    views_count = models.IntegerField(default=0)
    comment_count = models.IntegerField(default=0)
    created_time = models.DateTimeField(auto_now_add=True)
    updated_time = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.title

    class Meta:
        app_label ='blog'
        verbose_name = '文章'
        verbose_name_plural = '文章'
```

上面定义的 Article Model 对应数据库中的 blog\_article 表，而 Article 中的每个字段和数据库表中字段一一对应。定义好 Model 之后我们就可以通过 Model 操作数据库了。

### 操作数据库

数据库操作无非是“增删改查”四种，我们一一举例看看 Django Model 怎么实现数据库操作。

#### 增（Add）

```
import datetime
from blog.models import Article

article = Article(title='test', slug='test', description='test', content='test')
article.save()
```

#### 删（Delete）

```
from blog.models import Article

article = Article.objects.get(id=1)
article.delete()
```

#### 改（Update）

```
from blog.models import Article

article = Article.objects.get(id=1)
article.title = 'test123'
article.save()
```

#### 查（Query）

```
from blog.models import Article

articles = Article.objects.all().order_by('-created_time')
for article in articles:
    print(article)
```

到目前为止，我们已经完整地了解了 Django Model 是如果操作数据库的。后面我们会详细的介绍 Django 的 Model 层内容。

