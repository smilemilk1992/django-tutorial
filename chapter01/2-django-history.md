# Django 历史

在具体使用 Django 编码之前，我想先介绍下 Django 的历史背景，了解 Django 诞生的背景和发展历史有助于我们理解 Django 框架的设计模式和理念。

Django 项目诞生于 2003 年秋天，最初由 Lawrence Journal-World 报纸的 程序员 Adrian Holovaty 和 Simon Willison 用 Python 来编写程序。

当时他们所在的 World Online 小组制作并维护当地的几个新闻站点，并在以新闻界特有的快节奏开发环境中逐渐发展。 这些站点包括有 [LJWorld.com](http://ljworld.com/)、[Lawrence.com](http://lawrence.com/) 和 [KUsports.com](http://kusports.com/)， 记者（或管理层） 要求增加的特征或整个程序都能在计划时间内快速的被建立，这些时间通常只有几天 或几个小时。 因此，Adrian 和 Simon 开发了一种节省时间的网络程序开发框架， 这是在截止时间前能完成程序的唯一途径。

2005 年的夏天，当这个框架开发完成时，它已经用来制作了很多个 World Online 的站点。 当时 World Online 小组中的 Jacob Kaplan-Moss 决定把这个框架发布为一个开源软件。 他们在 2005 年的 7 月发布并取名为 Django，名字来源于一个著名的爵士乐吉他演奏家 [Django Reinhardt](https://baike.baidu.com/item/Django Reinhardt/9447083?fr=aladdin)。

从今往后数年，Django 成长成为了一个有着数以万计的用户和贡献者，在世界广泛传播的完善开源项目。2008 年 9 月的时候，他们发布了 Django 的第一个正式版本1.0， 原来的World Online的两个开发者（Adrian and Jacob）仍然掌握着Django，但是其发展方向受社区团队的影响更大。

# Django 特点

Django 是应用于 Web 开发的高级动态语言框架，从其本身的发展历史上我们可以看到 Django 框架的某些特点：

### 内容管理系统

上面说过，Django 最初用来搭建一些新闻网站，所以提供了一个非常强大的内容管理系统（Admin Site，具体见后续章节介绍），非常适合一些提供内容的网站。不过，虽然 Django 擅长于内容管理系统，但并不表示 Django 能做到就只有这些，Django 是个非常完善的 Web 框架，Web 开发相关的功能都有提供，后续会具体介绍。

### 开源社区

Django 是一个开源系统（感谢 Jacob Kaplan-Moss 当时明智的决定），这吸引了很多优秀的开发者来完善 Django 框架，同时拥有了活跃的 Django 社区和很多优秀的第三方 Django APP。

除此之外，Django 还有一个非常非常完善的[在线文档](https://docs.djangoproject.com/en/1.11/)，Django 文档远超任何其他 Python Web 框架，你可以在里面找到基本上所有你想要了解的内容。不过，Django 文档提供了英语，法语，日语，韩语等多种版本，就是没有中文的！略显坑爹。。

### ALL IN ONE

Django 的诞生是为了提高开发网站的效率，节省时间，所以 Django 本身大包大揽地实现了很多基础东西，比如 ORM 系统，模板系统等。前期的 Django 版本中，这些功能并不算特别完善，有些地方并不能满足开发者的需求，但是由于这些模块的强耦合性，开发者并不能在 Django 的基础上选择使用第三方ORM 或者模板系统。这也成为让很多开发者诟病 Django 的地方。所以，Django 的最新几个版本在这方面做了不少改进，解决了部分强耦合的模块，让开发者使用其他的模板系统，ORM 系统。

所以，Django 是一个“大包大揽”的 Web 框架，它不像 Flask，Bottle 那样可以让你在开发时有很多选择。这也是 Django 的设计哲学所决定的：DRY（Don't Repeat Yourself），鼓励快速开发！

