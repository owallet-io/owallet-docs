---
description: Guide to adding new blockchain to have OWallet support
---

# Integration

## Adding Cosmos-based blockchains

### Chain config

| Property            | Type                                                                                                                                                                                                                |                                                                                                                   Function |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------: |
| rpc                 | `string`                                                                                                                                                                                                            |                                                                                                        RPC of a blockchain |
| rest                | `string`                                                                                                                                                                                                            |                                                                                                        LCD of a blockchain |
| chainId             | `string`                                                                                                                                                                                                            |                                                                                                                   Chain ID |
| chainName           | `string`                                                                                                                                                                                                            |                                                                                                                 Chain Name |
| networkType         | `string`                                                                                                                                                                                                            | Network Type `("cosmos" or "evm")`: To declare whether the network is Cosmos-based or Ethereum Virtual Machine (EVM)-based |
| stakeCurrency       | <p><code>{</code></p><p><code>coinDenom: string, coinMinimalDenom: string, coinDecimals: number, coinGeckoId: string, coinImageUrl: string, gasPriceStep: { low: number, average: number, high: number}}</code></p> |                                                                                                      Native stake currency |
| bip44               | `{ coinType: number}`                                                                                                                                                                                               |                                                                                                               Bip44 config |
| coinType            | `number`                                                                                                                                                                                                            |                                                                        The coin type is usually 118 for Cosmos, 60 for EVM |
| bech32Config        | `Bech32Address.defaultBech32Config(string)`                                                                                                                                                                         |                                                                                                  Config for bech32 address |
| currencies          | `Array<Currency>`                                                                                                                                                                                                   |                                                                                                    Currencies of the chain |
| feeCurrencies       | `Array<Currency>`                                                                                                                                                                                                   |                                                                                                Fee currencies of the chain |
| features            | `Array<Currency>`                                                                                                                                                                                                   |                                              To declare what features this chain have`(ex: ["ibc-transfer", "cosmwasm")])` |
| chainSymbolImageUrl | `string`                                                                                                                                                                                                            |                                                                                                     Chain symbol image URL |
| txExplorer          | `{name: string, txUrl: string, accountUrl: string}`                                                                                                                                                                 |                                                                                                Transaction explorer config |

### How to add a chain into OWallet?

{% hint style="info" %}
**If your chain needs to use special packages, please consider taking a look at the** [**System Desgin** ](broken-reference)**section to learn how to implement your chain into OWallet**
{% endhint %}

1. Clone this repo to desired directory

```shell
git clone https://github.com/oraichain/owallet
```

2. Checkout to main

```shell
git checkout main
```

3. Checkout to new branch

```shell
git checkout -b feat/add-new-chain-config
```

4. Create PR into main

### Example

````typescript
```typescript
{
    rpc: "https://rpc.orai.io",
    rest: "https://lcd.orai.io",
    chainId: "Oraichain",
    chainName: "Oraichain",
    networkType: "cosmos",
    stakeCurrency: {
      coinDenom: "ORAI",
      coinMinimalDenom: "orai",
      coinDecimals: 6,
      coinGeckoId: "oraichain-token",
      coinImageUrl:
        "https://s2.coinmarketcap.com/static/img/coins/64x64/7533.png",
      gasPriceStep: {
        low: 0.003,
        average: 0.005,
        high: 0.007,
      },
    },
    bip44: {
      coinType: 118,
    },
    coinType: 118,
    bech32Config: Bech32Address.defaultBech32Config("orai"),
    get currencies() {
      return [
        this.stakeCurrency,
        {
          type: "cw20",
          coinDenom: "AIRI",
          coinMinimalDenom:
            "cw20:orai10ldgzued6zjp0mkqwsv2mux3ml50l97c74x8sg:aiRight Token",
          contractAddress: "orai10ldgzued6zjp0mkqwsv2mux3ml50l97c74x8sg",
          coinDecimals: 6,
          coinGeckoId: "airight",
          coinImageUrl: "https://i.ibb.co/m8mCyMr/airi.png",
        },

        {
          type: "cw20",
          coinDenom: "OCH",
          coinMinimalDenom:
            "cw20:orai1hn8w33cqvysun2aujk5sv33tku4pgcxhhnsxmvnkfvdxagcx0p8qa4l98q:OCH",
          contractAddress:
            "orai1hn8w33cqvysun2aujk5sv33tku4pgcxhhnsxmvnkfvdxagcx0p8qa4l98q",
          coinDecimals: 6,
          coinGeckoId: "och",
          coinImageUrl:
            "https://assets.coingecko.com/coins/images/34236/standard/orchai_logo_white_copy_4x-8_%281%29.png",
        },
        {
          type: "cw20",
          coinDenom: "tBTC",
          coinMinimalDenom:
            "cw20:orai1d2hq8pzf0nswlqhhng95hkfnmgutpmz6g8hd8q7ec9q9pj6t3r2q7vc646:tBTC Token",
          contractAddress:
            "orai1d2hq8pzf0nswlqhhng95hkfnmgutpmz6g8hd8q7ec9q9pj6t3r2q7vc646",
          coinDecimals: 6,
          coinGeckoId: "bitcoin",
          coinImageUrl: "https://i.ibb.co/NVP6CDZ/images-removebg-preview.png",
        },
        {
          type: "cw20",
          coinDenom: "BTC",
          coinMinimalDenom:
            "cw20:orai10g6frpysmdgw5tdqke47als6f97aqmr8s3cljsvjce4n5enjftcqtamzsd:orai BTC Token",
          contractAddress:
            "orai10g6frpysmdgw5tdqke47als6f97aqmr8s3cljsvjce4n5enjftcqtamzsd",
          coinDecimals: 6,
          coinGeckoId: "bitcoin",
          coinImageUrl: "https://i.ibb.co/NVP6CDZ/images-removebg-preview.png",
        },
        {
          type: "cw20",
          coinDenom: "ORAIX",
          coinMinimalDenom:
            "cw20:orai1lus0f0rhx8s03gdllx2n6vhkmf0536dv57wfge:OraiDex Token",
          contractAddress: "orai1lus0f0rhx8s03gdllx2n6vhkmf0536dv57wfge",
          coinDecimals: 6,
          coinGeckoId: "oraidex",
          coinImageUrl: "https://i.ibb.co/VmMJtf7/oraix.png",
        },
        {
          type: "cw20",
          coinDenom: "USDT",
          coinMinimalDenom:
            "cw20:orai12hzjxfh77wl572gdzct2fxv2arxcwh6gykc7qh:Tether",
          contractAddress: "orai12hzjxfh77wl572gdzct2fxv2arxcwh6gykc7qh",
          coinDecimals: 6,
          coinGeckoId: "tether",
          coinImageUrl:
            "https://s2.coinmarketcap.com/static/img/coins/64x64/825.png",
        },
        {
          type: "cw20",
          coinDenom: "USDC",
          coinMinimalDenom:
            "cw20:orai15un8msx3n5zf9ahlxmfeqd2kwa5wm0nrpxer304m9nd5q6qq0g6sku5pdd:USDC",
          contractAddress:
            "orai15un8msx3n5zf9ahlxmfeqd2kwa5wm0nrpxer304m9nd5q6qq0g6sku5pdd",
          coinDecimals: 6,
          coinGeckoId: "usd-coin",
          coinImageUrl:
            "https://s2.coinmarketcap.com/static/img/coins/64x64/3408.png",
        },
        {
          type: "cw20",
          coinDenom: "wTRX",
          coinMinimalDenom:
            "cw20:orai1c7tpjenafvgjtgm9aqwm7afnke6c56hpdms8jc6md40xs3ugd0es5encn0:wTRX",
          contractAddress:
            "orai1c7tpjenafvgjtgm9aqwm7afnke6c56hpdms8jc6md40xs3ugd0es5encn0",
          coinDecimals: 6,
          coinGeckoId: "tron",
          coinImageUrl:
            "https://s2.coinmarketcap.com/static/img/coins/64x64/1958.png",
        },
        {
          type: "cw20",
          coinDenom: "INJ",
          coinMinimalDenom:
            "cw20:orai19rtmkk6sn4tppvjmp5d5zj6gfsdykrl5rw2euu5gwur3luheuuusesqn49:INJ",
          contractAddress:
            "orai19rtmkk6sn4tppvjmp5d5zj6gfsdykrl5rw2euu5gwur3luheuuusesqn49",
          coinDecimals: 6,
          coinGeckoId: "injective-protocol",
          coinImageUrl:
            "https://s2.coinmarketcap.com/static/img/coins/64x64/7226.png",
        },
        {
          type: "cw20",
          coinDenom: "KWT",
          coinMinimalDenom:
            "cw20:orai1nd4r053e3kgedgld2ymen8l9yrw8xpjyaal7j5:Kawaii Islands",
          contractAddress: "orai1nd4r053e3kgedgld2ymen8l9yrw8xpjyaal7j5",
          coinDecimals: 6,
          coinGeckoId: "kawaii-islands",
          coinImageUrl:
            "https://s2.coinmarketcap.com/static/img/coins/64x64/12313.png",
        },
        {
          type: "cw20",
          coinDenom: "MILKY",
          coinMinimalDenom:
            "cw20:orai1gzvndtzceqwfymu2kqhta2jn6gmzxvzqwdgvjw:Milky Token",
          contractAddress: "orai1gzvndtzceqwfymu2kqhta2jn6gmzxvzqwdgvjw",
          coinDecimals: 6,
          coinGeckoId: "milky-token",
          coinImageUrl:
            "https://s2.coinmarketcap.com/static/img/coins/64x64/14418.png",
        },
        {
          coinDenom: "WETH",
          coinGeckoId: "weth",
          coinMinimalDenom:
            "cw20:orai1dqa52a7hxxuv8ghe7q5v0s36ra0cthea960q2cukznleqhk0wpnshfegez:WETH",
          type: "cw20",
          contractAddress:
            "orai1dqa52a7hxxuv8ghe7q5v0s36ra0cthea960q2cukznleqhk0wpnshfegez",
          coinDecimals: 6,
          coinImageUrl:
            "https://s2.coinmarketcap.com/static/img/coins/64x64/1027.png",
        },
      ];
    },
    get feeCurrencies() {
      return [this.stakeCurrency];
    },
    features: ["stargate", "ibc-transfer", "cosmwasm", "no-legacy-stdTx"],
    chainSymbolImageUrl: "https://orai.io/images/logos/logomark-dark.png",
    txExplorer: {
      name: "Oraiscan",
      txUrl: "https://scan.orai.io/txs/{txHash}",
      accountUrl: "https://scan.orai.io/account/{address}",
    },
    // beta: true // use v1beta1
  }
```
````

## Adding EVM-based blockchains

### Chain config

| Property            |                                                                                      Type                                                                                     |                                                                                                                   Function |
| ------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | -------------------------------------------------------------------------------------------------------------------------: |
| rpc                 |                                                                                    `string`                                                                                   |                                                                                                        RPC of a blockchain |
| chainId             |                                                                                    `string`                                                                                   |                                                                                                                   Chain ID |
| chainName           |                                                                                    `string`                                                                                   |                                                                                                                 Chain Name |
| networkType         |                                                                                    `string`                                                                                   | Network Type `("cosmos" or "evm")`: To declare whether the network is Cosmos-based or Ethereum Virtual Machine (EVM)-based |
| stakeCurrency       | `{coinDenom: string, coinMinimalDenom: string, coinDecimals: number, coinGeckoId: string, coinImageUrl: string, gasPriceStep: { low: number, average: number, high: number}}` |                                                                                                      Native stake currency |
| bip44               |                                                                             `{ coinType: number}`                                                                             |                                                                                                               Bip44 config |
| coinType            |                                                                                    `number`                                                                                   |                                                                        The coin type is usually 118 for Cosmos, 60 for EVM |
| bech32Config        |                                                                  `Bech32Address.defaultBech32Config(string)`                                                                  |                                                                                                  Config for bech32 address |
| currencies          |                                                                               `Array<Currency>`                                                                               |                                                                                                    Currencies of the chain |
| feeCurrencies       |                                                                               `Array<Currency>`                                                                               |                                                                                                Fee currencies of the chain |
| features            |                                                                               `Array<Currency>`                                                                               |                                                                 To declare what features this chain have`(ex: ["isEVM")])` |
| chainSymbolImageUrl |                                                                                    `string`                                                                                   |                                                                                                     Chain symbol image URL |
| txExplorer          |                                                              `{name: string, txUrl: string, accountUrl: string}`                                                              |                                                                                                Transaction explorer config |

### How to add a chain into OWallet?

1. Clone this repo to desired directory

```shell
git clone https://github.com/oraichain/owallet
```

2. Checkout to main

```shell
git checkout main
```

3. Checkout to new branch

```shell
git checkout -b feat/add-new-chain-config
```

4. Create PR into main

**If your chain needs to use special packages, please consider taking a look at the** [**System Desgin** ](broken-reference)**section to learn how to implement your chain into OWallet**

### Example

```shell
{
    rpc: "https://rpc.ankr.com/eth",
    rest: "https://rpc.ankr.com/eth",
    chainId: "0x01",
    chainName: "Ethereum",
    bip44: {
      coinType: 60,
    },
    coinType: 60,
    stakeCurrency: {
      coinDenom: "ETH",
      coinMinimalDenom: "eth",
      coinDecimals: 18,
      coinGeckoId: "ethereum",
      coinImageUrl:
        "https://s2.coinmarketcap.com/static/img/coins/64x64/1027.png",
      gasPriceStep: {
        low: 1,
        average: 1.25,
        high: 1.5,
      },
    },
    chainSymbolImageUrl:
      "https://s2.coinmarketcap.com/static/img/coins/64x64/1027.png",
    bech32Config: Bech32Address.defaultBech32Config("evmos"),
    networkType: "evm",
    currencies: [
      {
        coinDenom: "ETH",
        coinMinimalDenom: "eth",
        coinDecimals: 18,
        coinGeckoId: "ethereum",
        coinImageUrl:
          "https://s2.coinmarketcap.com/static/img/coins/64x64/1027.png",
      },
      {
        coinDenom: "OCH",
        coinMinimalDenom:
          "erc20:0x19373EcBB4B8cC2253D70F2a246fa299303227Ba:OCH Token",
        contractAddress: "0x19373EcBB4B8cC2253D70F2a246fa299303227Ba",
        coinDecimals: 18,
        coinGeckoId: "och",
        coinImageUrl:
          "https://assets.coingecko.com/coins/images/34236/standard/orchai_logo_white_copy_4x-8_%281%29.png",
      },
      {
        coinDenom: "ORAI",
        coinMinimalDenom:
          "erc20:0x4c11249814f11b9346808179cf06e71ac328c1b5:Oraichain Token",
        contractAddress: "0x4c11249814f11b9346808179cf06e71ac328c1b5",
        coinDecimals: 18,
        coinGeckoId: "oraichain-token",
        coinImageUrl:
          "https://s2.coinmarketcap.com/static/img/coins/64x64/7533.png",
      },
      {
        coinDenom: "ORAIX",
        coinMinimalDenom:
          "erc20:0x2d869aE129e308F94Cc47E66eaefb448CEe0d03e:ORAIX Token",
        contractAddress: "0x2d869aE129e308F94Cc47E66eaefb448CEe0d03e",
        coinDecimals: 18,
        coinGeckoId: "oraidex",
        coinImageUrl: "https://i.ibb.co/VmMJtf7/oraix.png",
      },
    ],
    get feeCurrencies() {
      return [this.stakeCurrency];
    },

    features: ["ibc-go", "stargate", "isEvm"],
    txExplorer: {
      name: "Etherscan",
      txUrl: "https://etherscan.io/tx/{txHash}",
      accountUrl: "https://etherscan.io/address/{address}",
    },
  }
```
