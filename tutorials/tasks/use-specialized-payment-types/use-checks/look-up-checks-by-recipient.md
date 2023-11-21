---
description: 수표 개정에 의해 추가되었습니다.
---

# 수취인별 수표 조회(Look Up Checks by Recipient)

이 튜토리얼에서는 수취인별로 수표를 조회하는 방법을 보여드립니다. 발신자별로 수표를 조회할 수도 있습니다.

## 1. 주소에 대한 모든 수표 조회(Look up all Checks for the address)

계정에 대해 수신 및 발신되는 모든 수표 목록을 가져오려면 받는 사람 계정의 주소와 함께 account\_objects 명령을 사용하고 요청의 type 필드를 checks로 설정합니다.

{% hint style="info" %}
Note:

account\_objects 명령에 대한 커맨드라인 인터페이스는 type 필드를 허용하지 않습니다. 대신 [json method](https://xrpl.org/json.html)를 사용하여 명령줄에서 JSON-RPC 형식 요청을 보낼 수 있습니다.
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

  const account_objects_request = {
    command: "account_objects",
    account: "rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za",
    ledger_index: "validated",
    type: "check"
  }

  return api.connection.request(account_objects_request)
}).then(response => {
  console.log("account_objects response:", response)

// Disconnect and return
}).then(() => {
  api.disconnect().then(() => {
    console.log('Disconnected')
    process.exit()
  })
}).catch(console.error)
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "account_objects",
    "params": [
        {
            "account": "rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za",
            "ledger_index": "validated",
            "type": "check"
        }
    ]
}
```
{% endtab %}
{% endtabs %}

## &#x20;응답 예시(Example Response)

{% tabs %}
{% tab title="ripple-lib 1.x" %}
```javascript
Connected
account_objects response: { account: 'rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za',
  account_objects:
   [ { Account: 'rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za',
       Destination: 'rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis',
       DestinationNode: '0000000000000000',
       Flags: 0,
       LedgerEntryType: 'Check',
       OwnerNode: '0000000000000000',
       PreviousTxnID: '37D90463CDE0497DB12F18099296DA0E1E52334A785710B5F56BC9637F62429C',
       PreviousTxnLgrSeq: 8003261,
       SendMax: '999999000000',
       Sequence: 5,
       index: '2E0AD0740B79BE0AAE5EDD1D5FC79E3C5C221D23C6A7F771D85569B5B91195C2' },
     { Account: 'rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis',
       Destination: 'rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za',
       DestinationNode: '0000000000000000',
       Flags: 0,
       LedgerEntryType: 'Check',
       OwnerNode: '0000000000000000',
       PreviousTxnID: 'EF462F1D004E97850AECFB8EC4836DA57706FAFADF8E0914010853C1EC7F2055',
       PreviousTxnLgrSeq: 8003480,
       SendMax: [Object],
       Sequence: 2,
       index: '323CE1D169135513085268EF81ED40775725C97E7922DBABCCE48FE3FD138861' },
     { Account: 'rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za',
       Destination: 'rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis',
       DestinationNode: '0000000000000000',
       DestinationTag: 1,
       Flags: 0,
       InvoiceID: '46060241FABCF692D4D934BA2A6C4427CD4279083E38C77CBE642243E43BE291',
       LedgerEntryType: 'Check',
       OwnerNode: '0000000000000000',
       PreviousTxnID: '09D992D4C89E2A24D4BA9BB57ED81C7003815940F39B7C87ADBF2E49034380BB',
       PreviousTxnLgrSeq: 7841263,
       SendMax: '100000000',
       Sequence: 4,
       index: '84C61BE9B39B2C4A2267F67504404F1EC76678806C1B901EA781D1E3B4CE0CD9' },
     { Account: 'rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za',
       Destination: 'rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis',
       DestinationNode: '0000000000000000',
       Flags: 0,
       LedgerEntryType: 'Check',
       OwnerNode: '0000000000000000',
       PreviousTxnID: 'C0B27D20669BAB837B3CDF4B8148B988F17CE1EF8EDF48C806AE9BF69E16F441',
       PreviousTxnLgrSeq: 7835887,
       SendMax: '100000000',
       Sequence: 2,
       index: 'CEA5F0BD7B2B5C85A70AE735E4CE722C43C86410A79AB87C11938AA13A11DBF9' } ],
  ledger_hash: 'DD577D96A1064E16A5DB64C3C25BFF5EF0D8E36A18E4540B162731FA6320C46D',
  ledger_index: 8004101,
  validated: true }
Disconnecteda
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
200 OK

{
  "result": {
    "account": "rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za",
    "account_objects": [
      {
        "Account": "rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za",
        "Destination": "rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis",
        "DestinationNode": "0000000000000000",
        "Flags": 0,
        "LedgerEntryType": "Check",
        "OwnerNode": "0000000000000000",
        "PreviousTxnID": "37D90463CDE0497DB12F18099296DA0E1E52334A785710B5F56BC9637F62429C",
        "PreviousTxnLgrSeq": 8003261,
        "SendMax": "999999000000",
        "Sequence": 5,
        "index": "2E0AD0740B79BE0AAE5EDD1D5FC79E3C5C221D23C6A7F771D85569B5B91195C2"
      },
      {
        "Account": "rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis",
        "Destination": "rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za",
        "DestinationNode": "0000000000000000",
        "Flags": 0,
        "LedgerEntryType": "Check",
        "OwnerNode": "0000000000000000",
        "PreviousTxnID": "EF462F1D004E97850AECFB8EC4836DA57706FAFADF8E0914010853C1EC7F2055",
        "PreviousTxnLgrSeq": 8003480,
        "SendMax": {
          "currency": "BAR",
          "issuer": "rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za",
          "value": "1000000000000000e-66"
        },
        "Sequence": 2,
        "index": "323CE1D169135513085268EF81ED40775725C97E7922DBABCCE48FE3FD138861"
      },
      {
        "Account": "rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za",
        "Destination": "rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis",
        "DestinationNode": "0000000000000000",
        "DestinationTag": 1,
        "Flags": 0,
        "InvoiceID": "46060241FABCF692D4D934BA2A6C4427CD4279083E38C77CBE642243E43BE291",
        "LedgerEntryType": "Check",
        "OwnerNode": "0000000000000000",
        "PreviousTxnID": "09D992D4C89E2A24D4BA9BB57ED81C7003815940F39B7C87ADBF2E49034380BB",
        "PreviousTxnLgrSeq": 7841263,
        "SendMax": "100000000",
        "Sequence": 4,
        "index": "84C61BE9B39B2C4A2267F67504404F1EC76678806C1B901EA781D1E3B4CE0CD9"
      },
      {
        "Account": "rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za",
        "Destination": "rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis",
        "DestinationNode": "0000000000000000",
        "Flags": 0,
        "LedgerEntryType": "Check",
        "OwnerNode": "0000000000000000",
        "PreviousTxnID": "C0B27D20669BAB837B3CDF4B8148B988F17CE1EF8EDF48C806AE9BF69E16F441",
        "PreviousTxnLgrSeq": 7835887,
        "SendMax": "100000000",
        "Sequence": 2,
        "index": "CEA5F0BD7B2B5C85A70AE735E4CE722C43C86410A79AB87C11938AA13A11DBF9"
      }
    ],
    "ledger_hash": "4002E4E84CABAAF1BDD5636097F3042547EBAE2DEE647E1036E64AA9FDA2A10C",
    "ledger_index": 8004173,
    "status": "success",
    "validated": true
  }
}
```
{% endtab %}
{% endtabs %}

## 2. 수신자별로 응답 필터링(Filter the responses by recipient)

응답에는 요청의 계정이 발신자인 경우의 Check와 계정이 수신자인 경우의 Check가 포함될 수 있습니다. 응답의 account\_objects 배열의 각 멤버는 하나의 수표를 나타냅니다. 이러한 각 수표 객체에 대해, Destination 의 주소는 해당 수표의 수신자 주소입니다.

다음 pseudocode는 수신자별로 응답을 필터링하는 방법을 보여줍니다:

```javascript
recipient_address = "rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za"
account_objects_response = get_account_objects({
    account: recipient_address,
    ledger_index: "validated",
    type: "check"
})

for (i=0; i < account_objects_response.account_objects.length; i++) {
  check_object = account_objects_response.account_objects[i]
  if (check_object.Destination == recipient_address) {
    log("Check to recipient:", check_object)
  }
}
```
