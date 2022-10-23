# Mysql基础

## 什么是关系型数据库（Relational databases)？

数据库由若干张`表`(Table)组成，这里说的数据Table很像Excel里的表; 正如Excel里的表格，Table也是由 `行(rows)`和`列(columns)`组成

一个Table存储一个类别的数据，每一行是一条数据，每一列是这种数据的一个属性； Table就像一个二维的表格，`列(columns)`是有限固定的，`行(rows)`是无限不固定的

## 查询单列

```mysql
select device_id from user_profile
上述语句利用 SELECT 语句从 user_profile表中查询名为 device_id 的列。查询所需的列名写在 SELECT 关键字之后，FROM 关键字指出从哪个表中查询数据。
```

## 查询多个列

```mysql
select device_id,gender from user_profile
要想从一个表中查询多个列，仍然使用相同的 SELECT 语句。唯一的不同是必须在SELECT 关键字后给出多个列名，列名之间必须以逗号分隔
```

## 查询所有列

```mysql
select * from user_profile
SELECT 语句还可以查询所有的列而不必逐个列出它们。在实际列名的位置使用星号（*）通配符可以做到这点
```

## 用distinct，group by关键字可以去掉结果中的重复行

```mysql
使用distinct：
select distinct  university from user_profile;
使用group by：
select university from user_profile group by university;
```

## 查询结果限制返回行数(限制两行）
```mysql
select device_id from user_profile where id in(1,2)  
select device_id from user_profile where id <= 2 
Limit语句一般加在SQL语句末尾，并且与数字搭配，写作Limit N，N代表想要限制返回的行数。
select device_id from user_profile  limit 2 
select device_id from user_profile limit 0,2 
```

## 将查询后的列重新命名
```mysql
SQL也提供了列重命名的语法--'AS' 。
在下面的例子中可以看到Select查询列后使用了 as 函数，将device_id赋予了一个新的列名 user_infos_example
select device_id as user_infos_example from user_profile 
```

## 查找某个年龄段的用户信息
```mysql
使用WHERE语句对数据进行过滤，WHERE语句通常跟在From语句
select device_id,gender,age from user_profile where age between 20 and 23
```

## 查找除复旦大学的用户信息
```mysql
Where语句需要和操作符搭配使用，在前一章节中搭配的通配符是“=”，即等于号，选择合适的操作符可以大大提高查询的效率，下面对常见的操作符进行一些介绍。
不等于号
不等于号在SQL中的写法为 < > 或 ！=，代表筛选出不满足某条件的数据。
大于号小于号
大于号SQL中的写法为 > ，小于号为<，代表筛选出大于或小于某个条件的数据。
范围值
范围值限制方法为between n1 and n2，n1和n2为要限制的区间范围，使用中需要注意两点：
一是 and 之前的值 需要小于and 之后的值，不然查询会返回空结果。
二是在hive sql中结果会包括两端值，即如果语句写为 betwen 10 and 20, 那么结果中会包括取值等于10或20的数据。
空值
在表存储的数据类型中，有一类特殊的值叫空值，其定义为当一个字段不包括任何值时，称其包含空值 NULL，空值与字段包含 0、空字符串或仅仅包含空格是不同的概 念。
在对空值进行筛选时，不能用等于号简单的判断是否 ‘= NULL’。SQL 语句有一个特殊的 WHERE 子句，可用来检查具有 NULL 值的列。这个 WHERE 子句就 是 IS NULL 子句。 
select device_id,gender,age,university from user_profile where university <>"复旦大学"
select device_id,gender,age,university from user_profile where university !="复旦大学"
select device_id,gender,age,university from user_profile where university not in("复旦大学")
select device_id,gender,age from user_profile Where gender is Not Null
```

## 查找后排序(升序和降序)
```mysql
在默认情况下，ORDER BY会对数据进行升序排序（从 A 到Z），我们还可以使用 ORDER BY 子句进行降序（从 Z 到 A）排 序。为了进行降序排序， 必须指定DESC 关键字
select device_id,age from user_profile order by age desc =>降序
同样的查询语句在order by最后加上desc后可以看到结果按照age进行了降序排序 。
select device_id,age from user_profile order by age asc =>升序
```

## 单列排序

```mysql
 为了明确地排序用 SELECT语句检索出的数据，可使用 ORDER BY 子句。 ORDER BY 子句取一个或多个列的名字，据此对输出进行排序。
 select age from user_profile order by age
```

## 多列排序
```mysql
有时候我们经常需要按不止一个列进行数据排序， 要按多个列排序，简单指定列名，列名之间用逗号分开即可，在排序时会按照列给出的先后顺序依次排序。
select device_id,gpa,age from user_profile order by gpa asc,age desc
```

## 用where过滤空值
```mysql
select device_id,gender,age,university from user_profile where age is not null
```

## 高级操作符练习 In()
```mysql
IN 操作符用来指定条件范围，范围中的每个条件都可以进行匹配。IN 取 一组由逗号分隔、括在圆括号中的合法值。
select device_id,gender,age,university,gpa from user_profile where gpa > 3.5 and gender in ('male')
```

## 高级操作符练习 Not in()

```mysql
WHERE 子句中的 NOT 操作符有且只有一个功能，那就是否定其后所跟的任何条件
select device_id,gender,age,university，gpa from user_profilewhere university not in ('北京大学','复旦大学')
```

## 高级操作符练习 OR

```mysql
OR 操作符逻辑与 AND 操作符正好相反，在过滤数据时如果我们想要结果只需满足多个条件中的一个，可以使用OR操作符对条件进行连接
select device_id,gender,age,university,gpa from user_profile where university in ('北京大学') or gpa > 3.7
```

## 高级操作符练习 And

```mysql
在过滤数据时如果我们想结果同时满足多个条件，可以使用 AND 操作符给 WHERE子句附加条件
select device_id,age From user_profile Where age <30 and gender = 'male'
```

## 操作符混合运用 and 和 or
```mysql
WHERE 子句其实可以包含任意数目的 AND 和 OR 操作符，允许两者结合以进行复杂、高级的过滤。在使用时需要注意的一点是两者优先级的问题， SQL在处理 OR操作符前，优先处理 AND 操作符。
select device_id,gender,age,university,gpa from user_profile where 
(gpa > 3.5 and university='山东大学') or (gpa >3.8 and university='复旦大学')
代码中Where函数限定条件正确的理解逻辑为：筛选出学校为北京大学或山东大学中gpa大于3.5的学生，SQL是先对and逻辑进行了理解，再处理or的逻辑。
```

## Like操作符-模糊匹配
```mysql
Like操作符需要和通配符结合使用，一般最常用的通配符是 %， 在搜索串中，%表示任何字符出现任意次数。例如，为了找出所有学校中以北京开头的用户
select device_id,age,university from user_profile where university like "%北京%"
```

## 计算函数-Avg()

```mysql
AVG() 可用来返回所有列的平均值，也可以用来返回特定列或行的平均值
select avg(gpa) from user_profile
```

## 计算函数-Count()

```mysql
COUNT()函数为计数函数，可利用 COUNT()确定表中行的数目或符合特定 条件的行的数目。
COUNT()函数有两种使用方式：
使用 COUNT(*)对表中行的数目进行计数，不管表列中包含的是空值（NULL）还是非空值。 使用 COUNT(column)对特定列中具有值的行进行计数，忽略 NULL 值。
select count(*) as num_cnt from user_profile; =>4
select count(age) as num_cnt from user_profile; =>3
```

![1666546020164](D:\learning\mysql\1666546020164.png)

## 计算函数-Max()

```mysql
MAX()返回指定列中的最大值。MAX在使用时，（）需指定要返回最大值的列名
select max(gpa) as gpa from user_profile where university in('复旦大学')
```

## 计算函数-Min()

```mysql
MIN()的功能正好与 MAX()功能相反，它返回指定列的最小值。与 MAX() 一样，MIN()要求指定列名
select min(gpa) as gpa from user_profile where university in('复旦大学')
```

## 计算函数-Sum()

```mysql
SUM()用来返回指定列值的和（总计）。如下所示函数返回了age列所有用户年纪的总和
select sum(age) as sum_age from user_profile;
```

## 取整函数-round()

```mysql
在一些聚集运算中，容易出现结果为非整数的情况，这时候如果想要限定结果返回的小数位数就可以使用SQL中内置的round函数，语法格式为round（value，n），其中 value代表想要限制小数位数的字段，n代表想要限制的小数位数。
select round(avg(age),1) as avg_age from user_profile;
```

## 分组

```mysql
select gender,university,count(id) user_num,avg(active_days_within_30) avg_avtive_day,avg(question_cnt) avg_auestion_cnt from user_profile group by gender,university
```

## 分组过滤(having 关键字) where 从记录中法过滤出某一条记录  having 可以从一组组记录中过滤掉其哪几组
select university,round(avg(question_cnt),3) as avg_question_cnt,round(avg(answer_cnt),3) as avg_answer_cnt
from user_profile group by university having avg_question_cnt < 5 or avg_answer_cnt < 20

## 多表查询 => 子查询

子查询：即 嵌套在其他查询中的查询

