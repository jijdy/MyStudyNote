### sql的常用语法和意义



#### 1. 创建表（create

~~~sql
CREATE TABLE `tablename`(
    # id为bigint类型，不为空，自增，一般作为主键
	`id` bigint not null auto_increment,
    
    `column1` int not null default 1,
    #varchar相当于String，其自设置字符串的长度，null表示可以为空，一般默认不写
    `column2` varchar(20) null,
    `column3` date null, -- date是sql中的一个日期类，表示时间
    primary key(`id`)
) engine=innobd, default charset=utf8mb4, comment='table表的注释';
~~~

* 其中，表的创建一般可以指定引擎和编码集，以提高之后sql代码的复用性，多使用comment加注释是个好习惯

#### 2. 修改表中的列（alter

* alter：表示修改一个表中的列数据

~~~sql
# 向一个表中添加列，指明该列的属性
ALTER TABLE `tablename`
	ADD `newcolnme` char(10) comment='新添加的列';
	
# 删除一个表中的列
ALTER TABLE `teblename`
	DROP COLUMN `columnname`;
	
# 删除表
DROP TABLE `tablename`;
~~~

#### 3. 向表中插入数据（insert into

~~~sql
# 直接向表中的列插入数据，相当于更新了一行数据，
INSERT INTO `tablename`(`column1`,`column2`)
	VALUES(val1, val2);

# 插入检索出来的数据，从另一个表中拿出数据，插入到这个表中
INSERT INTO `tablename`(`column1`,`column2`)
	SELECT `column11`,`column22`
	FROM `tablename2`;
	
# 将一个表中的内容插到一个新表中
CREATE TABLE `tablename` AS
	SELECT * FROM `tablename2`;
~~~

#### 4. 更新表中数据update

~~~sql
# 更新表中的列数据，一般指定条件
UPDATE `tablename`
	SET `column1` = 'val'
	WHERE `id`=1;
~~~

#### 5. 删除表中行数据（delete

* 删除时一定记得添加whree限定，否则有可能删除整个表数据

~~~sql
# 删除限定条件所在的一行
DELETE FROM `tablename`
	WHERE `id`=1;

# 删除表中的所有数据，清空表
TRUNCATE TABLE `tablename`;
~~~

#### 6. 查询数据（select

~~~sql
# distinct 相同的值只会出现一次，需要所有列都相同才会相同
select distinct `column1` `column2` from `table`;

# limit限定条件，用于分页查询逻辑
select * from `table` limit 5, 5; -- 表示从表的第5行开始，向后查询5个数据
~~~

#### 7. 排序 （order by

~~~sql
# asc：升序，默认为该值
# desc：降序，从大向下
# 通过选择表中的列数据，将查询出的数据进行排序，若有多个排序，则只有第一个有效。
select * from `tablename` order by `cloumn1` asc, --`cloumn2` desc--;
~~~

#### 8. 过滤 （where

* **where**中使用**and**和**or**来连接多个过滤条件
* **in**用于匹配一组值，让两列的数据相匹配，in之后跟子查询语句
* **not**用于否定一个条件

~~~sql
select * from `tablenam`
	where `cloumnx` is null; //null值用is来进行等于的判断
~~~

#### 9. 通配符（like

* 用于**where**从句中，但支农用于查询文本字段(字符串字段)
* **%** 匹配 >=0 个任意字符
* **_** 匹配 ==1 个任意字符
* **[]** 可以匹配集合中的字符，也可以使用[^]来表示不匹配括号中的字符

~~~sql
select * from `tablename`
	where `cloumn` like '[^ab]%'; -- 表示匹配字段中比以ab开头的字符段
~~~

#### 10. 计算字段（as,concat

* 可以使用**as**将表或列，或者多列运算后的结果取别名
* **concat()**可将两个列连接在一起，可以使用**trim()**函数去除列前或列后的空格，相当于将两个字符串连接在了一起

~~~sql
select `cloumn1` + `cloumn2` as `sums`
	from `tablename`;
	
select concat(trim(`column1`), '+' trim(`column2`)) as `concat_column`
	from `tablename`;
~~~

#### 11. 常用函数

* 需要注意的是不同的数据库的函数有可能是不相同的，但是一些较常用的函数还是差不多的

* **汇总**相关函数，其中常用就是**count()**用于统计该列的行数，和**sun()**列值之和

* |      函 数      |      说 明       |
  | :-------------: | :--------------: |
  |   AVG() / avg   | 返回某列的平均值 |
  | COUNT() / count |  返回某列的行数  |
  |   MAX() / max   | 返回某列的最大值 |
  |   MIN() / min   | 返回某列的最小值 |
  |   SUM() / sum   |  返回某列值之和  |

* **文本处理**

* |      函数       |      说明      |
  | :-------------: | :------------: |
  | LEFT(str, len)  |   左边的字符   |
  | RIGHT(str, len) |   右边的字符   |
  |     LOWER()     | 转换为小写字符 |
  |     UPPER()     | 转换为大写字符 |
  |     LTRIM()     | 去除左边的空格 |
  |     RTRIM()     | 去除右边的空格 |
  |    LENGTH()     |      长度      |
  |    SOUNDEX()    |  转换为语音值  |

* **日期与时间处理**，常用的为**current_timestamp**当前时间戳，常在存储时默认为该值

* |     函 数     |             说 明              |
  | :-----------: | :----------------------------: |
  |   ADDDATE()   |    增加一个日期（天、周等）    |
  |   ADDTIME()   |    增加一个时间（时、分等）    |
  |   CURDATE()   |          返回当前日期          |
  |   CURTIME()   |          返回当前时间          |
  |    DATE()     |     返回日期时间的日期部分     |
  |  DATEDIFF()   |        计算两个日期之差        |
  |  DATE_ADD()   |     高度灵活的日期运算函数     |
  | DATE_FORMAT() |  返回一个格式化的日期或时间串  |
  |     DAY()     |     返回一个日期的天数部分     |
  |  DAYOFWEEK()  | 对于一个日期，返回对应的星期几 |
  |    HOUR()     |     返回一个时间的小时部分     |
  |   MINUTE()    |     返回一个时间的分钟部分     |
  |    MONTH()    |     返回一个日期的月份部分     |
  |     NOW()     |       返回当前日期和时间       |
  |   SECOND()    |      返回一个时间的秒部分      |
  |    TIME()     |   返回一个日期时间的时间部分   |
  |    YEAR()     |     返回一个日期的年份部分     |

* **数学**：了解

* |  函数  |  说明  |
  | :----: | :----: |
  | SIN()  |  正弦  |
  | COS()  |  余弦  |
  | TAN()  |  正切  |
  | ABS()  | 绝对值 |
  | SQRT() | 平方根 |
  | MOD()  |  余数  |
  | EXP()  |  指数  |
  |  PI()  | 圆周率 |
  | RAND() | 随机数 |

#### 12. 分组（group by

* 在where之后使用**group by**子句对查询出来的数据进行分组，并按照其字段进行默认分组，
* **order by**在其之后使用，排序使用，
* 最后可以使用**having**对分组进行过滤，即在分组之后再进行一次过滤操作，但一般使用较少

~~~sql
SELECT col, COUNT(*) AS num
FROM mytable
WHERE col > 2
GROUP BY col
HAVING num >= 2
~~~

#### 13. 子查询

* 子查询相当于将一个select语句所查询到的列作为主句的列，
* 子查询只能允许其有一个返回字段

~~~sql
SELECT cust_name, (SELECT COUNT(*)
                   FROM Orders
                   WHERE Orders.cust_id = Customers.cust_id)
                   AS orders_num
FROM Customers
ORDER BY cust_name;
~~~

#### 14. 连接（join on

* **内连接**：**inner join**，等值连接，其等价于使用where调价查询

~~~mysql
select A.value, B.value
	from `tableA` as A inner join `tableB` as B
	on A.key = B.key;
	
# 等价于
SELECT A.value, B.value
FROM tablea AS A, tableb AS B
WHERE A.key = B.key;
~~~

* **自连接**：相当于内连接，只是连接的表都是自己本身

~~~sql
# 通过内连接自己，查询出与jiangyao有相同value的所有name
select t1.name
	from `tablename` as t1 inner join `tablename` as t2
	on t1.value = t2.value 
	and t2.name = 'jiangyao';
~~~

* **自然连接**：直接将两个表中的相名列通过等值连接在一起，不需要显式的写出，也是连表的应用

~~~sql
 #不能够使用on来进行限制
select t1.name, t2.name
	from `tablename` as t1 natural join `tablenam` t2;
~~~

* **外连接**：保留了没有别连接进入的行，分为左外连接和右外连接和全外连接**left outer join**

~~~sql
# 较常用的就是左外连接，因为两者几乎相同
select t1.id, t2.name
from `tablename1` as t1 left outer join `tablename2` as t2
on t1.id = t2.id;

# 查询结果为左边表的全部数据，右表中不满足要求的都不会展示。
~~~

#### 15. 组合查询

* 使用**union**来进行组合查询，组合两个查询的结果，若第一个查询的结果为m行，第二个为n行，则组合之后为通过m+n行的表，但是组合的两张表中的必须包含相同列，表达式和聚集函数。
* 查询结果会默认去除两表中的相同行，若需要保留则使用**union all**进行连接
* 查询出的量表结果集中的数据列名和数据类型必须一致

~~~sql
select col
from `tablename1`
where col =1
union
select col
from `tablenmae2`
where col = 2;
~~~

#### 16. 视图

~~~sql
create view `myview`
select *
from `tablename`
where xxx;  -- 直接生成视图，能够查询数据，但是不能够修改数据
~~~



