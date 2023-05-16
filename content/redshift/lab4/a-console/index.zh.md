---
title : "可视化创建Datashares"
weight : 11
---
::alert[本小节内容完成通过AWS管理控制台完成Datashares]{type=info}

::alert[开始本实验前请按照Lab1中创建Redshit集群的方式，再创建一个名称为`redshift-consumer`的新集群。接下来的实验我们会将`redshift-producer`集群中的`test_db`库中的`product`表共享给`redshift-consumer`集群]{type=warning}

- [Poducer Create Datahare](#poducer-create-datahare)
- [Consumer Query Data](#consumer-query-data)

##### Poducer Create Datahare

1. 在Redshift管理控制台中，进入到`redshift-producer`集群管理页面 ![ds-create](/static/imgs/redshift/lab4/ds-create.png)
   
2. 点击Connect to database连接到数据库。 之后点击create datashare。 ![ds-create-page-01](/static/imgs/redshift/lab4/ds-create-page-01.png)  ![ds-create-page-02](/static/imgs/redshift/lab4/ds-create-page-02.png)

3. 在创建页面输入Datashare的名称`producer_share` ![ds-create-page-03](/static/imgs/redshift/lab4/ds-create-page-03.png)

4. 滑动到Datashare ojbects选项卡，点击右上角的Add。![ds-create-page-04](/static/imgs/redshift/lab4/ds-create-page-04.png)

5. Schema选择test_db, Object types选择Tables and views, Add objects选择添加指定对象。在列表中选择`product`表，点击Add即可 ![ds-create-page-05](/static/imgs/redshift/lab4/ds-create-page-05.png)  ![ds-create-page-06](/static/imgs/redshift/lab4/ds-create-page-06.png)
   
6. 滑动到Data consumer配置项，勾选Add namespaces to the datashare, 在下拉框找那个选择`redshift-consumer`，点击创建即可。![ds-create-page-07](/static/imgs/redshift/lab4/ds-create-page-07.png)

7. 创建完毕后可以在Datashare页面查看管理。![ds-create-page-08](/static/imgs/redshift/lab4/ds-create-page-08.png)


##### Consumer Query Data

1. 进入到`redshift-consumer`集群管理页面，点击Datashares选项卡，滚动页面到Datashares from other namespaces and AWS accounts配置项，可以看到`redshift-producer`的分享数据，勾选`producer_share`, 点击Create database from datashare。![ds-cons-create](/static/imgs/redshift/lab4/ds-cons-create.png)

2. 输入要创建的数据库名称`producer_share_db` ,点击创建即可 ![ds-cons-create-page-01](/static/imgs/redshift/lab4/ds-cons-create-page-01.png)
   
3. 在Redshift查询编辑器中，连接到Redshift Consumer集群，选择创建的Share Database，可以看到已经分享过来的数据Schema和product表。执行`select * from product` 可以查询到Share过来的数据。![ds-query-create](/static/imgs/redshift/lab4/ds-query-create.png) ![ds-query-result](/static/imgs/redshift/lab4/ds-query-result.png)