# CheckCancel

_(수표 수정안에 의해 추가되었습니다.)_

상환되지 않은 수표를 취소하여 송금하지 않고 ledger에서 제거합니다. 수표의 발신자 또는 수취인은 언제든지 이 트랜잭션 유형을 사용하여 수표를 취소할 수 있습니다. 수표가 만료된 경우 모든 주소에서 수표를 취소할 수 있습니다.

## CheckCancel JSON 예시

```
{
    "Account": "rUn84CUYbNjRoTQ6mSW7BVJPSVJNLb1QLo",
    "TransactionType": "CheckCancel",
    "CheckID": "49647F0D748DC3FE26BDACBC57F251AADEFFF391403EC9BF87C97F67E9977FB0",
    "Fee": "12"
}
```

## CheckCancel 필드

일반적인 필드 외에도 CheckCancel 트랜잭션은 다음 필드를 사용합니다:

| 필드        | JSON 유형 | [내부 유형](https://xrpl.org/serialization.html) | 설명                                            |
| --------- | ------- | -------------------------------------------- | --------------------------------------------- |
| `CheckID` | 문자열     | 해시256                                        | 취소할 Check ledger 객체 의 ID로, 64자의 16진수 문자열입니다.  |

## 오류 사례

* CheckID로 식별된 객체가 존재하지 않거나 수표가 아닌 경우, 트랜잭션이 실패하고 tecNO\_ENTRY라는 결과를 반환합니다.
* 수표가 만료되지 않았고 CheckCancel 트랜잭션의 발신자가 수표의 발신자 또는 수신자가 아닌 경우 트랜잭션은 tecNO\_PERMISSION이라는 결과와 함께 실패합니다.
