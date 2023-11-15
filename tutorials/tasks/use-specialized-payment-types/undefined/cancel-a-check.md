---
description: 수표 개정에 의해 추가되었습니다.
---

# 수표 취소(Cancel a Check)

이 튜토리얼에서는 송금하지 않고 수표 객체를 ledger으로부터 제거하는 수표 취소 방법을 설명합니다.

수표를 원하지 않는 경우 수신 수표를 취소할 수 있습니다. 송금할 때 실수를 했거나 상황이 변경된 경우 보내는 수표를 취소할 수 있습니다. 수표의 유효기간이 만료된 경우에도 수표를 취소하여 ledger에서 제거해야 송금인이 소유자 reserve를돌려받을 수 있습니다.

## 요구 조건(Prerequisites)

이 튜토리얼에서 수표를 취소하려면 다음이 필요합니다:

* 현재 ledger에 있는 수표 객체의 ID가 필요합니다.
  * 예를 들어, 이 튜토리얼에는 ID가 49647F0D748DC3FE26BDACBC57F251AADEFFF391403EC9BF87C97F67E9977FB0인 수표를 취소하는 예제가 포함되어 있지만, 이 단계를 직접 수행하려면 다른 ID를 사용해야 합니다.
* CheckCancel 트랜잭션을 전송할 자금이 입금된 계좌의 주소와 비밀 키입니다. 이 주소는 수표가 만료되지 않는 한 수표의 발신자 또는 수취인이어야 합니다.
* 트랜잭션에 서명하는 안전한 방법.
* 클라이언트 라이브러리 또는 HTTP 또는 웹소켓 라이브러리.

## 1. CheckCancel 트랜잭션 준비하기(Prepare the CheckCancel transaction)

CheckCancel 트랜잭션 필드의 값을 파악합니다. 다음 필드는 최소값이며, 다른 모든 필드는 선택사항이거나 서명할 때 자동으로 채워질 수 있습니다:

| 필드                | 값                | 설명명                                                                                                                           |
| ----------------- | ---------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `TransactionType` | String           | 수표를 취소할 때 CheckCancel 문자열을 사용합니다.                                                                                             |
| `Account`         | String (Address) | 계정 문자열(주소) 수표를 취소하는 발신자의 주소입니다. (즉, 사용자 주소입니다.)                                                                               |
| `CheckID`         | String           | 취소할 ledger에 있는 수표 객체의 ID입니다. 이 정보는 tx 메소드를 사용하여 CheckCreate 트랜잭션의 메타데이터를 조회하거나 account\_objects 메소드를 사용하여 수표를 조회하여 얻을 수 있습니다. |

## CheckCancel 준비 예시(Example CheckCancel Preparation)

다음 예는 수표를 취소하는 방법을 보여줍니다.

{% tabs %}
{% tab title="JSON-RPC WebSocket, or Commandline" %}
```json
{
  "TransactionType": "CheckCancel",
  "Account": "rUn84CUYbNjRoTQ6mSW7BVJPSVJNLb1QLo",
  "CheckID": "49647F0D748DC3FE26BDACBC57F251AADEFFF391403EC9BF87C97F67E9977FB0",
  "Fee": "12"
}
```
{% endtab %}
{% endtabs %}

## 2. CheckCancel 트랜잭션에 서명하기(Sign the CheckCancel transaction)

트랜잭션에 서명하는 가장 안전한 방법은 클라이언트 라이브러리를 사용하여 로컬로 서명하는 것입니다. 또는 자체 rippled 노드를 실행하는 경우 서명 방법을 사용하여 트랜잭션에 서명할 수 있지만, 신뢰할 수 있고 암호화된 연결 또는 로컬(동일 컴퓨터) 연결을 통해 수행해야 합니다.

어떤 경우든 나중에 사용할 수 있도록 서명된 트랜잭션의 식별 해시를 기록해 두세요.

## 요청 예시(Example Request)

{% tabs %}
{% tab title="ripple-lib 1.x" %}
```javascript
'use strict'
const RippleAPI = require('ripple-lib').RippleAPI

// Can sign offline if the txJSON has all required fields
const api = new RippleAPI()

const txJSON = '{"Account":"rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za","TransactionType":"CheckCancel","CheckID":"2E0AD0740B79BE0AAE5EDD1D5FC79E3C5C221D23C6A7F771D85569B5B91195C2","Flags":2147483648,"LastLedgerSequence":8004884,"Fee":"12","Sequence":7}'

// Be careful where you store your real secret.
const secret = 's████████████████████████████'

const signed = api.sign(txJSON, secret)

console.log("tx_blob is:", signed.signedTransaction)
console.log("tx hash is:", signed.id)
```
{% endtab %}

{% tab title="Commandline" %}
```json
rippled sign s████████████████████████████ '{
  "TransactionType": "CheckCancel",
  "Account": "rUn84CUYbNjRoTQ6mSW7BVJPSVJNLb1QLo",
  "CheckID": "49647F0D748DC3FE26BDACBC57F251AADEFFF391403EC9BF87C97F67E9977FB0",
  "Fee": "12"
}'
```
{% endtab %}
{% endtabs %}

## 응답 예시(Example Response)

{% tabs %}
{% tab title="ripple-lib 1.x" %}
```javascript
tx_blob is: 12001222800000002400000007201B007A251450182E0AD0740B79BE0AAE5EDD1D5FC79E3C5C221D23C6A7F771D85569B5B91195C268400000000000000C732103B6FCD7FAC4F665FE92415DD6E8450AD90F7D6B3D45A6CFCF2E359045FF4BB4007446304402205D77451B0D7BCDA1FE5B98763C5B3B2837453371FE93C2B86157C44B1867AE36022003800273848BC2F8E1C6EC7EE4B0CB2425A888AE80E586886C306C796B25678B8114735FF88E5269C80CD7F7AF10530DAB840BBF6FDF
tx hash is: 54A7A917BE9AC13962251BCF1D09803C7BBE75882B8BFC987B5933A566A48215
```
{% endtab %}

{% tab title="Commandline" %}
```json
Loading: "/etc/opt/ripple/rippled.cfg"
2018-Jan-24 01:11:07 HTTPClient:NFO Connecting to 127.0.0.1:5005

{
   "result" : {
      "status" : "success",
      "tx_blob" : "12001222800000002400000003501849647F0D748DC3FE26BDACBC57F251AADEFFF391403EC9BF87C97F67E9977FB068400000000000000C7321022C53CD19049F32F31848DD3B3BE5CEF6A2DD1EFDA7971AB3FA49B1BAF12AEF78744630440220615F9D19FA182F08530CD978A4C216C8676D0BA9EDB53A620AC909AA0EF0FE7E02203A09CC34C3DB85CCCB3137E78081F8F2B441FB0A3B9E40901F312D3CBA0A67A181147990EC5D1D8DF69E070A968D4B186986FDF06ED0",
      "tx_json" : {
         "Account" : "rUn84CUYbNjRoTQ6mSW7BVJPSVJNLb1QLo",
         "CheckID" : "49647F0D748DC3FE26BDACBC57F251AADEFFF391403EC9BF87C97F67E9977FB0",
         "Fee" : "12",
         "Flags" : 2147483648,
         "Sequence" : 3,
         "SigningPubKey" : "022C53CD19049F32F31848DD3B3BE5CEF6A2DD1EFDA7971AB3FA49B1BAF12AEF78",
         "TransactionType" : "CheckCancel",
         "TxnSignature" : "30440220615F9D19FA182F08530CD978A4C216C8676D0BA9EDB53A620AC909AA0EF0FE7E02203A09CC34C3DB85CCCB3137E78081F8F2B441FB0A3B9E40901F312D3CBA0A67A1",
         "hash" : "414558223CA8595916BB1FEF238B3BB601B7C0E52659292251CE613E6B4370F9"
      }
   }
}
```
{% endtab %}
{% endtabs %}

## 3. 서명된 CheckCancel 트랜잭션 제출하기(Submit the signed CheckCancel transaction)

이전 단계에서 서명된 트랜잭션 blob을 가져와 rippled 서버에 제출합니다. rippled 서버를 실행하지 않아도 안전하게 이 작업을 수행할 수 있습니다. 응답에는 임시 결과가 포함되며, 이 결과는 일반적으로 tesSUCCESS여야 하지만 최종 결과는 아닙니다. 대기 중인 트랜잭션은 일반적으로 다음 오픈 ledger 버전에 포함되므로(일반적으로 제출 후 약 10초 후), terQUEUED의 임시 응답도 괜찮습니다.

{% hint style="info" %}
Tip:

예비 결과가 tefMAX\_LEDGER인 경우, 트랜잭션이 영구적으로 실패한 것은 LastLedgerSequence 매개변수가 현재 ledger보다 낮기 때문입니다. 이는 트랜잭션 준비와 제출 사이에 예상되는 ledger 버전 수보다 오래 걸리는 경우 발생합니다. 이 경우 더 높은 LastLedgerSequence 값으로 1단계부터 다시 시작하세요.
{% endhint %}

## 요청 예시(Example Request)

{% tabs %}
{% tab title="ripple-lib 1.x" %}
```javascript
'use strict'
const RippleAPI = require('ripple-lib').RippleAPI

// This example connects to a public Test Net server
const api = new RippleAPI({server: 'wss://s.altnet.rippletest.net:51233'})
api.connect().then(() => {
  console.log('Connected')

  const tx_blob = "12001222800000002400000007201B007A251450182E0AD0740B79BE0AAE5EDD1D5FC79E3C5C221D23C6A7F771D85569B5B91195C268400000000000000C732103B6FCD7FAC4F665FE92415DD6E8450AD90F7D6B3D45A6CFCF2E359045FF4BB4007446304402205D77451B0D7BCDA1FE5B98763C5B3B2837453371FE93C2B86157C44B1867AE36022003800273848BC2F8E1C6EC7EE4B0CB2425A888AE80E586886C306C796B25678B8114735FF88E5269C80CD7F7AF10530DAB840BBF6FDF"

  return api.submit(tx_blob)
}).then(response => {
  console.log("Preliminary transaction result:", response.resultCode)

// Disconnect and return
}).then(() => {
  api.disconnect().then(() => {
    console.log('Disconnected')
    process.exit()
  })
}).catch(console.error)
```
{% endtab %}

{% tab title="Commandline" %}
```json
rippled submit 12001222800000002400000003501849647F0D748DC3FE26BDACBC57F251AADEFFF391403EC9BF87C97F67E9977FB068400000000000000C7321022C53CD19049F32F31848DD3B3BE5CEF6A2DD1EFDA7971AB3FA49B1BAF12AEF78744630440220615F9D19FA182F08530CD978A4C216C8676D0BA9EDB53A620AC909AA0EF0FE7E02203A09CC34C3DB85CCCB3137E78081F8F2B441FB0A3B9E40901F312D3CBA0A67A181147990EC5D1D8DF69E070A968D4B186986FDF06ED0
```
{% endtab %}
{% endtabs %}

## 응답 예시(Example Response)

{% tabs %}
{% tab title="ripple-lib 1.x" %}
```javascript
Connected
Preliminary transaction result: tesSUCCESS
Disconnected
```
{% endtab %}

{% tab title="Commandline" %}
```json
Loading: "/etc/opt/ripple/rippled.cfg"
2018-Jan-24 01:11:07 HTTPClient:NFO Connecting to 127.0.0.1:5005

{
  "result" : {
    "engine_result" : "tesSUCCESS",
    "engine_result_code" : 0,
    "engine_result_message" : "The transaction was applied. Only final in a validated ledger.",
    "status" : "success",
    "tx_blob" : "12001222800000002400000003501849647F0D748DC3FE26BDACBC57F251AADEFFF391403EC9BF87C97F67E9977FB068400000000000000C7321022C53CD19049F32F31848DD3B3BE5CEF6A2DD1EFDA7971AB3FA49B1BAF12AEF78744630440220615F9D19FA182F08530CD978A4C216C8676D0BA9EDB53A620AC909AA0EF0FE7E02203A09CC34C3DB85CCCB3137E78081F8F2B441FB0A3B9E40901F312D3CBA0A67A181147990EC5D1D8DF69E070A968D4B186986FDF06ED0",
    "tx_json" : {
      "Account" : "rUn84CUYbNjRoTQ6mSW7BVJPSVJNLb1QLo",
      "CheckID" : "49647F0D748DC3FE26BDACBC57F251AADEFFF391403EC9BF87C97F67E9977FB0",
      "Fee" : "12",
      "Flags" : 2147483648,
      "Sequence" : 3,
      "SigningPubKey" : "022C53CD19049F32F31848DD3B3BE5CEF6A2DD1EFDA7971AB3FA49B1BAF12AEF78",
      "TransactionType" : "CheckCancel",
      "TxnSignature" : "30440220615F9D19FA182F08530CD978A4C216C8676D0BA9EDB53A620AC909AA0EF0FE7E02203A09CC34C3DB85CCCB3137E78081F8F2B441FB0A3B9E40901F312D3CBA0A67A1",
      "hash" : "414558223CA8595916BB1FEF238B3BB601B7C0E52659292251CE613E6B4370F9"
    }
  }
}
```
{% endtab %}
{% endtabs %}

## 4. 유효성 검사 대기(Wait for validation)

라이브 네트워크(mainnet, testnet 또는 devnet 포함)에서는 ledger이 자동으로 닫힐 때까지 4\~7초 정도 기다릴 수 있습니다.

stand-alone 모드에서 rippled을 실행하는 경우 ledger\_accept 메소드를 사용하여 ledger를 수동으로 닫아야 합니다.

## 5. 최종 결과 확인(Confirm final result)

tx 메소드와 CheckCancel 트랜잭션의 식별 해시를 사용하여 트랜잭션의 상태를 확인합니다. 트랜잭션의 메타데이터에서 "TransactionResult": "tesSUCCESS" 필드와 이 결과가 최종 결과임을 나타내는 "validated": true 필드가 있는지 확인합니다.

트랜잭션 메타데이터에서 "LedgerEntryType": "Check"가 있는 DeletedNode 객체를 찾습니다. 이것은 트랜잭션이 수표 ledger 객체를 제거했음을 나타냅니다. 이 객체의 LedgerIndex는 수표의 ID와 일치해야 합니다.

## 요청 예시(Example Request)

{% tabs %}
{% tab title="ripple-lib 1.x" %}
```javascript
'use strict'
const RippleAPI = require('ripple-lib').RippleAPI

// This example connects to a public Test Net server
const api = new RippleAPI({server: 'wss://s.altnet.rippletest.net:51233'})
api.connect().then(() => {
  console.log('Connected')

  const tx_hash = "54A7A917BE9AC13962251BCF1D09803C7BBE75882B8BFC987B5933A566A48215"

  return api.getTransaction(tx_hash)
}).then(response => {
  console.log("Final transaction result:", response)

// Disconnect and return
}).then(() => {
  api.disconnect().then(() => {
    console.log('Disconnected')
    process.exit()
  })
}).catch(console.error)
```
{% endtab %}

{% tab title="Commandline" %}
```json
rippled tx 414558223CA8595916BB1FEF238B3BB601B7C0E52659292251CE613E6B4370F9
```
{% endtab %}
{% endtabs %}

## 응답 예시(Example Response)

{% tabs %}
{% tab title="ripple-lib 1.x" %}
```javascript
Connected
Final transaction result: { type: 'checkCancel',
  address: 'rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za',
  sequence: 7,
  id: '54A7A917BE9AC13962251BCF1D09803C7BBE75882B8BFC987B5933A566A48215',
  specification:
   { checkID: '2E0AD0740B79BE0AAE5EDD1D5FC79E3C5C221D23C6A7F771D85569B5B91195C2' },
  outcome:
   { result: 'tesSUCCESS',
     timestamp: '2018-04-02T23:42:22.000Z',
     fee: '0.000012',
     balanceChanges: { rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za: [Array] },
     orderbookChanges: {},
     ledgerVersion: 8004870,
     indexInLedger: 3 } }
Disconnected
```
{% endtab %}

{% tab title="Commandline" %}
```json
Loading: "/etc/opt/ripple/rippled.cfg"
2018-Jan-24 01:11:53 HTTPClient:NFO Connecting to 127.0.0.1:5005

{
   "result" : {
      "Account" : "rUn84CUYbNjRoTQ6mSW7BVJPSVJNLb1QLo",
      "CheckID" : "49647F0D748DC3FE26BDACBC57F251AADEFFF391403EC9BF87C97F67E9977FB0",
      "Fee" : "12",
      "Flags" : 2147483648,
      "Sequence" : 3,
      "SigningPubKey" : "022C53CD19049F32F31848DD3B3BE5CEF6A2DD1EFDA7971AB3FA49B1BAF12AEF78",
      "TransactionType" : "CheckCancel",
      "TxnSignature" : "30440220615F9D19FA182F08530CD978A4C216C8676D0BA9EDB53A620AC909AA0EF0FE7E02203A09CC34C3DB85CCCB3137E78081F8F2B441FB0A3B9E40901F312D3CBA0A67A1",
      "date" : 570071520,
      "hash" : "414558223CA8595916BB1FEF238B3BB601B7C0E52659292251CE613E6B4370F9",
      "inLedger" : 7,
      "ledger_index" : 7,
      "meta" : {
         "AffectedNodes" : [
            {
               "ModifiedNode" : {
                  "FinalFields" : {
                     "Flags" : 0,
                     "Owner" : "rfkE1aSy9G8Upk4JssnwBxhEv5p4mn2KTy",
                     "RootIndex" : "032D861D151E38E86F46805ED1896D1A50144F65459717B6D12470A9E6E3B66E"
                  },
                  "LedgerEntryType" : "DirectoryNode",
                  "LedgerIndex" : "032D861D151E38E86F46805ED1896D1A50144F65459717B6D12470A9E6E3B66E"
               }
            },
            {
               "DeletedNode" : {
                  "FinalFields" : {
                     "Account" : "rUn84CUYbNjRoTQ6mSW7BVJPSVJNLb1QLo",
                     "Destination" : "rfkE1aSy9G8Upk4JssnwBxhEv5p4mn2KTy",
                     "DestinationNode" : "0000000000000000",
                     "DestinationTag" : 1,
                     "Expiration" : 570113521,
                     "Flags" : 0,
                     "InvoiceID" : "46060241FABCF692D4D934BA2A6C4427CD4279083E38C77CBE642243E43BE291",
                     "OwnerNode" : "0000000000000000",
                     "PreviousTxnID" : "5463C6E08862A1FAE5EDAC12D70ADB16546A1F674930521295BC082494B62924",
                     "PreviousTxnLgrSeq" : 6,
                     "SendMax" : "100000000",
                     "Sequence" : 2
                  },
                  "LedgerEntryType" : "Check",
                  "LedgerIndex" : "49647F0D748DC3FE26BDACBC57F251AADEFFF391403EC9BF87C97F67E9977FB0"
               }
            },
            {
               "ModifiedNode" : {
                  "FinalFields" : {
                     "Flags" : 0,
                     "Owner" : "rUn84CUYbNjRoTQ6mSW7BVJPSVJNLb1QLo",
                     "RootIndex" : "AD136EC2A266027D8F202C97D294BBE32F6FC2AD5501D9853F785FE77AB94C94"
                  },
                  "LedgerEntryType" : "DirectoryNode",
                  "LedgerIndex" : "AD136EC2A266027D8F202C97D294BBE32F6FC2AD5501D9853F785FE77AB94C94"
               }
            },
            {
               "ModifiedNode" : {
                  "FinalFields" : {
                     "Account" : "rUn84CUYbNjRoTQ6mSW7BVJPSVJNLb1QLo",
                     "Balance" : "4999999999964",
                     "Flags" : 0,
                     "OwnerCount" : 1,
                     "Sequence" : 4
                  },
                  "LedgerEntryType" : "AccountRoot",
                  "LedgerIndex" : "D3A1DBAA28717975A9119EC4CBC891BA9A66236C484F03C9911F463AD3B66DE0",
                  "PreviousFields" : {
                     "Balance" : "4999999999976",
                     "OwnerCount" : 2,
                     "Sequence" : 3
                  },
                  "PreviousTxnID" : "5463C6E08862A1FAE5EDAC12D70ADB16546A1F674930521295BC082494B62924",
                  "PreviousTxnLgrSeq" : 6
               }
            }
         ],
         "TransactionIndex" : 0,
         "TransactionResult" : "tesSUCCESS"
      },
      "status" : "success",
      "validated" : true
   }
}
```
{% endtab %}
{% endtabs %}
