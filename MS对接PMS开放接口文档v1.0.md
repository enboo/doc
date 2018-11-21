# WMS对接PMS开放接口文档v1.0  
### 注意：
由于有些规范目前还没有确定，该版本字段不代表最终版本，比如PMS作为服务端的接口、仓库编码的统一、系统单据编码规范、扩展字段的增加等等，文档会在评审沟通和开发过程中不断修正。  
接口交互状态码已有统一规范，详细参考文档底部状态码说明。
### 1. PMS推送新采购合同数据推送到WMS
**接口地址**： 域名/api/pms/createArriveNotice  
**请求方式**： POST  
**参数校验**： sign  
**接口描述**： 新的采购合同推送至WMS，如果WMS已存在则会返回错误信息，处于扩展规范等考虑更新请使用updateArriveNotice接口（思考买票和退票窗口）    
**Auth-date**：[add by guoqunchao 2018-11-21] 
#####   请求格式：  
```json  
{
        "sign": "81c4b6e5fd80424289f81a1f3d068e54",
        "data": [
            {
                "arriveOriginSn": "1302788053",// 采购合同编码
                "username": "xiaoli",//采购负责人员工账号
                "arriveNoticeTime": "2018-11-21 09:21:45",//下单日期
                "supplierId": "562",//供应商ID
                "supplierName": "深圳市凯雷斯顿科技有限公司",//供应商名称
                "enterpriseDominant": "1",//采购主体
                "warehouseCode": "_HUANAN_",//目的仓库编码（编码待统一确定）
                "estimateArriveDate:"2018-11-25",//预计到达仓库日期（是否放到明细去待确定）
                "shippingCode": "3382678057526,3382678057526",//快递单号（可能多次发货多个快递单号）
                "arriveRemark": "",//采购员备忘（可以为空）
                "purchaseMoney": "1620",//采购总金额（可为空，支持扩展）
                "transportFee": "10",//运费（可为空，支持扩展）
                "detail"： [{
                    "sku": "XD284002",//SKU编码
                    "cnName": "micro转USB2.0转换头 银色",//中文名称
                	"isPackagingMaterial": "0",//是否是包材
                    "purchaseSinglePrice": "54",采购单价（可为空，支持扩展）
                    "purchaseQuantity": "30",//采购数量
                    "currentCost": "53",//当时成本价
                	"unit": "个",//单位
                }]
            }
        ]
    }
```  
#####   返回格式：  
```json  
{
    "code": 000001,
    "message": "成功",
    "data": [
        {
            "apiCode":"createArriveNotice",//接口代码
            "arriveNoticeSn": "A20181121015",//WMS系统预到货登记编码
        }
    ]
}
  
```  
***  

### 2. PMS修改采购合同数据推送到WMS
**接口地址**： 域名/api/pms/updateArriveNotice  
**请求方式**： POST  
**参数校验**： sign  
**接口描述**： 采购合同已经存在情况下进行的覆盖更新，目前允许数据跟createArriveNotice接口一致  
**Auth-date**：[add by guoqunchao 2018-11-21] 
#####   请求格式：  
```json  
{
        "sign": "81c4b6e5fd80424289f81a1f3d068e54",
        "data": [
            {
                "arriveOriginSn": "1302788053",//采购合同编码
                "username": "xiaoli",//采购负责人员工账号
                "arriveNoticeTime": "2018-11-21 09:21:45",//下单日期
                "supplierId": "562",//供应商ID
                "supplierName": "深圳市凯雷斯顿科技有限公司",//供应商名称
                "enterpriseDominant": "1",//采购主体
                "warehouseCode": "_HUANAN_",//目的仓库编码（编码待统一确定）
                "estimateArriveDate:"2018-11-25",//预计到达仓库日期（是否放到明细去待确定）
                "shippingCode": "3382678057526,3382678057526",//快递单号（可能多次发货多个快递单号）
                "arriveRemark": "",//采购员备忘（可以为空）
                "purchaseMoney": "1620",//采购总金额（可为空，支持扩展）
                "transportFee": "10",//运费（可为空，支持扩展）
                "detail"： [{
                    "sku": "XD284002",//SKU编码
                    "cnName": "micro转USB2.0转换头 银色",//中文名称
                	"isPackagingMaterial": "0",//是否是包材
                    "purchaseSinglePrice": "54",采购单价（可为空，支持扩展）
                    "purchaseQuantity": "30",//采购数量
                    "currentCost": "53",//当时成本价
                	"unit": "个",//单位
                }]
            }
        ]
    }
```  
#####   返回格式：  
```json  
{
    "code": 000001,
    "message": "成功",
    "data": [
        {
            "apiCode":"updateArriveNotice",//接口代码
            "arriveNoticeSn": "A20181121015",//WMS系统预到货登记编码
        }
    ]
}
  
```  
*** 

### 3. WMS推送到货数据给PMS
**接口地址**： PMS服务端接口  
**请求方式**： POST  
**参数校验**： sign  
**接口描述**： 收货按采购合同编码或者快递单号收货，数量仅代表供应商意向，实际到货数量在接口4，同时该接口在样品归还和调拨入库也会同时推送数据到对应的外部系统    
**Auth-date**：[add by guoqunchao 2018-11-21] 
#####   请求格式：  
```json  
{
        "sign": "81c4b6e5fd80424289f81a1f3d068e54",
        "data": [
            {
                "arriveProcessSn": "P2018112400125",//到货进程编码
                "arriveOriginSn": "1302788053",//采购合同编码
                "warehouseCode": "_HUANAN_",//收货仓库编码（编码待统一确定）
                "receiveTime": "2018-11-24 09:32:26",//收货时间
                "receiveUsername": "hangmeimei",//收货人员工账号
                detail：[{
                    "sku": "XD284002",//SKU编码
                    "receiveQuantity":"30",//收货数量
                }]
            }
        ]
    }
```  
#####   返回格式：  
```json  
{
    "code": 000001,
    "message": "成功",
    "data": [
        {
            "apiCode":"",//PMS接口代码
            "arriveProcessSn":"P2018112400125",//到货进程编码
        }
    ]
}
  
```  
*** 

### 4. WMS推送对图质检数据给PMS
**接口地址**： PMS服务端接口  
**请求方式**： POST  
**参数校验**： sign  
**接口描述**： WMS推送质检数据给PMS，推送正常范畴的合格量和不合格量，多收和合法范围不良在异常情况待反馈包，需要采购员抉择后使用updateSpeacilCase接口推送给WMS  
**Auth-date**：[add by guoqunchao 2018-11-21] 
#####   请求格式：  
```json  
{
        "sign": "81c4b6e5fd80424289f81a1f3d068e54",
        "data": [
            {
                "arriveProcessSn": "P2018112400125",//到货进程编码
                "arriveOriginSn": "1302788053",//采购合同编码
                "contrastUsername": "lilei",//对图人员工账号
                "contrastTime": "2018-11-21 10:15:13",//对图时间
                "inspectUsername": "liushasha",//质检人员工账号
                "inspectSku": "XD284002",//质检SKU编码
                "supplierQuantity": "30",//供应商发货数量（意向）
                "actualArriveQuantity": "35",//实际到货数量
                "stGoodQuantity": "30",//合法范围合格量
                "stUnqualifiedQuantity": "30",合法范围不合格量
                "unqualifiedType": "",//不合格类型（跟实物描述不同/外观问题/功能问题/配件问题/尺码不符/款式不符/其他问题），所有的不合格数量的类型，编码待确定
                "unqualifiedImages": "",//不合格图片串（可以为空，支持扩展）
                "inspectMemo": "质检备忘",
                "specialCase": [
                    {
                        "specialCaseSn": "SC20181124002",//异常情况编码
                    	"specialType": "_SPECIAL_QUALIFIED_SURPLUS_",//异常类型（多收合格量、多收不合格量、合法范围质检不良品）
                    	"specialSku": "XD284002",//异常SKU
                    	"specialQuantity": "3",//异常数量（实例为多收合格量）
                    },
                    {
                        "specialCaseSn": "SC20181124003",//异常情况编码
                    	"specialType": "_SPECIAL_DEFECTIVE_SURPLUS_",//异常类型（多收合格量、多收不合格量、合法范围质检不良品）
                    	"specialSku": "XD284002",//异常SKU
                    	"specialQuantity": "2",//异常数量（实例为多收不合格量）
                    },
                ]
            }
        ]
    }
```  
#####   返回格式：  
```json  
{
    "code": 000001,
    "message": "成功",
    "data": [
        {
            "apiCode":"",//PMS接口代码
            "arriveProcessSn":"P2018112400125",//到货进程编码
        }
    ]
}
  
```  
*** 

### 5. PMS推送异常解决方法给WMS
**接口地址**： 域名/api/pms/updateSpeacilCase  
**请求方式**： POST  
**参数校验**： sign  
**接口描述**： 多收、不良品解决方案需要采购员处理抉择后给到仓库，仓库来实物处理  
**Auth-date**：[add by guoqunchao 2018-11-21] 
#####   请求格式：  
```json  
{
        "sign": "81c4b6e5fd80424289f81a1f3d068e54",
        "data": [
            {
                "specialCaseSn": "SC20181124002",//异常情况编码
                "warehouseCode": "_HUANAN_",//异常解决仓库编码（编码待统一确定）
				"specialSku": "XD284002",//异常SKU
				"specialQuantity": "3",//异常总数量（实例为多收合格量）
				"choiceUsername": "liulibing",//异常决策人员工账号
				"choiceTime": "2018-11-24 16:20:12",//异常决策时间
				"choice": [
				{
				    "threadQuantity": 1,
					"threadChoice": "_SPECIAL_QUALIFIED_SURPLUS_GIFT_",//异常处理抉择类型（实例1个为按赠品入库）
				},
				{
				    "threadQuantity": 2,
					"threadChoice": "_SPECIAL_QUALIFIED_SURPLUS_OTHER_ORDER_",//异常处理抉择类型（实例2个为补发其他采购单）
					"arriveOriginSn": "1302712053",
				}]
            }
        ]
    }
```  
#####   返回格式：  
```json  
{
    "code": 000001,
    "message": "成功",
    "data": [
        {
            "apiCode":"updateSpeacilCase",//PMS接口代码
            "specialCaseSn": "SC20181124002",//异常编码
        }
    ]
}
  
```  
*** 

### 6. WMS推送异常实物处理结果数据给PMS
**接口地址**： PMS服务端接口  
**请求方式**： POST  
**参数校验**： sign  
**接口描述**： 退回供应商会告知发货和快递等，报废/关联其他采购单需要给到实物处理响应  
**Auth-date**：[add by guoqunchao 2018-11-21] 
#####   请求格式：  
```json  
{
        "sign": "81c4b6e5fd80424289f81a1f3d068e54",
        "data": [
            {
                "specialCaseSn": "SC20181124002",//异常情况编码
                "warehouseCode": "_HUANAN_",//异常解决仓库编码（编码待统一确定）
				"overUsername": "liulibing",//异常实物处理人员工账号
				"overTime": "2018-11-24 17:56:32",//异常实物处理时间
				"overMemo": ""//如果是退回供应商，则可能会带有快递单号反馈给采购员
            }
        ]
    }
```  
#####   返回格式：  
```json  
{
    "code": 000001,
    "message": "成功",
    "data": [
        {
            "apiCode":"",//PMS接口代码
            "specialCaseSn":"SC20181124002",//异常编码
        }
    ]
}
  
```  
*** 

### 7. WMS推送库存上架数据给PMS
**接口地址**： PMS服务端接口  
**请求方式**： POST  
**参数校验**： sign  
**接口描述**： 库存上架是按采购单SKU来，也可能存在多次到货，故单个采购合同会多次推送到WMS  
**Auth-date**：[add by guoqunchao 2018-11-21] 
#####   请求格式：  
```json  
{
        "sign": "81c4b6e5fd80424289f81a1f3d068e54",
        "data": [
            {
                "arriveOriginSn": "1302788053",//采购合同编码
                "shelveUsername": "liqiang",//上架人员工账号
                "shelveTime": "2018-11-24 16:30:58",//上架时间
                "detail": [{
                    "sku": "XD284002",//上架SKU（目前是单个SKU，未来可能多个支持扩展）
                    "shelveQuantity": 30,//上架数量
                }]
            }
        ]
    }
```  
#####   返回格式：  
```json  
{
    "code": 000001,
    "message": "成功",
    "data": [
        {
            "apiCode":"",//PMS接口代码
        }
    ]
}
  
```  
*** 

### *8. 外部系统（PMS）推送样品借料请求到WMS  
**接口地址**： 域名/api/pms/createSample  
**请求方式**： POST  
**参数校验**： sign  
**接口描述**： 接料仅限制发生在仓库有库存情况下  
**Auth-date**：[add by guoqunchao 2018-11-21] 
#####   请求格式：  
```json  
{
        "sign": "81c4b6e5fd80424289f81a1f3d068e54",
        "data": [
            {
                "sampleSn": "SA20181121001",//样品申请编码（其他系统生成）
                "warehouseCode": "_HUANAN_",//申请样品来源仓库编码（编码待统一确定）
                "sampleUsername": "lijun",//样品申请人员工账号
				"sampleTime": "2018-11-24 16:30:58",//样品借出申请时间
                "enterpriseDominant": "1",//采购主体
				"hopeArriveDate": "2018-11-26 16:30:58",//希望拿到样品日期
                "usePeriod": "24",//使用时长(h)
                "detail": [{
                    "sku": "XD284002",//样品SKU
                    "sampleQuantity": 1,//样品数量
                }]
            }
        ]
    }
```  
#####   返回格式：  
```json  
{
    "code": 000001,
    "message": "成功",
    "data": [
        {
            "apiCode":"createSample",//接口代码
            "sampleSn": "SA20181121001",//样品申请编码
        }
    ]
}
  
```  
***  

### *9. WMS样品借料出库推送数据到外部系统（PMS）  
**接口地址**： 外部系统服务端接口  
**请求方式**： POST  
**参数校验**： sign  
**接口描述**： 不一定是采购部，摄影部、销售部都有可能  
**Auth-date**：[add by guoqunchao 2018-11-21] 
#####   请求格式：  
```json  
{
        "sign": "81c4b6e5fd80424289f81a1f3d068e54",
        "data": [
            {
                "sampleSn": "SA20181121001",//样品申请编码（其他系统生成）
                "warehouseCode": "_HUANAN_",//样品借出仓库编码（编码待统一确定）
                "sendUsername": "liushasha",//样品借出人员工账号
				"sendTime": "2018-11-24 16:30:58",//样品借出时间
                "enterpriseDominant": "1",//采购主体
				"sendMemo": "圆通快递，单号XXXXX",//邮寄信息
				"hopeBackDate": "2018-11-29 16:30:58",//期望返仓日期（面临缺货可能）
                "detail": [{
                    "sku": "XD284002",//SKU
                    "sampleQuantity": 1,//样品数量
                }]
            }
        ]
    }
```  
#####   返回格式：  
```json  
{
    "code": 000001,
    "message": "成功",
    "data": [
        {
            "apiCode":"",//外部系统接口代码
            "sampleSn": "SA20181121001",//样品申请编码
        }
    ]
}
  
```  
***  
