# 수표

수표 기능은 개인용 종이 수표와 유사한 연기된 결제를 생성할 수 있게 해줍니다. 에스크로나 결제 채널과 달리 수표를 생성할 때 자금이 따로 설정되지 않으므로 수표가 현금화될 때만 돈이 이동합니다. 수표를 현금화할 때 송신자에게 자금이 없으면 트랜잭션은 실패합니다; 수령인은 수표가 만료될 때까지 실패한 트랜잭션을 다시 시도할 수 있습니다.

XRP Ledger 수표는 더 이상 현금화될 수 없는 만료 시간을 가질 수 있습니다. 수령인이 만료되기 전에 수표를 성공적으로 현금화하지 않으면 수표는 더 이상 현금화될 수 없지만, 누군가 그것을 취소하기 전까지 XRP Ledger에 객체로 남아 있습니다. 누구나 만료된 후에 수표를 취소할 수 있습니다. 만료되기 전에는 송신자와 수령인만 수표를 취소할 수 있습니다. 수표를 성공적으로 현금화하거나 누군가 그것을 취소하면 Ledger에서 수표 객체가 제거됩니다.

## 사용 사례

* 수표는 은행 업계에서 친숙하게 알고 있고 받아들여지는 과정을 사용하여 자금을 교환하게 해줍니다.
* 당신의 예상 수령인이 낯선 사람들로부터의 직접 결제를 차단하기 위해 [입금 승인](<../undefined-2/undefined-3 (1).md>)을 사용한다면, 수표는 좋은 대안입니다.
* 유연한 수표 현금화. 수령인은 최소 금액과 최대 금액 사이에서 수표를 인출할 수 있습니다.

## 수표 수명주기

1. 송신자는 [CheckCreate 트랜잭션](../../references/xrp-ledger/undefined-1/undefined-1/checkcreate.md)을 보냅니다, 이것은 다음을 정의합니다:

* 수령인.
* 만료 날짜.
* 그들의 계정에서 인출될 수 있는 최대 금액.&#x20;

2. 트랜잭션이 처리될 때, XRP Ledger는 <mark style="background-color:yellow;">수표</mark> 객체를 생성합니다. 수표는 송신자나 수신자가 [CheckCancel 트랜잭션](../../references/xrp-ledger/undefined-1/undefined-1/checkcancel.md)으로 취소할 수 있습니다.
3. 수령인은 자금을 이체하고 <mark style="background-color:yellow;">수표</mark> 객체를 파괴하는 [CheckCash 트랜잭션](../../references/xrp-ledger/undefined-1/undefined-1/checkcash.md)을 제출합니다. 수령인은 수표를 현금화하는 데 두 가지 옵션이 있습니다:

* 정확한 금액: 그들은 수표 최대 금액을 초과하지 않는 정확한 금액을 현금화하도록 지정합니다.
* 유연한 금액: 그들은 현금화할 최소 금액을 지정하고 XRP Ledger는 수표 최대 금액까지 가능한 한 많은 금액을 전달합니다. 송신자가 지정된 최소 금액을 적어도 충족시키기 위한 자금을 보유하고 있지 않으면 트랜잭션은 실패합니다.&#x20;

4. 수령인이 수표를 현금화하기 전에 수표가 만료되면 <mark style="background-color:yellow;">수표</mark> 객체는 누군가 그것을 취소할 때까지 남아 있습니다.

## 참고자료

For more information about Checks in the XRP Ledger, see:

* [Transaction Reference](https://xrpl.org/transaction-types.html)
  * [CheckCreate](https://xrpl.org/checkcreate.html)
  * [CheckCash](https://xrpl.org/checkcash.html)
  * [CheckCancel](https://xrpl.org/checkcancel.html)
* [Checks Tutorials](https://xrpl.org/use-checks.html)
  * [Send a Check](https://xrpl.org/send-a-check.html)
  * [Look up Checks by sender address](https://xrpl.org/look-up-checks-by-sender.html)
  * [Look up Checks by recipient address](https://xrpl.org/look-up-checks-by-recipient.html)
  * [Cash a Check for an exact amount](https://xrpl.org/cash-a-check-for-an-exact-amount.html)
  * [Cash a Check for a flexible amount](https://xrpl.org/cash-a-check-for-a-flexible-amount.html)
  * [Cancel a Check](https://xrpl.org/cancel-a-check.html)
* [Checks amendment](https://xrpl.org/known-amendments.html#checks)

For more information about related features, see:

* [Deposit Authorization](https://xrpl.org/depositauth.html)
* [Escrow](https://xrpl.org/escrow.html)
* [Payment Channels Tutorial](https://xrpl.org/use-payment-channels.html)



\---

수표는 [에스크로](undefined-2.md)와 [결제 채널](undefined-4.md)과 몇 가지 유사점이 있지만, 이러한 기능과 수표 사이에 중요한 차이점이 있습니다:

* 수표를 사용하여 [토큰](../tokens/)을 보낼 수 있습니다. 결제 채널과 에스크로에서는 XRP만 보낼 수 있습니다.
* 수표는 어떠한 자금도 잠기거나 보류하지 않습니다. 결제 채널과 에스크로에서 사용되는 XRP는 송신자가 제공하는 클레임으로 인출되기 전까지(결제 채널) 또는 만료 또는 암호 조건(에스크로)이 충족되기 전까지 사용할 수 없습니다.
* 에스크로를 통해 자신에게 XRP를 보낼 수 있습니다. 그러나 수표는 자신에게는 보낼 수 없습니다.

{% hint style="info" %}
Note:&#x20;

[수표 수정안](../xrp-ledger/amendments/undefined.md)은 [OfferCreate](../../references/xrp-ledger/undefined-1/undefined-1/offercreate.md) 트랜잭션의 만료 동작을 변경합니다. 자세한 내용은 [제안 만료](../tokens/decentralized-exchange/offers.md#undefined-6)를 참조하세요.
{% endhint %}

## 수표의 필요성

전통적인 종이 수표는 현금을 즉시 교환하지 않고 자금을 이동시킬 수 있게 합니다. XRP Ledger 수표는 은행 업계에서 친숙하고 인정받는 프로세스를 통해 사람들이 비동기적으로 자금을 교환할 수 있도록 합니다.

XRP Ledger 수표는 또한 XRP Ledger에만 있는 문제를 해결합니다. 원하지 않는 지불을 거부하거나 지불의 일부만 수락할 수 있는 기능을 제공하여 규정 준수에 신경을 써야 하는 기관에 유용합니다.

## 사용 사례: 지불 승인

**문제:** [BSA, KYC, AML 및 CFT](../../tutorials/xrp-ledger/undefined.md)와 같은 규정을 준수하기 위해 금융 기관은 받은 자금의 원천에 대한 문서를 제공해야 합니다. 이러한 규정은 기관이 처리하는 모든 지불의 원천과 목적지를 알고자 하여 불법 자금 이체를 방지하기 위한 것입니다. XRP Ledger의 특성 상, 누구든지 잠재적으로 XRP(그리고 적절한 조건하에 토큰)를 기관의 XRP Ledger 계정으로 송금할 수 있습니다. 이러한 원하지 않는 지불을 처리하는 것은 이러한 기관의 규정 준수 부서에 상당한 비용과 시간 지연을 유발하며, 잠재적으로 벌금이나 처벌을 받을 수도 있습니다.

**해결책:** 기관은 [AccountSet 거래에서 <mark style="background-color:yellow;">asfDepositAuth</mark> 플래그를 설정](../../references/xrp-ledger/undefined-1/undefined-1/accountset.md)하여 XRP Ledger 계정에서 [입금 승인](<../undefined-2/undefined-3 (1).md>)을 활성화할 수 있습니다. 이렇게 하면 해당 계정은 결제 거래를 통해 자금을 받을 수 없게 됩니다. 예금 승인이 활성화된 계정은 에스크로, 결제 채널, 또는 수표를 통해 자금을 받을 수 있습니다. 수표는 예금 승인이 활성화된 경우 자금을 이동시키는 가장 직관적이고 유연한 방법입니다.

## 사용 방법

수표는 일반적으로 아래에 설명된 생명주기를 따릅니다.

<figure><img src="https://xrpl.org/img/checks-happy-path.png" alt=""><figcaption></figcaption></figure>

**1단계:** 수표를 생성하기 위해 송신자는 CheckCreate 거래를 제출하고 수령인(<mark style="background-color:yellow;">Destination</mark>), 만료 시간(<mark style="background-color:yellow;">Expiration</mark>), 송신자의 계정에서 인출될 수 있는 최대 금액(<mark style="background-color:yellow;">SendMax</mark>)을 지정합니다.

**2단계:** CheckCreate 거래가 처리된 후, XRP Ledger에 [Check 객체](../../references/xrp-ledger/ledger/ledger-1/check.md)가 생성됩니다. 이 객체는 생성한 거래에서 정의된 Check의 속성을 포함합니다. 만료 시간이 경과하기 전까지는 송신자([CheckCancel](../../references/xrp-ledger/undefined-1/undefined-1/checkcancel.md) 거래를 사용하여 취소) 또는 수령인(취소 또는 현금화)만이 객체를 수정할 수 있습니다. 만료 시간이 지나면 누구나 Check를 취소할 수 있습니다.

**3단계:** 수표를 현금화하기 위해 수령인은 [CheckCash](../../references/xrp-ledger/undefined-1/undefined-1/checkcash.md) 거래를 제출합니다. 수령인은 다음 두 가지 옵션 중 하나를 선택하여 수표를 현금화할 수 있습니다:

* <mark style="background-color:yellow;">Amount</mark> - 수령인은 정확한 현금화 금액을 지정할 수 있습니다. 이는 송신자가 [수수료](../transactions/fees.md)를 고려하여 수표를 패딩한 경우에 유용할 수 있으며, 수령인이 송장이나 계약서와 같은 정확한 금액을 수락하고자 할 때 유용할 수 있습니다.
* <mark style="background-color:yellow;">DeliverMin</mark> - 수령인은 이 옵션을 사용하여 수표로부터 수령하려는 최소 금액을 지정할 수 있습니다. 수령인이 이 옵션을 사용하는 경우, XRP Ledger는 가능한 한 많은 금액을 제공하고 항상 이 금액 이상을 제공합니다. 수령인에게 제공될 수 있는 금액이 요청한 금액 이상이 아닌 경우 거래는 실패합니다.

송신자가 충분한 자금을 가지고 있고 만료 시간이 지나지 않은 경우, 자금은 송신자의 계정에서 인출되어 수령인의 계정에 입금되며, Check 체는 파괴됩니다.

## 만료

만료되는 경우, 수표의 생명주기는 아래와 같습니다.

<figure><img src="../../.gitbook/assets/Checks_1.png" alt=""><figcaption></figcaption></figure>

모든 수표는 동일한 방식으로 시작되므로 **1단계와 2단계**는 동일합니다.

**3단계a:** 수령인이 수표를 현금화하기 전에 수표가 만료되면 수표를 더 이상 현금화할 수 없지만 객체는 ledger에 남아 있습니다.

**4단계a:** 수표가 만료된 후에는 누구나 CheckCancel 거래를 제출하여 수표를 취소할 수 있습니다. 이 거래는 수표를 ledger에서 제거합니다.

## 수표의 사용 가능 여부

수표 수정안은 2020년 6월 18일에 XRP Ledger Mainnet에서 활성화되었습니다. 수정안이 활성화되고 투표되는 방법에 대한 자세한 내용은 수정안 프로세스를 참조하세요.

테스트 네트워크나 개인 네트워크에서 수정안의 상태를 확인하려면 feature 메소드를 사용하세요.

&#x20;
