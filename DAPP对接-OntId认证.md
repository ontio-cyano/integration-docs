## OntId认证和授权

本协议将帮助您的应用实现ONT ID认证、授权，典型的应用之一是CandyBox。

目前已支持的功能：
* 认证Authentication(请求认证，人脸识别和提交认证)
* 授权Authorization（请求授权）

## 认证和授权流程

认证和授权页面由官方提供，钱包接入方需要提供后台服务器，服务器负责签名和与ONTPASS通信。

### 认证Authentication

ONT ID用户身份认证流程，认证过程中如果发现该ONTID没有注册到链上，会帮忙注册：

![输入密码](https://raw.githubusercontent.com/ontio/documentation/master/pro-website-docs/assets/ui-register.jpg) 

### 授权Authorization

ONT ID授权指的是把用户已经获得的认证，授权给某个DAPP场景方，比如在CandyBox场景中，用户需要将授权信息提供给Candy项目方，才可以获得Candy。 流程是这样的：

![](https://raw.githubusercontent.com/ontio/documentation/master/pro-website-docs/assets/auth.png)

## 实现步骤

认证：

1. CandyBox发送认证请求。
2. 钱包处理请求，打开认证DAPP。

授权：

1. CandyBox发送授权请求
2. 钱包处理请求



### CandyBox请求认证
<br>

认证时需要注册ontid，打开认证页面时，向钱包请求注册ontid交易的hex数据。

#### CandyBox请求认证

数据如下，**URI编码，Base64编码**后DAPP发送请求：
```
{
	"action": "authentication",
	"version": "v1.0.0",
	"id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",		
	"params": {
	    "subaction": "requestAuthentication
	}
}
```
#### CandyBox请求认证处理
钱包先**URI解码，Base64解码**后，处理完返回：


```

{
	"action": "authentication",
	"version": "v1.0.0",
	"id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",		
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
		"callback": "http://candybox.com/callback",
		"dappUrl": "http://candybox.com/",
		"auth_templete": "authtemplate_kyc01"
	}
}
```

|字段|类型|定义|
| :---| :---| :---|
| action | string | 操作类型|
| params | string | 方法要求的参数 |


#### 钱包处理认证请求


1. 钱包处理认证请求
2. 授权DAPP发送原文给CandyBox后台。

DAPP后台接受 callback 接收消息格式：

```
{
  "action": "authorization",
  "version": "v1.0.0",  
  "id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",	 
  "params": {
      "message":"hello world"
  }
}
```
响应成功或失败：

```
{
  "action": "authentication",
  "version": "1.0.0", 
  "id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",	  
  "error": 0,
  "desc": "SUCCESS",
  "result": true
}
```

