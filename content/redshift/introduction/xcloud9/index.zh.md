---
title : "创建Cloud9"
weight : 11
---

#### redshift-workshop-cloud9

Cloud9是AWS提供的云原生IDE开发环境，本实验中创建Cloud9是为了使用它和MySQL及KDS交互生成实验所需数据。

::alert[开始本步骤前，请确保已经完成了VPC创建]{type=error}

1. 在AWS管理控制台，搜索Cloud9点击进入 ![cloud9-search](/static/imgs/redshift/cloud9-search.png)

2. 点击`Create Environment`进入创建页面 ![cloud9-create](/static/imgs/redshift/cloud9-create.png)

3. 在Name输入框中填写`redshift-workshop-cloud9`,在Description输入框中同样填写`redshift-workshop-cloud9`,之后点击`Next Step` ![cloud9-create-page-01](/static/imgs/redshift/cloud9-create-page-01.png)

4. 其他选择保持默认配置，滑动页面到下方打开Network Settings, 在Network(VPC)中选择`redshift-workshop-vpc`，在Subnet中选择`redshift-workshop-subnet-public1-xxx`，之后点击Next step。![cloud9-create-page-02](/static/imgs/redshift/cloud9-create-page-02.png)

5. 点击`Create Environment`完成创建即可，创建需要等待几分钟时间。![cloud9-create-finish](/static/imgs/redshift/cloud9-create-finish.png)

6. 创建完毕后会进入Cloud9的欢迎页面，后续实验中我们会使用Cloud9创建我们需要的数据集 ![cloud9-welcome](/static/imgs/redshift/cloud9-welcome.png)