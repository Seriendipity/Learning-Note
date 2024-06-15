
# 视图（外模式）

​	视图是一个或多个关系操作时产生的 **动态结果**。是一个不存放在数据库中的 **虚表** , 而只是把他的定义存放在 **数据字典** 里。只有在请求的时候，才显示视图中的数据。

**所有的视图都是外模式！！！**

> **视图分解**：任何对视图的操作都会被视图分解转化为对基本交的操作。
>
> **视图物化**：当基本表更新的时候，视图被存放在一个临时表里。

## 1. 创建视图

```mysql
CREATE VIEW ViewName[(newColumnName[,...])]
AS
SELECT...
[WITH CHECK OPTION]
```

> 说明：

* newColumnName : 为视图中的属性名。可以省略。如果省略，视图中的属性名就为子查询结果中的属性名。如果存在，那么属性名的个数必须和子查询结果中的属性个数相同。
* 如果列名存在歧义，那么要指明表明。
* WITH CHECK OPTION：如果插入或者更新数据时不满足WHERE子句中的条件，那么拒绝该操作。当使用该子句的时候，不能通过视图插入不满足视图条件的元组。***基本表不受WITH CHECK OPTION的影响。***
* 创建视图需要具有对子查询中涉及的所有表的查询权限。

```mysql
# 创建一个视图，视图内容为在B003工作的员工
CREATE VIEW ManagerStaff
AS
SELECT *
FROM Staff
WHERE branchNo = 'B003';
```

 	下面介绍 **分组视图**，也叫多表连接视图。即在视图使用了聚合函数，或者进行了多表连接操作。

```mysql
# 创建一个视图，视图内容为每个公司的每个员工所经手的房产数
CREATE VIEW StaffPropCnt(branchNo,staffNo,cnt)
AS
SELECT s.branchNo,s.staffNo,COUNT(*) as cnt
FROM staff s ,Property p
WHERE s.staffNo = p.staffNo
GROUP BY s.branchNo,s.staffNo
```

## 2. 删除视图

```mysql
DROP VIEW ViewName[RESTRICT|CASCADE]
```

> 说明：

* 默认为RESTRICT。如果有其他视图基于该视图，那么不允许删除该视图。
* CASCADE：使用CASCADE，删除该视图的同时，删除所有基于该视图的视图。

## 3. 查询视图

​	在用户的角度来看，从视图中查找数据和在基本表中查找数据是一样的。

​	但是实际上，对视图的操作和对基本表的操作是不一样的。

以上述分组视图的例子为例：

```mysql
# 创建一个视图，视图内容为每个公司的每个员工所经手的房产数
CREATE VIEW StaffPropCnt(branchNo,staffNo,cnt)
AS
SELECT s.branchNo,s.staffNo,COUNT(*) as cnt
FROM staff s ,Property p
WHERE s.staffNo = p.staffNo
GROUP BY s.branchNo,s.staffNo
```

下面执行一个对视图的查询

```mysql
SELECT staffNo,cnt
FROM StaffPropCnt
WHERE branchNo = 'B003'
ORDER BY staffNo;
```

那么经过视图分解，一共分为以下5步。

1. 查询视图语句中的SELECT后面的参数转化为定义视图时子查询中的参数。

```mysql
SELECT staffNo,cnt
#<-------转换为------->
SELECT s.branchNo,COUNT(*) 
```

2. 查询视图语句中的FROM后面的视图名转化为定义视图时子查询的FROM后的表名。

```mysql
FROM StaffPropCnt
#<-------转换为-------->
FROM staff s , Property p
```

3. 查询视图语句中WHERE的条件使用AND连接，拼接到定义视图时WHERE条件的后面。

```mysql
WHERE branchNo = 'B003'
#<-------转换为-------->
WHERE s.staffNo = p.staffNo AND branchNo = 'B003'
```

4. 定义视图时的GROUP BY子句和HAVING子句拼接到查询视图语句后面

```mysql
GROUP BY s.branchNo,s.staffNo
```

5. ORDER BY子句中的属性名转化为定义查询语句中的列名

```mysql
ORDER BY staffNo
#<-------转换为--------->
ORDER BY s.staffNo
```

**最终经过视图分解之后的语句为：**

```mysql
SELECT staffNo,cnt
FROM StaffPropCnt
WHERE branchNo = 'B003'
ORDER BY staffNo;

#<--------转换为--------->

SELECT s.staffNo,COUNT(*)
FROM Staff s , Property p
WHERE s.staffNo = p.staffNo AND branchNo = 'B003'
GROUP BY s.branchNo,s.staffNo
ORDER BY s.staffNo
```

然而，并不是所有视图都能够进行视图分解。以下是一些限制

* 如果视图中的属性是基于聚合函数而来

> 那么在查询该视图时，该属性只能出现在 SELECT 和 ORDER BY子句中
>
> 在查询该视图的时候，该属性不能出现在WHERE和聚合函数内部。
>
> ```mysql
> # 如果我们执行下面的操作，编译器不会执行
> SELECT COUNT(cnt)
> FROM StaffPropCnt
> 
> #下面的操作也不会执行
> SELECT *
> FROM StaffPropCnt
> WHERE cnt > 2
> ```
>
> 如果视图为分组视图，那么不能与其他视图或者基本表进行 JOIN 操作

## 4. 更新视图

​	在基本表上的数据更新操作会反映到基于该基本表的视图上。同理，我们也希望在视图上进行更新操作的时候，数据库基本表也会有所反应。

​	然而，并不是所有的视图都是可更新视图。下面举个栗子：

还是以StaffPropCnt视图为例。

```mysql
# 如果我们执行该操作
INSERT INTO StaffPropCnt
VALUES ('B003','SG5',2)
```

​	上面的语句会执行失败。上面的语句为。在B003工作的SG5员工经手了两套房产。但是我们只知道经手了两套房产，但是并没有该房产的信息。插入Property表的时候，主键为空，不满足实体完整性。

那么如果我们换一个视图：

```mysql
CREATE VIEW StaffPropList(branchNo,StaffNo,propertyNo)
AS
SELECT s.branchNo,s.staffNo,p.propertyNo
FROM Staff s , Property p
WHERE s.staffNo = p.staffNo

#仍然执行插入操作
INSERT INTO StaffPropList
VALUES ('B003','SG5','PG19')
```

​	上面的语句仍然有可能执行失败。因为向Property表中插入一个PG19的元组。我们对于这个元组只知道这一个主键的值，其他的值都是NULL。如果其他属性有不允许取空值的属性，那么插入就会失败。

​	通过上面两个例子，我们可以看到，并非所有的视图都是可更新的。

> ***ISO 标准给出了可更新视图的标准：***
>
> 1. 视图中的属性名 **不能使用DISTINCT**
> 2. 若视图中的字段来自 **表达式** 或者 **常量** ，那么不允许对该视图执行INSERT和UPDATE操作。**允许执行DELETE操作**。
> 3. 若视图中的字段来自 **聚合函数** ，那么 **不允许进行UPDATE操作**。
> 4. 若视图是由 **两个及以上基本表导出** 的，那么不允许更新
> 5. 视图中不允许 **包含相关子查询**。可以包含不相关子查询。
> 6. 若视图定义中 **含有HAVING和GROUP BY子句** ，那么不允许更新。

---

## 5. 视图物化

​	视图物化在第一次视图被执行之后，将视图查询的结果存储到一个临时表当中。

**优点** ：每次在该物化的视图上进行查询的时候，速度比每次都执行视图查询快。

**缺点** ：如果后续更新了基本表或者视图，需要重新进行视图物化。



