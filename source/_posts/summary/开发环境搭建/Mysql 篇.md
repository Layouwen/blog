---
title: Mysql 篇
date: 2024-01-23T15:23:12.000Z
tags:
  - 汇总
  - mysql
categories:
  - 汇总
  - 开发环境
uuid: 35aa0acc-e4f6-4023-bb27-309ae3dfa6d5
---

# 一、Window

## 安装

[Mysql下载地址](https://dev.mysql.com/downloads/mysql/)

配置环境变量，例如：`D:\mysql-8.0.27-winx64\bin`

创建 `mysql.ini` 配置文件

```ini
[mysqld]
# 设置3306端口
port=3306
# 设置mysql的安装目录
basedir=D:\mysql-8.0.27-winx64
# 设置mysql数据库的数据存放目录
datadir=D:\mysql-8.0.27-winx64\data
# 允许最大连接数
max_connections=200
# 允许链接失败的次数
max_connect_errors=10
# 服务端的字符集
character-set-server=utf8
# 创建新表使用的存储引擎
default-storage-engine=INNODB
# 默认使用"mysql_native_password"插件认证
default_authentication_plugin=mysql_native_password
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8
[client]
# 设置mysql客户端连接服务器的默认端口
port=3306
default-character-set=utf8
```

打开bin文件夹，执行下面命令
```bash
> mysql --initialize --console
```

接下来基础输出的最后一行信息，包含账号和密码
```bash
2021-11-08T16:02:37.113703Z 0 [Warning] [MY-010918] [Server] 'default_authentication_plugin' is deprecated and will be removed in a future release. Please use authentication_policy instead.
2021-11-08T16:02:37.113725Z 0 [System] [MY-013169] [Server] D:\mysql-8.0.27-winx64\bin\mysqld.exe (mysqld 8.0.27) initializing of server in progress as process 7092
2021-11-08T16:02:37.114851Z 0 [Warning] [MY-013242] [Server] --character-set-server: 'utf8' is currently an alias for the character set UTF8MB3, but will be an alias for UTF8MB4 in a future release. Please consider using UTF8MB4 in order to be unambiguous.
2021-11-08T16:02:37.173348Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
2021-11-08T16:02:44.033460Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
2021-11-08T16:03:01.405311Z 0 [Warning] [MY-013746] [Server] A deprecated TLS version TLSv1 is enabled for channel mysql_main
2021-11-08T16:03:01.405983Z 0 [Warning] [MY-013746] [Server] A deprecated TLS version TLSv1.1 is enabled for channel mysql_main
2021-11-08T16:03:01.737008Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: <+woA1sid<y!
```

```bash
root@localhost: <+woA1sid<y!
```

> 如果不记得也没关系，删掉 `data` 文件夹，重新执行命令可以重新重置数据库

安装 `mysql` 服务
```bash
> mysqld --install 
```

启动服务

```bash
> net start mysql
```

```bash
The MySQL service is starting..
The MySQL service was started successfully.
```

停止服务
```bash
> net stop mysql
```

连接数据库，输入密码回车
```bash
> mysql -u root -p
```

修改密码
```bash
alter user 'root'@'localhost' identified with mysql_native_password BY '密码';
```
例如：
```bash
alter user 'root'@'localhost' identified with mysql_native_password BY '123123';
```

## 卸载

停止服务
```bash
net stop mysql
```

删除mysql的服务

```bash
sc delete mysql
或者
mysqld remove mysql
```

删除mysql的数据文件

# 二、Mac

[Mysql下载地址](https://dev.mysql.com/downloads/mysql/)


# 三、Docker

安装镜像

```bash
> docker pull mysql:8.0.27
```

运行
```bash
> docker run -itd --name test_server -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123123 mysql:8.0.27
```

# 四、常见命令

## 操作数据库

1、创建数据库
编码方式
- gb2312
- utf-8
- gbk
- iso-8859-1

```sql
create database 数据库名;
create database 数据库名 character set 编码方式;
create database 数据库名 character set 编码方式 collate 排序规则;
```

2、查看所有数据库

```sql
show databases;
# 查看数据定义信息
show create database 数据库名;
```

3、修改数据库

```sql
alter databse 数据库名 character set 编码方式;
```

4、删除数据库

```sql
drop database 数据库名;
```

5、其他操作

查看当前使用的数据库

```sql
select database();
```

切换数据库

```sql
use 数据库名;
```

## 操作数据表

1、创建表

```sql
create table student(
  name varchar(5),
  age int,
  sex char(1)
);
```

2、查看所有表

```sql
show tabals;
```

3、查看表字段信息

```sql
desc student;
```

4、删除表

```sql
drop table student;
```

5、添加列

```sql
# alter table 表名 add 列名 数据类型;
alter table student add image blob;
```

6、修改列

```sql
# alter table 表名 change 旧列名 新列名 新数据类型;
alter table student change name new_name varchar(10);
```

7、删除列

```sql
alter table 表名 drop 列名;
```

8、修改表名

```sql
alter table 旧表名 rename 新表名;
```

9、查看表定义

```sql
show create table 表名;
```

10、修改表字符集

```sql
alter table 表名 character set 编码方式;
```

## DML操作

插入数据

```sql
insert into studnet(stuname, stuage, stusex, birthday) values('avan', 21, 'g', '2000-1-1');
# 可以同时添加多行
insert into studnet(stuname, stuage, stusex, birthday) values('avan', 21, 'g', '2000-1-1'), ('ljj', 22, 'g', '2010-1-1');
# 不指定字段，则需要全部添加
insert into studnet values('avan', 21, 'g', '2000-1-1');
```

查询数据

```sql
select * from student;
```

更新数据

```sql
update student set stuname="lyw", stuage=30 where stuname="layouwen";
```

删除数据

```sql
# 删除所有数据
delete from student;
# 指定数据
delete from student where stuname='layouwen';
# 删除drop表重新创建一样的表，效率比delete高，数据不能找回
trunate table student;
```

## DCL操作

创建用户

```sql
create user 用户名@指定ip identified by 密码;
```

> % 表示任何人都可以访问

权限授权

```sql
grant 权限1, 权限2 on 数据库.表名 to 用户名@ip;
# 所有权限
grant all on *.* to 'root'@'%';
```

查询权限

```sql
show grants for 用户名@ip;
```

撤销用户权限

```sql
revoke 权限1, 权限2 on *.* from 用户名@ip;
```

删除用户

```sql
drop user 用户名@ip;
```

## DQL操作

```sql
select 列名
from 表名
where 条件条件
group by 组名
having 分组后的行条件
order by 排序
limit 开始, 结束
```

测试数据

```sql
# 创建表stu
CREATE TABLE stu (
  sid CHAR(6),
  sname VARCHAR(50),
  age INT,
  gender VARCHAR(50)
);

# 添加数据 
INSERT INTO
  stu
VALUES
  ('S_1001', 'liuYi', 35, 'male');

INSERT INTO
  stu
VALUES
  ('S_1002', 'chenEr', 15, 'female');

INSERT INTO
  stu
VALUES
  ('S_1003', 'zhangSan', 95, 'male');

INSERT INTO
  stu
VALUES
  ('S_1004', 'liSi', 65, 'female');

INSERT INTO
  stu
VALUES
  ('S_1005', 'wangWu', 55, 'male');

INSERT INTO
  stu
VALUES
  ('S_1006', 'zhaoLiu', 75, 'female');

INSERT INTO
  stu
VALUES
  ('S_1007', 'sunQi', 25, 'male');

INSERT INTO
  stu
VALUES
  ('S_1008', 'zhouBa', 45, 'female');

INSERT INTO
  stu
VALUES
  ('S_1009', 'wuJiu', 85, 'male');

INSERT INTO
  stu
VALUES
  ('S_1010', 'zhengShi', 5, 'female');

INSERT INTO
  stu
VALUES
  ('S_1011', 'xxx', NULL, NULL);

# 创建雇员表并添加数据
# 创建雇员表 
CREATE TABLE emp2(
  empno INT,
  ename VARCHAR(50),
  job VARCHAR(50),
  mgr INT,
  hiredate DATE,
  sal DECIMAL(7, 2),
  comm decimal(7, 2),
  deptno INT
);

# 添加数据
INSERT INTO
  emp2
values
  (
    7369,
    'SMITH',
    'CLERK',
    7902,
    '1980-12-17',
    800,
    NULL,
    20
  );

INSERT INTO
  emp2
values
  (
    7499,
    'ALLEN',
    'SALESMAN',
    7698,
    '1981-02-20',
    1600,
    300,
    30
  );

INSERT INTO
  emp2
values
  (
    7521,
    'WARD',
    'SALESMAN',
    7698,
    '1981-02-22',
    1250,
    500,
    30
  );

INSERT INTO
  emp2
values
  (
    7566,
    'JONES',
    'MANAGER',
    7839,
    '1981-04-02',
    2975,
    NULL,
    20
  );

INSERT INTO
  emp2
values
  (
    7654,
    'MARTIN',
    'SALESMAN',
    7698,
    '1981-09- 28',
    1250,
    1400,
    30
  );

INSERT INTO
  emp2
values
  (
    7698,
    'BLAKE',
    'MANAGER',
    7839,
    '1981-05-01',
    2850,
    NULL,
    30
  );

INSERT INTO
  emp2
values
  (
    7782,
    'CLARK',
    'MANAGER',
    7839,
    '1981-06-09',
    2450,
    NULL,
    10
  );

INSERT INTO
  emp2
values
  (
    7788,
    'SCOTT',
    'ANALYST',
    7566,
    '1987-04-19',
    3000,
    NULL,
    20
  );

INSERT INTO
  emp2
values
  (
    7839,
    'KING',
    'PRESIDENT',
    NULL,
    '1981-11-17',
    5000,
    NULL,
    10
  );

INSERT INTO
  emp2
values
  (
    7844,
    'TURNER',
    'SALESMAN',
    7698,
    '1981-09-08',
    1500,
    0,
    30
  );

INSERT INTO
  emp2
values
  (
    7876,
    'ADAMS',
    'CLERK',
    7788,
    '1987-05-23',
    1100,
    NULL,
    20
  );

# 创建部门表
CREATE TABLE dept(deptno INT, dname varchar(14), loc varchar(13));

#添加数据
INSERT INTO
  dept
values
(10, 'ACCOUNTING', 'NEW YORK');

INSERT INTO
  dept
values
(20, 'RESEARCH', 'DALLAS');

INSERT INTO
  dept
values
(30, 'SALES', 'CHICAGO');

INSERT INTO
  dept
values
(40, 'OPERATIONS', 'BOSTON');
```

查询所有列

```sql
select * from stu;
```

查询指定列

```sql
select sid, sname, age from stu;
```

条件查询

> =、!=、<>、<、<=、>、>=、between...and、in(set)、is null、and、or、not

1、查询性别为女，并且年龄50以内的记录  
```sql
SELECT * FROM stu WHERE gender='female' AND age<50;
```

2、查询学号为S_1001，或者姓名为liSi的记录  
```sql
SELECT * FROM stu WHERE sid ='S_1001' OR sname='liSi';
```

3、查询学号为S_1001，S_1002，S_1003的记录  
列名 in (列值1,列值2)
```sql
SELECT * FROM stu WHERE sid IN ('S_1001','S_1002','S_1003');
```

4、查询学号不是S_1001，S_1002，S_1003的记录  
```sql
SELECT * FROM tab_student WHERE sid NOT IN('S1001','S1002','S_1003');
```

5、查询年龄为null的记录
```sql
SELECT * FROM stu WHERE age IS NULL;
```

6、查询年龄在20到40之间的学生记录
```sql
SELECT * FROM stu WHERE age>=20 AND age<=40;
SELECT * FROM stu WHERE age BETWEEN 20 AND 40;
```

7、查询性别非男的学生记录
```sql
SELECT * FROM stu WHERE gender!='male';
SELECT * FROM stu WHERE gender<>'male';
SELECT * FROM stu WHERE NOT gender='male';
```

8、查询姓名不为null的学生记录
```sql
SELECT * FROM stu WHERE NOT sname IS NULL;
SELECT * FROM stu WHERE sname IS NOT NULL;
```

模糊查询
> 列名 like '表达式'  
> 通配符  
> _: 任意一个字符  
> %：任意0~n个字符

1、查询姓名由3个字构成的学生记录
```sql
SELECT * FROM stu WHERE sname LIKE '___';
```

2、查询姓名由5个字母构成，并且第5个字母为“i”的学生记录
```sql
SELECT * FROM stu WHERE sname LIKE '____i';
```

3、查询姓名以“z”开头的学生记录
```sql
SELECT * FROM stu WHERE sname LIKE 'z%';
```

4、查询姓名中第2个字母为“i”的学生记录
```sql
SELECT * FROM stu WHERE sname LIKE '_i%';
```

5、查询姓名中包含“a”字母的学生记录
```sql
SELECT * FROM stu WHERE sname LIKE '%a%';
```

字段控制查询

排序

聚合函数

分组查询

limit

## 数据完整性

引用完整性
- primary key
- unique [key]
- not null
- default
- auto_increment
- foregin key
