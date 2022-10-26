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

![1666546020164](D:\learning\mysql\material\1666546020164.png)

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

## 分组计算

```mysql
select university,avg(gpa) as avg_gpa from user_profile group by university
示范的SELECT 语句指定了两个列：university为用户的学校， avg(gpa)为计算字段，代表平均gpa。 GROUP BY 子句指示SQL按university分组分别计算每个学校的平均gpa情况，从输出中可以看到，结果返回了每个大学的平均gpa数值。
```

## 分组过滤(having 关键字) where 从记录中法过滤出某一条记录  having 可以从一组组记录中过滤掉其哪几组
```mysql
select university,round(avg(question_cnt),3) as avg_question_cnt,round(avg(answer_cnt),3) as avg_answer_cnt from user_profile group by university having avg_question_cnt < 5 or avg_answer_cnt < 20
```

## 分组排序

```mysql
如果想让分组查询的结果按照某个特殊的字段进行升序或降序排列，那么应该怎么做呢？分组查询结果也支持排序功能，所需要用到的语句是Order By
select university,avg(active_days_within_30) as avg_active_days from user_profile group by university order bY avg_active_days
```



## 多表查询 => 子查询

```mysql
子查询：即嵌套在其他查询中的查询
题库练习明细表：question_practice_detail
question_practice表中存储了每个用户在在线题库中进行题目练习的明细情况，每个
用户每次做的每道练习会在表中生成一行记录，表形式如下：
```

![1666602438359](D:\learning\mysql\material\1666602438359.png)

```mysql
查看来自山东大学的学生每天的题库练习情况， 应该怎么做呢？
select device_id,question_id,result from question_practice_detail where device_id in (select device_id from user_profile where university ='山东大学');
在 SELECT 语句中，子查询总是从内向外处理。在处理上面的 SELECT 语 句时，DBMS实际上执行了两个操作。
首先，它执行下面的查询：
select device_id from user_active WHERE university ='山东大学'
这个查询可以返回所有的山东大学用户的device_id， 然后，这些值以IN 操作符要求的逗号分隔的格式传递给外部查询的 WHERE 子句外部查询变成：
select cust_id from orders where order_num in (5432，,2131) ，
通过这样的方式，就可以得到我们想要的结果，返回所有山东大学。
```

## 连接查询

SQL 最强大的功能之一就是能在数据查询的执行中联结（join）表。联结是利用 SQL 的SELECT 能执行的最重要的操作，很好地理解联结及其语法是学习 SQL 的极为重要的部分。关系表的设计就是要把信息分解成多个表，一类数据一个表。各表通过某些共同的值互相关联（所以才叫关系数据库）。在实际开发中，大部分的情况下都不是从单表中查询数据，一般都是多张表[联合查询](https://so.csdn.net/so/search?q=联合查询&spm=1001.2101.3001.7020)取出最终的结果。一个业务都会对应多张表，比如：学生和班级，起码两张表。(避免产生数据的冗余)。

![1666622556947](D:\learning\mysql\material\1666622556947.png)

### 内连接和外连接的区别

1、内连接：
    假设A和B表进行连接，使用内连接的话，凡是A表和B表能够匹配上的记录查询出来，这就是内连接。AB两张表没有主副之分，两张表是平等的。

2、外连接：
    假设A和B表进行连接，使用外连接的话， AB两张表中有一张表是主表，一张表是副表，主要查询主表中的数据，捎带着查询副表。当副表中的数据没有和主表中的数据匹配上，副表自动模拟出NULL与之匹配。

### 连接查询的分类

1、根据语法出现的年代来划分：

 sql97--仅仅支持内连接(一些老的DBA可能还在使用这种语法。)
 sql99--推荐使用,支持左外+右外(左外+右外)+交叉

2、根据功能划分分类:

交叉连接：笛卡尔积。()
内连接：等值连接、非等值连接、自连接。（还可分为隐式【无join】和显式【有join】）
外连接：左外连接(左连接)、右外连接(右连接)、全连接。

### 七种常用连接查询详解

#### 1、笛卡尔积： 

笛卡尔积也称交叉连接，交叉连接是内连接的一种。

假设集合A={a,b}，集合B={0,1,2}，则两个集合的笛卡尔积为{(a,0),(a,1),(a,2),(b,0),(b,1), (b,2)}。如果A表示某学校学生的集合，B表示该学校所有教师的集合，则A与B的笛卡尔积表示学生选择老师所有可能的情况。

笛卡尔积特点：它不使用任何匹配或者选取条件，而是直接将一个数据源中的每个行与另一个数据源的每个行 一 一 匹配。

      重点记：
      笛卡尔积：用的比较少，因为存在重复数据
      笛卡尔积：一个表的每条数据都和另一个表的所有数据匹配一次
      结       果： 一表 9 条 乘以 另一表 6 条 = 54 条
案例：

```mysql
查询学生对应的老师
select * from student ,teacher；
学生表 中数据每 1 个学生都和 教师表 中的 所有教师 都匹配一次。
问题：
	当两张表进行连接查询的时候，没有任何条件进行限制，最终的查询结果条数是两张表记录条数的乘积。这就是笛卡尔积现象。 查询出来的结果是两张表的记录的乘积 9*6=54，许多数据是无效数据。如何避免笛卡尔积现象？
解决方案： 增加加条件进行过滤，但只会显示有效记录。此时也是隐式（无join）内连接。

```

![1666623182281](D:\learning\mysql\material\1666623182281.png)

```mysql
根据教师id查询学生对应的选课老师
select st.*,th.* from student st ,teacher th where st.teacher_id = th.id;
```

![1666623507552](D:\learning\mysql\material\1666623507552.png)

#### 2、内连接

 内连接，取的就是两张表的交集。

![1666623563409](D:\learning\mysql\material\1666623563409.png)

##### 2.1隐式与显式连接

**隐式（无join）连接语法**：select 字段 from 表A, 表B where 消除笛卡尔积的连接条件

```mysql
案例：根据教师id查询学生对应的选课老师
select st.*,th.* from student st ,teacher th where st.teacher_id = th.id;
```

![1666623748802](D:\learning\mysql\material\1666623748802.png)

**显式（有join）连接语法**：select 字段* from 表A 别名 INNER(可以省略) JOIN 表B 别名 ON 消除笛卡尔积的连接条件

```mysql
案例：根据教师id查询学生对应的选课老师
select st.*,th.* from student st inner join teacher th on st.teacher_id = th.id;
select st.*,th.* from student st join teacher th on st.teacher_id = th.id;（inner 可以省略）
```

![1666623855543](D:\learning\mysql\material\1666623855543.png)

##### 2.2等值连接

等值连接的最大的特点就是：条件是等量关系，SQL99(最常用)。

**等值连接语法**
select 字段* from 表1  **INNER(可以省略) JOIN** 表2  **ON** 消除笛卡尔积的连接条件A=B

```mysql
案例：根据教师id查询学生对应的选课老师
select st.*,th.* from student st join teacher th on st.teacher_id = th.id;
```

![1666623993494](D:\learning\mysql\material\1666623993494.png)

##### 2.3非等值连接

非等值连接的最大的特点就是：条件不是是等量关系，SQL99(最常用)。

 **非等值连接语法**
 select 字段* from 表1  **INNER(可以省略) JOIN** 表2  **ON** 消除笛卡尔积的连接条件

```mysql
案例：查询教师id在4-6之间所教的学生和老师信息
select st.*,th.* from student st join teacher th on st.teacher_id = th.id and th.id between 4 and 6;
```

![1666624086733](D:\learning\mysql\material\1666624086733.png)

#####  2.4自连接

自连接的最大的特点：就是一张表看做两张表，自己连接自己

实质就是等值连接，只不过是连接表本身。

```mysql
 案例：查询学生id和教师id相同的学生
select s.*,st.teacher_id from student s,student st where s.id = st.teacher_id;
```

#### 3、外连接

外连接又分为左外连接、右外连接、全连接

```mysql
左外连接（LEFT OUTER JOIN），简称左连接（LEFT JOIN）
右外连接（RIGHT OUTER JOIN），简称右连接（RIGHT JOIN）
全外连接（FULL OUTER JOIN），简称全连接（FULL JOIN）
```
##### 3.1左外连接：

左外连接：left join 或 left outrer join  （outer可以省略）

左外连接：左边的是主表，左表数据全部显示，右表显示符合ON后的条件的数据，不符合的用NULL代替。

![1666624586278](D:\learning\mysql\material\1666624586278.png)

```mysql
select * from student st left join teacher th on st.teacher_id = th.id;（outer可以省略）
```

![1666624666400](D:\learning\mysql\material\1666624666400.png)

![1666624742551](D:\learning\mysql\material\1666624742551.png)

```mysql
案例：查询没有教师的学生信息
select * from student st left join teacher th on st.teacher_id = th.id where th.id is null;
```

![1666624779654](D:\learning\mysql\material\1666624779654.png)

#####  3.2右外连接：

 右外连接：right join 或 right outrer join  （outer可以省略）

 右外连接：右边边的是主表，右边表数据全部显示，左边表显示符合ON后的条件的数据，不符合的用NULL代替。

![1666624824181](D:\learning\mysql\material\1666624824181.png)

```mysql
select * from student st right join teacher th on st.teacher_id = th.id;（outer可以省略）
```

![1666624876046](D:\learning\mysql\material\1666624876046.png)

![1666624891822](D:\learning\mysql\material\1666624891822.png)

```mysql
案例：查询没有学生的教师信息
select * from student st right join teacher th on st.teacher_id = th.id where st.teacher_id is null;
```

![1666624929446](D:\learning\mysql\material\1666624929446.png)

##### 3.3全外连接

全外连接：full join 或 full outer join（outer可以省略），但Mysql不支持，可以使用union组合并去重实现。 

简单理解：**全外接查询**：就是 **左表独有的数据** 加上 **右表独有的数据**

![1666625028325](D:\learning\mysql\material\1666625028325.png)

```mysql
select * from student st left join teacher th on st.teacher_id = th.id where th.id is null
union 
select * from student st right join teacher th on st.teacher_id = th.id where st.teacher_id is null
```

![1666625082448](D:\learning\mysql\material\1666625082448.png)

##### 3.4全连接

全连接：full join 或 full outer join（outer可以省略），但Mysql不支持，可以使用union组合并去重实现。

简单理解：**全连接查询**的是 左表所有的数据  加上 右表所有的数据 并去重。

![1666625153526](D:\learning\mysql\material\1666625153526.png)

```mysql
select * from student st left join teacher th on st.teacher_id = th.id union 
select * from student st right join teacher th on st.teacher_id = th.id
```

![1666625200174](D:\learning\mysql\material\1666625200174.png)

## IF 条件函数

```mysql
IF函数是最常用到的条件函数，在Excel中也有if函数，想必大部分同学对其都比较熟悉，其写法为 if(x=n,a,b)，x=n代表判断条件，如果x=n时，那么结果返回a，否则返回b
select device_id,if(university = '北京大学','北京大学','其他大学'）as university from user_profile
```

![1666799193000](D:\learning\mysql\material\1666799193000.png)

## Case when

```mysql
case when与if的作用基本相同，也是按照条件更换列中的内容，区别是case when可以 对多个条件进行转换
select case when SCORE = 'A' then '优' when SCORE = 'B' then '良'
when SCORE = 'C' then '中' else '不及格'
END --注意这里需要加end作为结束
```

## 日期函数

在DBMS中日期和时间值以特殊的格式存储，以便能快速和有效地排序或过滤。常见的日期数据格式有两种：'yyyy-MM-dd' 和 'yyyyMMdd'

### **时间戳-日期格式转化**

时间戳是数据库中自动生成的唯一二进制数字，表明数据库中数据修改发生的相对顺序，其记录形式类似：1627963699 ，在实际工作环境中，对于用户行为发生的时间通常都是用时间戳进行记录，**时间戳和日期格式之间可以利用from_unixtime和 unix_timestamp进行转换。**

![1666799428586](D:\learning\mysql\material\1666799428586.png)

```mysql
from_unixtime可以将时间戳转换成日期，其使用语法和输出如下所示：
select from_unixtime(time,'yyyy-MM-dd') as time from question_practice_detail
得出
Time
2021-08-03
2021-08-03
2021-08-03
unix_timestamp可以将日期转换回时间戳，其使用语法和输出如下所示：
select from_unixtime('2021-08-01','yyyy-MM-dd') as time
```

