# 입금 승인(Deposit Authorization)

_(DepositAuth 수정안으로 추가됨)_

입금 승인은 XRP Ledger에서 선택적인 [계정](./) 설정입니다. 활성화하면, 입금 승인은 모든 외부로부터의 이체를 차단하게 됩니다. 이는 XRP와 [토큰](../tokens/)의 전송을 포함합니다. 입금 승인이 설정된 계정은 오직 두 가지 방법으로만 자금을 받을 수 있습니다:

* [미리 인증](deposit-authorization.md#undefined-4)한 계정들로부터.
* 자금을 받기 위해 거래를 직접 보내는 방식. 예를 들어, 입금 승인이 설정된 계정은 낯선 사람이 시작한 [에스크로](../undefined-1/undefined-2.md)를 완료할 수 있습니다.&#x20;

기본적으로, 새 계정들은 입금 승인이 비활성화되어 있어 누구든지 XRP를 보낼 수 있습니다.

## 배경 (Background)

금융 서비스 규정과 허가는 사업체나 기관이 받는 모든 거래의 발신자를 알아야 한다는 요구사항이 있을 수 있습니다. 이는 참여자들이 자유롭게 생성할 수 있는 가명으로 식별되며 모든 주소가 다른 주소로 지불할 수 있는 분산 시스템인 XRP Ledger에서는 도전적인 문제입니다.

입금 승인 플래그는 XRP Ledger를 사용하는 사람들이 이러한 규정을 준수하면서 분산 원장의 근본적인 특성을 바꾸지 않는 옵션을 도입합니다. 입금 승인이 활성화된 계정은 계정이 돈을 받게 하는 거래를 보내기 전에 필요한 조사를 수행하여 자금의 발신자를 식별할 수 있습니다.

입금 승인이 활성화되어 있을 때, 당신은 [수표](../undefined-1/undefined-1.md), [에스크로](../undefined-1/undefined-2.md), 그리고 [결제 채널](../undefined-1/undefined-4.md)에서 돈을 받을 수 있습니다. 이러한 거래의 "두 단계" 모델에서는, 먼저 소스가 자금을 보내는 것을 인증하는 거래를 보내고, 그 다음에 목적지가 그 자금을 받는 것을 인증하는 거래를 보냅니다.

입금 승인이 활성화되어 있을 때 [결제 트랜잭션](../../references/xrp-ledger/undefined-1/undefined-1/payment.md)에서 돈을 받으려면, 당신은 그러한 지불의 발신자를 [미리 인증](deposit-authorization.md#undefined-4)해야 합니다. _(_[_DepositPreauth 수정안_](../xrp-ledger/amendments/undefined.md#depositpreauth)_에 의해 추가되었습니다.)_

## 추천 사용법(Recommended Usage)

입금 승인의 전체 효과를 얻으려면, Ripple은 다음과 같이 하는 것을 추천합니다:

* 항상 최소 [reserve requirement](reserves.md)보다 높은 XRP 잔액을 유지하세요.&#x20;
* Default Ripple 플래그를 기본값(비활성화) 상태로 유지하세요. 신뢰선에 [rippling](../tokens/rippling.md)을 활성화하지 마세요. [TrustSet 트랜잭션](../../references/xrp-ledger/undefined-1/undefined-1/trustset.md)을 보낼 때는 항상 [tfSetNoRipple 플래그](../../references/xrp-ledger/undefined-1/undefined-1/trustset.md)를 사용하세요.&#x20;
* [제안](../../references/xrp-ledger/undefined-1/undefined-1/offercreate.md)을 내지 마세요. 어떤 매칭 제안이 거래를 실행하기 위해 소비될지 미리 알 수 없습니다.

## 정확한 의미론(Precise Semantics)

입금 승인이 활성화된 계정:

* [결제 트랜잭션](../../references/xrp-ledger/undefined-1/undefined-1/payment.md)의 목적지가 될 수 **없으며**, **다음과 같은 예외 사항**이 있습니다:
  * 목적지가 지불의 발신자를 미리 인증한 경우. _(_[_DepositPreauth 수정안_](../xrp-ledger/amendments/undefined.md#depositpreauth)_에 의해 추가됨)_
  * 계정의 XRP 잔액이 최소 계정 [reserve requirement](reserves.md)과 같거나 그 이하인 경우, <mark style="background-color:yellow;">금액</mark>이 최소 계정 reserve(현재 10 XRP)와 같거나 작은 XRP 지불의 목적지가 될 수 있습니다. 이는 계정이 거래를 보낼 수 없고 또한 XRP를 받을 수 없는 상태가 되는 것을 방지하기 위함입니다. 이 경우에는 계정의 소유자 reserve가 문제가 되지 않습니다.
* [PaymentChannelClaim 트랜잭션](../../references/xrp-ledger/undefined-1/undefined-1/paymentchannelclaim.md)에서 XRP를 받을 수 있는 경우는 **다음의 경우들 뿐입니다**:
  * PaymentChannelClaim 트랜잭션의 발신자가 결제 채널의 목적지인 경우.
  * PaymentChannelClaim 트랜잭션의 목적지가 PaymentChannelClaim의 발신자를 [미리 인증](deposit-authorization.md#undefined-4)한 경우. _(_[_DepositPreauth 수정안_](../xrp-ledger/amendments/undefined.md)_에 의해 추가됨)_
* [EscrowFinish 트랜잭션](../../references/xrp-ledger/undefined-1/undefined-1/escrowfinish.md)에서 XRP를 받을 수 있는 경우는 다음과 같습니다:
  * EscrowFinish 트랜잭션의 발신자가 에스크로의 목적지인 경우.
  * EscrowFinish 트랜잭션의 목적지가 EscrowFinish의 발신자를 미리 인증한 경우. _(_[_DepositPreauth 수정안_](../xrp-ledger/amendments/undefined.md)_에 의해 추가됨)_
* [CheckCash](../../references/xrp-ledger/undefined-1/undefined-1/checkcash.md) 트랜잭션을 보내서 XRP 또는 토큰을 **받을 수** 있습니다. _(_[_Checks 수정안_](../xrp-ledger/amendments/undefined.md#checks)_에 의해 추가됨)_
* [OfferCreate 트랜잭션](../../references/xrp-ledger/undefined-1/undefined-1/offercreate.md)을 보내서 XRP 또는 토큰을 받을 수 있습니다.
  * 계정이 즉시 완전히 실행되지 않는 OfferCreate 트랜잭션을 보낸 경우, 나중에 다른 계정의 결제및 OfferCreate 트랜잭션이 제안을 소비할 때 나머지 주문된 XRP 또는 토큰을 받을 수 있습니다.
* 계정이 No Ripple 플래그가 활성화되지 않은 신뢰선을 생성했거나, Default Ripple 플래그를 활성화하고 통화를 발행한 경우, 계정은 r ippling의 결과로 이러한 신뢰선의 토큰을 [Payment 트랜잭션](../../references/xrp-ledger/undefined-1/undefined-1/payment.md)에서 받을 수 있습니다. 그러나 그 트랜잭션의 목적지가 될 수는 없습니다.
* 일반적으로 XRP Ledger의 계정은 다음 사항이 모두 해당되는 한 XRP Ledger에서 XRP가 아닌 통화를 받을 수 없습니다.(이 규칙은 DepositAuth 플래그에만 특정한 것은 아닙니다.)
  * 계정에 한도가 0이 아닌 신뢰선이 생성되지 않았습니다.
  * 계정이 다른 사람들이 생성한 신뢰선에서 토큰을 발행하지 않았습니다.
  * 계정이 제안을 내지 않았습니다.

다음 표는 입금 승인이 활성화되거나 비활성화된 경우 트랜잭션 유형이 돈을 입금할 수 있는지를 요약합니다:

| <p><br>DepositAuth Disabled</p> |                                            | DepositAuth 사용             |   |                       |                                                     |                                                     |
| ------------------------------- | ------------------------------------------ | -------------------------- | - | --------------------- | --------------------------------------------------- | --------------------------------------------------- |
| Transaction Type                | 목적지로 전송                                    | 다른 사람이 보냄                  |   | Sent by Destination   | Sent by Others                                      | Sent by Preauthorized Others                        |
| AccountSet                      | (이 거래 유형은 송금하지 않습니다.)                      |                            |   |                       |                                                     |                                                     |
| CheckCancel                     | (이 거래 유형은 송금하지 않습니다.)                      |                            |   |                       |                                                     |                                                     |
| CheckCash                       | OK                                         | 권한 없음                      |   | OK                    | No Permission                                       | No Permission                                       |
| CheckCreate                     | (This transaction type never sends money.) |                            |   |                       |                                                     |                                                     |
| EscrowCancel                    | 만료된 에스크로에서 XRP 반환 가능                       |                            |   |                       |                                                     |                                                     |
| EscrowCreate                    | (이 거래 유형은 XRP를 인출할 수만 있고 입금할 수는 없습니다.)     |                            |   |                       |                                                     |                                                     |
| EscrowFinish                    | OK                                         | OK                         |   | OK                    | No Permission                                       | OK                                                  |
| OfferCancel                     | 이 거래 유형은 송금하지 않습니다.                        |                            |   |                       |                                                     |                                                     |
| OfferCreate                     | OK                                         | 계정이 이전에 매칭 오퍼를 생성한 경우에만 해당 |   | OK                    | Only if account previously created a matching offer | Only if account previously created a matching offer |
| Payment                         | 교차 화폐만 해당                                  | OK                         |   | Cross-currency only 1 | No Permission                                       | OK                                                  |
| Payment                         | 교차 화폐만 해당                                  | OK                         |   | Cross-currency only 1 | XRP payments up to the minimum reserve              | OK                                                  |
| Payment                         | 교차 화폐만 해당                                  | OK                         |   | Cross-currency only 1 | Balance changes from rippling                       | OK                                                  |
| Payment                         | 교차 화폐만 해당                                  | OK                         |   | Cross-currency only 1 | Balance changes from executing offers               | OK                                                  |
| PaymentChannelClaim             | OK                                         | OK                         |   | OK                    | No Permission                                       | OK                                                  |
| PaymentChannelCreate            | (이 거래 유형은 XRP를 인출할 수만 있고 입금할 수는 없습니다.)     |                            |   |                       |                                                     |                                                     |
| PaymentChannelFund              | 직접 만든 채널을 닫을 때 XRP를 반환할 수 있습니다.            |                            |   |                       |                                                     |                                                     |
| SetRegularKey                   | (이 거래 유형은 송금하지 않습니다.)                      |                            |   |                       |                                                     |                                                     |
| SignerListSet                   | (이 거래 유형은 송금하지 않습니다.)                      |                            |   |                       |                                                     |                                                     |
| TrustSet                        | (이 거래 유형은 송금하지 않습니다.)                      |                            |   |                       |                                                     |                                                     |

_1: DepositPreauth 개정안은 계정이 입금 승인을 요구하는 경우 자신에게 이체하는 통화 간 지불이 실패하는 DepositAuth의 버그를 수정합니다. DepositPreauth 개정안이 활성화되지 않은 경우, 이러한 경우는 "No Permission"으로 결과를 나타냅니다._

## 입금 승인 활성화 또는 비활성화(Enabling or Disabling Deposit Authorization)

계정은 <mark style="background-color:yellow;">asfDepositAuth</mark> 값(9)으로 <mark style="background-color:yellow;">SetFlag</mark> 필드를 설정하여 [AccountSet 트랜잭션](../../references/xrp-ledger/undefined-1/undefined-1/accountset.md)을 보내 입금 승인을 활성화 할 수 있습니다. 계정은 <mark style="background-color:yellow;">asfDepositAuth</mark> 값(9)으로 <mark style="background-color:yellow;">ClearFlag</mark> 필드를 설정하여 AccountSet 트랜잭션을 보내 입금 승인을 비활성화 할 수 있습니다. AccountSet 플래그에 대한 자세한 정보는 AccountSet 플래그를 참조하세요.

## 계정이 DepositAuth를 활성화했는지 확인 계정이 입금 승인을 활성화했는지 확인하려면(Checking Whether an Account Has DepositAuth Enabled)

[account\_info 메소드](../../references/http-websocket-apis/api-1/undefined/account\_info.md)를 사용하여 계정을 조회합니다. <mark style="background-color:yellow;">Flags</mark> 필드의 값(<mark style="background-color:yellow;">result.account\_data</mark> 객체 내)을 AccountRoot ledger 객체에 대해 정의된 [비트와이즈 플래그와 비교하세요.](../../references/xrp-ledger/undefined-1/undefined-1/accountset.md)

<mark style="background-color:yellow;">Flags</mark> 값과 <mark style="background-color:yellow;">lsfDepositAuth</mark> 플래그 값(<mark style="background-color:yellow;">0x01000000</mark>)의 비트와이즈 AND 연산 결과가 비 0이면, 계정은 DepositAuth를 활성화했습니다. 결과가 0이면, 계정은 DepositAuth를 비활성화했습니다.

## 미리 인증(Preauthorization)

_(_[_DepositPreauth 수정안_](../../references/xrp-ledger/ledger/ledger-1/depositpreauth.md)_에 의해 추가됨.)_

DepositAuth를 활성화한 계정은 특정 발신자를 _미리 인증_할 수 있어, 입금 승인이 활성화되어 있어도 그러한 발신자로부터의 지불이 성공하게 할 수 있습니다. 이는 특정 발신자가 수신자가 각각의 거래에 대해 개별적으로 조치를 취하지 않고도 직접 자금을 보낼 수 있게 합니다. 미리 인증은 DepositAuth를 사용하는 데 필요하지 않지만, 특정 작업을 더 편리하게 만들 수 있습니다.

미리 인증은 통화에 대해 불가침입니다. 특정 통화에 대해서만 계정을 미리 인증할 수 없습니다.

특정 발신자를 미리 인증하려면, [DepositPreauth 트랜잭션](../../references/xrp-ledger/ledger/ledger-1/depositpreauth.md)을 보내고 <mark style="background-color:yellow;">Authorize</mark> 필드에 미리 인증할 다른 계정의 주소를 입력하세요. 미리 인증을 철회하려면, 대신 <mark style="background-color:yellow;">Unauthorize</mark> 필드에 다른 계정의 주소를 제공하새요. 평소처럼 <mark style="background-color:yellow;">Account</mark> 필드에 자신의 주소를 지정하세요. 현재 DepositAuth를 활성화하지 않은 상태에서도 다른 계정에 대한 미리 인증 상태를 설정하고 저장할 수 있지만, DepositAuth를 활성화하지 않으면 효과가 없습니다. 계정은 자신을 미리 인증할 수 없습니다. 미리 인증은 일방적이며, 반대 방향으로 이동하는 결제에는 영향을 미치지 않습니다.

다른 계정을 미리 인증하면, ledger에 [DepositPreauth 객체](../../references/xrp-ledger/ledger/ledger-1/depositpreauth.md)가 추가되어 인증을 제공하는 계정의 [소유자 reserve](reserves.md)가 증가합니다. 계정이 이 미리 인증을 철회하면, 이 작업은 객체를 제거하고 소유자 reserve를 감소시킵니다.

DepositPreauth 트랜잭션이 처리된 후, 인증된 계정은 입금 승인을 활성화하고 있는 경우에도 다음 트랜잭션 유형 중 하나를 사용하여 자금을 계정에 보낼 수 있습니다:

* [Payment.](../../references/xrp-ledger/undefined-1/undefined-1/payment.md)
* [EscrowFinish. ](../../references/xrp-ledger/undefined-1/undefined-1/escrowfinish.md)
* [PaymentChannelClaim. ](../../references/xrp-ledger/undefined-1/undefined-1/paymentchannelclaim.md)

미리 인증은 DepositAuth가 활성화된 계정에 돈을 보내는 다른 방법에는 영향을 미치지 않습니다. 정확한 규칙에 대해서는 Precise Semantics를 참조하세요.

## 인증 확인(Checking for Authorization)

[deposit\_authorized 메소드를](../../references/http-websocket-apis/api-1/undefined-2/deposit\_authorized.md) 사용하여 계정이 다른 계정에 입금할 수 있는지 인증되어 있는지 확인할 수 있습니다. 이 방법은 두 가지를 확인합니다:&#x20;

* 대상 계정이 입금 인증을 요구하는지 여부. (인증을 요구하지 않는 경우, 모든 소스 계정은 인증된 것으로 간주됩니다.)&#x20;
* 원본 계정이 대상에게 돈을 보낼 수 있는지 미리 인증되었는지 여부.\
