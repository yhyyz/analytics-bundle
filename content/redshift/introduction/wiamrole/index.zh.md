---
title : "创建IAM Role"
weight : 11
---

#### redshift-workshop-iam

::alert[为workshop创建一个名为`redshift-workshop-admin-role`的IAM Role, 并赋予其Admin权限。请注意AWS权限的最佳实践是最小化原则，这里为了给大家快速上手，并未遵从最小化原则。请在生产环境中缩小权限范围遵从最小化权限原则。]{type=error}

1. 在AWS管理控制台中搜索`iam`，点击进入IAM管理界面 ![iam-search](/static/imgs/redshift/iam-search.png)

2. 点击左侧列表的Roles，进入Role的创建页面 ![iam-create-role](/static/imgs/redshift/iam-create-role.png)

3. 点击右上角Create role按钮 ![iam-create-role-page-01](/static/imgs/redshift/iam-create-role-page-01.png)

4. 在Use case中选择EC2, 其它保持默认，点击Next ![iam-create-role-page-02](/static/imgs/redshift/iam-create-role-page-02.png)

5. 在搜索框中输入`AdministratorAccess`,之后按Enter建，在搜索列表中勾选`AdministratorAccess`,点击Next ![iam-create-role-page-03](/static/imgs/redshift/iam-create-role-page-03.png) ![iam-create-role-page-04](/static/imgs/redshift/iam-create-role-page-04.png)

6. 在Role Name输入框中输入的名称`redshift-workshop-admin-role`，其它选择保持默认，滑动到页面最下方点击创建即可 ![iam-create-role-page-05](/static/imgs/redshift/iam-create-role-page-05.png) ![iam-create-role-page-06](/static/imgs/redshift/iam-create-role-page-06.png)

7. 为创建好的Role添加Trust relationships。 点击在IAM左侧列表Roles ,搜索`redshift-workshop-admin-role`, 点击这个Role Name ![iam-update-trust](/static/imgs/redshift/iam-update-trust.png)

8. 选择Trust relationships, 点击Edit trust policy  ![iam-update-trust-page-01](/static/imgs/redshift/iam-update-trust-page-01.png)

9. 复制下方的JSON内容覆盖到文本框，之后滑动到页面下方点击Update policy。![iam-update-trust-page-02](/static/imgs/redshift/iam-update-trust-page-02.png) ![iam-update-trust-page-03](/static/imgs/redshift/iam-update-trust-page-03.png)
:::code{showCopyAction=true showLineNumbers=true language=json}
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": "ec2.amazonaws.com"
            },
            "Action": "sts:AssumeRole"
        },
        {
            "Effect": "Allow",
            "Principal": {
                "Service": "redshift.amazonaws.com"
            },
            "Action": "sts:AssumeRole"
        },
        {
            "Effect": "Allow",
            "Principal": {
                "Service": "glue.amazonaws.com"
            },
            "Action": "sts:AssumeRole"
        },
        {
            "Effect": "Allow",
            "Principal": {
                "Service": "sagemaker.amazonaws.com"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
:::