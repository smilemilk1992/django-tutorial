# Django管理工具

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

# 创建Django项目

在命令行输入下面的命令创建第一个 django 项目 GeekBlog：

```
django-admin.py startproject GeekBlog
```



