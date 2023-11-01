# JavaScript를 이용한 NFTs 발행 및 소각(Mint and Burn NFTs Using JavaScript)

이 예제는 다음을 보여줍니다:

1. 새로운 Non-fungible Tokens (NFTs) 발행하기.
2. 기존 NFTs의 목록을 얻는 방법.
3. NFT를 삭제(소각)하는 방법.

<figure><img src="https://xrpl.org/img/quickstart8.png" alt=""><figcaption></figcaption></figure>

## 사용법

[Quickstart Samples](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/quickstart/js/) 아카이브를 다운로드하여 각 샘플을 브라우저에서 시도해볼 수 있습니다.

## 계정 받기&#x20;

1. 브라우저에서 `3.mint-nfts.html`을 엽니다.&#x20;
2. 테스트 계정을 가져옵니다.
   1. 계정 seed가 있는 경우&#x20;
      1. **Seeds** 필드에 계정 seed를 붙여넣습니다.
      2. **Get Accounts from Seeds**를 클릭합니다
   2. 계정 seed가 없는 경우
      1. **Get New Standby Account**를 클릭합니다.
      2. **Get New Operational Account**를 클릭합니다.

<figure><img src="https://xrpl.org/img/quickstart9.png" alt=""><figcaption></figcaption></figure>

## NFT 발행하기&#x20;

{% embed url="https://youtu.be/Qyb_x_GlUDg" %}

non-fungible token 객체를 발행하려면:

1. Flags 필드를 설정합니다. 테스트 목적으로는 이 값을 _8_로 설정하는 것을 권장합니다. 이는 _tsTransferable_ 플래그를 설정하며, 이는 NFT 객체를 다른 계정으로 전송할 수 있음을 의미합니다. 그렇지 않으면, NFT 객체는 발행 계정으로만 전송할 수 있습니다. NFT 발행에 사용할 수 있는 모든 플래그에 대한 정보는 [NFToken Mint](https://xrpl.org/nftokenmint.html)를 참조하십시오.&#x20;
2. Token URL을 입력합니다. 이는 NFT 객체와 연결된 데이터 또는 메타데이터를 가리키는 URI입니다. 자신의 URI가 없다면 제공된 샘플 URI를 사용할 수 있습니다.&#x20;
3. Transfer Fee를 입력합니다. 이는 NFT의 향후 판매 수익의 일정 비율이 원래의 창작자에게 반환될 것임을 의미합니다. 이는 0-50000 사이의 값으로, 전송 비율을 0.000%에서 50.000%까지 0.001%의 증분으로 허용합니다. NFT가 전송 가능하도록 Flags 필드를 설정하지 않는다면, 이 필드를 0으로 설정합니다.&#x20;
4. Mint NFT를 클릭합니다.

<figure><img src="https://xrpl.org/img/quickstart10.png" alt=""><figcaption></figcaption></figure>

## 토큰 받기&#x20;

**Get NFTs**를 클릭하여 계정이 소유한 NFTs의 목록을 얻습니다.

<figure><img src="https://xrpl.org/img/quickstart11.png" alt=""><figcaption></figcaption></figure>

## 토큰 소각하기&#x20;

NFT의 현재 소유자는 언제나 NFT 객체를 파괴(또는 소각)할 수 있습니다.

NFT를 영구히 파괴하는 방법:

1. Token ID를 입력합니다.&#x20;
2. Burn NFT를 클릭합니다.

<figure><img src="https://xrpl.org/img/quickstart12.png" alt=""><figcaption></figcaption></figure>

## Code Walkthrough <a href="#code-walkthrough" id="code-walkthrough"></a>

[Quickstart Samples](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/quickstart/js/) 아카이브를 다운로드하여 각 샘플을 브라우저에서 시도해볼 수 있습니다.

### ripplex3-mint-nfts.js <a href="#ripplex3-mint-nftsjs" id="ripplex3-mint-nftsjs"></a>

### Mint Token <a href="#mint-token" id="mint-token"></a>

```javascript
// *******************************************************
// ********************** Mint Token *********************
// *******************************************************

async function mintToken() {
```

ledger에 연결해서 계정지갑을 가져오세요.

```javascript
  results = 'Connecting to ' + getNet() + '....'
  standbyResultField.value = results
  let net = getNet()
  const standby_wallet = xrpl.Wallet.fromSeed(standbySeedField.value)
  const client = new xrpl.Client(net)
  await client.connect()
  results += '\nConnected. Minting NFT.'
  standbyResultField.value = results
```

트랜잭션을 정의하세요.

```javascript
  const transactionJson = {
    "TransactionType": "NFTokenMint",
    "Account": standby_wallet.classicAddress,
```

URI 필드에는 리터럴 URI 문자열이 아닌 16진수 값이 필요합니다. `convertStringToHex`유틸리티를 사용하여 URI를 실시간으로 변환할 수 있습니다.

```javascript
    "URI": xrpl.convertStringToHex(standbyTokenUrlField.value),
```

타사에 NFT를 전송하려면 _플래그_ 필드를 8로 설정합니다.

```javascript
    "Flags": parseInt(standbyFlagsField.value),
```

트랜잭션 수수료는 0 \~ 50000의 값이며, 로열티를 0.001씩 0.000% \~ 50.000%로 설정하는 데 사용됩니다.

```javascript
    "TransferFee": parseInt(standbyTransferFeeField.value),
```

`TokenTaxon`은 필수 값입니다. 발행자가 정의한 임의의 값입니다. 필드를 사용하지 않는 경우 필드를 _0_으로 설정할 수 있습니다.

```javascript
    "NFTokenTaxon": 0 //Required, but if you have no use for it, set to zero.
  }
```

트랜잭션을 보내고 응답을 기다립니다.

```javascript
  const tx = await client.submitAndWait(transactionJson, { wallet: standby_wallet} )
```

계정이 소유한 NFT 목록을 요청합니다.

```javascript
  const nfts = await client.request({
    method: "account_nfts",
    account: standby_wallet.classicAddress
  })
```

결과를 보고합니다.

```javascript
  results += '\n\nTransaction result: '+ tx.result.meta.TransactionResult
  results += '\n\nnfts: ' + JSON.stringify(nfts, null, 2)
  standbyBalanceField.value = (await client.getXrpBalance(standby_wallet.address))
  standbyResultField.value = results    
```

ledger에서 연결을 끊습니다.

```javascript
  client.disconnect()
} //End of mintToken()
```

### Get Tokens <a href="#get-tokens-1" id="get-tokens-1"></a>

```javascript
// *******************************************************
// ******************* Get Tokens ************************
// *******************************************************

async function getTokens() {
```

ledger에 연결해 계정을 가져오세요.

```javascript
  const standby_wallet = xrpl.Wallet.fromSeed(standbySeedField.value)
  let net = getNet()
  const client = new xrpl.Client(net)
  results = 'Connecting to ' + net + '...'
  standbyResultField.value = results
  await client.connect()
  results += '\nConnected. Getting NFTs...'
  standbyResultField.value = results
```

계정이 소유한 NFT 목록을 요청합니다.

```javascript
  const nfts = await client.request({
    method: "account_nfts",
    account: standby_wallet.classicAddress
  })
```

결과를 보고합니다.

```javascript
  results += '\nNFTs:\n ' + JSON.stringify(nfts,null,2)
  standbyResultField.value = results
```

ledger에서 연결을 끊습니다.

```javascript
  client.disconnect()
} //End of getTokens()
```

### Burn Token <a href="#burn-token" id="burn-token"></a>

```javascript
// *******************************************************
// ********************* Burn Token **********************
// *******************************************************

async function burnToken() {
```

ledger에 연결해서 계정 지갑을 가져오세요.

```javascript
  const standby_wallet = xrpl.Wallet.fromSeed(standbySeedField.value)
  let net = getNet()
  const client = new xrpl.Client(net)
  results = 'Connecting to ' + net + '...'
  standbyResultField.value = results
  await client.connect()
  results += '\nConnected. Burning NFT...'
  standbyResultField.value = results
```

트랜잭션을 정의합니다.

```javascript
  const transactionBlob = {
    "TransactionType": "NFTokenBurn",
    "Account": standby_wallet.classicAddress,
    "NFTokenID": standbyTokenIdField.value
  }
```

트랜잭션을 제출하고 결과를 기다립니다.

```javascript
  const tx = await client.submitAndWait(transactionBlob,{wallet: standby_wallet})
```

계정이 소유한 NFT 목록을 요청합니다.

```javascript
  const nfts = await client.request({
    method: "account_nfts",
    account: standby_wallet.classicAddress
  })
```

결과를 보고합니다.

```javascript
  results += '\nTransaction result: '+ tx.result.meta.TransactionResult
  results += '\nBalance changes: ' +
  JSON.stringify(xrpl.getBalanceChanges(tx.result.meta), null, 2)
  standbyResultField.value = results
  standbyBalanceField.value = (await client.getXrpBalance(standby_wallet.address))
  results += '\nNFTs: \n' + JSON.stringify(nfts,null,2)
  standbyResultField.value = results
  client.disconnect()
}// End of burnToken()
```

### Reciprocal Transactions <a href="#reciprocal-transactions" id="reciprocal-transactions"></a>

이러한 트랜잭션은  Standby account 트랜잭션과 동일하지만 Operational account에 대한 트랜잭션입니다.

```javascript
// *******************************************************
// ************** Operational Mint Token *****************
// *******************************************************

async function oPmintToken() {
  results = 'Connecting to ' + getNet() + '....'
  operationalResultField.value = results
  let net = getNet()
  const operational_wallet = xrpl.Wallet.fromSeed(operationalSeedField.value)
  const client = new xrpl.Client(net)
  await client.connect()
  results += '\nConnected. Minting NFT.'
  operationalResultField.value = results

  // Note that you must convert the token URL to a hexadecimal 
  // value for this transaction.
  // ------------------------------------------------------------------------
  const transactionBlob = {
    "TransactionType": 'NFTokenMint',
    "Account": operational_wallet.classicAddress,
    "URI": xrpl.convertStringToHex(operationalTokenUrlField.value),
    "Flags": parseInt(operationalFlagsField.value),
    "TransferFee": parseInt(operationalTransferFeeField.value),
    "NFTokenTaxon": 0 //Required, but if you have no use for it, set to zero.
  }

  // ----------------------------------------------------- Submit signed blob 
  const tx = await client.submitAndWait(transactionBlob, { wallet: operational_wallet} )
  const nfts = await client.request({
    method: "account_nfts",
    account: operational_wallet.classicAddress
  })

  // ------------------------------------------------------- Report results
  results += '\n\nTransaction result: '+ tx.result.meta.TransactionResult
  results += '\n\nnfts: ' + JSON.stringify(nfts, null, 2)
  operationalBalanceField.value = (await client.getXrpBalance(operational_wallet.address))
  operationalResultField.value = results
  client.disconnect()
} //End of oPmintToken

// *******************************************************
// ************** Operational Get Tokens *****************
// *******************************************************

async function oPgetTokens() {
  const operational_wallet = xrpl.Wallet.fromSeed(operationalSeedField.value)
  let net = getNet()
  const client = new xrpl.Client(net)
  results = 'Connecting to ' + getNet() + '...'
  operationalResultField.value = results
  await client.connect()
  results += '\nConnected. Getting NFTs...'
  operationalResultField.value = results
  const nfts = await client.request({
    method: "account_nfts",
    account: operational_wallet.classicAddress
  })
  results += '\nNFTs:\n ' + JSON.stringify(nfts,null,2)
  operationalResultField.value = results
  client.disconnect()
} //End of oPgetTokens

// *******************************************************
// ************* Operational Burn Token ******************
// *******************************************************

async function oPburnToken() {
  const operational_wallet = xrpl.Wallet.fromSeed(operationalSeedField.value)
  let net = getNet()
  const client = new xrpl.Client(net)
  results = 'Connecting to ' + getNet() + '...'
  operationalResultField.value = results
  await client.connect()
  results += '\nConnected. Burning NFT...'
  operationalResultField.value = results

  // ------------------------------------------------------- Prepare transaction
  const transactionBlob = {
    "TransactionType": "NFTokenBurn",
    "Account": operational_wallet.classicAddress,
    "NFTokenID": operationalTokenIdField.value
  }

  //-------------------------------------------------------- Submit signed blob
  const tx = await client.submitAndWait(transactionBlob,{wallet: operational_wallet})
  const nfts = await client.request({
    method: "account_nfts",
    account: operational_wallet.classicAddress
  })
  results += '\nTransaction result: '+ tx.result.meta.TransactionResult
  results += '\nBalance changes: ' +
    JSON.stringify(xrpl.getBalanceChanges(tx.result.meta), null, 2)
  operationalResultField.value = results
  operationalBalanceField.value = (await client.getXrpBalance(operational_wallet.address))
  operationalBalanceField.value = (await client.getXrpBalance(operational_wallet.address))
  results += '\nNFTs: \n' + JSON.stringify(nfts,null,2)
  operationalResultField.value = results
  client.disconnect()
}
// End of oPburnToken()3.mint-nfts.html
```

```html
<html>
  <head>
    <title>Token Test Harness</title>
    <link href='https://fonts.googleapis.com/css?family=Work Sans' rel='stylesheet'>
    <style>
       body{font-family: "Work Sans", sans-serif;padding: 20px;background: #fafafa;}
       h1{font-weight: bold;}
       input, button {padding: 6px;margin-bottom: 8px;}
       button{font-weight: bold;font-family: "Work Sans", sans-serif;}
       td{vertical-align: middle;}
    </style>
    <script src='https://unpkg.com/xrpl@2.7.0/build/xrpl-latest-min.js'></script>
    <script src='ripplex1-send-xrp.js'></script>
    <script src='ripplex2-send-currency.js'></script>
    <script src='ripplex3-mint-nfts.js'></script>
    <script>
      if (typeof module !== "undefined") {
        const xrpl = require('xrpl')
      }
    </script>
  </head>

<!-- ************************************************************** -->
<!-- ********************** The Form ****************************** -->
<!-- ************************************************************** -->

  <body>
    <h1>Token Test Harness</h1>
    <form id="theForm">
      Choose your ledger instance:  
      &nbsp;&nbsp;
      <input type="radio" id="tn" name="server"
        value="wss://s.altnet.rippletest.net:51233" checked>
      <label for="testnet">Testnet</label>
      &nbsp;&nbsp;
      <input type="radio" id="dn" name="server"
        value="wss://s.devnet.rippletest.net:51233">
      <label for="devnet">Devnet</label>
      <br/><br/>
      <button type="button" onClick="getAccountsFromSeeds()">Get Accounts From Seeds</button>
      <br/>
      <textarea id="seeds" cols="40" rows= "2"></textarea>
      <br/><br/>
      <table>
        <tr valign="top">
          <td>
            <table>
              <tr valign="top">
                <td>
                <td>
                  <button type="button" onClick="getAccount('standby')">Get New Standby Account</button>
                  <table>
                    <tr valign="top">
                      <td align="right">
                        Standby Account
                      </td>
                      <td>
                        <input type="text" id="standbyAccountField" size="40"></input>
                        <br>
                      </td>
                    </tr>
                    <tr>
                      <td align="right">
                        Public Key
                      </td>
                      <td>
                        <input type="text" id="standbyPubKeyField" size="40"></input>
                        <br>
                      </td>
                    </tr>
                    <tr>
                      <td align="right">
                        Private Key
                      </td>
                      <td>
                        <input type="text" id="standbyPrivKeyField" size="40"></input>
                        <br>
                      </td>
                    </tr>
                    <tr>
                      <td align="right">
                        Seed
                      </td>
                      <td>
                        <input type="text" id="standbySeedField" size="40"></input>
                        <br>
                      </td>
                    </tr>
                    <tr>
                      <td align="right">
                        XRP Balance
                      </td>
                      <td>
                        <input type="text" id="standbyBalanceField" size="40"></input>
                        <br>
                      </td>
                    </tr>
                    <tr>
                      <td align="right">
                        Amount
                      </td>
                      <td>
                        <input type="text" id="standbyAmountField" size="40"></input>
                        <br>
                      </td>
                    </tr>
                    <tr>
                      <td align="right">
                        Destination
                      </td>
                      <td>
                        <input type="text" id="standbyDestinationField" size="40"></input>
                        <br>
                      </td>
                    </tr>
                    <tr valign="top">
                      <td><button type="button" onClick="configureAccount('standby',document.querySelector('#standbyDefault').checked)">Configure Account</button></td>
                      <td>
                        <input type="checkbox" id="standbyDefault" checked="true"/>
                        <label for="standbyDefault">Allow Rippling</label>
                      </td>
                    </tr>
                    <tr>
                      <td align="right">
                        Currency
                      </td>
                      <td>
                        <input type="text" id="standbyCurrencyField" size="40" value="USD"></input>
                      </td>
                    </tr>
                    <tr>
                      <td align="right">NFT URL</td>
                      <td><input type="text" id="standbyTokenUrlField"
                        value = "ipfs://bafybeigdyrzt5sfp7udm7hu76uh7y26nf4dfuylqabf3oclgtqy55fbzdi" size="80"/>
                      </td>
                    </tr>
                    <tr>
                      <td align="right">Flags</td>
                      <td><input type="text" id="standbyFlagsField" value="1" size="10"/></td>
                    </tr>
                    <tr>
                      <td align="right">NFT ID</td>
                      <td><input type="text" id="standbyTokenIdField" value="" size="80"/></td>
                    </tr>
                            <tr>
                              <td align="right">Transfer Fee</td>
                                <td><input type="text" id="standbyTransferFeeField" value="" size="80"/></td>
                            </tr>
                  </table>
                  <p align="left">
                    <textarea id="standbyResultField" cols="80" rows="20" ></textarea>
                  </p>
                </td>
                </td>
                <td>
                  <table>
                    <tr valign="top">
                      <td align="center" valign="top">
                        <button type="button" onClick="sendXRP()">Send XRP&#62;</button>
                        <br/><br/>
                        <button type="button" onClick="createTrustline()">Create TrustLine</button>
                        <br/>
                        <button type="button" onClick="sendCurrency()">Send Currency</button>
                        <br/>
                        <button type="button" onClick="getBalances()">Get Balances</button>
                        <br/><br/>
                        <button type="button" onClick="mintToken()">Mint NFT</button>
                        <br/>
                        <button type="button" onClick="getTokens()">Get NFTs</button>
                        <br/>
                        <button type="button" onClick="burnToken()">Burn NFT</button>
                      </td>
                    </tr>
                    </td>
                    </tr>
                  </table>
                </td>
              </tr>
            </table>
          </td>
          <td>
            <table>
              <tr>
                <td>
                <td>
                  <table>
                    <tr>
                      <td align="center" valign="top">
                        <button type="button" onClick="oPsendXRP()">&#60; Send XRP</button>
                        <br/><br/>
                        <button type="button" onClick="oPcreateTrustline()">Create TrustLine</button>
                        <br/>
                        <button type="button" onClick="oPsendCurrency()">Send Currency</button>
                        <br/>
                        <button type="button" onClick="getBalances()">Get Balances</button>
                        <br/><br/>
                        <button type="button" onClick="oPmintToken()">Mint NFT</button>
                        <br/>
                        <button type="button" onClick="oPgetTokens()">Get NFTs</button>
                        <br/>
                        <button type="button" onClick="oPburnToken()">Burn NFT</button>
                      <td valign="top" align="right">
                        <button type="button" onClick="getAccount('operational')">Get New Operational Account</button>
                        <table>
                          <tr valign="top">
                            <td align="right">
                              Operational Account
                            </td>
                            <td>
                              <input type="text" id="operationalAccountField" size="40"></input>
                              <br>
                            </td>
                          </tr>
                          <tr>
                            <td align="right">
                              Public Key
                            </td>
                            <td>
                              <input type="text" id="operationalPubKeyField" size="40"></input>
                              <br>
                            </td>
                          </tr>
                          <tr>
                            <td align="right">
                              Private Key
                            </td>
                            <td>
                              <input type="text" id="operationalPrivKeyField" size="40"></input>
                              <br>
                            </td>
                          </tr>
                          <tr>
                            <td align="right">
                              Seed
                            </td>
                            <td>
                              <input type="text" id="operationalSeedField" size="40"></input>
                              <br>
                            </td>
                          </tr>
                          <tr>
                            <td align="right">
                              XRP Balance
                            </td>
                            <td>
                              <input type="text" id="operationalBalanceField" size="40"></input>
                              <br>
                            </td>
                          </tr>
                          <tr>
                            <td align="right">
                              Amount
                            </td>
                            <td>
                              <input type="text" id="operationalAmountField" size="40"></input>
                              <br>
                            </td>
                          </tr>
                          <tr>
                            <td align="right">
                              Destination
                            </td>
                            <td>
                              <input type="text" id="operationalDestinationField" size="40"></input>
                              <br>
                            </td>
                          </tr>
                          <tr>
                            <td>
                            </td>
                            <td align="right">                        <input type="checkbox" id="operationalDefault" checked="true"/>
                              <label for="operationalDefault">Allow Rippling</label>
                              <button type="button" onClick="configureAccount('operational',document.querySelector('#operationalDefault').checked)">Configure Account</button>
                            </td>
                          </tr>
                          <tr>
                            <td align="right">
                              Currency
                            </td>
                            <td>
                              <input type="text" id="operationalCurrencyField" size="40" value="USD"></input>
                            </td>
                          </tr>
                          <tr>
                            <td align="right">NFT URL</td>
                            <td><input type="text" id="operationalTokenUrlField"
                              value = "ipfs://bafybeigdyrzt5sfp7udm7hu76uh7y26nf4dfuylqabf3oclgtqy55fbzdi" size="80"/>
                            </td>
                          </tr>
                          <tr>
                            <td align="right">Flags</td>
                            <td><input type="text" id="operationalFlagsField" value="1" size="10"/></td>
                          </tr>
                          <tr>
                            <td align="right">NFT ID</td>
                            <td><input type="text" id="operationalTokenIdField" value="" size="80"/></td>
                          </tr>
                          <tr>
                            <td align="right">Transfer Fee</td>
                            <td><input type="text" id="operationalTransferFeeField" value="" size="80"/></td>
                          </tr>
                                      </table>
                        <p align="right">
                          <textarea id="operationalResultField" cols="80" rows="20" ></textarea>
                        </p>
                      </td>
                      </td>
                    </tr>
                    </td>
                    </tr>
                  </table>
                </td>
              </tr>
            </table>
          </td>
        </tr>
      </table>
    </form>
  </body>
</html>
```

