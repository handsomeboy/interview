# MySQL的联合索引什么情况下可以生效？联合索引和单列索引有什么区别？



## 1. 问题回顾

### 1.1 为什么使用索引

**在无索引的情况下，MySQL会扫描整张表来查找符合sql条件的记录**，其时间开销与表中数据量呈正相关。对关系型数据表中的某些字段建索引可以极大提高查询速度（当然，不同字段是否selective会导致这些字段建立的索引对查询速度的提升幅度不同，而且**索引也并非越多越好，因为写入或删除时需要更新索引信息**）。

对于MySQL的Innodb储存引擎来说，大部分类型的index均以**B-Tree数据结构的变种B+Tree**来存储（MEMORY类型的表还支持hash类型的索引）。B-Tree是数据库或文件系统中常用的一种数据结构，它是一种N叉平衡树，这种树结构保证了同层节点保存的key有序，对于某个节点来说，其左子树保存的所有key均小于该节点保存的key，其右子树保存的所有key均大于该节点保存的key。此外，在工程实现上，还结合操作系统的局部性原理做了很多优化，总之，b-tree的各种特性或优化技巧能保证：

1) 查询磁盘记录时，读盘次数最少；

2) 任何insert和delete操作对树结构的影响均很小；

3) 树本身的rebalance操作很高效。

### 1.2 MySQL使用索引的场景

MySQL在以下操作场景下会使用索引：

1) 快速查找符合where条件的记录

2) 快速确定候选集。**若where条件使用了多个索引字段，则MySQL会优先使用能使候选记录集规模最小的那个索引，以便尽快淘汰不符合条件的记录。**

3) **如果表中存在几个字段构成的联合索引，则查找记录时，这个联合索引的最左前缀匹配字段也会被自动作为索引来加速查找。**
例如，若为某表创建了3个字段(c1, c2, c3)构成的联合索引，则(c1), (c1, c2), (c1, c2, c3)均会作为索引，(c2,c3)就不会被作为索引，而(c1,c3)其实只利用到c1索引。

4) 多表做join操作时会使用索引（如果参与join的字段在这些表中均建立了索引的话）

5) 若某字段已建立索引，求该字段的min()或max()时，MySQL会使用索引

6) 对建立了索引的字段做sort或group操作时，MySQL会使用索引

### 1.3 哪些SQL语句会真正利用索引
从MySQL官网文档"Comparison of B-Tree and Hash Indexes"可知，下面这些类型的SQL可能会真正用到索引：

1) B-Tree可被用于sql中对列做比较的表达式，如=, >, >=, <, <=及between操作

2) 若like语句的条件是**不以通配符开头的常量串**，MySQL也会使用索引
比如，SELECT * FROM tbl_name WHERE key_col LIKE 'Patrick%'或SELECT * FROM tbl_name WHERE key_col LIKE 'Pat%_ck%'可以利用索引，而SELECT * FROM tbl_name WHERE key_col LIKE '%Patrick%'（以通配符开头）和SELECT * FROM tbl_name WHERE key_col LIKE other_col（like条件不是常量串）无法利用索引。
对于形如LIKE '%string%'的sql语句，若通配符后面的string长度大于3，则MySQL会利用Turbo Boyer-Moore algorithm算法进行查找。

3) 若已对名为col_name的列建了索引，则形如"col_name is null"的SQL会用到索引

4) 对于联合索引，sql条件中的最左前缀匹配字段会用到索引

5) 若sql语句中的where条件不只1个条件，则MySQL会进行Index Merge优化来缩小候选集范围

## 2. MySQL的联合索引什么情况下可以生效？

联合索引也就是我们说的组合索引


查询时使用联合索引的一个字段，如果这个字段在联合索引中所有字段的第一个，那就会用到索引，否则就无法使用到索引。

例如联合索引 IDX(字段A,字段B,字段C,字段D)，当仅使用字段A查询时，索引IDX就会使用到；如果仅使用字段B或字段C或字段D查询，则索引IDX都不会用到。  

这个规则在oracle和mysql数据库中均成立。

## 3. 联合索引和单列索引有什么区别？

为了形象地对比两者，建一个表：
```sql
CREATE TABLE myIndex (
 i_testID INT NOT NULL AUTO_INCREMENT, 
vc_Name VARCHAR(50) NOT NULL, 
vc_City VARCHAR(50) NOT NULL, 
i_Age INT NOT NULL, 
i_SchoolID INT NOT NULL, 
PRIMARY KEY (i_testID) 
);
```

在这 10000 条记录里面 7 上 8 下地分布了 5 条vc_Name="erquan" 的记录，只不过 city,age,school 的组合各不相同。

来看这条T-SQL：
```sql
SELECT i_testID FROM myIndex WHERE vc_Name='erquan' AND vc_City='郑州' AND i_Age=25;
```
<br>

首先考虑建MySQL单列索引：

在vc_Name列上建立了索引。执行T-SQL时，MYSQL很快将目标锁定在了vc_Name=erquan 的 5条记录上，取出来放到一中间结果集。在这个结果集里，先排除掉vc_City不等于"郑州"的记录，再排除 i_Age 不等于25的记录，最后筛选出唯一的符合条件的记录。

虽然在 vc_Name 上建立了索引，查询时MYSQL不用扫描整张表，效率有所提高，但离我们的要求还有一定的距离。同样的，在vc_City和i_Age分别建立的MySQL单列索引的效率相似。

**为了进一步榨取 MySQL的效率，就要考虑建立组合索引**。就是将vc_Name,vc_City，i_Age建到一个索引里：

```sql
ALTER TABLE myIndex ADD INDEX name_city_age (vc_Name(10),vc_City,i_Age);
```

建表时，vc_Name 长度为50，这里为什么用10呢？因为一般情况下名字的长度不会超过 10，这样会加速索引查询速度，还会减少索引文件的大小，提高INSERT的更新速度。

执行 T-SQL 时，MySQL 无须扫描任何记录就到找到唯一的记录。

肯定有人要问了，如果分别在 vc_Name,vc_City，i_Age 上建立单列索引，让该表有 3 个单列索引，查询时和上述的组合索引效率一样吗？大不一样，远远低于我们的组合索引。虽然此时有了三个索引，但MySQL只能用到其中的那个它认为似乎是最有效率的单列索引。

建立这样的组合索引，其实是相当于分别建立了(vc_Name,vc_City,i_Age)(vc_Name,_City ) ( vc_Name)

这样的三个组合索引!为什么没有 vc_City，i_Age 等这样的组合索引呢？这是因为 mysql 组合索引“**最左前缀**”的结果。简单的理解就是只从最左面的开始组合。并不是只要包含这三列的查询都会用到该组合索引，下面的几个 T-SQL 会用到：

```sql
SELECT * FROM myIndex WHREE vc_Name="erquan" AND vc_City="郑州"
SELECT * FROM myIndex WHREE vc_Name="erquan"
```

而下面几个则不会用到：

```sql
SELECT * FROM myIndex WHREE i_Age=20 AND vc_City="郑州" 
SELECT * FROM myIndex WHREE vc_City="郑州"
```


