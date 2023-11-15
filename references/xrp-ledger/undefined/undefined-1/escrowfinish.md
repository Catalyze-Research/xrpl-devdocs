# EscrowFinish

_에스크로 수정안에 의해 추가되었습니다._

보류된 결제에서 수취인에게 XRP를 전달합니다.

## EscrowFinish JSON 예시

```
{
    "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "TransactionType": "EscrowFinish",
    "Owner": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "OfferSequence": 7,
    "Condition": "A0258020E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855810100",
    "Fulfillment": "A0028000"
}
```

## EscrowFinish 필드

일반적인 필드 외에도 EscrowFinish 트랜잭션은 다음 필드를 사용합니다:

| 필드              | JSON 유형  | 내부 유형  | 설명                                                                                                            |
| --------------- | -------- | ------ | ------------------------------------------------------------------------------------------------------------- |
| `Owner`         | 문자열      | 계정 ID  | 보류된 지불금을 입금한 소스 계정의 주소입니다.                                                                                    |
| `OfferSequence` | 부호 없는 정수 | UInt32 | 보류된 지불을 생성한 EscrowCreate 트랜잭션의 트랜잭션 순서가 완료됩니다.                                                                |
| `Condition`     | 문자열      | blob   | (선택 사항) 이전에 제공한 보류된 지불의 PREIMAGE-SHA-256 암호화 조건과 일치하는 16진수 값입니다.                                              |
| `Fulfillment`   | 문자열      | blob   | (선택 사항) 보류된 결제의 <mark style="background-color:yellow;">조건</mark>과 일치하는 PREIMAGE-SHA-256 암호화 조건 이행의 16진수 값입니다. |

모든 계정이 EscrowFinish 트랜잭션을 제출할 수 있습니다.

* 보류된 결제에 FinishAfter 시간이 있는 경우, 이 시간 이전에는 결제를 실행할 수 없습니다. 특히, 해당 EscrowCreate 트랜잭션이 가장 최근에 마감한 ledger의 마감 시간 이후인 FinishAfter 시간을 지정한 경우 EscrowFinish 트랜잭션은 실패합니다.
* 보류된 결제에 조건이 있는 경우 조건에 일치하는 주문 처리를 제공하지 않으면 보류된 결제를 실행할 수 없습니다.
* 보류된 결제는 만료된 후에는 실행할 수 없습니다. 특히, 해당 EscrowCreate 트랜잭션이 가장 최근에 마감된 ledger의 마감 시간 이전인 CancelAfter 시간을 지정한 경우 EscrowFinish 트랜잭션은 실패합니다.

{% hint style="info" %}
Note:

트랜잭션에 주문 처리가 포함된 경우 EscrowFinish 트랜잭션을 제출하는 데 드는 최소 트랜잭션 비용이 증가합니다. 트랜잭션에 주문 처리가 없는 경우 트랜잭션 비용은 표준 10드롭입니다. 트랜잭션에 주문 처리가 포함된 경우 트랜잭션 비용은 330드롭에 프리이미지 크기 16바이트당 10드롭이 추가됩니다.
{% endhint %}

비프로덕션 네트워크에서는 보류 중인 에스크로의 대상 계정을 삭제할 수 있습니다. 이 경우 에스크로를 완료하려는 시도는 tecNO\_TARGET 결과로 실패하지만 에스크로 객체는 정상적으로 만료되지 않는 한 남아 있습니다. 다른 결제로 대상 계좌가 다시 생성되면 에스크로가 성공적으로 완료될 수 있습니다. 에스크로의 대상 계정은 수정안사항 1523이 활성화되기 전에 에스크로가 생성된 경우에만 삭제할 수 있습니다. 프로덕션 XRP 레저에는 이러한 에스크로가 존재하지 않으므로 프로덕션 XRP 레저에서는 이 에지 케이스가 불가능합니다. 이 에지 케이스는 새로운 제네시스 ledger를 시작할 때 기본값인 fix1523과 에스크로 수정안을 동시에 활성화하는 테스트 네트워크에서도 불가능합니다.

&#x20;
