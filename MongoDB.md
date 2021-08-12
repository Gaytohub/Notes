# MongoDB

## Mac OS 

运行MongoDB : brew services start mongodb-community@4.4 

停止MongoDB : brew services stop mongodb-community@4.4 



| SQL术语/概念 | MongoDB术语/概念 |              解释/说明              |
| :----------: | :--------------: | :---------------------------------: |
|   database   |     database     |               数据库                |
|    table     |    collection    |            数据库表/集合            |
|     row      |     document     |           数据记录行/文档           |
|    column    |      field       |             数据字段/域             |
|    index     |      index       |                索引                 |
| table joins  |                  |        表连接, MongoDB不支持        |
| primary key  |   primary key    | 主键,MongoDB自动将_id字段设置为主键 |

show abs 能够现实所有的数据库列表。

执行 db 命令可以显示当前数据库对象或集合。

运行 use 命令可以连接到一个指定的数据库。



运行MongoDB服务后，可以通过 mongo 命令连接到MongoDB 服务。

![image-20210506101520258](/Users/paranoid/Library/Application Support/typora-user-images/image-20210506101520258.png)



使用 db.stats() 显示当前数据库的状态，包括 数据库名称，集合数量，文档数量，占用空间等信息。

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210506101924960.png" alt="image-20210506101924960" align = " left " style="zoom:50%;" />



使用use + 数据库名称 切换需要操作的数据库。



## 创建数据库

使用use + 数据库名称 （例如 ： use XueFeiyue） 切换到指定的数据库，如果数据库不存在，则创建数据库。

使用 show abs 显示MongoDB中包含的数据库：

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210506102643336.png" alt="image-20210506102643336" align = 'left' style="zoom:50%;" />

此时期望创建的 XueFeiyue 数据库并不会显示（因为我们还没有插入任何数据）



使用 db.XueFeiyue.insert({'name':'薛飞跃'}) 向 XueFeiyue 数据库插入一条数据

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210506102947226.png" alt="image-20210506102947226" align = 'left' style="zoom:50%;" />

可以看到此时 XueFeiyue 出现在数据库列表中。



## 删除数据库

使用 db.dropDatabase() 删除当前数据库，默认是test（可以使用 db 命令，查看当前数据库）。

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210506103913491.png" alt="image-20210506103913491" align = 'left ' style="zoom:50%;" />

以上操作实现了对 XueFeiyue 数据库的删除。



## 创建集合

使用 db.createCollection(name, options) 方法来创建集合。

- name ： 要创建的集合名称
- options ：可选参数，指定有关内存大小及索引的选项



====================================================================================

**没理解**

options 可以是如下的参数：

|  字段  | 类型 |                             描述                             |
| :----: | :--: | :----------------------------------------------------------: |
| capped | 布尔 | （可选）如果为True，则创建固定集合。固定集合是指有着固定大小的集合，当达到最大值时，它会自动覆盖最早的文档。**当该值为True时，必须制定size参数** |
|  size  | 数值 | （可选）为固定集合指定一个最大值，即字节数。**如果capped为True，必须制定该参数** |
|  max   | 数值 |          （可选）指定固定集合中包含的文档的最大数量          |

====================================================================================

在默认的test数据库创建XueFeiyue集合

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210506105957786.png" alt="image-20210506105957786" align = 'left ' style="zoom:50%;" />



带有参数的db.createCollection()用法：

创建固定集合 mycol，整个集合空间为2000000B，文档最大个数为10000个。

![image-20210506111514764](/Users/paranoid/Library/Application Support/typora-user-images/image-20210506111514764.png)



可以不用主动创建集合。当插入一些文档时，MongoDB会自动创建集合。比如：

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210506111708609.png" alt="image-20210506111708609" style="zoom:50%;" />	



## 删除集合

使用 db.collection.drop()方法删除集合。 （collection需要换成等待删除的集合名称）

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210506112153273.png" alt="image-20210506112153273" style="zoom:50%;" />	

成功删除 mycol 集合。



## 插入文档

MongoDB提供两种插入文档的操作。

### 插入单个文档 

使用 db.collection.insertOne(document)，如果插入的主键已经存在，会抛出**DuplicateKeyException**异常，提示主键重复，不保存当前数据。

![image-20210506115610750](/Users/paranoid/Library/Application Support/typora-user-images/image-20210506115610750.png)



### 插入多个文档 

使用 db.collection.insertMany(document)   **如果插入多个文档中，某些文档的主键已经存在，是否会出现异常 ？ **

![image-20210506141010055](/Users/paranoid/Library/Application Support/typora-user-images/image-20210506141010055.png)



```
db.document.insertMany(
	[<document 1>, <document 2>, <document 3> .....],
	{
	    writeConcern : 0 or 1 ,  
	    ordered : <boolean>
	}
)
```

在插入时，还可以通过以上的方式传递 writeConcern 和 ordered 两个参数。

wirteConcern ：**写入策略（还不了解）**

ordered ： 指定是否按照顺序写入，默认为True，按顺序写入。



## 查询文档

首先创建 inventory 集合，并添加数据，SQL代码如下：

```
db.inventory.insertMany([
   { item: "journal", qty: 25, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
   { item: "notebook", qty: 50, size: { h: 8.5, w: 11, uom: "in" }, status: "A" },
   { item: "paper", qty: 100, size: { h: 8.5, w: 11, uom: "in" }, status: "D" },
   { item: "planner", qty: 75, size: { h: 22.85, w: 30, uom: "cm" }, status: "D" },
   { item: "postcard", qty: 45, size: { h: 10, w: 15.25, uom: "cm" }, status: "A" }
]);
```



### 查找集合中的所有文档 

使用 db.collection.find() 与此操作对应的SQL语句为 SELECT * FROM collection

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210506143151636.png" alt="image-20210506143151636" style="zoom:50%;" />



### 查找某相等条件的所有文档

使用 db.collection.find( { <field1>: <value1>, ... } )

实例：

```
db.inventory.find( { status : "D" } )
```

  对应 SQL 中的

```mysql
SELECT * FROM inventory WHERE status = "D"
```

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210506144006854.png" alt="image-20210506144006854" style="zoom:50%;" />	

### 使用查询运算符指定条件

使用 db.collection.find( { <field1>: { <operator1>: <value1> }, ... } )



实例：

```
db.inventory.find( { status : { $in: [ "A", "D" ] } } )
```

 对应 SQL 中的 

```mysql
SELECT * FROM inventory WHERE status in ("A", "D")
```

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210506145437919.png" alt="image-20210506145437919" style="zoom:50%;" />	Notice ： 尽管可以使用 $or 运算符表示此查询，但是在同一字段上执行相等性检查时，请使用 	$in 而不是 $or 



### 指定 AND 条件

实例：

```
db.inventory.find( { status : "A", qty : { $lt : 30 } } ) 
```

对应 SQL 中的 

```mysql
SELECT * FROM inventory WHERE status : "A" AND qty < 30 
```

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210506145924106.png" alt="image-20210506145924106" style="zoom:50%;" />	

### 指定 OR 条件

实例：

```
db.inventory.find( { $or : [ { status : "A"}, {qty : { $lt : 30} } ] } )
```

对应 SQL 中的 

```mysql
SELECT * FROM inventory WHERE status = "A" or qty < 30
```

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210506150511625.png" alt="image-20210506150511625" style="zoom:50%;" />	

### 指定 AND 以及 OR 条件

实例：

```
db.inventory.find( {
		status : "A",
		$or : [ { qty : { $lt : 30} }, { item : /^p/} ]
} )
```

对应 SQL 中的 

```mysql
SELECT * FROM inventory WHERE status = "A" AND ( qty < 30 or item LIKE "p%")
```

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210506151425592.png" alt="image-20210506151425592" style="zoom:50%;" />	



## 更新文档

首先创建 inventory 集合，并添加数据，代码如下：

```
db.inventory.insertMany( [
   { item: "canvas", qty: 100, size: { h: 28, w: 35.5, uom: "cm" }, status: "A" },
   { item: "journal", qty: 25, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
   { item: "mat", qty: 85, size: { h: 27.9, w: 35.5, uom: "cm" }, status: "A" },
   { item: "mousepad", qty: 25, size: { h: 19, w: 22.85, uom: "cm" }, status: "P" },
   { item: "notebook", qty: 50, size: { h: 8.5, w: 11, uom: "in" }, status: "P" },
   { item: "paper", qty: 100, size: { h: 8.5, w: 11, uom: "in" }, status: "D" },
   { item: "planner", qty: 75, size: { h: 22.85, w: 30, uom: "cm" }, status: "D" },
   { item: "postcard", qty: 45, size: { h: 10, w: 15.25, uom: "cm" }, status: "A" },
   { item: "sketchbook", qty: 80, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
   { item: "sketch pad", qty: 95, size: { h: 22.85, w: 30.5, uom: "cm" }, status: "A" }
] );
```



### 更新单个文档 

使用 db.collection.updateOne()

```
db.inventory.updateOne(
   { item: "paper" },
   {
     $set: { "size.uom": "cm", status: "P" },
     $currentDate: { lastModified: true }
   }
)
```

使用上面的命令，可以更新 inventory 这个集合中的第一个 item 为 paper 的文档，将该文档中 size.uom 的值更改为 cm ，status 更改为 P 。

由于文档中不包含 lastModified 字段，则会在该文档中创建该字段并赋值。

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210506160608199.png" alt="image-20210506160608199" style="zoom:50%;" />	

### 更新多个文档

使用 db.collection.updateMany()

```
db.inventory.updateMany(
   { "qty": { $lt: 50 } },
   {
     $set: { "size.uom": "in", status: "P" },
     $currentDate: { lastModified: true }
   }
)
```

使用上面的命令，可以更新 inventory 这个集合中的每一个 qty 小于 50 的文档，将每个文档中 size.uom 的值更改为 cm ，status 更改为 P 。

由于文档中不包含 lastModified 字段，则会在该文档中创建该字段并赋值。

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210506161321749.png" alt="image-20210506161321749" style="zoom:50%;" />	

### 替换一个文档

使用 db.collection.replaceOne() 能够将某个文档，完整的替换为新的文档内容。

```
db.inventory.replaceOne(
   { item: "paper" },
   { item: "paper", instock: [ { warehouse: "A", qty: 60 }, { warehouse: "B", qty: 40 } ] }
)
```

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210506162315913.png" alt="image-20210506162315913" style="zoom:50%;" />	



## 删除文档

首先创建 inventory 集合，并添加数据，代码如下：

```
db.inventory.insertMany( [
   { item: "journal", qty: 25, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
   { item: "notebook", qty: 50, size: { h: 8.5, w: 11, uom: "in" }, status: "P" },
   { item: "paper", qty: 100, size: { h: 8.5, w: 11, uom: "in" }, status: "D" },
   { item: "planner", qty: 75, size: { h: 22.85, w: 30, uom: "cm" }, status: "D" },
   { item: "postcard", qty: 45, size: { h: 10, w: 15.25, uom: "cm" }, status: "A" },
] );
```



### 删除所有文档

使用 db.collection.deleteMany()，传入空的 document 即可。

```
db.inventory.deleteMany({})
```



### 删除所有符合条件的文档

使用 db.collection.deleteMany()，根据需要传入 document 。

```
db.inventory.deleteMany({ status : "A" })
```

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210506163902712.png" alt="image-20210506163902712" style="zoom:50%;" />	

### 仅删除一个符合条件的文档

使用 db.collection.deleteOne()，根据需要传入 document 。

```
db.inventory.deleteOne({ status : "D" })
```



## 查询运算符和投影运算符



### 比较查询运算符

| Name |       Description        |
| :--: | :----------------------: |
| $eq  |    匹配等于指定值的值    |
| $gt  |    匹配大于指定值的值    |
| $gte | 匹配大于或等于指定值的值 |
| $in  |  匹配数组中指定的任何值  |
| $lt  |    匹配小于指定值的值    |
| $lte | 匹配小于或等于指定值的值 |
| $ne  | 匹配所有不等于指定值的值 |
| $nin | 不匹配数组中指定的任何值 |

Note : 使用比较运算符的查询受 Type Bracketing约束。



### 逻辑查询运算符

| Name |                      Description                      |
| :--: | :---------------------------------------------------: |
| $and |  用逻辑AND连接查询子句将返回两个子句都匹配的所有文档  |
| $not | 反转查询表达式的效果，并返回与查询表达式不匹配的文档  |
| $nor | 以逻辑NOR联接查询子句将返回两个子句均不匹配的所有文档 |
| $or  | 以逻辑OR联接查询子句将返回符合任一子句条件的所有文档  |



### 元素查询运算符

|  Name   |          Description           |
| :-----: | :----------------------------: |
| $exists |     匹配具有指定字段的文档     |
|  $type  | 如果字段是指定类型，则选择文档 |



### 评估查询运算符

....



### 地理空间查询运算符

...



### 数组查询运算符

....



### 按位查询运算符

...



### 投影运算符

...



## MongoDB Limit 与 Skip 方法

如果需要在MongoDB中读取指定数量的数据记录，可以使用 MongoDB 中的 Limit 方法，limit() 方法接受一个数字参数 ，该参数指定从MongoDB中读取的记录条数。

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210506174721292.png" alt="image-20210506174721292" style="zoom:50%;" />	

还可以使用 skip 方法，跳过指定数量的记录。



## MongoDB 排序

如果需要在MongoDB中对数据进行排序，可以使用MongoDB 中的 sort() 方法。sort() 方法可以通过参数指定排序的字段，并使用 1 和 -1来指定排序的方式，其中 1 为升序，-1 为降序。

```
db.collection.find().sort({KEY:1})
```

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210506180042902.png" alt="image-20210506180042902" style="zoom:50%;" />	

图片中的命令对 inventory 集合中 status 为 A 的文档进行排序，排序方式为根据 qty 进行降序排序。



## MongoDB 索引

索引是特殊的数据结构，索引存储在一个易于遍历读取的数据集合中，索引是对数据库表中一列或多列的值进行排序的一种数据结构。

MongoDB  使用 createIndex() 方法创建索引。 该方法基本语法格式如下：

```
db.collection.createIndex(keys, options)
其中key值是想要创建的索引字段， options为可选参数。
```



### 单字段索引

#### 在单个字段上创建升序索引

假设存在某个 records 集合，其中文档内容大都如下所示：

```json
{
  "_id": ObjectId("570c04a4ad233577f97dc459"),
  "score": 1034,
  "location": { state: "NY", city: "New York" }
}
```

可以通过如下操作在 records 集合中的 score 字段创建升序索引：

```
db.records.createIndex( { score : 1 } )
```

可以通过如下操作在 records 集合中的 score 字段创建降序索引：

```
db.records.createIndex( { score : -1 } )
```

创建的索引支持在字段 score 上选择的查询，比如：

```
db.record.find( {score : 2} )
db.record.find( {score : { $gt : 10 } } )
```



#### 在嵌入式字段上创建索引

假设存在某个 records 集合，其中文档内容大都如下所示：

```json
{
  "_id": ObjectId("570c04a4ad233577f97dc459"),
  "score": 1034,
  "location": { state: "NY", city: "New York" }
}
```

可以通过如下操作，在 location.state 字段上创建索引：

```
db.records.createIndex( { "location.state" : 1 } )
```

在字段 location.state 上的查询将会通过索引：

```
db.records.find( { "location.state" : "CA" } )
db.records.find( { "location.city" : "Albany"}, {"location.state" ; "NY"} )
```



#### 在嵌入式文档上创建索引

假设存在某个 records 集合，其中文档内容大都如下所示：

```json
{
  "_id": ObjectId("570c04a4ad233577f97dc459"),
  "score": 1034,
  "location": { state: "NY", city: "New York" }
}
```

location 字段是嵌入式文档，包含嵌入式字段 city 和 state 。可以通过如下操作，在 location 字段上创建索引：

```
db.records.createIndex( { location : 1} )
```

以下查询将会使用location字段上的索引：

```
db.records.find( { location : { city : "New York", state : "NY"} } )
```



### 复合索引

#### 创建复合索引

创建复合索引可以使用类似一下原型的操作：

```
db.collection.createIndex( { <field1>: <type>, <field2>: <type2>, ... } )
```

假设存在某个 products 集合，其中文档内容大都如下所示：

```json
{
 "_id": ObjectId("570c04a4ad233577f97dc459"),
 "item": "Banana",
 "category": ["food", "produce", "grocery"],
 "location": "4th Street Store",
 "stock": 4,
 "type": "cases"
}
```

可以通过如下操作，在 item 和 stock 字段上创建索引， item 上升序，stock 上降序。

```
db.products.createIndex( {item : 1, stock : -1 } )
```

复合索引中列出的字段 **顺序** 很重要。建立以上索引，文档会先按照 item 字段的值进行升序排序，并在今通过 item 值无法比较两个文档先后顺序的情况下，根据 stock 字段进行降序排序。

复合索引除了支持在所有索引字段上都匹配的查询之外，还支持在索引字段的前缀上匹配的查询。比如以上建立的索引，支持对 item 字段以及 item 和 stock 字段的查询。

```
db.products.find( { item : "banana"} )
db.products.find( { item : "banana", stock : { $gt : 5 } } )
```



#### 前缀

假设有以下复合索引：

```json
{ "item": 1, "location": 1, "stock": 1 }
```

该索引具有以下索引前缀：

```json
{ "item" : 1}
{ "item" : 1, "location" : 1}
```

对于复合索引，MongoDB可以使用索引来支持对索引前缀的查询。所以，MongoDB可以将索引用于以下字段的查询：

- item 字段
- item 字段和 location 字段
- item 字段和 location 字段 和 stock 字段

MongoDB 可以使用索引来支持对 item 和 stock 字段的查询（查询时 item 走索引， stock 不走索引）。这种情况下虽然走了索引，但是不如仅 在 item 和 stock 上有效。

**索引字段按顺序解析，如果查询省略了特定的索引前缀，则无法使用任何该前缀后的索引字段**



#### 排序

对于单字段索引，字段的排序（1 ：升序， -1 ：降序）无关紧要， 因为**MongoDB可以在任一方向上遍历索引**。但是对于复合索引来说，字段的排序可能对 该索引是否支持排序 有影响。

假设存在某个 events 集合，其中包含具有字段 username 和 date 的文档。现做如下两种查询：

- ​	查询，返回结果按照 username 升序， date 降序

```
db.events.find().sort( { username : 1, date : -1 } )
```

- ​    查询，返回结果按照 username 降序，date 升序

```
db.events.find().sort( { username : -1, date : 1 } )
```

建立以下索引可以支持以上两种查询操作：

```
db.events.createIndex( { "username" : 1, "date" : -1 })
```

但是，上述索引**无法支持通过username升序，date升序的查询**

```
db.events.find().sort( { username: 1, date: 1 } )
```



### 有关索引的常用命令

...



## MongoDB 聚合

聚合操作就是对来自多个（当然也可以是一个）的文档的值分组在一起，并且可以对分组的数据执行各种操作最终返回单个结果。 MongoDB提供了三种执行聚合的方式：聚合管道，...





## MongoDB 副本集

MongoDB 副本集是一组 **mongod**进程，提供了数据的冗余和高可用性。副本集有两个成员：

- 主节点
  - 主节点负责接收所有写操作。
- 从节点
  - 从主数据库复制数据，用来维护相同的数据集。



### 主节点

主节点是副本集中集中接收写操作的唯一成员。 MongoDB在主节点上应用写操作，然后在**主节点oplog**上记录操作，从节点成员复制此日志并将操作应用于其数据集。

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210507172610219.png" alt="image-20210507172610219" style="zoom:50%;" />

副本集的所有成员都可以接受读取操作。但是，默认情况下，应用程序将其读取操作定向到主节点。

副本集最多可以有一个主节点。如果当前主节点不可用，会用过**选举**确定新的主节点。



### 从节点

从节点维护主节点数据集的副本。为了复制数据，从节点在异步进程中将**主节点oplog**的操作应用于自己的数据集。一个副本集可以有一个或多个从节点。

尽管客户无法将数据写入从节点，但是客户端可以从从节点中读取数据。

可以对从节点进行特殊的配置，使其成为：

- 防止其成为选举中的主要对象，从而使其可以驻留在辅助数据中心或充当冷备用数据库。 **优先级0副本集成员**
- 阻止应用程序从中读取数据，这使其可以运行需要与正常流量隔离的应用程序。**隐藏副本集成员**
- 保留运行中的“历史”快照，用于从某些错误（比如意外删除的数据库）中恢复。**延迟副本集成员**





```mysql
CREATE TABLE 获利最高的游戏(
  id serial PRIMARY KEY,
  game_name char(256) NOT NULL,
  tag CHAR(256),
  description TEXT,
  家人共享内容库 CHAR(256),
  更新日期 CHAR(256),
  大小 CHAR(256),
  安装次数 CHAR(256),
  当前版本 CHAR(256),
  Android系统版本要求 CHAR(256),
  内容分级 CHAR(256),
  互动元素 CHAR(256),
  应用内商品 CHAR(256),
  提供者： CHAR(256)
);
```

