---
title : "Federated Query"
weight : 11
---
::alert[本小节内容将完成Redshift通过Federated Query查询MySQL中的数据]{type=info}

::alert[Federated Query需要将MySQL的链接信息存储到Secrets Manager中,Redshift从Secrets Manager获取相关链接信息与MySQL连通。同时我们为联邦查询创建一个IAM Role并关联到Redshift集群]{type=warning}

- [Craete MySQL Secrets](#craete-mysql-secrets)
- [Craete Redshift Federated Role](#craete-redshift-federated-role)
- [Associated IAM roles to Redshift](#associated-iam-roles-to-redshift)
- [Get Secrets and Role ARN](#get-secrets-and-role-arn)
- [Federated Query](#federated-query)

##### Craete MySQL Secrets

1. 在AWS管理控制台搜索Secrets Manager进入到管理界面，点击Store a new secret。![fed-query-secret-create](/static/imgs/redshift/fed-query-secret-create.png)

2. 选择RDS database, 在Credentials中输入之前创建MySQL设定的用户名和密码。![fed-query-secret-create-page-01](/static/imgs/redshift/fed-query-secret-create-page-01.png)

3. 滑动到页面下方在Database选项卡中勾选workshop-mysql，点击Next进入下一步。![fed-query-secret-create-page-02](/static/imgs/redshift/fed-query-secret-create-page-02.png)

4. 在Secret name中输入`federated/mysql`,其它参数保持不变，点击Next至最后页面，点击Store即可。![fed-query-secret-create-page-03](/static/imgs/redshift/fed-query-secret-create-page-03.png) ![fed-query-secret-create-page-04](/static/imgs/redshift/fed-query-secret-create-page-04.png)

##### Craete Redshift Federated Role

1. 在AWS管理控制台中搜索`iam`，点击进入IAM管理界面 ![iam-search](/static/imgs/redshift/iam-search.png)

2. 点击左侧列表的Roles，进入Role的创建页面 ![iam-create-role](/static/imgs/redshift/iam-create-role.png)

3. 点击右上角Create role按钮 ![iam-create-role-page-01](/static/imgs/redshift/iam-create-role-page-01.png)

4. 在Use case的搜索框中输入Redshift，之后选择Redshift customizable, 点击Next ![fed-query-role-create](/static/imgs/redshift/fed-query-role-create.png)

5. 在搜索框中输入`AdministratorAccess`,之后按Enter建，在搜索列表中勾选`AdministratorAccess`,点击Next ![iam-create-role-page-03](/static/imgs/redshift/iam-create-role-page-03.png) ![iam-create-role-page-04](/static/imgs/redshift/iam-create-role-page-04.png)
  ::alert[注意这里并未遵循权限最小化原则，生产使用时请根据时间情况控制权限]{type=warning}

6. Role名称填写`redshift-federated-admin-role`, 其它保持默认，滑动到页面下方点击创建即可 ![fed-query-role-create-finish](/static/imgs/redshift/fed-query-role-create-finish.png)


##### Associated IAM roles to Redshift

1. 进入到我们创建的`redshift-producer`集群管理页面，点击Properties. ![fed-query-sql-create](/static/imgs/redshift/fed-query-sql-create.png)

2. 滑动页面到Associated IAM roles选项卡，点击右侧Manage IAM roles, 选择Associate IAM roles. ![fed-query-sql-create-page-01](/static/imgs/redshift/fed-query-sql-create-page-01.png)

3. 在弹出的对话框中勾选redshift-federated-admin-role，点击Associated iam roles即可, 需要几十秒时间生效 ![fed-query-sql-create-page-02](/static/imgs/redshift/fed-query-sql-create-page-02.png)

##### Get Secrets and Role ARN

登陆到Cloud9控制台，执行如下命令获取，之前创建的名称为`federated/mysql`的Secrets的ARN和`redshift-federated-admin-role`的ARN，创建Federated Query需要此信息。ARN也可在AWS管理控制台访问Secrets Manager服务和IAM服务获取。

:::code{showCopyAction=true showLineNumbers=true language=shell}
aws rds  describe-db-instances |grep Address|grep "workshop-mysql"|awk '{print $2}'|sed 's/\"//g'
aws iam  get-role --role-name redshift-federated-admin-role |grep Arn |awk '{print $2}'|sed 's/\"//g' 
aws secretsmanager list-secrets |grep ARN|grep "federated/mysql"|awk '{print $2}'|sed 's/\"//g'
:::
![fed-query-arn-get](/static/imgs/redshift/fed-query-arn-get.png)

##### Federated Query

1. 进入到Redshift查询编辑器，连接到数据库。执行如下SQL创建External Schema. SQL语句中URI，IAM_ROLE, SECRET_ARN 从上一步在Cloud9执行的命令的结果获取。
:::code{showCopyAction=true showLineNumbers=true language=sql}

DROP SCHEMA if exists mysql;

CREATE EXTERNAL SCHEMA mysql
FROM MYSQL
DATABASE 'test_db'
URI 'workshop-mysql.xxxxxx.us-east-1.rds.amazonaws.com'
IAM_ROLE 'arn:aws:iam::xxxxxx:role/redshift-federated-admin-role'
SECRET_ARN 'arn:aws:secretsmanager:us-east-1:xxxxxx:secret:federated/mysql-tjCFzm';

select * from mysql.product;
:::

执行SQL后我们可以在Redshift中看到MySQL test_db的所有表，同时也可以直接执行SQL查询表中的数据 ![fed-query-start-result](/static/imgs/redshift/fed-query-start-result.png) ![fed-query-start-result-01](/static/imgs/redshift/fed-query-start-result-01.png)

