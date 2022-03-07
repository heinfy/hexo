---
title: moogoDB入门一
abbrlink: 24755
date: 2020-07-08 18:25:03
categories:
  - 数据库
tags:
  - MongoDB
---

<!-- more -->

1.[MONGODB 入门篇](https://www.cnblogs.com/clsn/p/8214194.html#auto_id_0)

2.[centos 安装 mongodb 4.x 及配置用户名密码（官方推荐的方式）](https://cloud.tencent.com/developer/article/1455000)

3.[MONGODB 手册](https://docs.mongodb.com/manual/)

4.[Express 教程 3：使用数据库 (Mongoose)](https://developer.mozilla.org/zh-CN/docs/Learn/Server-side/Express_Nodejs/mongoose)

5.[菜鸟教程](https://www.runoob.com/mongodb/mongodb-tutorial.html)

## 基本概念

### NoSQL

NoSQL(NoSQL = Not Only SQL )，意即"不仅仅是 SQL"。

NoSQL，指的是非关系型的数据库。NoSQL 有时也称作 Not Only SQL 的缩写，是对不同于传统的关系型数据库的数据库管理系统的统称。

NoSQL 用于超大规模数据的存储。（例如个人信息，社交网络，地理位置，用户生成的数据和用户操作日志）。这些类型的数据存储不需要固定的模式，无需多余操作就可以横向扩展。

### 基本术语

| SQL 术语/概念 | MongoDB 术语/概念 | 解释/说明                              |
| :------------ | :---------------- | :------------------------------------- |
| database      | database          | 数据库                                 |
| table         | collection        | 数据库表/集合                          |
| row           | document          | 数据记录行/文档                        |
| column        | field             | 数据字段/域                            |
| index         | index             | 索引                                   |
| table joins   |                   | 表连接,MongoDB 不支持                  |
| primary key   | primary key       | 主键,MongoDB 自动将\_id 字段设置为主键 |

### 数据库

一个 mongodb 中可以建立多个数据库。

数据库命名规范：

- 不能是空字符串（"")。
- 不得含有' '（空格)、.、$、/、\和\0 (空字符)。
- 应全部小写。
- 最多 64 字节。

数据库保留字：`admin`、`local`、`config`

### 文档

RDBMS 与 MongoDB 对应的术语：

| RDBMS  | MongoDB                            |
| :----- | :--------------------------------- |
| 数据库 | 数据库                             |
| 表格   | 集合                               |
| 行     | 文档                               |
| 列     | 字段                               |
| 表联合 | 嵌入文档                           |
| 主键   | 主键 (MongoDB 提供了 key 为 \_id ) |

## 数据库基本操作

- MongoDB 连接

```bash
mongodb://[username:password@]host1[:port1][,host2[:port2],...[,hostN[:portN]]][/[database][?options]]

# mongodb://admin:123456@localhost/test
```

### CRUD

```bash
# 展示数据库
show dbs
# 创建或进入数据库（如果该数据库中没有数据，将不会 show dbs 中子展示出来）
use mydb
# 向mydb的mycol集合中插入一条数据
db.mycol.insert({"name": "houfei"})
# MongoDB 中默认的数据库为 test，如果你没有创建新的数据库，集合将存放在 test 数据库中。
```

- [创建/删除集合](https://www.runoob.com/mongodb/mongodb-create-collection.html)

`db.createCollection(name, options)`

```bash
# 创建或进入数据库（如果该数据库中没有集合时，将不会 show dbs 中子展示出来）
use mydb
# 创建mycol集合
db.createCollection("mycol")
# 在 MongoDB 中，你不需要创建集合。当你插入一些文档时，MongoDB 会自动创建集合。
db.mycol.insert({"name": "houfei"})

# 进入数据库
use mydb
# 查看已存在的集合
show collections
# 删除集合 mycol
db.mycol.drop()
```

- [文档的 CRUD](https://www.runoob.com/mongodb/mongodb-insert.html)

**插入文档**——`db.<collecetion>.insert(document)`

```bash
# 向test数据库中插入stus集合中插入一个新的学生对象
db.stus.insert(
  {name: "小明", gender: "男"}
)

# 向test数据库中插入teather集合中插入2个新的老师对象
db.teather.insert([
  {name: "马云", gender: "男"},
  {name: "马化腾", gender: "男"}
])



# db.stus.insertOne() 插入一个
# db.stus.insertMany() 插入多个
```

**查询文档**——`db.<collection>.find(query（查询条件）, projection（投影）)`

| 操作       |     格式     | 范例                                        | RDBMS 中的类似语句      |
| :--------- | :----------: | :------------------------------------------ | :---------------------- |
| 等于       |    `{:`}     | `db.col.find({"by":"菜鸟教程"}).pretty()`   | `where by = '菜鸟教程'` |
| 小于       | `{:{$lt:}}`  | `db.col.find({"likes":{$lt:50}}).pretty()`  | `where likes < 50`      |
| 小于或等于 | `{:{$lte:}}` | `db.col.find({"likes":{$lte:50}}).pretty()` | `where likes <= 50`     |
| 大于       | `{:{$gt:}}`  | `db.col.find({"likes":{$gt:50}}).pretty()`  | `where likes > 50`      |
| 大于或等于 | `{:{$gte:}}` | `db.col.find({"likes":{$gte:50}}).pretty()` | `where likes >= 50`     |
| 不等于     | `{:{$ne:}}`  | `db.col.find({"likes":{$ne:50}}).pretty()`  | `where likes != 50`     |

```bash
# 查询 “likes” 大于50，并且“by”为“菜鸟教程”或者“title”为“MongoDB 教程”的数据
db.col.find({
	"likes": {$gt:50},
	$or: [{"by": "菜鸟教程"},{"title": "MongoDB 教程"}]
}).pretty()
```

[_案例_](https://docs.mongodb.com/manual/reference/method/db.collection.find/#query-an-array-of-documents)

```bash
# 向 bios 集合插入如下文档
{
    "_id" : <value>,
    "name" : { "first" : <string>, "last" : <string> },       // embedded document
    "birth" : <ISODate>,
    "death" : <ISODate>,
    "contribs" : [ <string>, ... ],                           // Array of Strings
    "awards" : [
        { "award" : <string>, year: <number>, by: <string> }  // Array of embedded documents
        ...
    ]
}

# 全部查询
# 查询所有数据
db.bios.find()

# 标准查询
# 查询_id 为5的数据
db.bios.find( { _id: 5 } )
# 查询 name 对象下的 last 属性值为 Hopper 的所有数据
db.bios.find( { "name.last": "Hopper" } )


# 使用运算符的查询
# 使用$in操作符返回id等于5和ObjectId为(“507c35dd8fada716c89d0013”)的文档
db.bios.find(
   { _id: { $in: [ 5, ObjectId("507c35dd8fada716c89d0013") ] } }
)

# 查询 生日大于 new Date('1950-01-01') 的文档
db.bios.find(
	{ birth: { $gt: new Date('1950-01-01') } }
)

# 查询 name 对象下的 last 属性值符合 /^N/表达式 的所有数据
db.bios.find(
   { "name.last": { $regex: /^N/ } }
)


# 查询范围
# 查询 生日大于 new Date('1940-01-01') 小于 new Date('1960-01-01' 的文档
db.bios.find(
	{ birth: {
		$gt: new Date('1940-01-01'),
		$lt: new Date('1960-01-01')
  } }
)

# 查询 生日大于 new Date('1920-01-01') 并且 death 为 false 的文档
db.bios.find( {
   birth: { $gt: new Date('1920-01-01') },
   death: { $exists: false }
} )


# 查询精确匹配嵌入的文档
# 查询 name 对象为 { first: "Yukihiro", last: "Matsumoto" } 的文档（包括顺序，并且不能包含其他属性）
db.bios.find(
    { name: { first: "Yukihiro", last: "Matsumoto" } }
)

# 嵌入式文档的查询字段
# 其中嵌入的文档名称包含第一个值为“Yukihiro”的字段和最后一个值为“Matsumoto”的字段。
db.bios.find(
   {
     "name.first": "Yukihiro",
     "name.last": "Matsumoto"
   }
)

# 查询数组元素
# 查询数组字段contribs包含元素“UNIX”
db.bios.find( { contribs: "UNIX" } )

# 数组字段设计包含元素“ALGOL”或“Lisp”
db.bios.find( { contribs: { $in: [ "ALGOL", "Lisp" ]} } )

# 数组字段设计包含元素“ALGOL”和“Lisp”
db.bios.find( { contribs: { $all: [ "ALGOL", "Lisp" ] } } )

# contribs的数组大小为4
db.bios.find( { contribs: { $size: 4 } } )
```

[**更新文档**](https://www.runoob.com/mongodb/mongodb-update.html)

```bash
db.collection.update(
   <query>, # update的查询条件，类似sql update查询内where后面的。
   <update>, # 更新内容
   {
     upsert: <boolean>, # 如果不存在update的记录，是否插入objNew,true为插入，默认是false，不插入。
     multi: <boolean>, # 可选，mongodb 默认是false,是否只更新第一条
     writeConcern: <document> # 可选，抛出异常的级别。
   }
)
```

```bash
# db.<collecetion>.update(查询条件, 修改条件)
# - update 会默认将 新对象 替换 旧对象
# - 如果是修改指定的属性，而不是替换，需要使用“替换操作符”来完成修改
#  $set 可以用来修改文档中的指定的属性
#  $unset 可以用来删除文档中的指定的属性


db.stus.update(
  {gender: "女", name: "蜘蛛精" },
  {gender: "男", name: "孙悟空", age: 30 }
)

db.stus.update({name: "one" }, {
  $set: {
    gender: "男",
    name: " 猪八戒",
    addr: "高老庄"
  }
}, {
  multi: true
})
```

[**删除文档**](https://www.runoob.com/mongodb/mongodb-remove.html)

```bash
db.<collecetion>.remove(
  删除条件,
  是否删除一个<true为删除一个，默认false>
)
# 会删除符合条件的所有数据
# 如果传递一个空对象，会把所有数据删除

db.<collecetion>.remove({}) # 性能较差，删除数据，但是不清空集合

db.<collecetion>.deleteOne(删除条件)
db.<collecetion>.deleteMany(删除条件)


show collecetions
db.dropDatabase() # 删除 数据库
db.<collecetion>.drop() # 清空集合
```

### 条件操作符

```bash
$gt > $gte >=  $lt  < $lte <=  $ne !=  $eq  =
```

### $type 操作符

根据字符数据类型查询

```bash
db.col.insert([
	{title: 'PHP 教程'},
	{title: 'Java 教程'},
	{title: 'MongoDB 教程'}
])

# 查询 title 为 string 的文档
db.col.find({"title" : {$type : 2}})
或
db.col.find({"title" : {$type : 'string'}})
```

### Limit 与 Skip 方法

```bash
db.COLLECTION_NAME.find().limit(NUMBER) # 限制查询多少条
db.COLLECTION_NAME.find().skip(NUMBER) # 跳过多少条
```

### sort() 方法

```bash
db.col.find({}).sort({"likes":-1}) # 数据按字段 likes 的降序排列 1为升序 -1为降序
```

### createIndex() 方法

索引存储在一个易于遍历读取的数据集合中，索引是对数据库表中一列或多列的值进行排序的一种结构。

```bash
db.col.createIndex({"title":1})

# 1、查看集合索引
db.col.getIndexes()
# 2、查看集合索引大小
db.col.totalIndexSize()
# 3、删除集合所有索引
db.col.dropIndexes()
# 4、删除集合指定索引
db.col.dropIndex("索引名称")
```
