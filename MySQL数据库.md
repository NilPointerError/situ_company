# MySQL数据库

## 数据库概念

- 在计算机中, 通过一定的结构,来组织,存储和管理数据的软件系统
- 数据库管理系统（Database Management System，简称DBMS）是为管理数据库而设计的电脑软件系统，一般具有存储、截取、安全保障、备份等基础功能

## 数据库分类

##### 关系型数据库

- 关系表

  - 数据表

    - 字段(数据类型)

      - 数值型

        ![image-20220722224131087](C:\Users\Admin\Desktop\Java Notes\pic\image-20220722224131087.png)

        - 无符号
          - unsigned的作用就是不能插入负数
          - 如果插入负数会报错
        - 填充零
          - zerofill的作用就是根据你的默认形式设置来补零

      - 时间

        ![image-20220722224307438](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20220722224307438.png)

      - 字符型/二进制
  
        ![image-20220722224321912](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20220722224321912.png)
  
        - char 和 varchar 后面的长度表示的是字符的个数，而不是字节数
        
        - text列不允许拥有默认值
        
        - varchar和char 的区别
          - 区别一，定长和变长
          - 存储的容量不同
          
        - MySQL中varchar最大长度是多少？
          - 这不是一个固定的数字。先简要说明一下限制规则。
          
            a) 存储限制
            varchar 字段是将实际内容单独存储在聚簇索引之外，内容开头用1到2个字节表示实际长度（长度超过255时需要2个字节），因此最大长度不能超过65535。
            b) 编码长度限制
            字符类型若为gbk，每个字符最多占2个字节，最大长度不能超过32766;
            字符类型若为utf8，每个字符最多占3个字节，最大长度不能超过21845。
            若定义的时候超过上述限制，则varchar字段会被强行转为text类型，并产生warning。
            c) 行长度限制
            导致实际应用中varchar长度限制的是一个行定义的长度。 MySQL要求一个行的定义长度不能超过65535。若定义的表长度超过这个值，则提示
            ERROR 1118 (42000): Row size too large. The maximum row size for the used table type, not counting BLOBs, is 65535. You have to change some columns to TEXT or BLOBs。
          
        - 在使用UTF8字符集的时候,存储不同文本的字节数
        
        - 二进制类型
  
    - 约束
  
      - 主键约束 primary key
      - 外键约束 foreign key
      - 唯一约束 unique
      - 非空约束 not null
      - 默认值 default
  
  - 数据库设计
  
    - 三大范式
      - 1NF:字段不可分隔(原子性)
      - 2NF:主键依赖(唯一性)(消除非主键部分依赖联合主键中的部分字段)
      - 3NF: 独立性，消除传递依赖(非主键值不依赖于另一个非主键值，都应该依赖于主键)

Oracle，MySQL，DB2，SQLite，Microsoft SQL Server, PostgreSQL等

##### 非关系型数据库

- key-value数据库

  ​	Redis, Amazon DynamoDB, Memcached

 - 文档数据库

 - 搜索引擎

 - 分布式数据库

## 数据库操作(SQL)

##### DDL(数据定义语言)

- 数据库

  - 创建

    ```sql
    create database t_test;
    ```

  - 删除

    ```sql
    drop database t_test;
    ```

  - 查看

    ```sql
    show databases;
    show create database t_test;
    ```

  - xxxxxxxxxx     public static final <R extends List> R test2(R r) {        //在方法中指定泛型  规范类型的范围        return null;    }java

    ```sql
    use t_test;
    ```

- 数据表

  - 创建

    ```sql
    create table `t_staff`(
    	`id` int(11) primary key,
        `name` varchar(20) not null
    )
    ```

  - 查询建表

    - ```sql
      create table t_test as select * from student where ssex='男'
      ```

  - 删除

    ```sql
    drop table `t_staff`;
    ```
  
  - 查看

    - 查看表结构

      ```sql
      desc `t_staff`;
      ```
  
  - 修改

    - 修改表名

      ```sql
      alter table `t_staff` rename to|as `t_test`
      ```

    - 设置表编码

      ```sql
      alter table `t_staff` character set utf8
      ```

    - 添加列

      ```sql
      alter table t_staff add salay varchar(20)
      ```

    - 修改

      ```sql
      alter table t_staff modify name varchar(30) not null
      ```
  
    - 更改列的位置
  
      ```sql
      alter table t_staff modify col1 varchar(20) after col2
      -- 重新放置某列之后
      alter table t_staff modify col1 varchar(20) first 
      -- 将某列放在表结构的第一列
      ```

    - 修改列名

      ```sql
      alter table t_staff change col1 col2 varchar(200)
      ```
  
    - 删除列
  
      ```sql
      alter table t_staff drop column col;
      ```

##### DML(数据操纵语言)

- 添加记录

  ```sql
  insert into t_staff(id, name, sex) values (2, '李四', 0)
  insert into t_staff values (4, '赵六', 1, 2000)
  
  -- insert多条语句
  insert into t_staff values(4, 'Jake',1,3000),(5, 'Mike', 1, 2000);
  ```

- 修改记录

  ```sql
  update t_staff set salay=salay+2000 where sex=0
  ```

- 删除记录

  ```sql
  delete from t_staff where salay<4000
  ```

- 清空表

  ```sql
  truncate t_test;
  ```

##### DQL(数据查询语言)

- 简单查询

  ```sql
  select * from t_staff where sex=0;
  -- 对结果去重
  select distinct name from t_staff;
  -- 合并结果集
  #union 默认去重
  select * from t_staff where salay=2000 
  union 
  select * from t_staff where salay=7000
  #union all 不去重
  select * from t_staff UNION all select * from t_staff
  #比较运算
  select * from t_staff where salay != | <> 2000
  -- 查询null
  select * from t_staff where salay = null; -- 错误
  select * from t_staff where salay is null
  select * from t_staff where salay is not null
  select * from t_staff where salay <=> null
  
  #逻辑运算
  select * from t_staff where sex=0 and salay < 5000
  select * from t_staff where salay=3000 or salay=5000 or salay = 7000
  select * from t_staff where salay in (3000, 5000, 7000)
  #模糊查询like
  # _ 有且仅有一个字符
  # % 任意个任意字符
  select * from t_staff where name like '李_' -- 李某t
  select * from t_staff where name like '%A%' -- 含有a的字符
  # any和all
  -- 查询比所有姓李的薪资都高的人员
  select * from t_staff where salay > All(select distinct salay from t_staff where name like '%李%')
  -- 查询比姓李的薪资高的人员
  select * from t_staff where salay > ANY(select distinct salay from t_staff where name like '%李%')
  ```

  - 排序order by

    - 正序asc

      ```sql
      select * from t_staff ORDER BY salay asc;
      select * from t_staff order by salay;
      ```

    - 倒序desc

      ```sql
      -- 使用多列排序，首先按照第一指定列排序(主列)，主列值一样时，再按照副列排序
      select * from t_staff ORDER BY salay,name desc;
      ```

  - 分组查询group by

    - 聚合函数

      ```sql
      select count(*) from t_staff #查询数据的行数
      select Max(salay) from t_staff
      select MIN(salay) from t_staff
      select SUM(salay) from t_staff
      select avg(salay) from t_staff
      ```

    - having与group by配合使用

      ```sql
      -- 分组查询 根据某一列的值分组，每一组得到一个结果
      -- 对分组之后的结果进行筛选使用having
      -- as 起别名
      select sum(salay) as salaysum, avg(salay) as salayavg,department from t_staff as a GROUP BY department
      having SUM(salay) > 10000
      ```

  - 部分查询limit分页应用

    ```sql
    -- limit A,B  A起始位置 0开始计数 B代表条数
    select * from t_staff limit 4  #检索前4条数据，显示1-4条数据
    select * from student limit 2,5  #检索5条数据，显示3-7条数据
    ```

  ![image-20220727214200446](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20220727214200446.png)

  ![image-20220727214750524](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20220727214750524.png)

- 多表查询

  <img src="C:\Users\Admin\Desktop\Java Notes\pic\7.jpg" style="zoom: 80%;" />

  - 交叉连接

    ```sql
    select * from table1, table2 where table1.id == table2.id
    ```

  - 内连接 INNER JOIN

    ```sql
    select a.*, b.* from student as a [inner] join score as b on a.sid = b.sid 
    ```

  - 外连接

    - 左连接

      ```sql
      select a.*, b.* from student as a left join score as b on a.sid = b.sid
      #查询a表中不含a、b表公共部分的内容
      select a.*,b.* from student as a LEFT JOIN score as b on a.sid = b.sid where b.sid is null 
      ```

    - 右连接

      ```sql
      select a.*, b.* from student right join score as b on a.sid == b.sid
      ```

- 行列转换

  - 使用case when

    例1：

    ```sql
    select sid, score, 
    case when cid = '01' then '语文' 
    when cid = '02' then '数学' 
    when cid = '03' then '英语' 
    end from score
    ```

    ![image-20220721191532712](C:\Users\Admin\Desktop\Java Notes\pic\image-20220721191532712.png)

    ![image-20220721191604869](C:\Users\Admin\Desktop\Java Notes\pic\image-20220721191604869.png)

    例2：

    ```sql
    select * from course
    select cid, cname,
    case tid when '01' then '张三'
    when '02' then '李四'
    when '03' then '王五'
    end from course
    ```

    ![image-20220721191934371](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20220721191934371.png)

    ![image-20220721191951845](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20220721191951845.png)

    例3：

    ```sql
    select sname, cname, score, 
    case when score >= 85 then '优秀' 
    when score >= 60 then '及格'
    else '不及格'
    end from student as a left join score as b on a.sid = b.sid
    left join course as c on b.cid = c.cid
    ```

    ![image-20220721192232963](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20220721192232963.png)

    例4：

    ```sql
    select name, 
    sum(case when subject = '语文' then fraction end) as '语文',
    max(case when subject = '数学' then fraction end) as '数学',
    max(case when subject = '英语' then fraction end) as '英语',
    avg(fraction) as '平均成绩',
    sum(fraction) as '总成绩'
    from t_score group by name
    ```

    ![image-20220721192708855](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20220721192708855.png)

  - if(字段名1=字段值,  ,  )

    ```sql
    select sname,
    sum(if(cname = '语文', score, 0)) as '语文',
    sum(if(cname = '数学', score, 0)) as '数学',
    sum(if(cname = '英语', score, 0)) as '英语'
    from student as a left join score as b on a.sid = b.sid
    left join course as c on b.cid = c.cid group by sname
    ```

    ![image-20220721193217813](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20220721193217813.png)

##### DQL执行顺序

from: 首先对from子句中前两个表执行笛卡尔乘积，生产虚拟表1

on: 使用on筛选器，在虚拟表1中根据逻辑表达式筛选出满足条件的行，生产虚拟表2

join: 如果是join，则添加匹配的外部行；如果是left join九八左边第二步中未匹配的行添加进来，如果是right join就把右表在第二步中未匹配的行添加进来，生成虚拟表3

where : 对虚拟表3执行where条件进行过滤，符合条件的生成虚拟表4

group by: 对虚拟表4进行分组操作生成虚拟表5

having: 在虚拟表5上继续执行having后的条件筛选，将满足条件的生产虚拟表6

select: 在虚拟表6上进行查询操作，生成虚拟表7 

order by: 将虚拟表7按照order by后的条件进行升序或降序排序: desc, asc(默认asc降序)生成虚拟表8

limit: 在虚拟表8上取出指定的行的记录，生成虚拟表9并返回

##### DCL(数据管理语言)

- 用户管理

  - 添加

    ```sql
    #创建easy用户
    create user 'easy'@'%' indentified by '123456'
    ```

- 授权管理

## 数据库工具

#### 视图

- 概念

- 操作

  - 创建视图

    ```sql
    CREATE view v_student_score as 
    
    select a.sid,a.sname, c.cname, b.score
    FROM student a LEFT JOIN score b on a.sid=b.Sid
    LEFT JOIN course c on c.Cid=b.Cid
    ```

  - 查看视图

  - 

- **优点**

  - 定制用户数据，聚焦特定的数据
    - 不同的用户可能对不同的数据有不同的要求

  - 简化数据操作
    - 在使用查询时，很多时候要使用聚合函数，同时还要显示其他字段的信息，可能还需要关联到其他表，语句可能会很长，如果这个动作频繁发生的话，可以创建视图来简化操作

  - 提高数据的安全性
    - 视图是虚拟的，物理上是不存在的。可以只授予用户视图的权限，而不具体指定使用表的权限，来保护基础数据的安全。

  - 共享所需数据
    - 通过使用视图，每个用户不必都定义和存储自己所需的数据，可以共享数据库中的数据，同样的数据只需要存储一次。

  - 更改数据格式
    - 通过使用视图，可以重新格式化检索出的数据

#### 触发器

- 概念

- 触发时机
  - 增删改
  - 操作前Before,操作后After
  - new, old

- 创建触发器

  ```sql
  create trigger a_u_log after update on t_test
  for each row begin
  	insert into t_log values(concat(new.sid,new.sname,new.sage,new.ssex));
  	insert into t_log values(concat(old.sid,old.sname,old.sage,old.ssex));
  end;
  update t_test set sname='钱老大' where sid='02'
  ```

  - 注意:在命令行中要使用delimiter来重新定义结束符一般临时使用$$

    ![image-20220721202542981](C:\Users\Admin\Desktop\Java Notes\pic\image-20220721202542981.png)

    ![image-20220721202620544](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20220721202620544.png)

- 删除触发器

  ```sql
  drop trigger a_u_log;
  ```

- **优点**

  - 触发器的执行是自动的，当对触发器相关表的数据做出相应的修改后立即执行。
  - 触发器可以实施比foreign key约束、check约束更为复杂的检查和操作。
  - 触发器可以实现表数据的级联更改，在一定程度上保证了数据的完整性。

- MySQL变量的定义和赋值

  - 用户变量  声明  赋值

    set @num=12;

    select @num

  - 局部变量

    ```sql
    create trigger b_d_log before delete on t_score
    for each row BEGIN
    DECLARE num int default 0;
    select sum(fraction) into num from t_score;
    insert into t_log VALUES (CONCAT('删除之前总成绩为',num,'分'));
    end;
    ```

  - 会话变量

    ```sql
    show session variables -- 显示会话变量
    select @@auto_increment_increment;  -- 查询会话变量
    ```

  - 全局变量

    ```sql
    show global VARIABLES; -- 显示全局变量
    ```

#### 函数

- 开启创建函数的权限

  ```sql
  select  @@global.log_bin_trust_function_creators;
  set @@global.log_bin_trust_function_creators=1;
  ```

- 定义函数

  例1：

  ```sql
  create FUNCTION doublenum(num int) RETURNS int 
  BEGIN
  	DECLARE double_num int;
  		set double_num=2*num;
  	return double_num;
  end;
  select *, doublenum(20) from t_score
  ```

  ![image-20220721204853648](C:\Users\Admin\Desktop\Java Notes\pic\image-20220721204853648.png)

  例2：

  ```sql
  create function checksex(sex int)returns VARCHAR(5)
  BEGIN
  	declare result varchar(5);
  	select case when sex=1 then '男'
  	when sex=0 then '女'
  	else 'N'
  	end into result;
  	return result;
  END
  select checksex(2)
  ```

  ![image-20220721204932922](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20220721204932922.png)

  例3：删除函数

  ```sql
  drop function checksex;
  ```

- mysql 数值常用函数

  ![image-20220721205236639](C:\Users\Admin\Desktop\Java Notes\pic\image-20220721205236639.png)

- mysql 字符串常用函数

  ```sql
  select LENGTH('你好')
  
  select left('123456789', 7)
  select right('123456789', 7)
  
  
  SELECT insert('123456789', 2, 5, '你好')
  
  select replace('123456789','123','你好')
  
  select replace('123456789123','123','你好')
  
  select substr('12345678',4,6)
  
  select NOW();
  ```

  ![image-20220721205250818](C:\Users\Admin\Desktop\Java Notes\pic\image-20220721205250818.png)

- mysql 日期类型函数

  ![image-20220721205307028](C:\Users\Admin\Desktop\Java Notes\pic\image-20220721205307028.png)

- **优点**

#### MySQL流程控制语句

- IF 语句

  ```sql
  create procedure p_addlog(int num int,inout num2 int)
  begin
  	if num%2=0 then set num2=num*2;
  	elseif num%2!=0 then set num2=num*3;
  	end if;
  end;
  set @result=4;
  call p_addlog(13,@result);
  select @result#39
  ```

- CASE 语句

  ```sql
  create procedure p_addlog(in num int, inout num2 int)
  begin
  	case when num%2=0 then
  		set num2=num*2;
  	when num%2!=0 then
  		set num2=num*3;
  	end case;
  end;
  set @result=4;
  call p_addlog(13,@result);
  select @result#39
  ```

- LOOP 语句

  ```sql
  create procedure p_addlog(in count int)
  begin
  
  insertb:loop
  	insert into t_log values(count);
  	set count=count-1;
  	if count=0 then leave insertb;
  	end if;
  end loop;
  
  end;
  call p_addlog(10)
  ```

  ![image-20220722210013986](C:\Users\Admin\Desktop\Java Notes\pic\image-20220722210013986.png)

  

- LEAVE 语句(相当于break)

- ITERATE 语句(相当于continue)

  ```sql
  create procedure p_addlog(in count int)
  begin
  	
  add_num:loop
  	set count=count+1;
  	if count=100 then leave add_num;
  	elseif count%2=0 then iterate add_num;
  	end if;
  	insert into t_log values(concat('iterate',count));	
  end loop;
  end;
  
  call p_addlog(90)
  ```

  ![image-20220722210043559](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20220722210043559.png)

- REPEAT 语句

- REPEAT 语句是有条件控制的循环语句，每次语句执行完毕后，会对条件表达式进行判断，如果表达式返回值为 TRUE，则循环结束，否则重复执行循环中的语句。

  ```sql
  create PROCEDURE p_addlog(in count int)
  BEGIN
  
  repeat
  insert into t_log values (count);
  	set count=count-1;
  until count=0
  end repeat;
  
  END;
  call p_addlog(8)#8 7 6 5 4 3 2 1
  ```

- WHILE 语句

  ```sql
  create PROCEDURE p_addlog(in count int)
  begin
  while count!=0 do
  
  	insert into t_log values (count);
  	set count=count-1;
  
  end while;
  end;
  call p_addlog(5)# 5 4 3 2 1
  ```

#### 存储过程

- 概念

- 参数有in, out, inout三种

- 可以返回参数和结果集

- 语法

  - 定义存储过程

    ```sql
    create procedure p_addlog(in str varchar(20))begin
    	declare item varchar(20);
    	set item=str;
    	insert into t_log values(item);
    end;
    ```

  - 调用存储过程

    ```sql
    call p_addlog('张三')
    ```

  - 删除存储过程

    ```sql
    drop procedure p_addlog;
    ```

  - 传入数字，记录到t_log中，返回双倍

    ```sql
    create procedure p_addlog(in num int,out result int)
    begin
    	declare n int;
    	set n=num;
    	set result=2*n;
    	insert into t_log values(n);
    end;
    call p_addlog(12,@result);
    select @result; #24
    ```

- **优点**

  - 封装性
  - 可增强SQL语句的功能和灵活性
  - 可减少网络流量
  - 高性能
  - 提高数据库的安全性和数据的完整性
  - 使数据独立
  
- 索引

  - 概念

    - B+树实现

  - 索引类型

    - 主键索引

      - 是一种特殊的唯一索引，一个表只能有一个主键，不允许有空值。一般是在建表的时候同时创建主键索引

    - 普通索引

      - 是最基本的索引，它没有任何限制

      ```sql
      create index student_name_index on student(sname(10));
      explain select * from student where sname='赵雷'#解析sql语句 key 查询语句中使用了哪个索引
      ```

      - 删除索引

      ```sql
      drop index student_name_index on student
      ```

    - 联合索引

      - 指多个字段上创建的索引，只有在查询条件中使用了创建索引时的第一个字段，索引才会被使用。使用组合索引时遵循最左前缀集合

        ```sql
        create index stu_sex_age_index on student (ssex,sage)
        explain select * from student 
        where ssex='男' and sage = '1990-08-09'
        ```

    - 唯一索引

      - 索引列的值必须唯一，但允许有空值。如果是组合索引，则列值的组合必须唯一

        ```sql
        create unique index student_name_unique_index on student(sname) 
        ```

    - 全文索引

  - 索引的缺点

    - 虽然索引大大提高了查询速度，同时却会降低更新表的速度，如对表进行insert、update和delete。因为更新表时，不仅要保存数据，还要保存一下索引文件。
    - 建立索引会占用磁盘空间的索引文件。一般情况这个问题不太严重，但如果你在一个大表上创建了多种组合索引，索引文件的会增长很快。
    - 索引只是提高效率的一个因素，如果有大数据量的表，就需要花时间研究建立最优秀的索引，或优化查询语句。

  - 索引注意事项

  - 索引失效

    - 索引列数据类型发生转换，索引失效

      ```sql
      explain select * from student where sid='01'
      
      explain select * from student where sid = 1
      ```

      ![image-20220722221404980](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20220722221404980.png)

      ![image-20220722221451262](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20220722221451262.png)

    - 索引列进行运算时，索引失效

      ```sql
      create INDEX score_index on score(score)
      explain select * from score where score+12=12
      ```

    - 联合索引，没有引用最左侧索引列，索引会失效；单独引用最左侧索引列会生效

      ```sql
      create index stu_sex_age_index on student (ssex,sage)
      explain select * from student where sage = '1990-08-09'
      ```

    - 有or时全表检索，索引失效

      ```sql
      explain select * from score where score=12-12 or sid = '01'
      ```

    - where中使用了 !=   

      ```sql
      explain select * from score where score != 12.0
      ```

    - like以%开头

      ```sql
      explain select * from student where sname like '%赵雷'
      ```

    - 如果mysql觉得全表扫描更快时（数据少）

    - where中索引列使用了函数

  - 没必要使用索引的情况

  - 索引的原理

    - 聚集索引
    - 非聚集索引

#### 数据库优化

软件方面：

建表的时候选用合适的数据类型，适当添加冗余列

选用合适的存储引擎

优化SQL语句，尽量使用索引

查询时不要使用*，定义要检索的数据列

创建优秀的索引

常用的事务可以使用存储过程



硬件方面：

分库分表

集群

增加内存





浦景路 华侨城
