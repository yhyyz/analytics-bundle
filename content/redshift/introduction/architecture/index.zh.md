---
title : "实验架构说明"
weight : 11
---
- [架构图](#架构图)
- [流程说明](#流程说明)
  - [Lab1](#lab1)
  - [Lab2](#lab2)
  - [Lab3](#lab3)
  - [Lab4](#lab4)
  - [Lab5](#lab5)

#### 架构图

![redshift-workshop](/static/imgs/redshift/redshift-workshop.png)

#### 流程说明

实验如上图所示分为5个部分，分别用不同的编号标识，下面会按标号解释相关的步骤

##### Lab1

- 1.1 - 1.2 使用DMS通过CDC方式实时同步RDS[`本实验以MySQL为例，DMS也支持PostgreSQL,Oracle,Microsoft SQL Server`]数据到Redshift，DMS会自动处理数据在Redshift的更新逻辑，同时支持Schema变更，但有限制[点击查看](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Target.Redshift.html#CHAP_Target.Redshift.Limitations)。DMS将数据同步到Redshift过程中，会先将数据写入到S3之后Copy数据到Redshift，更新会做批的Update操作，此过程对用户是透明的。
  
- 1.3 Redshift支持Federated Query,可以直接实时查询[`Amazon RDS for PostgreSQL, Amazon Aurora PostgreSQL-Compatible Edition, Amazon RDS for MySQL, and Amazon Aurora MySQL-Compatible Edition`]中的数据，同时可以和Redshift中的其他数据做Join，快速获得数据见解。这里会使用Federated Query查询MySQL数据。

##### Lab2

- Redshift Streaming Ingesting功能可以直接对接消息系统，将数据以秒级延迟摄入到Redshift中，当前Redshift支持从KDS中摄入数据[MSK的支持已经在计划中]。有了此功能Kinesis数据实时摄入Redshift只需要以SQL方式创建一个KDS的物化视图表，无需借助任何其他工具，这降低实时数仓架构的复杂度，同时也节省成本。本实验中会使用SDK向KDS发送数据，通过Streaming Ingesting将数据实时摄入到Redshift进行查询。

##### Lab3

- 3.1 - 3.2 如果数据在S3上，需要对数据做ETL后再将数据加载到Redshift。可以使用`Glue Studio`以可视化的方式对数据做ETL之后直接Sink到Redshift。Glue Studio由可视化自动生成的代码会遵循写Redshift的最佳实践，先将数据落到S3之后通过Copy方式将数据加载到Redshift以保证性能。值得一提的是Glue Studio自动生成的代码也支持手动修改代码逻辑[`注意手动修改后不能再回到可视化界面`]。Glue在编写作业时，可以使用`Dynamic Frame`，也可以使用和开源Spark完全相同的`DataFrame`，两者可以相互转换。

- 3.3 Redshift Spectrum可以直接查询S3中数据，只需要在Redshift中创建外部表指向S3中数据即，无需将数据加载到Redshift自身存储即可查询数据。Redshift Spectrum使用Glue Catalog管理元数据，已经在Glue中存在的表(`数据在S3`)在Redshift中也是可以直接查询的。Copy命令可以将S3数据加载到Redshift中，Copy加载数据是非常高效的，它并行执行其并行度和集群的Slice个数相关，Copy命令对应Redshift而言就是一个常规SQL命令。对于半结构化数据Redshift提供了专有的Super类型来支持，查询性能很好，多层级嵌套结构的查询语法简单易用。

##### Lab4

- Redshift提供Data Sharing功能，可以在Redshift集群之间实时共享数据，可以跨账号，跨区域实时安全的共享数据。本实验中将数据摄入和ETL放到生产者集群中数据处理之后的结果数据共享给消费者集群，做读写分离。如果生产者集群每天只在固定时间做ETL，可以做完ETL后将集群暂停来节省成本，生产者集群暂停对消费者集群无影响。使用QuickSight做数据可视化。

##### Lab5

- Redshift ML可以通过SQL的方式直接创建机器学习的Model，Redshift ML 通过SageMaker Autopilot可以帮助我们自动选择最优的模型，根据我们输入的数据集自动训练调优模型。我们也可以指定特定的算法模型，比如XGBoost。本实验中我们会使用IRIS数据集来训练一个分类模型。Redshift ML帮助我们快速使用SQL运行一个机器学习模型，快速获得数据见解，降低学习门槛。
