# 트랜잭션 결과 조회(Look Up Transaction Results)

XRP Ledger를 효과적으로 사용하려면 거래 결과를 이해해야 합니다: 거래가 성공했는가? 거래는 무엇을 수행했는가? 실패했다면 그 이유는 무엇인가?

XRP Ledger는 모든 데이터가 공개적으로 기록되고 각 새로운 ledger 버전으로 신중하고 안전하게 업데이트되는 공유 시스템입니다. 누구나 어떤 거래의 정확한 결과를 조회하고 거래 메타데이터를 읽어 그 거래가 무엇을 수행했는지 확인할 수 있습니다.

이 문서는 거래가 특정 결과에 도달한 이유를 저수준에서 이해하는 방법에 대해 설명합니다. 최종 사용자의 경우, 거래에 대한 처리된 뷰를 보는 것이 더 쉽습니다. 예를 들어, 기록된 모든 거래에 대한 영어로 된 설명을 얻기 위해 XRP 차트를 사용할 수 있습니다.

## 요구 사항(Prerequisites)

이 지침에서 설명한 바와 같이 거래 결과를 이해하려면 다음이 필요합니다:

* 이해하려는 거래를 알고 있어야 합니다. 거래의 식별 해시를 알고 있다면 그 방법으로 거래를 찾을 수 있습니다. 또한 최근에 실행된 ledger의 거래나 특정 계정에 가장 최근에 영향을 준 거래를 확인할 수 있습니다.
* 거래가 제출되었을 때 필요한 기록을 가지고 있고 신뢰할 수 있는 정보를 제공하는 rippled 서버에 액세스 할 수 있어야 합니다.
  * 최근에 제출한 거래의 결과를 확인하기 위해 제출한 서버를 사용해야 합니다. 이때 서버가 그 시간 동안 네트워크와 동기화를 유지하는 것이 중요합니다.
  * 오래된 거래의 결과를 확인하기 위해서는 전체 기록 서버를 사용하는 것이 좋습니다.

{% hint style="info" %}
Tip:

XRP Ledger에서 거래 데이터를 조회하는 다른 방법도 있습니다. 예를 들어 Data API 및 기타 내보낸 데이터베이스가 있지만, 이들 인터페이스는 비공식적입니다. 이 문서에서는 가능한 가장 직접적이고 공식적인 결과를 얻기 위해 rippled API를 직접 사용하여 데이터를 조회하는 방법을 설명합니다.
{% endhint %}

## 1. 거래 상태 얻기(Get Transaction Status)

거래가 성공했는지 실패했는지 여부를 알아내는 것은 두 부분으로 나뉩니다:

* 거래가 검증된 ledger에 포함되었는가?
* 만약 그렇다면 ledger 상태에 어떤 변화가 일어났는가?

거래가 검증된 ledger에 포함되었는지 알아내려면, 일반적으로 거래가 포함될 수 있는 모든 ledger에 접근할 수 있어야 합니다. 이를 수행하는 가장 간단하고 효과적인 방법은 전체 기록 서버에서 거래를 조회하는 것입니다. rippled의 tx 메소드, account\_tx 메소드, 또는 다른 응답을 사용하고, "validated": true를 조회하여 이 응답이 컨센서스에 의해 검증된 ledger 버전을 사용하는지 확인합니다.

* 결과에 "validated": true가 없다면, 결과는 잠정적일 수 있고 거래 결과가 최종적인 것인지 알기 위해 ledger가 검증될 때까지 기다려야 합니다.
* 결과가 해당 거래를 포함하고 있지 않거나 txnNotFound 오류를 반환하면, 거래는 서버가 가지고 있는 사용 가능한 기록의 ledger에 없습니다. 이는 거래가 실패했을 수도 있고, 거래가 서버가 가지고 있지 않은 검증된 ledger 버전에 있을 수 있고, 미래의 검증된 ledger에 포함될 수 있기 때문에 반드시 실패했다는 것을 의미하지는 않습니다. 거래가 있을 수 있는 ledger의 범위를 제한하기 위해서는 다음 사항을 알아야 합니다:
  * 거래가 있을 수 있는 가장 이른 ledger, 즉 거래가 처음 제출된 후 검증된 첫 번째 ledger입니다.
  * 거래가 있을 수 있는 마지막 ledger는 거래의 LastLedgerSequence 필드에 의해 정의됩니다.

아래 예는 검증된 ledger 버전에 있는 성공한 거래를 보여줍니다. 이는 tx 메서드에 의해 반환되며, 이해하기 쉽게 하기 위해 JSON 응답의 필드 순서가 재배치되고 일부 부분이 생략되었습니다.

```json
{
  "TransactionType": "AccountSet",
  "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
  "Sequence": 376,
  "hash": "017DED8F5E20F0335C6F56E3D5EE7EF5F7E83FB81D2904072E665EEA69402567",

  ... (omitted) ...

  "meta": {
    "AffectedNodes": [
      ... (omitted) ...
    ],
    "TransactionResult": "tesSUCCESS"
  },
  "ledger_index": 46447423,
  "validated": true
}
```

이 예시는 주소가 rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn인 계정에서 보낸 AccountSet 거래를 보여주며, 이는 시퀀스 번호 376을 사용합니다. 거래의 식별 해시는 017DED8F5E20F0335C6F56E3D5EE7EF5F7E83FB81D2904072E665EEA69402567이고 결과는 tesSUCCESS입니다. 이 거래는 46447423 버전의 ledger에 포함되었으며, 이 ledger는 검증되었으므로 이 결과는 최종적인 것입니다.

## 사례: 검증된 Ledger에 포함되지 않음(Case: Not Included in a Validated Ledger)

거래가 검증된 ledger에 포함되지 않으면 공유 XRP Ledger 상태에 어떠한 영향도 미칠 수 없습니다. 거래가 ledger에 포함되지 않은 것이 최종적이라면, 이 거래는 앞으로도 어떠한 영향도 미칠 수 없습니다.

거래의 실패가 최종적이지 않다면, 이 거래는 여전히 미래의 검증된 ledger에 포함될 수 있습니다. 현재 열린 ledger에 거래를 적용하는 잠정 결과를 사용하여 거래가 최종 ledger에서 미칠 가능성 있는 영향을 미리 볼 수 있지만, 그 결과는 많은 요인들로 인해 변할 수 있습니다.

## 사례: 검증된 Ledger에 포함됨(Case: Included in a Validated Ledger)

거래가 검증된 ledger에 포함되면, 거래 메타데이터는 거래 처리의 결과로 ledger 상태에 생긴 모든 변화에 대한 전체 보고서를 포함하고 있습니다. 메타데이터의 TransactionResult 필드에는 거래 결과를 요약한 코드가 들어 있습니다:

* 코드 tesSUCCESS는 거래가 대체로 성공했음을 나타냅니다.
* tec 클래스 코드는 거래가 실패했음을 나타내며, ledger 상태에 미친 유일한 영향은 XRP 거래 비용을 소멸시키고 만료된 오더북이나 닫힌 결제 채널을 제거하는 등의 회계 작업을 수행하는 것입니다.
* 다른 코드는 어떤 ledger에도 나타날 수 없습니다.

결과 코드는 거래 결과의 요약일 뿐입니다. 거래가 무엇을 수행했는지 더 자세히 이해하려면, 거래의 지시사항과 거래 실행 전의 ledger 상태에 대한 맥락 속에서 메타데이터의 나머지 부분을 읽어야 합니다.

## 2. 메타데이터 해석하기(Interpret Metadata)

거래 메타데이터는 거래가 ledger에 어떻게 적용되었는지를 정확하게 설명하며, 다음과 같은 필드를 포함합니다:

| Field                                                                              | 값        | 설명명                                                                                                                                                                                                                                                                                                            |
| ---------------------------------------------------------------------------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `AffectedNodes`                                                                    | 배열       | 이 트랜잭션에 의해 생성, 삭제 또는 수정된  [ledger objects](https://xrpl.org/ledger-object-types.html) 및 각 객체의 특정 변경 사항 목록입니다.                                                                                                                                                                                                  |
| `DeliveredAmount`                                                                  | 화폐 금액    | (생략 가능) 부분 결제의경우 실제 목적지까지 통화가 전달된 금액을 기록하는란입니다. 트랜잭션을 읽을 때 오류를 방지하려면 부분적이든 아니든 모든 지불 트랜잭션에 대해 제공되는 delivery\_amount 필드를 대신 사용하십시오.                                                                                                                                                                             |
| `TransactionIndex`                                                                 | 부호 없는 정수 | 트랜잭션을 포함하는 l   edger 내의 위치입니다. 이것은 인덱스가 0입니다. (예를 들어 값 2는 해당 원장에서 세 번째 트랜잭션임을 의미합니다.)                                                                                                                                                                                                                          |
| `TransactionResult`                                                                | 문자열      | 트랜잭션의 성공 여부 또는 실패 방법을 나타내는 결과 코드입니다.                                                                                                                                                                                                                                                                           |
| [`delivered_amount`](https://xrpl.org/transaction-metadata.html#delivered\_amount) | 화폐 금액    | <p><em>(비지급 거래의 경우 생략)</em> 대상 계정에서 실제로 받은 통화 금액입니다. 이 필드를 사용하여 거래가 부분 결제인지 여부에 관계없이 배송된 금액을 확인합니다. 자세한 내용은 이 설명을 참조하십시오.<br><a href="https://github.com/ripple/rippled/releases/tag/0.27.0"><img src="https://img.shields.io/badge/New%20in-rippled%200.27.0-blue.svg" alt="New in: rippled 0.27.0"> </a></p> |

대부분의 메타데이터는 AffectedNodes 배열에 포함되어 있습니다. 이 배열에서 찾아야 할 것은 거래의 유형에 따라 다릅니다. 거의 모든 거래는 보내는 사람의 AccountRoot 객체를 수정하여 XRP 거래 비용을 소멸시키고 계정의 시퀀스 번호를 증가시킵니다.

**Info:** 규칙의 예외는 의사-거래(pseudo-transactions)로, 실제 계정에서 보내지 않으므로 AccountRoot 객체를 수정하지 않습니다. 계정의 Balance 필드를 변경하지 않고 AccountRoot 객체를 수정하는 다른 예외도 있습니다: 무료 키 재설정 거래는 보내는 사람의 XRP 잔액을 변경하지 않습니다. 또한 거래로 인해 계정이 파괴한 만큼의 XRP를 정확히 받게 되는 경우, 계정의 Balance는 변화를 보이지 않습니다. (XRP의 순 감소는 메타데이터의 다른 부분에서 발생하며, 계정이 XRP를 보낸 곳에서 차감됩니다.)

이 예는 위의 1단계에서의 전체 응답을 보여줍니다. 이것이 ledger에 어떤 변화를 만들었는지 확인해 보세요:

```json
{
  "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
  "Fee": "12",
  "Flags": 2147483648,
  "LastLedgerSequence": 46447424,
  "Sequence": 376,
  "SigningPubKey": "03AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB",
  "TransactionType": "AccountSet",
  "TxnSignature": "30450221009B2910D34527F4EA1A02C375D5C38CF768386ACDE0D17CDB04C564EC819D6A2C022064F419272003AA151BB32424F42FC3DBE060C8835031A4B79B69B0275247D5F4",
  "date": 608257201,
  "hash": "017DED8F5E20F0335C6F56E3D5EE7EF5F7E83FB81D2904072E665EEA69402567",
  "inLedger": 46447423,
  "ledger_index": 46447423,
  "meta": {
    "AffectedNodes": [
      {
        "ModifiedNode": {
          "LedgerEntryType": "AccountRoot",
          "LedgerIndex": "13F1A95D7AAB7108D5CE7EEAF504B2894B8C674E6D68499076441C4837282BF8",
          "FinalFields": {
            "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
            "AccountTxnID": "017DED8F5E20F0335C6F56E3D5EE7EF5F7E83FB81D2904072E665EEA69402567",
            "Balance": "396015164",
            "Domain": "6D64756F31332E636F6D",
            "EmailHash": "98B4375E1D753E5B91627516F6D70977",
            "Flags": 8519680,
            "MessageKey": "0000000000000000000000070000000300",
            "OwnerCount": 9,
            "Sequence": 377,
            "TransferRate": 4294967295
          },
          "PreviousFields": {
            "AccountTxnID": "E710CADE7FE9C26C51E8630138322D80926BE91E46D69BF2F36E6E4598D6D0CF",
            "Balance": "396015176",
            "Sequence": 376
          },
          "PreviousTxnID": "E710CADE7FE9C26C51E8630138322D80926BE91E46D69BF2F36E6E4598D6D0CF",
          "PreviousTxnLgrSeq": 46447387
        }
      }
    ],
    "TransactionIndex": 13,
    "TransactionResult": "tesSUCCESS"
  },
  "validated": true
}
```

[no-op](https://xrpl.org/cancel-or-skip-a-transaction.html) 트랜잭션으로 인해 발생한 유일한 변경사항은, 보내는 사람의 계정을 표시하는 AccountRoot 객체를 다음과 같이 업데이트하는 것입니다:

* 시퀀스 값이 376에서 377로 증가합니다.
* 이 계정의 XRP 잔액이 396015176에서 396015164 드롭으로 변경됩니다. 이 정확히 12 드롭의 감소는 거래 비용을 나타내며, 이는 거래의 Fee 필드에 명시되어 있습니다.
* AccountTxnID가 변경되어 이 트랜잭션이 이 주소에서 가장 최근에 보낸 것임을 나타냅니다.
* 이 계정에 영향을 미친 이전 트랜잭션은 PreviousTxnID 및 PreviousTxnLgrSeq 필드에 명시된 대로, 46447387 렛저 버전에서 실행된 E710CADE7FE9C26C51E8630138322D80926BE91E46D69BF2F36E6E4598D6D0CF 트랜잭션이었습니다. (계정의 거래 기록을 거꾸로 따라가고 싶은 경우 유용할 수 있습니다.)

{% hint style="info" %}
Note:

메타데이터가 명시적으로 보여주지는 않지만, 트랜잭션이 ledger 객체를 수정할 때마다, 해당 객체의 PreviousTxnID 및 PreviousTxnLgrSeq 필드를 현재 트랜잭션의 정보로 업데이트합니다. 같은 보내는 사람이 단일 ledger 버전에서 여러 트랜잭션을 가지고 있는 경우, 첫 번째 이후의 각 트랜잭션은 그 트랜잭션을 모두 포함한 ledger 버전의 ledger 인덱스 값을 가진 PreviousTxnLgrSeq를 제공합니다.
{% endhint %}

rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn의 계정에 대한 ModifiedNode 항목이 AffectedNodes 배열의 유일한 객체이므로, 이 트랜잭션으로 인해 ledger에 다른 변화는 없었습니다.

{% hint style="info" %}
Tip:

거래가 XRP를 보내거나 받는 경우, 보내는 사람의 잔액 변동은 거래 비용과 결합되어, 순 금액의 Balance 필드에 단일 변동이 발생합니다. 예를 들어, 1 XRP (1,000,000 드롭)를 보내고 거래 비용으로 10 드롭을 소멸시킨 경우, 메타데이터는 당신의 Balance가 1,000,010 드롭의 XRP로 감소하는 것을 보여줍니다.
{% endhint %}

## 일반 목적의 회계(General-Purpose Bookkeeping)

거의 모든 트랜잭션은 다음 유형의 변경을 초래할 수 있습니다:

* **시퀀스 및 트랜잭션 비용 변경**: 앞서 언급했듯이, 모든 트랜잭션 (의사-트랜잭션 제외)은 보내는 사람의 AccountRoot 객체를 수정하여 보내는 사람의 시퀀스 번호를 증가시키고, 거래 비용으로 사용된 XRP를 소멸시킵니다.&#x20;
* **계정 쓰레딩**: 객체를 생성하는 일부 트랜잭션은 관련 계정이 변경되었음을 나타내기 위해 예정된 수신자 또는 대상 계정의 AccountRoot 객체를 수정합니다. 이 "태깅" 기법은 그 객체의 PreviousTxnID 및 PreviousTxnLgrSeq 필드만 변경합니다. 이는 이러한 필드에서 언급한 트랜잭션의 "스레드"를 따라 계정의 거래 이력을 찾는 것이 더 효율적이게 합니다.&#x20;
* **디렉터리 업데이트**: 객체를 생성하거나 제거하는 트랜잭션은 종종 DirectoryNode 객체를 변경하여 어떤 객체가 존재하는지 추적합니다. 또한, 트랜잭션이 소유자 reserve에 포함되는 객체를 추가하면, 소유자의 AccountRoot 객체의 OwnerCount를 증가시킵니다. 객체를 제거하면 OwnerCount가 감소합니다. 이는 XRP Ledger가 어느 시점에서 각 계정이 얼마의 소유자 예약을 지불해야 하는지를 추적하는 방법입니다.

계정의 OwnerCount를 증가시키는 예시:

```json
{
  "ModifiedNode": {
    "FinalFields": {
      "Account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
      "Balance": "9999999990",
      "Flags": 0,
      "OwnerCount": 1,
      "Sequence": 2
    },
    "LedgerEntryType": "AccountRoot",
    "LedgerIndex": "4F83A2CF7E70F77F79A307E6A472BFC2585B806A70833CCD1C26105BAE0D6E05",
    "PreviousFields": {
      "Balance": "10000000000",
      "OwnerCount": 0,
      "Sequence": 1
    },
    "PreviousTxnID": "B24159F8552C355D35E43623F0E5AD965ADBF034D482421529E2703904E1EC09",
    "PreviousTxnLgrSeq": 16154
  }
}on
```

많은 트랜잭션 유형들이 DirectoryNode 객체를 생성하거나 수정합니다. 이러한 객체들은 회계를 위한 것입니다: 계정에 연결된 모든 객체나 동일한 환율에서의 모든 교환 제안들을 추적합니다. 트랜잭션이 ledger에 새로운 객체를 생성했다면, 기존의 DirectoryNode 객체에 항목을 추가하거나, 디렉터리의 다른 페이지를 나타내기 위해 다른 DirectoryNode 객체를 추가해야 할 수 있습니다. 트랜잭션이 ledger에서 객체를 제거했다면, 더 이상 필요하지 않은 하나 이상의 DirectoryNode 객체를 삭제할 수 있습니다.

새로운 제안 디렉터리를 나타내는 CreatedNode 예시:

```json
{
  "CreatedNode": {
    "LedgerEntryType": "DirectoryNode",
    "LedgerIndex": "F60ADF645E78B69857D2E4AEC8B7742FEABC8431BD8611D099B428C3E816DF93",
    "NewFields": {
      "ExchangeRate": "4E11C37937E08000",
      "RootIndex": "F60ADF645E78B69857D2E4AEC8B7742FEABC8431BD8611D099B428C3E816DF93",
      "TakerPaysCurrency": "0000000000000000000000004254430000000000",
      "TakerPaysIssuer": "5E7B112523F68D2F5E879DB4EAC51C6698A69304"
    }
  }
},
```

트랜잭션 메타데이터를 처리할 때 확인해야 할 다른 사항은 트랜잭션 유형에 따라 다릅니다.

## 결제&#x20;

Payment 트랜잭션은 직접적인 XRP 대 XRP 거래, 교차 화폐 결제, 또는 대체 가능한 토큰의 직접 거래를 나타낼 수 있습니다. 직접적인 XRP 대 XRP 거래를 제외한 어떤 것도 부분 결제가 될 수 있으며, 이는 토큰 대 XRP 또는 XRP 대 토큰 거래를 포함합니다.

XRP 금액은 AccountRoot 객체의 Balance 필드에서 추적됩니다. (XRP는 Escrow 객체와 PayChannel 객체에도 존재할 수 있지만, Payment 트랜잭션은 이러한 것들에 영향을 줄 수 없습니다.)

결제가 얼마나 전달했는지 알아보기 위해 항상 delivered\_amount 필드를 사용해야 합니다.

결제에 "LedgerEntryType": "AccountRoot"을 가진 CreatedNode가 포함되어 있다면, 이는 결제가 ledger에 새 계정을 자금화했다는 것을 의미합니다.

## 토큰 결제&#x20;

토큰을 포함하는 결제는 약간 더 복잡합니다.

모든 토큰 잔액의 변동은 RippleState 객체, 즉 신뢰선을 반영합니다. 신뢰선에서 한 측의 잔액이 증가하면 이는 상대방의 잔액이 같은 금액으로 감소한 것으로 간주됩니다; 메타데이터에서는 이것이 RippleState 객체의 공유 Balance에 대한 단일 변동으로만 기록됩니다. 이 변동이 "증가"로 또는 "감소"로 기록되는지는 어느 계정이 수치상 더 높은 주소를 가지고 있는지에 따라 다릅니다.

단일 결제는 여러 신뢰선과 오더북을통해 간접적으로 연결된 파티 간에 신뢰선의 잔액을 변경하는 프로세스인 rippling을 통해 긴 경로를 가질 수 있습니다. 트랜잭션의 Amount 필드에 명시된 발행자에 따라, 전달될 금액이 목적지 계정에 연결된 여러 신뢰선 (RippleState 계정) 사이에 분할될 가능성도 있습니다.

{% hint style="info" %}
Tip:

수정된 객체가 메타데이터에 표시되는 순서는 반드시 결제 처리 시 해당 객체들이 방문된 순서와 일치하지는 않습니다. 결제 실행을 더 잘 이해하기 위해, 자금이  ledger를 통해 어떤 경로를 따라 이동했는지를 재구성하기 위해 AffectedNodes 멤버를 재정렬하는 것이 도움이 될 수 있습니다.
{% endhint %}

교차 화폐 결제는 다른 화폐 코드와 발행자 간에 변경하기 위해 제안을 부분적으로 또는 완전히 소비합니다. 트랜잭션이 Offer 유형에 대한 DeletedNode 객체를 보여주는 경우, 이는 완전히 소비된 제안, 또는 처리 시점에 만료되거나 미자금화된 것으로 판명된 제안을 나타낼 수 있습니다. 트랜잭션이 Offer 유형의 ModifiedNode를 보여주는 경우, 이는 부분적으로 소비된 제안을 나타냅니다.

신뢰선의 QualityIn 및 QualityOut 설정은 신뢰선의 한 측이 토큰을 어떻게 평가하는지에 영향을 줄 수 있어, 잔액의 숫자 변경이 발신자가 해당 토큰을 평가하는 방식과 다를 수 있습니다. delivered\_amount는 수신자가 평가한 대로 얼마나 전달되었는지 보여줍니다.

전송 또는 수신할 금액이 토큰 정밀도의 범위를 벗어날 경우, 트랜잭션의 다른 쪽에서는 둥근형으로 인해 차감된 금액이 없는 것으로 나타날 수 있습니다. 따라서 두 파티가 그들의 잔액이 10^16으로 다르게 거래할 때, 둥근형이 작은 토큰의 금액을 실질적으로 "창출"하거나 "파괴"할 수 있습니다. (XRP는 절대로 반올림되지 않으므로, 이는 XRP에서는 불가능합니다.)

경로의 길이에 따라, 교차 화폐 결제에 대한 메타데이터는 길어질 수 있습니다. 예를 들어, 트랜잭션 8C55AFC2A2AA42B5CE624AEECDB3ACFDD1E5379D4E5BF74A8460C5E97EF8706B는 XRP를 지출하지만 2명의 발행자로부터 USD를 통과하고, 2개의 계정에 XRP를 지불하며, r9ZoLsJ...로부터 EUR를 ETH로 거래하는 미자금화된 제안을 제거하고, 총 17개의 다른 ledger 객체가 수정된 책장 작업을 위한 것으로 2.788 GCB를 rHaaans...에게 발행하였습니다.

## 제안&#x20;

OfferCreate 트랜잭션은 얼마나 많이 매칭되었는지와 tfImmediateOrCancel과 같은 플래그가 트랜잭션에서 사용되었는지에 따라 ledger에 객체를 생성하거나 생성하지 않을 수 있습니다. "LedgerEntryType": "Offer"를 가진 CreatedNode 항목을 찾아 트랜잭션이 ledger의 오더북에 새로운 제안을 추가했는지 확인하세요. 예를 들면:

```json
{
  "CreatedNode": {
    "LedgerEntryType": "Offer",
    "LedgerIndex": "F39B13FA15AD2A345A9613934AB3B5D94828D6457CCBB51E3135B6C44AE4BC83",
    "NewFields": {
      "Account": "rETSmijMPXT9fnDbLADZnecxgkoJJ6iKUA",
      "BookDirectory": "CA462483C85A90DB76D8903681442394D8A5E2D0FFAC259C5B0C59269BFDDB2E",
      "Expiration": 608427156,
      "Sequence": 1082535,
      "TakerGets": {
        "currency": "EUR",
        "issuer": "rhub8VRN55s94qWKDv6jmDy1pUykJzF3wq",
        "value": "2157.825"
      },
      "TakerPays": "7500000000"
    }
  }
}
```

ModifiedNode of type Offer은 부분적으로 소비된 제안을 나타냅니다. 하나의 거래는 여러 제안을 소비할 수 있습니다. 두 토큰을 거래하는 제안은 auto-bridging 때문에 XRP를 거래하는 제안도 소비할 수 있습니다. 거래의 전부 또는 일부가 자동으로 auto-bridging을 만들 수 있습니다.

DeletedNode of type Offer는 완전히 소비된 매칭 제안, 처리 시점에 만료되거나 미지급으로 판명된 제안, 또는 새로운 제안을 제시하는 과정에서 취소된 제안을 나타낼 수 있습니다. 취소된 제안을 알아볼 수 있는 방법은 그것을 제시한 계정이 그것을 삭제한 트랜잭션의 송신자라는 점입니다.

삭제된 제안의 예시:

```javascript
{
  "DeletedNode": {
    "FinalFields": {
      "Account": "rETSmijMPXT9fnDbLADZnecxgkoJJ6iKUA",
      "BookDirectory": "CA462483C85A90DB76D8903681442394D8A5E2D0FFAC259C5B0C595EDE3E1EE9",
      "BookNode": "0000000000000000",
      "Expiration": 608427144,
      "Flags": 0,
      "OwnerNode": "0000000000000000",
      "PreviousTxnID": "0CA50181C1C2A4D45E9745F69B33FA0D34E60D4636562B9D9CDA1D4E2EFD1823",
      "PreviousTxnLgrSeq": 46493676,
      "Sequence": 1082533,
      "TakerGets": {
        "currency": "EUR",
        "issuer": "rhub8VRN55s94qWKDv6jmDy1pUykJzF3wq",
        "value": "2157.675"
      },
      "TakerPays": "7500000000"
    },
    "LedgerEntryType": "Offer",
    "LedgerIndex": "9DC99BF87F22FB957C86EE6D48407201C87FBE623B2F1BC4B950F83752B55E27"
  }
}
```

제안은 어느 계정이 어느 제안을 제시했는지, 어느 환율에서 어느 제안이 가능한지를 추적하기 위해 두 종류의 DirectoryNode 객체를 만들고, 삭제하며, 수정할 수 있습니다. 일반적으로 사용자들은 이러한 분장을 주의 깊게 살피지 않아도 됩니다.

OfferCancel 트랜잭션은 삭제할 제안이 없더라도 tesSUCCESS 코드를 가질 수 있습니다. 트랜잭션이 실제로 제안을 삭제했는지 확인하기 위해 제안 타입의 삭제된 노드(DeletedNode of type Offer)를 찾아보세요. 그렇지 않으면, 제안은 이미 이전 트랜잭션에 의해 삭제되었거나, OfferCancel 트랜잭션이 OfferSequence 필드에서 잘못된 순서 번호를 사용했을 수 있습니다.

OfferCreate 트랜잭션이 RippleState 타입의 CreatedNode를 보여주면, 그 제안이 거래에서 받은 토큰을 보유하기 위해 신뢰선을 생성했다는 것을 나타냅니다.

## 에스크로

성공적인 EscrowCreate 트랜잭션은 XRP Ledger에 Escrow 객체를 생성합니다. Escrow 타입의 CreatedNode 항목을 찾아보세요. NewFields는 에스크로된 XRP 금액과 동등한 Amount를 보여야 하며, 다른 속성들도 지정되어야 합니다.

성공적인 EscrowCreate 트랜잭션은 또한 보내는 사람으로부터 동일한 양의 XRP를 차감합니다. 최종 필드의 Account에서 트랜잭션 지시 사항의 Account와 일치하는 주소를 찾는 AccountRoot 타입의 ModifiedNode를 찾아보세요. Balance는 에스크로된 XRP (및 트랜잭션 비용을 지불하기 위해 파괴된 XRP)로 인한 XRP 감소를 보여야 합니다.

성공적인 EscrowFinish 트랜잭션은 수신자의 AccountRoot를 수정하여 그들의 XRP 잔액을 증가시키며(Balance 필드에서), Escrow 객체를 삭제하고, 에스크로 생성자의 소유자 수를 줄입니다. 에스크로의 생성자, 수신자, 완료자는 모두 다른 계정일 수도 있고 같은 계정일 수도 있으므로, 이는 AccountRoot 타입의 하나에서 세 개의 ModifiedNode 객체를 만들 수 있습니다. 성공적인 EscrowCancel 트랜잭션은 매우 유사하지만, XRP를 원래의 에스크로 생성자에게 돌려보냅니다.

당연히, EscrowFinish는 에스크로의 조건을 충족하는 경우에만 성공할 수 있고, EscrowCancel은 에스크로 객체의 만료가 이전 ledger의 마감 시간 이전인 경우에만 성공할 수 있습니다.

에스크로 트랜잭션은 또한 송신자의 소유자 reserve를 조정하고 관련 계정의 디렉터리를 정상적으로 조정하는 분장을 합니다.

다음 발췌문에서, r9UUEX...의 잔액이 10억 XRP 증가하고 소유자 수가 1감소하는 것을 볼 수 있습니다. 왜냐하면 그 계정에서 자신에게 보낸 에스크로가 성공적으로 완료되었기 때문입니다. 세 번째 당사자가 에스크로를 완료했으므로 Sequence 번호는 변경되지 않습니다:

```json
{
  "ModifiedNode": {
    "FinalFields": {
      "Account": "r9UUEXn3cx2seufBkDa8F86usfjWM6HiYp",
      "Balance": "1650000199898000",
      "Flags": 1048576,
      "OwnerCount": 11,
      "Sequence": 23
    },
    "LedgerEntryType": "AccountRoot",
    "LedgerIndex": "13FDBC39E87D9B02F50940F9FDDDBFF825050B05BE7BE09C98FB05E49DD53FCA",
    "PreviousFields": {
      "Balance": "650000199898000",
      "OwnerCount": 12
    },
    "PreviousTxnID": "D853342BC27D8F548CE4D7CB688A8FECE3229177790453BA80BC79DE9AAC3316",
    "PreviousTxnLgrSeq": 41005507
  }
},
{
  "DeletedNode": {
    "FinalFields": {
      "Account": "r9UUEXn3cx2seufBkDa8F86usfjWM6HiYp",
      "Amount": "1000000000000000",
      "Destination": "r9UUEXn3cx2seufBkDa8F86usfjWM6HiYp",
      "FinishAfter": 589075200,
      "Flags": 0,
      "OwnerNode": "0000000000000000",
      "PreviousTxnID": "D5FB1C7D18F931A4FBFA468606220560C17ADF6DE230DA549F4BD11A81F19DFC",
      "PreviousTxnLgrSeq": 35059548
    },
    "LedgerEntryType": "Escrow",
    "LedgerIndex": "62F0ABB58C874A443F01CDCCA18B12E6DA69C254D3FB17A8B71CD8C6C68DB74D"
  }
},
```

## 결제 채널(Payment Channels)&#x20;

결제 채널을 생성할 때 PayChannel 타입의 CreatedNode를 찾아보세요. 또한, 보내는 사람의 잔액 감소를 보여주는 AccountRoot 타입의 ModifiedNode를 찾을 수 있어야 합니다. FinalFields의 Account 필드에서 주소가 보내는 사람과 일치하는지 확인하고, Balance 필드의 차이를 보아 XRP 잔액의 변화를 확인하세요.

메타데이터는 또한 목적지의 소유자 디렉터리에 새로 생성된 결제 채널을 나열합니다. 이는 계정이 열린 결제채널의 목적지일 경우 삭제되지 않도록 방지합니다. (이 행동은 fixPayChanRecipientOwnerDir 수정으로 추가되었습니다.)

채널의 불변의 CancelAfter 시간(생성 시에만 설정됨) 외에도 채널을 닫을 요청을 하는 몇 가지 방법이 있습니다. 트랜잭션이 채널을 닫기 위해 예정되면, 채널에 대한 PayChannel 타입의 ModifiedNode 항목이 있고, FinalFields의 Expiration 필드에 새로 추가된 종료 시간이 있습니다. 다음 예는 송신자가 청구를 상환하지 않고 채널을 닫을 요청한 경우의 PayChannel 변화를 보여줍니다:

```javascript
{
  "ModifiedNode": {
    "FinalFields": {
      "Account": "rNn78XpaTXpgLPGNcLwAmrcS8FifRWMWB6",
      "Amount": "1000000",
      "Balance": "0",
      "Destination": "rwWfYsWiKRhYSkLtm3Aad48MMqotjPkU1F",
      "Expiration": 608432060,
      "Flags": 0,
      "OwnerNode": "0000000000000002",
      "PublicKey": "EDEACA57575C6824FC844B1DB4BF4AF2B01F3602F6A9AD9CFB8A3E47E2FD23683B",
      "SettleDelay": 3600,
      "SourceTag": 1613739140
    },
    "LedgerEntryType": "PayChannel",
    "LedgerIndex": "DC99821FAF6345A4A6C41D5BEE402A7EA9198550F08D59512A69BFC069DC9778",
    "PreviousFields": {},
    "PreviousTxnID": "A9D6469F3CB233795B330CC8A73D08C44B4723EFEE11426FEE8E7CECC611E18E",
    "PreviousTxnLgrSeq": 41889092
  }
}
```

## TrustSet 트랜잭션&#x20;

TrustSet 트랜잭션은 RippleState 객체로 표현되는 신뢰선을 생성하고, 수정하거나, 삭제합니다. 하나의 RippleState 객체는 참여한 양쪽 당사자의 설정을 포함하며, 그들의 한도, 리플링 설정 등을 포함합니다. 신뢰선을 생성하고 수정하는 것은 또한 송신자의 소유자 reserve과 소유자 디렉터리를 조정할 수 있습니다.

다음 예는 새로운 신뢰선을 보여줍니다. 여기서 rf1BiG...는 rsA2Lp...가 발행한 최대 110달러를 보유할 의향이 있습니다.

```json
 {
  "CreatedNode": {
    "LedgerEntryType": "RippleState",
    "LedgerIndex": "9CA88CDEDFF9252B3DE183CE35B038F57282BC9503CDFA1923EF9A95DF0D6F7B",
    "NewFields": {
      "Balance": {
        "currency": "USD",
        "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
        "value": "0"
      },
      "Flags": 131072,
      "HighLimit": {
        "currency": "USD",
        "issuer": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
        "value": "110"
      },
      "LowLimit": {
        "currency": "USD",
        "issuer": "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW",
        "value": "0"
      }
    }
  }
}
```

## 다른 트랜잭션들(Other Transactions)

대부분의 다른 트랜잭션들은 특정한 유형의 ledger 항목을 생성하고, 발신자의 소유자 reserve와 소유자 디렉터리를 조정합니다:

* AccountSet 트랜잭션은 발신자의 기존 AccountRoot 객체를 수정하여 지정된 설정과 플래그를 변경합니다.&#x20;
* DepositPreauth 트랜잭션은 특정 발신자에 대한 DepositPreauth 객체를 추가하거나 제거합니다.&#x20;
* SetRegularKey 트랜잭션은 발신자의 AccountRoot 객체를 수정하여 지정된 RegularKey 필드를 변경합니다.&#x20;
* SignerListSet 트랜잭션은 SignerList 객체를 추가하거나 제거하거나 교체합니다.

## Pseudo-Transactions

Pseudo-Transactions도 메타데이터를 가지고 있지만, 일반적인 트랜잭션의 모든 규칙을 따르지는 않습니다. 그들은 실제 계정에 연결되지 않아서(계정 값은 숫자 0의 base58-encoded 형태입니다), 순서 번호를 증가시키거나 XRP를 파괴하기 위해 ledger의 AccountRoot 객체를 수정하지 않습니다. 사Pseudo-Transactions은 특별한 ledger 객체에 대한 특정 변경사항만 만들어냅니다:

* EnableAmendment pseudo-transactions은 Amendments ledger 객체를 수정하여 어떤 수정이 활성화되었는지, 어떤 것들이 다수의 지원과 함께 얼마나 오랫동안 보류 중인지를 추적합니다.&#x20;
* SetFee pseudo-transactions은 FeeSettings ledger 객체를 수정하여 트랜잭션 비용과 [reserve requirements](https://xrpl.org/reserves.html)에 대한 기본 수준을 변경합니다.

### 참고 <a href="#see-also" id="see-also"></a>

* **Concepts:**
  * [Finality of Results](https://xrpl.org/finality-of-results.html)
  * [Reliable Transaction Submission](https://xrpl.org/reliable-transaction-submission.html)
* **Tutorials:**
  * [Monitor Incoming Payments with WebSocket](https://xrpl.org/monitor-incoming-payments-with-websocket.html)
* **References:**
  * [Ledger Entry Types Reference](https://xrpl.org/ledger-object-types.html) - All possible fields of all types of ledger entries
  * [Transaction Metadata](https://xrpl.org/transaction-metadata.html) - Summary of the metadata format and fields that appear in metadata
  * [Transaction Results](https://xrpl.org/transaction-results.html) - Tables of all possible result codes for transactions.
