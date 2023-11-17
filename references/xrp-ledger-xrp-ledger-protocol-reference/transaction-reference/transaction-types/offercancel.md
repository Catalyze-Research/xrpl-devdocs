# OfferCancel

제안 취소 트랜잭션은 XRP Ledger에서 제안 객체를 제거합니다.

## OfferCancel JSON 예시

```
{
    "TransactionType": "OfferCancel",
    "Account": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
    "Fee": "12",
    "Flags": 0,
    "LastLedgerSequence": 7108629,
    "OfferSequence": 6,
    "Sequence": 7
}
```

## OfferCancel 필드

OfferCancel 트랜잭션은 일반적인 필드 외에도 다음 필드를 사용합니다:

| 필드              | JSON 유형 | [내부 유형](https://xrpl.org/serialization.html) | 설명                                                                                                                   |
| --------------- | ------- | -------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| `OfferSequence` | 숫자      | UInt32                                       | 이전 OfferCreate 트랜잭션의 시퀀스 번호(또는 티켓 번호)입니다. 지정한 경우 해당 트랜잭션에 의해 생성된 원장의 오퍼 개체를 취소합니다. 지정된 오퍼가 존재하지 않는 경우 오류로 간주되지 않습니다. |

{% hint style="info" %}
Tip:

기존 제안을 제거하고 새 제안으로 바꾸려면 OfferCancel과 다른 [OfferCreate](https://xrpl.org/offercreate.html)를 사용하는 대신 OfferSequence 매개변수가 있는 OfferCreate 트랜잭션을 사용할 수 있습니다.
{% endhint %}

OfferCancel 메소드는 일치하는 시퀀스 번호가 있는 제안을 찾지 못한 경우에도 tesSUCCESS를 반환합니다.
