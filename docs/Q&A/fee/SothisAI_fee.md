# 曙光平台计费规则
## 计费模式
曙光平台采取预计费模式，即先计费后扣费。
每位用户预先拥有10000（1W）默认核时数，在用户申请容器/作业时，根据用户申请容器/作业所需小时数（<span style="color:red">用户申请的CPU和GPU的**核数**×GPU/CPU每一核的**小时费用**×用户申请的容器/作业的**时间**</span>）对用户剩余小时数进行预先扣除，如果用户剩余小时数小于用户申请的小时数，则容器/作业创建失败。
## 计费价格

| 名称    | 价格（费用/核*时） |
| :-------------- | :-----------:|
| CPU      | 1       |
| GPU:T4   | 1        |
| GPU:Z100|1|

## 由费用不足所引起的问题

1.容器/作业成功启动后被强制关闭，或者弹出如下error提示

![报错提醒](./fee_images/free_error.png)



