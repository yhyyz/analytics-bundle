---
title : "加载数据到MySQL"
weight : 11
---
::alert[本节内容通过Cloud9 MySQL client加载数据到 MySQL中]{type=info}

1. 配置安全组，允许Cloud9访问RDS MySQL。选择我们创建的VPC的Default安全组(MySQL配置的为此默认安全组)，点击`Inbound rules->Edit Inbound rule->Add rule`, 添加一条rule, Type选中`All traffic` ，Target选则`cloud9安全组`之后点击保存。 ![mysql-load-data-sg](/static/imgs/redshift/mysql-load-data-sg.png)  ![mysql-load-data-sg-01](/static/imgs/redshift/mysql-load-data-sg-01.png)

2. 在AWS管理控制台搜索Cloud9，进入已经创建好Cloud9环境。创建一个新的命令行终端。 ![mysql-load-data](/static/imgs/redshift/mysql-load-data.png)
   
3. 在终端中执行下面命令，获取MySQL的Endpoint(也可以直接在AWS控制台搜索RDS进入到Databases页面点击数据库实例查看)
:::code{showCopyAction=true showLineNumbers=true language=shell}
mysql_host=`aws rds  describe-db-instances |grep Address|grep "workshop-mysql"|awk '{print $2}'|sed 's/\"//g'`
echo $mysql_host
:::
![mysql-load-data-page-01](/static/imgs/redshift/mysql-load-data-page-01.png)

4. 执行如下命令通过MySQL Client连接到MySQL
:::code{showCopyAction=true showLineNumbers=true language=shell}
mysql -h $mysql_host -u admin -p
:::
![mysql-load-data-page-02](/static/imgs/redshift/mysql-load-data-page-02.png)

5. 复制以下命令到MySQL Client创建表加载样例数据，并查看执行结果
:::code{showCopyAction=true showLineNumbers=true language=sql}
-- create databases
create database if not exists test_db default character set utf8mb4 collate utf8mb4_general_ci;
use test_db;

-- create  address table
drop table if exists address;
create table if not exists address
(
    id           int auto_increment primary key,
    aid          varchar(155)                        null,
    acity        varchar(155)                        null,
    aname        varchar(155)                        null,
    create_time  timestamp default CURRENT_TIMESTAMP not null,
    modify_time  timestamp default CURRENT_TIMESTAMP null on update CURRENT_TIMESTAMP
)charset = utf8mb4;

-- insert data
insert into address(aid,acity,aname) values 
('a10001','beijing','chaoyang'),
('a10002','shanghai','pudong');

-- create  user table
drop table if exists user;
create table if not exists user
(
    id           int auto_increment primary key,
    name         varchar(155)                        null,
    device_model varchar(155)                        null,
    email        varchar(50)                         null,
    phone        varchar(50)                         null,
    create_time  timestamp default CURRENT_TIMESTAMP not null,
    modify_time  timestamp default CURRENT_TIMESTAMP null on update CURRENT_TIMESTAMP,
    aid          varchar(155)                        null
)charset = utf8mb4;

-- insert data
insert into user(name,device_model,email,phone,aid) values
('customer-01','dm-01','abc01@email.com','188776xxxxx','a10001'),
('customer-02','dm-02','abc02@email.com','166776xxxxx','a10002');

-- create product table
drop table if exists product;
create table if not exists product
(
    pid          int not null primary key,
    pname        varchar(155)                        null,
    pprice       decimal(10,2)                           ,
    create_time  timestamp default CURRENT_TIMESTAMP not null,
    modify_time  timestamp default CURRENT_TIMESTAMP null on update CURRENT_TIMESTAMP
)charset = utf8mb4;

-- insert data
insert into product(pid,pname,pprice) values
('1','prodcut-001',125.12),
('2','prodcut-002',225.31);

-- create order table
drop table if exists user_order;
create table if not exists user_order
(
    id           int auto_increment primary key,
    oid          varchar(155)                        not null,
    uid          int                                         ,
    pid          int                                         ,
    onum         int                                         ,
    create_time  timestamp default CURRENT_TIMESTAMP not null,
    modify_time  timestamp default CURRENT_TIMESTAMP null on update CURRENT_TIMESTAMP
)charset = utf8mb4;

-- insert data
insert into user_order(oid,uid,pid,onum) values 
('o10001',1,1,100),
('o10002',1,2,30),
('o10001',2,1,22),
('o10002',2,2,16);

-- select data
select *from address;
select * from user;
select * from product;
select * from user_order;
:::
![mysql-load-data-finish](/static/imgs/redshift/mysql-load-data-finish.png)