<h1 align="center">DApp Docking - Wallet Opens DApp</h1>


## The difference between the dApi of mobile version and the dApi of chrome extension

The dApi of mobile version only provides several important interfaces. Query related interfaces can directly call the query API of blockchain browser, [explorer api](http://dev-docs.ont.io/#/docs-en/explorer/overview).

The usage of the dApi of mobile version: [cyano-dapi-mobile](https://github.com/ontio-cyano/cyano-bridge)

The usage of the dApi of chrome extension: [cyano-dapi](https://github.com/ontio/ontology-dapi)

### dApi interface initialization 

Registration is required for the version of chrome extension and is not required for mobile version.


#### Mobile version
```
import { client } from 'cyanobridge'
client.registerClient();

```

#### Chrome extension
```
import {client} from 'ontology-dapi'
client.registerClient({})

```

### Get account or identity

For getting account or identity, the mobile terminal can choose to fill in DApp information or not.


#### Mobile version

```
import { client } from 'cyanobridge'

const params = {
​    dappName: 'My dapp',
​    dappIcon: '' // some url points to the dapp icon
}

try {
​    const res = await client.api.asset.getAccount(params);
    const res = await client.api.identity.getIdentity(params);
​    console.log(res)
} catch(err) {
​    console.log(err)
}

```


#### Chrome extension
```
account = await client.api.asset.getAccount()
res = await client.api.identity.getIdentity();
```

### Login

Login is a signature by wallet and DApp verifies the signature.


#### Mobile version

```
const params = {
​    type: 'account',// account or identity that will sign the message
​    dappName: 'My dapp', // dapp's name
​    dappIcon: 'http://mydapp.com/icon.png', // some url that points to the dapp's icon
​    message: 'test message', // message sent from dapp that will be signed by native client
​    expired: new Date('2019-01-01').getTime(), // expired date of login
​    callback: '' // callback url of dapp
}
let res;
try {
​    res = await client.api.message.login(params);
​    console.log(res)
}catch(err) {
​    console.log(err)
}
```

#### Chrome extension
```
const result = await client.api.message.signMessage({ message });
```

### Invoke a smart contract


#### Mobile version


```
const scriptHash = 'cd948340ffcf11d4f5494140c93885583110f3e9';
const operation = 'test'
const args = [
​    {
​        type: 'String',
​        value: 'helloworld'
​    }
]
const gasPrice = 500;
const gasLimit = 20000;
const payer = 'AecaeSEBkt5GcBCxwz1F41TvdjX3dnKBkJ'
const config = {
​    "login": true,
​    "message": "invoke smart contract test",
​    "qrcodeUrl": "" ,
    "callback": ""
}
const params = {
          scriptHash,
          operation,
          args,
          gasPrice,
          gasLimit,
          payer,
          config
        }
try {
   const res = await client.api.smartContract.invoke(params);
   } catch(err) {
​    console.log(err)
}

```

#### Chrome extension
````
const contract = '16edbe366d1337eb510c2ff61099424c94aeef02';
const gasLimit = 30000;
const gasPrice = 500;

parameters = [
   {
	name: "msg",
	type: "String",
	value: "hello world"
   } 
]
 params = {
            contract,
            method,
            parameters,
            gasPrice,
            gasLimit
}
await client.api.smartContract.invoke(params)

```
