---
title : "可视化ETL"
weight : 11
---

::alert[本小节内容使用Glue Studio读取S3上的数据发送到Redshift]{type=info}

1. [点击链接](https://us-east-1.console.aws.amazon.com/gluestudio/home?region=us-east-1#/jobs)进入Glue Studio 作业创建页面。 选择Visual with a blank canvas，点击Create。![studio-01](/static/imgs/redshift/lab3/studio-01.png)

2. 添加作业名称为`glue-etl-redshift`,点击Source选择S3,右侧列表中选择s3 location, 点击Browse S3，找到我们上床到S3上的`customer.json`文件(也可以选择input路径，会自动读取该路径下所有文件)，文件类型选择Json。![studio-02](/static/imgs/redshift/lab3/studio-02.png)

3. 点击Target，选择Redshfit，左侧配置页面选择选择Database为default，表为dev_public_customer, 数据写入方式选择Insert。![studio-03](/static/imgs/redshift/lab3/studio-03.png)

4. 打开高级选项卡，临时目录添加你自己的S3桶加上路径，例如`s3://bucket/glue/tmp/`。IAM Role选择`redshift-workshop-admin-role`。![studio-04](/static/imgs/redshift/lab3/studio-04.png)

5. 切换到Job details选择卡， IAM Role选择`redshift-workshop-admin-role`。![studio-05](/static/imgs/redshift/lab3/studio-05.png)

6. 滚动页面设置Workers的个数为2，作业重试次数为0.![studio-06](/static/imgs/redshift/lab3/studio-06.png)

7. 展开高级配置项, Script path 填写自己的S3桶加上路径, 例如：`s3://bucket/scripts/`。禁用Spark UI日志，一般调试作业时可以打开。其它配置保持默认，点击页面上方的Save后点Run击运行即可。![studio-07](/static/imgs/redshift/lab3/studio-07.png)

8. 在运行页面选项卡可以看到作业运行的状态，以及作业在CloudWatch中的日志链接等信息。![studio-finish](/static/imgs/redshift/lab3/studio-finish.png)

9. 进入Redshit查询编辑器执行SQL查询`select * from customer` 可以看到数据已经写入到customer表。![studio-view](/static/imgs/redshift/lab3/studio-view.png)

