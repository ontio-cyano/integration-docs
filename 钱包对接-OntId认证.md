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
	"id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",		
	"params": {
	    "reqId": "http://www.authorize.com/?id=wtgetyeyhewyey"  //已认证的内容的地址
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
	"id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",		
	"params": {
	    "reqId": "http://www.authorize.com/?id=wtgetyeyhewyey"
	}
}
```
1. 转发认证过的内容的地址给服务器
2. 服务器获取认证内容，服务器签名
3. 服务器发送认证请求地址和签名给ONTPASS，ONTPASS处理认证请求

```
Host：域名+/api/v1/ontta/ocr/certification 
Method：POST /HTTP/1.1 Content-Type: application/json 

 RequestExample: 
 { 
    "action": "certification", 
    "reqId":"http://www.authorize.com/?id=wtgetyeyhewyey", 
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
  "id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",	  
  "error": 0,
  "desc": "SUCCESS",
  "result": true
}
```


### 授权Authorization

<br>

用户在DAPP页面（如Candybox）点击授权  

#### DAPP发送授权请求
<br>

数据如下，**URI编码，Base64编码**后DAPP发送请求：
```
{
	"action": "authorization",
	"version": "v1.0.0",
	"id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",		
	"params": {
		"seqno": "0001",
		"user_ontid": "did:ont:Assxxxxxxxxxxxxx",
		"app_ontid": "did:ont:Assxxxxxxxxxxxxx",
		"to_ontid": "did:ont:Assxxxxxxxxxxxxx",
		"callback": "http://candybox.com/",
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
	"id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",		
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
3. 转发数据给授权DAPP，DAPP到ONTPASS获取数据，展示授权页面

4. 用户选择claim，点击授权，发送签名请求。

```
{
  "action": "signMessage",
  "version": "v1.0.0",  
  "id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",	 
  "error": 0,
  "desc": "SUCCESS",
  "result": {
      "signature":"AXFqy6w/xg+IFQBRZvucKXvTuIZaIxOS0pesuBj1IKHvw56DaFwWogIcr1B9zQ13nUM0w5g30KHNNVCTo14lHF0="
  }
}
```

5. 返回签名给DAPP。**URI编码，Base64编码**后发送

```
{
  "action": "certification",
  "version": "v1.0.0",  
  "id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",	  
  "error": 0,
  "desc": "SUCCESS",
  "result": {
      "signature":"AXFqy6w/xg+IFQBRZvucKXvTuIZaIxOS0pesuBj1IKHvw56DaFwWogIcr1B9zQ13nUM0w5g30KHNNVCTo14lHF0="
  }
}
```