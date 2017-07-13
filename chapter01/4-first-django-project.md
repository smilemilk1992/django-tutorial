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
│   ├── __init__.py
│   ├── settings.py    # 项目配置文件
│   ├── urls.py    # 项目所有 URL 的定义文件
│   └── wsgi.py    # WSGI 兼容的 Web 服务器的入口，用来运行 Django 项目
└── manage.py    # 实用的命令行工具，借此可以与 Django 项目进行交互
```

# 创建 Django APP

# 项目管理工具 Django manage.py



