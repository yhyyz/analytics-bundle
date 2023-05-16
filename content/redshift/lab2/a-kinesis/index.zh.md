---
title : "数据发送至KDS"
weight : 11
---

::alert[本小节内容将完成KDS创建和数据实时发送到KDS]{type=info}

1. 在AWS管理控制台搜索Kinesis, 进入到Kinsis管理页面，选择Kinesis Data Streams，点击Create data stream。![kds-create](/static/imgs/redshift/lab2/kds-create.png)

2. 输入Data stream的名称`workshop-kds`, Capacity mode选择On-demand会根据我们的流量自动扩缩KDS的分片。![kds-create-page-01](/static/imgs/redshift/lab2/kds-create-page-01.png)

3. 其它选项保持默认，滑动到页面下方点击创建即可。![kds-create-page-02](/static/imgs/redshift/lab2/kds-create-page-02.png)

4. 创建过程需要等几十秒，在控制台查看创建的Stream。![kds-create-finish](/static/imgs/redshift/lab2/kds-create-finish.png)

5. 在Cloud9控制台，执行如下命令发送数据，可以使用Ctrl+C结束。
:::code{showCopyAction=true showLineNumbers=true language=shell}
for i in {1..100000}
do
ctime=`date '+%Y-%m-%d %H:%M:%S'`
action_data='{"uid":'$i',"event_type":"click'$RANDOM'","element_name":"buy_button'$RANDOM'", "model":"iPhone","ctime":"'$ctime'"}'
echo "$action_data"
aws kinesis put-record --stream-name workshop-kds --data "$action_data" --partition-key $i
sleep 1
done
:::
![kds-send-data](/static/imgs/redshift/lab2/kds-send-data.png)
