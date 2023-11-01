# JavaScript를 이용한 계정 생성 및 XRP 전송(Create Accounts and Send XRP Using JavaScript)

이번 페이지에서 아래의 3가지를 배울 수 있습니다:

1. 테스트넷에 계정을 생성하고, 실제 가치가 없는 1000개의 테스트 XRP를 받습니다.
2. Seed Values에서 계정을 검색합니다.&#x20;
3. 계정 간에 XRP를 전송합니다.

계정을 만들면 오프라인에서 공개/개인 키 쌍을 받게 됩니다. 계정은 XRP로 자금을 조달할 때까지 원장에 표시되지 않습니다. 이 예시는 테스트넷용 계정을 만드는 방법을 보여드리며, 메인넷에서 사용할 수 있는 계정을 만드는 방법은 설명하지 않습니다.

<figure><img src="../../../../.gitbook/assets/quickstart2.png" alt=""><figcaption></figcaption></figure>

## 전제 조건

시작하려면 로컬 디스크에 새 폴더를 만들고 npm을 사용하여 JavaScript 라이브러리를 설치하세요.

```
    npm install xrpl
```

&#x20;[Quickstart Samples](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/quickstart/js/)를 다운로드하여 활용하세요.

{% hint style="info" %}
**Note:**

[Quickstart Samples](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/quickstart/js/)이 없으면 다음에 나오는 예제를 사용할 수 없습니다.
{% endhint %}

## 사용법

{% embed url="https://youtu.be/GX436F-FaV4" %}

**Test 계정 만들기:**

1. `1.get-accounts-send-xrp.html` 를 브라우저에서 열기
2. **'Testnet'** 또는 '**Devnet'**을 선택하기
3. **'Get New Standby Account'** 클릭하기
4. **'Get New Operational Account'** 클릭하기
5. Copy and paste the **Seeds** field in a persistent location, such as a Notepad, so that you can reuse the accounts after reloading the form.
6. Reloading 이후에도 계정을 재사용할 수 있도록 **Seeds** field를 메모장 등의 위치에 복사하여 붙여넣습니다.

<figure><img src="../../../../.gitbook/assets/quickstart3.png" alt=""><figcaption></figcaption></figure>

## 실전 예제

이 웹사이트의 소스 리포지토리에서 [Quickstart Samples](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/quickstart/js/)을 다운로드받을 수 있습니다.



### ripplex-1-send-xrp.js <a href="#ripplex-1-send-xrpjs" id="ripplex-1-send-xrpjs"></a>

이 예제는 모든 XRP 레저 네트워크, 테스트넷 또는 데브넷에서 사용할 수 있습니다. 코드를 업데이트하여 다른 또는 추가적인 XRP 레저 네트워크를 선택할 수 있습니다.

#### getNet() <a href="#getnet" id="getnet"></a>

```
// ******************************************************
// ************* Get the Preferred Network **************
// ******************************************************   

    function getNet() {
```

이 함수는 brute force(무차별 대입) if 문을 사용하여 선택한 네트워크 인스턴스를 검색하고 URI를 반환합니다.

```
  let net
  if (document.getElementById("tn").checked) net = "wss://s.altnet.rippletest.net:51233"
  if (document.getElementById("dn").checked) net = "wss://s.devnet.rippletest.net:51233"
  return net
} // End of getNet()
```

#### getAccount(type) <a href="#getaccounttype" id="getaccounttype"></a>

```
// *******************************************************
// ************* Get Account *****************************
// *******************************************************

async function getAccount(type) {
```

선택한 ledger를 가져옵니다.

```
  let net = getNet()
```

클라이언트를 인스턴스화합니다.

```
  const client = new xrpl.Client(net)
```

결과 변수를 사용하여 진행률 정보를 저장합니다.

```
  results = 'Connecting to ' + net + '....'
```

Null 값을 이용하여 default faucet를 사용합니다.

```
  let faucetHost = null
```

해당 결과 필드에 진행 상황을 보고합니다.

```
  if (type == 'standby') {
    standbyResultField.value = results
  } else {
    operationalResultField.value = results
  }
```

서버에 연결합니다.

```
  await client.connect()

  results += '\nConnected, funding wallet.'
  if (type == 'standby') {
    standbyResultField.value = results
  } else {
    operationalResultField.value = results
  }
```

test account 을 만들고 XRP를 받습니다.

```
  const my_wallet = (await client.fundWallet(null, { faucetHost })).wallet

  results += '\nGot a wallet.'
  if (type == 'standby') {
    standbyResultField.value = results
  } else {
    operationalResultField.value = results
  }       
```

계정의 현재 XRP 잔액을 확인합니다.

```
  const my_balance = (await client.getXrpBalance(my_wallet.address))  
```

standby account인 경우 standby account fields를 채웁니다.

```
  if (type == 'standby') {
    standbyAccountField.value = my_wallet.address
    standbyPubKeyField.value = my_wallet.publicKey
    standbyPrivKeyField.value = my_wallet.privateKey
    standbyBalanceField.value = (await client.getXrpBalance(my_wallet.address))
    standbySeedField.value = my_wallet.seed
    results += '\nStandby account created.'
    standbyResultField.value = results
```

그렇지 않으면 operational account fields를 채웁니다.

```
  } else {
    operationalAccountField.value = my_wallet.address
    operationalPubKeyField.value = my_wallet.publicKey
    operationalPrivKeyField.value = my_wallet.privateKey
    operationalSeedField.value = my_wallet.seed
    operationalBalanceField.value = (await client.getXrpBalance(my_wallet.address))
    results += '\nOperational account created.'
    operationalResultField.value = results
  }
```

편의를 위해 두 계정의 seed values를 생성한 그대로 **Seeds** field에 삽입합니다. 값을 복사하여 오프라인으로 저장할 수 있습니다. 이 튜토리얼에서 이 양식이나 다른 양식을 다시 로드할 때 복사하여 **Seeds** field에 붙여넣고 getAccountsFromSeeds() 함수를 사용하여 계정을 검색합니다.

```
  seeds.value = standbySeedField.value + '\n' + operationalSeedField.value
```

XRP Ledger 와 연결을 끊습니다.

```
  client.disconnect()
} // End of getAccount()
```

#### Get Accounts from Seeds <a href="#get-accounts-from-seeds" id="get-accounts-from-seeds"></a>

```
// *******************************************************
// ********** Get Accounts from Seeds ******************** 
// *******************************************************

async function getAccountsFromSeeds() {
```

선택한 네트워크에 연결합니다.

```
  let net = getNet()
  const client = new xrpl.Client(net)
  results = 'Connecting to ' + getNet() + '....'
  standbyResultField.value = results
  await client.connect()
  results += '\nConnected, finding wallets.\n'
  standbyResultField.value = results
```

**Seeds** field를 파싱(parse)합니다.

```
  var lines = seeds.value.split('\n')
```

첫 번째 줄의 seed를 기반으로 standby\_wallet을 가져옵니다. 두 번째 줄의 seed를 기반으로 operational\_wallet을 가져옵니다.

```
  const standby_wallet = xrpl.Wallet.fromSeed(lines[0])
  const operational_wallet = xrpl.Wallet.fromSeed(lines[1])
```

계정의 현재 XRP 잔액을 가져옵니다.

```
  const standby_balance = (await client.getXrpBalance(standby_wallet.address))  
  const operational_balance = (await client.getXrpBalance(operational_wallet.address))  
```

standby와 operational accounts 의 fields 를 채웁니다.

```
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
```

XRP Ledger에서 연결을 끊습니다.

```
  client.disconnect()
} // End of getAccountsFromSeeds()
```

#### Send XRP <a href="#send-xrp" id="send-xrp"></a>

```
// *******************************************************
// ******************** Send XRP *************************
// *******************************************************

async function sendXRP() {
```

선택한 ledger에 연결합니다.

```
  results  = "Connecting to the selected ledger.\n"
  standbyResultField.value = results
  let net = getNet()
  results = 'Connecting to ' + getNet() + '....'
  const client = new xrpl.Client(net)
  await client.connect()

  results  += "\nConnected. Sending XRP.\n"
  standbyResultField.value = results

  const standby_wallet = xrpl.Wallet.fromSeed(standbySeedField.value)
  const operational_wallet = xrpl.Wallet.fromSeed(operationalSeedField.value)
  const sendAmount = standbyAmountField.value

  results += "\nstandby_wallet.address: = " + standby_wallet.address
  standbyResultField.value = results
```

트랜잭션을 준비합니다. 대기 주소에서 운영 주소로의 결제 트랜잭션입니다.

결제 트랜잭션은 XRP를 방울, 즉 XRP의 100만분의 1로 표현할 것으로 예상합니다. xrpToDrops() 메서드를 사용하여 송금액을 변환할 수 있습니다(1XRP를 보내기 위해 0을 6개 더 입력하는 것보다 훨씬 간편합니다).

```
  const prepared = await client.autofill({
    "TransactionType": "Payment",
    "Account": standby_wallet.address,
    "Amount": xrpl.xrpToDrops(sendAmount),
    "Destination": standbyDestinationField.value
  })
```

준비된 거래에 서명합니다.

```
  const signed = standby_wallet.sign(prepared)
```

거래를 제출하고 결과를 기다립니다.

```
  const tx = await client.submitAndWait(signed.tx_blob)
```

거래로 인한 잔액 변경을 요청하고 결과를 보고합니다.

```
  results  += "\nBalance changes: " + 
    JSON.stringify(xrpl.getBalanceChanges(tx.result.meta), null, 2)
  standbyResultField.value = results

  standbyBalanceField.value =  (await client.getXrpBalance(standby_wallet.address))
  operationalBalanceField.value = (await client.getXrpBalance(operational_wallet.address))                 
  client.disconnect()    
} // End of sendXRP()
```



#### Reciprocal Transactions <a href="#reciprocal-transactions" id="reciprocal-transactions"></a>

각 트랜잭션에는 운영 계정에 대해 접두사 oP가 붙은 상호 트랜잭션이 수반됩니다. 코드 설명은 standby account의 해당 함수를 참조하세요.

```
// **********************************************************************
// ****** Reciprocal Transactions ***************************************
// **********************************************************************

// *******************************************************
// ********* Send XRP from Operational account ***********
// *******************************************************

async function oPsendXRP() {

  results  = "Connecting to the selected ledger.\n"
  operationalResultField.value = results
  let net = getNet()
  results = 'Connecting to ' + getNet() + '....'
  const client = new xrpl.Client(net)
  await client.connect()

  results  += "\nConnected. Sending XRP.\n"
  operationalResultField.value = results

  const operational_wallet = xrpl.Wallet.fromSeed(operationalSeedField.value)
  const standby_wallet = xrpl.Wallet.fromSeed(standbySeedField.value)
  const sendAmount = operationalAmountField.value

  results += "\noperational_wallet.address: = " + operational_wallet.address
  operationalResultField.value = results

// ---------------------------------------------------------- Prepare transaction
  const prepared = await client.autofill({
    "TransactionType": "Payment",
    "Account": operational_wallet.address,
    "Amount": xrpl.xrpToDrops(operationalAmountField.value),
    "Destination": operationalDestinationField.value
  })

// ---------------------------------------------------- Sign prepared instructions
  const signed = operational_wallet.sign(prepared)

// ------------------------------------------------------------ Submit signed blob
  const tx = await client.submitAndWait(signed.tx_blob)

  results  += "\nBalance changes: " +
    JSON.stringify(xrpl.getBalanceChanges(tx.result.meta), null, 2)
  operationalResultField.value = results
  standbyBalanceField.value = (await client.getXrpBalance(standby_wallet.address))
  operationalBalanceField.value = (await client.getXrpBalance(operational_wallet.address))                 

  client.disconnect()    
} // End of oPsendXRP()
```



### 1. get-accounts-send-xrp.html <a href="#1get-accounts-send-xrphtml" id="1get-accounts-send-xrphtml"></a>

트랜잭션 및 요청을 전송하는 표준 HTML 양식을 만든 다음 결과를 표시합니다.

```
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
                  </table>
                  <p align="right">
                    <textarea id="standbyResultField" cols="80" rows="20" ></textarea>
                  </p>
                </td>
                </td>
                <td>
                  <table>
                    <tr valign="top">
                      <td align="center" valign="top">
                        <button type="button" onClick="sendXRP()">Send XRP&#62;</button>
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
                  <table>
                    <tr>
                      <td align="center" valign="top">
                        <button type="button" onClick="oPsendXRP()">&#60;Send XRP</button>
                        </td>
                        <td align="right">
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
