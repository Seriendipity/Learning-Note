# 程序性SQL

​	程序性SQL有三种方法：

> 1. PSM : 预编译好的高级语言与SQL语句结合的集合。包括存储过程和触发器
> 2. 嵌入式SQL：嵌入在高级语言中的SQL语句。分为静态SQL和动态SQL
> 3. API：无需预编译的一些列函数集合。如 JDBC 和 ODBC。

## 一、PSM

### 1.1 存储过程

> SP 是预编译好的存储在数据库里的程序。可以完成数据库的制定操作。
>
> SP 是SQL语句和控制流语句的混合，在第一次被执行的时候编译，之后不再编译。

```mysql
CREATE PROCEDURE <name>(
	<parameter list>
)
BEGIN
	[<local declarations>]
	<body>;
END;
```

举个栗子：转账

```mysql
DELIMITER //

CREATE PROCEDURE transfer(
    IN inAccount INT,
    IN outAccount INT,
    IN amount DECIMAL(10,2)
)
BEGIN
    DECLARE balance DECIMAL(10,2);
    DECLARE inAccountExists INT;
    DECLARE outAccountExists INT;
    
    -- 开始事务
    START TRANSACTION;
    
    -- 检查outAccount是否存在并获取其余额
    SELECT Total INTO balance
    FROM Account
    WHERE accountnum = outAccount
    FOR UPDATE;
    
    IF balance IS NULL THEN
        -- 账户不存在，抛出错误信息并回滚
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'The outAccount does not exist.';
    END IF;
    
    -- 检查outAccount是否有足够的资金
    IF balance < amount THEN
        -- 资金不足，抛出错误信息并回滚
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Insufficient funds.';
    END IF;
    
    -- 检查inAccount是否存在
    SELECT COUNT(*) INTO inAccountExists
    FROM Account
    WHERE accountnum = inAccount
    FOR UPDATE;
    
    IF inAccountExists = 0 THEN
        -- inAccount不存在，抛出错误信息并回滚
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'The inAccount does not exist.';
    END IF;

    -- 执行转账
    UPDATE Account
    SET total = total - amount
    WHERE accountnum = outAccount;

    UPDATE Account
    SET total = total + amount
    WHERE accountnum = inAccount;

    -- 提交事务
    COMMIT;
END//

DELIMITER ;



# 调用存储过程
CALL PROCEDURE transfer(0103815868,0103815828,100);

# 删除存储过程
DROP PROCEDURE transfer;
```

再举个例子：修改学生姓名：

```mysql
CREATE PROCEDURE changeName(
	IN oldName varchar(10),
    IN newName varchar(10)
)
BEGIN
	DECLARE studentExists int DEFAULT 0;
	-- 检查学生是否存在
	SELECT COUNT(*) INTO studentExists
	FROM Student
	WHERE Sname = oldName;
	
	IF(studentExists = 0) THEN
	-- 学生不存在，抛出错误信息并终止执行
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'The student does not exist.';
    END IF;
    
    UPDATE Student
    SET Sname = newName
    WHERE Sname = oldName;

END
```

### 1.2 约束

> 列级约束 ：对特定属性的值的约束。
>
> 表级约束 ：对整个表的约束。

```mysql
CREATE TABLE s(
	Sno varchar(8) NOT NULL, # 列约束
    Sname varchar(8) NOT NULL,# 列约束
    Age smallint,
    sex varchar(1),
    PRIMARY KEY(Sno),
    CHECK((sex = 'f' AND age < 21) 
          OR
          (sex = 'm' AND age < 23)
         )# 表约束
);
```

### 1.3 触发器

> 触发器是存储在数据库中的代码，被在数据库中发生的 **事件** 触发。

故，触发器也叫做 ECA（Event-Condition-Action）规则

> Event : 通常是增删改操作，
>
> Condition：SQL布尔值表达式
>
> Action：SQL语句

下面介绍一下Mysql触发器

> 触发器的类型

​	Mysql的触发器类型分为两种

* BEFORE触发器：在触发事件之前执行
* AFTER触发器：在触发事件之后执行

> 触发器的事件类型

​	Mysql的触发器事件类型分为三种

* INSERT：在表中插入新纪录时触发
* UPDATE：在表中更新记录时触发
* DELETE：在表中删除记录时触发

> 触发器的限制

* 每个表的每种事件只能有一个BEFORE触发器和一个AFTER触发器。例如，对于Student表的INSERT可以有一个BEFORE触发器和一个AFTER触发器，但是不能有两个AFTER触发器或两个BEFORE触发器。
* 触发器内部不能调用另一个触发器
* 触发器不能包含事物控制语句，如：START TRANSACTION、COMMIT、ROLLBACK等。
* 触发器不支持直接在控制台或者日志输出语句。
* ***MYSQL不支持在视图中建立触发器。***

> 访问旧值和新值

* INSERT触发器中，可以使用NEW关键字访问新插入记录中的属性
* UPDATE触发器中，可以使用NEW和OLD关键字访问更改后的记录和更改签的记录中的属性。
* DELETE触发器中，可以使用OLD关键字访问删除前的记录中的属性。

```mysql
CREATE TRIGGER trigger_name
BEFORE|AFTER INSERT|UPDATE|DELETE
ON table_name
FOR EACH ROW
BEGIN
   <trigger body>
END;
```

举个栗子：

```mysql
# BEFORE 触发器，插入学生记录
CREATE TRIGGER before_student_insert
BEFORE INSERT ON students
FOR EACH ROW
BEGIN
    -- 确保学生的分数在0到100之间
    IF NEW.score < 0 OR NEW.score > 100 THEN
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Score must be between 0 and 100';
    END IF;
END;

# 删除触发器
DROP TRIGGER IF EXISTS trigger_name;

```

## 二、嵌入式SQL

### 2.1 静态SQL

> 在程序执行的时候，整个SQL语句是知道的，没有未知参数。

* 嵌入式SQL通常以 EXEC SQL作为开始，在C语言中，以分号作为结尾。
* 嵌入式SQL可以出现在任何宿主语言语句能出现的位置。

举个栗子：在使用C在Oracle中创建一个表

```c
EXEC SQL CREATER TABLE Viewing
(
    propertyNo VARCHAR(25) NOT NULL,
    clintNo VARCHAR(25) NOT NULL,
    viewDate DATE NOT NULL,
    comment VARCHAR(40)
);
if(sqlca.sqlcode >=0)
    printf("Creation successful");
```

---

> ***SQL 通信区（SQLCA）***

SQLCA用于向应用程序反映运行时错误。其中SQLCODE是SQLCA中最重要的一个变量。

>SQLCODE = 0 ------> SQL语句成功执行
>
>SQLCODE < 0 -------> SQL语句执行失败，有Error发生
>
>SQLCODE > 0 -------->SQL 语句执行成功，但是有异常发生。比如SELECT返回空。

---

> **WHENERVER 语句**

该语句用来简化代码，无论何时有指令来预编译生成代码处理SQL语句错误，

```
EXEC SQL WHENEVER <condition><action>
```

其中，Condition可以为：

> * SQLERROR：告知预编译器使用代码解决错误（SQLCODE < 0）
> * SQLWARNING : 告知预编译器使用代码解决警告（SQLCODE > 0）
> * NOT FOUND：告知预编译器解决未找到记录问题 (SQLCODE > 0)

Action 可以为

> * CONTINUE : 忽略condition，执行下一个语句
> * DO：将控制转移到处理错误函数上，然后回到该语句
> * DO BREAK：在程序中放一个中断语句，如果再循环中使用，则跳出循环。
> * DO CONTINUE：在程序中放一个继续执行语句，如果再循环中使用，继续下一次循环。
> * GOTO：把控制转移到GOTO的label处
> * STOP：事务回滚，并且撤销未完成的工作，终止程序。 

```
EXEC SQL WHENEVER SQLERROR GOTO error1;
EXEC SQL INSERT INTO Viewing VALUES('CR76','PA14','12-May-2001','Not Enough Space');

//如果不用WHENEVER。如果有多个sql语句，每一个语句后面都要写一个if
EXEC SQL INSERT INTO Viewing VALUES('CR76','PA14','12-May-2001','Not Enough Space');
if(SQLCA.SQLCODE < 0 ) GOTO error1;
```

---

> **共享变量** 

​	用于数据库接受来自宿主语言的变量，使得数据库中能够使用该值。

* 他可以出现在任何一个常量可以出现的地方
* 他不能用于表示数据库对象，如表明或者列名
* 需要用冒号：加以区分。
* 需要事先声明。

```
EXEC SQL BEGIN DECLARE SECTION;
float increment;
EXEC SQL END DECLARE SECTION;

EXEC SQL UPDATE Staff
		SET salary = salary + :increment
		WHERE staffNo = 'SL21';
```

> 指标变量

​	他表示存在空值。

* = 0 表示宿主语言的值是一个有效值
* < 0 表示宿主语言的值为空，把它当做NULL
* \> 0 表示对宿主语言的值要么做截断，要么四舍五入。

举个栗子：

```
EXEC SQL BEGIN DECLARE SECTION;
char address[51];
short addressInd;
EXEC SQL END DECLARE SECTION;

addressInd = -1;

EXEC SQL UPDATE PrivateOwner
		SET address = :address:addressInd //相当于给Address赋null
		WHERE ownerNo = 'CO21';
```

再举个栗子：

```
EXEC SQL SELECT fName,IName,address
		INTO :firstName , :laseName , :address:addressInd
		FROM PrivateOwner
		WHERE ownerNo = 'CO21';
```

> **游标**

​	当数据库查询返回不止一行元组的时候，需要使用 **游标** 。游标允许宿主语言一次访问查询结果中的一行。

* 游标相当于一个指针，最开始指向表头（第零行）
* **游标必须声明和开启，结束之后必须关闭。**
* 使用FETCH来获取游标所指行的数据。

举个栗子：

```C
//声明游标
EXEC SQL DECLARE propertyCursor 
CURSOR FOR SELECT propertyNo,street,city
			FROM PropertyForRent
			WHERE staffNo = 'SL41';
//开启游标
EXEC SQL OPEN propertyCursor;

// 将数据库结果保存到宿主语言中
while(SQLCODE != NOT FOUND){
    EXEC SQL FETCH propertyCursor
        INTO :propertyNo,:street,:city
}

//关闭游标
EXEC SQL CLOSE propertyCursor;

```

### 2.2 动态SQL

​	动态SQL允许部分或者全部的SQL语句在运行时被赋值。

> ***静态SQL不允许用共享变量替代数据库对象名。***

举个栗子：

```
EXEC SQL BEGIN DECLARE SECTION;
char TableName[2];
EXEC SQL END DECLARE SECTION;

//在静态SQL中不允许这样写，但是动态SQL允许
EXEC SQL INSERT INTO :TableName
	VALUES('S12','John',18,'m');
```

两种类型的动态SQL：

* EXECUTE IMMEDIATE（立即执行）
* PREPARE AND EXEcute（准备执行）

#### 2.2.1 立即执行

如果SQL语句不包含SELECT语句，使用立即执行的动态SQL具有如下格式：

```
EXEC SQL EXECUTE IMMEDIATE [hostVariable | StringLiteral]
```

​	上述语句允许SQL语句存储在共享变量或者字符串常量中。他不能够传递参数。

举个栗子：

```C
//增加‘SL21’员工的薪水
EXEC SQL BEGIN DECLARE SECTION;
char buffer[100];
EXEC SQL END DECLARE SECTION;

sprintf(buffer,"UPDATE Staff SET salary = salary + %f WHERE staffNo = 'SL21'",increment);

EXEC SQL EXECUTE IMMEDIATE :buffer;
```

#### 2.2.2 准备执行

```
EXEC SQL PREPARE statementName
	FROM [hostVariable | stringLiteral]
EXEC SQL EXECUTE statementName
	[USING hostVariable[indicator Variable][,...]]
	| USING DESCRIPTOR descriptorName]
```

## 3. API

#### 3.1 ODBC

#### 3.2 JDBC

