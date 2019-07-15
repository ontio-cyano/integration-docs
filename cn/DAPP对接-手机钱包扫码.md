<h1 align="center">扫码接入流程</h1>
<p align="center" class="version">Version 0.8.0 </p>

## 概述

本文用于指导DApp方如何接入Provider，并使用扫码登陆，扫码调用智能合约等服务。
流程中涉及到的参与方包括：

* DApp方：对ONT生态内的用户提供dApp，是本体生态中重要的组成部分。
* Provider：实现daApi mobile规范的钱包

## 交互流程说明

![login-invoke](../images/split-login-invoke.png)

### Login
- 1 DApp方提供二维码（[登陆二维码标准](#登陆二维码标准)）
- 2 DApp服务端登陆方法（[DApp服务端登陆接口](#DApp服务端登陆接口)）
- 3 DApp后端验证签名（[签名验证方法](#签名验证方法)）后返回验证结果

### Invoke Smart contract
- 1 DApp方提供二维码（[调用合约二维码标准](#调用合约二维码标准)）
- 2 Provider构造交易，用户签名，预执行交易，用户确认，发送到链上，返回交易hash给DApp后端
- 3 dApp后端查询这笔合约交易（[交易事件查询方法](#交易事件查询方法)）

## 接入步骤

### 前提条件
使用前，你需要联系[本体机构合作](https://info.ont.io/cooperation/en)

### 登陆接入步骤

#### 登陆二维码标准
扫码获取

```
{
	"action": "login",
	"version": "v1.0.0",
	"id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",	
	"params": {
		"type": "ontid or account",
		"dappName": "dapp Name",
		"dappIcon": "dapp Icon",
		"message": "helloworld",
		"expire": 1546415363, // QR Code expire time
		"callback": "http://101.132.193.149:4027/blockchain/v1/common/test-onto-login"
	}
}
```

|字段|类型|定义|
| :---| :---| :---|
| action   |  string |  定义此二维码的功能，登录设定为"Login"，调用智能合约设定为"invoke" |
| id   |  string |  消息序列号，可选 |
| type   |  string |  定义是使用ontid登录设定为"ontid"，钱包地址登录设定为"account" |
| dappName   | string  | dapp名字 |
| dappIcon   | string  | dapp icon信息 |
| message   | string  | 随机生成，用于校验身份  |
| expire   | long  | 可选  |
| callback   | string  |  用户扫码签名后发送到DApp后端URL |

### DApp服务端登陆接口
method: post 

```
{
	"action": "login",
	"version": "v1.0.0",
	"id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",	
	"params": {
		"type": "ontid or account",
		"user": "did:ont:AUEKhXNsoAT27HJwwqFGbpRy8QLHUMBMPz",
		"message": "helloworld",
		"publickey": "0205c8fff4b1d21f4b2ec3b48cf88004e38402933d7e914b2a0eda0de15e73ba61",
		"signature": "01abd7ea9d79c857cd838cabbbaad3efb44a6fc4f5a5ef52ea8461d6c055b8a7cf324d1a58962988709705cefe40df5b26e88af3ca387ec5036ec7f5e6640a1754"
	}
}
```

|字段|类型|定义|
| :---| :---| :---|
| action | string | 操作类型 |
| id   |  string |  消息序列号，可选 |
| params | string | 方法要求的参数 |
| type   |  string |  定义是使用ontid登录设定为"ontid"，钱包地址登录设定为"account" |
| user | string | 用户做签名的账户，比如用户的ontid或者钱包地址 |
| message   | string  | 随机生成，用于校验身份  |
| publickey | string | 账户公钥 |
| signature  |  string |  用户签名 |

#### Response
* Success

```
{
  "action": "login",
  "id": "10ba038e-48da-487b-96e8-8d3b99b6d18a", 
  "error": 0,
  "desc": "SUCCESS",
  "result": true
}
```

* Failed

```
{
  "action": "login",
  "id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",   
  "error": 80001,
  "desc": "PARAMS ERROR",
  "result": 1
}
```


### 调用合约二维码标准
扫码获取

```
{
	"action": "invoke",
	"version": "v1.0.0",
	"id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",	
	"params": {
		"login": true,
		"callback": "http://101.132.193.149:4027/invoke/callback",
		"expire": 1546415363, // 二维码过期时间
		"qrcodeUrl": "http://101.132.193.149:4027/qrcode/AUr5QUfeBADq6BMY6Tp5yuMsUNGpsD7nLZ"
	}
}
```

|字段|类型|定义|
| :---        | :---    | :---                                                              |
| action      | string  | 操作类型，登录设定为"Login"，调用智能合约设定为"invoke" |
| qrcodeUrl         | string  | 二维码参数地址                                           |
| callback         | string  | 选填，返回交易hash给dApp服务端                                           |
| expire         | long  | 可选，二维码过期时间                         |

根据二维码中qrcodeUrl链接，GET的的数据如下：

```
{
	"action": "invoke",
	"version": "v1.0.0",
	"id": "10ba038e-48da-487b-96e8-8d3b99b6d18a",	
	"params": {
		"invokeConfig": {
			"contractHash": "16edbe366d1337eb510c2ff61099424c94aeef02",//合约hash
			"functions": [{
				"operation": "method name", //调用合约的函数名
				"args": [{  //调用合约的参数，共三个，第一个参数值是数组类型
					"name": "arg0-list",
					"value": [true, 100, "Long:100000000000", "Address:AUr5QUfeBADq6BMY6Tp5yuMsUNGpsD7nLZ", "ByteArray:aabb", "String:hello", [true, 100], {
						"key": 6
					}]
				}, {
					"name": "arg1-map",  //第二个参数值是Map类型
					"value": {
						"key": "String:hello",
						"key1": "ByteArray:aabb",
						"key2": "Long:100000000000",
						"key3": true,
						"key4": 100,
						"key5": [100],
						"key6": {
							"key": 6
						}
					}
				}, {  //第三个参数值是String类型
					"name": "arg2-str",
					"value": "String:test"
				}]
			}],
			"payer": "AUr5QUfeBADq6BMY6Tp5yuMsUNGpsD7nLZ",
			"gasLimit": 20000,
			"gasPrice": 500
		}
	}
}


```
>> 调用合约时，如果二维码里payer没填写，就由钱包填写。如果二维码里payer有填写，钱包要验证是否与钱包的资产地址是一致的。

Provider 构造交易，用户签名，预执行交易，发送交易，POST 交易hash给callback url。

* 发送交易成功，POST给callback

```
{
  "action": "invoke",
  "id": "10ba038e-48da-487b-96e8-8d3b99b6d18a", 
  "error": 0,
  "desc": "SUCCESS",
  "result": "tx hash"
}
```

* 发送交易失败，POST给callback

```
{
  "action": "invoke",
  "id": "10ba038e-48da-487b-96e8-8d3b99b6d18a", 
  "error": 80001,
  "desc": "SEND TX ERROR",
  "result": 1
}
```




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

##### dApi
* [cyano-bridge](https://github.com/ontio-cyano/cyano-bridge)
* [dAPI for chrome](https://github.com/ontio/ontology-dapi)

##### dApi-mobile provider sdk
* [cyano-android-sdk](https://github.com/ontio-cyano/cyano-android-sdk)
* [cyano-ios-sdk](https://github.com/ontio-cyano/cyano-ios-sdk)
