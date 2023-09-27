# EscrowCancel

_에스크로 수정안에 추가되었습니다._

에스크로된 XRP를 발신자에게 반환합니다.

## EscrowCancel JSON 예시

```
{
    "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "TransactionType": "EscrowCancel",
    "Owner": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "OfferSequence": 7,
}
```

## EscrowCancel 필드

일반적인 필드 외에도 EscrowCancel 트랜잭션은 다음 필드를 사용합니다:

| 필드              | JSON 유형 | [내부 유형](https://xrpl.org/serialization.html) | 설명                                                      |
| --------------- | ------- | -------------------------------------------- | ------------------------------------------------------- |
| `Owner`         | 문자열     | 계정 ID                                        | 에스크로 결제에 자금을 제공한 출처 계좌의 주소입니다.                          |
| `OfferSequence` | 숫자      | UInt32                                       | 취소할 에스크로를 생성한 EscrowCreate 트랜잭션의 트랜잭션 시퀀스(또는 티켓 번호)입니다. |

모든 계정이 EscrowCancel 트랜잭션을 제출할 수 있습니다.

* 해당 EscrowCreate 트랜잭션에 CancelAfter 시간이 지정되지 않은 경우 EscrowCancel 트랜잭션은 실패합니다.
* 그렇지 않고 CancelAfter 시간이 가장 최근에 닫힌 ledger의 마감 시간 이후인 경우 EscrowCancel 트랜잭션은 실패합니다.
