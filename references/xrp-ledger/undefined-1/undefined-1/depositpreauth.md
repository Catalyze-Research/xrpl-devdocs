# DepositPreauth

[_DepositPreauth_](https://xrpl.org/known-amendments.html#depositpreauth) _수정안으로 추가되었습니다._

입금 전 승인 트랜잭션은 다른 계정이 이 트랜잭션의 발신자에게 송금을 전달할 수 있도록 사전 승인을 부여합니다. 이 기능은 트랜잭션 발신자가 입금 승인을 사용 중이거나 사용할 계획인 경우에만 유용합니다.

{% hint style="info" %}
Tip:

입금 승인을 사용하도록 설정하기 전에 이 트랜잭션을 사용하여 특정 트랜잭션 상대방을 사전 승인할 수 있습니다. 이는 입금 승인이 필요하지 않은 상태에서 필요한 상태로 원활하게 전환하는 데 유용할 수 있습니다.
{% endhint %}

## DepositPreauth JSON 예시

```
{
  "TransactionType" : "DepositPreauth",
  "Account" : "rsUiUMpnrgxQp24dJYZDhmV4bE3aBtQyt8",
  "Authorize" : "rEhxGqkqPPSxQ3P25J66ft5TwpzV14k2de",
  "Fee" : "10",
  "Flags" : 2147483648,
  "Sequence" : 2
}
```

## DepositPreauth 필드

일반적인 필드 외에도 DepositPreauth 트랜잭션은 다음 필드를 사용합니다:

| 필드            | JSON 유형 | [내부 유형](https://xrpl.org/serialization.html) | 설명                                            |
| ------------- | ------- | -------------------------------------------- | --------------------------------------------- |
| `Authorize`   | 문자열     | 계정 ID                                        | (선택 사항) 사전 승인할 발신자의 XRP Ledger 주소입니다.         |
| `Unauthorize` | 문자열     | 계정 ID                                        | (선택 사항) 사전 승인을 취소해야 하는 발신자의 XRP Ledger 주소입니다. |

Authorize 또는 Unauthorize 중 하나를 제공해야 하지만 둘 다 제공하면 안 됩니다.

이 트랜잭션에는 다음과 같은 제한 사항이 있습니다:

* 계정은 자신의 주소를 사전 승인(또는 승인 취소)할 수 없습니다. 사전 승인 시도가 실패하면 temCANNOT\_PREAUTH\_SELF 결과가 반환됩니다.
* 이미 사전 인증된 계정을 사전 인증하려고 시도하면 tecDUPLICATE라는 결과가 표시됩니다.
* 사전 승인되지 않은 계정에 대한 사전 승인 취소 시도에 실패하여 tecNO\_ENTRY 결과가 반환됩니다.
* ledger에 자금이 입금되지 않은 주소를 사전 승인하려고 시도하면 tecNO\_TARGET 결과가 표시됩니다.
* 승인을 추가하면 소유자 준비금 요건에 포함되는 DepositPreauth 객체가 ledger에 추가됩니다. 트랜잭션 발신자가 증가된 준비금을 지불하기에 충분한 XRP를 보유하고 있지 않은 경우, 트랜잭션은 tecINSUFFICIENT\_RESERVE라는 결과와 함께 실패합니다. 계정 발신자가 이미 최대 소유 개체 수에 도달한 경우 트랜잭션은 tecDIR\_FULL이라는 결과와 함께 실패합니다.
