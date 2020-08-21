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
有关于回款以及部分回款我们可以根据数据库进行查看。

1.判断是否回款？


我觉得可以通过一个接口获取这两个：
总接口：
/api/v1/erp/crm-order-requests/orderCCv2?orderType=1&web=2

出库情况接口：
/api/v1/erp/inventoryorder/orderleft/10191549?containApproved=1  
https://www.erplus.co/api/v1/crm/inventoryorder/orderleft/10153754?containApproved=1
通过调用此接口，然后通过判断
{
    "respCode": "000",
    "items": [
        {
            "id": 10228466,
            "contactId": 11292,
            "price": 1000,
            "srcPrice": 1000,
            "num": 10,
            "total": 10000,
            "orderId": 10191549,
            "productId": 10033580,
            "srcTotal": 10000,
            "subProductWarehouseId": 10030918
        }
    ]
}
通过判断此中num的值来判断是否出库。


查看回款：
/api/v1/erp/crm-order-requests/10191549/payment
[
    {
        "companyName": "成交客户",
        "id": 10079838,
        "orderNO": "M2020070800021",
        "crmCustomer": {
            "id": 11660444,
            "name": "成交客户",
            "address": "123",
            "realAddress": "广东省深圳市宝安区航城大道",
            "mapLocation": {
                "lat": 22.60340975361765,
                "log": 113.84410515476927
            },
            "mapLocations": null,
            "status": 3,
            "source": null,
            "notes": "123",
            "crtAt": "2020-02-05 16:07",
            "updAt": "2020-04-02 20:19",
            "crtBy": null,
            "crtById": 90041743,
            "inviteContact": null,
            "inviteContactId": null,
            "contacts": null,
            "salesContactId": 90041743,
            "salesContact": null,
            "isRecommended": false,
            "type": null,
            "profession": null,
            "commentAt": "2020-04-08 15:13",
            "tag": null,
            "professionDomain": null,
            "visitNum": 0,
            "allowVisitTimes": "0",
            "isVisitOk": "0",
            "cost": null,
            "visitNumType": 0,
            "publicTypeId": null,
            "publicType": null,
            "cooperationTime": "2020-02-22 16:43",
            "crmCode": "KH20-0200010",
            "visitStoreTime": null,
            "customerType": 1,
            "parentId": null,
            "parent": null,
            "customerAmount": null,
            "toMyCustomerTime": "2020-02-24 17:08",
            "activityAt": "2020-04-02 20:20",
            "image": null,
            "cooperationContactId": 90041743,
            "integrity": 100,
            "clueCrmContactId": null,
            "estimatedSales": 0,
            "estimatedDate": null,
            "stageRatio": "0"
        },
        "topic": "成交客户",
        "amount": 5000,
        "notes": "",
        "payType": "支付宝",
        "payDate": "2020-06-04",
        "crtAt": "2020-07-08 17:02",
        "crtBy": {
            "id": 11292,
            "phone": "17444440306",
            "name": "YiBO",
            "position": "",
            "userId": 8514,
            "companyId": 1829,
            "company": {
                "name": "【测试】西木由",
                "crtAt": "2016-10-06T10:31:41+08:00",
                "hash": "814ca20b63355918434741bcee8b8c47",
                "isDatastatisticBeta": 0,
                "isApproveBeta": 0,
                "isCrmBeta": 1,
                "isNoticeBeta": 0,
                "isBorrowBeta": 0,
                "isKpiBeta": 1,
                "isKpiUse": 1,
                "state": 1,
                "contactsUpdateAt": "2020-08-20 11:30:54",
                "isContactQuit": 1,
                "companyVitality": 1000,
                "iconSetting": "[{badgeType=1, bgImgType=3, headTextColor=0, bodyTextColor=0, bigOfHeadTitle='ERPlus', bigOfBodyTitle='副总', smallOfBodyTitle='副'}, {badgeType=3, bgImgType=3, headTextColor=0, bodyTextColor=0, bigOfHeadTitle='ERPlus', bigOfBodyTitle='副主管', smallOfBodyTitle='副'}, {badgeType=2, bgImgType=0, headTextColor=1, bodyTextColor=1, bigOfHeadTitle='ERPlus', bigOfBodyTitle='主管', smallOfBodyTitle='主'}]",
                "lite": 0,
                "contactNum": null,
                "salaryDay": "3"
            },
            "departmentId": 21528,
            "department": {
                "id": 21528,
                "name": "IM Test TEam",
                "path": [
                    "21528"
                ],
                "parentId": null,
                "totalPeople": 6,
                "managers": [
                    90014176
                ],
                "order": 7,
                "isBusinessDepartment": null
            },
            "isAdmin": false,
            "isDepManager": false,
            "crtAt": "2017-04-18T20:02:57+08:00",
            "imageName": "/41/1879/23929/e125/4be7a42c4737370678e9.jpg",
            "deletedAt": null,
            "isContactManager": false,
            "isSkipPhone": true,
            "parentId": 9793,
            "isIdirector": null,
            "isTaskApprove": 1,
            "isAudioApprove": 1,
            "selfTaskOrder": 1,
            "patiTaskOrder": 1,
            "isNeedKpi": 1,
            "isAndroid": 1,
            "leftDay": 8,
            "systemVersion": "4.7.1.27",
            "quitReason": null,
            "quitCode": 0,
            "isAssistantManager": 0,
            "kpiRatio": 0,
            "mainContactId": 11292,
            "state": 1,
            "isAdminIdentity": 0,
            "userCurrentTime": "2020-08-18T16:22:41+08:00",
            "china_current_time": "2020-08-20T10:16:57+08:00",
            "userTimeZone": "Asia/Shanghai",
            "appSort": "0,19,18,20,14,10,9,12,5,16,8,3,2,7,1,15,6,17",
            "appSortV2": null,
            "isCurrentContact": null,
            "hasCrmPermission": null,
            "sort": 0,
            "language": "zh",
            "wechatAlarm": null,
            "callAlarm": null,
            "visitAlarm": null,
            "visitstoreAlarm": null,
            "commentAlarm": null,
            "iconSetting": null,
            "appItem": null,
            "companyInfoId": 10010204,
            "companyInfo": {
                "id": 10010204,
                "companyId": 1829,
                "crtAt": "2017-04-18T20:02:57+08:00",
                "infoId": 10010981,
                "info": {
                    "id": 10010981,
                    "phone": "17444440306",
                    "name": "YiBO",
                    "imageName": "/41/1879/23929/e125/4be7a42c4737370678e9.jpg",
                    "crtAt": "2017-04-18T20:02:57+08:00"
                },
                "passwordKey": null
            },
            "isCrmCommentParticipant": 0,
            "isApproveTaskkrParticipant": 1,
            "isAddKpi": 1
        },
        "crtById": 11292,
        "curContactStep": null,
        "curContactStepId": null,
        "curFlowStep": {
            "id": 10095202,
            "contact": null,
            "contactId": 11292,
            "ordering": 1,
            "isApproved": true,
            "isApprovedAt": "2020-07-08 17:02",
            "comment": null,
            "orderId": "10079838",
            "companyId": 1829,
            "flowType": 0,
            "isAlternative": 1
        },
        "isApproved": true,
        "isApprovedAt": "2020-07-08 17:02:42",
        "contactFlowIds": "11292",
        "passedContacts": [
            11292
        ],
        "isCanceled": null,
        "requestFlow": [
            {
                "id": 10095202,
                "contact": null,
                "contactId": 11292,
                "ordering": 1,
                "isApproved": true,
                "isApprovedAt": "2020-07-08 17:02",
                "comment": null,
                "orderId": "10079838",
                "companyId": 1829,
                "flowType": 0,
                "isAlternative": 1
            }
        ],
        "orderId": null,
        "order": null,
        "files": "",
        "vouchers": null,
        "unread": null,
        "atUnread": null,
        "state": 1,
        "cancelTime": null,
        "orderUnRead": null,
        "sort": null,
        "applicantId": 11292,
        "applicant": null,
        "updateTime": "2020-07-08 17:02",
        "operatorId": null,
        "orderType": 2,
        "paymentType": 0,
        "advancesReceived": 0,
        "advancesCut": 0,
        "writeOffReceivables": 5000,
        "paymentMoneyJson": [
            {
                "name": "支付宝",
                "amount": "5000"
            }
        ],
        "autoApprove": null,
        "paymentToOrders": [
            {
                "id": 82902,
                "companyId": 1829,
                "paymentId": 10079838,
                "orderId": "10191549",
                "order": {
                    "companyName": "",
                    "id": 10191549,
                    "crmCustomer": null,
                    "crmCustomerId": 11660444,
                    "chance": null,
                    "chanceId": 12054936,
                    "topic": "成交客户",
                    "amount": 10000,
                    "notes": "",
                    "contractTitle": null,
                    "contractNo": null,
                    "contractPaymentTime": null,
                    "contractPaymentMethod": null,
                    "contractNote": null,
                    "crtAt": "2020-07-08 17:02",
                    "startAt": null,
                    "endAt": null,
                    "clue": null,
                    "clueId": null,
                    "invite": null,
                    "inviteId": null,
                    "contactInfoAdd": null,
                    "contactInfoAddId": null,
                    "crtBy": null,
                    "crtById": 90041743,
                    "product": null,
                    "curContactStep": null,
                    "curContactStepId": null,
                    "curFlowStep": {
                        "id": 10260034,
                        "contact": null,
                        "contactId": 11292,
                        "ordering": 1,
                        "isApproved": true,
                        "isApprovedAt": "2020-07-08 17:02",
                        "comment": null,
                        "orderId": "10191549",
                        "companyId": 1829,
                        "flowType": 0,
                        "isAlternative": 1
                    },
                    "isApproved": true,
                    "isApprovedAt": "2020-07-08 17:02:39",
                    "contactFlowIds": "11292",
                    "passedContacts": [
                        11292
                    ],
                    "isCanceled": null,
                    "requestFlow": [
                        {
                            "id": 10260034,
                            "contact": null,
                            "contactId": 11292,
                            "ordering": 1,
                            "isApproved": true,
                            "isApprovedAt": "2020-07-08 17:02",
                            "comment": null,
                            "orderId": "10191549",
                            "companyId": 1829,
                            "flowType": 0,
                            "isAlternative": 1
                        }
                    ],
                    "unread": null,
                    "atUnread": null,
                    "orderId": "hhhhhhhh",
                    "discount": null,
                    "discountType": null,
                    "parent": null,
                    "parentId": null,
                    "state": 1,
                    "isFirst": 1,
                    "paymentAmount": 5000,
                    "orderUnRead": null,
                    "outInventoryContact": null,
                    "outInventoryContactId": null,
                    "receiverCrmContactName": null,
                    "receiverAddress": null,
                    "receiverPhone": null,
                    "outInventoryTime": null,
                    "outWarningTime": null,
                    "updateTime": "2020-07-08 17:02",
                    "outInventoryFlag": 0,
                    "applicant": null,
                    "applicantId": 11292,
                    "warehouse": {
                        "id": 10000001,
                        "crtAt": "2018-09-28 20:21",
                        "updateAt": null,
                        "crtById": 5215,
                        "name": "默认仓库",
                        "state": 1,
                        "checkAt": null,
                        "address": null,
                        "realAddress": "广东省深圳市宝安区福永街道兴围锦灏大厦",
                        "mapLocation": {
                            "lat": 22.694777,
                            "lng": 113.831361
                        },
                        "code": "Ck001",
                        "image": "",
                        "notes": "测试阿罗裤",
                        "admin": null,
                        "isDefault": 1
                    },
                    "warehouseId": 10000001,
                    "sort": null,
                    "productAlias": "[{\"id\":10033580,\"name\":\"清晨蛋糕\",\"alias\":\"\"}]",
                    "promotionInfo": null,
                    "discountAmount": null,
                    "operatorId": null,
                    "allReturn": null,
                    "freezeProductNum": null,
                    "orderType": 1,
                    "clueRatio": null,
                    "inviteRatio": null,
                    "infoAddRatio": null,
                    "cooperationRatio": "100",
                    "salesTypeId": 7754,
                    "salesTypeName": null,
                    "autoApprove": null,
                    "signTime": "2020-07-08 17:02",
                    "files": null,
                    "cc": "5215,11292",
                    "commentUnRead": null,
                    "commentAtUnRead": null
                },
                "amount": 5000,
                "orderAmount": 10000,
                "amountReceived": null,
                "state": 1,
                "crtAt": "2020-07-08 17:02",
                "notes": "",
                "files": ""
            }
        ],
        "orderAmountInfo": null,
        "commentUnRead": null,
        "commentAtUnRead": null,
        "cc": null
    }
]









sys_crm_order_participants  抄送人


/api/v1/erp/inventoryorder/cclist
每次和代码前一定要更新。


获取所有抄送接口中的url：
crm_order_participant.service
crm_order.service
crm_common.service
crm_order_participant.service





paymentType中2表示未回款
payment




有关于这个里面所有的东西。。
/uploadFileAppendCustomerContact


