## 招生通-腾讯云API网关接入说明

### 环境定义
测试环境：  https://service-2iy312rw-1258344699.gz.apigw.tencentcs.com/test

预发布环境: https://service-6ucx1e74-1258344699.gz.apigw.tencentcs.com/prepub

正式环境：  https://service-miudptpc-1258344699.gz.apigw.tencentcs.com/release

## 调用方式
招生通的以下API以Https的方式对外提供服务。对这些API的请求都要通过密钥进行签名。
签名所需的密钥可以找相关的技术人员提供。

## 密钥内容
【secret_id】示例：`AKIDCg*****j548pN`  用于标识所使用的哪个密钥，并参与签名计算，传输过程中体现。

【secret_key】示例：`ZxF2wh*****N2oPrC`  用于签名计算，传递过程中无体现。

## 调用方法

[常用语言的签名 Demo>>](https://github.com/apigateway-demo)

招生通的服务端API仅支持Http POST方式，如果demo中的演示代码是GET方法，请自行修改为POST方法。

如果所用语言未在Demo中覆盖，可参考[API签名文档](https://cloud.tencent.com/document/product/628/11819)自行实现。

## 接口使用流程
![Image text](https://main.qcloudimg.com/raw/5c4943d3e5680fa0b0b42b8add88eae1.png)

## 接口说明
### 在请求url中需要带上的参数有（鉴权checktoken接口除外）：
| 公共参数 | 类型 | 参数要求 | 描述 |
| --- | --- | --- | --- |
|SdkAppID|int|必填|业务id|
|AppID|int|必填|学校id|
|RoleSpace|string|必填|角色空间|
|uin|string|必填|用户uin(唯一账号标识)|

### 1.鉴权

如果http 回包返回401，则表示Token 非法或者失效，根据场景，选择弹出登录页或者异常提醒

|请求基本信息|描述|
|-------------|-------------|
|方法|POST|
|请求URL| /v1/open/account/checktoken|
|header|Content-Type:application/json|

RequestBody: 

```json
{
  "Token":"xxx"     // 用户token，必填
}
```

ResponseBody:

请求成功:
```json
{
    "Response": {
        "Uin": "999",                         //用户唯一id
        "SdkAppID": "user",                   //用户sdkappid
        "AppID": "11",                        //学校id
        "RoleSpace": "admin",                 //用户角色空间
        "Session":{
          "uin": "999",                       //用户唯一id
          "username": "nick"                  //用户名，目前同uin
        },
        "RequestId": ""
    }
}
```
请求失败(判断是否有 "Error" 字段) 示例：
```json
{
    "Response": {
        "Error": {
            "Code": "ParamInvalid",
            "Message": "table not exist"
        },
        "RequestId": ""
    }
}
```

### 2.机器客服 
|请求基本信息|描述|
|-------------|-------------|
|方法|POST|
|请求URL| /v1/open/platform/qidian/robot-query?SdkAppID=xxxx&AppID=xxx&RoleSpace=xxx&uin=xxx|
|header|Content-Type:application/json|

Req :

```json
{
"question":"我要招生", //查询的问题，必填
"from_user":"test"      //用户名或ID，非必填
}
```

ResponseBody:

请求成功:
```json
{
   "Response":{
      "data":{
         "result":[
            {
               "type":1,
               "qaid":3,
               "question":"我要招生",
               "answer":"好的，干了这一杯再说"
            }
         ]
      },
      "RequestId":""
   }
}
```
请求失败(判断是否有 "Error" 字段) 示例：
```json
{
    "Response": {
        "Error": {
            "Code": "ParamInvalid",
            "Message": "table not exist"
        },
        "RequestId": ""
    }
}
```
### 3.拉取联系我列表
|请求基本信息|描述|
|-------------|-------------|
|方法|POST|
|请求URL| /v1/open/platform/qywx/get-contact-way-list?SdkAppID=xxxx&AppID=xxx&RoleSpace=xxx&uin=xxx|
|header|Content-Type:application/json|

RequestBody: 

```json
{
  "Offset":0,  // 偏移量，非必填
  "Limit":20, //都不填，则返回全量；非必填
  "Flag":1 // 1--企业微信小程序按钮；0--所有；非必填
}
```



ResponseBody: 

请求成功:
```json
{
  "Response": {
    "contact_way_list": [
      {
        "config_id":"42b34949e138eb6e027c123cba77fad7",
        "type":1,
        "scene":1,
        "style":2,
        "remark":"test remark",
        "skip_verify":true,
        "state": {
            "ID": "10001",
            "Name": "东东",
            "Owner": "dd",
            "Phone": "1221312312321"
        },
        "qr_code":"http://p.qpic.cn/wwhead/duc2TvpEgSdicZ9RrdUtBkv2UiaA/0",
        "user":[{
           "Name": "xxxx", // 姓名
           "UserID": "xxxx", // 企业微信userid
        }]
      }
    ]
  }
}
```
请求失败(判断是否有 "Error" 字段) 示例：
```json
{
    "Response": {
        "Error": {
            "Code": "ParamInvalid",
            "Message": "table not exist"
        },
        "RequestId": ""
    }
}
```

### 图文内容

### 1.添加图文

|请求基本信息|描述|
|-------------|-------------|
|方法|POST|
|请求URL| /v1/open/platform/material/add?SdkAppID=xxxx&AppID=xxx&RoleSpace=xxx&uin=xxx|
|header|Content-Type:application/json|

RequestBody: 

```json
{
  "Title":"图文标题",           // 必填
  "Author":"我是作者",          // 非必填
  "Remark":"我是摘要",          // 非必填
  "ImageURL":"上传图片地址",    // 非必填
  "Content":"我是正文"          // 必填
}
```



ResponseBody:

请求成功:
```json
{
  "Response": {
  }
}
```
请求失败(判断是否有 "Error" 字段) 示例：
```json
{
    "Response": {
        "Error": {
            "Code": "ParamInvalid",
            "Message": "table not exist"
        },
        "RequestId": ""
    }
}
```
### 2.编辑图文
|请求基本信息|描述|
|-------------|-------------|
|方法|POST|
|请求URL| /v1/open/platform/material/edit?SdkAppID=xxxx&AppID=xxx&RoleSpace=xxx&uin=xxx|
|header|Content-Type:application/json|

RequestBody: 

```json
{
  "Id":"图文id",      // 必填
  "Title":"图文标题", // 必填
  "Author":"我是作者",// 非必填
  "Remark":"我是摘要",// 非必填
  "ImageURL":"上传图片地址", // 非必填
  "Content":"我是正文" // 必填
}
```

ResponseBody:

请求成功:
```json
{
  "Response": {
  }
}
```
请求失败(判断是否有 "Error" 字段) 示例：
```json
{
    "Response": {
        "Error": {
            "Code": "ParamInvalid",
            "Message": "table not exist"
        },
        "RequestId": ""
    }
}
```
### 3.删除图文
|请求基本信息|描述|
|-------------|-------------|
|方法|POST|
|请求URL| /v1/open/platform/material/delete?SdkAppID=xxxx&AppID=xxx&RoleSpace=xxx&uin=xxx|
|header|Content-Type:application/json|

RequestBody: 

```json
{
  "IdSet":["图文id"]    // 必填
}
```


ResponseBody:

请求成功:
```json
{
  "Response": {
  }
}
```
请求失败(判断是否有 "Error" 字段) 示例：
```json
{
    "Response": {
        "Error": {
            "Code": "ParamInvalid",
            "Message": "table not exist"
        },
        "RequestId": ""
    }
}
```

### 4.拉取图文列表
|请求基本信息|描述|
|-------------|-------------|
|方法|POST|
|请求URL| /v1/open/platform/material/list?SdkAppID=xxxx&AppID=xxx&RoleSpace=xxx&uin=xxx|
|header|Content-Type:application/json|

RequestBody: 

```json
{
  "IdSet":["图文id"],   // 非必填
  "Offset":0,           // 非必填
  "Limit":15            // 非必填
}
```

ResponseBody:

请求成功:
```json
{
  "Response": {
      "DataSet":[{
          "Id":"图文id",
          "Title":"图文标题",
          "Author":"我是作者",
          "Remark":"我是摘要",
          "ImageURL":"封面图片地址",
          "Content":"我是正文",
          "CreateTime":1565177822
      }]
  }
}
```
请求失败(判断是否有 "Error" 字段) 示例：
```json
{
    "Response": {
        "Error": {
            "Code": "ParamInvalid",
            "Message": "table not exist"
        },
        "RequestId": ""
    }
}
```

### 生源
#### 1 生源入库
|请求基本信息|描述|
|-------------|-------------|
|方法|POST|
|请求URL| /v1/open/platform/qywx/add-customer?SdkAppID=xxxx&AppID=xxx&RoleSpace=xxx&uin=xxx|
|header|Content-Type:application/json|

RequestBody: 
```json
{
    "userType": "学生",  // 学生、家长、其它  // 必填
    "name": "朱小明",  //姓名  // 必填
    "gender": 1,  // 性别  0 未知  1 男  2  女  // 必填
    "wechat": "wwwewe",  // 微信号
    "wxnickname": "昵称",   //微信昵称
    "province": "湖南", // 必填 
    "subject": "文科", // 必填
    "middleSchool": "雅礼中学",  //毕业中学
    "mobile": "1663876323",
    "score": "556",
    "unionID": "5635624524" // 必填
}
```

ResponseBody:

请求成功:
```json
{
  "Response": {
    "IsSuccess": true,
    "TraceId": "d87904776a2ff02",
    "RequestId": "d87904776a2ff02"
  }
}
```
请求失败(判断是否有 "Error" 字段) 示例：
```json
{
    "Response": {
        "Error": {
          "Code": "FailedOperation",
          "Message": "failed operation"
        },
        "RequestId": ""
    }
}
```
#### 2 编辑数据
|请求基本信息|描述|
|-------------|-------------|
|方法|POST|
|请求URL| /v1/open/platform/qywx/modify-customer?SdkAppID=xxxx&AppID=xxx&RoleSpace=xxx&uin=xxx|
|header|Content-Type:application/json|

RequestBody: 
```json
{
    "userType": "学生",  // 学生、家长、其它  // 必填
    "name": "朱小明",  //姓名  // 必填
    "gender": 1,  // 性别  0 未知  1 男  2  女  // 必填
    "wechat": "wwwewe",  // 微信号
    "wxnickname": "昵称",   //微信昵称
    "province": "湖南", // 必填 
    "subject": "文科", // 必填
    "middleSchool": "雅礼中学",  //毕业中学
    "mobile": "1663876323",
    "score": "556",
    "unionID": "5635624524" // 必填
}
```

ResponseBody:

请求成功:
```json
{
  "Response": {
    "cust_id": "28857850030000000000000000000120",
    "RequestId": "d87904776a2ff02"
  }
}
```
请求失败(判断是否有 "Error" 字段) 示例：
```json
{
  "Response": {
    "Error": {
      "Code": "FailedOperation",
      "Message": "failed operation"
    },
    "RequestId": ""
  }
}
```
