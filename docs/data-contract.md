# 业务资料协议

服务规则仅为合成约定，非真实快递承诺。授权consented=false表示无法证明客户同意，不能据承运方引用而改变客户时钟。事实occurred_at与known_at分别表示发生与被知道。handoff_out和handoff_in允许时间不齐或缺证，不能预先视为责任已经明确。补偿支付回执仅证明客户收款，carrier_resolved另说明承担是否结清。

所有数字、身份和业务来件均为合成。记录的稳定编号只在对应表内唯一，跨表关联以明确字段为准。布尔值为原来源明确回报，空字符串引用表示没有已知对应。金额如有均用整数分，比例为0至1，其他单位在字段中注明。

## service_rules

合成送达服务约定。

| 字段 | 格式 | 含义 |
| --- | --- | --- |
| id | string | 记录的稳定编号 |
| route | string | 接力线路 |
| region | string | 适用地址区 |
| cutoff_hour | number | 当地截单小时 |
| promise_hours | number | 承诺的最长小时 |
| minimum_on_time | number | 允许展示的准时可靠性下限 |
| maximum_daily_guarantees | number | 此接力范围每天的保障量 |
| customer_compensation_minor | number | 已承诺失败时客户应收补偿分 |
| carrier_share | array | 按最终有依据的责任适用的各方分担份额 |

## orders

已被客户接受的原报价与保障。

| 字段 | 格式 | 含义 |
| --- | --- | --- |
| id | string | 记录的稳定编号 |
| route | string | 全程线路 |
| region | string | 目的地址区 |
| goods_class | string | 可比货型 |
| accepted_at | string | 客户接受报价时点 |
| promised_by | string | 原承诺最晚送达 |
| known_at | string | 订单获知时间 |
| rule_id | string | 接受的服务约定 |
| fee_minor | number | 费用分 |
| requested_window | array | 客户可接受到达窗口 |

引用关系：rule_id 对应 service_rules.id。

## events

接力和投递事件原件。

| 字段 | 格式 | 含义 |
| --- | --- | --- |
| id | string | 记录的稳定编号 |
| order_id | string | 订单 |
| kind | string | picked、handoff_out、handoff_in、attempt、signed或carrier_cancel；取消不抹除此前已接受的承诺 |
| occurred_at | string | 事实发生时间 |
| known_at | string | 资料获知时间 |
| carrier | string | 登记的承运方 |
| evidence | string | scanned有原始凭据，missing为缺失待核 |
| authorization_id | string | 声称引用的客户授权；空串无引用 |
| replaces | string | 明确更正的原事件；空串为初报 |

引用关系：order_id 对应 orders.id。

## reschedule_consents

客户改约授权原件。

| 字段 | 格式 | 含义 |
| --- | --- | --- |
| id | string | 记录的稳定编号 |
| order_id | string | 订单 |
| consented | boolean | 客户是否确实同意 |
| new_due | string | 同意后的最晚送达 |
| consented_at | string | 客户表示时点 |
| known_at | string | 授权收到时间 |

引用关系：order_id 对应 orders.id。

## capacity_notices

未来承接能力变化，不取消旧订单。

| 字段 | 格式 | 含义 |
| --- | --- | --- |
| id | string | 记录的稳定编号 |
| route | string | 线路 |
| date | string | 适用送达日期 |
| capacity | number | 该日接力保障上限 |
| known_at | string | 收到通知时间 |

## compensation_receipts

客户补偿支付状态。

| 字段 | 格式 | 含义 |
| --- | --- | --- |
| id | string | 记录的稳定编号 |
| order_id | string | 订单 |
| amount_minor | number | 支付额分 |
| at | string | 支付时点 |
| status | string | paid或pending |
| carrier_resolved | boolean | 承运方承担是否已经确认 |

引用关系：order_id 对应 orders.id。
