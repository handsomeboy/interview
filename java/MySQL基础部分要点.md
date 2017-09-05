# MySQL基础部分要点


**SQL组成？**

SQL: Structured Query Language, 结构化查询语言(数据以查询为主: 99%是在进行查询操作)

SQL分为三个部分：
**DDL:** Data Definition Language, 数据定义语言, 用来维护存储数据的结构(数据库,表), 代表指令: create, drop, alter等

**DML:** Data Manipulation Language, 数据操作语言, 用来对数据进行操作(数据表中的内容), 代表指令: insert, delete,update等: 其中DML内部又单独进行了一个分类: DQL(Data Query Language: 数据查询语言, 如select)

**DCL:** Data Control Language, 数据控制语言, 主要是负责权限管理(用户), 代表指令: grant,revoke等

SQL是关系型数据库的操作指令, SQL是一种约束,但不强制(类似W3C): 不同的数据库产品(如Oracle,mysql)可能内部会有一些细微的区别.

<br>
**MySQL组成？**

![MySQL基础部分要点](http://www.bcoder.top/img/interview/20.png)


<br>
**MySQL数据库语句**

```sql
-- 创建数据库
create database mydatabase charset utf8;

-- 创建关键字数据库
create database database charset utf8;

-- 使用反引号
create database `database` charset utf8;

-- 创建中文数据库
create database 中国 charset utf8;
create database `中国` charset utf8;

-- 解决方案: 告诉服务器当前中文的字符集是什么
set names gbk;
create database 中国 charset utf8;

-- 查看所有数据库
show databases;

-- 创建数据库
create database informationtest charset utf8;

-- 查看以information_开始的数据库: _需要被转义
show databases like 'information\_%';
show databases like 'information_%';	-- 相当于information%

-- 查看数据库创建语句
show create database mydatabase;
show create database `database`;	-- 关键字需要使用反引号

-- 修改数据库informationtest的字符集
alter database informationtest charset GBK;

-- 删除数据库
drop database informationtest;
```
<br>
**MySQL表命令？**
```sql

-- 创建数据表
-- 进入数据库
use mydatabase;


-- 创建表
create table if not exists mydatabase.student(	-- 显示的将student表放到mydatabase数据库下
name varchar(10),
gender varchar(10),
number varchar(10),
age int
)charset utf8;



-- 创建表
create table class(
name varchar(10),
room varchar(10)
)charset utf8;

-- 查看所有表
show tables;

-- 查看以s结尾的表
show tables like '%s';

-- 查看表创建语句
show create table student\g	-- \g ==== ;
show create table student\G	-- 将查到的结构旋转90度变成纵向

-- 查看表结构
desc class;
describe class;
show columns from class;

-- 重命名表: student表 -> my_student(取数据库名字前两个字母)
rename table student to my_student;

-- 修改表选项: 字符集
alter table my_student charset = GBK;

-- 给学生表增加ID放到第一个位置
alter table my_student
add column id int
first;	-- mysql会自动寻找分号: 语句结束符

-- 将学生表中的number学号字段变成固定长度,且放到第二位(id之后)
alter table my_student
modify number char(10) after id;

-- 修改学生表中的gender字段为sex
alter table my_student
change gender sex varchar(10);

-- 删除学生表中的年龄字段(age)
alter table my_student drop age;

-- 删除数据表
drop table class;

-- 插入数据
insert into my_student values(1,'itcast0001','Jim','male'),
(2,'itcast0002','Hanmeimei','female');

-- 插入数据: 指定字段列表
insert into my_student(number,sex,name,id) values
('itcast0003','male','Tom',3),
('itcast0004','female','Lily',4);

-- 查看所有数据
select * from my_student;

-- 查看指定字段,指定条件数据
select id,number,sex,name from my_student where id = 1;	-- 查看满足id为1的学生信息

-- 更新数据
update my_student set sex  = 'female' where name = 'jim';

-- 删除数据
delete from my_student where sex = 'male';

-- 插入数据(中文)
insert into my_student values(5,'itcast0005','张越','男');
```

<br>
**MySQL字符集**

```sql
-- 查看所有字符集
show character set;

-- 查看服务器默认的对外处理的字符集
show variables like 'character_set%';

-- 修改服务器认为的客户端数据的字符集为GBK
set character_set_client = gbk;

-- 修改服务器给定数据的字符集为GBK
set character_set_results = gbk;

-- 快捷设置字符集
set names gbk;


-- 查看所有校对集
show collation;

-- 创建表使用不同的校对集
create table my_collate_bin(
name char(1)
)charset utf8 collate utf8_bin;

create table my_collate_ci(
name char(1)
)charset utf8 collate utf8_general_ci;

-- 插入数据
insert into my_collate_bin values('a'),('A'),('B'),('b');
insert into my_collate_ci values('a'),('A'),('B'),('b');

-- 排序查找
select * from my_collate_bin order by name;
select * from my_collate_ci order by name;

-- 有数据后修改校对集
alter table my_collate_ci collate = utf8_bin;
alter table my_collate_ci collate = utf8_general_ci;
```

<br><br>
## MySQL数据类型

![MySQL基础部分要点](http://www.bcoder.top/img/interview/21.png)


<br>
**整形**

![MySQL基础部分要点](http://www.bcoder.top/img/interview/22.png)

SQL中的数值类型全部都是默认有符号: 分正负
有时候需要使用无符号数据: 需要给数据类型限定: int unsigned; -- 无符号: 从0开始
<br>
**小数型**

SQL中: 将小数型细分成两种: 浮点型和定点型
浮点型: 小数点浮动, 精度有限,而且会丢失精度
定点型: 小数点固定, 精度固定, 不会丢失精度

> 浮点型

浮点型数据是一种精度型数据: 因为超出指定范围之后, 会丢失精度(自动四舍五入)
浮点型: 理论分为两种精度
Float: 单精度, 占用4个字节存储数据, 精度范围大概为7位左右
Double: 双精度,占用8个字节存储数据, 精度方位大概为15位左右

![MySQL基础部分要点](http://www.bcoder.top/img/interview/23.png)

创建浮点数表: 浮点的使用方式: 直接float表示没有小数部分; float(M,D): M代表总长度,D代表小数部分长度, 整数部分长度为M-D

浮点数一定会进行四舍五入(超出精度范围): 浮点数如果是因为系统进位导致整数部分超出指定的长度,那么系统也允许成立.




> 定点型

定点型: 绝对的保证整数部分不会被四舍五入(不会丢失精度),小数部分有可能(理论小数部分也不会丢失精度)

![MySQL基础部分要点](http://www.bcoder.top/img/interview/24.png)

<br>
**时间日期类型**

 ![MySQL基础部分要点](http://www.bcoder.top/img/interview/25.png)   

时间time可以是负数,而且可以是很大的负数, year可以使用2位数插入,也可以使用4位数
Timestamp字段: 只要当前所在的记录被更新, 该字段一定会自动更新成当前时间

<br>
**字符串类型**

![MySQL基础部分要点](http://www.bcoder.top/img/interview/26.png)   


在SQL中,将字符串类型分成了6类: char,varchar,text , blob, enum和set

定长字符串: char, 磁盘(二维表)在定义结构的时候,就已经确定了最终数据的存储长度.最大长度值可以为255.

变长字符串: varchar, 在分配空间的时候, 按照最大的空间分配,但是实际上最终用了多少,是根据具体的数据来确定.理论长度是65536个字符.但是实际上如果长度超过255,既不用定长也不用变长,使用文本字符串text

枚举的使用方式:
定义: enum(可能出现的元素列表);	//如enum(‘男’,’女’,’不男不女’,’妖’,’保密’);
使用: 存储数据,只能存储上面定义好的数据


集合字符串
集合跟枚举很类似: 实际存储的是数值,而不是字符串(集合是多选)

集合使用方式:
定义: Set(元素列表)
使用: 可以使用元素列表中的元素(多个), 使用逗号分隔

<br>
**类型操作相关SQL**
```sql
-- 创建时间日期表
create table my_date(
d1 datetime,
d2 date,
d3 time,
d4 timestamp,
d5 year
)charset utf8;


-- 插入数据
insert into my_date values('2015-9-28 11:50:36','2015-9-28','11:50:54','2015-9-28 11:51:08',2015);

-- 创建枚举表
create table my_enum(
gender enum('男','女','保密')
)charset utf8;

-- 插入数据
insert into my_enum values('男'),('保密'); -- 有效数据

-- 错误数据
insert into my_enum values('male');	-- 错误: 没有该元素

-- 将字段结果取出来进行+0运算
select gender + 0, gender from my_enum;

-- 数值插入枚举元素
insert into my_enum values(1),(2);

-- 创建集合表
create table my_set(
hobby set('篮球','足球','乒乓球','羽毛球','排球','台球','网球','棒球')
--		   足球			          台球    网球
-- 集合中: 每一个元素都是对应一个二进制位,被选中为1,没有则为0: 最后反过来
--          0      1       0         0      0       1       1     0
-- 反过来    01100010 = 98

)charset utf8;

-- 插入数据
insert into my_set values('足球,台球,网球');
insert into my_set values(3); 

```
<br><br>
## 列属性
NULL(默认的)
NOT NULL(不为空)
comment, 描述, 没有实际含义: 是专门用来描述字段,会根据表创建语句保存: 用来给程序猿(数据库管理员)来进行了解的.
default,某一种数据会经常性的出现某个具体的值, 可以在一开始就指定好: 在需要真实数据的时候,用户可以选择性的使用默认值.

**主键:** primary key,主要的键. 一张表只能有一个字段可以使用对应的键, 用来唯一的约束该字段里面的数据, 不能重复,这种称之为主键.一张表只能有最多一个主键.
```sql
-- 增加主键

create table my_pri1(
name varchar(20) not null comment '姓名',
number char(10) primary key comment '学号: itcast + 0000, 不能重复'
)charset utf8;

-- 复合主键
create table my_pri2(
number char(10) comment '学号: itcast + 0000',
course char(10) comment '课程代码: 3901 + 0000',
score tinyint unsigned default 60 comment '成绩',
-- 增加主键限制: 学号和课程号应该是个对应的,具有唯一性
primary key(number,course)
)charset utf8;

-- 追加主键
create table my_pri3(
course char(10) not null comment '课程编号: 3901 + 0000',
name varchar(10) not null comment '课程名字'
);

alter table my_pri3 modify course char(10) primary key comment '课程编号: 3901 + 0000';
alter table my_pri3 add primary key(course);

-- 向pri1表插入数据
insert into my_pri1 values('古学星','itcast0001'),('蔡仁湾','itcast0002');
insert into my_pri2 values('itcast0001','39010001',90),('itcast0001','39010002',85),('itcast0002','39010001',92);

-- 主键冲突(重复)
insert into my_pri1 values('刘辉','itcast0002'); -- 不可以: 主键冲突
insert into my_pri2 values('itcast0001','39010001',100); -- 不可以:冲突

-- 删除主键
alter table my_pri3 drop primary key;
```

**自动增长:**当对应的字段,不给值,或者说给默认值,或者给NULL的时候, 会自动的被系统触发, 系统会从当前字段中已有的最大值再进行+1操作,得到一个新的在不同的字段.自增长通常是跟主键搭配.
1.	任何一个字段要做自增长必须前提是本身是一个索引(key一栏有值)
2.	自增长字段必须是数字(整型)
3.	一张表最多只能有一个自增长
```sql

-- 自增长
create table my_auto(
id int primary key auto_increment comment '自动增长',
name varchar(10) not null
)charset utf8;

-- 触发自增长
insert into my_auto(name) values('邓立军');
insert into my_auto values(null,'龚森');
insert into my_auto values(default,'张滔');

-- 指定数据
insert into my_auto values(6,'何思华');
insert into my_auto values(null,'陈少炼');


-- 修改表选项的值
alter table my_auto auto_increment = 4; -- 向下修改(小)
alter table my_auto auto_increment = 10; -- 向上修改 

-- 查看自增长变量
show variables like 'auto_increment%';

-- 修改自增长步长
set auto_increment_increment = 5;

-- 插入记录: 使用自增长
insert into my_auto values(null,'刘阳');
insert into my_auto values(null,'邓贤师');

-- 删除自增长
alter table my_auto modify id int primary key;
```
**唯一键约束:**唯一键与主键本质相同: 唯一的区别就是唯一键默认允许为空,而且是多个为空.
如果唯一键也不允许为空: 与主键的约束作用是一致的.

```sql
-- 唯一键
create table my_unique1(
number char(10) unique comment '学号: 唯一,允许为空',
name varchar(20) not null
)charset utf8;

create table my_unique2(
number char(10) not null comment '学号',
name varchar(20) not null,
-- 增加唯一键
unique key(number)
)charset utf8;


-- 追加唯一键
alter table my_unique3 add unique key(number);

-- 删除唯一键
alter table my_unique3 drop index number;
```
<br><br>
## 数据库三大范式

范式: Normal Format, 是一种离散数学中的知识, 是为了解决一种数据的存储与优化的问题: 保存数据的存储之后, 凡是能够通过关系寻找出来的数据,坚决不再重复存储: 终极目标是为了减少数据的冗余.

Mysql属于关系型数据库: 有空间浪费: 也是致力于节省存储空间: 与范式所有解决的问题不谋而合: 在设计数据库的时候, 会利用到范式来指导设计.

但是数据库不单是要解决空间问题,要保证效率问题: 范式只为解决空间问题, 所以数据库的设计又不可能完全按照范式的要求实现: 一般情况下,只有前三种范式需要满足.


**第一范式**
在设计表存储数据的时候, 如果表中设计的字段存储的数据,在取出来使用之前还需要额外的处理(拆分),那么说表的设计不满足第一范式: 第一范式要求字段的数据具有原子性: 不可再分.

<br>
**第二范式**
 在数据表设计的过程中,如果有复合主键(多字段主键), 且表中有字段并不是由整个主键来确定, 而是依赖主键中的某个字段(主键的部分): 存在字段依赖主键的部分的问题, 称之为部分依赖: 第二范式就是要解决表设计不允许出现部分依赖.
 
 <br>
 **第三范式**
要满足第三范式,必须满足第二范式.

第三范式: 理论上讲,应该一张表中的所有字段都应该直接依赖主键(逻辑主键: 代表的是业务主键), 如果表设计中存在一个字段, 并不直接依赖主键,而是通过某个非主键字段依赖,最终实现依赖主键: 把这种不是直接依赖主键,而是依赖非主键字段的依赖关系称之为传递依赖. 第三范式就是要解决传递依赖的问题.

## 数据库高级操作
**主键冲突**
当主键存在冲突的时候(Duplicate key),可以选择性的进行处理: 更新和替换

主键冲突: 更新操作
Insert into 表名[(字段列表:包含主键)] values(值列表) on duplicate key update 字段 = 新值;

<br>
**蠕虫复制**
蠕虫复制: 从已有的数据中去获取数据,然后将数据又进行新增操作: 数据成倍的增加.

表创建高级操作: 从已有表创建新表(复制表结构)
Create table 表名 like 数据库.表名;

蠕虫复制: 先查出数据, 然后将查出的数据新增一遍
Insert into 表名[(字段列表)] select 字段列表/* from 数据表名;

蠕虫复制的意义
1.	从已有表拷贝数据到新表中
2.	可以迅速的让表中的数据膨胀到一定的数量级: 测试表的压力以及效率
<br>
**更新数据**
基本语法
Update 表名 set 字段 = 值 [where条件];

高级新增语法
Update 表名 set 字段 = 值 [where条件] [limit 更新数量];

<br>
**删除数据**
与更新类似: 可以通过limit来限制数量
Delete from 表名 [where条件] [limit 数量];

数据的删除是不会改变表结构, 只能删除表后重建表
Truncate 表名;	-- 先删除改变,后新增改变


```sql

-- 插入数据
insert into my_class values('PHP0810','B203');
insert into my_class values('PHP0810','B205');
insert into my_class values('PHP0710','B203');

-- 主键冲突: 更新
insert into my_class values('PHP0810','B205')
-- 冲突处理
on duplicate key update
-- 更新教室
room = 'B205';

-- 主键冲突:替换
replace into my_class values('PHP0710','A203');
replace into my_class values('PHP0910','B207');

-- 复制创建表
create table my_copy like my_gbk;

-- 蠕虫复制
insert into my_copy select * from my_collate_bin;
insert into my_copy select * from my_copy;


-- 更新部分a变成c
update my_copy set name = 'c' where name = 'a' limit 3;


-- 删除数据:限制记录数为10
delete from my_copy where name = 'b' limit 10;

-- 清空表: 重置自增长
truncate my_student;

```


数据库下，UTF编码一个汉字三个字节，GBK编码一个汉字两个字节。
校对集有三种格式
_bin: binary,二进制比较, 取出二进制位,一位一位的比较, 区分大小写
_cs: case sensitive,大小写敏感, 区分大小写
_ci: case insensitice,大小写不敏感,不区分大小写


