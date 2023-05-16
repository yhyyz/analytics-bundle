---
title : "创建MySQL"
weight : 11
---
::alert[开始本步骤前，请确保已经完成了准备工作中的实验。本小节内容会完成创建RDS MySQL实例，开启Binlog相关参数。]{type=error}

1. 在AWS控制台搜索RDS,点击进入RDS管理页面 ![rds-mysql-search](/static/imgs/redshift/rds-mysql-search.png)
   
2. 选择左侧列表的Parameter groups ,点击右上角的创建按钮，创建一个参数组，我们可以通过参数组修改MySQL的Binlog format默认配置。![rds-mysql-create-pg](/static/imgs/redshift/rds-mysql-create-pg.png)

3. 选择mysql8.0 ,在Group name输入`workshop-group`, Description输入`workshop-group`, 点击页面下方创建按钮 ![rds-mysql-create-pg-page-01](/static/imgs/redshift/rds-mysql-create-pg-page-01.png)
   
4. 点击创建的参数组，搜索`binlog_format`, 点击编辑按钮，选择binlog_format的values为`ROW`,点击保存即可![rds-mysql-create-pg-page-02](/static/imgs/redshift/rds-mysql-create-pg-page-02.png)

5. 在左侧列表选择Subnet group, 点击右上角创建子网组![rds-mysql-create-sg](/static/imgs/redshift/rds-mysql-create-sg.png)

6. 输入子网组名称`workshop-mysql-sg`，描述信息`workshop-mysql-sg` 选择VPC `redshift-workshop-vpc` ![rds-mysql-create-sg-page-01](/static/imgs/redshift/rds-mysql-create-sg-page-01.png)

7. 在Add subnets选择可用区为`us-east-1a,us-east-1b`, 选择所有子网, 之后在页面下方点击创建即可![rds-mysql-create-sg-page-02](/static/imgs/redshift/rds-mysql-create-sg-page-02.png) ![rds-mysql-create-sg-page-03](/static/imgs/redshift/rds-mysql-create-sg-page-03.png)

8. 在左侧列表选择Databases, 点击右上角创建Database ![rds-mysql-create-db](/static/imgs/redshift/rds-mysql-create-db.png)

9.  选择Standard create, Engine options选择MySQL  ![rds-mysql-create-db-page-01](/static/imgs/redshift/rds-mysql-create-db-page-01.png)

10. 滑动到Settings选项卡，数据DB instance名称`workshop-mysql` ,填写您的密码, 之后需要用到该用户名密码登陆。![rds-mysql-create-db-page-02](/static/imgs/redshift/rds-mysql-create-db-page-02.png)

11. 滑动到Network type选择VPC`redshift-wrokshop-vpc`, 选择子网组`workshop-mysql-sg`。 ![rds-mysql-create-db-page-03](/static/imgs/redshift/rds-mysql-create-db-page-03.png) 

12. 滑动到Additionl Configuration，展开该选项，在DB parameter group 选择`workshop-group`。![rds-mysql-create-db-page-04](/static/imgs/redshift/rds-mysql-create-db-page-04.png)

13. 滑动到页面下方点击创建即可 ![rds-mysql-create-db-page-05](/static/imgs/redshift/rds-mysql-create-db-page-05.png)

14. 创建数据库实例需要几分钟的时间, 等到数据库状态变为可用时，即表明创建成功 ![rds-mysql-create-db-finish](/static/imgs/redshift/rds-mysql-create-db-finish.png)
