# JavaScript를 이용한 NFT 판매 중개(Broker an NFT Sale Using JavaScript)

이전의 예제들은 두 계정 간에 직접 구매 및 판매 제안을 만드는 방법을 보여주었습니다. 또 다른 옵션은 세 번째 계정을 판매의 중개인으로 사용하는 것입니다. 중개인은 NFT 소유자를 대신하여 행동합니다. 판매자는 목적지로 중개 계정을 가진 제안을 생성합니다. 중개인은 구매 제안을 수집하고 평가하며, 판매를 준비하는 데 동의한 수수료를 추가하여 어느 제안을 수락할지 결정합니다. 중개 계정이 판매 제안을 구매 제안으로 수락할 때, 자금과 NFT의 소유권은 동시에 전송되어 거래가 완료됩니다. 이렇게 하면 계정이 NFT 창작자와 거래자를 위한 마켓플레이스나 개인 대리인으로 행동할 수 있게 합니다.

## 사용법

이 예제는 다음을 보여줍니다:

1. 중개된 판매 제안 생성하기.
2. 중개된 아이템에 대한 제안 목록 받기.
3. 두개 다른 계정간의 판매를 중개하기.

<figure><img src="https://xrpl.org/img/quickstart21.png" alt=""><figcaption></figcaption></figure>

[Quickstart Samples](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/quickstart/js/) 아카이브를 다운로드하여 각 샘플을 브라우저에서 시도해볼 수 있습니다.

## 계정 받기&#x20;

1. 브라우저에서 `5.broker-nfts.html`을 엽니다.&#x20;
2. ledger 인스턴스를 선택합니다.
3. 테스트 계정을 가져옵니다.
   1. 계정 seed가 있는 경우&#x20;
      1. **Seeds** 필드에 계정 seed 3개를 붙여넣습니다.&#x20;
      2. **Get Accounts from Seeds**를 클릭합니다
   2. 계정 seed가 없는 경우
      1. **Get New Standby Account**를 클릭합니다.
      2. **Get New Operational Account**를 클릭합니다.
      3. **Get New Broker Account를** 클릭합니다.

<figure><img src="https://xrpl.org/img/quickstart22.png" alt=""><figcaption></figcaption></figure>

## 중개 트랜잭션 준비&#x20;

{% embed url="https://youtu.be/GemQ-t7g9fo" %}

1. Standby account을 사용하여 Broker account를대상으로 하는 NFT 판매 제안을 생성합니다.&#x20;
   1. 판매 제안 **Amount**을 드롭 단위로 입력합니다(XRP의 백만분의 일).&#x20;
   2. **Flags** 필드를 1로 설정합니다.&#x20;
   3. 판매할 NFT의 **NFT ID**를 입력합니다.&#x20;
   4. 선택적으로 **Expiration**까지 일 수를 입력합니다.&#x20;
   5. Broker 계정 번호를 **Destination**으로 입력합니다.&#x20;
   6. **Create Sell Offer**을 클릭합니다.

<figure><img src="https://xrpl.org/img/quickstart23.png" alt=""><figcaption></figcaption></figure>

2. Operational account 사용하여 NFT 구매 제안을 작성합니다.&#x20;
   1. 제안 **Amount**를 입력합니다.&#x20;
   2. **NFT ID**를 입력합니다.&#x20;
   3. **Owner** 필드에 소유자의 계정 문자열을 입력합니다.&#x20;
   4. 선택적으로 **Expiration**까지 일 수를 입력합니다.&#x20;
   5. **Create Buy Offer**을 클릭합니다.

<figure><img src="https://xrpl.org/img/quickstart24.png" alt=""><figcaption></figcaption></figure>

## 제안 받기

1. **NFT ID**를 입력합니다.
2. **Get Offers**를 클릭합니다.

<figure><img src="https://xrpl.org/img/quickstart25.png" alt=""><figcaption></figcaption></figure>

## 판매 중개&#x20;

1. 판매 제안의 _nft\_offer\_index_를 복사하여 **Sell NFT Offer Index** 필드에 붙여넣습니다.&#x20;
2. 매입 제의 _nft\_offer\_index_를 복사하여 **Buy NFT Offer Index** 필드에 붙여넣습니다.&#x20;
3. **Broker Fee**를 드롭 단위로 입력합니다.&#x20;
4. **Broker Sale**를 클릭합니다.

<figure><img src="https://xrpl.org/img/quickstart26.png" alt=""><figcaption></figcaption></figure>

## 제안 취소

중개인이 구매 제안을 수락한 후, 중개인에게 권한이 있는 경우 다른 모든 제안을 취소하는 것이 좋습니다. **Get Offers**를 사용하여 구매 제안의 전체 목록을 확인할 수 있습니다. 제안을 취소하려면:

1. **Buy NFT Offer Index** 필드에 취소할 구매 제안의 nft\_Offer\_index를 입력합니다.&#x20;
2. **Cancel Offer**를 클릭합니다.

<figure><img src="https://xrpl.org/img/quickstart27.png" alt=""><figcaption></figcaption></figure>

## Code Walkthrough <a href="#code-walkthrough" id="code-walkthrough"></a>

[Quickstart Samples](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/quickstart/js/) 아카이브를 다운로드하여 각 샘플을 브라우저에서 시도해볼 수 있습니다.

### ripplex5-broker-nfts.js <a href="#ripplex5-broker-nftsjs" id="ripplex5-broker-nftsjs"></a>

이 스크립트는 중개된 거래를 위한 새로운 기능과 동일한 화면에서 세 번째 계정을 지원하도록 수정된 기능을 가지고 있습니다.

### Broker Get Offers <a href="#broker-get-offers" id="broker-get-offers"></a>

```javascript
// *******************************************************
// *************** Broker Get Offers *********************
// *******************************************************

async function brGetOffers() {
```

ledger에 연결합니다.

```javascript
  let net = getNet()
  const client = new xrpl.Client(net)
  results = 'Connecting to ' + getNet() + '...'
  brokerResultField.value = results
  await client.connect()
  results += '\nConnected. Getting offers...'
    brokerResultField.value = results
```

토큰에 대한 판매 제안 목록을 요청합니다.

```javascript
  results += '\n\n***Sell Offers***\n'  
  let nftSellOffers
  try {
    nftSellOffers = await client.request({
      method: "nft_sell_offers",
      nft_id: brokerTokenIdField.value  
    })
  } catch (err) {
    nftSellOffers = 'No sell offers.'
  }
  results += JSON.stringify(nftSellOffers,null,2)
    brokerResultField.value = results
```

토큰에 대한 구매 제안 목록을 요청합니다.

```javascript
  results += '\n\n***Buy Offers***\n'
  let nftBuyOffers
  try {
    nftBuyOffers = await client.request({
      method: "nft_buy_offers",
      nft_id: brokerTokenIdField.value 
    })
  } catch (err) {
    nftBuyOffers =  'No buy offers.'
  }
  results += JSON.stringify(nftBuyOffers,null,2)    
    brokerResultField.value = results
```

ledger에서 연결을 끊습니다.

```javascript
  client.disconnect()  
}// End of brGetOffers()
```

### Broker Sale <a href="#broker-sale" id="broker-sale"></a>

```javascript
async function brokerSale() {
```

ledger에 연결해서 계정을 가져오세요.

```javascript
    const standby_wallet = xrpl.Wallet.fromSeed(standbySeedField.value)
    const operational_wallet = xrpl.Wallet.fromSeed(operationalSeedField.value)
    const broker_wallet = xrpl.Wallet.fromSeed (brokerSeedField.value)
    let net = getNet()
    const client = new xrpl.Client(net)
    results = 'Connecting to ' + getNet() + '...'
    brokerResultField.value = results
    await client.connect()
    results += '\nConnected. Brokering sale...'
    brokerResultField.value = results
```

트랜잭션을 준비합니다. 중개 판매와 직접 판매의 차이점은 합의된 중개인 수수료와 함께 판매 제안과 구매 제안을 모두 제공한다는 것입니다.

```javascript
  const transactionBlob = {
    "TransactionType": "NFTokenAcceptOffer",
    "Account": broker_wallet.classicAddress,
    "NFTokenSellOffer": brokerTokenSellOfferIndexField.value,
    "NFTokenBuyOffer": brokerTokenBuyOfferIndexField.value,
    "NFTokenBrokerFee": brokerBrokerFeeField.value
  }
```

트랜잭션을 제출하고 결과를 기다립니다.

```javascript
  const tx = await client.submitAndWait(transactionBlob,{wallet: broker_wallet}) 
```

결과를 보고합니다.

```javascript
    results += "\n\nTransaction result:\n" + 
            JSON.stringify(tx.result.meta.TransactionResult, null, 2)
    results += "\nBalance changes:\n" +
            JSON.stringify(xrpl.getBalanceChanges(tx.result.meta), null, 2)
    operationalBalanceField.value = (await client.getXrpBalance(operational_wallet.address))
    standbyBalanceField.value = (await client.getXrpBalance(standby_wallet.address))
    brokerBalanceField.value = (await client.getXrpBalance(broker_wallet.address))
    brokerResultField.value = results
```

ledger에서 연결을 끊습니다.

```javascript
  client.disconnect()
}// End of brokerSale()
```

### Broker Cancel Offer <a href="#broker-cancel-offer" id="broker-cancel-offer"></a>

```javascript
// *******************************************************
// ************* Broker Cancel Offer ****************
// *******************************************************


async function brCancelOffer() {
```

broker 계정을 가져오고 ledger에 연결합니다.

```javascript
  const wallet = xrpl.Wallet.fromSeed(brokerSeedField.value)
    let net = getNet()
    const client = new xrpl.Client(net)
    results = 'Connecting to ' + getNet() + '...'
  brokerResultField.value = results
  await client.connect()
  results +=  "\nConnected. Cancelling offer..."
  brokerResultField.value = results
```

토큰 ID는 반드시 array로 변환되어야 합니다.

```javascript
  const tokenOfferIDs = [brokerTokenBuyOfferIndexField.value]
```

트랜잭션을 준비합니다.

```javascript
  const transactionBlob = {
    "TransactionType": "NFTokenCancelOffer",
    "Account": wallet.classicAddress,
    "NFTokenOffers": tokenOfferIDs
  }
```

트랜잭션을 제출하고 결과를 기다립니다.

```javascript
  const tx = await client.submitAndWait(transactionBlob,{wallet})
```

sell offers를 받고 결과를 보고합니다.

```javascript
  results += "\n\n***Sell Offers***\n"
  let nftSellOffers
  try {
    nftSellOffers = await client.request({
      method: "nft_sell_offers",
      nft_id: brokerTokenBuyOfferIndexField.value
    })
  } catch (err) {
    nftSellOffers = "No sell offers."
  }
  results += JSON.stringify(nftSellOffers,null,2)
```

buy offers를 받고 결과를 보고합니다.

```javascript
  results += "\n\n***Buy Offers***\n"
  let nftBuyOffers
  try {
    nftBuyOffers = await client.request({
      method: "nft_buy_offers",
      nft_id: brokerTokenBuyOfferIndexField.value })
  } catch (err) {
    nftBuyOffers = "No buy offers."
  }
  results += JSON.stringify(nftBuyOffers,null,2)
```

트랜잭션 결과를 보고합니다.

```javascript
  results += "\nTransaction result:\n" +
    JSON.stringify(tx.result.meta.TransactionResult, null, 2)
  results += "\nBalance changes:\n" + 
    JSON.stringify(xrpl.getBalanceChanges(tx.result.meta), null, 2)
  document.getElementById('brokerResultField').value = results
```

ledger에서 연결을 끊습니다.

```javascript
  client.disconnect()
}// End of brCancelOffer()
```

### Get Account <a href="#get-account" id="get-account"></a>

broker account를 수용하기 위해 `getAccount(type)` 함수를 재정의하여 브로커 타입을 감시하도록 변경하세요.

```javascript
// ***************************************************************************
// ************** Revised Functions ******************************************
// ***************************************************************************

// *******************************************************
// ************* Get Account *****************************
// *******************************************************


async function getAccount(type) {
```

ledger에 연결합니다.

```javascript
  let net = getNet()
  const client = new xrpl.Client(net)
  results = 'Connecting to ' + net + '....'
```

올바른 네트워크 호스트를 가져옵니다.

```javascript
  let net = getNet()
  const client = new xrpl.Client(net)
  results = 'Connecting to ' + net + '....'

  let faucetHost = null
  if (type == 'standby') {
    standbyResultField.value = results
  } 
  if (type == 'operational') {
    operationalResultField.value = results
  }
  if (type == 'broker') {
    brokerResultField.value = results
  }
```

ledger에 연결합니다.

```javascript
  await client.connect()

  results += '\nConnected, funding wallet.'
  if (type == 'standby') {
    standbyResultField.value = results
  } 
  if (type == 'operational') {
    operationalResultField.value = results
  }
  if (type == 'broker') {
    brokerResultField.value = results
  }
```

테스트 계정을 만들고 자금을 지원하며 진행 상황을 보고합니다.

```javascript
  const my_wallet = (await client.fundWallet(null, { faucetHost })).wallet

  results += '\nGot a wallet.'
  if (type == 'standby') {
    standbyResultField.value = results
  } 
  if (type == 'operational') {
    operationalResultField.value = results
  }
  if (type == 'broker') {
    brokerResultField.value = results
  }
```

새 계정에 대한 XRP 잔액을 가져옵니다.

```javascript
  const my_balance = (await client.getXrpBalance(my_wallet.address))  
```

해당 계정의 양식 필드를 새 계정 정보로 채웁니다.

```javascript
  if (type == 'standby') {
    standbyAccountField.value = my_wallet.address
    standbyPubKeyField.value = my_wallet.publicKey
    standbyPrivKeyField.value = my_wallet.privateKey
    standbyBalanceField.value = (await client.getXrpBalance(my_wallet.address))
    standbySeedField.value = my_wallet.seed
    results += '\nStandby account created.'
    standbyResultField.value = results
  }
  if (type == 'operational') {
    operationalAccountField.value = my_wallet.address
    operationalPubKeyField.value = my_wallet.publicKey
    operationalPrivKeyField.value = my_wallet.privateKey
    operationalSeedField.value = my_wallet.seed
    operationalBalanceField.value = (await client.getXrpBalance(my_wallet.address))
    results += '\nOperational account created.'
    operationalResultField.value = results
  }
  if (type == 'broker') {
    brokerAccountField.value = my_wallet.address
    brokerPubKeyField.value = my_wallet.publicKey
    brokerPrivKeyField.value = my_wallet.privateKey
    brokerSeedField.value = my_wallet.seed
    brokerBalanceField.value = (await client.getXrpBalance(my_wallet.address))
    results += '\nBroker account created.'
    brokerResultField.value = results
  }
```

**Account Seeds** 필드의 해당 줄에 새 계정 시드를 추가합니다.

```javascript
  seeds.value = standbySeedField.value + '\n' + operationalSeedField.value + "\n" +
    brokerSeedField.value
```

ledger에서 연결을 끊습니다.

```javascript
  client.disconnect()
} // End of getAccount()
```

### Get Accounts from Seeds <a href="#get-accounts-from-seeds" id="get-accounts-from-seeds"></a>

broker account 필드를 포함하도록 `getAccountsFromSeeds()` 함수를 재정의합니다.

```javascript
async function getAccountsFromSeeds() {
```

ledger에 연결합니다.

```javascript
    let net = getNet()
    const client = new xrpl.Client(net)
    results = 'Connecting to ' + getNet() + '....'
    standbyResultField.value = results
    await client.connect()
    results += '\nConnected, finding wallets.\n'
    standbyResultField.value = results
```

`split` 함수를 사용하여 **Seeds** 필드의 값을 구문 분석합니다.

```javascript
  var lines = seeds.value.split('\n');
```

Seed 값에서 계정을 파생합니다.

```javascript
  const standby_wallet = xrpl.Wallet.fromSeed(lines[0])
  const operational_wallet = xrpl.Wallet.fromSeed(lines[1])
  const broker_wallet = xrpl.Wallet.fromSeed(lines[2])
```

계정에 대한 XRP 잔액을 가져옵니다.

```javascript
  const standby_balance = (await client.getXrpBalance(standby_wallet.address))  
  const operational_balance = (await client.getXrpBalance(operational_wallet.address))  
  const broker_balance = (await client.getXrpBalance(broker_wallet.address))  
```

계정 값을 기준으로 양식 필드를 채웁니다.

```javascript
    standbyAccountField.value = standby_wallet.address
    standbyPubKeyField.value = standby_wallet.publicKey
    standbyPrivKeyField.value = standby_wallet.privateKey
    standbySeedField.value = standby_wallet.seed
    standbyBalanceField.value = (await client.getXrpBalance(standby_wallet.address))

    operationalAccountField.value = operational_wallet.address
    operationalPubKeyField.value = operational_wallet.publicKey
    operationalPrivKeyField.value = operational_wallet.privateKey
    operationalSeedField.value = operational_wallet.seed
    operationalBalanceField.value = (await client.getXrpBalance(operational_wallet.address))

    brokerAccountField.value = broker_wallet.address
    brokerPubKeyField.value = broker_wallet.publicKey
    brokerPrivKeyField.value = broker_wallet.privateKey
    brokerSeedField.value = broker_wallet.seed
    brokerBalanceField.value = (await client.getXrpBalance(broker_wallet.address))
```

ledger에서 연결을 끊습니다.

```javascript
  client.disconnect()
```

`getBalance()` 함수를 사용하여 법정 통화의 현재 잔액을 가져옵니다.

```javascript
  getBalances()

} // End of getAccountsFromSeeds()
```

### Get Balances <a href="#get-balances" id="get-balances"></a>

브로커 잔액을 포함하도록 `getBalances()` 함수를 재정의합니다.

```javascript
// *******************************************************
// ****************** Get Balances ***********************
// *******************************************************

async function getBalances() {
```

ledger에 연결합니다.

```javascript
  let net = getNet()
  const client = new xrpl.Client(net)
  results = 'Connecting to ' + getNet() + '....'
  document.getElementById('standbyResultField').value = results

  await client.connect()

  results += '\nConnected.'
  document.getElementById('standbyResultField').value = results
```

세 개의 계정을 각각 도출합니다.

```javascript
  const standby_wallet = xrpl.Wallet.fromSeed(standbySeedField.value)
  const operational_wallet = xrpl.Wallet.fromSeed(operationalSeedField.value)
  const broker_wallet = xrpl.Wallet.fromSeed(brokerSeedField.value)
```

Standby account 잔액을 가져오고 보고합니다.

```javascript
  results = "\nGetting standby account balances...\n"
  const standby_balances = await client.request({
    command: "gateway_balances",
    account: standby_wallet.address,
    ledger_index: "validated",
    hotwallet: [operational_wallet.address]
  })
  results += JSON.stringify(standby_balances.result, null, 2)
  standbyResultField.value = results
```

Operational account 잔액을 가져오고 보고합니다.

```javascript
  results= "\nGetting operational account balances...\n"
  const operational_balances = await client.request({javascript
    command: "account_lines",
    account: operational_wallet.address,
    ledger_index: "validated"
  })
  results += JSON.stringify(operational_balances.result, null, 2)
  operationalResultField.value = results
```

브로커 계정 잔액을 가져오고 보고합니다.

```javascript
  results= "\nGetting broker account balances...\n"
  const broker_balances = await client.request({
    command: "account_lines",
    account: broker_wallet.address,
    ledger_index: "validated"
  })
  results += JSON.stringify(broker_balances.result, null, 2)
  brokerResultField.value = results
```

세 계정의 XRP 잔액을 업데이트합니다.

```javascript
  operationalBalanceField.value = (await client.getXrpBalance(operational_wallet.address))
  standbyBalanceField.value = (await client.getXrpBalance(standby_wallet.address))
  brokerBalanceField.value = (await client.getXrpBalance(broker_wallet.address))
```

ledger에서 연결을 끊습니다.

```javascript
  client.disconnect()
} // end of getBalances()
```

### 5.broker-nfts.html <a href="#5broker-nftshtml" id="5broker-nftshtml"></a>

HTML 양식을 수정하여 맨 위에 새 브로커 섹션을 추가합니다.

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
    <script src='ripplex5-broker-nfts.js'></script>
    <script>
      if (typeof module !== "undefined") {
        const xrpl = require('xrpl')
      }
    </script>

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
      <textarea id="seeds" cols="40" rows= "3"></textarea>
      <br/><br/>
        <table align="center">
          <tr valign="top">
            <td>
              <button type="button" onClick="getAccount('broker')">Get New Broker Account</button>
            </td>
          </tr>
          <tr valign="top">
            <td align="right">
              Broker Account
            </td>
            <td>
              <input type="text" id="brokerAccountField" size="40"></input>
              <button type="button" id="another" onclick="brokerSale()">Broker Sale</button>
            </td>
            <td rowspan="7">
            <textarea id="brokerResultField" cols="40" rows="12"></textarea>
            </td>
          </tr>
          <tr>
            <td align="right">
              Public Key
            </td>
            <td>
              <input type="text" id="brokerPubKeyField" size="40"></input>
              <button type="button" onClick="brGetOffers()">Get Offers</button>
              <br>
            </td>
          </tr>
          <tr>
            <td align="right">
              Private Key
            </td>
            <td>
              <input type="text" id="brokerPrivKeyField" size="40"></input>
              <button type="button" onClick="brCancelOffer()">Cancel Offer</button>
              <br>
            </td>
          </tr>
          <tr>
            <td align="right">
              Seed
              <br>
            </td>
            <td>
              <input type="text" id="brokerSeedField" size="40"></input>
            </td>
          </tr>
          <tr>
            <td align="right">
              XRP Balance
            </td>
            <td>
              <input type="text" id="brokerBalanceField" size="40"></input>
              <br>
            </td>
          </tr>
          <tr>
            <td align="right">
              Amount
            </td>
            <td>
              <input type="text" id="brokerAmountField" size="40"></input>
              <br>
            </td>
          </tr>
          <tr>
            <td align="right">NFT ID</td>
            <td><input type="text" id="brokerTokenIdField" value="" size="80"/></td>
          </tr>
          <tr>
            <td align="right">Sell NFT Offer Index</td>
            <td><input type="text" id="brokerTokenSellOfferIndexField" value="" size="80"/></td>
          </tr>
          <tr>
            <td align="right">Buy NFT Offer Index</td>
            <td><input type="text" id="brokerTokenBuyOfferIndexField" value="" size="80"/></td>
          </tr>
          <tr>
            <td align="right">Owner</td>
            <td><input type="text" id="brokerOwnerField" value="" size="80"/></td>
          </tr>
          <tr>
            <td align="right">Broker Fee</td>
            <td><input type="text" id="brokerBrokerFeeField" value="" size="80"/></td>
          </tr>
        </table>
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
