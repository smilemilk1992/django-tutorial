# Field 类型

Django Model 支持很多类型的字段类型，包括数据库中基础的整形，字符串，日期，外键等，也包括文件，图片类型。本质上所有的字段类型都继承 Field 类，你也可以在此基础上实现自定义类型的字段。

### 普通类型

#### AutoField

对应数据库中的 Integer 类型，根据数据库表中的 ID 自动增长。默认情况下 Django 会自动在 model 中添加一个叫 `id` 的 AutoField 字段，如果你想修改字段名称，也可以自己显式定义，但需要注意的是：**必须要指定该字段为主键**，不然会报错。

**示例：**

```
from django.db import models

class Article(models.Model):
    aid = models.AutoField(primary_key=True)  # 必须是主键
    title = models.CharField(max_length=100, unique=True)
    ....
```

#### BigAutoField（Django 1.10 新增）

该字段和 AutoField 基本一样，不过 BigAutoField 表示一个64位（8字节）的整数，它的取值范围是 1 ~ 9223372036854775807

**示例：**

```
from django.db import models

class Article(models.Model):
    aid = models.BigAutoField(primary_key=True)  # 必须是主键
    title = models.CharField(max_length=100, unique=True)
    ....
```

#### IntegerField

对应数据库的 Integer 类型，32位（4字节）整数，取值返回是-2147483648 ~ 2147483647

**示例：**

```
from django.db import models

class Article(models.Model):
    views_count = models.IntegerField(default=0)
    comment_count = models.IntegerField(default=0)
    ....
```

#### BigIntegerField

和 IntegerField 基本一样，表示一个 64位（8字节）整数，取值范围是-9223372036854775808 ~ 9223372036854775807

**示例：**

```
from django.db import models

class Article(models.Model):
    views_count = models.BigIntegerField()
    comment_count = models.BigIntegerField()
    ....
```

#### SmallIntegerField

和 IntegerField 基本一样，表示一个 16位（2字节）整数，取值范围是-32768 ~ 32767

**示例：**

```
from django.db import models

class Article(models.Model):
    views_count = models.SmallIntegerField()
    comment_count = models.SmallIntegerField()
    ....
```

#### PositiveIntegerField

和 IntegerField 基本一样，表示一个 32位（4字节）**正整数**，取值范围是0 ~ 2147438647

**示例：**

```
from django.db import models

class Article(models.Model):
    views_count = models.PositiveIntegerField()
    comment_count = models.PositiveIntegerField()
    ....
```

#### PositiveSmallIntegerField

和 IntegerField 基本一样，表示一个 16位（2字节）**正整数**，取值范围是0 ~ 32767

**示例：**

```
from django.db import models

class Article(models.Model):
    views_count = models.PositiveSmallIntegerField()
    comment_count = models.PositiveSmallIntegerField()
    ....
```

#### BinaryField

一个存储二进制原始数据的字段，对应数据库中的 `Blob` 类型。该字段实用性不强，只允许 bytes 赋值操作，同时不支持 QuerySet 等数据库操作。建议尽量不要使用

**示例：**

```
from django.db import models

class Article(models.Model):
    thumbnail = models.BinaryKey(null=True, blank=True)  # 用来存储图片，实际项目中用static files更为合理
    ....
```

#### BooleanField

true & false 字段，对应数据库中的 bool 类型，默认值为None。该字段不接受 null 参数，如果想设置 `null=True`，你需要使用 `NullBooleanField`

**示例：**

```
from django.db import models

class Article(models.Model):
    enable_comment = models.BooleanField(default=True)  # 不能使用 null 参数
    ....
```

#### NullBooleanField

和 `BooleanField` 基本一样，但是允许 null 参数，使用该字段来替代 null=True 的 BooleanField 字段

**示例：**

```
from django.db import models

class Article(models.Model):
    enable_comment = models.NullBooleanField(default=True)
    ....
```

#### CharField

字符串字段，对应数据库中的 `varchar` 类型，声明时必须指定 `max_length` 参数，声明字段的最大长度

**示例：**

```
from django.db import models

class Article(models.Model):
    title = models.CharField(max_length=100, unique=True)
    .....
```

#### TextField

大文本字段，对应数据库中的 `text` 类型，在 Django 中使用 `Textarea` 表单组件。该字段的 `max_length` 参数不是必须声明的，如果设置了 `max_length`，其限制并不会在 model 或者数据库层面提现，而只会在页面的表单中体现。

**示例：**

```
from django.db import models

class Article(models.Model):
    description = models.TextField(max_length=10000, verbose_name='描述')
    ....
```

#### CommaSeparatedIntegerField

本身是一种特殊的 `CharField` 类型，以逗号分隔的方式存储整数。对应数据库中的 `varchar` 类型，声明时必须指定 `max_length` 参数，声明字段的最大长度。

**示例：**

```
from django.db import models

class Article(models.Model):
    tags = models.CommaSeparatedIntegerField(max_length=100, verbose_name='标签')
    ....
```

#### DateField

日期字段，使用 Python 中的 `datetime.date` 实例表示，对应数据库中的 `date` 类型。使用时比其他字段多了几个特殊设置参数：

`DateField.auto_now`：每次保存对象时，自动设置该字段为当前日期。

`DateField.auto_now_add`：当对象第一次被创建时自动设置为当前日期。

**几点需要注意的地方：**

1、`auto_now_add`，`auto_now` 和 `default` 设置参数相互是排斥的，同时设置会导致错误的结果；

2、当设置 `auto_now` 或者 `auto_now_add` 为 True 的时候，默认会为这个字段添加两个额外的设置项：`editable = False` 和 `blank = True`

3、如果你设置了 `auto_now` 或者 `auto_now_add`，则在对象被创建或者被修改的时候，会使用默认时区的日期更新对象。如果这不是你所期望的，那你可能需要通过其他方式实现你的需求，比如重写 `save` 方法等。

**示例：**

```
from django.db import models

class Article(models.Model):
    publish_date = models.DateField(auto_now_add=True, verbose_name='发布日期')
    ....
```

#### DateTimeField

日期和时间字段，使用 Python 中的 datetime.datetime 实例表示，对应数据库中的 `datetime` 类型。设置参数同 `DateField` 一样。

**示例：**

```
from django.db import models

class Article(models.Model):
    created_time = models.DateTimeField(auto_now_add=True, verbose_name='创建时间')
    updated_time = models.DateTimeField(auto_now=True, verbose_name='更新时间')
    ....
```

#### DecimalField

十进制浮点数字段，精准数据类型，使用 Python 中的 `Decimal` 实例表示，对应数据库中的 `decimal` 类型。该字段类型有两个**必须**提供的设置参数：

`DecimalField.max_digits`：字段的位数总数，包括小数点后的位数。必须大于或等于 `decimal_places`。

`DecimalField.decimal_places`：小数点后的数字位数。

**示例：**

```
# 存储最大为9999，小数点后保留2位
models.DecimalField(max_digits=6, decimal_places=2)
# 存储最大为10亿，小数点后保留10位
models.DecimalField(max_digits=19, decimal_places=10)
```

#### DurationField（Django 1.8 新增）

用来存储一段时间的字段，使用 Python 中的 `timedelta` 实现。实际代码中没有用过，感觉较为鸡肋。

#### EmailField

邮箱字段，继承自 `CharField` 类型，只是在对象存储时会检查输入内容是否是合法的邮箱地址。`max_length` 默认为254。

**示例：**

```
from django.db import models

class Article(models.Model):
    author_email = models.EmailField(verbose_name='作者邮箱')
    ....
```

#### FloatField

浮点数字段，使用 Python 中的 float 实例表示，对应数据库中的 double 类型。

**示例：**

```
from django.db import models

class Article(models.Model):
    rate = models.FloatField(verbose_name='评分')
    ....
```

#### GenericIPAddressField

IPv4 和 IPv6 地址字段，字符串格式如：192.168.0.1 或者 3b13:51fa::4。额外的设置参数有：

`GenericIPAddressField.protocol`：限制指定协议的有效输入。接受的值为'both'（默认值），'IPv4'或'IPv6'。匹配不区分大小写。

`GenericIPAddressField.unpack_ipv4`：解析IPv4映射地址如 ::ffff:192.0.2.1.如果启用该选项，则地址将被解析到192.0.2.1.默认为禁用。只有当协议 设置为'both'时才可以使用。

```
from django.db import models

class Article(models.Model):
    author_ip = models.GenericIPAddressField(verbose_name='作者IP')
    ....
```

#### SlugField

继承自 `CharField`类型，Slug 本身是一个新闻术语，通常也被称为短标题（Django果真是起源于新闻界啊）。一个 Slug 只能包含字母、数字、下划线或者连字符。通常它们被用在URL中。

SlugField 默认的 `max_length` 为 50 个字符，当然你也可以像 CharField 一样指定 `max_length` 的长度。

```
from django.db import models

class Article(models.Model):
    short_url = models.SlugField(verbose_name='短链', max_length=100)
    ....
```

#### TimeField

时间字段，使用 Python 中的 `datetime.time` 实现。其声明参数和 `DateField` 一样。

```
from django.db import models

class Article(model.Model):
    generate_time = models.TimeField(auto_now=True)
    ...
```

#### URLField

继承自 `CharField`类型，存储 URL 的字段。接受可选的 `max_length` 参数，如果不指定，默认值为200。

和 `CharField` 不同的是，`URLField` 在实际存储的时候回检验字段内容是否是合法的 URL 地址，非法则不允许保存。

```
from django.db import models

class Article(model.Model):
    detail_url = models.URLField(verbose_name='详情链接', max_length=500)
    ...
```

#### UUIDField

用来储存 UUID 数据的字段，使用 Python 的 UUID Class 实现。在某些特定的数据库中，使用数据库类型 uuid 存储，比如 `PostgreSQL`，在其他数据库中则是使用 chart\(32\) 类型表示。

在某些情况下，你不希望使用整数来作为主键，这时候你可以选择使用 `UUIDField` 字段作为你的数据库表主键，比如：

```
import uuid
from django.db import models

class Article(model.Model):
    id = models.UUIDField(primary_key=True, default=uuid.uuid4, editable=False)
    ...
```

---

### 关系类型

Django 的字段中包含了一些用来表示数据库表之间的关系的字段，主要包括三种关系类型：一对多，一对一，多对多。

#### ForeignKey

一对多关系型字段，声明时通过第一个参数指定关联的 Model，如果需要和自己关联，则需要使用`'self'`，如：

```
from django.db import models

class Category(model.Model):
    creator = models.ForeignKey(User)
    parent = models.ForeignKey('self')
    ...
```

如果你需要关联到一个还未定义的 Model，你可以直接使用 Model 名称，而不是 Model 本身来声明，如：

```
creator = models.ForeignKey('User')
```

如果你需要关联到另外 APP 的 Model，也可以直接使用带有完整标签名的 Model 来声明，如：

```
creator = models.ForeignKey('auth.User')
```

后面两种声明方式可以避免一些“**循环引用**”的问题

ForeignKey 涉及到很多参数，这里一一说明下 \(不包含下面的 Field 参数\)：

on\_delete

limit\_choices\_to

related\_name

to\_field

db\_constraint

swappable

#### ManyToManyField

多对多关系型字段

#### OneToOneField

一对一关系型字段

### 文件类型

#### FileField

文件上传字段，不支持 `primary_key` 和 `unique` 参数

#### FilePathField

#### ImageField

图片文件上传字段，继承自 FileField，该字段会对上传的文件进行校验，确保文件是有效的图片文件。

### 自定义类型

上一章中，Article Model 中的 `UEditorField` 字段并非是 Django 内置的字段类型，而是所谓的自定义字段类型。这里我们以 `UEditorField` 为例，介绍下如何自定义一个 Model Field。

# Field 参数

#### null

#### blank

#### choices

#### db\_column

#### db\_index

#### db\_tablespace

#### default

#### editable

#### error\_messages

#### help\_text

#### primary\_key

#### unique

#### unique\_for\_date

#### unique\_for\_month

#### unique\_for\_year

#### verbose\_name

#### validators



