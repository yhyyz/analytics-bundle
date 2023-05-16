---
title : "QuickSight Redshift"
weight : 11
---
::alert[本小节内容使用QuickSight连接到Redshift查询数据]{type=info}

1. 在AWS管理控制台搜索QuickSight点击进入, 点击Signup在下个页面选择Enterpise,点击页面下方的Continue。 ![qs-create](/static/imgs/redshift/lab4/qs-create.png) ![qs-create-page-01](/static/imgs/redshift/lab4/qs-create-page-01.png)

2. 输入QuickSight的账号名称`workshopquicksightxxxx`, 因为名字要唯一所以`xxxx`替换为随机的字符串， 输入您的邮箱地址。 ![qs-create-page-02](/static/imgs/redshift/lab4/qs-create-page-02.png)

3. 其它配置保持默认，点击页面下方的Finish即可。 ![qs-create-page-03](/static/imgs/redshift/lab4/qs-create-page-03.png)

4. [点击链接](https://us-east-1.quicksight.aws.amazon.com/sn/console/vpc-connections/new?#)创建QuickSight VPC连通到Redshift，VPC name填写`workshop-qs-vpc`,VPC ID选择Redshit VPC，子网ID任选一个，Security Group ID点击蓝色的Security Gropp链接，获取redshift-workshop-vpc所在的安全组ID。其它保持默认，点击创建即可。VPC创建后需要等待`1~3`分钟后再使用 ![qs-vpc-create](/static/imgs/redshift/lab4/qs-vpc-create.png)

5. [点击链接](https://us-east-1.quicksight.aws.amazon.com/sn/data-sets/new)进入创建数据源页面, 选择Redshift[Auto-discovered], Data source name填写`redshift-consumer`, Connection type选择`workship-qs-vpc`,数据库名称填写`dev`,用户名密码填写创建Redshift时的用户名密码。点击Validated验证是否通过，通过后点击创建即可。![qs-datasource-create](/static/imgs/redshift/lab4/qs-datasource-create.png)

6. 点击use customer sql。填写SQL名称，输入SQL语句`select * from producer_share_db.test_db.product` ,点击Confirm query ![qs-datasource-create-page-01](/static/imgs/redshift/lab4/qs-datasource-create-page-01.png) ![qs-datasource-create-page-02](/static/imgs/redshift/lab4/qs-datasource-create-page-02.png)

7. 点击Visualize进入可视化界面。![qs-datasource-create-page-03](/static/imgs/redshift/lab4/qs-datasource-create-page-03.png)

8. 拖动pname列到X axis,拖动ppice到Value，可以看到可视化的图表。QuickSight功能很强大，可以[点击Workshop链接学习使用](https://catalog.us-east-1.prod.workshops.aws/workshops/cd8ebba2-2ef8-431a-8f72-ca7f6761713d/) ![qs-query-result](/static/imgs/redshift/lab4/qs-query-result.png)

