# Django 管理工具

安装 django 时，安装脚本会自动生成一个 django-admin.py 命令（window命令行下是 django-admin.exe），使用这个命令可以执行创建 django 项目，创建APP等操作。

在命令行输入 django-admin.py 可以查看所有操作命令：

```
Available subcommands:

[django]
    check
    compilemessages
    createcachetable
    dbshell
    diffsettings
    dumpdata
    flush
    inspectdb
    loaddata
    makemessages
    makemigrations
    migrate
    runserver
    sendtestemail
    shell
    showmigrations
    sqlflush
    sqlmigrate
    sqlsequencereset
    squashmigrations
    startapp
    startproject
    test
    testserver
```

# 创建 Django 项目

在命令行输入下面的命令创建第一个 django 项目 GeekBlog：

```
django-admin.py startproject GeekBlog
```

创建之后我们会在当前目录下看到 GeekBlog 目录，这就是创建的 Django Project，项目结构如下：

```
(blog)  xianglong@ubuntu  ~/blog/GeekBlog  tree ./
./
├── GeekBlog
│   ├── __init__.py
│   ├── settings.py    # 项目配置文件
│   ├── urls.py    # 项目所有 URL 的定义文件
│   └── wsgi.py    # WSGI 兼容的 Web 服务器的入口，用来运行 Django 项目
└── manage.py    # 实用的命令行工具，借此可以与 Django 项目进行交互
```

# 创建 Django APP

Django 中有 project（项目）和 app（应用）的概念，我们可以用上述的 `startproject` 创建一个 Django 项目。Django 中 APP 我们可以认为是一个功能模块，这方面和其他 Web 项目不同：Django 将不同的功能模块放在不同的 APP 中，这样做主要是方便代码的复用。

一个 Django project 中可以包含一个或多个 APP，甚至可以不用 APP；但是如果你需要用到 Django Models（模型），就必须创建一个 Django APP，因为 model 的定义必须在一个 APP 中。

Django 本身内建了一些 APP，比如 Admin Site，Auth等，我们可以通过修改 `settings.py` 中的 `INSTALLED_APPS` 配置，快速地添加/删除 Django APP，包括自己创建的，Django 内建的，甚至第三方 Django APPs。

创建 Django APP 的方式有两种，第一种使用 `django-admin.py` 命令：

```
django-admin.py startapp blog
```

或者使用 `manage.py` 命令：

```
python manage.py startapp blog
```

命令运行之后会创建一个 blog 文件夹，里面包含以下文件：

```
(blog)  xianglong@ubuntu  ~/blog/GeekBlog  tree blog
blog
├── admin.py    # admin site 相关代码
├── apps.py    # app 本身配置信息
├── __init__.py
├── migrations    # 数据库更新相关文件
│   └── __init__.py
├── models.py    # 数据库 model 定义
├── tests.py    # 测试用例
└── views.py    # django 视图定义
```

创建 Django APP 之后，APP 并没有安装到项目中，需要手动修改 `settings.py` 中的 `INSTALLED_APPS` 才行：

```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    'blog',
]
```

# 项目管理工具 Django manage.py

在创建了 Django 项目之后，你会看到项目根目录有个 `manage.py` 文件，该文件是 Django 提供的项目管理工具，提供了很多便捷操作命令，上面我们已经提到了其中 `startapp` 命令。我们可以在命令行输入 `python manage.py` 来查看可用的命令：

```
Available subcommands:

[auth]
    changepassword
    createsuperuser

[django]
    check    # 检查指定 APP 中的常见问题，默认检查整个项目
    compilemessages    # 编译翻译 po 文件为 mo 文件
    createcachetable    # 创建缓存表
    dbshell    # 进入项目数据库命令行
    diffsettings    # 检查当前项目 settings.py 和默认 settings.py 文件的区别
    dumpdata    # 导出数据库数据
    flush    # 清除数据库数据
    inspectdb    # 介绍数据库表
    loaddata    # 将数据文件中的数据导入数据库
    makemessages    # 生成翻译文件 *.po
    makemigrations    # 根据 models 的修改生成新的数据库更新文件
    migrate    # 同步数据库修改
    sendtestemail    # 发送测试邮件
    shell    # 进入项目 python 命令行
    showmigrations    # 显示所有的数据库更新
    sqlflush    # 输出 flush命令执行的 SQL 语句
    sqlmigrate    # 输出指定数据库更新文件的 SQL 语句
    sqlsequencereset    # 输出重置数据库主键（Primary Key）的 SQL 语句
    squashmigrations    # 合并部分数据库更新操作
    startapp    # 创建 APP
    startproject    # 创建 project
    test    # 运行测试用例
    testserver    # 使用指定的数据启动 django 测试服务

[sessions]
    clearsessions    # 清除项目session

[staticfiles]
    collectstatic    # 收集静态文件
    findstatic   # 查找静态文件
    runserver    # 运行 django 项目
```

对比之前介绍过的 `django-admin.py` 命令，你会发现可操作命令和 `manage.py` 几乎完全一样，后者多了 `auth`，`sessions` 和 `staticfiles` 等命令。是的，`manage.py` 和 `django-admin.py` 命令一样，只是前者多做了几件事儿：

1. 将当前 Django 项目路径加入到 sys.path 中
2. 设置环境变量 `DJANGO_SETTINGS_MODULE`，可以在命令中获取 Django `settings.py` 中的配置信息

更具体的介绍，可以参看官方文档：[django-admin.py and manage.py](https://docs.djangoproject.com/en/1.11/ref/django-admin/ "django-admin.py and manage.py")。

# 运行 Django 项目

开发环境运行 Django 项目非常简单，使用上述的 `manage.py` 命令即可：

```
python manage.py runserver    # 不指定 IP 和端口，默认为 127.0.0.1:8000
python manage.py runserver 8080    # 指定端口为8080
python manage.py 0.0.0.0:8080   # 指定 IP 和端口
```



