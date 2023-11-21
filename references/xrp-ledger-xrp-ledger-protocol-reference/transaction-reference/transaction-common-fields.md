# 트랜잭션 공통 필드

금금액모든 트랜잭션에는 동일한 공통 필드 세트와 트랜잭션 유형에 따른 추가 필드가 있습니다. 필드 이름은 대소문자를 구분합니다. 모든 트랜잭션의 공통 필드는 다음과 같습니다:

| 필드                                                                           | JSON 유형 | 내부 유형  | 설명                                                                                                                                                        |
| ---------------------------------------------------------------------------- | ------- | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Account                                                                      | 문자열     | 계정 ID  | (필수) 트랜잭션을 시작한 계정 의 고유 주소입니다.                                                                                                                             |
| TransactionType                                                              | 문자열     | UInt16 | (필수) 트랜잭션 유형입니다. 유효한 트랜잭션 유형은 다음과 같습니다: Payment, OfferCreate, TrustSet 등이 있습니다.                                                                           |
| Fee                                                                          | 문자열     | 금액     | (필수, 자동 입력) 이 트랜잭션을 네트워크에 배포하는 데 드는 비용으로 소멸할 XRP의 정숫값(드롭 단위)입니다. 일부 트랜잭션 유형에는 최소 요구 사항이 다릅니다. 자세한 내용은 트랜잭션 비용을 참조하세요.                                     |
| Sequence                                                                     | 숫자      | UInt32 | (필수, 자동 입력) 트랜잭션을 전송하는 계정의 시퀀스 번호입니다. 시퀀스 번호가 같은 계정에서 보낸 이전 트랜잭션보다 정확히 1이 큰 경우에만 트랜잭션이 유효합니다. 특수 케이스 0은 트랜잭션이 티켓을 대신 사용한다는 의미입니다(티켓 배치수정안에 의해 추가되었습니다.)   |
| AccountTxnID                                                                 | 문자열     | 해시256  | (선택 사항) 다른 트랜잭션을 식별하는 해시 값입니다. 제공된 경우 이 거래는 보내는 계정의 이전에 보낸 거래가 제공된 해시와 일치하는 경우에만 유효합니다.                                                                   |
| Flags                                                                        | 숫자      | UInt32 | (선택 사항) 이 트랜잭션에 대한 비트 플래그 집합입니다.                                                                                                                          |
| LastLedgerSequence                                                           | 숫자      | UInt32 | (선택 사항, 적극 권장) 이 트랜잭션이 표시될 수 있는 가장 높은 ledger 인덱스입니다. 이 필드를 지정하면 트랜잭션이 유효성 검사 또는 거부될 때까지 대기할 수 있는 시간에 대한 엄격한 상한선이 지정됩니다. 자세한 내용은 신뢰할 수 있는 트랜잭션 제출을 참조하십시오. |
| [Memos](https://xrpl.org/transaction-common-fields.html#memos-field)         | 객체 배열   | 배열     | (선택 사항) 이 트랜잭션을 식별하는 데 사용되는 추가 임의 정보입니다.                                                                                                                  |
| [NetworkID](https://xrpl.org/transaction-common-fields.html#networkid-field) | 숫자      | UInt32 | (네트워크별) 이 거래가 의도된 체인의 네트워크 ID입니다. 메인넷 및 일부 테스트 네트워크의 경우 반드시 생략해야 합니다. 네트워크 ID가 1025 이상인 체인에서는 필수입니다.                                                      |
| [Signers](https://xrpl.org/transaction-common-fields.html#signers-field)     | 배열      | 배열     | (선택 사항) 이 트랜잭션을 승인하는 다중 서명을 나타내는 개체 배열입니다.                                                                                                                |
| SourceTag                                                                    | 숫자      | UInt32 | (선택 사항) 이 결제의 이유 또는 이 거래를 대신하여 결제한 발신자를 식별하는 데 사용되는 임의의 정수입니다. 일반적으로 환불은 최초 결제의 소스 태그를 환불 결제의 데스티네이션 태그로 지정해야 합니다.                                        |
| SigningPubKey                                                                | 문자열     | blob   | (서명할 때 자동으로 추가됨) 이 트랜잭션에 서명하는 데 사용된 개인 키에 해당하는 공개 키의 16진수 표현입니다. 빈 문자열이면 서명자 필드에 대신 다중 서명이 있음을 나타냅니다.                                                     |
| TicketSequence                                                               | 숫자      | UInt32 | (선택 사항) 시퀀스 번호 대신 사용할 티켓의 시퀀스 번호입니다. 이 값을 제공하는 경우 시퀀스는 0이어야 합니다. AccountTxnID와 함께 사용할 수 없습니다.                                                             |
| TxnSignature                                                                 | 문자열     | blob   | (서명 시 자동으로 추가됨) 이 거래가 발신자 계정에서 발생한 거래임을 확인하는 서명입니다.                                                                                                       |

[![Removed in: rippled 0.28.0](https://img.shields.io/badge/Removed%20in-rippled%200.28.0-red.svg)](https://github.com/ripple/rippled/releases/tag/0.28.0): 트랜잭션의 PreviousTxnID 필드가 AccountTxnID 필드로 대체되었습니다. 이 문자열/해시256 필드는 일부 과거 트랜잭션에 존재합니다. 이는 일부 ledger 객체에서 PreviousTxnID라고도 하는 필드와 관련이 없습니다.

## AccountTxnID

AccountTxnID 필드를 사용하면 트랜잭션을 서로 연결하여 동일한 계정에서 보낸 이전 트랜잭션에 특정 트랜잭션 해시가 없으면 현재 트랜잭션이 유효하지 않도록 할 수 있습니다.

보낸 사람에 관계없이 계정을 수정안하기 위해 마지막으로 보낸 트랜잭션을 추적하는 PreviousTxnID 필드와 달리, AccountTxnID는 계정에서 보낸 마지막 트랜잭션을 추적합니다. AccountTxnID를 사용하려면 먼저 ledger이 계정의 이전 트랜잭션에 대한 ID를 추적할 수 있도록 asfAccountTxnID 플래그를 활성화해야 합니다. (이에 비해 PreviousTxnID는 항상 추적됩니다.)

이 기능이 유용한 상황은 트랜잭션 제출을 위한 기본 시스템과 수동 백업 시스템이 있는 경우입니다. 수동 백업 시스템이 기본 시스템과 연결이 끊어졌지만 기본 시스템이 완전히 죽지 않은 상태에서 두 시스템이 동시에 작동하기 시작하면 일부 트랜잭션은 두 번 전송되고 다른 트랜잭션은 전혀 전송되지 않는 등 심각한 문제가 발생할 수 있습니다. 트랜잭션을 AccountTxnID와 함께 연결하면 두 시스템이 모두 활성화되어 있어도 한 번에 한 시스템만 유효한 트랜잭션을 제출할 수 있습니다.

티켓을 사용하는 트랜잭션에는 AccountTxnID 필드를 사용할 수 없습니다. AccountTxnID를 사용하는 트랜잭션은 트랜잭션 대기열에 배치할 수 없습니다.

## 자동 입력 가능 필드

일부 필드는 rippled 서버나 클라이언트 라이브러리에서 트랜잭션이 서명되기 전에 자동으로 채워질 수 있습니다. 값을 자동 채우려면 최신 상태를 가져오기 위해 XRP Ledger에 대한 활성 연결이 필요하므로 오프라인에서는 수행할 수 없습니다. 라이브러리에 따라 세부 사항은 다를 수 있지만, 자동 채우기는 최소한 다음 필드에 대해 항상 적절한 값을 제공합니다:

* 수수료 - 네트워크에 따라 트랜잭션 비용을 자동으로 입력합니다.

{% hint style="info" %}
Note:

rippled의 서명 명령을 사용할 때 수수료\_mult\_max 및 수수료\_mult\_div 매개변수를 사용하여 자동 입력 가능한 최대 값을 제한할 수 있습니다.)
{% endhint %}

* 시퀀스 - 트랜잭션을 전송하는 계정의 다음 시퀀스 번호를 자동으로 사용합니다.

프로덕션 시스템의 경우 이 필드를 서버가 채우도록 두지 않는 것이 좋습니다. 예를 들어 네트워크 부하가 일시적으로 급증하여 트랜잭션 비용이 높아지는 경우 일시적으로 높은 비용을 지불하는 대신 비용이 낮아질 때까지 기다렸다가 일부 트랜잭션을 전송하는 것이 좋습니다.

결제 트랜잭션 유형의 경로 필드도 자동으로 입력할 수 있습니다.

## 플래그 필드

플래그 필드에는 트랜잭션의 작동 방식에 영향을 주는 다양한 옵션이 포함될 수 있습니다. 옵션은 비트 단위로 결합할 수 있는 이진 값으로 표시되거나 한 번에 여러 플래그를 설정하는 연산으로 표시됩니다.

트랜잭션에 지정된 플래그가 활성화되어 있는지 확인하려면 플래그 값과 플래그 필드에 비트 연산자를 사용합니다. 결과가 0이면 플래그가 비활성화되어 있고 플래그 값과 같으면 플래그가 활성화되어 있음을 나타냅니다. (다른 결과가 나오면 뭔가 잘못한 것입니다.)

대부분의 플래그는 특정 트랜잭션 유형에 대해서만 의미가 있습니다. 다른 트랜잭션 유형의 플래그에 동일한 비트 값이 재사용될 수 있으므로 플래그를 설정하고 읽을 때 트랜잭션 유형 필드에 주의를 기울이는 것이 중요합니다.

플래그로 정의되지 않은 비트는 반드시 0이어야 합니다. (fix1543 수정안안은 일부 트랜잭션 유형에 이 규칙을 적용합니다. 대부분의 트랜잭션 유형은 기본적으로 이 규칙을 적용합니다.)

## 전역 플래그

모든 트랜잭션에 전역적으로 적용되는 유일한 플래그는 다음과 같습니다:

| 플래그 이름              | 16진수 값     | 소수점 값      | 설명                                                                                    |
| ------------------- | ---------- | ---------- | ------------------------------------------------------------------------------------- |
| tfFullyCanonicalSig | 0x80000000 | 2147483648 | 지원 중단됨 효과가 없습니다. ( RequireFullyCanonicalSig 수정안이 활성화되지 않은 경우 이 플래그는 전체 표준 서명을 적용합니다.) |

서명 메소드(또는 "서명 후 제출" 모드에서 제출 메소드)를 사용할 때, rippled는 Flags 필드가 이미 존재하지 않는 한 tfFullyCanonicalSig가 활성화된 Flags 필드를 추가합니다. Flags가 명시적으로 지정되어 있는 경우 tfFullyCanonicalSig 플래그는 자동으로 활성화되지 않습니다. [sign\_for](https://xrpl.org/sign\_for.html) 메소드를 사용하여 다중 서명 트랜잭션에 서명을 추가할 때 이 플래그는 자동으로 활성화되지 않습니다.

{% hint style="info" %}
Note:

2014년부터 2020년까지 레거시 서명 소프트웨어와의 호환성을 유지하면서 트랜잭션의 가변성을 방지하기 위해 tfFullyCanonicalSig 플래그가 사용되었습니다. RequireFullyCanonicalSig 수정안으로 이러한 레거시 소프트웨어와의 호환성이 종료되었으며, 모든 트랜잭션에 대해 보호 기능이 기본값으로 설정되었습니다. RequireFullyCanonicalSig가 활성화되지 않은 병렬 네트워크를 사용하는 경우, 트랜잭션 가변성으로부터 보호하기 위해 항상 tfFullyCanonicalSig 플래그를 활성화해야 합니다.
{% endhint %}

## 플래그 범위

트랜잭션의 플래그 필드에는 다양한 수준 또는 컨텍스트에 적용되는 플래그가 포함될 수 있습니다. 각 컨텍스트에 대한 플래그는 다음 범위로 제한됩니다:

| 범위 이름     | 비트 마스크     | 설명                                                  |
| --------- | ---------- | --------------------------------------------------- |
| 유니버설 플래그  | 0xff000000 | 모든 트랜잭션 유형에 동일하게 적용되는 플래그입니다.                       |
| 유형 기반 플래그 | 0x00ff0000 | 플래그를 사용하는 트랜잭션 유형 에 따라 의미가 다른 플래그입니다.               |
| 예약 플래그    | 0x0000ffff | 현재 정의되지 않은 플래그입니다. 트랜잭션은 이러한 플래그가 비활성화된 경우에만 유효합니다. |

{% hint style="info" %}
Note:

[AccountSet](https://xrpl.org/accountset.html) 트랜잭션 유형에는 유형 기반 플래그와 비슷한 용도로 사용되는 자체 비비트 단위 플래그가 있습니다. ledger 객체에는 다른 비트 단위 플래그 정의가 있는 플래그 필드도 있습니다.
{% endhint %}

## 메모 필드

메모 필드에는 트랜잭션과 함께 임의의 메시징 데이터가 포함됩니다. 이는 객체 배열로 표시됩니다. 각 객체에는 메모 필드가 하나만 있으며, 이 메모 필드는 다음 필드 중 하나 이상을 가진 다른 객체를 포함합니다:

| 필드         | 유형  | 내부 유형 | 설명                                                                           |
| ---------- | --- | ----- | ---------------------------------------------------------------------------- |
| MemoData   | 문자열 | blob  | 일반적으로 메모의 내용을 포함하는 임의의 16진수 값입니다.                                            |
| MemoFormat | 문자열 | blob  | URL에 허용되는 문자를 나타내는 16진수 값입니다. 일반적으로 메모가 인코딩되는 방법에 대한 정보(예: MIME 유형)를 포함합니다.  |
| MemoType   | 문자열 | blob  | URL에 허용되는 문자를 나타내는 16진수 값입니다. 일반적으로 이 메모의 형식을 정의하는 고유한 관계(RFC 5988 에 따름)입니다. |

MemoType 및 MemoFormat 필드는 다음 문자로만 구성되어야 합니다: <mark style="background-color:yellow;">ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789-.\_\~:/?#\[]@!$&'()\*+,;=%</mark>

메모 필드의 크기는 1KB 이하로 제한됩니다(바이너리 형식으로 직렬화할 경우).

메모 필드가 있는 트랜잭션의 예시입니다:

```
{
    "TransactionType": "Payment",
    "Account": "rMmTCjGFRWPz8S2zAUUoNVSQHxtRQD4eCx",
    "Destination": "r3kmLJN5D28dHuH8vZNUZpMC43pEHpaocV",
    "Memos": [
        {
            "Memo": {
                "MemoType": "687474703a2f2f6578616d706c652e636f6d2f6d656d6f2f67656e65726963",
                "MemoData": "72656e74"
            }
        }
    ],
    "Amount": "1"
}
```

## 네트워크 ID 필드

[![New in: rippled 1.11.0](https://img.shields.io/badge/New%20in-rippled%201.11.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.11.0)

NetworkID 필드는 "크로스 체인" 트랜잭션 리플레이 공격에 대한 보호 기능으로, 동일한 트랜잭션이 복사되어 의도하지 않은 병렬 네트워크에서 실행되는 것을 방지합니다. 기존 체인과의 호환성을 위해 네트워크 ID가 1024 이하인 네트워크에서는 NetworkID 필드를 생략해야 하지만, 네트워크 ID가 1025 이상인 네트워크에서는 반드시 포함해야 합니다. 다음 표는 알려진 다양한 네트워크의 상태와 값을 보여줍니다:

| 회로망                | ID    | NetworkID필드              |
| ------------------ | ----- | ------------------------ |
| 메인넷                | 0     | 허용되지 않음                  |
| 테스트넷               | 1     | 허용되지 않음                  |
| 데브넷                | 2     | 허용되지 않음                  |
| AMM 데브넷            | 25    | 허용되지 않음                  |
| 사이드체인 Devnet 잠금 체인 | 2551  | 허용되지 않지만 업데이트 후 필수가 됩니다. |
| 사이드체인 Devnet 발급 체인 | 2552  | 허용되지 않지만 업데이트 후 필수가 됩니다. |
| Hooks V3 테스트넷      | 21338 | 필수의                      |

트랜잭션 리플레이 공격은 이론적으로 가능하지만 두 번째 네트워크에서 특정 조건이 필요합니다. 다음 조건이 모두 참이어야 합니다:

* 트랜잭션 발신자가 두 번째 네트워크의 자금이 있는 계정입니다.
* 두 번째 네트워크에 있는 발신자의 시퀀스 번호가 트랜잭션의 시퀀스와 일치하거나 트랜잭션이 두 번째 네트워크에서 사용할 수 있는 티켓을 사용합니다.
* 트랜잭션에 LastLedgerSequence 필드가 없거나 두 번째 ledger의 현재 ledger 인덱스보다 높은 값을 지정합니다.
  * 메인넷은 일반적으로 테스트 네트워크나 사이드체인보다 ledger 인덱스가 높기 때문에, 트랜잭션이 의도한 대로 LastLedgerSequence를 사용하는 경우 사이드체인이나 테스트 네트워크에서 메인넷 트랜잭션을 리플레이하는 것이 그 반대보다 더 쉽습니다.
* 두 네트워크의 ID가 모두 1024 이하이거나, 두 네트워크가 동일한 ID를 사용하거나, 두 번째 네트워크에는 NetworkID 필드가 필요하지 않습니다.

## 서명자 필드

서명자 필드에는 최대 32개 키 쌍의 서명이 있는 다중 서명이 포함되어 있으며, 이 서명이 합쳐져 트랜잭션을 승인해야 합니다. 서명자 목록은 각각 하나의 필드인 서명자가 있는 객체 배열입니다. 서명자 필드에는 다음과 같은 중첩 필드가 있습니다:

| 필드            | 유형  | 내부 유형 | 설명                                          |
| ------------- | --- | ----- | ------------------------------------------- |
| Account       | 문자열 | 계정 ID | 서명자 목록에 표시되는 이 서명과 연결된 주소입니다.               |
| TxnSignature  | 문자열 | blob  | SigningPubKey를 사용하여 확인할 수 있는 이 트랜잭션의 서명입니다. |
| SigningPubKey | 문자열 | blob  | 이 서명을 만드는 데 사용되는 공개 키입니다.                   |

SigningPubKey는 계정 주소와 연결된 키여야 합니다. 참조된 계정이 ledger의 펀딩된 계정인 경우, 해당 계정의 현재 일반 키가 설정되어 있는 경우 SigningPubKey가 해당 계정의 현재 일반 키일 수 있습니다. lsfDisableMaster 플래그가 활성화되어 있지 않은 경우 해당 계정의 마스터 키일 수도 있습니다. 참조된 계정 주소가 ledger에 있는 자금이 있는 계정이 아닌 경우, SigningPubKey는 해당 주소와 연결된 마스터 키여야 합니다.

서명 확인은 컴퓨팅 집약적인 작업이므로 다중 서명 트랜잭션은 네트워크에 릴레이하는 데 추가 XRP가 필요합니다. 다중 서명에 포함된 각 서명은 트랜잭션에 필요한 트랜잭션 비용을 증가시킵니다. 예를 들어, 현재 트랜잭션을 네트워크에 릴레이하는 데 필요한 최소 트랜잭션 비용이 10000드롭이라면 서명자 배열에 3개의 항목이 있는 다중 서명 트랜잭션은 릴레이하는 데 최소 40000드롭의 수수료 값이 필요합니다.