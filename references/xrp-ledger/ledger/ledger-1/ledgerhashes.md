# LedgerHashes

(ledger 버전을 고유하게 식별하는 "ledger 해시" 문자열 데이터 유형과 혼동하지 마세요. 이 섹션에서는 LedgerHashes ledger 객체 유형에 대해 설명합니다.)

LedgerHashes 객체 유형에는 이 ledger 버전에 이르는 이전 ledger의 내역이 해시 형태로 포함되어 있습니다. 이 ledger 유형의 객체는 ledger을 닫을 때 자동으로 수정됩니다. (이는 트랜잭션이나 의사 트랜잭션 없이 ledger의 상태 데이터가 수정되는 유일한 경우 중 하나입니다.) LedgerHashes 객체는 현재 ledger 버전과 이전 ledger 버전에 대한 최대 한 번의 조회만으로 이전 ledger의 해시를 조회할 수 있도록 하기 위해 존재합니다.

LedgerHashes 객체에는 두 가지 종류가 있습니다. 두 유형 모두 동일한 필드를 가집니다. 각 ledger 버전에는 다음이 포함됩니다:

* 정확히 하나의 "최근 내역" LedgerHashes 객체.
* 현재 ledger 인덱스(즉, ledger 기록의 길이)를 기반으로 하는 여러 개의 "이전 기록" LedgerHashes 객체. 구체적으로, XRP Ledger는 65536개의 ledger 버전마다 새로운 "이전 내역" 객체를 추가합니다.

{% hint style="info" %}
Note:

예외적으로, 새로운 제네시스 ledger는 ledger 내역이 없기 때문에 LedgerHashes 객체가 전혀 없습니다.
{% endhint %}

LedgerHashes 객체 예시(길이에 맞게 잘림):

```
{
  "LedgerEntryType": "LedgerHashes",
  "Flags": 0,
  "FirstLedgerSequence": 2,
  "LastLedgerSequence": 33872029,
  "Hashes": [
    "D638208ADBD04CBB10DE7B645D3AB4BA31489379411A3A347151702B6401AA78",
    "254D690864E418DDD9BCAC93F41B1F53B1AE693FC5FE667CE40205C322D1BE3B",
    "A2B31D28905E2DEF926362822BC412B12ABF6942B73B72A32D46ED2ABB7ACCFA",
    "AB4014846DF818A4B43D6B1686D0DE0644FE711577C5AB6F0B2A21CCEE280140",
    "3383784E82A8BA45F4DD5EF4EE90A1B2D3B4571317DBAC37B859836ADDE644C1",
    ... (up to 256 ledger hashes) ...
  ],
  "index": "B4979A36CDC7F3D3D5C31A4EAE2AC7D7209DDA877588B9AFC66799692AB0D66B"
}
```

LedgerHashes 객체에는 다음과 같은 필드가 있습니다:

| 이름                    | JSON 유형 | 내부 유형  | 필수 여부 | 설명                                                                                                                                                                                                            |
| --------------------- | ------- | ------ | ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `FirstLedgerSequence` | 숫자      | UInt32 | 예     | **DEPRECATED** 사용하지 마세요. (프로덕션 XRP Ledger의 "최근 해시" 개체는 이전 rippled 소프트웨어의 결과로 이 필드에 2라는 값을 가집니다. 이 값은 "최근 해시" 개체가 업데이트될 때 이월됩니다. 새로운 "이전 기록" 객체에는 이 필드가 없으며, 병렬 네트워크의 "최근 해시" 객체는 더 최신 버전의 rippled에서 시작되었습니다). |
| `Flags`               | 숫자      | UInt32 | 예     | 이 개체에 대해 활성화된 부울 플래그의 비트맵입니다. 현재 프로토콜은 `LedgerHashes`개체에 대한 플래그를 정의하지 않습니다. 값은 항상 0입니다.                                                                                                                       |
| `Hashes`              | 문자열 배열  | 벡터256  | 예     | 최대 256개의 ledger 해시 배열입니다. `LedgerHashes`내용은 객체 의 하위 유형에 따라 다릅니다.                                                                                                                                              |
| `LastLedgerSequence`  | 숫자      | UInt32 | 예     | 이 개체 `Hashes`배열의 마지막 항목에 대한 원장 인덱스 입니다.                                                                                                                                                                       |
| `LedgerEntryType`     | 문자열     | UInt16 | 예     | `0x0068`문자열에 매핑된 값은 `LedgerHashes`이 개체가 원장 해시 목록임을 나타냅니다.                                                                                                                                                     |

## 최근 내역 LedgerHashes

생성 ledger 이후의 모든 ledger에는 "최근 내역" 하위 유형의 LedgerHashes 객체가 정확히 하나씩 있습니다. 이 객체에는 가장 최근 256개 ledger 버전(또는 ledger 기록의 총 ledger이 256개 미만인 경우 그 이하)의 식별 해시가 해시 배열에 포함됩니다. 새 ledger을 닫을 때마다, ledger을 닫는 과정의 일부에는 이 ledger 버전이 파생된 이전 ledger 버전(이 ledger 버전의 부모 ledger이라고도 함)의 해시로 "최근 내역" 객체를 업데이트하는 작업이 포함됩니다. 256개 이상의 해시가 있는 경우 가장 오래된 해시가 제거됩니다.

특정 ledger의 "최근 내역" LedgerHashes 객체를 사용하면 주어진 ledger 버전 이전의 256개 ledger 버전 내 모든 ledger 인덱스의 해시를 얻을 수 있습니다.

## 이전 기록 LedgerHashes

"이전 기록" LedgerHashes 항목에는 ledger의 전체 기록에서 256번째 ledger 버전("플래그 ledger"이라고도 함)의 해시가 집합적으로 포함됩니다. 플래그 ledger의 하위 ledger이 닫히면 플래그 ledger의 해시는 최신 "이전 기록" LedgerHashes 객체의 해시 배열에 추가됩니다. rippled은 65536개의 ledger이 닫힐 때마다 새 LedgerHashes 객체를 생성하므로, 각 "이전 기록" 객체는 256개의 플래그 ledger의 해시를 갖게 됩니다.

{% hint style="info" %}
Note:

가장 오래된 "이전 기록" LedgerHashes 객체에는 255개의 항목만 포함되는데, 이는 생성 ledger의 ledger 인덱스가 0이 아니라 1이기 때문입니다.
{% endhint %}

"이전 기록" LedgerHashes 객체는 스킵 목록 역할을 하므로 해당 인덱스에서 과거 플래그 ledger의 해시를 가져올 수 있습니다. 거기에서 해당 플래그 ledger의 "최근 내역" 객체를 사용하여 다른 ledger의 해시를 얻을 수 있습니다.

## LedgerHashes ID 형식

객체가 "최근 내역" 하위 유형인지, 아니면 "이전 내역" 하위 유형인지에 따라 LedgerHashes 객체 ID에는 두 가지 형식이 있습니다.

"최근 기록" LedgerHashes 객체는 LedgerHashes 스페이스 키(0x0073)의 SHA-512 절반인 ID를 가집니다. 즉, "최근 기록"은 항상 ID가 B4979A36CDC7F3D3D5C31A4EAE2AC7D7209DDA877588B9AFC66799692AB0D66B입니다.

"이전 기록" LedgerHashes 객체의 ID는 다음 값의 절반인 SHA-512를 순서대로 연결한 것입니다:

* LedgerHashes 스페이스 키(0x0073)
* 객체의 해시 배열에 있는 플래그 ledger의 32비트 ledger 인덱스를 65536으로 나눈 값입니다.

{% hint style="info" %}
Tip:

65536으로 나누면 "이전 기록" 객체에 나열된 모든 플래그 ledger에 대해 가장 중요한 16비트가 유지되며, 해당 ledger만 유지됩니다. 이 사실을 사용하여 플래그 ledger의 해시가 포함된 LedgerHashes 객체를 조회할 수 있습니다.
{% endhint %}
