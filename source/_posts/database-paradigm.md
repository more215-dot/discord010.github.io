---
title: 数据库三大范式与反范式
date: 2020-04-14 15:53:55
tags: [范式]
categories: MySQL
---



在设计合理的关系型数据库时总结出的需遵守的规范要求，这些不同的规范要求被称为不同的范式。可以用以下这句话来说明范式是什么：

> 一张数据表的表结构所符合的某种设计标准的级别

各种范式呈递次规范，也就是说我们需要先符合第一范式，在此基础上再满足更多的规范要求来符合第二范式。

# 第一范式

1NF的定义为：

> 数据库表的每一列都是不可分割的原子数据项

假设如下表结构：

| id    | name | info              |
| ----- | ---- | ----------------- |
| 10001 | 小王 | 年龄 14，身高 140 |

这样的设计就不符合第一范式。需要修改设计为：

| id    | name | age  | height |
| ----- | ---- | ---- | ------ |
| 10001 | 小王 | 14   | 140    |

# 第二范式

2NF的定义为：

> 实体的属性完全依赖于主关键字

依赖如何解释呢？假设Y依赖于X，记为X->Y。那么在X确定的情况下，Y也一定是确定的。

假设如下表结构：

| id    | name | course | teacher |
| ----- | ---- | ------ | ------- |
| 10001 | 小王 | 语文   | 王老师  |
| 10001 | 小王 | 数学   | 陈老师  |
| 10001 | 小王 | 英语   | 张老师  |
| 10002 | 小张 | 语文   | 丁老师  |
| 10002 | 小张 | 数学   | 马老师  |
| 10002 | 小张 | 英语   | 金老师  |

主键为[id, course]

非主属性为：name，teacher

可以得出 [id, course]->teacher; id->name

也就是说name属性部分依赖于主键，那么是不符合第二范式的，需要进行修改：

| id    | name |
| ----- | ---- |
| 10001 | 小王 |
| 10002 | 小张 |

| id    | course | teacher |
| ----- | ------ | ------- |
| 10001 | 语文   | 王老师  |
| 10001 | 数学   | 陈老师  |
| 10001 | 英语   | 张老师  |
| 10002 | 语文   | 丁老师  |
| 10002 | 数学   | 马老师  |
| 10002 | 英语   | 金老师  |

可以得出一个结论：当符合第一范式，且主键为单一字段的时候一定符合第二范式

# 第三范式

3NF的定义为：

> 任何非主属性不依赖其他非主属性

假设如下表结构：

| id    | name | school   | principal |
| ----- | ---- | -------- | --------- |
| 10001 | 小王 | 第一小学 | 陈先生    |
| 10002 | 小陈 | 第一中学 | 陈先生    |

主键为[id]

可以看出：id->name,id->school,id->principal

且 school->principal

则不符合第三范式，需要进行如下修改：

| id    | name | school   |
| ----- | ---- | -------- |
| 10001 | 小王 | 第一小学 |
| 10002 | 小陈 | 第一中学 |

| school   | principal |
| -------- | --------- |
| 第一小学 | 陈先生    |

# 反范式

符合规范的设计可以尽量减少冗余，节省了存储空间，保证了数据的一致性。但是有些应用场景也需要保持适当的冗余，以空间换取查询时间，这样的设计即为反范式。例如以下设计：

|      |      |      |      |
| ---- | ---- | ---- | ---- |
|      |      |      |      |





符合范式的设计在数据查询时需要进行多表的关联，导致查询性能低下。而反范式的设计需要多维护一份冗余数据。需要根据使用的业务场景进行取舍。

# 参考

[知乎-如何理解关系型数据库的常见设计范式](https://www.zhihu.com/question/24696366/answer/29189700 )

