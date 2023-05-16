---
title : "CDC实时同步"
weight : 11
---
::alert[本小节内容将完成MySQL 数据通过DMS CDC同步到Redshift]{type=info}

1. 在DMS管理控制台左侧列表选择Data migration task，点击右上角创建按钮。![dms-cdc-create](/static/imgs/redshift/dms-cdc-create.png)

2. 输入Task identifier名字`mysql-redshift-cdc`, 复制实例选择mysql-cdc-instance, Source endpoint选择`workshop-mysql`, Target endpoint 选择`redshift-endpoint`, Migration type 选择先批量加载历史之后转实时同步。![dms-cdc-create-page-01](/static/imgs/redshift/dms-cdc-create-page-01.png)

3. 滑动到Task settings的页面，勾选Create recovery table on target DB此选项会将checkpoint信息持续写入到Redshift awsdms_txn_state表。Target table的准备模式选择`Do nothing`表示表如果已经存在不会删除也不会清空而是直接写入。勾选Enable CloudWatch日志，将DMS作业运行日志发送到CloudWatch. 日志级别可以根据情况选择，比如调试某个作业时可以选择Debug或者info,调试完毕生产上线时可以选择ERROR ![dms-cdc-create-page-02](/static/imgs/redshift/dms-cdc-create-page-02.png)  ![dms-cdc-create-page-log](/static/imgs/redshift/dms-cdc-create-page-log.png)

4. 滑动到Table mappings配置项在其中的Selection rules添加rule，输入要同步的MySQL数据库`test_db`,同步的表选择`%`,表示通配符匹配所有表。![dms-cdc-create-page-03](/static/imgs/redshift/dms-cdc-create-page-03.png)

5. DMS支持Transformation, 这里配置将MySQL的`test_db.user_order`表中的`oid`字段重命名为`order_id`字段。其他参数保持默认，到页面下方点击创建即可。 ![dms-cdc-create-page-04](/static/imgs/redshift/dms-cdc-create-page-04.png)

6. 创建完毕后在Task页面, 选择mysql-redshift-cdc，选择Action选项中的Restart/Resume开始作业。![dms-cdc-create-view](/static/imgs/redshift/dms-cdc-create-view.png)

7. 可以在DMS TASk控制台查看任务的统计信息，比如同步的表，加载的数据条数等。![dms-cdc-create-view-01](/static/imgs/redshift/dms-cdc-create-view-01.png) ![dms-cdc-create-view-02](/static/imgs/redshift/dms-cdc-create-view-02.png)

8. 在Cloud9通过MySQL客户端执行如下SQL，插入`user`表一条新数据，更新一条历史数据，在Redshift查看实时同步的数据结果。
:::code{showCopyAction=true showLineNumbers=true language=sql}
insert into user(name,device_model,email,phone,aid) values
('customer-03','dm-03','abc03@email.com','19988xxxxx','a10002');

update user set name='new-cusomter-02' where id =2;
:::
![dms-cdc-udpate](/static/imgs/redshift/dms-cdc-update.png)

9. 在Redshift中查询数据，可以看到test_db.user表中已经新插入的`customer-03`已经同步到Redshift，`customer-02`数据已经修改为`new-customer-02`
![dms-cdc-udpate-02](/static/imgs/redshift/dms-cdc-update-02.png)
![dms-cdc-udpate-03](/static/imgs/redshift/dms-cdc-update-03.png)
