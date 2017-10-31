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

大文本字段，对应数据库中的 text 类型，在 Django 中使用 `Textarea` 表单组件。该字段的 `max_length` 参数不是必须声明的，如果设置了 `max_length`，其限制并不会在 model 或者数据库层面提现，而只会在页面的表单中体现。

**示例：**

```
from django.db import models

class Article(models.Model):
    description = models.TextField(max_length=10000, verbose_name='描述')
    ....
```

#### CommaSeparatedIntegerField

#### DateField

#### DateTimeField

#### DecimalField

#### DurationField

#### EmailField

#### FloatField

#### GenericIPAddressField

#### SlugField

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



