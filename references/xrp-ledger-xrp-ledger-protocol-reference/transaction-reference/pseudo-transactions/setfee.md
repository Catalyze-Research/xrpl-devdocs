# SetFee

`SetFee` [pseudo-transaction](https://xrpl.org/pseudo-transaction-types.html)은 [Fee Voting](https://xrpl.org/fee-voting.html)의 결과로 [transaction cost](https://xrpl.org/transaction-cost.html) 또는 [reserve requirements](https://xrpl.org/reserves.html)이 변경되었음을 나타냅니다.

{% hint style="info" %}
**Note**:

Pseudo-transaction를 보낼 수는 없지만, Ledgers을 처리할 때 pseudo-transaction를 찾을 수 있습니다.
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

## SetFee Fields

SetFee pseudo transaction은 공통 필드([common fields](https://xrpl.org/pseudo-transaction-types.html)) 외에도 다음 필드를 사용합니다:

| 필드                  | JSON 유형  | [내부 유형](https://xrpl.org/serialization.html) | 설명                                                                                                           |
| ------------------- | -------- | -------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| `BaseFee`           | 문자열      | UInt64                                       | 참조 트랜잭션에 대한 요금(XRP 드롭 단위)을 헥스로 표시합니다. (로드를 위해 스케일링하기 전의 트랜잭션 비용입니다.)                                         |
| `ReferenceFeeUnits` | 부호 없는 정수 | UInt32                                       | 참조 트랜잭션의 수수료 단위 비용(부호 없음 정수 UInt32)                                                                          |
| `ReserveBase`       | 부호 없는 정수 | UInt32                                       | 기본 reserve(드롭 단위)                                                                                            |
| `ReserveIncrement`  | 부호 없는 정수 | UInt32                                       | 증분 reserve(드롭 단위)                                                                                            |
| `LedgerSequence`    | 숫자       | UInt32                                       | (일부 과거 SetFee 의사 트랜잭션의 경우 생략) 이 의사 트랜잭션이 나타나는 원장 버전의 인덱스입니다. 이를 통해 의사 트랜잭션을 동일한 변경이 발생한 다른 트랜잭션과 구별할 수 있습니다. |

[_XRPFees amendment_](https://xrpl.org/known-amendments.html#xrpfees)이 활성화된 경우,  `SetFee` pseudo-transactions은 이 필드를 대신 사용합니다:

| 필드                      | JSON 유형 | [내부 유형](https://xrpl.org/serialization.html) | 설명                                                                                             |
| ----------------------- | ------- | -------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| `BaseFeeDrops`          | 문자열     | 금액                                           | 참조 트랜잭션에 대한 요금(XRP 드롭 단위)입니다. (로드를 위해 스케일링하기 전의 트랜잭션 비용입니다.)                                   |
| `ReserveBaseDrops`      | 문자열     | 금액                                           | 기본 reserve(드롭 단위)                                                                              |
| `ReserveIncrementDrops` | 문자열     | 금액                                           | 증분 reserve(드롭 단위)                                                                              |
| `LedgerSequence`        | 숫자      | UInt32                                       | (일부 과거 SetFee 의사 트랜잭션의 경우 생략) 이 의사 트랜잭션이 표시되는 원장 버전의 인덱스입니다. 이는 의사 트랜잭션을 동일한 변경의 다른 발생과 구별합니다. |

{% hint style="info" %}
**Note**:

XRP Ledger의 전체 기록에서 트랜잭션 해시는 고유하다는 규칙에 예외가 있습니다. 두 개의 초기 [SetFee pseudo-transactions](https://xrpl.org/setfee.html)은 정확히 동일한 필드를 가지고 있었으며, 그 결과 동일한 해시인 `1C15FEA3E1D50F96B6598607FC773FF1F6E0125F30160144BE0C5CBC52F5151B`를 생성했습니다. 이 트랜잭션 중 첫 번째 트랜잭션은 [ledger 3715073](https://xrpl.org/websocket-api-tool.html?server=wss%3A%2F%2Fs2.ripple.com%2F\&req=%7B%22id%22%3A%22setfee\_nonunique\_hash\_1%22%2C%22command%22%3A%22transaction\_entry%22%2C%22tx\_hash%22%3A%221C15FEA3E1D50F96B6598607FC773FF1F6E0125F30160144BE0C5CBC52F5151B%22%2C%22ledger\_index%22%3A3715073%7D)에, 두 번째 트랜잭션은 [ledger 3721729](https://xrpl.org/websocket-api-tool.html?server=wss%3A%2F%2Fs2.ripple.com%2F\&req=%7B%22id%22%3A%22setfee\_nonunique\_hash\_1%22%2C%22command%22%3A%22transaction\_entry%22%2C%22tx\_hash%22%3A%221C15FEA3E1D50F96B6598607FC773FF1F6E0125F30160144BE0C5CBC52F5151B%22%2C%22ledger\_index%22%3A3721729%7D)에 나타납니다. 최신 [SetFee pseudo-transactions](https://xrpl.org/setfee.html)은 고유성을 보장하기 위해  `LedgerSequence`필드를 포함합니다.
{% endhint %}
