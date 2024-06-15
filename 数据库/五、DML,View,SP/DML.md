
# DML

​	之前我们看到的都是创建表，然后在表中搜索指定的数据。但是我们还没向表中插入数据，因此，DML就是用于插入数据、修改数据和删除数据的。

## 1. INSERT语句

```mysql
INSERT INTO tableName[(columnList)]
VALUES (dataValueList)[(,...)]
```

> 该操作用于向表中插入一行或者多行元组。如果指定了 columnList，那么就向表中插入指定属性列表的属性，其他值取NULL（如果允许取空值）或者DEFAULT（如果设置了默认值）。如果省略了 columnList，那么就要向表中插入建表时所有的属性。
>
> 其中，dataValueList 要匹配 columnList。

举个栗子：

```mysql
# Staff(staffNo,fName,IName,position,sex,DOB,salary,branchNo)
#<---------插入所有属性---------->
INSERT INTO Staff
VALUES ('SG16','Alan','Brown','Assistant','M','1957-05-25','8300','B003'),
#<---------插入指定属性---------->
INSERT INTO Staff(staffNo,fName,IName,position,salary,branchNo)
VALUES('SG44','Anne','Jones','Assistant','8100','B003');
#另一种写法
INSERT INTO Staff
VALUES('SG44','Anne','Jones','Assistant',NULL,NULL,'8100','B003');
```

> INSERT语句也可以插入查询结果。

```mysql
INSERT INTO TableName[(columnList)]
SELECT ...
```

举个栗子：

```mysql
# 查找每个员工管理房产的信息
INSERT INTO StaffPropCount(
	SELECT s.staffNo,fName,IName,COUNT(*)
    FROM Staff s,Property p
    WHERE s.StaffNo = p.StaffNo
    GROUP BY s.staffNo,fName,IName
)
# 如果省略下面的UNION，那么没有管理房产的员工的信息不会出现在结果集。
UNION
(
	SELECT staffNo,fName,IName,0
    FROM staff
    WHERE staffNo NOT IN(
    	SELECT DISTINCT staffNo
        FROM property
    )
)
```

## 2. UPDATE语句

```mysql
UPDATE TableName
SET columnName1 = dataValue1[,columnName2 = dataValue2,...]
[WHERE serchCondition]
```

> 说明：

* TableName 可以是 ***基本表*** ，也可以是 ***可更新视图***。
* SET 指明一个或者多个需要修改的属性值。
* WHERE指明修改条件。如果省略，那么表中的所有数据都被修改。如果存在WHERE，只有满足WHERE条件的元组会被更新。

举个栗子：

```mysql
#给所有员工涨薪3%
UPDATE Staff
SET salary = salary*1.03;

#给经理涨薪3%
UPDATE Staff
SET salary = salary*1.03
WHERE position = 'Manager';

#给在London工作的员工涨薪5%
UPDATE Staff
SET salary = salary * 1.05
WHERE branchNo in (
	SELECT branchNo
    FROM Branch
    WHERE city = 'London'
)

```

***！！！注意 ！！！***

> UPDATE 只能更新一个表的内容，不能更新多个表的内容。

举个栗子：

```mysql
#给在London工作的员工涨薪5%的错误写法
UPDATE Staff,Branch
SET salary = salary * 1.05
WHERE staff.Sno = Branch.Sno AND city = 'London';
```

> 但是，在 Mysql5.0 以后的版本，如果这样写是可以的

```mysql
# 多表更改，改的是Staff表中的数据。
UPDATE Staff
JOIN Branch b ON b.Sno = staff.Sno
SET salary = salary*1.05
WHERE city = 'London';
```

> 如果想要给某个属性赋NULL，要使用 = ，而不能用 IS NULL。也就是说，SET后面不能有IS NULL

```mysql
UPDATE sc
SET Grade = NULL
WHERE Grade < 60;
```

# 3. DELETE语句

```mysql
DELETE FROM TableName[WHERE serchCondition]
```

> 说明：

* TableName 可以是一个 ***基本表*** ，或者是一个 ***可更新视图***。
* WHERE子句是可选的。如果省略，那么把表中的所有数据都删除，但是表结构还在，不等同于DROP。如果存在WHERE子句，那么只删除符合WHERE子句条件的元组。
* **如果我们想要删除一列，可以将其赋值为NULL，那么在形式上等同于被删除。**

```mysql
# 删除View表中的所有数据
DELETE FROM Viewing;

# 删除房产号为‘PG40’的所有看房记录
DELETE FROM Viewing
WHERE propertyNo = 'PG4';
```

***！！！注意！！！***

> ISO标准定义DELETE也只能从一个表中删除数据，不能多表连接删除数据。

```mysql
# 删除所有女同学的选课信息
DELETE FROM Sc
WHERE Sno IN(
	SELECT Sno
    FROM Student
    WHERE sex = 'F'
);

# 错误写法
DELETE FROM Sc,Student
WHERE Sc.Sno = Student.Sno AND sex = 'F';
```

> 但是，在Mysql5.0以后的版本，按照如下方式多表删除是允许的。

```mysql
# 删除刘晨的所有选课信息。删除的是Sc表中的信息。
DELETE Sc
FROM Student,Sc
WHERE Student.Sno = Sc.Sno AND Sname = '刘晨';
```

