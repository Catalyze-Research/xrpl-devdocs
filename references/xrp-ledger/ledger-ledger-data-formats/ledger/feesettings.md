# FeeSettings

금FeeSettings 객체 유형에는 수수료 투표로 결정된 현재 기본 트랜잭션 비용과 준비금이 포함됩니다. 각 ledger 버전에는 최대 하나의 FeeSettings 객체가 포함됩니다.

## FeeSettings JSON 예시

FeeSettings 객체 예시입니다:

```
{
   "BaseFee": "000000000000000A",
   "Flags": 0,
   "LedgerEntryType": "FeeSettings",
   "ReferenceFeeUnits": 10,
   "ReserveBase": 20000000,
   "ReserveIncrement": 5000000,
   "index": "4BC50C9B0D8515D3EAAE1E74B29A95804346C491EE1A95BF25E4AAB854A6A651"
}
```

## FeeSettings 필드

FeeSettings 객체에는 다음과 같은 필드가 있습니다:

| 이름                  | JSON 유형 | [내부 유형](https://xrpl.org/serialization.html) | 필수 여부 | 설명                                                                                           |
| ------------------- | ------- | -------------------------------------------- | ----- | -------------------------------------------------------------------------------------------- |
| `BaseFee`           | 문자열     | Uint64                                       | 예     | "참조 거래"의 거래 비용은 XRP 에서 16진수로 표시됩니다.                                                          |
| `Flags`             | 숫자      | Uint32                                       | 예     | 이 개체에 대해 활성화된 boolean 플래그의 비트맵입니다. 현재 프로토콜은 `FeeSettings`개체에 대한 플래그를 정의하지 않습니다. 값은 항상`0`입니다. |
| `LedgerEntryType`   | 문자열     | Uint16                                       | 예     | `0x0073`문자열에 매핑된 값은 `FeeSettings`이 개체에 원장의 수수료 설정이 포함되어 있음을 나타냅니다.                           |
| `ReferenceFeeUnits` | 숫자      | Uint32                                       | 예     | `BaseFee`"수수료 단위"로 번역됩니다 .                                                                   |
| `ReserveBase`       | 숫자      | Uint32                                       | 예     | XRP Ledger에 있는 계정의 기본 준비금(XRP 드롭)입니다.                                                        |
| `ReserveIncrement`  | 숫자      | Uint32                                       | 예     | XRP 드롭과 같은 개체 소유를 위한 증분 소유자 reserve입니다.                                                      |

{% hint style="info" %}
Warning:

이 ledger 객체 유형의 JSON 형식이 비정상적입니다. BaseFee, ReserveBase, ReserveIncrement는 XRP의 하락을 나타내지만 XRP를 지정하는 일반적인 형식이 아닙니다.
{% endhint %}

XRPFees 수정이 활성화된 경우 FeeSettings 객체에 이러한 필드가 대신 표시됩니다:

| 이름                      | JSON 유형 | 내부 유형  | 필수 여부 | 설명                                                                                            |
| ----------------------- | ------- | ------ | ----- | --------------------------------------------------------------------------------------------- |
| `BaseFeeDrops`          | 문자열     | 금액     | 예     | "참조 트랜잭션"의 트랜잭션 비용을 XRP 드롭 단위로 표시합니다.                                                         |
| `Flags`                 | 숫자      | Uint32 | 예     | 이 개체에 대해 활성화된 boolean 플래그의 비트맵입니다. 현재 프로토콜은 `FeeSettings`개체에 대한 플래그를 정의하지 않습니다. 값은 항상 `0`입니다. |
| `LedgerEntryType`       | 문자열     | Uint16 | 예     | `0x0073`문자열에 매핑된 값은 `FeeSettings`이 개체에 원장의 수수료 설정이 포함되어 있음을 나타냅니다.                            |
| `ReserveBaseDrops`      | 문자열     | 금액     | 예     | XRP Ledger에 있는 계정의 기본 준비금(XRP 드롭)입니다.                                                         |
| `ReserveIncrementDrops` | 문자열     | 금액     | 예     | XRP 드롭과 같은 개체 소유를 위한 증분 소유자 reserve입니다.                                                       |

## FeeSettings ID 형식

FeeSettings 객체 ID는 FeeSettings 스페이스 키(0x0065)의 해시일 뿐입니다. 즉, ledger에 있는 FeeSettings 객체의 ID는 항상 0입니다:

```
4BC50C9B0D8515D3EAAE1E74B29A95804346C491EE1A95BF25E4AAB854A6A651
```
