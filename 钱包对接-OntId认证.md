## OntId认证和授权

本协议将帮助您的应用实现ONT ID创建、认证、管理、授权。

目前已支持的功能：
* 认证Certification
* 授权Authorization

## 认证和授权流程

认证和授权页面由官方提供，钱包接入方需要提供后台服务器，服务器负责签名和与ONTPASS通信。

### 认证certification

ONT ID用户身份认证流程，认证过程中如果发现该ONTID没有注册到链上，会帮忙注册：

![输入密码](https://raw.githubusercontent.com/ontio/documentation/master/pro-website-docs/assets/ui-register.jpg) 

### 授权Authorization

ONT ID授权指的是把用户已经获得的认证，授权给某个DAPP场景方，比如在CandyBox场景中，用户需要将授权信息提供给Candy项目方，才可以获得Candy。 流程是这样的：

![](https://raw.githubusercontent.com/ontio/documentation/master/pro-website-docs/assets/auth.png)

## 实现步骤

钱包需要分别实现认证和授权两个Action。


### 认证certification
<br>

认证过程中需要钱包后台服务器签名，再发送认证信息给ONTPASS。

#### DAPP发送认证请求

数据如下，**URI编码，Base64编码**后DAPP发送请求：
```
{
	"action": "certification",
	"version": "v1.0.0",
	"params": {
	    "url": "http://www.authorize.com/?id=wtgetyeyhewyey"  //已认证的内容的地址
	}
}
```

|字段|类型|定义|
| :---| :---| :---|
| action | string | 操作类型|
| params | string | 方法要求的参数 |


#### 钱包处理认证请求

钱包先**URI解码，Base64解码**后得到：


```

{
	"action": "certification",
	"version": "v1.0.0",
	"params": {
	    "url": "http://www.authorize.com/?id=wtgetyeyhewyey"
	}
}
```
1. 转发认证过的内容的地址给服务器
2. 服务器获取认证内容，服务器签名
3. 发送认证内容和签名给ONTPASS

```
Host：域名+/api/v1/ontta/ocr/authentication 
Method：POST /HTTP/1.1 Content-Type: application/json 

 RequestExample: 
 { 
    "action": "certification", 
    "auth_id":"xxxxxxxxxxx", 
    "auth_context":"xxxxxxxxxxxxxxxx", 
    "ontpass_ontid":"didontA9Kn1v4zRzepHY344g2K1eiZqdskhnh2Jv", 
    "signature":"AZMju/RtF5a594gR5VALto+nAQgk8mb41RT...isjt4wFKmkSMCRx3Mh0sk521jU5S4=" 
  } 
 
 SuccessResponse： 
 { 
    "action": "certification", 
    "error": 0, 
    "desc": "SUCCESS", 
    "version": "1.0.0", 
    "result": true 
 }
```


4. 响应DAPP请求。**URI编码，Base64编码**后发送

```
{
  "action": "certification",
  "version": "1.0.0", 
  "error": 0,
  "desc": "SUCCESS",
  "result": true
}
```


### 授权Authorization

<br>

用户在选择授权选项并点击确定按钮

#### DAPP发送授权请求
<br>

数据如下，**URI编码，Base64编码**后DAPP发送请求：
```
{
	"action": "authorization",
	"version": "v1.0.0",
	"params": {
		"seqno": "0001",
		"user_ontid": "did:ont:Assxxxxxxxxxxxxx",
		"app_ontid": "did:ont:Assxxxxxxxxxxxxx",
		"to_ontid": "did:ont:Assxxxxxxxxxxxxx",
		"redirect_uri": "http://candybox.com/",
		"auth_templete": "authtemplate_kyc01"
	}
}
```

|字段|类型|定义|
| :---| :---| :---|
| action | string | 操作类型|
| params | string | 方法要求的参数 |


#### 钱包处理认证请求

钱包先**URI解码，Base64解码**后得到：


```

{
	"action": "authorization",
	"version": "v1.0.0",
	"params": {
		"seqno": "0001",
		"user_ontid": "did:ont:Assxxxxxxxxxxxxx",
		"app_ontid": "did:ont:Assxxxxxxxxxxxxx",
		"to_ontid": "did:ont:Assxxxxxxxxxxxxx",
		"redirect_uri": "http://candybox.com/",
		"auth_templete": "authtemplate_kyc01"
	}
}
```
1. 弹出密码框
2. 用户输入密码，签名
3. 返回签名给DAPP。**URI编码，Base64编码**后发送

```
{
  "action": "certification",
  "error": 0,
  "desc": "SUCCESS",
  "result": {
      "signature":"AXFqy6w/xg+IFQBRZvucKXvTuIZaIxOS0pesuBj1IKHvw56DaFwWogIcr1B9zQ13nUM0w5g30KHNNVCTo14lHF0="
  }
}
```