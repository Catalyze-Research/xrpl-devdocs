# RippleState

RippleState 객체 유형은 단일 화폐로 두 개의 계정을 연결합니다. 개념적으로 RippleState 객체는 양쪽 계정 사이에 있는 두 개의 신뢰선을 나타냅니다. 각 계정은 RippleState 객체의 해당 쪽 설정을 변경할 수 있지만 잔액은 하나의 공유 값입니다. 완전히 기본 상태인 신뢰선은 존재하지 않는 신뢰선과 동일하게 간주되므로 rippled은 RippleState 객체의 속성이 완전히 기본값일 때 RippleState 객체를 삭제합니다.

## 높은 계정과 낮은 계정

특정 계정 쌍에 대해 화폐당 하나의 RippleState 객체만 있을 수 있습니다. XRP Ledger에 권한이 있는 계정은 없으므로, RippleState 객체는 계정 주소를 숫자순으로 정렬하여 표준 형식을 보장합니다. 디코딩할 때 숫자로 더 낮은 주소는 "낮은 계정"으로 간주되고 다른 하나는 "높은 계정"으로 간주됩니다. 신뢰선의 순 잔액은 낮은 계좌의 관점에서 저장됩니다.

신뢰선의 잔액에 대한 "발행자"는 잔액이 양수인지 음수인지에 따라 달라집니다. RippleState 객체에 양수 잔액이 표시되면 높은 계정이 발행자입니다. 잔액이 마이너스인 경우, 낮은 계정이 발행자입니다. 발행자는 한도를 0으로 설정하고 다른 계정은 양수 한도를 설정하는 경우가 많지만, 한도는 기존 잔액에 영향을 주지 않고 변경될 수 있으므로 신뢰할 수 없습니다.

## RippleState JSON 예시

```
{
    "Balance": {
        "currency": "USD",
        "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
        "value": "-10"
    },
    "Flags": 393216,
    "HighLimit": {
        "currency": "USD",
        "issuer": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
        "value": "110"
    },
    "HighNode": "0000000000000000",
    "LedgerEntryType": "RippleState",
    "LowLimit": {
        "currency": "USD",
        "issuer": "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW",
        "value": "0"
    },
    "LowNode": "0000000000000000",
    "PreviousTxnID": "E3FE6EA3D48F0C2B639448020EA4F03D4F4F8FFDB243A852A0F59177921B4879",
    "PreviousTxnLgrSeq": 14090896,
    "index": "9CA88CDEDFF9252B3DE183CE35B038F57282BC9503CDFA1923EF9A95DF0D6F7B"
}
```

## RippleState 필드

RippleState 객체에는 다음과 같은 필드가 있습니다:

| 이름                | JSON 유형 | 내부 유형  | 필수 여부 | 설명                                                                                                                  |
| ----------------- | ------- | ------ | ----- | ------------------------------------------------------------------------------------------------------------------- |
| Balance           | 객체      | 금액     | 예     | 낮은 계정의 관점에서 본 신뢰선의 잔액입니다. 잔액이 마이너스이면 높은 계정이 낮은 계정에서 발행한 토큰을 보유하고 있음을 나타냅니다. 여기서 발행자는 항상 중립 값인 ACCOUNT\_ONE으로 설정됩니다. |
| Flags             | 숫자      | UInt32 | 예     | 이 객체에 대해 활성화된 boolean 옵션의 비트맵입니다.                                                                                   |
| HighLimit         | 물체      | 금액     | 예     | 계좌가 신뢰선에 설정한 한도입니다. 발행자는 이 한도를 설정한 고액 계정의 주소입니다.                                                                    |
| HighNode          | 문자열     | UInt64 | 예     | (일부 기록 ledger에서는 생략) 디렉터리가 여러 페이지로 구성된 경우 높은 계정 소유자 디렉터리의 어느 페이지가 이 개체에 연결되는지 나타내는 힌트입니다.                           |
| HighQualityIn     | 숫자      | UInt32 | 아니요   | 높은 계정에 의해 설정된 인바운드 품질로, 암시된 비율 HighQualityIn:1,000,000,000의 정수입니다. 특별한 경우로 0은 액면가인 10억에 해당합니다.                      |
| HighQualityOut    | 숫자      | UInt32 | 아니요   | 높은 계정에서 설정한 아웃바운드 품질로, 내재 비율 HighQualityOut:1,000,000,000의 정수입니다. 특수한 경우로, 값 0은 10억 또는 액면가에 해당합니다                   |
| LedgerEntryType   | 문자열     | UInt16 | 예     | 0x0072 값은 RippleState 문자열에 매핑되어 이 객체가 RippleState 객체임을 나타냅니다.                                                       |
| LowLimit          | 물체      | 금액     | 예     | 낮은 계정이 신뢰선에 설정한 한도입니다. 발행자는 이 한도를 설정한 낮은 계정의 주소입니다.                                                                 |
| LowNode           | 문자열     | UInt64 | 예     | (일부 기록 ledger에서는 생략) 디렉터리가 여러 페이지로 구성된 경우 낮은 계정의 소유자 디렉토리의 어느 페이지가 이 객체에 연결되는지 나타내는 힌트입니다.                          |
| LowQualityIn      | 숫자      | UInt32 | 아니요   | 낮은 계정에 의해 설정된 인바운드 품질로, 암시된 비율 LowQualityIn:1,000,000,000의 정수입니다. 특수한 경우로, 값 0은 10억 또는 액면가에 해당합니다.                  |
| LowQualityOut     | 숫자      | UInt32 | 아니요   | 낮은 계정에서 설정한 아웃바운드 품질로, 내재 비율 LowQualityOut:1,000,000,000의 정수입니다. 특수한 경우로 0은 액면가인 10억에 해당합니다.                        |
| PreviousTxnID     | 문자열     | 해시256  | 예     | 이 개체를 가장 최근에 수정한 트랜잭션의 식별 해시입니다.                                                                                    |
| PreviousTxnLgrSeq | 숫자      | UInt32 | 예     | 이 객체를 가장 최근에 수정한 트랜잭션이 포함된 ledger의 인덱스입니다.                                                                          |

## RippleState 플래그

신뢰선에 대해 활성화 또는 비활성화할 수 있는 몇 가지 옵션이 있습니다. 이러한 옵션은 [TrustSet](https://xrpl.org/trustset.html) 트랜잭션으로 변경할 수 있습니다. ledger에서 플래그는 비트 또는 연산과 결합할 수 있는 이진 값으로 표시됩니다. ledger에 있는 플래그의 비트값은 트랜잭션에서 해당 플래그를 활성화 또는 비활성화하는 데 사용되는 값과 다릅니다. ledger 플래그의 이름은 lsf로 시작합니다.

RippleState 객체는 다음과 같은 플래그 값을 가질 수 있습니다:&#x20;

| 플래그 이름            | 16진수 값       | 소수값     | 해당 [TrustSet 플래그](https://xrpl.org/trustset.html#trustset-flags) | 설명                                            |
| ----------------- | ------------ | ------- | ---------------------------------------------------------------- | --------------------------------------------- |
| `lsfLowReserve`   | `0x00010000` | 65536   | (없음)                                                             | 이 RippleState 객체는 낮은 계정의 소유자 보유액에 기여합니다.      |
| `lsfHighReserve`  | `0x00020000` | 131072  | (없음)                                                             | 이 RippleState 개체는 높은 계정의 소유자 보유액에 기여합니다.      |
| `lsfLowAuth`      | `0x00040000` | 262144  | `tfSetAuth`                                                      | 낮은 계정은 낮은 계정에서 발행한 토큰을 보유하도록 높은 계정을 승인했습니다.   |
| `lsfHighAuth`     | `0x00080000` | 524288  | `tfSetAuth`                                                      | 상위 계정은 상위 계정에서 발행한 토큰을 보유하도록 하위 계정을 승인했습니다.   |
| `lsfLowNoRipple`  | `0x00100000` | 1048576 | `tfSetNoRipple`                                                  | 낮은 계정은 이 신뢰 라인에서 rippling을 비활성화했습니다.          |
| `lsfHighNoRipple` | `0x00200000` | 2097152 | `tfSetNoRipple`                                                  | 높은 계정은 이 신뢰 라인에서 rippling을 비활성화했습니다.          |
| `lsfLowFreeze`    | `0x00400000` | 4194304 | `tfSetFreeze`                                                    | 낮은 계좌는 신뢰 한도를 동결시켜 높은 계좌가 자산을 이전할 수 없도록 했습니다. |
| `lsfHighFreeze`   | `0x00800000` | 8388608 | `tfSetFreeze`                                                    | 높은 계정은 신뢰 한도를 동결하여 낮은 계정의 자산 이전을 방지했습니다.      |

## 소유자 Reserve에 기여

계정이 신뢰선을 수정해 기본값이 아닌 상태로 만들면, 해당 신뢰선은 계정의 소유자 reserve에 포함됩니다. RippleState 오브젝트에서 lsfLowReserve와 lsfHighReserve 플래그는 어떤 계정이 소유자 reserve을 책임지는지를 나타냅니다. rippled 서버는 신뢰선을 수정할 때 이 플래그를 자동으로 설정합니다.

신뢰선의 기본값이 아닌 상태에 포함되는 값은 다음과 같습니다:

| 높은 계정 책임이 있는 경우...                           | 낮은 계정 책임이 있는 경우...                           |
| -------------------------------------------- | -------------------------------------------- |
| `Balance`음수(높은 계정에 통화 보유)                    | `Balance`양수(낮은 계정에 통화 보유)                    |
| `HighLimit` 0이 아니다.                          | `LowLimit` 0이 아니다.                           |
| LowQualityIn은 0이 아니며 그리고 1000000000이 아닙니다.   | HighQualityIn은 0이 아니며 그리고 1000000000이 아닙니다.  |
| LowQualityOut0은 0이 아니며 그리고 1000000000이 아닙니다. | HighQualityOut은 0이 아니며 그리고 1000000000이 아닙니다. |
| `lsfHighNoRipple`플래그가 기본 상태가 아닙니다.           | `lsfLowNoRipple`플래그가 기본 상태가 아닙니다.            |
| `lsfHighFreeze`플래그가 활성화됨.                    | `lsfLowFreeze`플래그가 활성화됨.                     |

&#x20;lsfLowAuth 및 lsfHighAuth 플래그는 비활성화할 수 없으므로 기본 상태에 포함되지 않습니다.

두 Ripple 없음 플래그의 기본 상태는 해당 AccountRoot 객체에 있는 lsfDefaultRipple 플래그의 상태에 따라 달라집니다. 기본 Ripple이 비활성화되어 있으면(기본값) 계정의 모든 신뢰선에 대해 lsfNoRipple 플래그의 기본 상태가 활성화됩니다. 계정에서 기본 Ripple을 사용하도록 설정한 경우에는 기본적으로 계정의 신뢰 관계에 대해 lsfNoRipple 플래그가 비활성화됩니다(rippling이 활성화됨).

{% hint style="info" %}
Note:

rippled 버전 0.27.3(2015년 3월 10일)에 기본 Ripple 플래그가 도입되기 전에는 모든 신뢰선의 기본 상태가 rippled 없음 플래그가 모두 비활성화된 상태(rippled링 활성화)였습니다.
{% endhint %}

다행히도 rippled은 지연 평가를 사용하여 소유자 reserve 계산합니다. 즉, 계정이 기본 rippled 플래그를 변경하여 모든 신뢰선의 기본 상태를 변경하더라도 해당 계정의 reserve는 처음에 동일하게 유지됩니다. 계정이 신뢰선을 수정하면 rippled는 해당 개별 신뢰선이 기본 상태인지, 소유자 reserve에 기여해야 하는지 다시 평가합니다.

## RippleState ID 형식

RippleState 객체의 ID는 다음 값의 절반을 순서대로 연결한 SHA-512입니다:

* RippleState 스페이스 키(0x0072)
* 낮은 계정의 AccountID
* 상위 계정의 계정ID
* 신뢰선의 160비트 화폐 코드
