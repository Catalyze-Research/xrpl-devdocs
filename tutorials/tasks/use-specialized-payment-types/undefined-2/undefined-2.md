---
description: 수표 개정에 의해 추가되었습니다.
---

# 유연한 금액으로 수표 현금화

수표가 ledger에 있고 만료되지 않은 경우, 지정된 수취인은 DeliverMin 필드가 있는 CheckCash 트랜잭션을 전송하여 수표를 현금화하여 유연한 금액을 받을 수 있습니다. 이러한 방식으로 수표를 현금화하면 수취인은 수표의 송금인에게 수표의 전체 SendMax 금액 또는 사용 가능한 금액만큼 인출하여 배송할 수 있는 만큼의 금액을 받게 됩니다. 수표 수취인에게 최소 DeliverMin 금액 이상이 전달되지 않으면 현금화가 실패합니다.

수표에서 최대한 많은 금액을 받으려는 경우 수표를 유연한 금액으로 현금화할 수 있습니다.

지정된 수취인이 정확한 금액으로 수표를 현금화할 수도 있습니다.

## 요구 조건

수표를 현금화하기 위한 전제 조건은 정확한 금액으로 현금화하든 유동적인 금액으로 현금화하든 동일합니다.

* 현재 ledger에 있는 수표 객체의 ID가 필요합니다.
  * 예를 들어, 이 예제에서 수표 한 장의 ID는 838766BA2B995C00744175F69A1B11E32C3DBC40E64801A4056FCBD657F57334이지만, 이 단계를 직접 수행하려면 다른 ID를 사용해야 합니다.
* 수표에 명시된 수취인의 주소 및 비밀 키입니다. 주소는 수표 객체의 대상 주소와 일치해야 합니다.
* 수표가 토큰에 대한 수표인 경우, 수취인(받는 사람)은 발급자에 대한 신뢰선이 있어야 합니다. 신뢰선의 한도는 이전 잔액에 수령할 금액을 더한 금액을 보유할 수 있을 만큼 충분히 높아야 합니다.
* 트랜잭션에 서명하는 안전한 방법이 있어야 합니다.
* XRP Ledger에 연결할 수 있는 클라이언트 라이브러리 또는 HTTP 또는 웹소켓 클라이언트가 있어야 합니다.

## 1. CheckCash 트랜잭션 준비하기

CheckCash 트랜잭션 필드의 값을 파악합니다. 유연한 금액으로 수표를 현금화하려면 다음 필드는 최소한이며, 나머지 필드는 선택사항이거나 서명할 때 자동으로 채워질 수 있습니다:

| 필드                | 값                         | 설명                                                                                                                                                                                                                                                        |
| ----------------- | ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `TransactionType` | String                    | CheckCash 값은 이 트랜잭션이 CheckCash 트랜잭션임을 나타냅니다.                                                                                                                                                                                                              |
| `Account`         | String (Address)          | 수표를 현금화하는 발신자의 주소입니다. (즉, 회원님의 주소입니다.)                                                                                                                                                                                                                    |
| `CheckID`         | String                    | 현금화할 ledger 내 수표 객체의 ID입니다. 이 정보는 tx 메소드를 사용하여 CheckCreate 트랜잭션의 메타데이터를 조회하거나 account\_objects 메소드를 사용하여 수표를 조회하여 얻을 수 있습니다.                                                                                                                              |
| `DeliverMin`      | String or Object (Amount) | 수표에서 수령할 최소 금액입니다. 이 금액 이상을 받을 수 없으면 수표 현금화가 실패하고 다시 시도할 수 있도록 ledger에 수표가 남습니다. XRP의 경우, XRP 드롭을 지정하는 문자열이어야 합니다. 토큰의 경우 통화, 발행자, 가치 필드가 있는 객체입니다. 통화 및 발행자 필드는 수표 객체의 해당 필드와 일치해야 하며, 값은 수표 객체의 금액보다 작거나 같아야 합니다. 통화 금액 지정에 대한 자세한 내용은 통화 금액 지정을 참조하세요. |

## 유연한 금액에 대한 CheckCash 준비 예제

다음 예제에서는 수표를 자유 금액으로 현금화하기 위해 트랜잭션을 준비하는 방법을 보여 줍니다.

{% tabs %}
{% tab title="JSON-RPC WebSocket or Commandline" %}
```json
{
  "Account": "rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis",
  "TransactionType": "CheckCash",
  "DeliverMin": "95000000",
  "CheckID": "2E0AD0740B79BE0AAE5EDD1D5FC79E3C5C221D23C6A7F771D85569B5B91195C2"
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
  "Account": "rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis",
  "TransactionType": "CheckCash",
  "DeliverMin": "95000000",
  "CheckID": "84C61BE9B39B2C4A2267F67504404F1EC76678806C1B901EA781D1E3B4CE0CD9"
}'
```
{% endtab %}
{% endtabs %}

## 응답 예시

{% tabs %}
{% tab title="Commandline" %}
```json
Loading: "/etc/opt/ripple/rippled.cfg"
2018-Apr-03 00:09:53 HTTPClient:NFO Connecting to 127.0.0.1:5005

{
   "result" : {
      "status" : "success",
      "tx_blob" : "12001122800000002400000004501884C61BE9B39B2C4A2267F67504404F1EC76678806C1B901EA781D1E3B4CE0CD968400000000000000A6A4000000005A995C073210361ACFCB478BCAE01451F95060AF94F70365BF00D7B4661EC2C69EA383762516C7446304402203D7EC220D48AA040D6915C160275D202F7F808E2B58F11B1AB05FB5E5CFCC6C00220304BBD3AD32E13150E0ED7247F2ADFAE83D0ECE329E20CFE0F8DF352934DD2FC8114A8B6B9FF3246856CADC4A0106198C066EA1F9C39",
      "tx_json" : {
         "Account" : "rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis",
         "CheckID" : "84C61BE9B39B2C4A2267F67504404F1EC76678806C1B901EA781D1E3B4CE0CD9",
         "DeliverMin" : "95000000",
         "Fee" : "10",
         "Flags" : 2147483648,
         "Sequence" : 4,
         "SigningPubKey" : "0361ACFCB478BCAE01451F95060AF94F70365BF00D7B4661EC2C69EA383762516C",
         "TransactionType" : "CheckCash",
         "TxnSignature" : "304402203D7EC220D48AA040D6915C160275D202F7F808E2B58F11B1AB05FB5E5CFCC6C00220304BBD3AD32E13150E0ED7247F2ADFAE83D0ECE329E20CFE0F8DF352934DD2FC",
         "hash" : "A0AFE572E4736CBF49FF4D0D3FF8FDB0C4D31BD10CB4EB542230F85F0F2DD222"
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
rippled submit 12001122800000002400000004501884C61BE9B39B2C4A2267F67504404F1EC76678806C1B901EA781D1E3B4CE0CD968400000000000000A6A4000000005A995C073210361ACFCB478BCAE01451F95060AF94F70365BF00D7B4661EC2C69EA383762516C7446304402203D7EC220D48AA040D6915C160275D202F7F808E2B58F11B1AB05FB5E5CFCC6C00220304BBD3AD32E13150E0ED7247F2ADFAE83D0ECE329E20CFE0F8DF352934DD2FC8114A8B6B9FF3246856CADC4A0106198C066EA1F9C39
```
{% endtab %}
{% endtabs %}

## 응답 예시

{% tabs %}
{% tab title="Commandline" %}
```json
Loading: "/etc/opt/ripple/rippled.cfg"
2018-Apr-03 00:10:30 HTTPClient:NFO Connecting to 127.0.0.1:5005

{
   "result" : {
      "engine_result" : "tesSUCCESS",
      "engine_result_code" : 0,
      "engine_result_message" : "The transaction was applied. Only final in a validated ledger.",
      "status" : "success",
      "tx_blob" : "12001122800000002400000004501884C61BE9B39B2C4A2267F67504404F1EC76678806C1B901EA781D1E3B4CE0CD968400000000000000A6A4000000005A995C073210361ACFCB478BCAE01451F95060AF94F70365BF00D7B4661EC2C69EA383762516C7446304402203D7EC220D48AA040D6915C160275D202F7F808E2B58F11B1AB05FB5E5CFCC6C00220304BBD3AD32E13150E0ED7247F2ADFAE83D0ECE329E20CFE0F8DF352934DD2FC8114A8B6B9FF3246856CADC4A0106198C066EA1F9C39",
      "tx_json" : {
         "Account" : "rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis",
         "CheckID" : "84C61BE9B39B2C4A2267F67504404F1EC76678806C1B901EA781D1E3B4CE0CD9",
         "DeliverMin" : "95000000",
         "Fee" : "10",
         "Flags" : 2147483648,
         "Sequence" : 4,
         "SigningPubKey" : "0361ACFCB478BCAE01451F95060AF94F70365BF00D7B4661EC2C69EA383762516C",
         "TransactionType" : "CheckCash",
         "TxnSignature" : "304402203D7EC220D48AA040D6915C160275D202F7F808E2B58F11B1AB05FB5E5CFCC6C00220304BBD3AD32E13150E0ED7247F2ADFAE83D0ECE329E20CFE0F8DF352934DD2FC",
         "hash" : "A0AFE572E4736CBF49FF4D0D3FF8FDB0C4D31BD10CB4EB542230F85F0F2DD222"
      }
   }
}
```
{% endtab %}
{% endtabs %}

## 4. 유효성 검사 대기

라이브 네트워크(mainnet, testnet 또는 devnet 포함)에서는 ledger이 자동으로 닫힐 때까지 4\~7초 정도 기다릴 수 있습니다.

stand-alone 모드에서 rippled를 실행하는 경우, ledger\_accept 메소드를 사용하여 ledger을 수동으로 닫아야 합니다.

## 5. 최종 결과 확인

tx 메소드를 CheckCash 트랜잭션의 식별 해시와 함께 사용하여 트랜잭션의 상태를 확인합니다. 트랜잭션의 메타데이터에서 "TransactionResult": "tesSUCCESS" 필드와 이 결과가 최종 결과임을 나타내는 "validated": true 필드를 확인합니다.

## 요청 예시

{% tabs %}
{% tab title="Commandline" %}
```json
rippled tx A0AFE572E4736CBF49FF4D0D3FF8FDB0C4D31BD10CB4EB542230F85F0F2DD222
```
{% endtab %}
{% endtabs %}

## 응답 예시

{% tabs %}
{% tab title="Commandline" %}
```json
Loading: "/etc/opt/ripple/rippled.cfg"
2018-Apr-03 00:11:17 HTTPClient:NFO Connecting to 127.0.0.1:5005

{
   "result" : {
      "Account" : "rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis",
      "CheckID" : "84C61BE9B39B2C4A2267F67504404F1EC76678806C1B901EA781D1E3B4CE0CD9",
      "DeliverMin" : "95000000",
      "Fee" : "10",
      "Flags" : 2147483648,
      "Sequence" : 4,
      "SigningPubKey" : "0361ACFCB478BCAE01451F95060AF94F70365BF00D7B4661EC2C69EA383762516C",
      "TransactionType" : "CheckCash",
      "TxnSignature" : "304402203D7EC220D48AA040D6915C160275D202F7F808E2B58F11B1AB05FB5E5CFCC6C00220304BBD3AD32E13150E0ED7247F2ADFAE83D0ECE329E20CFE0F8DF352934DD2FC",
      "date" : 576029432,
      "hash" : "A0AFE572E4736CBF49FF4D0D3FF8FDB0C4D31BD10CB4EB542230F85F0F2DD222",
      "inLedger" : 8005386,
      "ledger_index" : 8005386,
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
               "ModifiedNode" : {
                  "FinalFields" : {
                     "Account" : "rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis",
                     "Balance" : "10099999960",
                     "Flags" : 0,
                     "OwnerCount" : 2,
                     "Sequence" : 5
                  },
                  "LedgerEntryType" : "AccountRoot",
                  "LedgerIndex" : "7939126A732EBBDEC715FD3CCB056EB31E65228CA17E3B2901E7D30B90FD03D3",
                  "PreviousFields" : {
                     "Balance" : "9999999970",
                     "Sequence" : 4
                  },
                  "PreviousTxnID" : "0283465F0D21BE6B1E91ABDE17266C24C1B4915BAAA9A88CC098A98D5ECD3E9E",
                  "PreviousTxnLgrSeq" : 8005334
               }
            },
            {
               "DeletedNode" : {
                  "FinalFields" : {
                     "Account" : "rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za",
                     "Destination" : "rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis",
                     "DestinationNode" : "0000000000000000",
                     "DestinationTag" : 1,
                     "Flags" : 0,
                     "InvoiceID" : "46060241FABCF692D4D934BA2A6C4427CD4279083E38C77CBE642243E43BE291",
                     "OwnerNode" : "0000000000000000",
                     "PreviousTxnID" : "09D992D4C89E2A24D4BA9BB57ED81C7003815940F39B7C87ADBF2E49034380BB",
                     "PreviousTxnLgrSeq" : 7841263,
                     "SendMax" : "100000000",
                     "Sequence" : 4
                  },
                  "LedgerEntryType" : "Check",
                  "LedgerIndex" : "84C61BE9B39B2C4A2267F67504404F1EC76678806C1B901EA781D1E3B4CE0CD9"
               }
            },
            {
               "ModifiedNode" : {
                  "FinalFields" : {
                     "Account" : "rBXsgNkPcDN2runsvWmwxk3Lh97zdgo9za",
                     "Balance" : "9899999920",
                     "Flags" : 0,
                     "OwnerCount" : 2,
                     "Sequence" : 8
                  },
                  "LedgerEntryType" : "AccountRoot",
                  "LedgerIndex" : "A9A591BA661F69433D5BEAA49F10BA2B8DEA5183EF414B9130BFE5E0328FE875",
                  "PreviousFields" : {
                     "Balance" : "9999999920",
                     "OwnerCount" : 3
                  },
                  "PreviousTxnID" : "54A7A917BE9AC13962251BCF1D09803C7BBE75882B8BFC987B5933A566A48215",
                  "PreviousTxnLgrSeq" : 8004870
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
         "TransactionIndex" : 4,
         "TransactionResult" : "tesSUCCESS"
      },
      "status" : "success",
      "validated" : true
   }
}
```
{% endtab %}
{% endtabs %}

## 오류 처리

수표 현금화에 tec-class 코드가 있는 경우 전체 트랜잭션 응답 목록에서 코드를 조회하고 그에 따라 응답하세요. CheckCash 트랜잭션에서 흔히 발생할 수 있는 몇 가지 가능성입니다:

| Result Code           | Meaning                                                                                                  | How to Respond                                                                                                                                                                                                     |
| --------------------- | -------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `tecEXPIRED`          | 수표의 유효기간이 만료되었습니다.                                                                                       | 수표를 취소하고 송금인에게 만료 시간을 나중에 설정하여 새 수표를 생성하도록 요청하세요.                                                                                                                                                                  |
| `tecNO_ENTRY`         | 수표 ID가 존재하지 않습니다.                                                                                        | CheckCash 거래의 CheckID가 올바른지 확인합니다. 수표가 이미 취소되었거나 성공적으로 현금화되지 않았는지 확인합니다.                                                                                                                                           |
| `tecNO_LINE`          | 수취인에게 수표 통화에 대한 trust line이 없습니다.                                                                        | 이 발행자의 화폐를 보유하려면 [TrustSet transaction](https://xrpl.org/trustset.html) 트랜잭션을 사용하여 지정된 통화 및 발행자에 대해 합리적인 한도로 신뢰선을 설정한 다음 수표를 다시 현금화해 보내세요.                                                                         |
| `tecNO_PERMISSION`    | CheckCash 트랜잭션의 발신자가 수표의 목적지가 아닙니다.      수표의 목적지를 다시 확인하세요.                                              | 수표의 목적지를 다시 확인하세요.                                                                                                                                                                                                 |
| `tecNO_AUTH`          | 발행자가 승인된 trust lines를 사용하고 있지만 수취인의 발행자에 대한 trust line이 승인되지 않았습니다.                                      | 발행자에게 이 신뢰선을 승인하도록 요청하고 승인한 후 수표를 다시 현금화해 보세요.                                                                                                                                                                     |
| `tecPATH_PARTIAL`     | 수표에 충분한 토큰이 전달되지 않은 이유는 trust line 한도 때문이거나 송금인에게 송금할 토큰 잔액이 충분하지 않기 때문입니다(발행자의 송금 수수료가 있는 경우 이 수수료 포함). | 신뢰선 한도가 문제인 경우, [TrustSet](https://xrpl.org/trustset.html) 트랜잭션을 보내 한도를 늘리거나(원하는 경우) 통화를 일부 사용하여 잔액을 낮춘 다음 수표를 다시 현금화해 보시기 바랍니다. 송금인의 잔액에 문제가 있는 경우 송금인이 수표의 통화를 더 많이 보유할 때까지 기다리거나 더 적은 금액으로 수표를 현금화하려고 다시 시도합니다. |
| `tecUNFUNDED_PAYMENT` | 수표에 충분한 XRP가 입금되지 않았습니다.                                                                                 | 송금인이 더 많은 XRP를 보유할 때까지 기다리거나 더 적은 금액으로 수표를 현금화하도록 다시 시도하세요.                                                                                                                                                        |

## 6. 송금된 금액 확인

수표를 유연한 최소 금액으로 현금화하여 성공했다면 수표가 최소 금액 이상으로 현금화된 것으로 간주할 수 있습니다. 정확한 송금액을 확인하려면 트랜잭션 메타데이터를 확인하세요. 메타데이터의 delivered\_amount 필드에 정확한 전달 금액이 표시됩니다. (이 필드는 수표가 유동적인 금액으로 현금화된 경우에만 제공됩니다. 수표가 고정 금액으로 성공적으로 현금화되었다면 인출된 금액은 CheckCash 트랜잭션의 금액과 같습니다).

* XRP의 경우, 수표 발신자의 AccountRoot 객체에는 XRP 잔액 필드가 차감됩니다. 수표 수취인(CheckCash 트랜잭션을 보낸 사람)의 AccountRoot 객체에는 CheckCash 트랜잭션의 최소 전달 금액에서 트랜잭션 전송 비용을 뺀 금액만큼의 XRP 잔액이 입금됩니다.\
  예를 들어, 다음 수정된 노드에서는 수표의 수취인이자 이 수표의 트랜잭션을 보낸 사람인 rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis 계정의 XRP 잔액이 9999999970 드롭에서 10099999960 드롭으로 변경되었음을 보여 줍니다. 이는 트랜잭션을 처리한 결과 수취인에게 99.99999 XRP가 순액으로 적립되었다는 뜻입니다.

```json
  {
    "ModifiedNode": {
      "FinalFields": {
         "Account": "rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis",
         "Balance": "10099999960",
         "Flags": 0,
         "OwnerCount": 2,
         "Sequence": 5
      },
      "LedgerEntryType": "AccountRoot",
      "LedgerIndex": "7939126A732EBBDEC715FD3CCB056EB31E65228CA17E3B2901E7D30B90FD03D3",
      "PreviousFields": {
         "Balance": "9999999970",
         "Sequence": 4
      },
      "PreviousTxnID": "0283465F0D21BE6B1E91ABDE17266C24C1B4915BAAA9A88CC098A98D5ECD3E9E",
      "PreviousTxnLgrSeq": 8005334
    }
  }
```

99.9999999 XRP의 순 금액에는 이 CheckCash 트랜잭션 전송에 대한 비용을 지불하기 위해 소멸된 트랜잭션 비용을 차감한 금액이 포함됩니다. 트랜잭션 지침의 다음 부분을 보면 트랜잭션 비용(수수료 필드)이 10드랍의 XRP임을 알 수 있습니다. 이를 순 잔액 변동에 더하면 수취인인 rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis가 수표 현금화에 대해 정확히 100XRP의 총액을 입금받았다는 결론을 내릴 수 있습니다.

```json
"Account" : "rGPnRH1EBpHeTF2QG8DCAgM7z5pb75LAis",
"TransactionType" : "CheckCash",
"DeliverMin" : "95000000",
"Fee" : "10",
```

* 수표의 발신자 또는 수신자가 발행자인 토큰의 경우, 해당 계정 간의 신뢰선을 나타내는 RippleState 객체는 수표의 수신자에게 유리하도록 잔액이 조정됩니다.
* 타사 발행자가 있는 토큰의 경우, 발신자와 발행자, 발행자와 수신자를 연결하는 신뢰선을 나타내는 두 개의 RippleState 객체에 변경 사항이 있습니다. 수표 발신자와 발행자 간의 관계를 나타내는 RippleState 객체는 발행자에게 유리하게 잔액이 변경되고, 발행자와 수취인 간의 관계를 나타내는 RippleState 객체는 수취인에게 유리하게 잔액이 변경됩니다.
  * 토큰에 송금 수수료가 있는 경우, 수표의 발신자에게는 수취인에게 입금된 금액보다 더 많은 금액이 인출될 수 있습니다. (차액은 이체 수수료이며, 이 수수료는 발행자에게 감소된 순 의무로 반환됩니다.)

&#x20;
