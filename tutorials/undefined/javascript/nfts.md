# NFTs 일괄 발행

한 번에 여러 NFT를 만드는 응용 프로그램을 만들 수 있습니다. `for` loop를 사용하여 트랜잭션을 차례로 전송할 수 있습니다.

티켓을 사용하여 트랜잭션 시퀀스 번호를 예약하는 것이 좋습니다. 티켓을 사용하지 않고 NFT를 생성하는 응용 프로그램을 만들면 어떤 이유로든 트랜잭션이 실패하면 응용 프로그램이 오류와 함께 중지됩니다. 티켓을 사용하면 응용 프로그램에서 계속 트랜잭션을 전송하고 이후에 실패 원인을 조사할 수 있습니다.

<figure><img src="https://xrpl.org/img/quickstart33-batch-mint.png" alt=""><figcaption></figcaption></figure>

## 사용법

[Quickstart Samples](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/quickstart/js/) 아카이브를 다운로드하여 각 샘플을 브라우저에서 시도해볼 수 있습니다.

## 계정 가져오기&#x20;

1. 브라우저에서 `7.batch-minting.html`을 엽니다.&#x20;
2. 테스트 계정을 가져옵니다.&#x20;
   1. 기존 계정 seed를 사용하려는 경우:&#x20;
      1. 시드 필드에 계정 **Seed**를 붙여넣습니다.&#x20;
      2. **Get Account from Seed**를 클릭합니다.&#x20;
   2. 기존 계정 seed를 사용하지 않으려면 **Get New Standby Account**를 클릭합니다.&#x20;

{% hint style="info" %}
Note:

이 명령을 실행하면 riplex1-send-xrp.js의 getAccountsFromSeeds 함수가 이 양식에 포함되지 않은 작동 시드 필드를 찾기 때문에 JavaScript 콘솔에 오류가 발생합니다. 오류를 무시하거나 자체 구현에서 오류를 수정할 수 있습니다.
{% endhint %}

## NFTs 일괄 발행

이 예제를 사용하면 고유한 단일 항목에 대해 여러 NFT를 만들 수 있습니다. NFT는 원본 아트워크의 "인쇄", 이벤트 티켓 또는 다른 제한된 고유 항목 세트를 나타낼 수 있습니다.

non-fungible token 객체를 일괄적으로 작성하는 방법:

1. **Flags** 필드를 설정합니다. 테스트를 위해 값을 8로 설정하는 것이 좋습니다. 그러면 _tsTransferable_ flag가 설정됩니다. 즉, NFT 객체가 다른 계정으로 전송될 수 있습니다. 그렇지 않으면 NFT 객체는 발급 계정으로만 다시 전송될 수 있습니다. NFT를 만드는 데 사용할 수 있는 모든 flags에 대한 자세한 내용은 [NFTokenMint](https://xrpl.org/nftokenmint.html)를 참조하십시오.&#x20;
2. **토큰 URL**을 입력합니다. 이것은 NFT 개체와 관련된 데이터 또는 메타데이터를 가리키는 URI입니다. 자체 URI가 없는 경우 제공된 샘플 URI를 사용할 수 있습니다.&#x20;
3. 한 배치에 생성할 최대 200개의 NFT의 **Token Count**를 입력합니다.&#x20;
4. 원본 작성자가 NFT의 향후 판매에서 받는 수익의 백분율인 **Transfer Fee**를 입력합니다. 이 값은 0-50000이며 0.001%씩 0.000%에서 50.000% 사이의 전송 수수료를 허용합니다. NFT를 전송할 수 있도록 **Flags** 필드를 설정하지 않은 경우 이 필드를 0으로 설정합니다.&#x20;
5. **Batch Mint**를 클릭합니다.&#x20;

## Get Batch NFTs <a href="#get-batch-nfts" id="get-batch-nfts"></a>

계정에 대한 현재 NFT 목록을 가져오려면 **Get Batch NFTs**를 클릭합니다.

이 함수와 이전에 사용된 `getTokens()` 함수와의 차이점은 더 큰 토큰 목록을 처리할 수 있으며, 토큰의 수가 단일 요청에서 허용되는 개체 수를 초과하는 경우 여러 요청을 보냅니다.

## Code Walkthrough <a href="#code-walkthrough" id="code-walkthrough"></a>

[Quickstart Samples](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/quickstart/js/) 아카이브를 다운로드하여 각 샘플을 브라우저에서 시도해볼 수 있습니다.

### Get Account From Seed <a href="#get-account-from-seed" id="get-account-from-seed"></a>

이것은 Standby 계정 정보만 가져오는 getAccountsFromSeeds()의 제거된 버전입니다.

```javascript
async function getAccountFromSeed() {
```

XRP Ledger에 연결합니다.

```javascript
    let net = getNet()
    const client = new xrpl.Client(net)
    results = 'Connecting to ' + getNet() + '....'
    standbyResultField.value = results
    await client.connect()
    results += '\nConnected, finding wallets.\n'
    standbyResultField.value = results
```

Seed를 사용하여 계정을 파생합니다.

```javascript
    var theSeed = seeds.value
    const standby_wallet = xrpl.Wallet.fromSeed(theSeed)
```

현재 XRP 잔액을 가져옵니다.

```javascript
  const standby_balance = (await client.getXrpBalance(standby_wallet.address))  
```

Standby account의 필드를 채웁니다.

```javascript
    standbyAccountField.value = standby_wallet.address
    standbyPubKeyField.value = standby_wallet.publicKey
    standbyPrivKeyField.value = standby_wallet.privateKey
    standbySeedField.value = standby_wallet.seed
    standbyBalanceField.value = (await client.getXrpBalance(standby_wallet.address))
```

XRP Ledger에서 연결을 끊습니다.

```javascript
 client.disconnect()    
} 
```

### Get Batch NFTs <a href="#get-batch-nfts-1" id="get-batch-nfts-1"></a>

이 버전의 `getTokens()`은 각 NFTs batch의 끝에서 `marker`를 감시하여 더 큰 NFT 세트를 허용합니다.

```javascript
// *******************************************************
// **************** Get Batch Tokens *********************
// *******************************************************

async function getBatchNFTs() {
```

XRP Ledger에 연결하여 계정을 가져옵니다.

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

`account_nfts`를 요청합니다. 한 번의 배치에서 최대 400개의 레코드를 검색하려면 제한을 최대 400개로 설정합니다.

```javascript
  results += "\n\nNFTs:\n"
  let nfts = await client.request({
    method: "account_nfts",
    account: standby_wallet.classicAddress,
    limit: 400
  })
  results += JSON.stringify(nfts,null,2)
```

`NFTs`목록이 제한을 초과하면 다음 `account_nfts`요청에 대한 매개 변수로 사용할 수 있는 `marker` 필드가 결과에 포함됩니다. `marker`는 다음 레코드 배치가 시작되는 위치를 나타냅니다. `marker`필드가 있는 동안 다른 일괄 NFT 레코드를 계속해서 요청합니다.&#x20;

```javascript
  while (nfts.result.marker)
  {
    nfts = await client.request({
    method: "account_nfts",
    account: standby_wallet.classicAddress,
    limit: 400,
    marker: nfts.result.marker
  })
    results += '\n' + JSON.stringify(nfts,null,2)
  }
```

최종 결과를 보고합니다.

```javascript
  standbyResultField.value = results
```

XRP Ledger에서 연결을 끊습니다.

```javascript
  client.disconnect()
} //End of getBatchTokens()
```

### Batch Mint <a href="#batch-mint" id="batch-mint"></a>

이 스크립트는 동일한 NFT의 여러 복사본을 만듭니다.

```javascript
// *******************************************************
// ******************  Batch Mint  ***********************
// *******************************************************

async function batchMint() {
```

XRP Ledger에 연결하여 계정을 가져옵니다.

```javascript
  let net = getNet()
  const client = new xrpl.Client(net)
  results = 'Connecting to ' + getNet() + '....'
  standbyResultField.value = results
  await client.connect() 
  results += '\nConnected, finding wallet.'
  standbyResultField.value = results
  standby_wallet = xrpl.Wallet.fromSeed(standbySeedField.value)
  standbyResultField.value = results
```

계정 정보, 특히 `Sequence`번호를 가져옵니다.

```javascript
  const account_info = await client.request({
    "command": "account_info",
    "account": standby_wallet.address
  })

  my_sequence = account_info.result.account_data.Sequence
  results += "\n\nSequence Number: " + my_sequence + "\n\n"
  standbyResultField.value = results
```

그런 다음 배치에 대한 티켓 번호를 만듭니다. 티켓이 없으면 한 트랜잭션이 실패하면 배치의 다른 트랜잭션도 모두 실패합니다. 티켓은 실패가 있을 수 있지만 나머지는 성공할 수 있으며 이후에 문제를 조사할 수 있습니다.&#x20;

NFT Count 필드 값을 정수로 구문 분석합니다.

```javascript
  const nftCount = parseInt(standbyNFTCountField.value)
```

기본값을 자동으로 채우는 `TicketCreate` 트랜잭션 해시를 만듭니다. XRP Ledger의 시작점을 나타내는 `Sequence` 번호를 제공합니다.

```javascript
  const ticketTransaction = await client.autofill({
    "TransactionType": "TicketCreate",
    "Account": standby_wallet.address,
    "TicketCount": nftCount,
    "Sequence": my_sequence
  })
```

트랜잭션에 서명하고 제출합니다.

```javascript
   const signedTransaction = standby_wallet.sign(ticketTransaction)
   const tx = await client.submitAndWait(signedTransaction.tx_blob)
```

요청을 보내고 결과를 기다립니다.

```javascript
  let response = await client.request({
    "command": "account_objects",
    "account": standby_wallet.address,
    "type": "ticket"
  })
```

`tickets` 배열 변수를 채웁니다.

```javascript
  let tickets = []

  for (let i=0; i < nftCount; i++) {
    tickets[i] = response.result.account_objects[i].TicketSequence
  }
```

함수 진행 상황을 보고합니다.

```javascript
  results += "Tickets generated, minting NFTs.\n\n"
  standbyResultField.value = results
```

`for`loop를 사용하여 지정한 숫자까지 한 번에 하나씩 NFT를 만듭니다.

```javascript
  for (let i=0; i < nftCount; i++) {
        const transactionBlob = {
            "TransactionType": "NFTokenMint",
            "Account": standby_wallet.classicAddress,
            "URI": xrpl.convertStringToHex(standbyTokenUrlField.value),
            "Flags": parseInt(standbyFlagsField.value),
            "TransferFee": parseInt(standbyTransferFeeField.value),
            "Sequence": 0,
```

loop 변수를 사용하여 배열을 단계별로 표시합니다. `LastLedgerSequence`를 null로 설정합니다.

```javascript
            "TicketSequence": tickets[i],
            "LastLedgerSequence": null,
            "NFTokenTaxon": 0 
        }
```

서명된 트랜잭션 해시를 제출합니다.

```javascript
        const tx =  client.submit(transactionBlob, { wallet: standby_wallet} )
  }
```

위의 `getBatchNFTs`와 동일한 논리를 사용하여 현재 NFT의 목록을 가져옵니다.

```javascript
  results += "\n\nNFTs:\n"
  let nfts = await client.request({
    method: "account_nfts",
    account: standby_wallet.classicAddress,
    limit: 400
  })

  results += JSON.stringify(nfts,null,2)
  while (nfts.result.marker)
  {
        nfts = await client.request({
            method: "account_nfts",
            account: standby_wallet.classicAddress,
            limit: 400,
            marker: nfts.result.marker
        })
    results += '\n' + JSON.stringify(nfts,null,2)
  }
```

결과를 보고합니다.

```javascript
    results += '\n\nTransaction result: '+ tx.result.meta.TransactionResult
    results += '\n\nnfts: ' + JSON.stringify(nfts, null, 2)
    standbyBalanceField.value = (await client.getXrpBalance(standby_wallet.address))
    standbyResultField.value = results
```

XRP Ledger에서 연결을 끊습니다.

```javascript
  client.disconnect()
} // End of batchMint()
```

### 7.batch-minting.html <a href="#7batch-mintinghtml" id="7batch-mintinghtml"></a>

다음 양식의 경우: \* 불필요한 스크립트 참조가 제거됩니다. \* 사용하지 않는 필드와 단추가 제거됩니다. \* `standbyResultField` 용량은 최대값 524288로 설정됩니다.

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
    <script src='ripplex3-mint-nfts.js'></script>
    <script src='ripplex7-batch-minting.js'></script>
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
      <button type="button" onClick="getAccountFromSeed()">Get Account From Seed</button>
      <br/>
      <textarea id="seeds" cols="40" rows= "1" maxlength="524,288"></textarea>
      <br/><br/>
            <table>
                <tr valign="top">
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
                                <td><input type="text" id="standbyFlagsField" value="8" size="10"/></td>
                            </tr>
                            <tr>
                                <td align="right">NFT ID</td>
                                <td><input type="text" id="standbyTokenIdField" value="" size="80"/></td>
                            </tr>
                            <tr>
                                <td align="right">
                                    NFT Count
                                    </td>
                                    <td>
                                        <input type="text" id="standbyNFTCountField" size="40"></input>
                                        <br>
                                    </td>
                                </tr>
                                <tr>
                                    <td align="right">Transfer Fee</td>
                                    <td><input type="text" id="standbyTransferFeeField" value="" size="80"/></td>
                                </tr>
                              </td>
                            </tr>
                        </table>
                    </td>
                    <td align="left" valign="top">
                      <button type="button" onClick="batchMint()">Batch Mint</button>
                        <br/>
                        <button type="button" onClick="getBatchNFTs()">Get Batch NFTs</button>
                        <br/>
                        <p align="left">

<!-- Note the increased maxlength to hold the most possible NFT info. -->

            <textarea id="standbyResultField" cols="80" rows="20" maxlength="524288"></textarea>
            </p>
          </td>
        </tr>
      </table>
    </form>
  </body>
</html>
```
