---
title : "数据集说明"
weight : 11
---
- [RDS MySQL](#rds-mysql)
- [KDS Data](#kds-data)

#### RDS MySQL

![dataset-mysql](/static/imgs/redshift/dataset-mysql.png)

#### KDS Data

以用户在移动端的某个行为数据为例

:::code{showCopyAction=true showLineNumbers=true language=json}
{
  "uid": 1, #  用户ID
  "event_type": "click", # 事件类型 click,view
  "element_name": "buy_button", # 页面元素名称
  "model": "iPhone", # 手机类型 iPhone,Android
  "ctime": "2022-06-17 11:00:00" # 服务器收到该日志的时间
}
:::
