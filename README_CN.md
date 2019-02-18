
[English](README.md) | 中文

# dapi-mobile使用场景说明

移动端dAPI规范文档包括唤醒、扫码、钱包中打开H5 DApp三种场景。已支持dAPI的钱包[麦子钱包](http://www.mathwallet.org/en/)、[Onion](http://onion.fun/),对接请参考对应的对接文档。协议详情请看[CEP1](https://github.com/ontio-cyano/CEPs/blob/master/CEPS/CEP1.mediawiki)。

参考钱包下载链接： http://101.132.193.149/files/app-debug.apk

钱包对接：


* [钱包对接-钱包打开DApp接入流程](cn/钱包对接-钱包打开DApp.md)
* [钱包对接-扫码接入流程](cn/钱包对接-扫码.md)
* [钱包对接-唤醒接入流程(暂不支持)](cn/钱包对接-唤醒(暂不支持).md)
* [钱包对接-ONTID认证授权(暂不支持)](cn/钱包对接-OntId认证授权(暂不支持).md)

DAPP对接：

* [DAPP对接-手机钱包打开DAPP](cn/DAPP对接-手机钱包打开DAPP.md)
* [DAPP对接-手机钱包扫码](cn/DAPP对接-手机钱包扫码.md)
* [DAPP对接-chrome插件钱包](cn/DAPP对接-chrome插件钱包.md)
* [DAPP对接-ONTID认证授权(暂不支持)](cn/DAPP对接-OntId认证授权(暂不支持).md)


## 场景1和2： 唤醒、扫码场景


##### 登录、调用智能合约

![](images/split-login-invoke.png)

##### 未登录时调用智能合约

![](images/invoke-with-login.png)


## 场景3： 钱包中打开H5 DApp

1. Open DApp in Provider
2. Get account or get identity
3. Login DApp
4. DApp Invoke smart contract

![](images/scenario3.png)

## 钱包演示

移动版Cyano钱包源码链接地址[cyano-android](https://github.com/ontio-cyano/cyano-android),[cyano-ios](https://github.com/ontio-cyano/cyano-ios)。

H5 DApp演示: [mobile-dapp-demo](https://github.com/ontio-cyano/mobile-dapp-demo)

### Open DApp in Provider

钱包中打开DApp：http://101.132.193.149:5000/#/

<div align="center">
  <img src="https://raw.githubusercontent.com/ontio-community/dapi-mobile-example/master/images/ios/01-open-dapp.jpg" height="350" width="200">
</div>

### Get account or get identity

DApp登录如果不需要验证用户身份，直接查询账号或身份信息：

<div align="center">
  <img src="https://raw.githubusercontent.com/ontio-community/dapi-mobile-example/master/images/ios/02-getAccount.jpg" height="350" width="200">
  <img src="https://raw.githubusercontent.com/ontio-community/dapi-mobile-example/master/images/ios/04-logined.jpg" height="350" width="200">
</div>

### Login DApp

DApp登录如果需要验证用户身份: DApp发消息到给钱包签名，DApp验证签名。

<div align="center">
  <img src="https://raw.githubusercontent.com/ontio-community/dapi-mobile-example/master/images/ios/03-login-message.jpg" height="350" width="200">
  <img src="https://raw.githubusercontent.com/ontio-community/dapi-mobile-example/master/images/ios/input-password.jpg" height="350" width="200">
  <img src="https://raw.githubusercontent.com/ontio-community/dapi-mobile-example/master/images/ios/04-logined.jpg" height="350" width="200">
</div>

### DApp Invoke smart contract

DApp调用合约，用户签名后预执行合约，用户确认并发送交易，返回交易hash给DAPP。

<div align="center">
  <img src="https://raw.githubusercontent.com/ontio-community/dapi-mobile-example/master/images/ios/05-invoke-message.jpg" height="350" width="200">
  <img src="https://raw.githubusercontent.com/ontio-community/dapi-mobile-example/master/images/ios/input-password.jpg" height="350" width="200">
  <img src="https://raw.githubusercontent.com/ontio-community/dapi-mobile-example/master/images/ios/05-pre-exec-result.jpg" height="350" width="200">
   <img src="https://raw.githubusercontent.com/ontio-community/dapi-mobile-example/master/images/ios/06-dapp-recv-txhash.jpg" height="350" width="200">
</div>

