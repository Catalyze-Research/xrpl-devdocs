# UNLModify

_(_[_NegativeUNL_ ](https://xrpl.org/known-amendments.html#negativeunl)_수정안에 의해 추가되었습니다.)_

UNLModify pseudo 트랜잭션은 신뢰할 수 있는 유효성 검증인이 오프라인 상태가 되었거나 다시 온라인 상태가 되었음을 나타내는 [Negative UNL](https://xrpl.org/negative-unl.html)로 변경되었음을 표시합니다.

{% hint style="info" %}
Note:

Pseudo 트랜잭션을 보낼 수는 없지만, ledger을 처리할 때 pseudo 트랜잭션을 찾을 수 있습니다.
{% endhint %}

## UNLModify JSON 예시

```
{
  "Account": "",
  "Fee": "0",
  "LedgerSequence": 1600000,
  "Sequence": 0,
  "SigningPubKey": "",
  "TransactionType": "UNLModify",
  "UNLModifyDisabling": 1,
  "UNLModifyValidator": "ED6629D456285AE3613B285F65BBFF168D695BA3921F309949AFCD2CA7AFEC16FE",
}
```

## UNLModify 필드

pseudo 트랜잭션은 공통 필드 외에도 다음 필드를 사용합니다:

| 이름                   | JSON 유형 | [내부 유형](https://xrpl.org/serialization.html) | 설명                                                                                                 |
| -------------------- | ------- | -------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| `TransactionType`    | 문자열     | UInt16                                       | 값 0x0066은 UNLModify 문자열에 매핑된 값으로, 이 객체가 UNLModify 의사 트랜잭션임을 나타냅니다.                                 |
| `LedgerSequence`     | 숫자      | UInt32                                       | 이 의사 트랜잭션이 나타나는 원장 인덱스입니다. 이는 의사 트랜잭션을 동일한 변경의 다른 발생과 구별합니다.                                       |
| `UNLModifyDisabling` | 숫자      | UInt8                                        | 1이면 이 변경은 부정적 UNL에 검증자를 추가하는 것을 나타냅니다. 0이면 이 변경은 부정적 UNL에서 유효성 검사기를 제거함을 나타냅니다. (다른 값은 허용되지 않습니다.) |
| `UNLModifyValidator` | 문자열     | blob                                         | 마스터 공개 키로 식별되는 추가 또는 제거할 유효성 검사기입니다.                                                               |
