# EnableAmendment

`EnableAmendment` pseudo-transaction은 제안된 수정안(proposed amendment)의 상태가 변경되었음을 표시합니다:

* 검증인으로부터 과반수 이상의 승인을 얻습니다.
* 과반수 승인을 잃은 경우.
* XRP Ledger 프로토콜에서 활성화됨.

## EnableAmendment JSON 예시

```
{
  "Account": "rrrrrrrrrrrrrrrrrrrrrhoLvTp",
  "Amendment": "42426C4D4F1009EE67080A9B7965B44656D7714D104A72F9B4369F97ABF044EE",
  "Fee": "0",
  "LedgerSequence": 21225473,
  "Sequence": 0,
  "SigningPubKey": "",
  "TransactionType": "EnableAmendment"
}  
```

## EnableAmendment Fields

공통 필드([common fields](https://xrpl.org/pseudo-transaction-types.html)) 외에도 EnableAmendment pseudo-transactions은 다음 필드를 사용합니다:

| 필드               | JSON 유형 | [내부 유형](https://xrpl.org/serialization.html) | 설명                                                                                                       |
| ---------------- | ------- | -------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| `Amendment`      | 문자열     | 해시256                                        | 수정안의 고유 식별자입니다. 이는 사람이 읽을 수 있는 이름이 아닙니다. 알려진 수정 사항 목록은 [수정 사항을](https://xrpl.org/amendments.html) 참조하세요. |
| `LedgerSequence` | 숫자      | UInt32                                       | 이 의사 트랜잭션이 나타나는 ledger 인덱스 입니다. 이는 의사 트랜잭션을 동일한 변경의 다른 발생과 구별합니다.                                        |

## EnableAmendment Flags

Pseudo-transactions의  `Flags`값은 pseudo transactions을 포함한 ledger 시점의 수정 상태를 나타냅니다.

&#x20;`Flags` 값이 0(플래그 없음) 또는 생략된 `Flags`필드는 수정이 활성화되었음을 나타내며 이후 모든 ledger에 적용됩니다. 다른 `Flags`값은 다음과 같습니다:

| 플래그 이름           | 16진수 값       | 소수점 값  | 설명                                                           |
| ---------------- | ------------ | ------ | ------------------------------------------------------------ |
| `tfGotMajority`  | `0x00010000` | 65536  | 이 수정 사항에 대한 지원은 이 ledger 버전부터 신뢰할 수 있는 검증자의 최소 80%로 증가되었습니다. |
| `tfLostMajority` | `0x00020000` | 131072 | 이 수정 사항에 대한 지원은 이 ledger 버전부터 신뢰할 수 있는 검증자의 80% 미만으로 감소했습니다. |
