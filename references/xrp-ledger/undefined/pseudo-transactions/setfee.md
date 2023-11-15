# SetFee

금금액금액SetFee pseudo 트랜잭션은 수수료 투표의 결과로 트랜잭션 비용 또는 [reserve requirements](https://xrpl.org/reserves.html)이 변경되었음을 나타냅니다.

{% hint style="info" %}
Note:

pseudo 트랜잭션을 전송할 수는 없지만, ledger을 처리할 때 pseudo 트랜잭션을 찾을 수 있습니다.
{% endhint %}

## SetFee JSON 예시

```
{
    "Account": "rrrrrrrrrrrrrrrrrrrrrhoLvTp",
    "BaseFee": "000000000000000A",
    "Fee": "0",
    "ReferenceFeeUnits": 10,
    "ReserveBase": 20000000,
    "ReserveIncrement": 5000000,
    "Sequence": 0,
    "SigningPubKey": "",
    "TransactionType": "SetFee",
    "date": 439578860,
    "hash": "1C15FEA3E1D50F96B6598607FC773FF1F6E0125F30160144BE0C5CBC52F5151B",
    "ledger_index": 3721729,
}
```

## SetFee 필드

SetFee pseudo 트랜잭션은 공통 필드 외에도 다음 필드를 사용합니다:

| 필드                  | JSON 유형  | [내부 유형](https://xrpl.org/serialization.html) | 설명                                                                                                           |
| ------------------- | -------- | -------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| `BaseFee`           | 문자열      | UInt64                                       | 참조 트랜잭션에 대한 요금(XRP 드롭 단위)을 헥스로 표시합니다. (로드를 위해 스케일링하기 전의 트랜잭션 비용입니다.)                                         |
| `ReferenceFeeUnits` | 부호 없는 정수 | UInt32                                       | 참조 트랜잭션의 수수료 단위 비용(부호 없음 정수 UInt32)                                                                          |
| `ReserveBase`       | 부호 없는 정수 | UInt32                                       | 기본 reserve(드롭 단위)                                                                                            |
| `ReserveIncrement`  | 부호 없는 정수 | UInt32                                       | 증분 reserve(드롭 단위)                                                                                            |
| `LedgerSequence`    | 숫자       | UInt32                                       | (일부 과거 SetFee 의사 트랜잭션의 경우 생략) 이 의사 트랜잭션이 나타나는 원장 버전의 인덱스입니다. 이를 통해 의사 트랜잭션을 동일한 변경이 발생한 다른 트랜잭션과 구별할 수 있습니다. |

XRPFees 수정안안이 활성화된 경우, SetFee pseudo 트랜잭션은 이 필드를 대신 사용합니다:

| 필드                      | JSON 유형 | [내부 유형](https://xrpl.org/serialization.html) | 설명                                                                                             |
| ----------------------- | ------- | -------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| `BaseFeeDrops`          | 문자열     | 금액                                           | 참조 트랜잭션에 대한 요금(XRP 드롭 단위)입니다. (로드를 위해 스케일링하기 전의 트랜잭션 비용입니다.)                                   |
| `ReserveBaseDrops`      | 문자열     | 금액                                           | 기본 reserve(드롭 단위)                                                                              |
| `ReserveIncrementDrops` | 문자열     | 금액                                           | 증분 reserve(드롭 단위)                                                                              |
| `LedgerSequence`        | 숫자      | UInt32                                       | (일부 과거 SetFee 의사 트랜잭션의 경우 생략) 이 의사 트랜잭션이 표시되는 원장 버전의 인덱스입니다. 이는 의사 트랜잭션을 동일한 변경의 다른 발생과 구별합니다. |

{% hint style="info" %}
Note:

XRP Ledger의 전체 기록에서 트랜잭션 해시는 고유하다는 규칙에 예외가 있습니다. 두 개의 초기 SetFee 유사 트랜잭션은 정확히 동일한 필드를 가지고 있었으며, 그 결과 동일한 해시인 1C15FEA3E1D50F96B6598607FC773FF1F6E0125F30160144BE0C5CBC52F5151B를 생성했습니다. 이 트랜잭션 중 첫 번째 트랜잭션은 ledger 3715073에, 두 번째 트랜잭션은 ledger 3721729에 나타납니다. 최신 SetFee pseudo 트랜잭션은 고유성을 보장하기 위해 LedgerSequence 필드를 포함합니다.
{% endhint %}
