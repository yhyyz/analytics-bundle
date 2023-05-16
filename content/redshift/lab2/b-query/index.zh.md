---
title : "Streaming Ingesting"
weight : 11
---

::alert[本小节内容将完成Streaming Ingesting实时查询KDS中数据]{type=info}


1. 在Redshift中为Kinesis创建External Schema。在Redshift查询编辑器页面执行如下SQL即可。SQL中需要IAM ROLE的ARN我们依然使用Lab1中为Redshift创建的`redshift-federated-admin-role`。 因为我们对该Role赋予了Admin权限，因此是可以访问Kinesis的, 获取Role RAN的方法Lab1中已经说明。
:::code{showCopyAction=true showLineNumbers=true language=sql}
DROP SCHEMA if exists kinesis_streming;

CREATE EXTERNAL SCHEMA kinesis_streming
FROM KINESIS
IAM_ROLE 'arn:aws:iam::xxxxxx:role/redshift-federated-admin-role';
:::


2. 创建Schema之后，我们可以执行下方SQL创建物化视图，该视图中定义了从哪个Stream Name中访问数据。Redshift从KDS中读取数据，会有如下几个字段信息。我们根据这些字段可以灵活创建物化视图, 如下SQL：从名字为`workshop-kds`的KDS中消费数据，通过from_varbyte函数将Kinesis中我们发送的数据`[本例中我们发送到Kinesis的数据为Json类型]`转成字符串类型，然后通过JSON_PARSE函数将数据解析为Redshift Super类型`[Super类型是Redshift专门为处理半结构化类型数据而设计的]` 。注意where调整中使用了is_valid_json来判断数据是不是合法的Json，如果不是这条数据将会被丢弃。如果不加这个条件判断，遇到不合法的Json类型加载数据时会抛出异常。

| Column                      | 含义                                                         |
| --------------------------- | ------------------------------------------------------------ |
| ApproximateArrivalTimestamp | 数据被Kinesis接收的时间                                      |
| PartitionKey                | 数据Put到Kinesis时的分区键                                   |
| SequenceNumber              | 记录在分区内的唯一标识，但分区内有序递增。不保证多分区全局有序       |
| Data                        | 发送到Kinesis的数据                                          |

:::code{showCopyAction=true showLineNumbers=true language=sql}
CREATE MATERIALIZED VIEW action_log AS
SELECT ApproximateArrivalTimestamp,
       JSON_PARSE(from_varbyte(Data, 'utf-8')) as Data
FROM kinesis_streming.workshop-kds
WHERE is_utf8(Data) AND is_valid_json(from_varbyte(Data, 'utf-8'));

:::

![streaming-create-sql](/static/imgs/redshift/lab2/streaming-create-sql.png)

3. 如果发送数据到KDA的命令已经停止，请重新执行发送数据。 在Reshift中执行如下SQL刷新物化视图，就可以看到数据了。需要注意的是当前Streaming的物化视图还不支持自动刷新，可以使用Redshift的Scheduler来刷新视图。
:::code{showCopyAction=true showLineNumbers=true language=sql}
REFRESH MATERIALIZED VIEW action_log;

select 
Data.uid::int,
Data.event_type::varchar,
Data.model::varchar,
Data.ctime::varchar
from action_log limit 50;  

:::
![streaming-query-result](/static/imgs/redshift/lab2/streaming-query-result.png)
![streaming-query-result-01](/static/imgs/redshift/lab2/streaming-query-result-01.png)



