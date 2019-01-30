## OntId认证和授权

本协议将帮助您的应用实现ONT ID认证、授权，典型的应用之一是CandyBox。

支持的功能：
* getIdentity
* 授权Authorization（请求授权）
* CandyBox服务器接口

## getIdentity和授权流程

认证和授权页面由官方提供，钱包接入方需要提供后台服务器，服务器负责签名和与ONTPASS通信。

### getIdentity

获取钱包当前身份信息。

### 授权Authorization

ONT ID授权指的是把用户已经获得的认证，授权给某个DAPP场景方，比如在CandyBox场景中，用户需要将授权信息提供给Candy项目方，才可以获得Candy。 流程是这样的：

![](https://raw.githubusercontent.com/ontio/documentation/master/pro-website-docs/assets/auth.png)

## 实现步骤

getIdentity：

1. CandyBox获取身份getIdentity。
2. 钱包处理请求，如果未注册，打开认证DAPP。

授权：

1. CandyBox发送授权请求
2. 钱包处理请求



### 获取身份getIdentity
    



#### 获取身份请求

数据如下，**URI编码，Base64编码**后DAPP发送请求：

```

{
	"action": "getIdentity",
	"version": "v1.0.0",
	"id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",	
	"params": {
		"dappName": "dapp Name",
		"dappIcon": "dapp Icon"
	}
}


```

|字段|类型|定义|
| :---| :---| :---|
| action   |  string |  操作类型 |
| dappName   | string  | dapp名字 |
| dappIcon   | string  | dapp icon信息 |

#### 获取身份请求处理
钱包先**URI解码，Base64解码**后。检查是否已经有Identity，如果没有，就进入创建ontid和认证。如果已经有Identity，就返回Identity。


处理完返回：


```
{
	"action": "authentication",
	"version": "v1.0.0",
	"id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",		
	"params": {
	    "subaction": "getIdentity"
	}
}
```



### 授权Authorization

<br>

用户在CandyBox页面点击授权  

#### CandyBox发送授权请求
<br>

数据如下，**URI编码，Base64编码**后CandyBox发送请求：
```
{
	"action": "authorization",
	"version": "v1.0.0",
	"id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",		
	"params": {
	    "subaction": "requestAuthorization"
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


1. 钱包处理认证请求
2. 授权DAPP发送原文给CandyBox后台。

CandyBox后台接受 callback 接收消息格式：

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


### CandyBox服务器接口


callback 接收claim原文：

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
