---
description: 수표 개정에 의해 추가되었습니다.
---

# 수표 보내기(Send a Check)

수표를 보내는 것은 의도한 수취인에게 결제를 받을 수 있는 권한을 부여하는 것과 같습니다. 이 프로세스의 결과는 수취인이 나중에 현금화할 수 있는 수표 객체가 ledger에 생성됩니다.

수표 대신 지급을 보내면 한 번에 수취인에게 직접 돈을 전달할 수 있으므로 많은 경우에 수표 대신 지급을 보내려고 합니다. 그러나 수취인이 DepositAuth를 사용하는 경우 직접 지급을 보낼 수 없으므로 수표를 보내는 것이 좋습니다.

이 튜토리얼에서는 가상의 회사인 BoxSend SG(XRP ledger 주소는 rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za)가 가상의 암호화폐 컨설팅 회사인 Grand Payments(XRP ledger 주소는 rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis)에 일부 컨설팅 작업에 대한 비용을 지불하는 예를 사용했습니다. Grand Payments는 XRP로 결제하는 것을 선호하지만, 세금과 규제를 간소화하기 위해 명시적으로 승인한 결제만 허용합니다.

Grand Payments는 XRP ledger 외부에서 ID 46060241FABCF692D4D934BA2A6C4427CD4279083E38C77CBE642243E43BE291로 BoxSend SG에 인보이스를 보내고, Grand Payment의 XRP ledger 주소인 rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis로 100 XRP 수표를 보내줄 것을 요청합니다.

## 요구 조건(Prerequisites)

이 튜토리얼로 수표를 보내려면 다음이 필요합니다:

* 수표를 보낼 자금이 입금된 계좌의 주소와 비밀 키.
  * XRP Ledger의 testnet Faucet을 사용해 10,000개의 testnet XRP로 펀딩된 주소와 비밀키를 얻을 수 있습니다.
* 수표를 받을 펀딩된 계좌의 주소입니다.
* 트랜잭션에 서명하는 안전한 방법.
* 클라이언트 라이브러리 또는 HTTP 또는 WebSocket 라이브러리.

## 1. 수표 만들기 트랜잭션 준비(Prepare the CheckCreate transaction)

수표의 금액과 현금화할 수 있는 사람을 결정합니다. CheckCreate 트랜잭션 필드의 값을 파악합니다. 다음 필드는 최소값이며, 다른 모든 필드는 선택 사항이거나 서명할 때 자동으로 채워질 수 있습니다:

| 필드                | 값                         | 설명명                                                                                                                                                                                                                                                                                                               |
| ----------------- | ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `TransactionType` | String                    | 여기에 CheckCreate 문자열을 사용합니다.                                                                                                                                                                                                                                                                                       |
| `Account`         | String (Address)          | 수표를 생성하는 발신자의 주소입니다. (즉, 사용자 주소입니다.)                                                                                                                                                                                                                                                                              |
| `Destination`     | String (Address)          | 수표를 현금화할 수 있는 수취인의 주소입니다.                                                                                                                                                                                                                                                                                         |
| `SendMax`         | String or Object (Amount) | 이 수표가 현금화될 때 송금인이 인출할 수 있는 최대 금액입니다. XRP의 경우 XRP drops 나타내는 문자열을 사용합니다. 토큰의 경우 통화, 발행자 및 값 필드가 있는 객체를 사용합니다. 자세한 내용은 통화 금액 지정을 참조하세요. 받는 사람이 이체 수수료가 포함된 정확한 금액의 비XRP 통화로 수표를 현금화할 수 있도록 하려면 이체 수수료를 지불할 추가 비율을 포함해야 합니다. (예를 들어 수취인이 송금 수수료가 2%인 발행자의 수표를 100캐나다달러로 현금화하려면 해당 발행자의 SendMax를 102캐나다달러로 설정해야 합니다.) |

## 수표 작성 준비 예시(Example CheckCreate Preparation)

다음 예는 BoxSend SG(rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za)에서 Grand Payments(rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis)로 100 XRP에 대해 준비된 수표를 보여 줍니다. 추가(선택 사항) 메타데이터로 BoxSend SG는 대금결제의 인보이스 ID를 추가하여 대금결제가 이 수표 어떤 인보이스를 결제할 것인지 알 수 있도록 합니다.

{% tabs %}
{% tab title="ripple-lib 1.x" %}
```javascript
'use strict'
const RippleAPI = require('ripple-lib').RippleAPI

// This example connects to a public Test Net server
const api = new RippleAPI({server: 'wss://s.altnet.rippletest.net:51233'})
api.connect().then(() => {
  console.log('Connected')

  const sender = 'rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za'
  const receiver = 'rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis'
  const options = {
    // Allow up to 60 ledger versions (~5 min) instead of the default 3 versions
    // before this transaction fails permanently.
    "maxLedgerVersionOffset": 60
  }
  return api.prepareCheckCreate(sender, {
    "destination": receiver,
    "sendMax": {
      "currency": "XRP",
      "value": "100" // RippleAPI uses decimal XRP, not integer drops
    },
    "invoiceID": "46060241FABCF692D4D934BA2A6C4427CD4279083E38C77CBE642243E43BE291"
  }, options)

}).then(prepared => {
  console.log("txJSON:", prepared.txJSON);

// Disconnect and return
}).then(() => {
  api.disconnect().then(() => {
    console.log('Disconnected')
    process.exit()
  })
}).catch(console.error)


// Example output:
//
// Connected
// txJSON: {"Account":"rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za",
//  "TransactionType":"CheckCreate",
//  "Destination":"rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis",
//  "SendMax":"100000000",
//  "Flags":2147483648,
//  "InvoiceID": "46060241FABCF692D4D934BA2A6C4427CD4279083E38C77CBE642243E43BE291",
//  "LastLedgerSequence":7835917,"Fee":"12","Sequence":2}
// Disconnected
```
{% endtab %}

{% tab title="JSON-RPC, WebSocket, or Commandline" %}
```json
{
  "TransactionType": "CheckCreate",
  "Account": "rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za",
  "Destination": "rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis",
  "SendMax": "100000000",
  "InvoiceID": "46060241FABCF692D4D934BA2A6C4427CD4279083E38C77CBE642243E43BE291"
}
```
{% endtab %}
{% endtabs %}

## 2. 수표 생성 트랜잭션에 서명(Sign the CheckCreate transaction)

트랜잭션에 서명하는 가장 안전한 방법은 클라이언트 라이브러리를 사용하여 로컬로 서명하는 것입니다. 또는 자체 rippled 노드를 실행하는 경우 서명 방법을 사용하여 트랜잭션에 서명할 수 있지만 신뢰할 수 있고 암호화된 연결 또는 로컬(동일 컴퓨터) 연결을 통해 수행해야 합니다.

어떤 경우든 나중에 사용할 수 있도록 서명된 트랜잭션의 식별 해시를 기록해 두세요.

## 요청 예시(Example Request)

{% tabs %}
{% tab title="ripple-lib 1.x" %}
```javascript
'use strict'
const RippleAPI = require('ripple-lib').RippleAPI

// Can sign offline if the txJSON has all required fields
const api = new RippleAPI()

const txJSON = '{"Account":"rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za", \
  "TransactionType":"CheckCreate", \
  "Destination":"rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis", \
  "SendMax":"100000000", \
  "Flags":2147483648, \
  "LastLedgerSequence":7835923, \
  "Fee":"12", \
  "Sequence":2}'

// Be careful where you store your real secret.
const secret = 's████████████████████████████'

const signed = api.sign(txJSON, secret)

console.log("tx_blob is:", signed.signedTransaction)
console.log("tx hash is:", signed.id)
```
{% endtab %}

{% tab title="WebSocket" %}
```json
{
  "id": "sign_req_1",
  "command": "sign",
  "tx_json": {
    "TransactionType": "CheckCreate",
    "Account": "rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za",
    "Destination": "rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis",
    "SendMax": "100000000",
    "InvoiceID": "46060241FABCF692D4D934BA2A6C4427CD4279083E38C77CBE642243E43BE291",
    "DestinationTag": 1,
    "Fee": "12"
  },
   "secret" : "s████████████████████████████"
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
rippled sign s████████████████████████████ '{
  "TransactionType": "CheckCreate",
  "Account": "rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za",
  "Destination": "rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis",
  "SendMax": "100000000",
  "Expiration": 570113521,
  "InvoiceID": "46060241FABCF692D4D934BA2A6C4427CD4279083E38C77CBE642243E43BE291",
  "DestinationTag": 1,
  "Fee": "12"
}'
```
{% endtab %}
{% endtabs %}

## 응답 예시(**Example Response)**

{% tabs %}
{% tab title="ripple-lib 1.x" %}
```javascript
tx_blob is: 12001022800000002400000002201B0077911368400000000000000C694000000005F5E100732103B6FCD7FAC4F665FE92415DD6E8450AD90F7D6B3D45A6CFCF2E359045FF4BB400744630440220181FE2F945EBEE632966D5FB03114611E3047ACD155AA1BDB9DF8545C7A2431502201E873A4B0D177AB250AF790CE80621E16F141506CF507586038FC4A8E95887358114735FF88E5269C80CD7F7AF10530DAB840BBF6FDF8314A8B6B9FF3246856CADC4A0106198C066EA1F9C39
tx hash is: C0B27D20669BAB837B3CDF4B8148B988F17CE1EF8EDF48C806AE9BF69E16F441
```
{% endtab %}

{% tab title="WebSocket" %}
```json
{
  "id": "sign_req_1",
  "result": {
    "tx_blob": "120010228000000024000000042E00000001501146060241FABCF692D4D934BA2A6C4427CD4279083E38C77CBE642243E43BE29168400000000000000C694000000005F5E100732103B6FCD7FAC4F665FE92415DD6E8450AD90F7D6B3D45A6CFCF2E359045FF4BB40074463044022071A341F911A8EF3B68399487CAF5BA3B59C6FE476B626698AEF044B8183721BC0220166053A859BD907251DFCCF34DD71202180EBABAE7098BB5903D16EBFC993C408114735FF88E5269C80CD7F7AF10530DAB840BBF6FDF8314A8B6B9FF3246856CADC4A0106198C066EA1F9C39",
    "tx_json": {
      "Account": "rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za",
      "Destination": "rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis",
      "DestinationTag": 1,
      "Fee": "12",
      "Flags": 2147483648,
      "InvoiceID": "46060241FABCF692D4D934BA2A6C4427CD4279083E38C77CBE642243E43BE291",
      "SendMax": "100000000",
      "Sequence": 4,
      "SigningPubKey": "03B6FCD7FAC4F665FE92415DD6E8450AD90F7D6B3D45A6CFCF2E359045FF4BB400",
      "TransactionType": "CheckCreate",
      "TxnSignature": "3044022071A341F911A8EF3B68399487CAF5BA3B59C6FE476B626698AEF044B8183721BC0220166053A859BD907251DFCCF34DD71202180EBABAE7098BB5903D16EBFC993C40",
      "hash": "09D992D4C89E2A24D4BA9BB57ED81C7003815940F39B7C87ADBF2E49034380BB"
    }
  },
  "status": "success",
  "type": "response"
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
Loading: "/etc/opt/ripple/rippled.cfg"
2018-Mar-21 21:00:05 HTTPClient:NFO Connecting to 127.0.0.1:5005

{
   "result" : {
      "status" : "success",
      "tx_blob" : "120010228000000024000000012A21FB3DF12E00000001501146060241FABCF692D4D934BA2A6C4427CD4279083E38C77CBE642243E43BE29168400000000000000C694000000005F5E100732103B6FCD7FAC4F665FE92415DD6E8450AD90F7D6B3D45A6CFCF2E359045FF4BB40074473045022100EB5A9001E14FC7304C4C2DF66507F9FC59D17FDCF98B43A4E30356658AB2A7CF02207127187EE0F287665D9552D15BEE6B00D3C6691C6773CE416E8A714B853F44FC8114735FF88E5269C80CD7F7AF10530DAB840BBF6FDF8314A8B6B9FF3246856CADC4A0106198C066EA1F9C39",
      "tx_json" : {
         "Account" : "rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za",
         "Destination" : "rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis",
         "DestinationTag" : 1,
         "Expiration" : 570113521,
         "Fee" : "12",
         "Flags" : 2147483648,
         "InvoiceID" : "46060241FABCF692D4D934BA2A6C4427CD4279083E38C77CBE642243E43BE291",
         "SendMax" : "100000000",
         "Sequence" : 1,
         "SigningPubKey" : "03B6FCD7FAC4F665FE92415DD6E8450AD90F7D6B3D45A6CFCF2E359045FF4BB400",
         "TransactionType" : "CheckCreate",
         "TxnSignature" : "3045022100EB5A9001E14FC7304C4C2DF66507F9FC59D17FDCF98B43A4E30356658AB2A7CF02207127187EE0F287665D9552D15BEE6B00D3C6691C6773CE416E8A714B853F44FC",
         "hash" : "07C3B2878B6941FED97BA647244531B7E2203268B05C71C3A1A014045ADDF408"
      }
   }
}
```
{% endtab %}
{% endtabs %}

## 3. 서명된 트랜잭션 제출하기(Submit the signed transaction)

이전 단계에서 서명한 트랜잭션 blob을 가져와 rippled 서버에 제출합니다. rippled 서버를 실행하지 않아도 안전하게 이 작업을 수행할 수 있습니다. 응답에는 임시 결과가 포함되며, 이 결과는 일반적으로 tesSUCCESS여야 하지만 최종 결과는 아닙니다. 대기 중인 트랜잭션은 일반적으로 다음 오픈 ledger 버전에 포함되므로(일반적으로 제출 후 약 10초 후), terQUEUED의 임시 응답도 괜찮습니다.

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

  const tx_blob = "12001022800000002400000002201B0077911368400000000000000"+
    "C694000000005F5E100732103B6FCD7FAC4F665FE92415DD6E8450AD90F7D6B3D45A6"+
    "CFCF2E359045FF4BB400744630440220181FE2F945EBEE632966D5FB03114611E3047"+
    "ACD155AA1BDB9DF8545C7A2431502201E873A4B0D177AB250AF790CE80621E16F1415"+
    "06CF507586038FC4A8E95887358114735FF88E5269C80CD7F7AF10530DAB840BBF6FD"+
    "F8314A8B6B9FF3246856CADC4A0106198C066EA1F9C39"

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

{% tab title="WebSocket" %}
```json
{
  "id": "submit_req_1",
  "command": "submit",
  "tx_blob": "120010228000000024000000042E00000001501146060241FABCF692D4D934BA2A6C4427CD4279083E38C77CBE642243E43BE29168400000000000000C694000000005F5E100732103B6FCD7FAC4F665FE92415DD6E8450AD90F7D6B3D45A6CFCF2E359045FF4BB40074463044022071A341F911A8EF3B68399487CAF5BA3B59C6FE476B626698AEF044B8183721BC0220166053A859BD907251DFCCF34DD71202180EBABAE7098BB5903D16EBFC993C408114735FF88E5269C80CD7F7AF10530DAB840BBF6FDF8314A8B6B9FF3246856CADC4A0106198C066EA1F9C39"
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
rippled submit 120010228000000024000000012A21FB3DF12E00000001501146060241FABCF692D4D934BA2A6C4427CD4279083E38C77CBE642243E43BE29168400000000000000C694000000005F5E100732103B6FCD7FAC4F665FE92415DD6E8450AD90F7D6B3D45A6CFCF2E359045FF4BB40074473045022100EB5A9001E14FC7304C4C2DF66507F9FC59D17FDCF98B43A4E30356658AB2A7CF02207127187EE0F287665D9552D15BEE6B00D3C6691C6773CE416E8A714B853F44FC8114735FF88E5269C80CD7F7AF10530DAB840BBF6FDF8314A8B6B9FF3246856CADC4A0106198C066EA1F9C39
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

{% tab title="WebSocket" %}
```javascript
{
  "id": "submit_req_1",
  "result": {
    "engine_result": "terQUEUED",
    "engine_result_code": -89,
    "engine_result_message": "Held until escalated fee drops.",
    "tx_blob": "120010228000000024000000042E00000001501146060241FABCF692D4D934BA2A6C4427CD4279083E38C77CBE642243E43BE29168400000000000000C694000000005F5E100732103B6FCD7FAC4F665FE92415DD6E8450AD90F7D6B3D45A6CFCF2E359045FF4BB40074463044022071A341F911A8EF3B68399487CAF5BA3B59C6FE476B626698AEF044B8183721BC0220166053A859BD907251DFCCF34DD71202180EBABAE7098BB5903D16EBFC993C408114735FF88E5269C80CD7F7AF10530DAB840BBF6FDF8314A8B6B9FF3246856CADC4A0106198C066EA1F9C39",
    "tx_json": {
      "Account": "rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za",
      "Destination": "rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis",
      "DestinationTag": 1,
      "Fee": "12",
      "Flags": 2147483648,
      "InvoiceID": "46060241FABCF692D4D934BA2A6C4427CD4279083E38C77CBE642243E43BE291",
      "SendMax": "100000000",
      "Sequence": 4,
      "SigningPubKey": "03B6FCD7FAC4F665FE92415DD6E8450AD90F7D6B3D45A6CFCF2E359045FF4BB400",
      "TransactionType": "CheckCreate",
      "TxnSignature": "3044022071A341F911A8EF3B68399487CAF5BA3B59C6FE476B626698AEF044B8183721BC0220166053A859BD907251DFCCF34DD71202180EBABAE7098BB5903D16EBFC993C40",
      "hash": "09D992D4C89E2A24D4BA9BB57ED81C7003815940F39B7C87ADBF2E49034380BB"
    }
  },
  "status": "success",
  "type": "response"
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
Loading: "/etc/opt/ripple/rippled.cfg"
2018-Mar-28 01:52:49 HTTPClient:NFO Connecting to 127.0.0.1:5005

{
  "result": {
    "engine_result": "terQUEUED",
    "engine_result_code": -89,
    "engine_result_message": "Held until escalated fee drops.",
    "status" : "success",
    "tx_blob" : "120010228000000024000000012A21FB3DF12E00000001501146060241FABCF692D4D934BA2A6C4427CD4279083E38C77CBE642243E43BE29168400000000000000C694000000005F5E100732103B6FCD7FAC4F665FE92415DD6E8450AD90F7D6B3D45A6CFCF2E359045FF4BB40074473045022100EB5A9001E14FC7304C4C2DF66507F9FC59D17FDCF98B43A4E30356658AB2A7CF02207127187EE0F287665D9552D15BEE6B00D3C6691C6773CE416E8A714B853F44FC8114735FF88E5269C80CD7F7AF10530DAB840BBF6FDF8314A8B6B9FF3246856CADC4A0106198C066EA1F9C39",
    "tx_json" : {
      "Account" : "rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za",
      "Destination" : "rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis",
      "DestinationTag" : 1,
      "Expiration" : 570113521,
      "Fee" : "12",
      "Flags" : 2147483648,
      "InvoiceID" : "46060241FABCF692D4D934BA2A6C4427CD4279083E38C77CBE642243E43BE291",
      "SendMax" : "100000000",
      "Sequence" : 1,
      "SigningPubKey" : "03B6FCD7FAC4F665FE92415DD6E8450AD90F7D6B3D45A6CFCF2E359045FF4BB400",
      "TransactionType" : "CheckCreate",
      "TxnSignature" : "3045022100EB5A9001E14FC7304C4C2DF66507F9FC59D17FDCF98B43A4E30356658AB2A7CF02207127187EE0F287665D9552D15BEE6B00D3C6691C6773CE416E8A714B853F44FC",
      "hash" : "07C3B2878B6941FED97BA647244531B7E2203268B05C71C3A1A014045ADDF408"
    }
  }
}
```
{% endtab %}
{% endtabs %}

## 4. 유효성 검사 대기(Wait for validation)

라이브 네트워크(mainnet, testnet 또는 devnet 포함)에서는 ledger가 자동으로 닫힐 때까지 4\~7초 정도 기다릴 수 있습니다.

stand-alone 모드에서 rippled을 실행하는 경우, ledger\_accept 메소드를 사용하여 ledger를 수동으로 닫아야 합니다.

## 5. 최종 결과 확인(Confirm final result)

트랜잭션의 상태를 확인하려면 CheckCreate 트랜잭션의 식별 해시와 함께 tx 메소드를 사용하세요. 트랜잭션의 메타데이터에서 "TransactionResult": "tesSUCCESS" 필드와 이 결과가 최종 결과임을 나타내는 결과에서 "validated": true 필드를 찾습니다.

트랜잭션 메타데이터에서 LedgerEntryType이 "Check"인 CreatedNode 객체를 찾습니다. 이는 트랜잭션이 수표 ledger 객체를 생성했음을 나타냅니다. 이 객체의 LedgerIndex는 수표의 ID입니다. 다음 예제에서 수표의 ID는 84C61BE9B39B2C4A2267F67504404F1EC76678806C1B901EA781D1E3B4CE0CD9입니다.

## 요청 예시(Example Request)

{% tabs %}
{% tab title="rippled-lib 1.x" %}
```javascript
'use strict'
const RippleAPI = require('ripple-lib').RippleAPI
const decodeAddress = require('ripple-address-codec').decodeAddress;
const createHash = require('crypto').createHash;

// This example connects to a public Test Net server
const api = new RippleAPI({server: 'wss://s.altnet.rippletest.net:51233'})
api.connect().then(() => {
  console.log('Connected')

  const tx_hash = "C0B27D20669BAB837B3CDF4B8148B988F17CE1EF8EDF48C806AE9BF69E16F441"

  return api.getTransaction(tx_hash)
}).then(response => {
  console.log("Final transaction result:", response)

  // Re-calculate checkID to work around issue ripple-lib#876
  const checkIDhasher = createHash('sha512')
  checkIDhasher.update(Buffer.from('0043', 'hex'))
  checkIDhasher.update(new Buffer(decodeAddress(response.address)))
  const seqBuf = Buffer.alloc(4)
  seqBuf.writeUInt32BE(response.sequence, 0)
  checkIDhasher.update(seqBuf)
  const checkID = checkIDhasher.digest('hex').slice(0,64).toUpperCase()
  console.log("Calculated checkID:", checkID)

// Disconnect and return
}).then(() => {
  api.disconnect().then(() => {
    console.log('Disconnected')
    process.exit()
  })
}).catch(console.error)
```
{% endtab %}

{% tab title="WebSocket" %}
```json
{
  "id": "tx_req_1",
  "command": "tx",
  "transaction": "09D992D4C89E2A24D4BA9BB57ED81C7003815940F39B7C87ADBF2E49034380BB"
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
rippled tx 07C3B2878B6941FED97BA647244531B7E2203268B05C71C3A1A014045ADDF408
```
{% endtab %}
{% endtabs %}

## 응답 예시(Example Response)

{% tabs %}
{% tab title="rippled-lib 1.x" %}
```javascript
Connected
Final transaction result: { type: 'checkCreate',
  address: 'rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za',
  sequence: 2,
  id: 'C0B27D20669BAB837B3CDF4B8148B988F17CE1EF8EDF48C806AE9BF69E16F441',
  specification:
   { destination: 'rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis',
     sendMax: { currency: 'XRP', value: '100' } },
  outcome:
   { result: 'tesSUCCESS',
     timestamp: '2018-03-27T20:47:40.000Z',
     fee: '0.000012',
     balanceChanges: { rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za: [Array] },
     orderbookChanges: {},
     ledgerVersion: 7835887,
     indexInLedger: 0 } }
Calculated checkID: CEA5F0BD7B2B5C85A70AE735E4CE722C43C86410A79AB87C11938AA13A11DBF9
Disconnected
```
{% endtab %}

{% tab title="WebSocket" %}
```json
{
  "id": "tx_req_1",
  "result": {
    "Account": "rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za",
    "Destination": "rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis",
    "DestinationTag": 1,
    "Fee": "12",
    "Flags": 2147483648,
    "InvoiceID": "46060241FABCF692D4D934BA2A6C4427CD4279083E38C77CBE642243E43BE291",
    "SendMax": "100000000",
    "Sequence": 4,
    "SigningPubKey": "03B6FCD7FAC4F665FE92415DD6E8450AD90F7D6B3D45A6CFCF2E359045FF4BB400",
    "TransactionType": "CheckCreate",
    "TxnSignature": "3044022071A341F911A8EF3B68399487CAF5BA3B59C6FE476B626698AEF044B8183721BC0220166053A859BD907251DFCCF34DD71202180EBABAE7098BB5903D16EBFC993C40",
    "date": 575516100,
    "hash": "09D992D4C89E2A24D4BA9BB57ED81C7003815940F39B7C87ADBF2E49034380BB",
    "inLedger": 7841263,
    "ledger_index": 7841263,
    "meta": {
      "AffectedNodes": [
        {
          "ModifiedNode": {
            "FinalFields": {
              "Flags": 0,
              "Owner": "rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za",
              "RootIndex": "3F248A0715ECCAFC3BEE0C63C8F429ACE54ABC403AAF5F2885C2B65D62D1FAC1"
            },
            "LedgerEntryType": "DirectoryNode",
            "LedgerIndex": "3F248A0715ECCAFC3BEE0C63C8F429ACE54ABC403AAF5F2885C2B65D62D1FAC1"
          }
        },
        {
          "CreatedNode": {
            "LedgerEntryType": "Check",
            "LedgerIndex": "84C61BE9B39B2C4A2267F67504404F1EC76678806C1B901EA781D1E3B4CE0CD9",
            "NewFields": {
              "Account": "rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za",
              "Destination": "rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis",
              "DestinationTag": 1,
              "InvoiceID": "46060241FABCF692D4D934BA2A6C4427CD4279083E38C77CBE642243E43BE291",
              "SendMax": "100000000",
              "Sequence": 4
            }
          }
        },
        {
          "ModifiedNode": {
            "FinalFields": {
              "Account": "rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za",
              "Balance": "9999999952",
              "Flags": 0,
              "OwnerCount": 2,
              "Sequence": 5
            },
            "LedgerEntryType": "AccountRoot",
            "LedgerIndex": "A9A591BA661F69433D5BEAA49F10BA2B8DEA5183EF414B9130BFE5E0328FE875",
            "PreviousFields": {
              "Balance": "9999999964",
              "OwnerCount": 1,
              "Sequence": 4
            },
            "PreviousTxnID": "45AF36CF7A810D0054C38C82C898EFC7E4898DF94FA7A3AAF80CB868708F7CE0",
            "PreviousTxnLgrSeq": 7841237
          }
        },
        {
          "ModifiedNode": {
            "FinalFields": {
              "Flags": 0,
              "Owner": "rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis",
              "RootIndex": "C6A30AD85346718C7148D161663F84A96A4F0CE7F4D68C3C74D176A6C50BA6B9"
            },
            "LedgerEntryType": "DirectoryNode",
            "LedgerIndex": "C6A30AD85346718C7148D161663F84A96A4F0CE7F4D68C3C74D176A6C50BA6B9"
          }
        }
      ],
      "TransactionIndex": 0,
      "TransactionResult": "tesSUCCESS"
    },
    "validated": true
  },
  "status": "success",
  "type": "response"
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
Loading: "/etc/opt/ripple/rippled.cfg"
2018-Mar-28 02:17:55 HTTPClient:NFO Connecting to 127.0.0.1:5005

{
   "result" : {
      "Account" : "rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za",
      "Destination" : "rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis",
      "DestinationTag" : 1,
      "Expiration" : 570113521,
      "Fee" : "12",
      "Flags" : 2147483648,
      "InvoiceID" : "46060241FABCF692D4D934BA2A6C4427CD4279083E38C77CBE642243E43BE291",
      "SendMax" : "100000000",
      "Sequence" : 1,
      "SigningPubKey" : "03B6FCD7FAC4F665FE92415DD6E8450AD90F7D6B3D45A6CFCF2E359045FF4BB400",
      "TransactionType" : "CheckCreate",
      "TxnSignature" : "3045022100EB5A9001E14FC7304C4C2DF66507F9FC59D17FDCF98B43A4E30356658AB2A7CF02207127187EE0F287665D9552D15BEE6B00D3C6691C6773CE416E8A714B853F44FC",
      "hash" : "07C3B2878B6941FED97BA647244531B7E2203268B05C71C3A1A014045ADDF408"

      "date" : 575516100,
      "inLedger" : 7841263,
      "ledger_index" : 7841263,
      "meta" : {
         "AffectedNodes" : [
            {
               "ModifiedNode" : {
                  "FinalFields" : {
                     "Flags" : 0,
                     "Owner" : "rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za",
                     "RootIndex" : "3F248A0715ECCAFC3BEE0C63C8F429ACE54ABC403AAF5F2885C2B65D62D1FAC1"
                  },
                  "LedgerEntryType" : "DirectoryNode",
                  "LedgerIndex" : "3F248A0715ECCAFC3BEE0C63C8F429ACE54ABC403AAF5F2885C2B65D62D1FAC1"
               }
            },
            {
               "CreatedNode" : {
                  "LedgerEntryType" : "Check",
                  "LedgerIndex" : "84C61BE9B39B2C4A2267F67504404F1EC76678806C1B901EA781D1E3B4CE0CD9",
                  "NewFields" : {
                     "Account" : "rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za",
                     "Destination" : "rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis",
                     "DestinationTag" : 1,
                     "InvoiceID" : "46060241FABCF692D4D934BA2A6C4427CD4279083E38C77CBE642243E43BE291",
                     "SendMax" : "100000000",
                     "Sequence" : 1
                  }
               }
            },
            {
               "ModifiedNode" : {
                  "FinalFields" : {
                     "Account" : "rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za",
                     "Balance" : "9999999952",
                     "Flags" : 0,
                     "OwnerCount" : 2,
                     "Sequence" : 2
                  },
                  "LedgerEntryType" : "AccountRoot",
                  "LedgerIndex" : "A9A591BA661F69433D5BEAA49F10BA2B8DEA5183EF414B9130BFE5E0328FE875",
                  "PreviousFields" : {
                     "Balance" : "9999999964",
                     "OwnerCount" : 1,
                     "Sequence" : 1
                  },
                  "PreviousTxnID" : "45AF36CF7A810D0054C38C82C898EFC7E4898DF94FA7A3AAF80CB868708F7CE0",
                  "PreviousTxnLgrSeq" : 7841237
               }
            },
            {
               "ModifiedNode" : {
                  "FinalFields" : {
                     "Flags" : 0,
                     "Owner" : "rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis",
                     "RootIndex" : "C6A30AD85346718C7148D161663F84A96A4F0CE7F4D68C3C74D176A6C50BA6B9"
                  },
                  "LedgerEntryType" : "DirectoryNode",
                  "LedgerIndex" : "C6A30AD85346718C7148D161663F84A96A4F0CE7F4D68C3C74D176A6C50BA6B9"
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
