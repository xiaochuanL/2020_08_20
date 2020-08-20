# java中关于tars使用
1. 在service.tars中写入需要生成的tars接口。
2. 通过tars插件中tars2java命令生成需要生成的文件。

添加回款状态、添加出库状态：
回款状态之间是与的关系，出库状态之间是与的关系。但是都是必填项。



这里需要查看他封装的几个值？
SELECT id,name,state,parent_id,hide_items,is_usable,crt_at FROM `mp-inventory`.`sys_crm_sales_type` WHERE company_id = ? ORDER BY ordering

可以获取销售类型：
Map封装：
 salesTypeMap.put(salesType.getId(), salesType);

salesOrderMap:（salesTypeId,）
订单有一个salesTypeId，如果相同的salesTypeId那么订单放在一起。




salesTypeMap：(salesTypeId,)
有很多张sheet,一个sheet就对应一个salestypeId,一个salesType就相当于封装了sheet的所有信息。
 1. 一个订单有多笔回款。
 2. 一个订单出库状态。
订单中有出库就会将其标志为1，那么全部出库将如何标志呢？

订单：西柚传媒有限公司。
出库单不一定与订单有直接联系。
出库单可以没有订单。

全部、未回款、部分回款、全部回款。
全部、未出库、部分出库、全部出库。
订单会出现待出库和已出库。

1.如果查询订单时，out_inventory_flag=1   如果需要查询库存表如果待出库大于0的话就是部分出库，否者的话全部出库。
2.如果查询订单时，out_inventory_flag=0   情况下就是未出库。

如何去理解这一切？











sys_crm_order_participants  抄送人


/api/v1/erp/inventoryorder/cclist
每次和代码前一定要更新。


获取所有抄送接口中的url：
crm_order_participant.service
crm_order.service
crm_common.service
crm_order_participant.service


