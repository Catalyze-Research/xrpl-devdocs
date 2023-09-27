# NFTokenCancelOffer

설NFTokenCancelOffer 트랜잭션은 NFTokenCreateOffer를 사용하여 생성된 기존 토큰 제안을 취소하는 데 사용할 수 있습니다.

_(NonFungibleTokenV1\_1 수정안에 의해 추가되었습니다.)_

### NFTokenCancelOffer JSON 예시

```
{
    "TransactionType": "NFTokenCancelOffer",
    "Account": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
    "NFTokenOffers": [
      "9C92E061381C1EF37A8CDE0E8FC35188BFC30B1883825042A64309AC09F4C36D"
    ]
}
```

## Permissions

NFTokenOffer 객체로 표현되는 기존 제안을 취소할 수 있는 대상은 다음과 같습니다:

* 원래 NFTokenOffer를 생성한 계정.
* NFTokenOffer의 대상 필드에 계정이 있는 경우 해당 계정.
* NFTokenOffer가 만료 시간을 지정하고 NFTokenCancelOffer가 포함된 부모 ledger의 마감 시간이 만료 시간보다 큰 경우, 모든 계정.

이 트랜잭션은 ledger에 나열된 NFTokenOffer 객체가 있는 경우 이를 제거하고 그에 따라 reserve requirements을 조정합니다. NFTokenOffer를 찾을 수 없는 것은 오류가 아니며, 이 경우 트랜잭션이 성공적으로 완료되어야 합니다.

## NFTokenCancelOffer 필드

일반적인 필드 외에도 NFTokenCancelOffer 트랜잭션은 다음 필드를 사용합니다:

| 필드              | JSON 유형 | 내부 유형 | 설명                                                                                                                                              |
| --------------- | ------- | ----- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| `NFTokenOffers` | 배열      | 벡터256 | 취소할 NFTokenOffer 오퍼 객체의 ID 배열(NFToken 객체의 ID가 아니라 NFToken 오퍼 객체의 ID). 각 항목은 NFTokenOffer 오퍼 객체의 다른 객체 ID여야 하며, 배열에 중복된 항목이 있으면 트랜잭션이 유효하지 않습니다. |

NFTokenOffers 필드에 있는 하나 이상의 ID가 현재 ledger에 존재하는 객체를 참조하지 않더라도 트랜잭션은 성공할 수 있습니다. (예를 들어, 해당 토큰 제안이 이미 삭제되었을 수 있습니다.) ID 중 하나가 존재하지만 NFTokenOffer 개체가 아닌 객체를 가리키면 트랜잭션은 오류와 함께 실패합니다.

실수로 nft\_offer\_index가 아닌 nft\_id를 제공하면 tesSUCCESS 응답을 받을 수 있다는 점에 유의해야 합니다. 그 이유는 찾을 수 없는 올바른 형식의 ID 값을 전달하면 시스템에서 NFTokenOffer가 이미 삭제된 것으로 간주하기 때문입니다.

ID 중 하나가 존재하지만 NFTokenOffer 객체가 아닌 객체를 가리키면 트랜잭션은 오류와 함께 실패합니다.

## 오류 사례

모든 트랜잭션에서 발생할 수 있는 오류 외에도 NFTokenCancelOffer 트랜잭션은 다음과 같은 트랜잭션 결과 코드를 발생시킬 수 있습니다:

| 에러 코드              | 설명                                                                                                                                        |
| ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `temDISABLED`      | NonFungibleTokensV1 수정안이 활성화되지 않았습니다.                                                                                                     |
| `temMALFORMED`     | 트랜잭션이 유효하게 포맷되지 않았습니다. 예를 들어 `NFTokenOffers`배열이 비어 있거나 한 번에 취소할 수 있는 최대 제안 수보다 많은 제안이 포함되어 있습니다.                                          |
| `tecNO_PERMISSION` | 필드 에 있는 ID 중 하나 이상이 `NFTokenOffers`취소할 수 없는 개체를 나타냅니다. 예를 들어, 이 거래의 보낸 사람이 소유자나 `Destination`제안의 소유자가 아니거나 개체가 `NFTokenOffer`유형 개체가 아닙니다. |
