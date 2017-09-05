# MySQL索引分为哪几种？


## 数据库为什么要有索引？
一个简单示例，20多条数据源随机生成200万条数据，平均每条数据源都重复大概10万次，表结构比较简单，仅包含一个自增ID，一个char类型，一个text类型和一个int类型，单表2G大小，使用MyIASM引擎。开始测试未添加任何索引。
执行下面的SQL语句：
```sql
mysql> SELECT id,FROM_UNIXTIME(time) FROM article WHERE a.title='测试标题'
```

查询需要的时间非常恐怖的，如果加上联合查询和其他一些约束条件，数据库会疯狂的消耗内存，并且会影响前端程序的执行。这时给title字段添加一个BTREE索引：

```sql
mysql> ALTER TABLE article ADD INDEX index_article_title ON title(200);
```

再次执行上述查询语句，其对比非常明显：




<br><br>


## MySQL索引的概念

索引是一种特殊的文件(InnoDB数据表上的索引是表空间的一个组成部分)，它们包含着对数据表里所有记录的引用指针。

更通俗的说，数据库索引好比是一本书前面的目录，能加快数据库的查询速度。

上述SQL语句，在没有索引的情况下，数据库会遍历全部200条数据后选择符合条件的；而有了相应的索引之后，数据库会直接在索引中查找符合条件的选项。

如果我们把SQL语句换成“SELECT * FROM article WHERE id=2000000”，那么你是希望数据库按照顺序读取完200万行数据以后给你结果还是直接在索引中定位呢？上面的两个图片鲜明的用时对比已经给出了答案（注：一般数据库默认都会为主键生成索引）。
<br>
**1. 普通索引(index)**

这是最基本的索引，它没有任何限制，比如上文中为title字段创建的索引就是一个普通索引，MyIASM中默认的BTREE类型的索引，也是我们大多数情况下用到的索引。
```sql
–直接创建索引
CREATE INDEX index_name ON table(column(length))
–修改表结构的方式添加索引
ALTER TABLE table_name ADD INDEX index_name (column(length))
–创建表的时候同时创建索引
CREATE TABLE `table` (
`id` int(11) NOT NULL AUTO_INCREMENT ,
`title` char(255) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL ,
`content` text CHARACTER SET utf8 COLLATE utf8_general_ci NULL ,
`time` int(10) NULL DEFAULT NULL ,
PRIMARY KEY (`id`),
INDEX index_name (title(length))
)
–删除索引
DROP INDEX index_name ON table
```
<br>
**2. 唯一索引(unique)**

与普通索引类似，不同的就是：索引列的值必须唯一，但**允许有空值**（注意和主键不同）。如果是组合索引，则列值的组合必须唯一，创建方法和普通索引类似。

```sql
–创建唯一索引
CREATE UNIQUE INDEX indexName ON table(column(length))
–修改表结构
ALTER TABLE table_name ADD UNIQUE indexName ON (column(length))
–创建表的时候直接指定
CREATE TABLE `table` (
`id` int(11) NOT NULL AUTO_INCREMENT ,
`title` char(255) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL ,
`content` text CHARACTER SET utf8 COLLATE utf8_general_ci NULL ,
`time` int(10) NULL DEFAULT NULL ,
PRIMARY KEY (`id`),
UNIQUE indexName (title(length))
);
```

<br>
**3. 全文索引（FULLTEXT）**
MySQL从3.23.23版开始支持全文索引和全文检索，**FULLTEXT索引仅可用于MyISAM 表**；他们可以从CHAR、VARCHAR或TEXT列中作为CREATE TABLE语句的一部分被创建，或是随后使用ALTER TABLE 或CREATE INDEX被添加。

对于较大的数据集，将你的资料输入一个没有FULLTEXT索引的表中，然后创建索引，其速度比把资料输入现有FULLTEXT索引的速度更为快。不过切记对于大容量的数据表，生成全文索引是一个非常消耗时间非常消耗硬盘空间的做法。


```sql
–创建表的适合添加全文索引
CREATE TABLE `table` (
`id` int(11) NOT NULL AUTO_INCREMENT ,
`title` char(255) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL ,
`content` text CHARACTER SET utf8 COLLATE utf8_general_ci NULL ,
`time` int(10) NULL DEFAULT NULL ,
PRIMARY KEY (`id`),
FULLTEXT (content)
);
–修改表结构添加全文索引
ALTER TABLE article ADD FULLTEXT index_content(content)
–直接创建索引
CREATE FULLTEXT INDEX index_content ON article(content)
```
<br>
**4. 单列索引、多列索引**

多个单列索引与单个多列索引的查询效果不同，因为执行查询时，MySQL只能使用一个索引，会从多个索引中选择一个限制最为严格的索引。
<br>
**5. 组合索引（最左前缀）**

平时用的SQL查询语句一般都有比较多的限制条件，所以为了进一步榨取MySQL的效率，就要考虑建立组合索引。例如上表中针对title和time建立一个组合索引：ALTER TABLE article ADD INDEX index_titme_time (title(50),time(10))。建立这样的组合索引，其实是相当于分别建立了下面两组组合索引：

–title,time

–title

为什么没有time这样的组合索引呢？这是因为MySQL组合索引“最左前缀”的结果。简单的理解就是只从最左面的开始组合。并不是只要包含这两列的查询都会用到该组合索引，如下面的几个SQL所示
```sql
–使用到上面的索引
SELECT * FROM article WHREE title='测试' AND time=1234567890;
SELECT * FROM article WHREE utitle='测试';
–不使用上面的索引
SELECT * FROM article WHREE time=1234567890;
```

<br>
**6.主键索引**

它是一种特殊的唯一索引，不允许有空值。一般是在建表的时候同时创建主键索引：
```aql
CREATE TABLE mytable( ID INT NOT NULL, username VARCHAR(16) NOT NULL, PRIMARY KEY(ID) );  
```
当然也可以用 ALTER 命令。记住：一个表只能有一个主键。