---
title : "创建DMS Endpoints"
weight : 11
---
::alert[本小节内容将完成DMS Endopints创建]{type=info}

- [MySQL Source Endpoint](#mysql-source-endpoint)
- [Redshift Target Endpoint](#redshift-target-endpoint)

#### MySQL Source Endpoint

1. 在AWS管理控制台搜索DMS,进入到DMS管理界面，选择左侧列表Endpoints,点击右上角的创建按钮。![dms-end-create](/static/imgs/redshift/dms-end-create.png)

2. 选择`Source endpoint`, 勾选Select RDS instance, 选择`workshop-mysql`实例![dms-end-create-page-01](/static/imgs/redshift/dms-end-create-page-01.png)

3. 在Endpoint configuration中勾选Provide access information manually，填写创建RDS MySQL设置的密码。![dms-end-create-page-02](/static/imgs/redshift/dms-end-create-page-02.png)

4. 滑动到页面下面点击Test endpoint connection, 选择`redshift-workshop-vpc`,点击Run test,等待测试状态为Successful时，点击创建即可![dms-end-create-page-03](/static/imgs/redshift/dms-end-create-page-03.png)

5. 创建完毕后可以在Enpoints页面查看![dms-end-create-finish](/static/imgs/redshift/dms-end-create-finish.png)

#### Redshift Target Endpoint

1. 进入到Endpoints创建页面,选择`Target endpoint`。Endpoint identifer输入名称`redshift-endpoint`, Target engine 选择`Amazon Redshift` ![dms-end-target-create](/static/imgs/redshift/dms-end-target-create.png)

2. 在AWS Redshift控制台界面Copy Redshift链接。![dms-end-target-create-page-01](/static/imgs/redshift/dms-end-target-create-page-01.png)

3. 将copy后的地址`redshift-producer.xxxxxx.redshift.amazonaws.com:5439/dev`, 只保留主机名`redshift-producer.xxxxxx.redshift.amazonaws.com`填入到Server name 字段中，Port字段填写`5439`，User name和Password填写创建Redshift集群时的用户名密码。Database name填写`dev`   ![dms-end-target-create-page-01](/static/imgs/redshift/dms-end-target-create-page-02.png)

4. 其它参数保持默认(不需要点击测试链接，测试链接可以在创建好Endpoint之后，再点击测试。在控制台创建时会自动创建`dms-access-for-endpoint` 的角色，因为DMS将数据投递到Redshift，是先将数据写入S3之后将数据从S3 copy到Redshift，因此该角色需要S3的访问权限)，滑动到页面下方点击创建即可。创建完毕后可以在Endpoints页面查看管理 ![dms-end-target-create-finish](/static/imgs/redshift/dms-end-target-create-finish.png) ![dms-end-target-create-view](/static/imgs/redshift/dms-end-target-create-view.png)
