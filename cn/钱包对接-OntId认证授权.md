## OntId认证和授权

本协议将帮助您的钱包实现ONT ID管理、认证、授权功能，完成对接后将支持基于本体生态的ONTID相关的DAPP，如CandyBox。

为了方便集成，ontid认证授权采用H5页面展示，钱包方需要根据协议做相应的处理，钱包需要对接四个功能：
* 处理认证（Authentication）：注册ontid，人脸识别和提交认证
* 处理授权（Authorization）：CandyBox发送授权请求
* 钱包ontid管理：创建、删除、导出
* 钱包方服务器：签名功能

认证包含的subaction处理请求有四个：

1. [CandyBox请求身份信息](#CandyBox请求身份信息)
2. [认证DAPP请求获取注册ontid交易](#认证DAPP请求获取注册ontid交易)
3. [认证DAPP请求人脸识别](#认证DAPP请求人脸识别)
4. [认证DAPP请求提交认证信息](#认证DAPP请求提交认证信息)

钱包打开的认证和授权URL地址：
* 认证URL：https://auth.ont.io/#/authHome 
* 授权URL：https://auth.ont.io/#/mgmtHome?ontid={ontid}

对接时可需要导入cyano钱包SDK，参考钱包下载链接： http://101.132.193.149/files/app-debug.apk

## 认证和授权流程

认证和授权页面由官方提供，钱包接入方需要提供后台服务器，服务器负责签名和与ONTPASS通信。

### 认证Authentication流程

ONTID用户身份认证流程，认证过程中如果发现该ONTID没有注册到链上，需要用户签名，ONTPASS会帮忙注册：

![输入密码](https://raw.githubusercontent.com/ontio/documentation/master/pro-website-docs/assets/ui-register.jpg) 

### 授权Authorization流程

ONT ID授权指的是把用户已经获得的认证，授权给某个DAPP场景方，比如在CandyBox场景中，用户需要将授权信息提供给Candy项目方，才可以获得Candy。 流程是这样的：

![](https://raw.githubusercontent.com/ontio/documentation/master/pro-website-docs/assets/auth.png)

### 认证、授权、管理页面

<p>
  <img width="250px" src="https://raw.githubusercontent.com/ontio-cyano/integration-docs/master/images/ontid/authenticate.png">
  <img width="250px" src="https://raw.githubusercontent.com/ontio-cyano/integration-docs/master/images/ontid/authorize.png">
  <img width="250px" src="https://raw.githubusercontent.com/ontio-cyano/integration-docs/master/images/ontid/ontid-manage.png">
</p>

## 实现步骤

钱包需要分别实现认证和授权两个Action。


### CandyBox请求资产账户信息

数据如下，**URI编码，Base64编码**后CandyBox发送请求：
```

{
	"action": "getAccount", 
	"version": "v1.0.0",
	"id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",	
	"params": {
		"dappName": "dapp Name", 
		"dappIcon": "dapp Icon"
	}
}


```
钱包返回资产账户信息：

**URI解码，Base64解码**后，获取到的数据如下：
```
{
	"action": "getAccount", 
	"version": "v1.0.0",
	"id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",	
	"error": 0,
	"desc": "SUCCESS",
	"result": "AUr5QUfeBADq6BMY6Tp5yuMsUNGpsD7nLZ"  
}
```

账户对应的身份是```did:ont:AUr5QUfeBADq6BMY6Tp5yuMsUNGpsD7nLZ```，认证和授权时会使用。


### 认证DAPP请求获取注册ontid交易
<br>

认证时需要注册ontid，打开认证页面时，向钱包请求注册ontid交易的hex数据。

#### 获取注册ontid交易请求

数据如下，**URI编码，Base64编码**后CandyBox发送请求：
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

### 认证DAPP请求人脸识别
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

### 认证DAPP请求提交认证

#### DAPP发送提交认证请求

数据如下，**URI编码，Base64编码**后CandyBox发送请求：
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

钱包先**URI解码，Base64解码**后得到：


```

{
	"action": "authentication",
	"version": "v1.0.0",
	"id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",		
	"params": {
	    "subaction": "submit", 
	    "authenticationId": "http://www.authorize.com/?id=wtgetyeyhewyey"
	}
}
```

1. 转发认证的内容的地址给钱包服务器

```

{
	"action": "authentication",
	"version": "v1.0.0",
	"id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",		
	"params": {
	    "subaction": "submit", 
	    "authenticationId": "http://www.authorize.com/?id=wtgetyeyhewyey"
	}
}
```
	    
2. 钱包服务器获取认证内容，服务器签名
3. 钱包服务器发送认证请求地址和签名给ONTPASS，ONTPASS处理认证请求并返回认证结果。钱包服务器返回认证结果给钱包。

```
Host：域名+/api/v1/authentication/submit
Method：POST /HTTP/1.1 Content-Type: application/json 

 RequestExample: 
 { 
    "action": "authentication", 
    "authenticationId":"http://www.authorize.com/?id=wtgetyeyhewyey", 
    "ontid":"did:ont:AUr5QUfeBADq6BMY6Tp5yuMsUNGpsD7nLZ",
    "signature":"AZMju/RtF5a594gR5VALto+nAQgk8mb41RT...isjt4wFKmkSMCRx3Mh0sk521jU5S4=" 
  } 
 
 SuccessResponse： 
 { 
    "action": "authentication", 
    "error": 0, 
    "desc": "SUCCESS", 
    "version": "1.0.0", 
    "result": true 
 }
```


4. 钱包响应CandyBox请求。**URI编码，Base64编码**后发送

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

用户在CandyBox页面（如Candybox）点击授权  

#### CandyBox发送授权请求
<br>

数据如下，**URI编码，Base64编码**后CandyBox发送请求：
```
{
	"action": "authorization",
	"version": "v1.0.0",
	"id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",		
	"params": {
        "subaction": "requestAuthorization",
	    "seqNo": "0001",
	    "userOntid": "did:ont:Assxxxxxxxxxxxxx",
		"dappOntid": "did:ont:Assxxxxxxxxxxxxx",
		"dappName": "candy box",
		"callback": "http://candybox.com/callback",
		"dappUrl": "http://candybox.com/",
		"authTemplate": "authtemplate_kyc01"
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
        "subaction": "requestAuthorization",
        "seqNo": "0001",
		"userOntid": "did:ont:Assxxxxxxxxxxxxx",
		"dappOntid": "did:ont:Assxxxxxxxxxxxxx",
		"dappName": "candy box",
		"callback": "http://candybox.com/callback",
		"dappUrl": "http://candybox.com/",
		"authTemplate": "authtemplate_kyc01"
	}
}
```

1. 钱包临时保存该数据，打开授权DAPP，授权DAPP发送：

```

{
	"action": "authorization",
	"version": "v1.0.0",
	"id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",		
	"params": {
		"subaction": "getAuthorizationInfo"
	}
}
```
2. 钱包返回给授权DAPP请求内容，钱包返回CandyBox已打开授权DAPP。

3. 授权DAPP到ONTPASS获取数据，展示授权页面

4. 用户点击授权，发送解密请求。

```
{
  "action": "authorization",
  "version": "v1.0.0",  
  "id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",	 
  "params": {
      "subaction": "decryptClaim",
      "message":"AXFqy6w/xg+IFQBRZvucKXvTuIZaIxOS0pesuBj1IKHvw56DaFwWogIcr1B9zQ13nUM0w5g30KHNNVCTo14lHF0="
  }
}
```


5. 弹出密码框，用户输入密码，解密消息，返回消息原文给授权DAPP。

```
{
  "action": "authentication",
  "version": "1.0.0", 
  "id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",	  
  "error": 0,
  "desc": "SUCCESS",
  "result": {
      "message":"hello world"
  }
}
```

6. 授权DAPP发送原文给CandyBox的后台callback地址。


callback 接收消息格式：

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


### 钱包方服务器接口

#### 提交认证

1. 认证DAPP提交认证

```

{
	"action": "authentication",
	"version": "v1.0.0",
	"id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",		
	"params": {
	    "subaction": "submit", 
	    "authenticationId": "http://www.authorize.com/?id=wtgetyeyhewyey"
	}
}
```
2. 服务器获取认证内容，服务器签名
3. 服务器发送认证请求地址和签名给ONTPASS



## 代码参考

##### 签名验证方法
* [java sdk验签](https://github.com/ontio/ontology-java-sdk/blob/master/docs/cn/interface.md#%E7%AD%BE%E5%90%8D%E9%AA%8C%E7%AD%BE)
* [ts sdk验签](https://github.com/ontio/ontology-ts-sdk/blob/master/test/message.test.ts)

##### DApp后端查询交易事件
* [java sdk 交易事件查询方法](https://github.com/ontio/ontology-java-sdk/blob/master/docs/cn/basic.md#%E4%B8%8E%E9%93%BE%E4%BA%A4%E4%BA%92%E6%8E%A5%E5%8F%A3)
* [ts sdk 交易事件查询方法](https://github.com/ontio/ontology-ts-sdk/blob/master/test/websocket.test.ts)

##### 钱包
* [cyano-android](https://github.com/ontio-cyano/cyano-android)
* [cyano-ios](https://github.com/ontio-cyano/cyano-ios)

##### dApi-mobile client sdk
* [cyano-bridge](https://github.com/ontio-cyano/cyano-bridge)

##### dApi-mobile provider sdk
* [cyano-android-sdk](https://github.com/ontio-cyano/cyano-android-sdk)
* [cyano-ios-sdk](https://github.com/ontio-cyano/cyano-ios-sdk)
