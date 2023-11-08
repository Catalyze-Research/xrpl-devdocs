---
description: 수표 개정에 의해 추가되었습니다.
---

# 정확한 금액의 수표 현금화

수표가 ledger에 있고 만료되지 않았다면, 지정된 수취인은 금액 필드가 있는 수표 현금 트랜잭션을 전송하여 수표에 지정된 금액까지 정확하게 현금화하여 받을 수 있습니다. 송장이나 청구서를 정확히 지불하는 등 특정 금액을 수령하려는 경우 이 방법으로 수표를 현금화할 수 있습니다.

지정된 수취인은 수표를 유연한 금액으로 현금화할 수도 있습니다.

## 요구 조건

수표를 현금화하기 위한 전제 조건은 정확한 금액으로 현금화하든, 유동적인 금액으로 현금화하든 동일합니다.

* 현재 ledger에 있는 수표 객체의 ID가 필요합니다.
  * 예를 들어, 이 예제에서 수표 한 장의 ID는 838766BA2B995C00744175F69A1B11E32C3DBC40E64801A4056FCBD657F57334이지만 이 단계를 직접 수행하려면 다른 ID를 사용해야 합니다.
* 수표에 명시된 수취인의 주소 및 비밀 키입니다. 주소는 수표 객체의 대상 주소와 일치해야 합니다.
* 수표가 토큰에 대한 수표인 경우, 수취인(받는 사람)은 발급자에 대한 신뢰 라인이 있어야 합니다. 신탁 한도는 이전 잔액에 수령할 금액을 더한 금액을 보유할 수 있을 만큼 충분히 높아야 합니다.
* 트랜잭션에 서명하는 안전한 방법.
* XRP Ledger에 연결할 수 있는 클라이언트 라이브러리 또는 HTTP 또는 웹소켓 클라이언트.

## 1. CheckCash 트랜잭션 준비하기

CheckCash 트랜잭션 필드의 값을 파악합니다. 정확한 금액으로 수표를 현금화하려면 다음 필드는 최소한이며, 나머지 필드는 선택사항이거나 서명할 때 자동으로 채워질 수 있습니다:

| Field필드           | 값                         | 설명                                                                                                                                                                                                                                                                                                                                       |
| ----------------- | ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `TransactionType` | String                    | 문자열 CheckCash 값은 이 트랜잭션이 CheckCash 트랜잭션임을 나타냅니다.                                                                                                                                                                                                                                                                                         |
| `Account`         | String (Address)          | 수표를 현금화하는 발신자의 주소입니다. (즉, 당신의 주소입니다.)                                                                                                                                                                                                                                                                                                    |
| `CheckID`         | String                    | 현금화할 ledger 내 수표 객체의 ID입니다. 이 정보는 tx 메소드를 사용하여 CheckCreate 트랜잭션의 메타데이터를 조회하거나 account\_objects 메소드를 사용하여 수표를 조회하여 얻을 수 있습니다.                                                                                                                                                                                                             |
| `Amount`          | String or Object (Amount) | 수표에서 상환할 금액입니다. XRP의 경우, XRP 드롭을 지정하는 문자열이어야 합니다. 토큰의 경우 통화, 발행자 및 값 필드가 있는 객체입니다. 통화 및 발행자 필드는 수표 객체의 해당 필드와 일치해야 하며, 값은 수표 객체의 금액보다 작거나 같아야 합니다. (송금 수수료가 있는 통화의 경우 송금 수수료가 송금 수수료로 결제될 수 있도록 수표를 SendMax보다 적은 금액으로 현금화해야 합니다.) 이 금액을 받을 수 없는 경우 수표 현금화가 실패하고 수표가 ledger에 남아 있으므로 다시 시도할 수 있습니다. 통화 금액 지정에 대한 자세한 내용은 통화 금액 지정을 참조하세요. |

## 정확한 금액에 대한 수표 현금 준비 예시

다음 예는 수표를 정해진 금액으로 현금화하기 위해 트랜잭션을 준비하는 방법을 보여줍니다.

{% tabs %}
{% tab title="JSON-RPC WebSocket, or Commandline" %}
```json
{
  "Account": "rfkE1aSy9G8Upk4JssnwBxhEv5p4mn2KTy",
  "TransactionType": "CheckCash",
  "Amount": "100000000",
  "CheckID": "838766BA2B995C00744175F69A1B11E32C3DBC40E64801A4056FCBD657F57334",
  "Fee": "12"
}
```
{% endtab %}
{% endtabs %}

## 2. CheckCash 트랜잭션에 서명하기

트랜잭션에 서명하는 가장 안전한 방법은 클라이언트 라이브러리를 사용하여 로컬로 서명하는 것입니다. 또는 자체 rippled 노드를 실행하는 경우 서명 방법을 사용하여 트랜잭션에 서명할 수 있지만, 신뢰할 수 있고 암호화된 연결 또는 로컬(동일 컴퓨터) 연결을 통해 수행해야 합니다.

어떤 경우든 나중에 사용할 수 있도록 서명된 트랜잭션의 식별 해시를 기록해 두세요.

## 요청 예시

{% tabs %}
{% tab title="Commandline" %}
```json
rippled sign s████████████████████████████ '{
  "Account": "rfkE1aSy9G8Upk4JssnwBxhEv5p4mn2KTy",
  "TransactionType": "CheckCash",
  "Amount": "100000000",
  "CheckID": "838766BA2B995C00744175F69A1B11E32C3DBC40E64801A4056FCBD657F57334",
  "Fee": "12"
}'
```
{% endtab %}
{% endtabs %}

## &#x20;응답 예시

{% tabs %}
{% tab title="Commandline" %}
```json
Loading: "/etc/opt/ripple/rippled.cfg"
2018-Jan-24 01:17:54 HTTPClient:NFO Connecting to 127.0.0.1:5005

{
   "result" : {
      "status" : "success",
      "tx_blob" : "120011228000000024000000015018838766BA2B995C00744175F69A1B11E32C3DBC40E64801A4056FCBD657F57334614000000005F5E10068400000000000000C732102F135B14C552968B0ABE8493CC4C5795A7484D73F6BFD01379F73456F725F66ED74473045022100C64278AC90B841CD3EA9889A4847CAB3AC9927057A34130810FAA7FAC0C6E3290220347260A4C0A6DC9B699DA12510795B2B3414E1FA222AF743226345FBAAEF937C811449FF0C73CA6AF9733DA805F76CA2C37776B7C46B",
      "tx_json" : {
         "Account" : "rfkE1aSy9G8Upk4JssnwBxhEv5p4mn2KTy",
         "Amount" : "100000000",
         "CheckID" : "838766BA2B995C00744175F69A1B11E32C3DBC40E64801A4056FCBD657F57334",
         "Fee" : "12",
         "Flags" : 2147483648,
         "Sequence" : 1,
         "SigningPubKey" : "02F135B14C552968B0ABE8493CC4C5795A7484D73F6BFD01379F73456F725F66ED",
         "TransactionType" : "CheckCash",
         "TxnSignature" : "3045022100C64278AC90B841CD3EA9889A4847CAB3AC9927057A34130810FAA7FAC0C6E3290220347260A4C0A6DC9B699DA12510795B2B3414E1FA222AF743226345FBAAEF937C",
         "hash" : "0521707D510858BC8AF69D2227E1D1ADA7DB7C5B4B74115BCD0D91B62AFA8EDC"
      }
   }
}
```
{% endtab %}
{% endtabs %}

## 3. 서명된 CheckCash 트랜잭션 제출하기

이전 단계에서 서명된 트랜잭션 blob을 가져와 rippled 서버에 제출합니다. rippled 서버를 실행하지 않아도 안전하게 이 작업을 수행할 수 있습니다. 응답에는 임시 결과가 포함되며, 이 결과는 일반적으로 최종 결과가 아닙니다. 대기 중인 트랜잭션은 일반적으로 다음 오픈 ledger 버전에 포함되므로(일반적으로 제출 후 약 10초 후), terQUEUED의 임시 응답도 괜찮습니다.

{% hint style="info" %}
Tip:

예비 결과가 tefMAX\_LEDGER인 경우, 트랜잭션이 영구적으로 실패한 것은 LastLedgerSequence 매개변수가 현재 ledger보다 낮기 때문입니다. 이는 트랜잭션 준비와 제출 사이에 예상되는 ledger 버전 수보다 오래 걸리는 경우 발생합니다. 이 경우 더 높은 LastLedgerSequence 값으로 1단계부터 다시 시작하세요.
{% endhint %}

## 요청 예시

{% tabs %}
{% tab title="Commandline" %}
```json
rippled submit 120011228000000024000000015018838766BA2B995C00744175F69A1B11E32C3DBC40E64801A4056FCBD657F57334614000000005F5E10068400000000000000C732102F135B14C552968B0ABE8493CC4C5795A7484D73F6BFD01379F73456F725F66ED74473045022100C64278AC90B841CD3EA9889A4847CAB3AC9927057A34130810FAA7FAC0C6E3290220347260A4C0A6DC9B699DA12510795B2B3414E1FA222AF743226345FBAAEF937C811449FF0C73CA6AF9733DA805F76CA2C37776B7C46B
```
{% endtab %}
{% endtabs %}

## 응답 예시

{% tabs %}
{% tab title="Commandline" %}
```json
Loading: "/etc/opt/ripple/rippled.cfg"
2018-Jan-24 01:17:54 HTTPClient:NFO Connecting to 127.0.0.1:5005

{
  "result" : {
    "engine_result" : "tesSUCCESS",
    "engine_result_code" : 0,
    "engine_result_message" : "The transaction was applied. Only final in a validated ledger.",
    "status" : "success",
    "tx_blob" : "120011228000000024000000015018838766BA2B995C00744175F69A1B11E32C3DBC40E64801A4056FCBD657F57334614000000005F5E10068400000000000000C732102F135B14C552968B0ABE8493CC4C5795A7484D73F6BFD01379F73456F725F66ED74473045022100C64278AC90B841CD3EA9889A4847CAB3AC9927057A34130810FAA7FAC0C6E3290220347260A4C0A6DC9B699DA12510795B2B3414E1FA222AF743226345FBAAEF937C811449FF0C73CA6AF9733DA805F76CA2C37776B7C46B",
    "tx_json" : {
      "Account" : "rfkE1aSy9G8Upk4JssnwBxhEv5p4mn2KTy",
      "Amount" : "100000000",
      "CheckID" : "838766BA2B995C00744175F69A1B11E32C3DBC40E64801A4056FCBD657F57334",
      "Fee" : "12",
      "Flags" : 2147483648,
      "Sequence" : 1,
      "SigningPubKey" : "02F135B14C552968B0ABE8493CC4C5795A7484D73F6BFD01379F73456F725F66ED",
      "TransactionType" : "CheckCash",
      "TxnSignature" : "3045022100C64278AC90B841CD3EA9889A4847CAB3AC9927057A34130810FAA7FAC0C6E3290220347260A4C0A6DC9B699DA12510795B2B3414E1FA222AF743226345FBAAEF937C",
      "hash" : "0521707D510858BC8AF69D2227E1D1ADA7DB7C5B4B74115BCD0D91B62AFA8EDC"
    }
  }
}
```
{% endtab %}
{% endtabs %}

## 4. 유효성 검사 대기

라이브 네트워크(mainnet, testnet 또는 devnet 포함)에서는 ledger 자동으로 닫힐 때까지 4\~7초 정도 기다릴 수 있습니다.

stand-alone 모드에서 rippled를 실행하는 경우, ledger\_accept 메소드를 사용하여 ledger을 수동으로 닫아야 합니다.

## 5. 최종 결과 확인

tx 메소드를 CheckCash 트랜잭션의 식별 해시와 함께 사용하여 트랜잭션의 상태를 확인합니다. 트랜잭션의 메타데이터에서 "TransactionResult": "tesSUCCESS" 필드와 이 결과가 최종 결과임을 나타내는 "validated": true 필드를 확인합니다.

수표가 정확한 금액으로 현금화되어 성공했다면 수취인에게 정확히 해당 금액이 입금된 것으로 간주할 수 있습니다(매우 큰 금액 또는 매우 작은 금액의 경우 반올림이 가능함).

수표 현금화에 실패한 경우 수표는 ledger에 남아 있으므로 나중에 다시 현금화를 시도할 수 있습니다. 대신 유연한 금액으로 수표를 현금화할 수 있습니다.

## 요청 예시

{% tabs %}
{% tab title="Commandline" %}
```
rippled tx 0521707D510858BC8AF69D2227E1D1ADA7DB7C5B4B74115BCD0D91B62AFA8EDC
```
{% endtab %}
{% endtabs %}

## 응답 예시

{% tabs %}
{% tab title="Commandline" %}
```json
Loading: "/etc/opt/ripple/rippled.cfg"
2018-Jan-24 01:18:39 HTTPClient:NFO Connecting to 127.0.0.1:5005

{
   "result" : {
      "Account" : "rfkE1aSy9G8Upk4JssnwBxhEv5p4mn2KTy",
      "Amount" : "100000000",
      "CheckID" : "838766BA2B995C00744175F69A1B11E32C3DBC40E64801A4056FCBD657F57334",
      "Fee" : "12",
      "Flags" : 2147483648,
      "Sequence" : 1,
      "SigningPubKey" : "02F135B14C552968B0ABE8493CC4C5795A7484D73F6BFD01379F73456F725F66ED",
      "TransactionType" : "CheckCash",
      "TxnSignature" : "3045022100C64278AC90B841CD3EA9889A4847CAB3AC9927057A34130810FAA7FAC0C6E3290220347260A4C0A6DC9B699DA12510795B2B3414E1FA222AF743226345FBAAEF937C",
      "date" : 570071920,
      "hash" : "0521707D510858BC8AF69D2227E1D1ADA7DB7C5B4B74115BCD0D91B62AFA8EDC",
      "inLedger" : 9,
      "ledger_index" : 9,
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
               "ModifiedNode" : {
                  "FinalFields" : {
                     "Account" : "rfkE1aSy9G8Upk4JssnwBxhEv5p4mn2KTy",
                     "Balance" : "1000099999988",
                     "Flags" : 0,
                     "OwnerCount" : 0,
                     "Sequence" : 2
                  },
                  "LedgerEntryType" : "AccountRoot",
                  "LedgerIndex" : "38E1EF3284A45B090D549EFFB014ACF68927FE0884CDAF01CE3629DF90542D66",
                  "PreviousFields" : {
                     "Balance" : "1000000000000",
                     "Sequence" : 1
                  },
                  "PreviousTxnID" : "3E14D859F6B4BE923323EFC94571606455921E65173147A89BC6EDDA4374B294",
                  "PreviousTxnLgrSeq" : 5
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
                     "InvoiceID" : "6F1DFD1D0FE8A32E40E1F2C05CF1C15545BAB56B617F9C6C2D63A6B704BEF59B",
                     "OwnerNode" : "0000000000000000",
                     "PreviousTxnID" : "0FD9F719CDE29E6F6DF752B93EB9AC6FBB493BF989F2CB63B8C0E73A8DCDF61A",
                     "PreviousTxnLgrSeq" : 8,
                     "SendMax" : "100000000",
                     "Sequence" : 4
                  },
                  "LedgerEntryType" : "Check",
                  "LedgerIndex" : "838766BA2B995C00744175F69A1B11E32C3DBC40E64801A4056FCBD657F57334"
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
                     "Balance" : "4999899999952",
                     "Flags" : 0,
                     "OwnerCount" : 1,
                     "Sequence" : 5
                  },
                  "LedgerEntryType" : "AccountRoot",
                  "LedgerIndex" : "D3A1DBAA28717975A9119EC4CBC891BA9A66236C484F03C9911F463AD3B66DE0",
                  "PreviousFields" : {
                     "Balance" : "4999999999952",
                     "OwnerCount" : 2
                  },
                  "PreviousTxnID" : "0FD9F719CDE29E6F6DF752B93EB9AC6FBB493BF989F2CB63B8C0E73A8DCDF61A",
                  "PreviousTxnLgrSeq" : 8
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
