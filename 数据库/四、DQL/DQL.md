# DQL

## Select语句

```mysql
SELECT [DISTINCT|ALL] 
{*|[columnExpression[AS newName]][,...]}
FROM TableName[alias][,...]
[WHERE condition]
[GROUP BY columnList]
[HAVING condition]
[ORDER BY columnList]
```

> 选择语句由这六个部分组成，每一部分的顺序固定，顺序不能改变。

说明：

1. From ：指明从哪个表或者哪些表来查找信息
2. DISTINCT：表明返回的信息是彼此不相同的行。
3. *：返回从指定表中搜索的所有信息。
4. WHERE：表明选择条件。只有满足WHERE条件的元组被返回。
5. GROUP BY：表明分组，返回制定分组。
6. HAVING：对组的限制条件，只有满足HAVING的条件的组能被返回。
7. ORDER BY：对查找到的元组按照某个属性进行排序。排序后的表是虚表，不保存在数据库中。

举个栗子：

```mysql
# 从Staff表中查找员工号和姓名
SELECT staffNo,fName
FROM Staff;
# 返回Staff表中的所有信息
SELECT *
FROM Staff
```

> 注意，使用DISTINCT的时候，若SELECT后面有多个属性，DISTINCT必须放在所有属性前面。

```mysql
# 正确写法
SELECT DISTINCT Sno,Cno
From sc;

#错误写法
SELECT Sno，DISTINCT Cno
From sc;
```

​	此外，在使用SELECT的时候，也可以使用派生属性。即通过数据库中的属性计算而得到的属性，该属性作为“虚”列出现。

```mysql
# 其中salary/12为一个派生属性，由数据库中的属性计算而来
# as为一个关键字，可以省略，as后的为派生属性的别名
SELECT staffNo,fName,Iname,salary/12 as monthlySalary
FROM Staff
```

## 1. Where子句

> 条件：
>
> 1. =，< , > , != , <= , >=
>
> 2. AND , OR , NOT.【NOT > AND > OR , （）的优先级最高】
> 3. BETWEEN...AND ; NOT BETWEEN ... END;【Between...and 包括边界，not between... and 不包括边界】(这两种表示可以由1,2组合进行表示)
> 4. IN , NOT IN 【元素属于集合】(这种表达方式可以用1,2组合来进行表示)
> 5. LIKE , NOT LIKE【字符串模式匹配】
> 6. NULL , IS NULL【空值测试】

```mysql
#<---------1,2--------->
#列出办公室在London或Glassgow的公司信息
SELECT *
FROM Branch
WHERE city = 'London' OR city = 'Glasgow';
#<---------3------------>
#列出薪水在20000-30000的员工信息
SELECT staffNo,fName，IName,position,salary
FROM Staff
WHERE salary BETWEEN 20000 AND 30000

# 上面的写法<==> 下面的写法【带边界】
SELECT staffNo,fName，IName,position,salary
FROM Staff
WHERE salary >= 20000 AND salary <= 30000

#列出薪水在不在20000-30000的员工信息
SELECT staffNo,fName，IName,position,salary
FROM Staff
WHERE salary NOT BETWEEN 20000 AND 30000

# 上面的写法<==> 下面的写法【不带边界】
SELECT staffNo,fName，IName,position,salary
FROM Staff
WHERE salary > 20000 OR salary < 30000
#<-----------4-------------->
# 查找职位是'Manager'或者'Supervisor'的员工
SELECT *
FROM Staff
WHERE position IN('Manager','Supervisor')

# 上面的写法<==> 下面的写法
SELECT *
FROM Staff
WHERE position = 'Manager' OR position = 'Supervisor'
```

​	前四种比较简单，直接给出例子即可。下面介绍一下 **模式匹配**。

> 在SQL中，提供了 ***两种模式匹配符，分别是%和_***
>
> %：意味着匹配0个或者多个字符
>
> _ : 意味着匹配任何一个单个的字符。

举个栗子：

```mysql
#<----------5----------->
LIKE '%Glasgow%' #意味着匹配任何一个包含Glasgow的字符串。前边的%表示Glasgow前边匹配0个或多个字符，后面的%表示Glasgow后面匹配0个或多个字符。

# 查找姓王的同学的信息
SELECT Sname，Sno,Ssex
FROM Student
WHERE Sname LIKE '王%';

# 查找姓名为两个字，且第一个字为王的同学信息
SELECT Sname，Sno,Ssex
FROM Student
WHERE Sname LIKE '王_';

# 查找第二个字为丽的所有同学的信息
SELECT *
FROM Student 
WHERE Sname LIKE '_丽%';
```

​	上面涉及查找条件中均没有 \_和%出现，因此可以直接使用。**如果查找条件中出现了\_和%，那么就要使用转义字符**，否则就会产生歧义。编译器不知道这是一个模式匹配符还是要查找的子串。

举个栗子：

```mysql
# 查找课程名为DB_Design的课程
# 这样写是错误的，因为他会匹配九个字符的名为DB[i]Design其中i为任何一个字符的子串
SELECT Cno,Ccredit
FROM Course
WHERE Cname LIKE 'DB_Design';

# 正确写法为
SELECT Cno,Ccredit
FROM Course
WHERE Cname LIKE 'DB\_Design' ESCAPE '\';

```

​	上述正确写法中，使用 **ESCAPE** 的关键字定义转义字符。其中，转义字符可以自定义为任何一个字符。在转义字符之后跟着的字符不被编译器认为是模式匹配符，而是子串中的一个字符。

举个栗子：

```mysql
# 查找以'DB_'开头，且倒数第3个字符为i的课程信息
SELECT *
FROM Course
WHERE Cname LIKE 'DB\_%i_ _' ESCAPE '\';# 严格而言，应该是DB\_%i__，但是两个_连在一起不好区分。
```

> 空值查询

```mysql
#举个栗子
#<-----------6------------>
# 查找成绩为NULL的学生信息
SELECT * 
FROM Sc
WHERE Grade IS NULL;
```

**说明：判断是否为空值要用 IS NULL 而不能用逻辑运算符 “ = ” **。等号为赋值，而不是判断。

## 2. Order子句

​	ORDER BY可以使结果按照一列或者多列进行排序，默认是升序，也可以显示说明为降序。当有NULL值的时候，具体排在哪取决于DBMS。

举个栗子：

```mysql
# 查找Staff表中的所有信息，按照降序进行输出结果
SELECT *
FROM Staff
Order By salary DESC;
# 在上述结果表中，由于没有次排序属性，所以如果有属性相同的元组，那么按照随意顺序进行排序这些具有相同排序属性元组。

#<-----改进后的排序------->
SELECT *
FROM Staff
ORDER BY salary DESC,position DESC;
# 首先按照salary降序排序，如果salary相同，对于具有相同salary的元组，再按照position进行降序排序。
```

## 3. 聚合函数

> ISO 标准定义了以下五个聚合函数。

* COUNT：返回指定列的值的数量。
* SUM：返回指定列的值的和
* AVG：返回指定列的值的平均值
* MIN：返回指定列的最小值
* MAX：返回指定列的最大值

说明：

> 1. 上述每一个聚合函数 ***作用于表的某一列，并且返回一个值***。
>
> 2. COUNT、MIN、MAX既可以作用于数字列，也可以作用于非数字列。***而SUM和AVG只能作用于数字列。***
>
> 3. 可以用DISTINCT消除重复行。但是 ***DISTINCT对MIN和MAX没有影响。对SUM和AVG有影响。***
>
> 4. 一种特殊的COUNT，COUNT(\*)。除了COUNT（\*），其他每个聚合函数都先消除NULL值，然后计算剩余非空值。而COUNT（\*）是计算表中的元组个数，与是否取NULL无关。
>
>    举个栗子：在student表中COUNT（*）返回所有元组的数量，COUNT（salary）返回薪水非空的元组数量。

举个栗子：

```mysql
CREATE TABLE s(
    sno varchar(2) primary key ,
    sname varchar(3),
    age int
);
INSERT INTO s
VALUES ('s1','ss1',19),
       ('s2','ss2',17),
       ('s3','ss3',18),
       ('s4','ss4',17);
#<----AVG---COUNT---MIN-----MAX---->
SELECT AVG(age),COUNT(age),COUNT(*)
FROM s;
# 输出结果 17.75 4 4
SELECT MIN(age),MAX(age)
FROM s;
# 输出结果 17 19
SELECT AVG(DISTINCT age)，COUNT(DISTINCT age)
FROM s;
# 输出结果： 18 3

UPDATE s SET age = null WHERE sname = 'ss4';# 设置s4的age为NULL


SELECT AVG(age),COUNT(age),COUNT(*)
FROM s;
#输出结果 18 3 4
SELECT MIN(age),MAX(age)
FROM s;
# 输出结果 17 19
SELECT MIN(DISTINCT age),MAX(DISTINCT age)
FROM s;
# 输出结果：17 19
```

***！！！说明！！！***

> ***1. 聚合函数只能又在 SELECT 列表以及 HAVING 子句中，不能用在 WHERE 子句中。***
>
> ***2. 如果 SELECT 列表中有聚合函数出现，并且还没有GROUP BY子句，那么SELECT列表中不允许出现非聚合函数的属性。***
>
> 举个栗子：
>
> ```mysql
> # WRONG
> SELECT Sno,AVG(Grade)
> FROM Sc
> WHERE AVG(Grade) > 60;
> 
> # WRONG
> SELECT SNO,AVG(Grade)
> FROM Sc;
> ```

## 4. GROUP 子句

SELECT的属性列表有以下几种情况：

> 列名、聚合函数、常量、或者前三种的组合。

**当SELECT后的属性列表中含有聚合函数和列名时，不在聚合函数内部出现的属性要放在GROUP BY子句内部。** 简言之，SELECT中的属性，要么出现在聚合函数内部，要么出现在GROUP BY子句中。

举个栗子：

```	mysql
# 正确的写法
SELECT C.Cno,Cname,AVG(Grade)
FROM Sc,Course C
WHERE sc.Cno = C.Cno
GROUP BY C.Cno,Cname

# 错误的写法（缺失了Cname）
SELECT C.Cno,Cname,AVG(Grade)
FROM Sc,Course C
WHERE sc.Cno = C.Cno
GROUP BY C.Cno
```

> **执行顺序问题：**
>
> 如果WHERE子句和GROUP BY子句同时出现，那么首先执行WHERE子句，对满足WHERE条件的元组执行GROUP BY命令。

举个栗子：

```mysql
SELECT branchNo,COUNT(StaffNo) ,SUM(Salary)
FROM Staff
GROUP BY branchNo
ORDER BY branchNo
```

## 5. HAVING 子句

> HAVING 子句是对 GROUP BY子句执行之后的组进行限制的语句。其中，***HAVING子句中出现的参数必须出现在GROUP BY子句中，或者出现在聚合函数内部。***

HAVING的作用和WHERE类似。只不过WHERE是对元组的限制，而HAVING是对于组的限制。

举个栗子：

```mysql
# 正确写法
SELECT branchNo,COUNT(StaffNo) ,SUM(Salary)
FROM Staff
GROUP BY branchNo
HAVING COUNT(StaffNo) > 1
ORDER BY branchNo

# 错误写法
SELECT branchNo,COUNT(StaffNo) as count1 ,SUM(Salary)
FROM Staff
GROUP BY branchNo
HAVING count1 > 1 # 执行顺序决定count1是错误的。
ORDER BY branchNo
```

## 6. 子查询

​	在 WHERE 子句或者 HAVING 子句中可以嵌套SELECT子句。其中，嵌套的SELECT子句被称为子查询，外层的SELECT语句被称为父查询。

举个栗子：

```mysql
# 外查询
SELECT staffNo,fName,IName,position
FROM Staff
WHERE branch in(
    # 内查询
	SELECT branch
    FROM Branch
    WHERE street = '163 Main St'
);
```

​	上述代码等价于，在Staff表中查找 branchNo = ’1003‘的员工

再举个例子：

```mysql
#查询薪水比平均薪水高的员工，并列出高了多少
SELECT staffNo，fName，IName，position，salary - (SELECT AVG(salary) FROM Staff) as SalDiff
FROM Staff
WHERE salary > (
	SELECT AVG(salary)
    FROM Staff
);

# 错误写法
SELECT staffNo，fName，IName，position，salary - AVG(salary) as SalDiff
FROM Staff
WHERE salary > AVG (salary)
```

> 子查询的规则：
>
> 1. ***ORDER BY子句必须放在最外层查询中*** ，不能放在内层子查询中。
> 2. **子查询选择列表必须包括单个列名或者表达式，除非使用EXIST子查询。**
> 3. 默认情况下，列名会引用其所在同一级 FROM 子句中的表，可以通过给表或子查询一个别名【临时表】（alias），然后在查询中通过该别名来引用该表或子查询的列。**如果声明了别名，那么就必须使用。否则就报错。**
>
> 举个栗子：
>
> ```mysql
> # 正确的写法
> SELECT Sno , Cname , Grade
> FROM sc,Course c
> WHERE Sc.Cno = c.cno
> 
> # 错误的写法,因为c没有使用
> SELECT Sno , Cname , Grade
> FROM sc,Course c
> WHERE Sc.Cno = course.cno
> ```

---

> ***全称标识符*** 和 ***存在标识符***

ANY和ALL可以用在单属性的子查询，而不能用在*上。

> 使用ALL，只有所有值满足条件才为True
>
> 使用ANY，只要有一个值满足条件就为True。（ANY可以被替换为SOME）

|          |  =   |  ！=   |  <   |  <=   |  >   |  >=   |
| :------: | :--: | :----: | :--: | :---: | :--: | :---: |
| SOME/ANY |  IN  |   -    | <MAX | <=MAX | >MIN | >=MAX |
|   ALL    |  -   | NOT IN | <MIN | <=MIN | >MAX | >=MAX |

​	其中，！= SOME 和 = ALL没有意义。

### 6.1 不相关子查询

> 1. 不相关子查询可以被分别执行
> 2. 从内向外执行。首先执行最内层子查询，然后依次向外执行，知道到达最外层。

​	之前写的子查询都是不相关子查询，因此这里不再赘述。

### 6.2 相关子查询

> EXISTS 和 NOT EXISTS

​	EXISTS 和 NOT EXISTS 只返回 TRUE / FALSE。当且仅当在子查询结果表中至少有一行元组的时候，返回TRUE，否则返回FALSE。由EXIST引出的子查询，其目标列一般都用 * 。因为带EXISTS的子查询只返回TRUE或者FALSE，给出列名也无意义。

举个栗子：

```mysql
# 查找在伦敦公司工作的员工。
SELECT staffNo,fName,IName,position
FROM Staff s
WHERE EXISTS(
	SELECT *
    FROM Branch b
    WHERE s.branchNo = s.branchNo
    	AND city = 'London'
)
```

相关子查询特点：

> 1. 子查询不能独立执行。
> 2. 执行顺序是从外向内。类似于一个循环。最外层查询首先执行，然后进入内一层，直至最内层。

*** 注意：***

​	如果内层子查询省略了 s.branchNo = s.branchNo，那么内层子查询将永远返回TRUE，就不会起到查询的效果。

举两个非常经典的例子：

```mysql
#<--------第一个例子-------->
#找选修了所有课程的学生。相当于查找这样的学生，不存在一门课是他不选的。
SELECT Sno，Sname
FROM Student s
WHERE NOT EXISTS(
	SELECT *
    FROM Course c
    WHERE NOT EXISTS(
    	SELECT *
        FROM Sc
        WHERE s.Sno = Sc.Sno
        	AND sc.Cno = c.Cno
    )
)
# 第二种做法
SELECT Sno,Sname
From Student
WHERE Sno in(
    SELECT Sno
    FROM Sc
    GROUP BY Sno
    HAVING COUNT(*) = (
        SELECT COUNT(*)
        FROM Course
    )
)

#<-------第二个例子--------->
# 查找选修了学生S3选修的所有课程的学生。相当于查找这样的学生，不存在一门课。学生S3选了，而这个学生没选。
SELECT Sno
FROM Sc x
WHERE NOT EXISTS(
	SELECT *
    FROM Sc y
    WHERE y.Sno = S3 
    	AND NOT EXISTS(
     		SELECT *
            FROM Sc z
            WHERE x.Sno = z.Sno
            	AND y.Cno = z.Cno
     )
)
```

## 7. 交并差（也可以被子查询代替）

> UNION : A UNION B ，是一个包含了表A和B所有行的表
>
> INTERSECTION ：A INTERSECTION B，是一个包含了A和B共有行的表
>
> EXCEPTION ：A EXCEPTION B，是一个仅包含A中独有行的表。

举个栗子：

```mysql
# 查找或者有一个公司、或者有一个房产所在的城市
(
	SELECT city
    FROM Branch
    WHERE city is NOT NULL
)
UNION
(
	SELECT city
    FROM Branch
    WHERE city is NOT NULL
)
# 查找既有一个公司、又有一个房产所在的城市
(
	SELECT city
    FROM Branch
    WHERE city is NOT NULL
)
INTERSECT
(
	SELECT city
    FROM Branch
    WHERE city is NOT NULL
)
# 查找有公司但没有一个房产所在的城市
(
	SELECT city
    FROM Branch
    WHERE city is NOT NULL
)
EXCEPT
(
	SELECT city
    FROM Branch
    WHERE city is NOT NULL
)
```


