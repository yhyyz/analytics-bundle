---
title : "创建Redshift"
weight : 11
---
::alert[本小节内容将完成Redshift集群创建]{type=info}

1. 在AWS管理控制台搜索Redshift,进入Redshift管理页面，选择Subnet group, 在右上角点击创建子网组 ![redshift-create-sg](/static/imgs/redshift/redshift-create-sg.png)

2. 输入子网组名称`workshop-sg`, 输入子网组描述`workshop subnet group`，选择`redshift-workshop-vpc`, 点击`add all the subnets for this vpc`将所有子网添加，之后在页面下方点击创建子网组即可 ![redshift-create-sg-page-01](/static/imgs/redshift/redshift-create-sg-page-01.png) ![redshift-create-sg-page-02](/static/imgs/redshift/redshift-create-sg-page-02.png)

3. 在左侧列表选择Provisioned clusters dashboard, 在右上角点击Create创建集群 ![redshift-create](/static/imgs/redshift/redshift-create.png)

4. 输入集群名称`redshift-producer`,滑动页面到Database Configurations填写用户名和密码，请记住您填写的用户名和密码，之后会使用到![redshift-create-page-01](/static/imgs/redshift/redshift-create-page-01.png) ![redshift-create-page-02](/static/imgs/redshift/redshift-create-page-02.png)

5. 滑动页面到Associated IAM roles，点击Associated，在弹出的页面中选择`redshift-workshop-admin-role`,点击Associate IAM Roles。![redshift-create-page-03](/static/imgs/redshift/redshift-create-page-03.png) ![redshift-create-page-04](/static/imgs/redshift/redshift-create-page-04.png)

6. 滑动页面到Network and security，关闭user default按钮，选择VPC为`redshift-workshop-vpc`, 其它保持默认，滑动到页面下方，点击创建集群即可  ![redshift-create-page-05](/static/imgs/redshift/redshift-create-page-05.png) ![redshift-create-finish](/static/imgs/redshift/redshift-create-finish.png)

7. 集群创建需要等待几分钟，创建完毕后可以在Reshift集群管理页面查看已经创建好的集群![redshift-create-finish-view](/static/imgs/redshift/redshift-create-finish-view.png)
