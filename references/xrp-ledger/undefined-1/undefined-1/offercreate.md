# OfferCreate

OfferCreate 트랜잭션은 탈중앙화 거래소에 제안을 배치합니다.

## OfferCreate JSON 예시

```
{
    "TransactionType": "OfferCreate",
    "Account": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
    "Fee": "12",
    "Flags": 0,
    "LastLedgerSequence": 7108682,
    "Sequence": 8,
    "TakerGets": "6000000",
    "TakerPays": {
      "currency": "GKO",
      "issuer": "ruazs5h1qEsqpke88pcqnaseXdm6od2xc",
      "value": "2"
    }
}
```

OfferCreate 필드

OfferCreate 트랜잭션은 일반적인 필드 외에도 다음 필드를 사용합니다:

| 필드                                                          | JSON 유형                                                                     | [내부 유형](https://xrpl.org/serialization.html) | 설명                                                                                                            |
| ----------------------------------------------------------- | --------------------------------------------------------------------------- | -------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| [Expiration](https://xrpl.org/offers.html#offer-expiration) | 숫자                                                                          | UInt32                                       | _(선택 사항)_ [Ripple 에포크 이후](https://xrpl.org/basic-data-types.html#specifying-time) 제안이 더 이상 활성화되지 않는 시간(초)입니다. |
| OfferSequence                                               | 숫자                                                                          | UInt32                                       | _(선택 사항)_ 먼저 삭제할 제안으로, [OfferCancel](https://xrpl.org/offercancel.html)과 동일한 방식으로 지정됩니다 .                     |
| TakerGets                                                   | [화폐 금액](https://xrpl.org/basic-data-types.html#specifying-currency-amounts) | 금액                                           | 판매되는 화폐의 금액 및 유형.                                                                                             |
| TakerPays                                                   | [화폐 금액](https://xrpl.org/basic-data-types.html#specifying-currency-amounts) | 금액                                           | 구매되는 화폐의 금액과 종류.                                                                                              |

## OfferCreate 플래그

OfferCreate 유형의 트랜잭션은 다음과 같이 플래그 필드에 추가 값을 지원합니다:

| 플래그 이름              | 16진수 값     | 소수점 값  | 설명                                                                                                                                                                                           |
| ------------------- | ---------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| tfPassive           | 0x00010000 | 65536  | 이 옵션을 활성화하면 오퍼는 정확히 일치하는 오퍼를 소비하지 않고 대신 ledger의 오퍼 개체가 됩니다. 여전히 교차하는 오퍼는 소비합니다.                                                                                                              |
| tfImmediateOrCancel | 0x00020000 | 131072 | 오퍼를 즉시 또는 취소 주문으로 처리합니다. 오퍼는 ledger에 오퍼 객체를 생성하지 않으며, 트랜잭션이 처리될 때 기존 오퍼를 소비하여 거래할 수 있는 만큼만 거래합니다. 일치하는 오퍼가 없으면 아무것도 거래하지 않고 "성공적으로" 실행됩니다. 이 경우에도 트랜잭션은 여전히 결과 코드 tesSUCCESS를 사용합니다.         |
| tfFillOrKill        | 0x00040000 | 262144 | 오퍼를 채우기 또는 죽이기 주문으로 처리합니다. 오퍼는 ledger에 오퍼 객체를 생성하지 않으며, 실행 시점에 완전히 채울 수 없는 경우 취소됩니다. 기본적으로 이는 소유자가 전체 TakerPays 금액을 받아야 함을 의미하며, tfSell 플래그가 활성화된 경우 소유자는 전체 TakerGets 금액을 대신 사용할 수 있어야 합니다. |
| tfSell              | 0x00080000 | 524288 | TakerPays 금액보다 더 많은 금액을 교환하는 것을 의미하더라도 전체 TakerGets 금액을 교환합니다.                                                                                                                               |

## 오류 사례

| 에러 코드                    | 설명                                                                                                                                                                                                          |
| ------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| temINVALID\_FLAG         | 트랜잭션이 tfImmediateOrCancel과 tfFillOrKill을 모두 지정한 경우 발생합니다.                                                                                                                                                   |
| tecEXPIRED               | 트랜잭션이 이미 경과한 만료 시간을 지정할 때 발생합니다.                                                                                                                                                                            |
| tecKILLED                | 트랜잭션이 tfFillOrKill을 지정하고 전체 금액을 채울 수 없는 경우 발생합니다. 즉시 오퍼킬 수정이 활성화된 경우, 트랜잭션이 tfImmediateOrCancel을 지정하고 자금을 이동하지 않고 실행될 때도 이 결과 코드가 발생합니다(이전에는 tesSUCCESS를 반환했습니다).                                           |
| temBAD\_EXPIRATION       | 트랜잭션에 유효하지 않은 형식의 만료 필드가 포함된 경우 발생합니다.                                                                                                                                                                      |
| temBAD\_SEQUENCE         | 트랜잭션에 형식이 올바르지 않거나 트랜잭션의 자체 시퀀스 번호보다 높은 OfferSequence가 포함된 경우 발생합니다.                                                                                                                                        |
| temBAD\_OFFER            | 오퍼가 XRP를 XRP로 거래하려고 하거나 유효하지 않거나 마이너스 금액의 토큰을 거래하려고 할 때 발생합니다.                                                                                                                                              |
| temREDUNDANT             | 트랜잭션이 동일한 토큰(동일한 발행자 및 통화 코드)에 대한 토큰을 지정하는 경우 발생합니다.                                                                                                                                                        |
| temBAD\_CURRENCY         | 트랜잭션이 통화 코드가 "XRP"인 토큰을 지정할 때 발생합니다.                                                                                                                                                                        |
| temBAD\_ISSUER           | 트랜잭션이 발행자 값이 ledger에 자금이 예치된 계정이 아닌 토큰을 지정할 때 발생합니다.                                                                                                                                                        |
| tecNO\_ISSUER            | 거래가 issuerledger의 자금 조달 계정이 아닌 토큰을 지정하는 경우 발생합니다.                                                                                                                                                           |
| tecFROZEN                | 트랜잭션이 동결된 신뢰선(로컬 및 글로벌 동결 포함)에 있는 토큰을 포함하는 경우 발생합니다.                                                                                                                                                        |
| tecUNFUNDED\_OFFER       | 소유자가 양수의 TakerGets 통화를 보유하지 않은 경우 발생합니다. (예외: TakerGets가 소유자가 발행하는 토큰을 지정하면 트랜잭션이 성공할 수 있습니다.)                                                                                                              |
| tecNO\_LINE              | 발행자가 승인된 신뢰선을 사용하는 토큰이 포함되어 있고 필요한 신뢰선이 존재하지 않는 경우 발생합니다.                                                                                                                                                   |
| tecNO\_AUTH              | 트랜잭션에 발행자가 승인된 신뢰선을 사용하는 토큰이 포함되고 토큰을 받을 신뢰선이 존재하지만 승인되지 않은 경우 발생합니다.                                                                                                                                       |
| tecINSUF\_RESERVE\_OFFER | 소유자가 새 오퍼 객체를 ledger에 추가하기 위한 reserve requirement을 충족하기에 충분한 XRP를 보유하고 있지 않고 거래에서 통화를 변환하지 않은 경우 발생합니다. (트랜잭션이 어떤 금액이든 성공적으로 거래한 경우, 결과 코드가 tesSUCCESS인 트랜잭션은 성공하지만 나머지 금액에 대해서는 ledger에 오퍼 객체를 생성하지 않습니다). |
| tecDIR\_FULL             | 소유자가 ledger에 너무 많은 아이템을 소유하고 있거나 오더북에 이미 동일한 환율의 오퍼가 너무 많이 포함되어 있는 경우 발생합니다.                                                                                                                                |

&#x20;
