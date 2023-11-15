# 트랜잭션(Transactions)

_트랜잭션_은 XRP Ledger을 수정할 수 있는 유일한 방법입니다. 거래는 서명, 제출, 그리고 [컨센서스 과정](../consensus-protocol/consensus-structure.md)을 거친 후에 검증된 ledger 버전에 편입되어야만 최종적인 것이 됩니다. 일부 ledger 규칙은 또한 [유사 거래](../../references/xrp-ledger/undefined/pseudo-transactions/)를 생성하는데, 이것들은 서명되거나 제출되지 않지만, 여전히 컨센서스에 의해 승인받아야 합니다. 실패한 거래도 또한 ledger에 포함되는데, 그 이유는 스팸 방지 [트랜잭션 비용](transaction-cost.md)을 지불하기 위해 XRP 잔액을 수정하기 때문입니다.

거래는 돈을 보내는 것 이상의 일을 할 수 있습니다. 다양한 [결제 유형](../undefined-1/)을 지원하는 것 외에도, XRP Ledger의 거래는 [암호 키](../undefined-2/cryptographic-keys.md)를 교체하고, 다른 설정을 관리하며, XRP Ledger의 [탈중앙화 거래소](../tokens/decentralized-exchange/)에서 거래를 수행하는데도 사용됩니다. [<mark style="background-color:yellow;">rippled</mark> API 참조](../../tutorials/http-websocket-apis/http-websocket-api-get-started-using-http-websocket-apis.md)는 [트랜잭션 유형의 전체 목록](../../references/xrp-ledger/undefined/undefined-1/)을 제공합니다.

## 거래 식별하기&#x20;

모든 서명된 거래는 그것을 식별하는 고유한 <mark style="background-color:yellow;">"해시"</mark>를 가지고 있습니다. 서버는 거래를 제출할 때 응답에서 해시를 제공하며; [account\_tx 메소드](../../references/http-websocket-apis/api-1/undefined/account\_tx.md)를 사용하여 계정의 거래 이력에서 거래를 찾을 수도 있습니다.

거래 해시는 "결제 증빙"으로 사용될 수 있습니다. 왜냐하면 누구나 그것의 [해시로 트랜잭션을 찾아서](finality-of-results/look-up-transaction-results.md) 최종 상태를 확인할 수 있기 때문입니다.

{% hint style="info" %}
Note:

XRP Ledger의 전체 이력에서는 거래 해시가 고유하다는 규칙에 예외가 있습니다. 초기 SetFee [유사 거래](../../references/xrp-ledger/undefined/pseudo-transactions/) 두 개가 동일한 필드를 가지고 있어서, 동일한 해시인 <mark style="background-color:yellow;">1C15FEA3E1D50F96B6598607FC773FF1F6E0125F30160144BE0C5CBC52F5151B</mark>를 가지게 되었습니다. 이들 거래 중 첫 번째는 ledger 3715073에서, 두 번째는 ledger 3721729에서 발견할 수 있습니다. 더 새로운 SetFee 유사 거래는 <mark style="background-color:yellow;">LedgerSequence</mark> 필드를 포함하므로 그들이 고유하다는 것이 보장됩니다.
{% endhint %}

## 비용 청구 근거&#x20;

실패한 거래에 대한 거래 비용을 부과하는 것이 불공평해 보일 수도 있지만, <mark style="background-color:yellow;">tec</mark> 클래스의 오류들은 다음과 같은 좋은 이유들 때문에 존재합니다:

* 실패한 트랜잭션 이후에 제출된 트랜잭션은 시퀀스 값을 다시 지정할 필요가 없습니다. 실패한 트랜잭션을 ledger에 통합하면 트랜잭션의 시퀀스 번호가 모두 사용되므로 예상되는 시퀀스는 그대로 유지됩니다.
* 트랜잭션을 네트워크 전체에 분산하면 네트워크 부하가 증가합니다. 비용을 부과하면 공격자가 트랜잭션 실패로 네트워크를 악용하기가 더 어려워집니다.&#x20;
* 트랜잭션 비용은 일반적으로 실제 가치로 환산하면 매우 작기 때문에 대량의 트랜잭션을 전송하지 않는 한 사용자에게 피해를 주지 않습니다.

## 거래 승인하기&#x20;

분산형 XRP Ledger에서, 디지털 서명은 거래가 특정한 일련의 행동을 할 수 있도록 승인하는 것을 증명합니다. 서명된 거래만이 네트워크에 제출되고 검증된 ledger에 포함될 수 있습니다. 서명된 거래는 불변의 특성을 가지며: 그 내용은 변경될 수 없으며, 서명은 다른 어떤 거래에 대해서도 유효하지 않습니다.&#x20;

거래는 다음과 같은 유형의 서명들로 승인될 수 있습니다:

* 보내는 주소와 수학적으로 연관된 마스터 개인 키로부터의 단일 서명. [AccountSet 트랜잭션](../../references/xrp-ledger/undefined/undefined-1/accountset.md)을 사용하여 마스터 키 페어를 비활성화하거나 활성화할 수 있습니다.&#x20;
* 주소와 연관된 일반 개인 키와 일치하는 단일 서명. [SetRegularKey 트랜잭션](../../references/xrp-ledger/undefined/undefined-1/setregularkey.md)을 사용하여 일반 키 페어를 추가, 제거, 또는 교체할 수 있습니다.&#x20;
* 주소가 소유한 서명자 목록과 일치하는 [다중 서명](../undefined-2/undefined-1.md). [SignerListSet 트랜잭션](../../references/xrp-ledger/ledger-ledger-data-formats/ledger/signerlist.md)을 사용하여 서명자 목록을 추가, 제거, 또는 교체할 수 있습니다.&#x20;

모든 서명 유형은 다음과 같은 예외를 제외하고 모든 유형의 거래를 승인할 수 있습니다:

* [마스터 공개 키를 비활성화](../../references/xrp-ledger/undefined/undefined-1/accountset.md) 할 수 있는 것은 오직 마스터 개인 키 뿐입니다.&#x20;
* [영구적으로 동결 기능을 포기할 수 있는 것](../tokens/freezing-tokens/)은 오직 마스터 개인 키 뿐입니다.&#x20;
* 당신은 주소에서 거래를 서명하는 마지막 방법을 절대 제거할 수 없습니다.&#x20;

마스터와 일반 키 페어에 대한 더 자세한 정보를 위해, [암호화 키](../undefined-2/cryptographic-keys.md)를 참조해보세요.

## 거래 서명 및 제출&#x20;

XRP Ledger에 거래를 보내는 것은 몇 단계를 포함합니다:

1. [JSON 형식으로 미서명 거래](./#undefined-4)를 생성합니다.&#x20;
2. 하나 이상의 서명을 사용하여 [거래를 승인](./#undefined-2)합니다.&#x20;
3. 거래를 XRP Ledger 서버(보통은 [<mark style="background-color:yellow;">rippled</mark> 인스턴스](../xrp-ledger/))에 제출합니다. 거래가 적절하게 형성되었다면, 서버는 거래를 그것의 현재 버전의 ledger에 잠정적으로 적용하고 거래를 P2P 네트워크의 다른 멤버들에게 전달합니다.&#x20;
4. 컨센서스 과정은 다음 검증된 ledger에 어떤 잠정적인 거래가 포함되는지를 결정합니다.&#x20;
5. 서버는 이러한 트랜잭션을 표준 순서에 따라 이전 ledger에 적용하고 그 결과를 공유합니다.
6. 만약 충분한 [신뢰하 검증자](../xrp-ledger/rippled-rippled-server-modes.md)들이 완전히 같은 ledger을 생성했다면, 그 ledger는 _검증된_ 것으로 선언되고 그 ledger의 [트랜잭션 결과](../../references/xrp-ledger/undefined/pseudo-transactions/undefined/)는 불변의 특성을 가지게 됩니다.&#x20;

XRP 결제를 보내는 대한 상호작용 튜토리얼을 보려면, XRP 보내기를 참조해보세요.

## 미서명 거래 예시&#x20;

다음은 JSON 형식의 미서명 [결제 트랜잭션](../../references/xrp-ledger/undefined/undefined-1/payment.md) 예시입니다:

```json
{
  "TransactionType" : "Payment",
  "Account" : "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
  "Destination" : "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
  "Amount" : {
     "currency" : "USD",
     "value" : "1",
     "issuer" : "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn"
  },
  "Fee": "12",
  "Flags": 2147483648,
  "Sequence": 2,
}
```

XRP Ledger는 트랜잭션 객체가 (<mark style="background-color:yellow;">계정</mark> 내) 발신 주소 필드에서 승인된 경우에만 트랜잭션을 중계하고 실행합니다. 안전하게 서명하는 방법에 대한 지침은 [보안 서명 설정](secure-signing.md)을 참조하세요.

## 서명된 거래 blob 예시&#x20;

거래에 서명하면 "blob"이라고 불리는 이진 데이터 덩어리가 생성되며, 이는 네트워크에 제출될 수 있습니다. 다음은 동일한 거래가 서명된 blob으로 WebSocket API를 사용하여 제출되는 예시입니다:

```json
{
  "id": 2,
  "command": "submit",
  "tx_blob" : "120000240000000461D4838D7EA4C6800000000000000000000000000055534400000000004B4E9C06F24296074F7BC48F92A97916C6DC5EA968400000000000000F732103AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB74483046022100982064CDD3F052D22788DB30B52EEA8956A32A51375E72274E417328EBA31E480221008F522C9DB4B0F31E695AA013843958A10DE8F6BA7D6759BEE645F71A7EB240BE81144B4E9C06F24296074F7BC48F92A97916C6DC5EA983143E9D4A2B8AA0780F682D136F7A56D6724EF53754"
}
```

## 메타데이터를 가진 실행된 거래 예시&#x20;

거래가 실행된 후, XRP Ledger는 거래의 최종 결과와 거래가 XRP Ledger의 공유 상태에 가한 모든 변경 사항을 보여주는 [메타데이터](../../references/xrp-ledger/undefined/pseudo-transactions/undefined-1.md)를 추가합니다.

API를 사용하여 거래 상태를 확인할 수 있습니다. 예를 들어, [tx 명령](../../references/http-websocket-apis/api-1/undefined-1/tx.md)을 사용할 수 있습니다.

{% hint style="info" %}
주의: 거래의 결과, 모든 메타데이터 포함, 거래가 **검증된** ledger에 나타나지 않는 한 최종적이지 않습니다. 결과의 최종성에 대해서도 참조하세요.
{% endhint %}

<mark style="background-color:yellow;">tx</mark> 명령으로부터의 응답 예시:

```json
{
  "id": 6,
  "status": "success",
  "type": "response",
  "result": {
    "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "Amount": {
      "currency": "USD",
      "issuer": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
      "value": "1"
    },
    "Destination": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
    "Fee": "10",
    "Flags": 2147483648,
    "Sequence": 2,
    "SigningPubKey": "03AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB",
    "TransactionType": "Payment",
    "TxnSignature": "3045022100D64A32A506B86E880480CCB846EFA3F9665C9B11FDCA35D7124F53C486CC1D0402206EC8663308D91C928D1FDA498C3A2F8DD105211B9D90F4ECFD75172BAE733340",
    "date": 455224610,
    "hash": "33EA42FC7A06F062A7B843AF4DC7C0AB00D6644DFDF4C5D354A87C035813D321",
    "inLedger": 7013674,
    "ledger_index": 7013674,
    "meta": {
      "AffectedNodes": [
        {
          "ModifiedNode": {
            "FinalFields": {
              "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
              "Balance": "99999980",
              "Flags": 0,
              "OwnerCount": 0,
              "Sequence": 3
            },
            "LedgerEntryType": "AccountRoot",
            "LedgerIndex": "13F1A95D7AAB7108D5CE7EEAF504B2894B8C674E6D68499076441C4837282BF8",
            "PreviousFields": {
              "Balance": "99999990",
              "Sequence": 2
            },
            "PreviousTxnID": "7BF105CFE4EFE78ADB63FE4E03A851440551FE189FD4B51CAAD9279C9F534F0E",
            "PreviousTxnLgrSeq": 6979192
          }
        },
        {
          "ModifiedNode": {
            "FinalFields": {
              "Balance": {
                "currency": "USD",
                "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                "value": "2"
              },
              "Flags": 65536,
              "HighLimit": {
                "currency": "USD",
                "issuer": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
                "value": "0"
              },
              "HighNode": "0000000000000000",
              "LowLimit": {
                "currency": "USD",
                "issuer": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
                "value": "100"
              },
              "LowNode": "0000000000000000"
            },
            "LedgerEntryType": "RippleState",
            "LedgerIndex": "96D2F43BA7AE7193EC59E5E7DDB26A9D786AB1F7C580E030E7D2FF5233DA01E9",
            "PreviousFields": {
              "Balance": {
                "currency": "USD",
                "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                "value": "1"
              }
            },
            "PreviousTxnID": "7BF105CFE4EFE78ADB63FE4E03A851440551FE189FD4B51CAAD9279C9F534F0E",
            "PreviousTxnLgrSeq": 6979192
          }
        }
      ],
      "TransactionIndex": 0,
      "TransactionResult": "tesSUCCESS"
    },
    "validated": true
  }
}
```

