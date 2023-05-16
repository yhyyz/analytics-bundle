---
title : "SQL语句创建(选做)"
weight : 11
---
::alert[本小节内容完成通过SQL语句创建Datashares，再和调度集系统集成时可以用SQL方式统一管理]{type=info}

:::code{showCopyAction=true showLineNumbers=true language=sql}
-- 获取consumer集群namespace
select current_namespace;

-- 在producer集群进行如下操作，其中NAMESPACE 6d043a27-b5ee-4fc2-9cad-cd3e748a8351替换为上一步获取的consumer集群的NAMESPACE
-- 创建 dev_share
CREATE DATASHARE dev_share SET PUBLICACCESSIBLE TRUE;
-- 将public schema 添加到 dev_share
ALTER DATASHARE dev_share ADD SCHEMA public;
-- 将public scheam中的表全部添加到dev_share
ALTER DATASHARE dev_share ADD ALL TABLES IN SCHEMA public;
-- 将创建的dev_share 给consumer集群
GRANT USAGE ON DATASHARE dev_share TO NAMESPACE '6d043a27-b5ee-4fc2-9cad-cd3e748a8351';

-- 查看创建datashares
show datashares;
select * from SVV_DATASHARES;
select * from svv_datashare_objects;

-- 移除某张表
ALTER DATASHARE dev_share REMOVE TABLE public.customer;

-- producer namespace（producer集群执行获取namespace）
select current_namespace;

-- 在consumer集群进行如下操作
-- 从datashare建database,NAMESPACE '28fa3f2a-06f7-4a46-a967-d2da6af2b175'替换为producer的namespace
CREATE DATABASE dev_producer FROM DATASHARE dev_share OF NAMESPACE '28fa3f2a-06f7-4a46-a967-d2da6af2b175';
select * from dev_producer.public.test limit 100;

-- 新建的表自动share （producer执行）
ALTER DATASHARE dev_share SET INCLUDENEW TRUE FOR SCHEMA public;
-- 创建表插入数据 （producer执行）
create table share_tb_01(id varchar(255),name varchar(255))
insert into share_tb_01 values('1','aws');

-- consumer 执行
select * from dev_producer.public.share_tb_01
:::
