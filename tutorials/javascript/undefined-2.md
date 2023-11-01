# JavaScript를 이용한 공인 발행인 지정(Assign an Authorized Minter Using JavaScript)

다른 계정에게 당신을 대신하여 NFT를 생성할 수 있는 권한을 부여할 수 있습니다.

다음 예제에서는 다음 작업을 수행하는 방법을 보여줍니다:

1. 다른 계정에게 당신의 계정을 위해 NFT를 생성할 수 있는 권한 부여하기.&#x20;
2. 권한이 부여된 경우, 다른 계정을 대신하여 NFT를 발행하기.&#x20;

<figure><img src="https://xrpl.org/img/quickstart28.png" alt=""><figcaption></figcaption></figure>

## 사용법

[Quickstart Samples](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/quickstart/js/) 아카이브를 다운로드하여 각 샘플을 브라우저에서 시도해볼 수 있습니다.

## 계정 가져오기&#x20;

1. 브라우저에서 6.authorized-minter.html 파일을 엽니다.&#x20;
2. ledger 인스턴스를 선택합니다 (_testnet_ 또는 _devnet_).&#x20;
   1. 기존의 테스트 계정 seeds가 있는 경우:&#x20;
      1. 계정 시드를 **Seeds** 필드에 붙여넣습니다.&#x20;
      2. **Get Accounts from Seeds**를 클릭합니다.&#x20;
   2. 기존의 테스트 계정이 없는 경우:&#x20;
      1. **Get New Standby Account**를 클릭합니다.&#x20;
      2. **Get New Operational Account**를 클릭합니다.&#x20;

## 계정에 NFT 생성 권한 부여

다른 계정이 당신의 계정을 위해 NFT를 생성할 수 있도록 하려면:

1. **Operational Account** 값을 복사합니다.&#x20;
2. **Operational Account** 값을 **Authorized Minter** 필드에 붙여넣습니다.&#x20;
3. **Set Minter**을 클릭합니다.&#x20;

<figure><img src="https://xrpl.org/img/quickstart29.png" alt=""><figcaption></figcaption></figure>

## 다른 계정을 위해 NFT 생성하기&#x20;

{% embed url="https://youtu.be/cXvyu2ZDCBM" %}

이 예제에서는 이전 단계에서 권한이 부여된 운영 계정을 사용하여 Standby account를 대신하여 토큰을 발행합니다.

다른 계정을 위해 non-fungible token을 발행하려면:

1. **Flags** 필드를 설정합니다. 테스트 목적으로는 값이 _8_로 설정하는 것을 권장합니다.&#x20;
2. **NFT URL**을 입력합니다. 이는 NFT 개체와 관련된 데이터 또는 메타데이터를 가리키는 URI입니다. 직접 URL을 소유하고 있지 않은 경우 샘플 URI를 사용할 수 있습니다.&#x20;
3. 이전 판매에서 원작자가 향후 판매에서 수익의 일부를 받는 이체 수수료를 입력합니다. 0에서 50000 사이의 값을 입력할 수 있으며, 0부터 0.001% 단위로 50.000%까지의 이체 비율을 허용합니다. NFT가 이전 가능하도록 하기 위해 Flags 필드를 설정하지 않았다면, 이 필드를 0으로 설정합니다.&#x20;
4. **Standby Account** 값을 복사합니다. **Standby Account** 값을 Operational account **Issuer** 필드에 붙여넣습니다.&#x20;
5. Operational account **Mint Other** 버튼을 클릭합니다.

<figure><img src="https://xrpl.org/img/quickstart30.png" alt=""><figcaption></figcaption></figure>

아이템이 발행되면 권한이 부여된 Minter는 일반적인 방식으로 NFT를 판매할 수 있습니다. 판매 수익은 이체 수수료를 제외한 권한이 부여된 Minter에게 지급됩니다. 발행자와 Minter는 가격 분배에 대해 별도로 협의할 수 있습니다.

## 판매 오더 생성하기&#x20;

NFT 판매 오더를 생성하려면:

1. Operational account 측에서 제안 **Amount**를 드롭 (XRP의 백만 분의 1)으로 입력합니다. 예를 들어, 100000000 (100 XRP)를 입력합니다.&#x20;
2. **Flags** 필드를 1로 설정합니다.&#x20;
3. 판매할 NFT의 **NFT ID**를 입력합니다.&#x20;
4. 선택적으로 **Expiration**까지 남은 일 수를 입력합니다.&#x20;
5. **Create Sell Offer**을 클릭합니다.&#x20;

응답에서 중요한 정보는 NFT Offer Index입니다. 이 인덱스는 sell offer를 수락하는 데 사용되며, `nft_offer_index`라고 표시됩니다.

<figure><img src="https://xrpl.org/img/quickstart31.png" alt=""><figcaption></figcaption></figure>

## Sell Offer 수락하기&#x20;

Sell Offer가 있는 경우, 새로운 계정을 생성하여 해당 offer를 수락하고 NFT를 구매할 수 있습니다.

사용 가능한 Sell Offer를 수락하려면:

1. **Get New Standby Account**를 클릭합니다.&#x20;
2. **NFT Offer Index** (NFT 판매 결과의 `nft_offer_index`로 표시됨)를 입력합니다. 이는 `nft_id`와 다른 값입니다.)
3. **Accept Sell Offer**을 클릭합니다.&#x20;

결과에서 발행자 계정에 25 XRP가 입금된 것을 확인할 수 있습니다. 구매자 계정은 100 XRP 가격과 12 드롭의 거래 비용을 차감한 것입니다. 판매자(권한이 부여된 Minter) 계정은 75 XRP를 입금받았습니다. 발행자와 판매자는 별도의 거래에서 수익을 분배할 수 있습니다.

<figure><img src="https://xrpl.org/img/quickstart32.png" alt=""><figcaption></figcaption></figure>

## Code Walkthrough <a href="#code-walkthrough" id="code-walkthrough"></a>

[Quickstart Samples](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/quickstart/js/) 아카이브를 다운로드하여 각 샘플을 브라우저에서 시도해볼 수 있습니다.

### Set Minter <a href="#set-minter" id="set-minter"></a>

이 함수는 계정에 대한 권한 있는 관리자를 설정합니다. 각 계정에는 NFT를 대신 작성할 수 있는 권한이 있는 관리자가 0개 또는 1개 있을 수 있습니다.

```javascript
// *******************************************************
// ****************  Set Minter  *************************
// *******************************************************

async function setMinter() {    
```

ledger에 연결해 계정을 가져오세요.

````javascript
  let net = getNet()
  const client = new xrpl.Client(net)
  results = 'Connecting to ' + getNet() + '....'
  standbyResultField.value = results    
  await client.connect()
  results += '\nConnected, finding wallet.'
  standbyResultField.value = results    
  my_wallet = xrpl.Wallet.fromSeed(standbySeedField.value)
  standbyResultField.value = results    ```

Define the AccountSet transaction, setting the `NFTokenMinter` account and the `asfAuthorizedNFTokenMinter` flag.

```javascript
  tx_json = {
    "TransactionType": "AccountSet",
    "Account": my_wallet.address,
````

`NFTokenMinter`를 인증된 Minter의 계정 번호로 설정합니다.

```javascript
    "NFTokenMinter": standbyMinterField.value,
```

`asfAuthorizedNFTokenMinter`플래그(숫자 값은 10) 설정.

```javascript
    "SetFlag": xrpl.AccountSetAsfFlags.asfAuthorizedNFTokenMinter
    }
```

진행 상황을 보고합니다.

```javascript
  results += '\nSet Minter.' 
  standbyResultField.value = results    
```

트랜잭션을 준비하고 전송한 다음 결과를 기다립니다.

```javascript
  const prepared = await client.autofill(tx_json)
  const signed = my_wallet.sign(prepared)
  const result = await client.submitAndWait(signed.tx_blob)
```

트랜잭션이 성공하면 결과를 표시합니다. 그렇지 않으면 트랜잭션이 실패했다고 보고합니다.

```javascript
  if (result.result.meta.TransactionResult == "tesSUCCESS") {
    results += '\nAccount setting succeeded.\n'
    results += JSON.stringify(result,null,2)
    standbyResultField.value = results    
  } else {
    throw 'Error sending transaction: ${result}'
    results += '\nAccount setting failed.'
  standbyResultField.value = results    
  }
```

ledger에서 연결을 끊습니다.

```javascript
  client.disconnect()
} // End of configureAccount()
```

### Mint Other <a href="#mint-other" id="mint-other"></a>

이 수정된 mint function을 통해 한 계정이 다른 발행자를 위해 mint할 수 있습니다.

```javascript
// *******************************************************
// ****************  Mint Other  *************************
// *******************************************************

async function mintOther() {
```

ledger에 연결해 계정을 가져오세요.

```javascript
  results = 'Connecting to ' + getNet() + '....'
  standbyResultField.value = results    
  let net = getNet()
  const standby_wallet = xrpl.Wallet.fromSeed(standbySeedField.value)
  const client = new xrpl.Client(net)
  await client.connect()
```

성공을 보고합니다.

```javascript
  results += '\nConnected. Minting NFT.'
  standbyResultField.value = results    
```

이 트랜잭션 blob은 이전의 [`mintToken()` function](https://xrpl.org/mint-and-burn-nfts.html#mint-token)에 사용된 것과 동일하며, `Issuer` 필드가 추가되었습니다.

```javascript
  const tx_json = {
    "TransactionType": "NFTokenMint",
    "Account": standby_wallet.classicAddress,
```

URI는 NFT로 표시되는 데이터 파일에 대한 링크입니다.

```javascript
    "URI": xrpl.convertStringToHex(standbyTokenUrlField.value),
```

최소한`tfTransferable`플래그(8)를 설정하여 계정이 테스트 목적으로 NFT를 판매 및 재판매할 수 있도록 하는 것이 좋습니다.

```javascript
    "Flags": parseInt(standbyFlagsField.value),
```

이전 수수료는 원래 발행자에게 반환될 재판매 가격의 0.001%를 나타내는 0-50000 값입니다. 예를 들어, _25000_은 재판매 시 발행자에게 보낼 가격의 25%를 의미합니다.

```javascript
    "TransferFee": parseInt(standbyTransferFeeField.value),
```

**Issuer**는 NFT로 표시되는 객체의 원래 작성자입니다.

```javascript
    "Issuer": standbyIssuerField.value,
```

`NFTokenTaxon`은 발행자가 자신의 목적을 위해 사용할 수 있는 선택적 번호 필드입니다. 여러 토큰에 동일한 분류법을 사용할 수 있습니다. 사용할 필요가 없으면 0으로 설정합니다.

```javascript
    "NFTokenTaxon": 0 //Required, but if you have no use for it, set to zero.
  }
```

트랜잭션을 제출하고 결과를 기다립니다.

```javascript
  const tx = await client.submitAndWait(tx_json, { wallet: standby_wallet} )
  const nfts = await client.request({
    method: "account_nfts",
    account: standby_wallet.classicAddress
  })
```

결과를 보고합니다.

```javascript
  results += '\n\nTransaction result: '+ tx.result.meta.TransactionResult
  results += '\n\nnfts: ' + JSON.stringify(nfts, null, 2)
  standbyResultField.value = results  + (await
    client.getXrpBalance(standby_wallet.address))
  standbyResultField.value = results    
```

XRP Ledger에서 연결을 끊습니다.

```javascript
  client.disconnect()
} //End of mintOther()
```

### Reciprocal Transactions <a href="#reciprocal-transactions" id="reciprocal-transactions"></a>

이러한 기능은 operational account에 대한 standby account의 기능을 복제합니다. 자세한 내용은 해당 standby account function을 참조하십시오.

```javascript
// *******************************************************
// ****************  Set Operational Minter  **************
// ********************************************************

async function oPsetMinter() {    
  let net = getNet()
  const client = new xrpl.Client(net)
  results = 'Connecting to ' + getNet() + '....'
  operationalResultField.value = results
  await client.connect()
  results += '\nConnected, finding wallet.'
  operationalResultField.value = results
  my_wallet = xrpl.Wallet.fromSeed(operationalSeedField.value)
  operationalResultField.value = results

  tx_json = {
    "TransactionType": "AccountSet",
    "Account": my_wallet.address,
    "NFTokenMinter": operationalMinterField.value,
    "SetFlag": xrpl.AccountSetAsfFlags.asfAuthorizedNFTokenMinter
  } 
  results += '\nSet Minter.' 
  operationalResultField.value = results

  const prepared = await client.autofill(tx_json)
  const signed = my_wallet.sign(prepared)
  const result = await client.submitAndWait(signed.tx_blob)
  if (result.result.meta.TransactionResult == "tesSUCCESS") {
    results += '\nAccount setting succeeded.\n'
    results += JSON.stringify(result,null,2)
    operationalResultField.value = results
  } else {
    throw 'Error sending transaction: ${result}'
    results += '\nAccount setting failed.'
    operationalResultField.value = results
  }

  client.disconnect()
} // End of oPsetMinter()


// *******************************************************
// ************** Operational Mint Other *****************
// *******************************************************

async function oPmintOther() {
  results = 'Connecting to ' + getNet() + '....'
  operationalResultField.value = results
  let net = getNet()
  const operational_wallet = xrpl.Wallet.fromSeed(operationalSeedField.value)
  const client = new xrpl.Client(net)
  await client.connect()
  results += '\nConnected. Minting NFT.'
  operationalResultField.value = results

  // This version adds the "Issuer" field.
  // ------------------------------------------------------------------------
  const tx_json = {
    "TransactionType": 'NFTokenMint',
    "Account": operational_wallet.classicAddress,
    "URI": xrpl.convertStringToHex(operationalTokenUrlField.value),
    "Flags": parseInt(operationalFlagsField.value),
    "Issuer": operationalIssuerField.value,
    "TransferFee": parseInt(operationalTransferFeeField.value),
    "NFTokenTaxon": 0 //Required, but if you have no use for it, set to zero.
  }

  // ----------------------------------------------------- Submit signed blob 
  const tx = await client.submitAndWait(tx_json, { wallet: operational_wallet} )
  const nfts = await client.request({
    method: "account_nfts",
    account: operational_wallet.classicAddress  
  })

  // ------------------------------------------------------- Report results
  results += '\n\nTransaction result: '+ tx.result.meta.TransactionResult
  results += '\n\nnfts: ' + JSON.stringify(nfts, null, 2)
  results += await client.getXrpBalance(operational_wallet.address)
  operationalResultField.value = results
  client.disconnect()
} //End of oPmintToken
```

### 6.authorized-minter.html <a href="#6authorized-minterhtml" id="6authorized-minterhtml"></a>

새 기능을 지원하도록 필드와 버튼으로 양식을 업데이트합니다.

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
    <script src='ripplex4-transfer-nfts.js'></script>
    <script src='ripplex6-authorized-minter.js'></script>
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
                      <td align="right">NFT Offer Index</td>
                      <td><input type="text" id="standbyTokenOfferIndexField" value="" size="80"/></td>
                    </tr>
                    <tr>
                      <td align="right">Owner</td>
                      <td><input type="text" id="standbyOwnerField" value="" size="80"/></td>
                    </tr>
                    <tr>
                        <td align="right">Authorized Minter</td>
                        <td><input type="text" id="standbyMinterField" value="" size="80"/></td>
                    </tr>
                    <tr>
                        <td align="right">Issuer</td>
                        <td><input type="text" id="standbyIssuerField" value="" size="80"/></td>
                    </tr>
                    <tr>
                        <td align="right">Destination</td>
                        <td><input type="text" id="standbyDestinationField" value="" size="80"/></td>
                    </tr>
                            <tr>
                                <td align="right">Expiration</td>
                                <td><input type="text" id="standbyExpirationField" value="" size="80"/></td>
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
                        <br/><br/>
                        <button type="button" onClick="setMinter('standby')">Set Minter</button>
                        <br/>
                        <button type="button" onClick="mintOther()">Mint Other</button>
                        <br/><br/>
                        <button type="button" onClick="createSellOffer()">Create Sell Offer</button>
                        <br/>
                        <button type="button" onClick="acceptSellOffer()">Accept Sell Offer</button>
                        <br/>
                        <button type="button" onClick="createBuyOffer()">Create Buy Offer</button>
                        <br/>
                        <button type="button" onClick="acceptBuyOffer()">Accept Buy Offer</button>
                        <br/>
                        <button type="button" onClick="getOffers()">Get Offers</button>
                        <br/>
                        <button type="button" onClick="cancelOffer()">Cancel Offer</button>
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
                        <br/><br/>
                        <button type="button" onClick="oPsetMinter()">Set Minter</button>
                        <br/>
                        <button type="button" onClick="oPmintOther()">Mint Other</button>
                        <br/><br/>
                        <button type="button" onClick="oPcreateSellOffer()">Create Sell Offer</button>
                        <br/>
                        <button type="button" onClick="oPacceptSellOffer()">Accept Sell Offer</button>
                        <br/>
                        <button type="button" onClick="oPcreateBuyOffer()">Create Buy Offer</button>
                        <br/>
                        <button type="button" onClick="oPacceptBuyOffer()">Accept Buy Offer</button>
                        <br/>
                        <button type="button" onClick="oPgetOffers()">Get Offers</button>
                        <br/>
                        <button type="button" onClick="oPcancelOffer()">Cancel Offer</button>
                      </td>
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
                            <td align="right">NFT Offer Index</td>
                            <td><input type="text" id="operationalTokenOfferIndexField" value="" size="80"/></td>
                          </tr>
                          <tr>
                            <td align="right">Owner</td>
                            <td><input type="text" id="operationalOwnerField" value="" size="80"/></td>
                          </tr>
                          <tr>
                        <td align="right">Authorized Minter</td>
                        <td><input type="text" id="operationalMinterField" value="" size="80"/></td>
                    </tr>
                    <tr>
                        <td align="right">Issuer</td>
                        <td><input type="text" id="operationalIssuerField" value="" size="80"/></td>
                    </tr>
                            <tr>
                              <td align="right">Destination</td>
                              <td><input type="text" id="operationalDestinationField" value="" size="80"/></td>
                            </tr>
                            <tr>
                              <td align="right">Expiration</td>
                              <td><input type="text" id="operationalExpirationField" value="" size="80"/></td>
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
