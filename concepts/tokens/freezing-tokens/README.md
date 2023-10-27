# Freezing Tokens(토큰 동결)

토큰 발행자는 XRP Ledger에서 발행한 토큰을 동결할수 있습니다. 이는 **XRP에는 적용되지 않으며**, XRP는 XRP Ledger의 기본 자산이므로 발행 토큰이 아닙니다.

특정 경우에는 규정 요건을 충족하거나 의심스러운 활동을 조사하는 동안 거래소나 게이트웨이가 토큰 잔액을 동결할 할 수도 있습니다.

{% hint style="info" %}
Tip:

아무도 XRP를 XRP Ledger에서 동결할 수 없습니다. 그러나 위탁 거래소는 자체 재량에 따라 보관 중인 자금을 항상 동결 수 있습니다. 자세한 내용은 [동결에 대한 일반적인 오해](common-misunderstandings-about-freezes.md)를 참조하세요.
{% endhint %}

동결과 관련된 세 가지 설정이 있습니다:

* [**개별 동결**](./#undefined)**(**Individual Freeze) - 특정 상대방 동결.&#x20;
* [**전역 동결**](./#undefined-1)(Global Freeze) - 모든 상대방 동결.&#x20;
* [**동결 없음**](./#undefined-2)(No Freeze) - 개별 상대방 동결 및 전역 동결 해제 기능 영구적으로 포기.&#x20;

모든 동결 설정은 양수 또는 음수인 동결될 잔액과 관계없이 활성화될 수 있습니다. 토큰 발행자나 통화 소지자 모두 신뢰선을 동결할 수 있지만, 통화 소지자가 동결을 활성화하는 경우 영향은 미미합니다.

## 개별 동결(Individual Freeze)

**개별 동결** 기능은 [신뢰선](../trust-lines-and-issuing.md)의 설정입니다. 발행자가 개별 동결 설정을 활성화하면 해당 신뢰선의 토큰에 다음 규칙이 적용됩니다:

* 동결된 신뢰선의 두 당사자 간에는 여전히 직접 결제가 가능합니다.
* 동결된 신뢰선의 상대방은 발행된 토큰 잔액을 발행자에게 직접적인 지불을 제외하고는 감소시킬 수 없습니다. 상대방은 동결된 토큰을 직접적으로 발행자에게 보낼 수만 있습니다.&#x20;
* 동결된 신뢰선 상에서 상대방은 여전히 다른 사람들로부터 지불을 받을 수 있습니다.
* 동결된 신뢰선의 토큰을 판매하기 위한 상대방의 거래 제안은 [미지급으로 간주](../../../references/xrp-ledger/ledger/ledger-1/offer.md)됩니다.

알림: 신뢰선은 XRP를 보유하지 않습니다. XRP는 동결할 수 없습니다.

금융 기관은 의심스러운 활동을 보이거나 금융 기관의 이용 약관을 위반하는 상대방과의 연결된 신뢰선을 동결할 수 있습니다. 금융 기관은 XRP Ledger에 연결된 다른 시스템에서도 해당 상대방을 동결시켜야 합니다. (그렇지 않으면 주소는 여전히 금융 기관을 통해 지불을 보내는 것을 통해 원하지 않는 활동을 수행할 수 있습니다.)

개별 주소는 금융 기관에 대한 신뢰선을 동결할 수 있습니다. 이는 기관과 다른 사용자 간의 거래에는 영향을 미치지 않습니다. 그러나 이는 [운영 주소](../../undefined-2/undefined-6.md)를 포함한 다른 주소에서 해당 금융 기관의 토큰을 개별 주소로 보낼 수 없도록 합니다. 이러한 개별 동결은 거래 제안에는 영향을 미치지 않습니다.

개별 동결은 단일 신뢰선에 적용됩니다. 특정 상대방과 여러 토큰을 동결 되려면 주소는 각각의 통화 코드에 대해 신뢰선에서 개별 동결을 활성화해야 합니다.

주소에 동결 없음 설정이 활성화되어 있는 경우 개별 동결 설정을 활성화할 수 없습니다.

주소가 동결 안 함 설정을 사용 설정한 경우 개별 [동결 설정을 사용할 수 없습니다](./#undefined-2).

## 전역 동결(Global Freeze)

**전역 동결** 기능은 계정의 설정입니다. 계정은 자체적으로 전역 동결 설정을 활성화할 수 있습니다. 발행자가 전역 동결 기능을 활성화하면 다음 규칙이 해당 발행자가 발행한 모든 토큰에 적용됩니다:

* 동결한 발행자의 모든 상대방은 해당 상대방과의 신뢰선 잔액을 발행된 계정에 대한 직접 지불을 제외하고는 감소시킬 수 없습니다. (이는 발행자 자체 [운영 주소](../../undefined-2/undefined-6.md)에도 영향을 미칩니다.)
* 동결된 발행자의 상대방은 여전히 발행 주소와 직접적으로 지불 및 수령할 수 있습니다.&#x20;
* 동결된 주소가 발행한 토큰을 판매하는 모든 제안은 [미지급으로 간주됩니다](../decentralized-exchange/offers.md).&#x20;

알림: 주소는 XRP를 발행할 수 없습니다. 전역 동결은 XRP에는 적용되지 않습니다.

발행 기관의 비밀 키가 침해된 경우에는 발행 기관의 발행 계정에 전역 동결을 활성화하는 것이 유용할 수 있습니다. 이렇게 하면 자금 흐름이 차단되어 공격자가 더 많은 자금을 탈취하는 것을 방지하거나 최소한 일어난 상황을 추적하기 쉽게 만들 수 있습니다. XRP Ledger에서 전역 동결을 시행하는 것 외에도, 발행 기관은 외부 시스템에서의 활동을 중지해야 합니다.

금융 기관이 새로운 발행 주소로 마이그레이션하려는 경우 또는 금융 기관이 사업을 중단하려는 경우에도 전역 동결을 활성화하는 것이 유용할 수 있습니다. 이렇게 하면 특정 시점에서 자금이 잠기며, 사용자가 다른 통화로 교환할 수 없도록 합니다.

전역 동결은 주소에서 발행한 모든 토큰에 적용됩니다. 단일 화폐 코드에 대해서만 전역 동결을 활성화할 수는 없습니다. 일부 토큰을 동결하고 다른 토큰을 동결하지 않기 위해서는 각각의 토큰에 대해 서로 다른 주소를 사용해야 합니다.

주소는 항상 전역 동결 설정을 활성화할 수 있습니다. 그러나 주소가 [동결 없음](./#undefined-2) 설정을 활성화한 경우에는 전역 동결을 비활성화할 수 없습니다.

## 동결 없음(No Freeze)

**동결 없음** 기능은 주소의 설정으로, 임의로 토큰을 동결하는 능력을 영구적으로 포기합니다. 발행자는 이 기능을 사용하여 자신의 토큰을 "물리적인 현금과 더 비슷하게" 만들 수 있습니다. 즉, 발행자가 상대방 간의 토큰 거래에 개입할 수 없도록 합니다.

알림: XRP는 이미 동결할 수 없습니다. 동결 없음 기능은 XRP Ledger에서 발행된 다른 토큰에만 적용됩니다.

동결 없음 설정에는 두 가지 효과가 있습니다:

* 발행자는 어떤 상대방에 대한 신뢰 선에 개별 동결을 활성화할 수 없습니다.&#x20;
* 발행자는 전역 동결을 실행할 수 있지만 전역 동결을 비활성화할 수는 없습니다.&#x20;

XRP Ledger는 발행자가 발행한 자금의 의무를 준수할 것을 강제할 수 없으므로, 동결 없음은 스테이블코인 발행자가 채무 불이행을 방지합니다. 그러나 동결 없음은 발행자가 특정 사용자에게 불공정하게 전역 동결 기능을 사용하지 않도록 보장합니다.

동결 없음 설정은 주소로부터 발행된 모든 토큰에 적용됩니다. 일부 토큰을 동결하고 다른 토큰을 동결하지 않기 위해서는 각각의 토큰에 대해 서로 다른 주소를 사용해야 합니다.

동결 없음 설정은 해당 주소의 마스터 키 비밀로 서명된 트랜잭션으로만 활성화할 수 있습니다. [일반 키](../../../references/xrp-ledger/undefined-1/undefined-1/setregularkey.md)나 [다중 서명 트랜잭션](../../undefined-2/undefined.md)은 동결 없음을 활성화하는 데 사용할 수 없습니다.

## See Also <a href="#see-also" id="see-also"></a>

* [Freeze Code Samples ](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/freeze)
* **Concepts:**
  * [Trust Lines and Issuing](https://xrpl.org/trust-lines-and-issuing.html)
* **Tutorials:**
  * [Enable No Freeze](https://xrpl.org/enable-no-freeze.html)
  * [Enact Global Freeze](https://xrpl.org/enact-global-freeze.html)
  * [Freeze a Trust Line](https://xrpl.org/freeze-a-trust-line.html)
* **References:**
  * [account\_lines method](https://xrpl.org/account\_lines.html)
  * [account\_info method](https://xrpl.org/account\_info.html)
  * [AccountSet transaction](https://xrpl.org/accountset.html)
  * [TrustSet transaction](https://xrpl.org/trustset.html)
  * [AccountRoot Flags](https://xrpl.org/accountroot.html#accountroot-flags)
  * [RippleState (trust line) Flags](https://xrpl.org/ripplestate.html#ripplestate-flags)
