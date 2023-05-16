---
title : "Super使用"
weight : 11
---

::alert[本小节内容使用Redshift加载Super类型数据]{type=info}

1. Super类型使用的实验[点击这里](https://catalog.us-east-1.prod.workshops.aws/workshops/9f29cdba-66c0-445e-8cbb-28a092cb5ba7/en-US/lab15)已经有详细的使用方式不在重复编写，可以直接使用该链接的文档在当前`redshift-producer`集群中进行实验。

2. Cloud9获取ARN，在Lab1中已有说明，这里将命令再写出来，执行如下命令即可获取
:::code{showCopyAction=true showLineNumbers=true language=shell}
aws iam  get-role --role-name redshift-workshop-admin-role |grep Arn |awk '{print $2}'|sed 's/\"//g' 
:::