# tem Codes

이러한 코드는 트랜잭션이 잘못되었으며, XRP Ledger 프로토콜에 따라 트랜잭션이 성공할 수 없음을 나타냅니다. 이 코드는 -299에서 -200 범위의 숫자 값을 갖습니다. 특정 오류에 대한 정확한 코드는 변경될 수 있으므로 이 코드에 의존하지 마세요.

{% hint style="info" %}
Tip:

임시 코드가 있는 트랜잭션은 ledger에 적용되지 않으며, XRP Ledger 상태를 변경할 수 없습니다. 유효한 트랜잭션의 규칙이 변경되지 않는 한 임시 결과는 최종 결과입니다. (예를 들어, 수정안이 활성화되기 전에 수정안의 기능을 사용하면 임시 비활성화되며, 나중에 수정안이 활성화되어 유효하게 되면 해당 트랜잭션이 성공할 수 있습니다.)
{% endhint %}

| 암호                            | 설명                                                                                                                                                                                            |
| ----------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `temBAD_AMOUNT`               | 트랜잭션에서 지정한 금액(예: 결제의 대상 금액 또는 SendMax 값)이 음수이기 때문에 유효하지 않습니다.                                                                                                                                 |
| `temBAD_AUTH_MASTER`          | 이 트랜잭션에 서명하는 데 사용된 키가 트랜잭션을 보내는 계정의 마스터 키와 일치하지 않으며, 계정에 일반 키가 설정되어 있지 않습니다.                                                                                                                  |
| `temBAD_CURRENCY`             | 트랜잭션이 통화 필드를 잘못 지정했습니다. 올바른 형식은 통화 금액 지정을 참조하세요.                                                                                                                                              |
| `temBAD_EXPIRATION`           | OfferCreate 트랜잭션의 일부와 같이 트랜잭션이 만료 값을 부적절하게 지정했습니다. 또는 트랜잭션에서 필요한 만료 값을 지정하지 않았습니다(예: EscrowCreate 트랜잭션의 일부).                                                                                  |
| `temBAD_FEE`                  | 트랜잭션이 수수료 값을 부적절하게 지정했습니다(예: XRP가 아닌 통화 또는 음수 금액의 XRP를 나열하는 등).                                                                                                                               |
| `temBAD_ISSUER`               | 트랜잭션이 요청에 포함된 일부 통화의 발행자 필드를 부적절하게 지정했습니다.                                                                                                                                                    |
| `temBAD_LIMIT`                | TrustSet 트랜잭션이 신뢰의 제한 금액 값을 부적절하게 지정했습니다.                                                                                                                                                     |
| `temBAD_NFTOKEN_TRANSFER_FEE` | NFTokenMint 트랜잭션이 트랜잭션의 TransferFee 필드를 잘못 지정했습니다. (NonFungibleTokenV1\_1 수정안에 의해 추가되었습니다.)                                                                                                   |
| `temBAD_OFFER`                | OfferCreate 트랜잭션이 XRP를 직접 거래하겠다고 제안하거나 마이너스 금액을 제안하는 등 잘못된 오퍼를 지정했습니다.                                                                                                                        |
| `temBAD_PATH`                 | 결제 트랜잭션이 하나 이상의 경로를 부적절하게 지정합니다(예: XRP 발행자를 포함하거나 계정을 다르게 지정).                                                                                                                                |
| `temBAD_PATH_LOOP`            | 결제 트랜잭션의 경로 중 하나가 루프로 플래그가 지정되었으므로 제한된 시간 내에 처리할 수 없습니다.                                                                                                                                      |
| `temBAD_SEND_XRP_LIMIT`       | XRP에서 XRP로 직접 결제하는 경우 전환이 포함되지 않음에도 불구하고 결제 트랜잭션에서 tfLimitQuality 플래그를 사용했습니다.                                                                                                                |
| `temBAD_SEND_XRP_MAX`         | 결제 트랜잭션이 XRP를 송금할 때 SendMax가 필요하지 않음에도 불구하고 XRP에서 XRP로 직접 결제에 SendMax 필드를 포함했습니다. (목적지 금액이 XRP가 아닌 경우에만 SendMax에서 XRP가 유효합니다.)                                                                |
| `temBAD_SEND_XRP_NO_DIRECT`   | 결제 트랜잭션은 XRP에서 XRP로 직접 결제하는 것이 항상 직접 결제임에도 불구하고 tfNoDirectRipple 플래그를 사용했습니다.                                                                                                                 |
| `temBAD_SEND_XRP_PARTIAL`     | 결제 트랜잭션은 항상 전체 금액을 전달해야 함에도 불구하고 XRP에서 XRP로 직접 결제할 때 tfPartialPayment 플래그를 사용했습니다.                                                                                                            |
| `temBAD_SEND_XRP_PATHS`       | 결제 트랜잭션이 XRP를 전송하는 동안 경로를 포함했습니다.                                                                                                                                                             |
| `temBAD_SEQUENCE`             | 트랜잭션이 자체 시퀀스 번호보다 높은 시퀀스 번호를 참조합니다(예: 오퍼를 취소하는 트랜잭션 이후에 진행해야 하는 오퍼를 취소하려고 하는 경우).                                                                                                             |
| `temBAD_SIGNATURE`            | 이 트랜잭션을 승인하는 서명이 누락되었거나 제대로 형성되지 않은 방식으로 형성되었습니다. (서명이 제대로 형성되었지만 이 계정에 대해 권한이 부여되지 않은 경우는 tecNO\_PERMISSION을 참조하세요.) temBAD\_SRC\_ACCOUNT 이 트랜잭션을 대신 전송하는 계정('소스 계정')이 올바르게 형성된 계정 주소가 아닙니다. |
| `temBAD_SRC_ACCOUNT`          | 계정 세트 트랜잭션의 TransferRate 필드의 형식이 올바르지 않거나 허용 범위를 벗어났습니다.                                                                                                                                      |
| `temBAD_TRANSFER_RATE`        | 계정 세트 트랜잭션의 TransferRate 필드의 형식이 올바르지 않거나 허용 범위를 벗어났습니다.                                                                                                                                      |
| `temCANNOT_PREAUTH_SELF`      | 예치 사전 승인 거래의 발신자가 사전 승인할 계정으로 지정되었습니다. 자신을 사전 승인할 수 없습니다.                                                                                                                                     |
| `temDST_IS_SRC`               | 트랜잭션이 대상 주소를 트랜잭션을 보내는 계정으로 부적절하게 지정했습니다. 여기에는 트러스트 라인(대상 주소가 한도 금액의 발급자 필드인 경우) 및 결제 채널(대상 주소가 목적지 필드인 경우)이 포함됩니다.                                                                           |
| `temDST_NEEDED`               | 트랜잭션에 목적지가 부적절하게 누락되었습니다. 결제 트랜잭션의 목적지 필드이거나 트러스트 세트 트랜잭션의 한도액 필드의 발급자 하위 필드일 수 있습니다.                                                                                                         |
| `temINVALID`                  | 그렇지 않으면 거래가 유효하지 않습니다. 예를 들어 거래 ID의 형식이 올바르지 않거나 서명이 제대로 구성되지 않았거나 거래를 이해하는 데 문제가 있을 수 있습니다.                                                                                                  |
| `temINVALID_COUNT`            | 거래에 `TicketCount`필드가 포함되어 있지만 지정된 티켓 수가 잘못되었습니다.                                                                                                                                              |
| `temINVALID_FLAG`             | 거래에 존재하지 않는 플래그가 포함되어 있거나 플래그의 모순된 조합이 포함되어 있습니다.                                                                                                                                             |
| `temMALFORMED`                | 거래 형식에 명시되지 않은 문제가 있습니다.                                                                                                                                                                      |
| `temREDUNDANT`                | 거래는 아무 것도 하지 않습니다. 예를 들어 송금 계좌로 직접 지불금을 보내거나 동일한 발행자로부터 동일한 통화를 사고 파는 제안을 생성하는 것입니다.                                                                                                          |
| `temREDUNDANT_SEND_MAX`       | [![제거 위치: 잔물결 0.28.0](https://img.shields.io/badge/Removed%20in-rippled%200.28.0-red.svg) ](https://github.com/ripple/rippled/releases/tag/0.28.0)                                            |
| `temRIPPLE_EMPTY`             | 결제 [거래에는](https://xrpl.org/payment.html) 빈 `Paths`필드가 포함되어 있지만 이 결제를 완료하려면 경로가 필요합니다.                                                                                                         |
| `temBAD_WEIGHT`               | SignerListSet [트랜잭션에는](https://xrpl.org/signerlistset.html)`SignerWeight` 잘못된 값(예: 0 또는 음수 값)이 포함되어 있습니다.                                                                                     |
| `temBAD_SIGNER`               | SignerListSet [트랜잭션에](https://xrpl.org/signerlistset.html) 유효하지 않은 서명자가 포함되어 있습니다. 예를 들어 중복된 항목이 있거나 SignerList의 소유자가 구성원일 수도 있습니다.                                                           |
| `temBAD_QUORUM`               | SignerListSet [트랜잭션에](https://xrpl.org/signerlistset.html) 잘못된 `SignerQuorum`값이 있습니다. 값이 0보다 크지 않거나 목록에 있는 모든 서명자의 합계보다 큽니다.                                                                  |
| `temUNCERTAIN`                | 내부적으로만 사용됩니다. 이 코드는 절대로 반환되어서는 안 됩니다.                                                                                                                                                         |
| `temUNKNOWN`                  | 내부적으로만 사용됩니다. 이 코드는 절대로 반환되어서는 안 됩니다.                                                                                                                                                         |
| `temDISABLED`                 | 트랜잭션에는 비활성화된 논리가 필요합니다. 일반적으로 이는 현재 원장에 대해 활성화되지 않은 [수정 사항을](https://xrpl.org/amendments.html) 사용하려고 함을 의미합니다.                                                                                |
