## OntId认证和授权

本协议将帮助您的应用实现ONT ID创建、认证、管理、授权。

目前已支持的功能：
* 认证Authentication(注册ontid，人脸识别和提交认证)
* 授权Authorization
* ontid管理

## 认证和授权流程

认证和授权页面由官方提供，钱包接入方需要提供后台服务器，服务器负责签名和与ONTPASS通信。

### 认证Authentication

ONT ID用户身份认证流程，认证过程中如果发现该ONTID没有注册到链上，会帮忙注册：

![输入密码](https://raw.githubusercontent.com/ontio/documentation/master/pro-website-docs/assets/ui-register.jpg) 

### 授权Authorization

ONT ID授权指的是把用户已经获得的认证，授权给某个DAPP场景方，比如在CandyBox场景中，用户需要将授权信息提供给Candy项目方，才可以获得Candy。 流程是这样的：

![](https://raw.githubusercontent.com/ontio/documentation/master/pro-website-docs/assets/auth.png)

## 实现步骤

钱包需要分别实现认证和授权两个Action。


### 获取注册ontid交易
<br>

认证时需要注册ontid，打开认证页面时，向钱包请求注册ontid交易的hex数据。

#### 获取注册ontid交易请求

数据如下，**URI编码，Base64编码**后DAPP发送请求：
```
{
	"action": "authentication",
	"version": "v1.0.0",
	"id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",		
	"params": {
	    "subaction": "getRegistryOntidTx"
	}
}
```
#### 获取注册ontid交易的请求处理
钱包先**URI解码，Base64解码**后，处理完返回：


```

{
	"action": "authentication",
	"version": "v1.0.0",
	"id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",		
	"result": {
	    "subaction": "getRegistryOntidTx", 
	    "ontid":"did:ont:AUr5QUfeBADq6BMY6Tp5yuMsUNGpsD7nLZ",
	    "registryOntidTx": "00d1fad4f3b3f40100000............e7493fa52c01f9c6f65ac"
	}
}
```

### 人脸识别
<br>

认证过程中需要钱包后台服务器签名，再发送认证信息给ONTPASS。如果需要人脸识别，请求打开原生做人脸识别。

#### DAPP发送人脸识别请求

数据如下，**URI编码，Base64编码**后DAPP发送请求：
```
{
	"action": "authentication",
	"version": "v1.0.0",
	"id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",		
	"params": {
	    "subaction": "faceRecognition"
	}
}
```
#### DAPP发送人脸识别请求处理
钱包先**URI解码，Base64解码**后，处理完action返回：


```

{
	"action": "authentication",
	"version": "v1.0.0",
	"id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",		
	"result": {
	    "subaction": "faceRecognition", 
	    "data": ""
	}
}
```

### 提交认证

#### DAPP发送提交认证请求

数据如下，**URI编码，Base64编码**后DAPP发送请求：
```
{
	"action": "authentication",
	"version": "v1.0.0",
	"id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",		
	"params": {
	    "subaction": "submit", 
	    "authenticationId": "http://www.authorize.com/?id=wtgetyeyhewyey"  //认证的内容存放的地址
	}
}
```

|字段|类型|定义|
| :---| :---| :---|
| action | string | 操作类型|
| params | string | 方法要求的参数 |


#### 钱包处理提交认证请求


1. 钱包处理提交认证请求
2. 响应DAPP请求。**URI编码，Base64编码**后发送

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


1. 钱包处理认证请求
2. 返回成功给给授权DAPP。

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


### ontid管理

删除ontid

```
{
  "action": "authorization",
  "version": "v1.0.0",  
  "id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",	 
  "params": {
      "subaction": "deleteOntid"
  }
}
```

导出ontid

```
{
  "action": "authorization",
  "version": "v1.0.0",  
  "id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",	 
  "params": {
      "subaction": "exportOntid"
  }
}
```