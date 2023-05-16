---
title : "湖仓融合Spectrum"
weight : 11
---

::alert[本小节内容使用Redshift Spectrum直接查询S3中数据]{type=info}

1. 使用Reshift Spectrum非常简单，只需要创建一个External Schema，之后创建表指向你要查的S3路径即可。登陆Redshift查询编辑器，执行如下SQL。 表中的database写的default指的是Glue Catalog中的databse。Redshift Spectrum的元数据可以在Glue Catalog中存储,也只在hive metastore中存储。iam_role我们依然使用`redshift-workshop-admin-role`，获取Role的方式命令Lab1中已有说明，这里再贴一下命令行方式获取

:::code{showCopyAction=true showLineNumbers=true language=shell}
aws iam  get-role --role-name redshift-federated-admin-role |grep Arn |awk '{print $2}'|sed 's/\"//g' 
:::

:::code{showCopyAction=true showLineNumbers=true language=sql}
create external schema spectrum_schema
from data catalog 
database 'default' 
iam_role 'arn:aws:iam::xxxxxxxxx:role/redshift-workshop-admin-role'
create external database if not exists;
:::
![spectrum-01](/static/imgs/redshift/lab3/spectrum-01.png)

1. 执行如下SQL创建一个JSON Format的表读取我们上传到S3的customer.json数据。 创建表语句的location指的是上传到S3的路径。如果忘记了路径可以在S3的管理控制台找到。也可以在Cloud9通过命令行查看

:::code{showCopyAction=true showLineNumbers=true language=shell}
bucket=`aws s3 ls |grep workshop-bucket |awk '{print $3}'`
echo s3://$bucket/input/
:::

![spectrum-03](/static/imgs/redshift/lab3/spectrum-03.png)

:::code{showCopyAction=true showLineNumbers=true language=sql}
create external table spectrum_schema.customer_s3(
id int,
name varchar,
nickname varchar,
age int
)
ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
stored as textfile
location 's3://workshop-bucket-xxxxxxx/input/'

select * from spectrum_schema.customer_s3
:::
![spectrum-02](/static/imgs/redshift/lab3/spectrum-02.png)

