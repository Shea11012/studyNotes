### MySQL 三范式

1. 属性原子性不可分割：

例如：一个用户表存在一个字段班级叫 3 年级 2 班，这是不符合第一范式的。这个字段可以分成 3 年级、2 班

2. 非主键字段和主键直接关联

第二范式需要确保数据库表中的每一列都和主键相关，而不能只与主键的某一部分相关。

例如：

![](assets/68747470733a2f2f7069633030322e636e626c6f67732e636f6d2f696d616765732f323031322f3237303332342f323031323034303131343036333937362e706e67.png)

在这个表中，存在订单信息和用户信息，和订单不是直接相关的。

3. 消除依赖传递

如果某一属性依赖与其他非主键属性，而其他非主键属性又依赖于主键，那么这个属性就是间接依赖于主键，这被称作传递依赖于主属性。

### 反三范式

有时为了提高运行效率，提高读性能，就必须降低范式标准，适当保留冗余数据。具体做法是：在概念数据模型设计时遵守第三范式，降低范式标准的工作放到物理数据模型设计时考虑。降低范式就是增加字段，减少查询时的关联，提高查询效率，因为数据库的操作中查询的比例要远远大于 DML 的比例。但反范式化一定要适度，并且在原本已满足三范式的基础上再做调整。