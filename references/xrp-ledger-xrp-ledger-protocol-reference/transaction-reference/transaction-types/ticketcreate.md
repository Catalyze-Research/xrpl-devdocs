# TicketCreate

(티켓배치 수정안에 의해 추가되었습니다.)

티켓 생성 트랜잭션은 하나 이상의 시퀀스 번호를 티켓으로 따로 설정합니다.

## TicketCreate JSON 예제

```
{
    "TransactionType": "TicketCreate",
    "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "Fee": "10",
    "Sequence": 381,
    "TicketCount": 10
}
```

## TicketCreate 필드

일반적인 필드 외에도 TicketCreate 트랜잭션은 다음 필드를 사용합니다:

| 필드            | JSON 유형 | [내부 유형](https://xrpl.org/serialization.html) | 설명                                                               |
| ------------- | ------- | -------------------------------------------- | ---------------------------------------------------------------- |
| `TicketCount` | 숫자      | UInt32                                       | 생성할 티켓 수. 이는 양수여야 하며 이 거래를 실행한 후 계정이 250장 이상의 티켓을 소유하게 할 수 없습니다. |

250-티켓 한도 또는 소유자 예약으로 인해 요청된 모든 티켓을 생성할 수 없는 경우 트랜잭션이 실패하고 티켓이 생성되지 않습니다. 계정이 현재 소유하고 있는 티켓 수를 조회하려면 account\_info 메소드를 사용하여 account\_data.TicketCount 필드를 확인하세요.

{% hint style="info" %}
Tip:

이 트랜잭션은 보내는 계정의 시퀀스 번호에 생성된 티켓 수(TicketCount)를 더하여 1씩 증가시킵니다. 이 트랜잭션은 계정의 시퀀스 번호가 1 이상 증가하는 유일한 트랜잭션입니다.
{% endhint %}

## 오류 사례

모든 트랜잭션에서 발생할 수 있는 오류 외에도 티켓 만들기 트랜잭션에서 다음과 같은 트랜잭션 결과 코드가 발생할 수 있습니다:

| 에러 코드                     | 설명                                                                       |
| ------------------------- | ------------------------------------------------------------------------ |
| `temINVALID_COUNT`        | 필드 `TicketCount`가 잘못되었습니다. 1에서 250 사이의 정수여야 합니다.                         |
| `tecDIR_FULL`             | 이 거래로 인해 계정은 한 번에 250장의 티켓 한도를 초과하거나 일반적으로 최대 원장 개체 수보다 많은 티켓을 소유하게 됩니다. |
| `tecINSUFFICIENT_RESERVE` | 보내는 계정에는 요청된 모든 티켓의 소유자 예비금을 충족할 만큼 충분한 XRP가 없습니다 .                      |