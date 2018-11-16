  **原文链接**：[https://github.com/bitshares/bitshares-ui/wiki/Gateway-Integration-Requirements](https://github.com/bitshares/bitshares-ui/wiki/Gateway-Integration-Requirements)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**： 

***  
  
### **<center>PLEASE BE AWARE THAT THIS IS A WORK IN PROGRESS AND CURRENTLY NEEDS VALIDATION</center>**

# Gateway Integration

This document will describe how an exchange can integrate their services to the Bitshares UI Reference Wallet in a simple and controlled manner.
The exchange has to provide a few API calls, where this document will go through the requirements of these.

## Required API calls

There are five API calls that are required.

### 1. Available Coins

Returns a JSON array containing all assets and backed assets.

For example, if the gateway exchange supports Bitcoin, lets call them DAC.xxx assets, it should include the following two entries.

#### NOTES
The walletSymbol on the real asset needs to have the same name as the backing asset name. Example: `walletSymbol: BTC` corresponds to the backed asset `DAC.BTC` 

```
object = {
    coinType: "dac.btc"
    walletName: null
    name: "Exchange Backed BTC"
    symbol: "DAC.BTC"
    walletSymbol: "DAC.BTC"
    walletType: null
    transactionFee: "0"
    precision: "100000000"
    supportsOutputMemos: false
    restricted: false
    authorized: null
    notAuthorizedReasons: null
    backingCoinType: "btc"
    gateFee: "0.000000"
    minAmount: "0.003"
    maxAmount: "999999999"
    intermediateAccount: "dac-dex"
    coinPriora: 0
    maintenanceReason: null
}

object = {
    coinType: "btc"
    walletName: "BTC"
    name: "BTC"
    symbol: "BTC"
    walletSymbol: "BTC"
    walletType: "btc"
    transactionFee: "0"
    precision: "100000000"
    supportsOutputMemos: false
    restricted: false
    authorized: null
    notAuthorizedReasons: null
    backingCoinType: null
    gateFee: "0.000900"
    minAmount: "0.00000001"
    maxAmount: "999999999"
    intermediateAccount: "dac-dex"
    coinPriora: 0
    maintenanceReason: ""
}
```

### 2. Available Tradeable Assets

The second API call we require is a list of corresponding tradeable assets. This should also return one entry for the coin and the backing coin.

```
object = {
    inputCoinType: "dac.btc"
    outputCoinType: "btc"
    rateFee: "0."
} 

object = {
    inputCoinType: "btc"
    outputCoinType: "dac.btc"
    rateFee: "0."
}
```

### 3. Available Wallets

The third API call we require is a list of available coin wallets. This will make sure that only assets with a `walletType` entry available in this list is enabled. This makes the gateway exchange disable wallets and make sure users can't deposit or withdraw assets during maintenance periods.

```
["btc","muse","incnt","esc","xdrac"]
```

### 4. Address Validation  

The fourth API call required is an address validation

The address should be situated at the address of `/wallets/<walletType>/address-validator?address=<address>`

- `walletType` element is the walletType from the coins list.
- `address` element is the supplied user address the UI required validation.

The response should return the following if the address is valid

```
isValid: true
```


### 5. Address Generator

The fifth API call required is an address generator

This address should be situated at the address of `/simple-api/initiate-trade` and accept a POST query.

The POST query will be the following format

```
inputCoinType: html
outputAddress: userBitSharesAccountName
outputCoinType: open.html
```

The response should return a coin wallet address for the user to deposit to. If a memo should be used, this should be included as well.

```
{
  "inputAddress": "HoRNZ6opWQ4P9vAADn3cufNWf3yDPQD8m3",
  "inputMemo": null,
  "inputCoinType": "html",
  "outputAddress": "userBisSharesAccountName",
  "outputCoinType": "open.html",
  "refundAddress": null,
  "comment": ""
}
```
### 6. Withdrawl Memo
When a user initiates a withdrawl, the wallet will generate a TX with a memo containing a string with the selected asset and the users address, example `btc:<my-address>`. 

If `useFullAssetName` is set for the gateway, the selected asset will also contain the gateway name, example `brdige.btc:<address>`

## Configuring Exchange Gateway

### apiConfig.js

This file contains the API call address.

Lets assume we wish to add a new exchange called "DAC".

``` 
export const dacAPIs = {
    BASE: "https://api.dacexchange.info/api/v1",
    COINS_LIST: "/coins",
    ACTIVE_WALLETS: "/active-wallets",
    TRADING_PAIRS: "/trading-pairs",
};
```

We then add it to the gateways.js file as the following

- Object name and `id` should always be the same and the same as the backed asset prepend name.
- `name` can be the full name
- `baseAPI` is the same as the const in apiConfig.js
- `isEnabled` can be set to false to disable the gateway
- `selected` is deprecated, and can be ignored
- `options{}` is visual placeholders and will be changed on runtime.

```
    DAC: {
        id: "DAC",
        name: "DAC Exchange Gateway",
        baseAPI: dacAPIs,
        isEnabled: true,
        selected: false,
        options: {
            enabled: false,
            selected: false
        },
    },
```

------

# How Gateway Integration works in the Reference Wallet

The following documentation goes in to the dept of how the code handles the various API calls in previous section of this document. This section is intended for development and integration understandings.

## Generating the available Backed Assets

By the various APIs the reference wallet builds up a list of assets that an be deposited or withdrawn through the Gateway Exchange service provided. It is up to this service provider to supply the correct responses to make sure this list stay accurate and valid.

### 1. Queries

We query the `/coins`, `/trading-pairs` and `/active-wallets` APIs. 

1. Build an array of `tradeableCoins[inputCoinType][outputCoinType]`
2. Itteriate all coins and check if we can deposit/withdraw them
- a. If `tradeableCoins[coin.backingCoinType][coin.coinType]` exists, deposits are enabled
- b. If `tradeableCoins[coin.coinType][coin.backingCoinType]` exists, withdrawls are enabled
3. Itteriate trough all backed coins and check
- a. That the coin has the backers name (DAC.xxx)
- b. That the coin has the backingCoinType string
- c. That is has a corresponding asset friend (ie. DAC.BTC required BTC to be present)
4. Ensure that the walletType is set and the name is present in the `/active-wallets` list.
- This sets the `isAvailable: true` flag

If all conditions are valid the asset will be pushed to the backed asset array as the following object structure.

```
object = {
    "name":"Bitcoin",
    "intermediateAccount":"dac-dex",
    "gateFee":"0.000000",
    "walletType":"btc",
    "backingCoinType":"bitcoin",
    "symbol":"DAC.BTC",
    "minAmount": "0.003"
    "maxAmount": "999999999"
    "supportsMemos":true,
    "depositAllowed":true,
    "withdrawalAllowed":true,
    "isAvailable":true
}
``` 

# Other Cases

There are other cases where gateways have been integrated in a "lighter" fashion or with a few tweaks to work.

### RUDEX
Rudex is implemented trough a "simple" system, only requiring one API call. The API returns all details in a single push, this limits the UI and is also the reason why the Backed Asset object for RuDex looks a little different.

### Crypto Bridge
Crypto Bridge is implemented without the requirements of `/active-wallets`, limiting the UI to be able to sense when an asset isn't possible to deposit/withdraw.

### BlockTrades
BlockTrades is implemented separately through a `fetchBackingAsset()` call, but uses the same techniques used by the Gateway Exchanges.