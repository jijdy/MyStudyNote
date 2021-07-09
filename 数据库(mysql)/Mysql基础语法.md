### sql的常用语法和意义

#### 1. 创建表

~~~sql
CREATE TABLE `tablename`(
    # id为bigint类型，不为空，自增，一般作为主键
	`id` bigint not null auto_increment,
    
    `column1` int not null default 1,
    #varchar相当于String，其自设置字符串的长度，null表示可以为空，一般默认不写
    `column2` varchar(20) null,
    `column3` date null, -- date是sql中的一个日期类，表示时间
    primary key(`id`)
) engine=innobd, default charset=utf8mb4, comment="table表的注释";
~~~

* 其中，表的创建一般可以指定引擎和编码集，以提高之后sql代码的复用性，多使用comment加注释是个好习惯

#### 2. 修改表中的列

~~~sql
# 向一个表中添加列，指明该列的属性
ALTER TABLE `tablename`
	ADD `newcolnme` char(10) comment="新添加的列";
	
# 删除一个表中的列
ALTER TABLE `teblename`
	DROP COLUMN `columnname`;
	
# 删除表
DROP TABLE `tablename`;
~~~

#### 3. 向表中插入数据

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

#### 4. 更新表中数据

~~~sql
# 更新表中的列数据，一般指定条件
UPDATE `tablename`
	SET `column1` = 'val'
	WHERE `id`=1;
~~~

#### 5. 删除表中行数据

~~~sql
# 删除限定条件所在的一行
DELETE FROM `tablename`
	WHERE `id`=1;

# 删除表中的所有数据，清空表
TRUNCATE TABLE `tablename`;
~~~

#### 6. 查询数据

