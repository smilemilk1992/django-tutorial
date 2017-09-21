# Field 类型

Django Model 支持很多类型的字段类型，包括数据库中基础的整形，字符串，日期，外键等，也包括文件，图片类型。本质上所有的字段类型都继承 Field 类，你也可以在此基础上实现自定义类型的字段。

### 普通类型

#### AutoField

#### BigAutoField

#### BigIntegerField

#### BinaryField

#### BooleanField

#### CharField

#### CommaSeparatedIntegerField

#### DateField

#### DateTimeField

#### DecimalField

#### DurationField

#### EmailField

#### FloatField

#### IntegerField

#### GenericIPAddressField

#### NullBooleanField

#### PositiveIntegerField

#### PositiveSmallIntegerField

#### SlugField

#### SmallIntegerField

#### TextField

#### TimeField

#### URLField

#### UUIDField

### 关系类型

#### ForeignKey

#### ManyToManyField

#### OneToOneField

### 文件类型

#### FileField

#### FilePathField

#### ImageField

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



