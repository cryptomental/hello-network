# Create an asset crowdsale

## What is an asset crowdsale?

In our "hello world" example, creating an asset crowdsale is the equivalent of setting up an account where everyone can deposit funds to later [finance our asset](https://developer.mybit.io/hello-network/fund-an-asset-crowdsale) i.e. coffee machine.  A crowdsale has a set of terms defined at its start such as:

* Crowdsale duration
* Funding required to be raised
* Operator responsible for delivering asset
* Account \(i.e. address\) to receive deposits

{% page-ref page="fund-an-asset-crowdsale.md" %}

## Starting a crowdsale for a coffee machine

To initiate the crowdsale, our application sets up an asynchronous `function startCrowdsale()` . Our application uses MyBit APIs and node.js network.js library to interact with the local instance of MyBit smart contracts deployed on the local blockchain.  We use await syntax to interact asynchronously with the blockchain.

Within our function our application will generate the asset ID, add an account \(i.e. token address\) associated with the asset and then actually create the asset including the terms for its funding. 

### Generate an asset ID

Assign ID variable and call the API to generate an asset id by passing the operator address, funding goal, the operator id and the asset URI. 

```javascript
var id = await api.generateAssetID(operatorAddress, 70000000000000000,
      operatorID, "CoffeeRun");
```

### Get asset crowdsale address

Creates a variable `token address` representing the asset account \(i.e. address\) and calls the API to get the asset address by passing the above mentioned variable `id` \(i.e. Asset ID\). 

```javascript
var tokenAddress = await api.getAssetAddress(id);
```

### Create asset and set asset crowdsale terms

First, our "hello world" application will do a validation on the asset address \(i.e. token address\) to verify if the asset crowdsale has already started. If not started, it will first get approval from the operator address to burn crowdsale tokens in case of negligence from operator or failure to finalise crowdsale within terms.   

```javascript
if (tokenAddress == '0x0000000000000000000000000000000000000000') {
      await Network.approveBurn(operatorAddress);
```

Then, the application will set a variable `response` and pass a `createAsset()` function and object with the following asset crowdsale terms as parameters in the object to **network.js**. 

```javascript
  var response = await Network.createAsset({
    assetURI: "CoffeeRun",
    operatorID: operatorID,
    fundingLength: 1000,
    amountToRaise: 70000000000000000, //about $20 CAD
    brokerPercent: 0,
    broker: operatorAddress //operator is also broker
  });  
```

The function returns a response object, representing the start of the start crowdsale, which contains \_assetID, \_assetOperator, \_assetURI, and \_tokenAddress.

```javascript
    return response;
```

Alternatively, in case an asset crowdsale has already been initiated, our application will return the following message:

```javascript
'Crowdsale already started'
```



## 





