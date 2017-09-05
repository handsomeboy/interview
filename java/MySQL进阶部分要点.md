# MySQL进阶部分要点


## 查询数据
基本语法
Select 字段列表/* from 表名 [where条件];

完整语法
Select [select选项] 字段列表[字段别名]/* from 数据源 [where条件子句] [group by子句] [having子句] [order by子句] [limit 子句];

<br>

**Select选项**

select对查出来的结果的处理方式
All: 默认的,保留所有的结果
Distinct: 去重, 查出来的结果,将重复给去除(所有字段都相同)

<br>

**字段别名**
字段别名: 当数据进行查询出来的时候, 有时候名字并一定就满足需求(多表查询的时候, 会有同名字段). 需要对字段名进行重命名: 别名

语法
字段名 [as] 别名;


<br>

**数据源**

数据源: 数据的来源, 关系型数据库的来源都是数据表 本质上只要保证数据类似二维表,最终都可以作为数据源.

数据源分为多种: 单表数据源, 多表数据源, 查询语句

单表数据源: select * from 表名;
多表数据源: select* from 表名1,表名2...;

从一张表中取出一条记录,去另外一张表中匹配所有记录,而且全部保留:(记录数和字段数),将这种结果成为: 笛卡尔积(交叉连接): 笛卡尔积没什么卵用, 所以应该尽量避免.

子查询: 数据的来源是一条查询语句(查询语句的结果是二维表)
Select * from (select 语句) as 表名;


<br>

**Where子句**

Where子句: 用来判断数据,筛选数据.
Where子句返回结果: 0或者1, 0代表false,1代表true.

判断条件: 
比较运算符: >, <, >=, <= ,!= ,<>, =, like, between and, in/not in
逻辑运算符: &&(and), ||(or), !(not)
注意：Between本身是闭区间;between左边的值必须小于或者等于右边的值

Where原理: where是唯一一个**直接从磁盘获取数据**的时候就开始判断的条件: 从磁盘取出一条记录, 开始进行where判断: 判断的结果如果成立保存到内存;如果失败直接放弃.

<br>

**Group by子句**

Group by:分组的意思, 根据某个字段进行分组(相同的放一组,不同的分到不同的组)

基本语法: group  by 字段名;

分组的意思: 是为了统计数据(按组统计:按分组字段进行数据统计)

SQL提供了一系列统计函数
+ Count(): 统计分组后的记录数: 每一组有多少记录：里面可以使用两种参数: *代表统计记录,字段名代表统计对应的字段(NULL不统计)
+ Max():	统计每组中最大的值
+ Min(): 统计最小值
+ Avg(): 统计平均值
+ Sum(): 统计和

分组会自动排序: 根据分组字段:默认升序
Group by 字段 [asc|desc];	-- 对分组的结果然后合并之后的整个结果进行排序

多字段分组: 先根据一个字段进行分组,然后对分组后的结果再次按照其他字段进行分组

<br>

**Having子句**

Having子句: 与where子句一样: 进行条件判断的.

Where是针对磁盘数据进行判断: 进入到内存之后,会进行分组操作: 分组结果就需要having来处理.

Having能做where能做的几乎所有事情, 但是where却不能做having能做的很多事情.

1.	分组统计的结果或者说统计函数都只有having能够使用.
2.	Having能够使用字段别名: where不能: where是从磁盘取数据,而名字只可能是字段名: 别名是在字段进入到内存后才会产生.

<br>

**Order by子句**

Order by: 排序, 根据某个字段进行升序或者降序排序, 依赖校对集.

使用基本语法
Order by 字段名 [asc|desc]; -- asc是升序(默认的),desc是降序

排序可以进行多字段排序: 先根据某个字段进行排序, 然后排序好的内部,再按照某个数据进行再次排序

<br>

**Limit子句**

Limit子句是一种限制结果的语句: 限制数量.

Limit有两种使用方式

方案1: 只用来限制长度(数据量): limit 数据量;
方案2: 限制起始位置,限制数量: limit 起始位置,长度;

Limit方案2主要用来实现数据的分页: 为用户节省时间,提交服务器的响应效率, 减少资源的浪费.
对于用户来讲: 可以点击的分页按钮: 1,2,3,4
对于服务器来讲: 根据用户选择的页码来获取不同的数据: limit offset,length;

Length: 每页显示的数据量: 基本不变
Offset: offset = (页码 - 1) * 每页显示量
<br>

**SQL示例：**

```sql
-- select选项
select * from my_copy;
select all * from my_copy;

-- 去重
select distinct * from my_copy;

-- 字段别名
select 
id,
number as 学号,
name as 姓名,
sex 性别 from my_student;

-- 多表数据源
select * from my_student,my_class;


-- 子查询
select * from (select * from my_student) as s;

-- where
select * from my_student where height between 190 and 180;

-- 根据性别分组
select * from my_student group by sex;

-- 多字段分组: 先班级,后男女
select c_id,sex,count(*),group_concat(name) from my_student group by c_id,sex; -- 多字段排序

-- 统计
select c_id,count(*) from my_student group by c_id;

-- 求出所有班级人数大于等于2的学生人数,不能同where
select c_id,count(*) from my_student group by c_id having count(*) >= 2;

select c_id,count(*) as total from my_student group by c_id having total >= 2;

-- 单字段排序
select * from my_student order by c_id;

-- 多字段排序: 先班级排序,后性别排序
select * from my_student order by c_id, sex desc;

-- 查询学生: 前两个
select * from my_student limit 2;

-- 查询学生: 前两个
select * from my_student limit 0,2; -- 记录数是从0开始编号

```

<br><br>

## 连接查询(多表查询)

连接查询: 将多张表(可以大于2张)进行记录的连接(按照某个指定的条件进行数据拼接): 最终结果是: 记录数有可能变化, 字段数一定会增加(至少两张表的合并)

SQL中将连接查询分成四类: **内连接,外连接,自然连接和交叉连接**

连接查询的意义: 在用户查看数据的时候,需要显示的数据来自多张表.

连接查询: join, 使用方式: 左表 join 右表
左表: 在join关键字左边的表
右表: 在join关键字右边的表


<br>

**交叉连接**

交叉连接: cross join, 从一张表中循环取出每一条记录, 每条记录都去另外一张表进行匹配: **匹配一定保留(没有条件匹配)**, 而连接本身字段就会增加(保留),最终形成的结果叫做: 笛卡尔积.

基本语法: 左表 cross join 右表; ===== from 左表,右表;

笛卡尔积没有意义: 应该尽量避免(交叉连接没用)
交叉连接存在的价值: 保证连接这种结构的完整性



<br>

**内连接**

内连接: [inner] join, 从左表中取出每一条记录,去右表中与所有的记录进行匹配: **匹配必须是某个条件在左表中与右表中相同最终才会保留结果,否则不保留.**

基本语法
左表 [inner] join 右表 on 左表.字段 = 右表.字段; on表示连接条件: 条件字段就是代表相同的业务含义(如my_student.c_id和my_class.id)

字段别名以及表别名的使用: 在查询数据的时候,不同表有同名字段,这个时候需要加上表名才能区分, 而表名太长, 通常可以使用别名.

内连接可以没有连接条件: 没有on之后的内容,这个时候系统会保留所有结果(笛卡尔积)

内连接还可以使用where代替on关键字(**where没有on效率高**)
<br>

**外连接**

外连接: outer join, 以某张表为主,取出里面的所有记录, 然后每条与另外一张表进行连接: 不管能不能匹配上条件,最终都会保留: 能比配,正确保留; 不能匹配,其他表的字段都置空NULL.

外连接分为两种: 是以某张表为主: 有主表
Left join: 左外连接(左连接), 以左表为主表
Right join: 右外连接(右连接), 以右表为主表

基本语法: 左表 left/right join 右表 on 左表.字段 = 右表.字段;

虽然左连接和右连接有主表差异, 但是显示的结果: 左表的数据在左边,右表数据在右边.

左连接和右连接可以互转.


<br>

**自然连接**

自然连接: natural join, 自然连接, 就是自动匹配连接条件: 系统以字段名字作为匹配模式(同名字段就作为条件, 多个同名字段都作为条件).

自然连接: 可以分为自然内连接和自然外连接.

自然内连接: 左表 natural join 右表;

<br>

**SQL示例**

```sql
-- 交叉连接
select * from my_student cross join my_class;
-- my_student cross join my_class是数据源

-- 内连接
select * from my_student inner join my_class on my_student.c_id = my_class.id;
select * from my_student inner join my_class on c_id = my_class.id;
select * from my_student inner join my_class on c_id = id; -- 两张表都有id字段

-- 字段和表别名
select s.*,c.name as c_name,c.room from -- 字段别名
my_student as s inner join my_class as c -- 表别名
on s.c_id = c.id;


-- where代替on
select s.*,c.name as c_name,c.room from -- 字段别名
my_student as s inner join my_class as c -- 表别名
where s.c_id = c.id;

-- 左连接
select s.*,c.name as c_name,c.room from
my_student as s left join my_class as c -- 左表为主表: 最终记录数至少不少于左表已有的记录数
on s.c_id = c.id;

-- 右连接
select s.*,c.name as c_name,c.room from
my_student as s right join my_class as c -- 右表为主表: 最终记录数至少不少于右表已有的记录数
on s.c_id = c.id;


-- 左表为主表: 最终记录数至少不少于左表已有的记录数
select s.*,c.name as c_name,c.room from
my_class as c right join my_student as s 
on s.c_id = c.id;

-- 自然内连接
select * from my_student natural join my_class;

-- 自然左外连接
select * from my_student natural left join my_class;

-- 外连接模拟自然外连接: using
select * from my_student left join my_class using(id);
```

<br><br>

## 外键

外键: foreign key, 外面的键(键不在自己表中): 如果一张表中有一个字段(非主键)指向**另外一张表的主键,**那么将该字段称之为外键. 

外键可以在创建表的时候或者创建表之后增加(但是要考虑数据的问题).
**一张表可以有多个外键.**

创建表的时候增加外键: 在所有的表字段之后,使用
foreign key(外键字段) references 外部表(主键字段)


在新增表之后增加外键: 修改表结构
Alter table 表名 add [constraint 外键名字] foreign key(外键字段) references 父表(主键字段);

外键不可修改: 只能先删除后新增.

删除外键语法
Alter table 表名 drop foreign key 外键名; -- 一张表中可以有多个外键,但是名字不能相同

**外键作用：**
外键默认的作用有两点: 一个对父表,一个对子表(外键字段所在的表)

对子表约束: 子表数据进行写操作(增和改)的时候, 如果对应的外键字段在父表找不到对应的匹配: 那么操作会失败.(约束子表数据操作)

对父表约束: 父表数据进行写操作(删和改: 都必须涉及到主键本身), 如果对应的主键在子表中已经被数据所引用, 那么就不允许操作
<br>

**外键条件**

1.	外键要存在: 首先必须保证表的存储引擎是innodb(默认的存储引擎): 如果不是innodb存储引擎,那么外键可以创建成功,但是没有约束效果.
2.	外键字段的字段类型(列类型)必须与父表的主键类型完全一致.
3.	一张表中的外键名字不能重复.
4.	增加外键的字段(数据已经存在),必须保证数据与父表主键要求对应.


<br>

**外键约束**

所谓外键约束: 就是指外键的作用.
之前所讲的外键作用: 是默认的作用; 其实可以通过对外键的需求, 进行定制操作.

外键约束有三种约束模式: 都是针对父表的约束

`District`: 严格模式(默认的),父表不能删除或者更新一个已经被子表数据引用的记录
`Cascade`: 级联模式: 父表的操作, 对应子表关联的数据也跟着被删除
`Set null`: 置空模式: 父表的操作之后,子表对应的数据(外键字段)被置空

通常的一个合理的做法(约束模式): 删除的时候子表置空, 更新的时候子表级联操作
指定模式的语法
Foreign key(外键字段) references 父表(主键字段) on delete set null on update cascade;


删除置空的前提条件: 外键字段允许为空(如果不满足条件,外键无法创建)

外键虽然很强大, 能够进行各种约束: 但是外键的约束降低了数据的可控性: 通常在实际开发中, 很少使用外键来处理.

<br>

**相关SQL**

```sql
-- 创建外键

create table my_foreign1(
id int primary key auto_increment,
name varchar(20) not null comment '学生姓名',
c_id int comment '班级id',	-- 普通字段
-- 增加外键
foreign key(c_id) references my_class(id)
)charset utf8;


-- 增加外键
alter table my_foreign2 add
-- 指定外键名
constraint student_class_1
-- 指定外键字段
foreign key(c_id)
-- 引用父表主键
references my_class(id);

-- 删除外键
alter table my_foreign1 drop  foreign key my_foreign1_ibfk_1;



-- 增加外键
alter table my_foreign1 add foreign key(c_id) references my_class(id);

-- 创建外键: 指定模式: 删除置空,更新级联
create table my_foreign3(
id int primary key auto_increment,
name varchar(20) not null,
c_id int,
-- 增加外键
foreign key(c_id)
-- 引用表
references my_class(id)
-- 指定删除模式
on delete set null
-- 指定更新默认
on update cascade)charset utf8;
```



<br>
<br>

## 联合查询

多条select语句构成: 每一条select语句获取的字段数**必须严格一致**(但是字段类型无关)

Select 语句1
Union [union选项]
Select语句2...

Union选项: 与select选项一样有两个
All: 保留所有(不管重复)
Distinct: 去重(整个重复): 默认的

**联合查询的意义分为两种:**

1.	查询同一张表,但是需求不同: 如查询学生信息, 男生身高升序, 女生身高降序.
2.	多表查询: 多张表的结构是完全一样的,保存的数据(结构)也是一样的.

**Order by使用**


在联合查询中: order by不能直接使用,需要对查询语句使用括号才行

若要order by生效: 必须搭配limit: limit使用限定的最大数即可.


```sql
-- 联合查询
select * from my_class
union -- 默认去重
select * from my_class;

select * from my_class
union all -- 不去重
select * from my_class;

select id,c_name,room from my_class
union all -- 不去重
select name,number,id from my_student;

-- 需求: 男生升序,女生降序(年龄)
(select * from my_student where sex = '男' order by age asc limit 9999999)
union 
(select * from my_student where sex = '女' order by age desc limit 9999999);
```
<br><br>
## 子查询

**子查询分类**

子查询有两种分类方式: 按位置分类; 按结果分类

按位置分类: 子查询(select语句)在外部查询(select语句)中出现的位置
+ From子查询: 子查询跟在from之后
+ Where子查询: 子查询出现where条件中
+ Exists子查询: 子查询出现在exists里面


按结果分类: 根据子查询得到的数据进行分类(理论上讲任何一个查询得到的结果都可以理解为二维表)
+ 标量子查询: 子查询得到的结果是一行一列
+ 列子查询: 子查询得到的结果是一列多行
+ 行子查询: 子查询得到的结果是多列一行(多行多列)
上面几个出现的位置都是在where之后
+ 表子查询: 子查询得到的结果是多行多列(出现的位置是在from之后)

<br>

**标量子查询**

需求: 知道班级名字为0710,想获取该班的所有学生.

1.	确定数据源: 获取所有的学生
Select * from my_student where c_id = ?;
2.	获取班级ID: 可以通过班级名字确定
Select id from my_class where c_name = ‘0710’;	-- id一定只有一个值(一行一列)
3.最终：select * from my_student where c_id = (select id from my_class where c_name = '0710')

<br>

**列子查询**

需求: 查询所有在读班级的学生(班级表中存在的班级)

1.	确定数据源: 学生
Select * from my_student where c_id in (?);
2.	确定有效班级的id: 所有班级id
Select id from my_class;
3.最终：
select * from my_student where c_id in(select id from my_class);

列子查询返回的结果会比较: 一列多行, 需要使用in作为条件匹配: 其实在mysql中有还有几个类似的条件: all, some, any

=Any  ====  in; -- 其中一个即可
Any ====== some;	-- any跟some是一样
=all    ==== 为全部

<br>
**行子查询**

行子查询: 返回的结果可以是多行多列(一行多列)

需求: 要求查询整个学生中,年龄最大且身高是最高的学生.
1.	确定数据源
Select * from my_student where age = ? And height = ?;
2.	确定最大的年龄和最高的身高;
Select max(age),max(height) from my_student;
3.最终：
select * from my_student where 
-- (age,height)称之为行元素
(age,height) = (select max(age),max(height) from my_student);

<br>

**表子查询**

表子查询: 子查询返回的结果是多行多列的二维表: 子查询返回的结果是当做二维表来使用

需求: 找出每一个班最高的一个学生.

1.	确定数据源: 先将学生按照身高进行降序排序
Select * from my_student order by height desc;
2.	从每个班选出第一个学生
Select * from my_student group by c_id; -- 每个班选出第一个学生
3.最终：
select * from (select * from my_student order by height desc) as student group by c_id;
表子查询: from子查询: 得到的结果作为from的数据源



<br>

**Exists子查询**

Exists: 是否存在的意思, exists子查询就是用来判断某些条件是否满足(跨表), exists是接在where之后: exists返回的结果只有0和1.

需求: 查询所有的学生: 前提条件是班级存在
1.	确定数据源
Select * from my_student where ?;
2.	确定条件是否满足
Exists(Select * from my_class); -- 是否成立
3.最终：
select * from my_student where 
exists(select * from my_class where id = 2);

<br>

**相关SQL**

```sql

-- 需求: 男生升序,女生降序(年龄)
(select * from my_student where sex = '男' order by age asc limit 9999999)
union 
(select * from my_student where sex = '女' order by age desc limit 9999999);

-- 标量子查询
select * from my_student where c_id = (select id from my_class where c_name = '0710');
select * from my_student having c_id = (select id from my_class where c_name = 'PHP0710');

-- 列子查询
select * from my_student where c_id in(select id from my_class);

-- any,some,all
select * from my_student where c_id =any(select id from my_class);
select * from my_student where c_id =some(select id from my_class);
select * from my_student where c_id =all(select id from my_class);

select * from my_student where c_id !=any(select id from my_class); -- 所有结果(null除外)
select * from my_student where c_id !=some(select id from my_class); -- 所有结果(null除外)
select * from my_student where c_id !=all(select id from my_class); -- 2(null除外)


select * from my_student where
age = (select max(age) from my_student)
and
height  = (select max(height) from my_student);

-- 行子查询
select * from my_student where 
-- (age,height)称之为行元素
(age,height) = (select max(age),max(height) from my_student);

-- 表子查询
select * from my_student group by c_id order by height desc;
select * from (select * from my_student order by height desc) as student group by c_id;

-- exists子查询
select * from my_student where 
exists(select * from my_class where id = 1);

select * from my_student where 
exists(select * from my_class where id = 2);
```
<br><br>
## 视图

视图: view, 是一种有结构(有行有列)但是没结果(结构中不真实存放数据)的虚拟表, 虚拟表的结构来源不是自己定义, 而是从对应的基表中产生(视图的数据来源).

**创建视图**

基本语法
Create view 视图名字 as select语句; -- select语句可以是普通查询;可以是连接查询; 可以是联合查询; 可以是子查询.

创建单表视图: 基表只有一个
创建多表视图: 基表来源至少两个

<br>

**查看视图**

查看视图: 查看视图的结构

视图是一张虚拟表:  表的所有查看方式都适用于视图: show tables [like]/desc 视图名字/show create table 视图名;
视图比表还是有一个关键字的区别: view. 查看”表(视图)”的创建语句的时候可以使用view关键字

<br>

**修改视图**

视图本身不可修改, 但是视图的来源是可以修改的.

修改视图: 修改视图本身的来源语句(select语句)
Alter view 视图名字 as 新的select语句;

<br>

**删除视图**

Drop view 视图名字;

<br>

**视图意义**

1.	视图可以节省SQL语句: 将一条复杂的查询语句使用视图进行保存: 以后可以直接对视图进行操作
2.	数据安全: 视图操作是主要针对查询的, 如果对视图结构进行处理(删除), 不会影响基表数据(相对安全).
3.	视图往往是在大项目中使用, 而且是多系统使用: 可以对外提供有用的数据, 但是隐藏关键(无用)的数据: 数据安全
4.	视图可以对外提供友好型: 不同的视图提供不同的数据, 对外好像专门设计
5.	视图可以更好(容易)的进行权限控制

**视图数据操作**

视图是的确可以进行数据写操作的: 但是有很多限制
将数据直接在视图上进行操作.

新增数据
数据新增就是直接对视图进行数据新增.
1.	多表视图不能新增数据
2.	可以向单表视图插入数据: 但是视图中包含的字段必须有基表中所有不能为空(或者没有默认值)字段
3.	视图是可以向基表插入数据的.

删除数据
多表视图不能删除数据
单表视图可以删除数据

更新数据
理论上不能单表视图还是多表示视图都可以更新数据.
更新限制: with check option, 如果对视图在新增的时候,限定了某个字段有限制: 那么在对视图进行数据更新操作时,系统会进行验证: 要保证更新之后,数据依然可以被实体查询出来,否则不让更新.

视图算法
视图算法: 系统对视图以及外部查询视图的Select语句的一种解析方式.
视图算法分为三种:
`Undefined`: 未定义(默认的), 这不是一种实际使用算法, 是一种推卸责任的算法: 告诉系统,视图没有定义算法, 系统自己看着办
`Temptable`: 临时表算法: 系统应该先执行视图的select语句,后执行外部查询语句
`Merge`: 合并算法: 系统应该先将视图对应的select语句与外部查询视图的select语句进行合并,然后执行(效率高: 常态)

算法指定: 在创建视图的时候
Create algorithm = 指定算法 view 视图名字 as select语句;

视图算法选择: 如果视图的select语句中会包含一个查询子句(五子句), 而且很有可能顺序比外部的查询语句要靠后, 一定要使用算法temptable,其他情况可以不用指定(默认即可).

<br>

**相关SQL**

```sql
-- 视图: 单表+多表
create view my_v1 as 
select * from my_student;

create view my_v2 as 
select * from my_class;

create view my_v3 as 
select * from my_student as s left join my_class c on s.c_id = c.id; -- id重复


-- 多表视图
create view my_v3 as 
select s.*,c.c_name,c.room from my_student as s 
left join my_class c 
on s.c_id = c.id;

-- 查看视图创建语句
show create view my_v3\G

-- 视图使用
select * from my_v1;
select * from my_v2;
select * from my_v3;

-- 修改视图
alter view my_v1 as
select id,name,age,sex,height,c_id from my_student;

-- 删除视图
drop view my_v4;

-- 多表视图插入数据(失败)
insert into my_v3 values(null,'itcast0008','张三丰','男',150,180,1,'PHP0326','D306');

-- 单表视图插入数据: 视图不包含所有不允许为空字段(学号)
insert into my_v1 values(null,'张无忌',68,'男',174,2);

-- 单表视图插入数据
insert into my_v2 values(2,'PHP0326','D306');

-- 多表视图删除数据(失败)
delete from my_v3 where id = 1;

-- 单表视图删除数据
delete from my_v2 where id = 4;

-- 多表视图更新数据
update my_v3 set c_id = 3 where id = 5;

-- 视图: age字段限制更新
create view my_v4 as 
select * from my_student where age > 30 with check option;
-- 表示视图的数据来源都是年龄大于30岁:where age > 30决定
-- with check option: 决定通过视图更新的时候,不能将已经得到的数据age > 30的改成小于30的

-- 将视图可以查到的数据改成小于30
update my_v4 set age = 29 where id = 1;

-- 可以修改数据让视图可以查到: 可以改,但是无效果
update my_v4 set age = 32 where id = 6;


-- 获取所有班级中最高的一个学生
create view my_v5 as 
select * from my_student order by height desc;

select * from my_v5 group by c_id;

-- 指定算法为临时表
create algorithm=temptable view my_v6 as 
select * from my_student order by height desc;

select * from my_v6 group by c_id;
```

## 数据备份与还原

备份: 将当前已有的数据或者记录保留
还原: 将已经保留的数据恢复到对应的表中

为什么要做备份还原?
1.	防止数据丢失: 被盗, 误操作
2.	保护数据记录


数据备份还原的方式有很多种:
数据表备份, 
单表数据备份, 
SQL备份, 
增量备份.


数据表备份
不需要通过SQL来备份: 直接进入到数据库文件夹复制对应的表结构以及数据文件, 以后还原的时候,直接将备份的内容放进去即可.

数据表备份有前提条件: 根据不同的存储引擎有不同的区别.

存储引擎: mysql进行数据存储的方式: 主要是两种: innodb和myisam(免费)

![MySQL进阶部分要点](http://www.bcoder.top/img/interview/27.png)

对比myisam和innodb: 数据存储方式
Innodb: 只有表结构,数据全部存储到ibdata1文件中
Myisam: 表,数据和索引全部单独分开存储

**单表数据备份**

每次只能备份一张表; 只能备份数据(表结构不能备份)

通常的使用: 将表中的数据进行导出到文件


备份: 从表中选出一部分数据保存到外部的文件中(outfile)
Select */字段列表 into outfile 文件所在路径 from 数据源; -- 前提: 外部文件不存

**SQL备份**

备份的是SQL语句: 系统会对表结构以及数据进行处理,变成对应的SQL语句, 然后进行备份: 还原的时候只要执行SQL指令即可.(主要就是针对表结构)

备份: mysql没有提供备份指令: 需要利用mysql提供的软件: mysqldump.exe
Mysqldump.exe也是一种客户端,需要操作服务器: 必须连接认证
Mysqldump/mysqldump.exe -hPup 数据库名字 [数据表名字1[ 数据表名字2...]] > 外部文件目录(建议使用.sql)


**增量备份**

不是针对数据或者SQL指令进行备份: 是针对mysql服务器的日志文件进行备份


增量备份: 指定时间段开始进行备份., 备份数据不会重复,

而且所有的操作都会备份(大项目都用增量备份)

**SQL还原数据**: 

两种方式还原
方案1: 使用mysql.exe客户端还原
Mysql.exe/mysql -hPup 数据库名字 < 备份文件目录

方案2: 使用SQL指令还原
Source 备份文件所在路径;


```sql
-- SQL 备份
mysqldump -uroot -proot mydatabase my_student > D:/server/temp/student.sql

-- 整库备份
mysqldump -uroot -proot mydatabase > D:/server/temp/mydatabase.sql

-- 还原数据:mysql客户端还原
mysql -uroot -proot mydatabase < D:/server/temp/student.sql

-- SQL 指令还原SQL备份
source D:/server/temp/student.sql;
```
