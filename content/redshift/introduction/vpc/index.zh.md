---
title : "创建VPC"
weight : 11
---

#### redshift-workshop-vpc

1. 首先我们创建一个VPC，本次实验的相关组件我们都部署到该VPC环境中。在你登陆的AWS Console中搜索`VPC`,进入到VPC管理页面![vpc-search](/static/imgs/redshift/vpc-search.png)

2. 点击`Create VPC`按钮 ![create-vpc-button](/static/imgs/redshift/create-vpc-button.png)
  
3. 进入到VPC创建页面，选择`VPC and more`选项，在`Auto-generate`复选框下面输入VPC的名称`redshift-workshop` ![create-vpc-page-01](/static/imgs/redshift/create-vpc-page-01.png)

4. 其他选择保持默认，滑动到页面下方在NAT gateways选择`1 per AZ`选项，为每个AZ创建一个NAT网关，其他选项保持默认点击`Create VPC`即可完成VPC和其相关联的资源创建，创建过程需要1~3分钟左右。![create-vpc-finish](/static/imgs/redshift/create-vpc-finish.png) ![create-vpc-view](/static/imgs/redshift/create-vpc-view.png)