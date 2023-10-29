# 마스터 키 쌍 비활성화

이 페이지에서는 계정 주소 와 수학적으로 연결된 마스터 키 쌍을 비활성화하는 방법을 설명합니다. 계정의 마스터 키 쌍이 손상되었을 수 있거나 다중 서명을 계정에서 거래를 제출하는 유일한 방법으로 만들려는 경우 이 작업을 수행해야 합니다.

{% hint style="info" %}
Warning:

마스터 키 쌍을 비활성화하면 트랜잭션을 인증하는 한 가지 방법이 제거됩니다. 마스터 키 쌍을 비활성화하기 전에 일반 키 또는 다중 서명과 같은 다른 트랜잭션 승인 방법 중 하나를 사용할 수 있는지 확인해야 합니다. (예를 들어 일반 키 쌍을 할당한 경우 해당 일반 키로 트랜잭션을 성공적으로 제출할 수 있는지 확인하십시오.) XRP Ledger의 분산 특성으로 인해 나머지를 사용할 수 없는 경우 아무도 계정에 대한 액세스를 복원할 수 없습니다.
{% endhint %}

**마스터 키 쌍을 비활성화하려면 마스터 키 쌍을 사용해야 합니다.** 그러나 트랜잭션을 승인하는 다른 방법을 사용하여 마스터 키 쌍을 다시 활성화 할 수 있습니다.

## 요구 사항 <a href="#prerequisites" id="prerequisites"></a>

계정의 마스터 키 쌍을 비활성화 하려면 다음 요구 조건을 충족해야 합니다.

* XRP Ledger 계정이 있어야 하며 마스터 키 쌍을 사용하여 해당 계정에서 트랜잭션에 서명하고 제출할 수 있어야 합니다. 참조: 보안 서명 설정. 이것이 작동하는 두 가지 일반적인 방법은 다음과 같습니다.
  * 계정의 마스터 시드 값을 알고 있습니다. 시드 값은 일반적으로 "s"로 시작하는 base58 값으로 표시됩니다(예: `sn3nxiW7v8KXzPzAqzyHXbSSKNuN9`.
  * 또는 시드 값을 안전하게 저장하는 전용 서명 장치를 사용하므로 알 필요가 없습니다.
* 귀하의 계정에는 마스터 키 쌍 이외의 거래를 승인하는 방법이 하나 이상 있어야 합니다. 즉, 다음 중 하나 또는 모두를 수행해야 합니다.
  * 일반 키 쌍을 할당.
  * 다중 서명 설정.

## 단계 <a href="#steps" id="steps"></a>

## 1. 트랜잭션 JSON 구성 <a href="#1-construct-transaction-json" id="1-construct-transaction-json"></a>

필드를 사용하여 계정에서 AccountSet 트랜잭션을 준비합니다 `"SetValue": 4`. 이것은 AccountSet 플래그 "Disable Master"( )의 값입니다 `asfDisableMaster`. 이 거래에 대한 유일한 다른 필수 필드는 필수 공통 필드 입니다. 예를 들어 자동 입력 가능한 필드를 생략하면 다음 거래 지침으로 충분합니다.

```json
{
  "TransactionType": "AccountSet",
  "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
  "SetFlag": 4
}
```

{% hint style="info" %}
Tip:

예측 가능한 시간 내에 트랜잭션 결과를 안정적으로 얻을 수 있도록 LastLedgerSequence 필드도 제공하는 것이 좋습니다.
{% endhint %}

## 2. 트랜잭션 서명 <a href="#2-sign-transaction" id="2-sign-transaction"></a>

**마스터 키 쌍을** 사용하여 트랜잭션에 서명 해야 합니다.

{% hint style="info" %}
Warning:

제어하지 않는 서버에 비밀을 제출하지 말고 암호화되지 않은 상태로 네트워크를 통해 전송하지 마십시오. 이 예에서는 로컬 `rippled`서버를 사용한다고 가정합니다. 다른 보안 서명 구성을 사용하는 경우 이 지침을 수정해야 합니다.
{% endhint %}

## **예시 요청**

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "command": "sign",
  "tx_json": {
    "TransactionType": "AccountSet",
    "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "SetFlag": 4
  },
  "secret": "s████████████████████████████"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
   "method": "sign",
   "params": [
      {
         "tx_json": {
           "TransactionType": "AccountSet",
           "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
           "SetFlag": 4
         },
         "secret": "s████████████████████████████"
      }
   ]
}
```
{% endtab %}

{% tab title="Commandline" %}
```
$ rippled sign s████████████████████████████ '{"TransactionType":"AccountSet",
    "Account":"rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn", "SetFlag":4}'
```
{% endtab %}
{% endtabs %}

## **예시 응답**

{% tabs %}
{% tab title="WebSocket" %}
```json
Loading: "/etc/opt/ripple/rippled.cfg"
2020-Feb-13 00:13:24.783570867 HTTPClient:NFO Connecting to 127.0.0.1:5005

{
   "result" : {
      "deprecated" : "This command has been deprecated and will be removed in a future version of the server. Please migrate to a standalone signing tool.",
      "status" : "success",
      "tx_blob" : "1200032280000000240000017C20210000000468400000000000000A732103AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB7446304402204457A890BC06F48061F8D61042975702B57EBEF3EA2C7C484DFE38CFD42EA11102202505A7C62FF41E68FDE10271BADD75BD66D54B2F96A326BE487A2728A352442D81144B4E9C06F24296074F7BC48F92A97916C6DC5EA9",
      "tx_json" : {
         "Account" : "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
         "Fee" : "10",
         "Flags" : 2147483648,
         "Sequence" : 380,
         "SetFlag" : 4,
         "SigningPubKey" : "03AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB",
         "TransactionType" : "AccountSet",
         "TxnSignature" : "304402204457A890BC06F48061F8D61042975702B57EBEF3EA2C7C484DFE38CFD42EA11102202505A7C62FF41E68FDE10271BADD75BD66D54B2F96A326BE487A2728A352442D",
         "hash" : "327FD263132A4D08170E1B01FE1BB2E21D0126CE58165C97A9173CA9551BCD70"
      }
   }
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "result": {
        "deprecated": "This command has been deprecated and will be removed in a future version of the server. Please migrate to a standalone signing tool.",
        "status": "success",
        "tx_blob": "1200032280000000240000017C20210000000468400000000000000A732103AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB7446304402204457A890BC06F48061F8D61042975702B57EBEF3EA2C7C484DFE38CFD42EA11102202505A7C62FF41E68FDE10271BADD75BD66D54B2F96A326BE487A2728A352442D81144B4E9C06F24296074F7BC48F92A97916C6DC5EA9",
        "tx_json": {
            "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
            "Fee": "10",
            "Flags": 2147483648,
            "Sequence": 380,
            "SetFlag": 4,
            "SigningPubKey": "03AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB",
            "TransactionType": "AccountSet",
            "TxnSignature": "304402204457A890BC06F48061F8D61042975702B57EBEF3EA2C7C484DFE38CFD42EA11102202505A7C62FF41E68FDE10271BADD75BD66D54B2F96A326BE487A2728A352442D",
            "hash": "327FD263132A4D08170E1B01FE1BB2E21D0126CE58165C97A9173CA9551BCD70"
        }
    }
}
```
{% endtab %}

{% tab title="Commandline" %}
```
Loading: "/etc/opt/ripple/rippled.cfg"
2020-Feb-13 00:13:24.783570867 HTTPClient:NFO Connecting to 127.0.0.1:5005

{
   "result" : {
      "deprecated" : "This command has been deprecated and will be removed in a future version of the server. Please migrate to a standalone signing tool.",
      "status" : "success",
      "tx_blob" : "1200032280000000240000017C20210000000468400000000000000A732103AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB7446304402204457A890BC06F48061F8D61042975702B57EBEF3EA2C7C484DFE38CFD42EA11102202505A7C62FF41E68FDE10271BADD75BD66D54B2F96A326BE487A2728A352442D81144B4E9C06F24296074F7BC48F92A97916C6DC5EA9",
      "tx_json" : {
         "Account" : "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
         "Fee" : "10",
         "Flags" : 2147483648,
         "Sequence" : 380,
         "SetFlag" : 4,
         "SigningPubKey" : "03AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB",
         "TransactionType" : "AccountSet",
         "TxnSignature" : "304402204457A890BC06F48061F8D61042975702B57EBEF3EA2C7C484DFE38CFD42EA11102202505A7C62FF41E68FDE10271BADD75BD66D54B2F96A326BE487A2728A352442D",
         "hash" : "327FD263132A4D08170E1B01FE1BB2E21D0126CE58165C97A9173CA9551BCD70"
      }
   }
}
```
{% endtab %}
{% endtabs %}

`"status": "success"`서버가 트랜잭션에 성공적으로 서명했음을 나타내는 을 찾습니다. `"status": "error"`대신 받는 경우 `error`및 `error_message`필드에서 자세한 내용을 확인하세요. 몇 가지 일반적인 가능성은 다음과 같습니다.

* `"error": "badSecret"secret`일반적으로 요청에 오타가 있음을 의미합니다.
* `"error": "masterDisabled"`이 주소의 마스터 키 쌍이 이미 비활성화되었음을 의미합니다.

`tx_blob`응답의 값을 기록해 둡니다. 이것은 네트워크에 제출할 수 있는 서명된 트랜잭션 바이너리입니다.

## 3. 트랜잭션 제출 <a href="#3-submit-transaction" id="3-submit-transaction"></a>

이전 단계에서 서명된 거래 blob을 XRP Ledger에 제출합니다.

## **예시 요청**

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "command": "submit",
    "tx_blob": "1200032280000000240000017C20210000000468400000000000000A732103AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB7446304402204457A890BC06F48061F8D61042975702B57EBEF3EA2C7C484DFE38CFD42EA11102202505A7C62FF41E68FDE10271BADD75BD66D54B2F96A326BE487A2728A352442D81144B4E9C06F24296074F7BC48F92A97916C6DC5EA9"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
   "method":"submit",
   "params": [
      {
         "tx_blob": "1200032280000000240000017C20210000000468400000000000000A732103AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB7446304402204457A890BC06F48061F8D61042975702B57EBEF3EA2C7C484DFE38CFD42EA11102202505A7C62FF41E68FDE10271BADD75BD66D54B2F96A326BE487A2728A352442D81144B4E9C06F24296074F7BC48F92A97916C6DC5EA9"
      }
   ]
}
```
{% endtab %}

{% tab title="Commandline" %}
```
$ rippled submit 1200032280000000240000017C20210000000468400000000000000A732103AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB7446304402204457A890BC06F48061F8D61042975702B57EBEF3EA2C7C484DFE38CFD42EA11102202505A7C62FF41E68FDE10271BADD75BD66D54B2F96A326BE487A2728A352442D81144B4E9C06F24296074F7BC48F92A97916C6DC5EA9
```
{% endtab %}
{% endtabs %}

## **예시 응답**

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "result": {
    "engine_result" : "tesSUCCESS",
    "engine_result_code" : 0,
    "engine_result_message" : "The transaction was applied. Only final in a validated ledger.",
    "tx_blob" : "1200032280000000240000017C20210000000468400000000000000A732103AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB7446304402204457A890BC06F48061F8D61042975702B57EBEF3EA2C7C484DFE38CFD42EA11102202505A7C62FF41E68FDE10271BADD75BD66D54B2F96A326BE487A2728A352442D81144B4E9C06F24296074F7BC48F92A97916C6DC5EA9",
    "tx_json" : {
      "Account" : "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
      "Fee" : "10",
      "Flags" : 2147483648,
      "Sequence" : 380,
      "SetFlag" : 4,
      "SigningPubKey" : "03AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB",
      "TransactionType" : "AccountSet",
      "TxnSignature" : "304402204457A890BC06F48061F8D61042975702B57EBEF3EA2C7C484DFE38CFD42EA11102202505A7C62FF41E68FDE10271BADD75BD66D54B2F96A326BE487A2728A352442D",
      "hash" : "327FD263132A4D08170E1B01FE1BB2E21D0126CE58165C97A9173CA9551BCD70"
    }
  },
  "status": "success",
  "type": "response"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
  "result" : {
    "engine_result" : "tesSUCCESS",
    "engine_result_code" : 0,
    "engine_result_message" : "The transaction was applied. Only final in a validated ledger.",
    "status" : "success",
    "tx_blob" : "1200032280000000240000017C20210000000468400000000000000A732103AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB7446304402204457A890BC06F48061F8D61042975702B57EBEF3EA2C7C484DFE38CFD42EA11102202505A7C62FF41E68FDE10271BADD75BD66D54B2F96A326BE487A2728A352442D81144B4E9C06F24296074F7BC48F92A97916C6DC5EA9",
    "tx_json" : {
      "Account" : "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
      "Fee" : "10",
      "Flags" : 2147483648,
      "Sequence" : 380,
      "SetFlag" : 4,
      "SigningPubKey" : "03AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB",
      "TransactionType" : "AccountSet",
      "TxnSignature" : "304402204457A890BC06F48061F8D61042975702B57EBEF3EA2C7C484DFE38CFD42EA11102202505A7C62FF41E68FDE10271BADD75BD66D54B2F96A326BE487A2728A352442D",
      "hash" : "327FD263132A4D08170E1B01FE1BB2E21D0126CE58165C97A9173CA9551BCD70"
    }
  }
}
```
{% endtab %}

{% tab title="Commandline" %}
```
Loading: "/etc/opt/ripple/rippled.cfg"
2020-Feb-13 00:25:49.361743460 HTTPClient:NFO Connecting to 127.0.0.1:5005

{
   "result" : {
      "engine_result" : "tesSUCCESS",
      "engine_result_code" : 0,
      "engine_result_message" : "The transaction was applied. Only final in a validated ledger.",
      "status" : "success",
      "tx_blob" : "1200032280000000240000017C20210000000468400000000000000A732103AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB7446304402204457A890BC06F48061F8D61042975702B57EBEF3EA2C7C484DFE38CFD42EA11102202505A7C62FF41E68FDE10271BADD75BD66D54B2F96A326BE487A2728A352442D81144B4E9C06F24296074F7BC48F92A97916C6DC5EA9",
      "tx_json" : {
         "Account" : "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
         "Fee" : "10",
         "Flags" : 2147483648,
         "Sequence" : 380,
         "SetFlag" : 4,
         "SigningPubKey" : "03AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB",
         "TransactionType" : "AccountSet",
         "TxnSignature" : "304402204457A890BC06F48061F8D61042975702B57EBEF3EA2C7C484DFE38CFD42EA11102202505A7C62FF41E68FDE10271BADD75BD66D54B2F96A326BE487A2728A352442D",
         "hash" : "327FD263132A4D08170E1B01FE1BB2E21D0126CE58165C97A9173CA9551BCD70"
      }
   }
}
```
{% endtab %}
{% endtabs %}

결과와 함께 거래가 실패하면 `tecNO_ALTERNATIVE_KEY`귀하의 계정에 현재 활성화된 거래를 승인하는 다른 방법이 없는 것입니다. 일반 키 쌍을 할당하거나 다중 서명을 설정한 다음 다시 마스터 키 쌍 비활성화를 시도해야 합니다.

## 4. 확인 대기 <a href="#4-wait-for-validation" id="4-wait-for-validation"></a>

라이브 네트워크(mainnet, testnet 또는 devnet 포함)에서 ledger 자동으로 닫힐 때까지 4-7초를 기다릴 수 있습니다.

`rippled` stand-alone 모드에서 실행 중인 경우 ledger\_accept 메소드를 사용하여 ledger를 수동으로 닫습니다.

## 5. 계정 플래그 확인 <a href="#5-confirm-account-flags" id="5-confirm-account-flags"></a>

account\_info 메소소드를 사용하여 계정의 마스터 키가 비활성화되었는지 확인합니다. 다음 매개변수를 지정해야 합니다.

| 필드             | 값                                      |
| -------------- | -------------------------------------- |
| `account`      | 귀하의 계정 주소.                             |
| `ledger_index` | `"validated"`검증된 최신 원장 버전에서 결과를 가져옵니다. |

## **예시 요청**

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "command": "account_info",
  "account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
  "ledger_index": "validated"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "account_info",
    "params": [{
        "account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
        "ledger_index": "validated"
    }]
}
```
{% endtab %}

{% tab title="Commandline" %}
```
rippled account_info rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn validated
```
{% endtab %}
{% endtabs %}

## **예시 응답**

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "result": {
    "account_data": {
      "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
      "AccountTxnID": "327FD263132A4D08170E1B01FE1BB2E21D0126CE58165C97A9173CA9551BCD70",
      "Balance": "423013688",
      "Domain": "6D64756F31332E636F6D",
      "EmailHash": "98B4375E1D753E5B91627516F6D70977",
      "Flags": 9633792,
      "LedgerEntryType": "AccountRoot",
      "MessageKey": "0000000000000000000000070000000300",
      "OwnerCount": 9,
      "PreviousTxnID": "327FD263132A4D08170E1B01FE1BB2E21D0126CE58165C97A9173CA9551BCD70",
      "PreviousTxnLgrSeq": 53391321,
      "RegularKey": "rD9iJmieYHn8jTtPjwwkW2Wm9sVDvPXLoJ",
      "Sequence": 381,
      "TransferRate": 4294967295,
      "index": "13F1A95D7AAB7108D5CE7EEAF504B2894B8C674E6D68499076441C4837282BF8",
      "urlgravatar": "http://www.gravatar.com/avatar/98b4375e1d753e5b91627516f6d70977"
    },
    "ledger_hash": "A90CEBD4AEDA24470AAC5CD307B6D26267ACE79C03669A0A0B8C41ACAEDAA6F0",
    "ledger_index": 53391576,
    "validated": true
  },
  "status": "success",
  "type": "response"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
  "result": {
    "account_data": {
      "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
      "AccountTxnID": "327FD263132A4D08170E1B01FE1BB2E21D0126CE58165C97A9173CA9551BCD70",
      "Balance": "423013688",
      "Domain": "6D64756F31332E636F6D",
      "EmailHash": "98B4375E1D753E5B91627516F6D70977",
      "Flags": 9633792,
      "LedgerEntryType": "AccountRoot",
      "MessageKey": "0000000000000000000000070000000300",
      "OwnerCount": 9,
      "PreviousTxnID": "327FD263132A4D08170E1B01FE1BB2E21D0126CE58165C97A9173CA9551BCD70",
      "PreviousTxnLgrSeq": 53391321,
      "RegularKey": "rD9iJmieYHn8jTtPjwwkW2Wm9sVDvPXLoJ",
      "Sequence": 381,
      "TransferRate": 4294967295,
      "index": "13F1A95D7AAB7108D5CE7EEAF504B2894B8C674E6D68499076441C4837282BF8",
      "urlgravatar": "http://www.gravatar.com/avatar/98b4375e1d753e5b91627516f6d70977"
    },
    "ledger_hash": "4C4AC95149B13B539369998675FE6860C52695E83658366F18872181C9F1AEBF",
    "ledger_index": 53391589,
    "status": "success",
    "validated": true
  }
}
```
{% endtab %}

{% tab title="Commandline" %}
```
Loading: "/etc/opt/ripple/rippled.cfg"
2020-Feb-13 00:41:38.642710734 HTTPClient:NFO Connecting to 127.0.0.1:5005

{
   "result" : {
      "account_data" : {
         "Account" : "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
         "AccountTxnID" : "327FD263132A4D08170E1B01FE1BB2E21D0126CE58165C97A9173CA9551BCD70",
         "Balance" : "423013688",
         "Domain" : "6D64756F31332E636F6D",
         "EmailHash" : "98B4375E1D753E5B91627516F6D70977",
         "Flags" : 9633792,
         "LedgerEntryType" : "AccountRoot",
         "MessageKey" : "0000000000000000000000070000000300",
         "OwnerCount" : 9,
         "PreviousTxnID" : "327FD263132A4D08170E1B01FE1BB2E21D0126CE58165C97A9173CA9551BCD70",
         "PreviousTxnLgrSeq" : 53391321,
         "RegularKey" : "rD9iJmieYHn8jTtPjwwkW2Wm9sVDvPXLoJ",
         "Sequence" : 381,
         "TransferRate" : 4294967295,
         "index" : "13F1A95D7AAB7108D5CE7EEAF504B2894B8C674E6D68499076441C4837282BF8",
         "urlgravatar" : "http://www.gravatar.com/avatar/98b4375e1d753e5b91627516f6d70977"
      },
      "ledger_hash" : "BBA4034FB5D5D89987E0987A9491E7B62B16708EECFF04CDB0367BD4D28EB1B5",
      "ledger_index" : 53391568,
      "status" : "success",
      "validated" : true
   }
}
```
{% endtab %}
{% endtabs %}

응답의 `account_data`객체에서 비트 AND( 가장 일반적인 프로그래밍 언어의 연산자)를 사용하여 필드를 플래그 값( 16진수 또는 10진수) `Flags`과 비교합니다. `lsfDisableMaster0x001000001048576&`

예제 코드:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// Assuming the JSON-RPC response above is saved as account_info_response
const lsfDisableMaster = 0x00100000;
let acct_flags = account_info_response.result.account_data.Flags;
if ((lsfDisableMaster & acct_flags) === lsfDisableMaster) {
  console.log("Master key pair is DISABLED");
} else {
  console.log("Master key pair is available for use");
}
```
{% endtab %}

{% tab title="Python" %}
```python
# Assuming the JSON-RPC response above is parsed from JSON
#  and saved as the variable account_info_response
lsfDisableMaster = 0x00100000
acct_flags = account_info_response["result"]["account_data"]["Flags"]
if lsfDisableMaster & acct_flags == lsfDisableMaster:
    print("Master key pair is DISABLED")
else:
    print("Master key pair is available for use")
```
{% endtab %}
{% endtabs %}

이 작업에는 가능한 결과가 두 가지뿐입니다.

* 값 과 같은 0이 아닌 결과는 **마스터 키가 성공적으로 비활성화되었음을**`lsfDisableMaster`이 나타냅니다.
* 결과가 0이면 계정의 마스터 키가 비활성화되지 않았음을 나타냅니다.

결과가 예상과 일치하지 않으면 이전 단계에서 보낸 트랜잭션이 성공적으로 실행되었는지 확인하십시오. 계정의 거래 내역( account\_tx 메소드 )에서 가장 최근 항목이어야 하며 결과 코드가 있어야 합니다 `tesSUCCESS`. 다른 결과 코드가 표시되면 트랜잭션이 성공적으로 실행되지 않은 것입니다. 오류의 원인에 따라 이 단계를 처음부터 다시 시작할 수 있습니다.
