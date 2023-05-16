---
title : "Crawler Redshift"
weight : 11
---

::alert[本小节内容使用Glue Crawler爬取Redshift中的表]{type=info}

- [Prepare data](#prepare-data)
- [Glue Crawler](#glue-crawler)

##### Prepare data

1. 登陆Cloud9的执行如下命令，创建S3桶，写临时数据到S3桶。
:::code{showCopyAction=true showLineNumbers=true language=shell}
aws s3api  create-bucket --bucket workshop-bucket-$RANDOM`date +%s`
echo '{"id":1,"name":"customer","age":16,"nickname":"awscustomer"}' > customer.json
bucket_name=`aws s3 ls / |grep workshop-bucket|gawk '{print $3}'`
aws s3 cp customer.json s3://$bucket_name/input/
:::
![s3-create](/static/imgs/redshift/lab3/s3-create.png)

2. 进入Redshift查询编辑器，在`redshift-producer`集群中执行如下SQL，创建Customer表
:::code{showCopyAction=true showLineNumbers=true language=sql} 
create table customer (id int, name varchar, age int ,nickname varchar);
:::
![s3-redshift-create](/static/imgs/redshift/lab3/s3-redshift-create.png)

3. [点击进入VPC Endpoint界面](https://us-east-1.console.aws.amazon.com/vpc/home?region=us-east-1#Endpoints:) 为Endpoint添加路由表。![s3-route](/static/imgs/redshift/lab3/s3-route.png) ![s3-route-01](/static/imgs/redshift/lab3/s3-route-01.png)

##### Glue Crawler

1. [点击链接进入Glue connection创建一个Redshift connection](https://us-east-1.console.aws.amazon.com/glue/home?region=us-east-1#addEditConnection:) 添加connection名称`glue-redshift-connection`, 选择链接类型(Amazon redshift) ![glue-conn-create](/static/imgs/redshift/lab3/glue-conn-create.png)

2. 选择`redshift-producer`集群，输入用户名密码。其它配置保持默认，点击直到完成。![glue-conn-create-01](/static/imgs/redshift/lab3/glue-conn-create-01.png)

3. 创建完成后选中创建的链接，在Action点击编辑，进入到网络连接配置项，选择子网为私有子网，之后选择完成即可 ![glue-conn-create-02](/static/imgs/redshift/lab3/glue-conn-create-02.png)

4. 选中创建的connection点击测试链接，IAM Role选择`redshift-workshop-amdin-role`。测试链接需要几分钟时间。![glue-conn-create-03](/static/imgs/redshift/lab3/glue-conn-create-03.png)

5. [点击链接进入Glue Crawler页面](https://us-east-1.console.aws.amazon.com/glue/home?region=us-east-1#addCrawler:),填写crawler名称`glue-redshift-crawler` ,点击Next ![glue-crawler](/static/imgs/redshift/lab3/glue-crawler.png)

6. 点击Next进入到Data store配置项，选择JDBC，链接选择glue-reshift-connection, Include path填写`dev/public/customer` 表示Redshift的dev数据库，默认的public Schema下的customer表。![glue-crawler-01](/static/imgs/redshift/lab3/glue-crawler-01.png)

7. 点击Next到IAM Role配置项，选择`redshift-workshop-admin-role` ![glue-crawler-02](/static/imgs/redshift/lab3/glue-crawler-02.png)

8. 点击Next到Output配置项，添加一个default数据库，然后选择它。![glue-crawler-03](/static/imgs/redshift/lab3/glue-crawler-03.png)

9. 其它配置保持默认，点击到最后一页点击创建即可。完成后勾选创建的Crawler，点击Run Crawler运行。![glue-crawler-run](/static/imgs/redshift/lab3/glue-crawler-run.png)

10. 运行成功后可以在Table页面查看爬取过来的`Customer`表。![glue-crawler-view](/static/imgs/redshift/lab3/glue-crawler-view.png) ![glue-crawler-view-01](/static/imgs/redshift/lab3/glue-crawler-view-01.png)