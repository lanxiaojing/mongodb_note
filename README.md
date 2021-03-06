### mongodb

mongoldb 是面向文档的数据库，不是关系型数据库。放弃关系型以获得更好的扩展性。基本思路是把 原来的行的概念换成文档模型。内嵌文档文档或者数组可以很方便的表述非常复杂的层次关系。

mongodb是无模式的，文档的key不会实现定义也不会固定不变。正是因为没有模式需要更改，通常也不需要迁移大量数据。不必将数据都放到一个模子里，应用层可以处理新增或丢失的key。

mongodb还有一些丰富的功能：	

1. 索引

2. 存储js

   不用使用存储过程，可以直接在服务端存取js的函数和值

3. 聚合

   支持mapReduce和其他聚合工具

4. 固定集合

5. 文件存储

   ​

#### 文档

文档是mongodb的核心概念

key不能以  下划线， $,开头， \0结尾。





#### 集合

集合就是一组文档，一个集合里的文档可以是各式各样的。比如：

{"a": "bda"}

{"foo": "dada", "yos": "gagaga"}

可以是在一个集合里面的。



集合名不能是空字符串，不能含有\0，不能以system.开头，不能含有$。





#### 数据库

多个集合组成数据库。有一些数据库名是保留的，可以直接访问。

- admin

  这是‘root’数据库，要是讲一个用户添加到这个数据库，这个用户自动继承所有数据库权限，一些特定的服务器端命令也只能从这个数据库运行

- local

  这个数据库永远不会被复制，可以用来存限于本地单台服务器的任意集合

- config

  当mongodb用于分片设置时，config数据库在内部使用，用于保存分片的相关信息。







## mongoldb 启动

​	mongod  --dbpath=/data/db/

​	记得添加后面数据库路径

​	mongod在没有参数情况下会使用默认数据目录／data／db，并使用27017端口。如果数据目录不存在或者不可写，服务器就会启动失败。

​	mongod还会启动一个非常基本的http服务器，监听28017端口，所以可以在浏览器访问localhost://28017获取数据库管理信息。



## mongodb shell

​	mongodb自带一个js shell，shell在启动时自动连接mongodb服务器，所以要确保在使用shell前启动mongo。

​	运行 mongo 启动shell：

​	./mongo



### shell 基本操作

### 数据类型

- null

- 布尔

- 32/64位整数（不支持，32位整数自动转换成64位浮点数，64位整数用特殊的内嵌文档显示）

- 64位浮点数

  > js只有一中数字类型，默认情况下，shell中的数字都被mongodb当成是双精度数。
  >
  > 如果要从数据库中获得的是一个32位整数，修改文档，将文档存回数据库时，这个证书被自动转成浮点数。
  >
  > 数字只能表示为双精度的另一个问题是，有些64位的整数并不能精确表示为64位浮点数。在shell中查看会显示一个内嵌文档。比如“int”键设为3时，shell查看会显示：
  >
  > {
  >
  > ​	"int": {
  >
  > ​		"floatApprox": 3
  >
  > ​	}
  >
  > }
  >
  > 要是插入的63位整数不能精确的作为双精度显示，shell会添加两个键：top（表示高32位）和bottom（表示底32位），比如插入9223372036854775807，shell会显示成：
  >
  > {
  >
  > ​	"int": {
  >
  > ​		"floatApprox": 9223372036854776000,
  >
  > ​		"top": 2147483647,
  >
  > ​		"bottom": 4294967295
  >
  > ​	}
  >
  > }
  >
  > ​

- 字符串

- 符号（不支持，转成字符串）

- 对象ID

  - objectId是"_id"的默认类型。"_id"在一个文档中惟一。

  - 12字节的唯一id。每个字节是16进制数。

    ​	90124   ｜  123  ｜  12  ｜  123

    ​	前4个是从标准纪元开始的毫秒数

    ​	中间三个是主机唯一标识。通常是主机名的散列值。

    ​	中间两个是进程标识符pid。

    ​	后四个是计数器

    同一秒钟最多允许每个进程拥有256的3次方个不同objectId。

- 日期

  从标准纪元开始的毫秒数。不存储时区。

- 正则表达式

- 代码

- 二进制数据

- 最大值

- 最小值

- 未定义 undefined

- 数组

- 内嵌文档



### 增删改查

-   创建

    -   insert方法

        1. 创建一个局部变量post， 内容是代表文档的js对象。post = {"title": "test1", "author": "lanjing"}
        2. 用insert方法将其保存到blog集合中。 db.blog.insert(post)

    -   批量插入

    -   插入原理和作用

          执行插入时，

-   读取

-   更新

    -   删除



### 索引

