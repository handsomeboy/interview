# MySQL高级部分要点



## 事务

事务: transaction, 一系列要发生的连续的操作
事务安全: 一种保护连续操作同时满足(实现)的一种机制

事务安全的意义: 保证数据操作的完整性


<br>

**事务操作**

事务操作分为两种: 自动事务(默认的), 手动事务

1.	开启事务: 告诉系统以下所有的操作(写)不要直接写入到数据表, 先存放到事务日志
Start transaction;
 
2.	进行事务操作: 一系列操作

 
3.	关闭事务: 选择性的将日志文件中操作的结果保存到数据表(同步)或者说直接清空事务日志(原来操作全部清空)
a)	提交事务: 同步数据表(操作成功): commit;
 
b)	回滚事务: 直接清空日志表(操作失败): rollback;

<br>

**事务原理**


事务操作原理: 事务开启之后, 所有的操作都会临时保存到事务日志, 事务日志只有在得到commit命令才会同步到数据表,其他任何情况都会清空(rollback, 断电, 断开连接)

![MySQL高级部分要点](http://www.bcoder.top/img/interview/28.png)

<br>

**回滚点**

回滚点: 在某个成功的操作完成之后, 后续的操作有可能成功有可能失败, 但是不管成功还是失败,前面操作都已经成功: 可以在当前成功的位置, 设置一个点: 可以供后续失败操作返回到该位置, 而不是返回所有操作, 这个点称之为回滚点. 

设置回滚点语法: savepoint 回滚点名字;
回到回滚点语法: rollback to 回滚点名字;

<br>

**自动事务处理**

在mysql中: 默认的都是自动事务处理, 用户操作完会立即同步到数据表中.

自动事务: 系统通过autocommit变量控制
Show variables like ‘autocommit’;

关闭自动提交: set autocommit = off/0;

自动关闭之后,需要手动来选择处理: commit提交, rollback回滚

注意: 通常都会使用自动事务

<br>

**事务特性**

事务有四大特性: ACID

A: Atomic原子性, 事务的整个操作是一个整体, 不可分割,要么全部成功,要么全部失败;

C: Consistency, 一致性, 事务操作的前后, 数据表中的数据没有变化

I: Isolation, 隔离性, 事务操作是相互隔离不受影响的.

D: Durability, 持久性, 数据一旦提交, 不可改变,永久的改变数据表数据

<br>

**锁机制** 

**innodb默认是行锁**, 但是如果在事务操作的过程中, 没有使用到索引,那么系统会自动全表检索数据, 自动升级为表锁
行锁: 只有当前行被锁住, 别的用户不能操作
表锁: 整张表被锁住, 别的用户都不能操作

<br><br>

## 变量

变量分为两种: 系统变量和自定义变量

**系统变量**

系统定义好的变量: 大部分的时候用户根本不需要使用系统变量: 系统变量是用来控制服务器的表现的: 如autocommit, auto_increment_increment等

> 查看系统变量
Show variables; -- 查看所有系统变量
 

查看具体变量值: 任何一个有数据返回的内容都是由select查看
Select @@变量名;
 

>修改系统变量

修改系统变量分为两种方式: 会话级别和全局级别

会话级别: 临时修改, 当前客户端当次连接有效
Set 变量名 = 值;/Set @@变量名 = 值;
 
全局级别: 一次修改,永久生效(对所有客户端都生效)
Set global 变量名 = 值;
 

如果对方(其他)客户端当前已经连上服务器,那么当次修改无效,要退出重新登录才会生效

>自定义变量

系统为了区分系统变量, 规定用户自定义变量必须使用一个@符号
Set @变量名 = 值;
 
> 自定义变量也是类似系统变量查看
Select @变量名;
 
在mysql中, “=”会默认的当做比较符号处理(很多地方), mysql为了区分比较和赋值的概念: 重新定义了一个新的的赋值符号: ` :=`
 

Mysql允许从数据表中获取数据,然后赋值给变量: 两种方式

方案1: 边赋值,边查看结果
Select @变量名 := 字段名 from 数据源; -- 从字段中取值赋值给变量名, 如果使用=会变成比较
 

方案2: 只有赋值不看结果: 要求很严格: 数据记录最多只允许获取一条: mysql不支持数组
Select 字段列表 from 表名 into 变量列表;
 

所有自定义的变量都是会话级别: 当前客户端当次连接有效
所有自定义变量不区分数据库(用户级别)

**相关SQL：**

```sql

-- 查看所有系统变量
show variables;

-- 查看系统变量值
select @@version,@@autocommit,@@auto_increment_offset,@@character_set_results;

-- 修改会话级别变量
set autocommit = 0;

-- 修改会话级别变量
set @@autocommit = 0;

-- 修改全局级别变量
set global autocommit = 0;


-- 定义自定义变量
set @name = '张三';

-- 查看变量
select @name;

-- 定义变量
set @age := 18;

-- 从表中获取数据赋值给变量
select @name := name,name from my_student;

select name,age from my_student where id = 2 into @name,@age;
```

## 触发器
触发器: trigger, 事先为某张表绑定好一段代码 ,当表中的某些内容发生改变的时候(增删改)系统会自动触发代码,执行.

**触发器: 事件类型, 触发时间, 触发对象**
事件类型: 增删改, 三种类型`insert,delete和update`
触发时间: 前后: `before和after`
触发对象: 表中的每`一条记录(行)`

一张表中只能拥有一种触发时间的一种类型的触发器: 最多一张表能有`6个`触发器

创建触发器
在mysql高级结构中: 没有大括号,  都是用对应的字符符号代替

<br>

**触发器基本语法**

-- 临时修改语句结束符
Delimiter 自定义符号: 后续代码中只有碰到自定义符号才算结束

Create trigger 触发器名字 触发时间 事件类型 on 表名 for each row
Begin		-- 代表左大括号: 开始
-- 里面就是触发器的内容: 每行内容都必须使用语句结束符: 分号
End			-- 代表右带括号: 结束
-- 语句结束符
自定义符号

-- 将临时修改修正过来
Delimiter  ;

<br>

**查看触发器**

查看所有触发器或者模糊匹配
Show triggers [like ‘pattern’];

可以查看触发器创建语句
Show create trigger 触发器名字;

所有的触发器都会保存一张表中: Information_schema.triggers

<br>

**使用触发器**

触发器: 不需要手动调用, 而是当某种情况发生时会自动触发.(例如订单里面插入记录之后)
 
 
 <br>

**修改触发器&删除触发器**

触发器不能修改,只能先删除,后新增.

Drop trigger 触发器名字;

<br>

**触发器记录**


触发器记录: 不管触发器是否触发了,只要当某种操作准备执行, 系统就会将当前要操作的记录的当前状态和即将执行之后新的状态给分别保留下来, 供触发器使用: 其中, 要操作的当前状态保存到old中, 操作之后的可能形态保存给new.

`old代表的是旧记录,new代表的是新记录`
删除的时候是没有new的; 插入的时候是没有old

Old和new都是代表记录本身: 任何一条记录除了有数据, 还有字段名字.
使用方式: old.字段名 / new.字段名(new代表的是假设发生之后的结果)

如果触发器内部只有一条要执行的SQL指令, 可以省略大括号(begin和end)
`Create trigger 触发器名字 触发时间 事件类型 on 表名 for each row
一条SQL指令;`

<br>

**相关SQL:**

```sql

-- 触发器: 订单生成一个, 商品库存减少

-- 临时修改语句结束符
delimiter $$

create trigger after_order after insert on my_order for each row
begin
    -- 触发器内容开始
    update my_goods set inv = inv - 1 where id = 2;
   
end
-- 结束触发器
$$

-- 修改临时语句结束符
delimiter  ;

-- 查看所有触发器
show triggers\G

-- 查看触发器创建语句
show create trigger after_order\G

-- 插入订单
insert into my_order values(null,1,2);

-- 删除触发器
drop trigger after_order;


-- 触发器: 订单生成一个, 商品库存减少

-- 临时修改语句结束符
delimiter $$

create trigger after_order after insert on my_order for each row
begin
    -- 触发器内容开始: 新增一条订单: old没有,new代表新的订单记录
    update my_goods set inv = inv - new.g_number where id = new.g_id;
   
end
-- 结束触发器
$$

-- 修改临时语句结束符
delimiter  ;

-- 触发器: 订单生成之前要判断库存是否满足

```

## 代码执行结构

if分支基本语法

If  条件判断  then
-- 满足条件要执行的代码;
Else
-- 不满足条件要执行的代码;
End if;
<br>


While循环(没有for循环)

While 条件判断 do
-- 满足条件要执行的代码
-- 变更循环条件
End while;

```sql
create trigger before_order before insert on my_order for each row
begin
	-- 判断商品库存是否足够

	-- 获取商品库存: 商品库存在表中
	select inv from my_goods where id = new.g_id into @inv;

	-- 比较库存
	if @inv < new.g_number then
		-- 库存不够: 触发器没有提供一个能够阻止事件发生的能力(暴力报错)
		insert into XXX values(XXX);
		
	end if;

end
%%
-- 改回语句结束符
delimiter ;
```
<br><br>
## 函数
函数: 将一段代码块封装到一个结构中, 在需要执行代码块的时候, 调用结构执行即可.(代码复用)

函数分为两类: 系统函数和自定义函数
<br>

**系统函数**

系统定义好的函数, 直接调用即可.
任何函数都有返回值, 因此函数的调用是通过select调用.

Mysql中,字符串的基本操作单位(最常见的是字符)
Substring: 字符串截取(字符为单位)
char_length: 字符长度
Length: 字节长度
Instr: 判断字符串是否在某个具体的字符串中存在, 存在返回位置
Lpad: 左填充, 将字符串按照某个指定的填充方式,填充到指定长度(字符)
Insert: 替换,找到目标位置,指定长度的字符串,替换成目标字符串
Strcmp: compare,字符串比较
<br>

**自定义函数**

函数要素: 函数名, 参数列表(形参和实参), 返回值, 函数体(作用域)

创建函数


Create function  函数名([形参列表]) returns 数据类型 -- 规定要返回的数据类型
Begin
-- 函数体
-- 返回值: return 类型(指定数据类型);
End


`定义函数`
自定义函数与系统函数的调用方式是一样: select 函数名([实参列表]);
 

`查看函数`
查看所有函数: show function status [like ‘pattern’];
 

查看函数的创建语句: show create function 函数名;
 
`修改函数&删除函数`
函数只能先删除后新增,不能修改.

Drop function 函数名;
 

`函数参数`

参数分为两种: 定义时的参数叫形参, 调用时的参数叫实参(实参可以是数值也可以是变量)
形参: 要求必须指定数据类型
Function 函数名(形参名字 字段类型) returns 数据类型
 

在函数内部使用@定义的变量在函数外部也可以访问
 

作用域
Mysql中的作用域与js中的作用域完全一样
全局变量可以在任何地方使用; 局部变量只能在函数内部使用.

全局变量: 使用set关键字定义, 使用@符号标志
局部变量: 使用declare关键字声明, 没有@符号: 所有的局部变量的声明,必须在函数体开始之前



```sql

-- 定义两个变量
set @cn = '世界你好';
set @en = 'hello world';

-- 字符串截取
select substring(@cn,1,1);
select substring(@en,1,1);
-- 字符串长度
select char_length(@cn),char_length(@en),length(@cn),length(@en);

-- 字符串寻找
select instr(@cn,'界'),instr(@en,'ll'),instr(@cn,'拜拜');

-- 字符串填充
select lpad(@cn,20,'欢迎'),lpad(@en,20,'hello');

-- 字符串替换
select insert(@en,3,3,'y'),@en;


-- 字符串比较
set @f = 'hello';
set @s = 'hey';
set @t = 'HEY';

select strcmp(@f,@s),strcmp(@s,@t),strcmp(@s,@f);

-- 创建函数
create function display1() returns int
return 100;

-- 调用函数
select display1();

-- 查看所有函数
show function status\G

-- 查看函数创建语句
show create function display1;

-- 删除函数
drop function display1;

-- 做函数: 计算1-指定数之间的和
delimiter $$
create function display1(int_1 int) returns int
begin
	-- 定义条件变量
	set @i = 1;	-- @定义的变量是全局变量,没有的可以理解为局部变量
	set @res = 0; -- 保存结果

	-- 循环求和
	while @i <= int_1 do
		-- 求和: 任何变量要修改必须使用set关键字
		-- mysql中没有+=,没有++
		set @res = @res + @i;

		-- 修改循环变量
		set @i = @i + 1;
	end while;

	-- 返回值
	return @res;
end
$$	-- 函数结束
delimiter ;


-- 求和: 1-到指定数之间的和,要求5的倍数不加
delimiter $$
create function display2(int_1 int) returns int
begin
	-- 声明变量: 循环变量, 结果变量
	declare i int default 1;
	declare res int default 0; -- 定义局部变量可以有属性

	-- 循环判断
	mywhile:while i <= int_1 do
		-- 相加: 判断
		if i % 5 = 0 then
			-- 修改循环条件
			set i = i + 1;
			-- 不符合条件: 循环重新来过
			iterate mywhile;
		end if;

		-- 相加
		set res = res + i;

		-- 改变循环变量
		set i = i + 1;
	
	end while;

	-- 返回结果
	return res;
end
$$
delimiter ;

create function display3() returns int
return 'a'; -- 错误: 编译阶段不会出错,执行阶段出错

```

<br><br>

## 存储过程

存储过程简称过程,procedure, 是一种用来处理数据的方式.
存储过程是一种没有返回值的函数.

<br>

**创建过程**
Create procedure 过程名字([参数列表])
Begin
-- 过程体
End
 
<br>

**查看过程**

函数的查看方式完全适用于过程: 关键字换成procedure

查看所有过程: show procedure status [like ‘pattern’];
 
查看过程创建语句: show create procedure 过程名;
 
<br>

**调用过程**

过程没有返回值: select是不能访问的.
 

过程有一个专门的调用关键字: call
 

<br>

**修改过程&删除过程**

过程只能先删除,后新增

Drop procedure 过程名;
 

<br>

**过程参数**
函数的参数需要数据类型指定, 过程比函数更严格.

过程还有自己的类型限定: 三种类型
`In:` 数据只是从外部传入给内部使用(值传递): 可以是数值也可以是变量
`Out:` 只允许过程内部使用(不用外部数据), 给外部使用的.(引用传递: 外部的数据会被先清空才会进入到内部): 只能是变量
`Inout:` 外部可以在内部使用,内部修改也可以给外部使用: 典型的引用传递: 只能传变量

基本使用
Create procedure 过程名(in 形参名字 数据类型, out 形参名字 数据类型, inout 形参名字 数据类型)
 

调用: out和inout类型的参数必须传入变量,而不能是数值
 

正确调用: 传入变量
 


存储过程对于变量的操作(返回)是滞后的: 是在存储过程调用结束的时候,才会重新将内部修改的值赋值给外部传入的全局变量.
 

测试: 传入数据1,2,3: 说明局部变量与全局变量无关
 

最后: 在存储过程调用结束之后, 系统会将局部变量重复返回给全局变量(out和inout)

<br>

**相关SQL：**

```sql

-- 创建存储过程
create procedure pro1() -- 假设过程中需要显示数据: 使用select
select * from my_student;


-- 查看过程
show procedure status like 'pro%'\G

-- 查看过程创建语句
show create procedure pro1;

-- 调用过程
call pro1();

-- 删除过程
drop procedure pro1;

-- 过程参数
delimiter $$
create procedure pro1(in int_1 int,out int_2 int,inout int_3 int)
begin
	-- 先查看三个变量
	select int_1,int_2,int_3; -- int_2的值一定是NULL
end
$$
delimiter ;

-- 设置变量
set @int_1 := 1;
set @int_2 := 2;
set @int_3 := 3;

select @int_1,@int_2,@int_3;
call pro1(@int_1,@int_2,@int_3);
select @int_1,@int_2,@int_3;


-- 过程参数
delimiter $$
create procedure pro2(in int_1 int,out int_2 int,inout int_3 int)
begin
	-- 先查看三个变量
	select int_1,int_2,int_3; -- int_2的值一定是NULL(三个当前是局部变量)

	-- 修改局部变量
	set int_1 = 10;
	set int_2 = 100;
	set int_3 = 1000;

	-- 查看局部变量
	select int_1,int_2,int_3; 

	-- 查看全局变量
	select @int_1,@int_2,@int_3; 

	-- 修改全局变量
	set @int_1 = 'a';
	set @int_2 = 'b';
	set @int_3 = 'c';

	-- 查看全局变量
	select @int_1,@int_2,@int_3; 
end
$$
delimiter ;

-- 设置变量
set @int_1 := 1;
set @int_2 := 2;
set @int_3 := 3;

call pro2(@int_1,@int_2,@int_3);
```