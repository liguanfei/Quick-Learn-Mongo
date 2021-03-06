# NoSQL和SQL的对比

### SQL和NoSQL的区别

#### 1. 存储方式

​		SQL数据存在特定结构的表中；而NoSQL则更加灵活和可扩展，存储方式可以省是JSON文档、哈希表或者其他方式。SQL通常以数据库表形式存储数据。例：

**SQL**

| 学号 | 姓名 | 电话        |
| ---- | ---- | ----------- |
| 001  | 张三 | 15922334455 |
| 002  | 李四 | 13666778899 |

而NoSQL存储方式比较灵活，比如使用类JSON文件存储

**NoSQL**

```powershell
{
	id: 001,
	name: "zhangsan",
	phone: 15922334455
},
{
	id: 002,
	name: "lisi",
	phone: 13666778899
}
```

#### 2. 数据关系

​		在SQL中，必须定义好表和字段结构后才能添加数据，例如定义表的主键(primary key)，索引(index),触发器(trigger),存储过程(stored procedure)等。表结构可以在被定义之后更新，但是如果有比较大的结构变更的话就会变得比较复杂。在NoSQL中，数据可以在任何时候任何地方添加，不需要先定义表。例：新建`users`集合并添加数据

```powershell
db.users.insert({
	name: "zhangsan",
	phone: 15922334455
})
```

​		NoSQL也可以在数据集中建立索引。如MongoDB，会自动在数据集合创建后创建唯一值`_id`字段，这样的话就可以在数据集创建后增加索引。

​		从这点来看，NoSQL可能更加适合初始化数据还不明确或者未定的项目中。

#### 3. 关联数据存储

​		SQL中如何需要增加外部关联数据的话，规范化做法是在原表中增加一个外键，关联外部数据表。例如：

在用户表中添加班级信息，在用户表中添加一个外键，外键对应的是另一个班级表的主键：

| 学号 | 姓名 | 电话        | 班级编号 |
| ---- | ---- | ----------- | -------- |
| 001  | 张三 | 15922334455 | 01       |
| 002  | 李四 | 13666778899 | 02       |

| 班级编号 | 班级名称 |
| -------- | -------- |
| 01       | 一班     |
| 02       | 二班     |

如果使用NoSQL，例如MongoDB，可以将班级信息直接内嵌到原数据集中，提高查询效率：

```powershell
{
	id: 001,
	name: "zhangsan",
	phone: 15922334455,
	class: {
		class_id: 01,
		class_name: "一班"
	}
},
{
	id: 002,
	name: "lisi",
	phone: 13666778899,
	class: {
		class_id: 02,
		class_name: "二班"
	}
}
```

#### 4. SQL中的join操作

​		SQL中可以使用JOIN表链接方式将多个关系数据表中的数据用一条简单的查询语句查询出来。NoSQL暂不支持join操作，但是MongoDB可以使用`$lookup`来实现链接查询的操作。

#### 5. 数据耦合性

​		SQL中不允许删除已经被引用的外部数据，例如用户表中的班级编号字段，用户表中引用了该条数据，则班级表中对应的01班级信息不能够被删除，以保证数据的完整性。而NoSQL中没有这种强耦合的性质，可以随时删除数据。

#### 6. 事务

​		SQL中如果多张表数据需要同批次被更新，即如果其中一张表更新失败的话其他表也不能更新成功。这种场景可以通过事务来控制，可以在所有命令完成后再统一提交事务。而NoSQL中每一个数据集的操作都是原子级的。

#### 7. 查询性能

​		在相同水平的系统设计的前提下，因为NoSQL中省略了JOIN查询的消耗，故理论上性能上是优于SQL的。

