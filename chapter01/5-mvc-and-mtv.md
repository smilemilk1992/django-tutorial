# MVC模式

接下来，先介绍一下MVC框架。M：model, V：View，C：control，model是系统与数据库的交互接口层，view是系统和用户的交互接口层，control是控制层，负责数据的中间处理部分。Django的MVC框架有以下含义：

M：数据存储部分。由Django数据库层进行处理。

V：选择哪些数据要显示以及怎样显示，由视图和模板处理。

C：根据用户的输入委派视图的部分，即由Django的URLconf的设置来决定调用视图中的哪个函数来响应。

由于Control的部分由框架自行处理，Django也经常被认为是MTV模式。T为template,即模板，负责页面的展示或其他格式文档的展示；V是view，负责模型的调取以及恰当调取模板的相关逻辑。

# MTV模式



