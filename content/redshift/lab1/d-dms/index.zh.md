---
title : "创建DMS复制实例"
weight : 11
---
::alert[本小节内容将完成DMS任务创建]{type=info}

1. 在AWS管理控制台搜索DMS,进入到DMS管理界面，选择左侧列表Replication instances,点击右上角的创建按钮。![dms-ri-create](/static/imgs/redshift/dms-ri-create.png)

2. 输入复制实例的名称`mysql-cdc-instance`, 输入描述`mysql-cdc-instance`，向下滑动页面到VPC选择`redshift-workshop-vpc`, Multi AZ选项选择`Dev or test`,注意生产环境建议选择Multi AZ。 ![dms-ri-create-page-01](/static/imgs/redshift/dms-ri-create-page-01.png) ![dms-ri-create-page-02](/static/imgs/redshift/dms-ri-create-page-02.png)

3. 滑动页面到到Advance security and network configuration选择Availability zone为`us-east-1a`, VPC security group 选择default。其它选择保持默认，到页面最下方点击创建即可![dms-ri-create-page-03](/static/imgs/redshift/dms-ri-create-page-03.png) ![dms-ri-create-page-04](/static/imgs/redshift/dms-ri-create-page-04.png)

4. 创建复制实例需要几分钟的时间，创建完毕后可以在DMS控制台查看![dms-ri-create-finish](/static/imgs/redshift/dms-ri-create-finish.png)