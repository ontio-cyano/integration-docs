
English | [中文](README_CN.md)

# Docking instructions

The mobile dAPI specification document includes three scenarios: wake-up, scan code, and open H5 DApp in the wallet. Wallet that already supports dAPI [MATH](http://www.mathwallet.org/en/), [Onion](http://onion.fun/). For docking, please refer to the corresponding docking document. Please see the details in [CEP1](https://github.com/ontio-cyano/CEPs/blob/master/CEPS/CEP1.mediawiki).DAPPs that already support dAPI as [HyperDragon](https://hyd-go.alfakingdom.com/).

Reference wallet download link: http://101.132.193.149/files/app-debug.apk

Wallet docking:

1. Integration the ONT/ONG/OEP4 assets, need install the corresponding [SDK](http://dev-docs.ont.io/#/docs-cn/SDKs/00-overview) integrated asset function.
2. Before docking cyano-provider, please download [cyano-android-provider-sdk](https://github.com/ontio-cyano/cyano-android-sdk), [cyano-ios-provider-sdk]( Https://github.com/ontio-cyano/cyano-ios-sdk).
3. Docking cyano-provider detailed documentation:
* [Wallet docking - open DApp in wallet](en/WalletDocking-wallet-open-DApp_en.md)
* [Wallet docking - scan QR code](en/WalletDocking-scan-qrcode_en.md)

DAPP docking:
1. Please use [cyano-bridge](https://github.com/ontio-cyano/cyano-bridge) for mobile DAPP docking and [dAPI for chrome](https://github.com/) Ontio/ontology-dapi)  for chrome plugin .
2. Docking cyano-dapi detailed documentation:

* [DAPP docking - open DApp in wallet](en/DAppDocking-Wallet-Opens-DApp.md)
* [DAPP docking - QR code](en/DAppDocking-QRcode.md)
* [DAPP docking - use chrome extension wallet](en/DAppDocking-use%20chrome%20extension%20wallet.md)


## Scenario 1 and 2: Wake up, scan the QRcode


##### Login, call smart contract

![](images/split-login-invoke.png)

##### Call smart contract when not logged in

![](images/invoke-with-login.png)


## Scenario 3: Open H5 DApp in the wallet

1. Open DApp in Provider
2. Get account or get identity
3. Login DApp
4. DApp Invoke smart contract

![](images/scenario3.png)

## DEMO

Mobile version of Cyano wallet source link [cyano-android](https://github.com/ontio-cyano/cyano-android),[cyano-ios](https://github.com/ontio-cyano/cyano-ios)。

H5 DApp DEMO source link: [mobile-dapp-demo](https://github.com/ontio-cyano/mobile-dapp-demo)

### Open DApp in Wallet

Open DApp in wallet: http://101.132.193.149:5000/#/

<div align="center">
  <img src="https://raw.githubusercontent.com/ontio-cyano/integration-docs/master/images/ios/01-dapps.jpg" height="350" width="200">
  <img src="https://raw.githubusercontent.com/ontio-cyano/integration-docs/master/images/ios/01-private-dapp.jpg" height="350" width="200">
  <img src="https://raw.githubusercontent.com/ontio-cyano/integration-docs/master/images/ios/01-open-dapp.png" height="350" width="200">
</div>

### DApp Get account or get identity

DApp login If you do not need to verify the user identity, directly query the account or identity information:

<div align="center">
  <img src="https://raw.githubusercontent.com/ontio-cyano/integration-docs/master/images/ios/01-open-dapp.png" height="350" width="200">
  <img src="https://raw.githubusercontent.com/ontio-cyano/integration-docs/master/images/ios/02-getAccount.jpg" height="350" width="200">
</div>

### DApp Login

DApp login if you need to verify the user's identity: DApp sends a message to the wallet signature, DApp verification signature.

<div align="center">
  <img src="https://raw.githubusercontent.com/ontio-cyano/integration-docs/master/images/ios/01-open-dapp.png" height="350" width="200">
  <img src="https://raw.githubusercontent.com/ontio-cyano/integration-docs/master/images/ios/03-login-pwd.png" height="350" width="200">
  <img src="https://raw.githubusercontent.com/ontio-cyano/integration-docs/master/images/ios/04-logined.jpg" height="350" width="200">
</div>

### DApp Invoke smart contract

DApp calls the contract:
1. the user signs the pre-execution contract.
2. the user confirms and sends the transaction,.
3. returns the transaction hash to DAPP.

<div align="center">
  <img src="https://raw.githubusercontent.com/ontio-cyano/integration-docs/master/images/ios/input-password.jpg" height="350" width="200">
  <img src="https://raw.githubusercontent.com/ontio-cyano/integration-docs/master/images/ios/05-pre-exec-result.png" height="350" width="200">
  <img src="https://raw.githubusercontent.com/ontio-cyano/integration-docs/master/images/ios/06-dapp-recv-txhash.jpg" height="350" width="200">
</div>

