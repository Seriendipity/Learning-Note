# Sql语句

> Sql语句由三部分组成。数据定义语言（DDL）【create、alter、drop】，数据操纵语言（DML）【insert、delete、update、retrieve（检索）】，数据控制语言（DCL）【grant、revoke、constrict、trigger、sp（存储过程）、transaction、lock等】。

**SQL语句的特点**

* SQL语句是 ***非过程性语言*** ，即只需要知道如何操作，而不需要知道操作是如何实现的。
* 一般的DBMS提供两种SQL，***嵌入式SQL（结合高级语言）和交互式SQL***
* 可移植性高

---

**SQL语句书写要求**

* SQL语句是 ***大小写不敏感*** 的，除了 ***字符型数据*** 。("SMITH" 数据和“smith"数据不等价)
* 通常，大写字符表示保留字，小写字符表示用户自定义的字，“|”表示从可选项中选择一个（eg：a|b|c），"{}"表示必须的元素，"[]"表示可有可无的元素，可选项，"..."表示0次或多次。
* **标识符必须以字母开头，否则用[]框起来。**eg：01DB作为标识符是错的，而[01DB]作为标识符是OK的。
* **标识符不能包含空格，否则用[]框起来**。eg: D B作为标识符是错的，而[D B]作为标识符是OK的。

---

>  **完整性增强特性:**
>
>  **数据限制、域限制、实体完整性、参照完整性、用户自定义完整性**。
>
>  > 其中，实体完整性和参照完整性是必须遵循的，称为 **关系的两个不变性**
>  >
>  > 用户自定义完整性是语义上的约束。

* 数据限制（非空限制）

> 主键包含的属性不能为空值，或者指定为not null的属性不能为空值，unique包含的属性不能为空值。

* 域限制(check 语句)

> 行限制：eg: Sex varchar(2) check (Sex in （‘男’，‘女’）)
>
> 表限制：eg：在创建表的最后加 check Sage < 40
>
> > 即行限制加在对应的属性后，而表限制不加在属性后。

* 实体完整性

> 每一个关系只能含有一个主键。并且 **主属性不能为空**，可以使用替代键unique来确保唯一识别一行元组。
>
> SQL语句提供两种设置主键的方式，其一为在建表是对应的属性后面直接加primary key。其二是在建表的最后，加上primary（col1,[...]）。

* 参照完整性

> 外键：是一个属性或者一系列属性组，在该表中参照父表中的属性或者属性组。
>
> **外键的属性或者属性组，要么为null，要么必须在父表已经存在。**
>
> 外键的SQL语句：Foreign Key （本表中的属性|属性组） Reference 父表（父表中的属性|属性组）。
>
> eg: Staff  ( staffNo , fName , IName , position , branchNo , leaderNo )
>
> ​	Branch ( branchNo , street , city , postcode )
>
> 那么在Staff表中，可以设置branchNo为外键，leaderNo为外键
>
> Foreign Key (branchNo) references Branch(branchNo)
>
> Foreign Key (leaderNo) references Staff(staffNo)【说明：外键可以参照自己本身】
>
> > 如果外键已经设置了，那么在insert和update操作的时候，如果不满足外键约束，是无法成功修改的。
>
> ***当在父表中修改候选键的值或者删除一个元组的时候，如果设置了外键，要遵循以下四种模式*** ，通过 ON UPDATE [] , ON DELETE []来加以约束
>
> > **NO ACTION**: 如果有子表参照该元组的属性，那么禁止更改。
> >
> > **SET DEFAULT：** 从父表中删除一行元组时，如果子表参照该元组，设置子表中对应的元组属性为默认值。【前提是子表中的属性设置了默认值】
> >
> > **SET NULL：** 从父表中删除一行元组的时候，如果子表参照该元组，设置子表中对应的元组属性为null。【前提是子表中该属性允许取null】
> >
> > **CASCADE：** 从父表中删除一行元组的时候，如果子表参照该元组，级联删除子表中与之关联的所有元组。

---

## 数据定义语句（DDL）

### （1）create/drop schema（创建/删除模式）

```mysql
CREATE DATABASE databaseName;
```

> 其 实质上是创建一个数据库。在 **mysql中使用create/drop database来实现** 。
>
> 在sqlServer中，**drop schema [RESTRICT | CASCADE]** ,其中，默认是RESTRICT，也就是说，如果数据库中存在表，那么就不允许删除该数据库。如果选择了CASCADE，那么在删除该数据库的时候，先级联删除数据库中的所有表，然后再删除该数据库。

### （2）create table(创建表)

> ```mysql
> CREATE TABLE TableName(
> 	colName dataType [NOT NULL][UNIQUE][DEFAULT defaultOption][CHECK serchCondition][PRIMARY KEY] # 创建属性时加的限制为列级限制
> 
> 	[...]
> 
>     # 创建属性之后加的限制为表级限制
> 	[PRIMARY KEY (list of columns)],
> 	[UNIQUE (list of columns)],
> 	[FOREIGN KEY (columnName) REFERENCES ParentTableName(columnName)[ON UPDATE referentialAction][ON DELETE referentialAction]],
> 	[CHECK (serchCondition)] 
> 
> ）;
> ```

1. not null 限制的属性不允许取空值
2. unique 限制的属性不允许取重复的值
3. default 限制的属性，如果没有赋值，会将defalult的值赋给该属性。
4. check 限制的属性，如果更改、插入数据时不满足check的条件，那么不允许更改或者插入数据。
5. primary key 限制的属性为主键，一个表中只能有一个主键，主键用来唯一确定一行元组。如果设置为主键，可以省略not null的限制，因为主键就已经规范了该属性不能取空值。
6. foreign key 限制的属性为外键。【ON UPDATE】【ON DELETE】为对外键的约束。**其中，外键要满足参照完整性。**

eg：

```mysql
CREATE TABLE SC(
	SNO CHAR(4),
    CNO CHAR(4),
    Grade smallint,
    PRIMARY KEY (SNO,CNO),
    FOREIGN KEY (SNO) REFERENCES S (SNO) ON DELETE CASCADE,
    FOREIGN KEY (CNO) REFERENCES C (CNO) ON DELETE CASCADE,
    CHECK ((G IS NULL) OR (G BETWEEN 0 AND 100) )
);
```

> **说明：要检查一个属性是否为null，不能用  “=” 来进行判断，要是用 IS NULL**

### (3) 修改表

```mysql
ALTER TABLE TABLENAME 
[ADD[COLUMN] columnName dataType[NOT NULL][UNIQUE][DEFAULT defaultOption][CHECK serchCondition]]
[DROP[COLUMN] columnName [RESTRICT|CASCADE]]
[ADD[CONSTRAINT][ConstraintName] tableConstraintDefination]
[DROP CONSTRAINT ConstraintName[RESTRICT|CASCADE]]
[MODIFY[COLUMN] datatype SET DEFAULT defaultOption]
[MODIFY[COLUMN] datatype DROP DEFAULT]
```

> 即，在创建表之后，如果想要修改表结构，可以通过 ALTER TABLE来更改表结构。
>
> 分别可以增加一列、删除一列、增加一个表级限制、删除一个表级限制、增加一个默认值、删除一个默认值。

举个栗子：

```mysql
alter table s add column addr char(20);
alter table s drop column addr;
alter table staff modify position varchar(20) drop default;
alter table stadd modify sex set varchar(1) default 'F';
alter table property drop constraint staffNotHandlingTooMuch;
alter table s add constraint sex_age check((sex = 'F' AND age < 21) || (sex = 'M' AND age < 23));
alter table s drop constraint sex_age;
```

### (4)删除表

```mysql
DROP TABLE TableName [RESTRICT|CASCADE]
```

> 如果表中有实例，那么RESTRICT限制不允许删除该表
>
> 如果想要删除该表，那么需要使用CASCADE限制。

举个栗子

```mysql
drop table sc;
```

### (5)索引

### **创建索引**

> 索引：是一个允许DBMS快速定位元组的数据结构。通常有哈希表、B+树两种。

```mysql
CREATE[UNIQUE]INDEX <indexName> ON <tableName>(<ColumnName>[<ASC|DESC>[,[...]]])
```

> 注意：mysql并不能直接定义 **聚簇索引** ，而是要通过InnoDB来设置聚簇索引。

**说明：**

1. UNIQUE：表明该索引列不能取重复值，即该属性能唯一确定一行元组。但是空值可以有多个，因为每一个空值在数据库中被认为是不同的值。
2. ASC：对索引中的元素按照升序排列，默认为升序
3. DESC：对索引中的元素按照降序排列，使用降序时需要手动指出。
4. **聚簇索引：** 在聚簇索引中的顺序和在原表中的顺序是一样的。即 ***聚簇索引会对原数据文件进行排序，改变了原数据文件的物理存储顺序。*** 而非聚簇索引不会改变原数据文件中的物理存储顺序。

举个栗子：

```mysql
CREATE UNIQUE INDEX staffNoInd ON staff(StaffNO DESC);
CREATE UNIQUE INDEX propertyNoInd ON property(propertyNo);
CREATE INDEX RentInd ON Property (city , rent DESC);# 其中，city是主索引，rent是次索引。只有在主索引相同的时候，才会按照次索引的方式来进行排序
```

基本概念：

1. 数据文件：用于存放实例数据。即原表中的数据。

2. 索引文件：用于存放索引记录的文件，即索引中的数据。

3. 索引文件由索引记录组成，每一个索引记录包含两个属性（Search-Key , pointer）。

   > search-key是一个用于查找记录的属性或者属性组
   >
   > pointer是数据文件中的地址

举个栗子：

 --数据文件--

| Record.No |  Sno  |  Name  | Sex  |  Birthday  | grade |
| :-------: | :---: | :----: | :--: | :--------: | :---: |
|     1     | 00001 |  王文  |  男  | 2004-02-04 |  510  |
|     2     | 00002 |  李道  |  男  | 2003-08-14 |  502  |
|     3     | 00003 | 邓选斌 |  男  | 2005-05-09 |  524  |
|     4     | 00004 |  王瑞  |  女  | 2004-11-23 |  522  |
|     5     | 00005 |  刘光  |  男  | 2004-12-22 |  526  |

--索引文件--

| Key（Grade） | Record.No |
| :----------: | :-------: |
|     502      |     2     |
|     510      |     1     |
|     522      |     4     |
|     524      |     3     |
|     526      |     5     |

> **比较一下排序和索引的不同：**
>
> > 排序会生成一个新的数据文件，而索引不会改变数据文件，会生成一个索引文件
> >
> > 排序会改变原数据文件的物理顺序，而索引不改变原数据文件的物理顺序
> >
> > 排序生成的新表可以独立使用，而索引文件不能独立使用。

下面来举例说明聚簇索引。仍然以上面的数据文件为例，如果建立聚簇索引，那么原数据文件会变为如下所示。

| Record.No |  Sno  |  Name  | Sex  |  Birthday  | grade |
| :-------: | :---: | :----: | :--: | :--------: | :---: |
|     1     | 00002 |  李道  |  男  | 2003-08-14 |  502  |
|     2     | 00001 |  王文  |  男  | 2004-02-04 |  510  |
|     3     | 00004 |  王瑞  |  女  | 2004-11-23 |  522  |
|     4     | 00003 | 邓选斌 |  男  | 2005-05-09 |  524  |
|     5     | 00005 |  刘光  |  男  | 2004-12-22 |  526  |

索引文件为如下所示

| Key（Grade） | Record.No |
| :----------: | :-------: |
|     502      |     1     |
|     510      |     2     |
|     522      |     3     |
|     524      |     4     |
|     526      |     5     |

​	可以看到，索引文件中的顺序和数据文件中的顺序是一样的，也就是说，他改变了数据文件中的元组顺序。使得其一致。



​	使用索引的时候要注意语义含义，不然可能会造成歧义。

举个栗子：

```mysql
CREATE UNIQUE INDEX SCno on SC(Sno)
```

​	我们来看上面的例子，在SC表中建立一个UNIQUE索引，乍一看没问题。但是仔细一想，上述索引是不合理的。

​	如果表中有数据，那么很可能执行失败。因为有可能一个学生选修了多门课程，而建立在学号上的Unique索引不允许有重复的Sno出现。那如果表中没有数据，那么该命令执行成功，但也限制了一个学生最多只能选修一门课程，这与实际情况不符，需要慎重。

改进后的索引为：

```mysql
CREATE UNIQUE INDEX SCno on SC (Sno,Cno DESC);
```

---

### **删除索引**

```mysql
DROP INDEX <indexName>;
```

​	从数据词典中删除描述。

举个栗子，删除Student表中的Stusname索引。

```mysql
DROP INDEX Stusname on Student;
```




