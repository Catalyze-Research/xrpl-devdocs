# Rippling

XRP Ledger에서 "rippling"은 동일한 토큰에 대해 [신뢰선](undefined.md)을 가진 여러 연결된 당사자 간의 원자적인 순수 결제 과정을 설명합니다. rippling은 토큰을 보유한 사용자들이 발행자를 수동 중개인으로 사용하여 서로에게 토큰을 전송할 수 있도록 합니다. 일종의 수동 양방향 [환전 주문](../dex/undefined.md)으로, 동일한 통화 코드를 가진 두 토큰 간에 1:1 환율과 제한 없이 교환할 수 있게 해줍니다.

rippling은 결제 [경로](undefined-5.md)를 따라 발생합니다. [직접적인 XRP 간의 결제](../undefined-1/xrp.md)에는 rippling이 포함되지 않습니다.

발행되지 않은 계정에 대해서는 rippling이 원하지 않을 수도 있습니다. 왜냐하면 rippling을 통해 사용자들이 동일한 통화 코드를 가진 서로 다른 발행자의 토큰 간에 부담을 이동시킬 수 있기 때문입니다. [No Ripple 플래그](rippling.md#no-ripple)는 다른 사용자가 당신의 계정에 대한 신뢰선을 열 때 기본적으로 rippling을 비활성화 합니다. 단, 기본 rippling을 활성화하려면 [기본 Ripple 플래그](rippling.md#ripple)를 사용해야 합니다.

{% hint style="info" %}
Caution:

신뢰선을 생성할 때, 해당 신뢰선에서 rippling을 차단하기 위해 <mark style="background-color:yellow;">tfSetNoRipple</mark> 플래그를 명시적으로 활성화해야 합니다.
{% endhint %}

## Rippling의 예시&#x20;

"rippling"은 두 개 이상의 신뢰선을 조정하여 결제를 수행할 때 발생합니다. 예를 들어, Alice가 Charlie에게 돈을 빚지고 Alice가 또한 Bob에게 돈을 빚진 경우, XRP Ledger에서는 다음과 같이 신뢰선으로 표시할 수 있습니다.

<figure><img src="https://xrpl.org/img/noripple-01.svg" alt=""><figcaption></figcaption></figure>

Bob이 Charlie에게 $3을 지불하려면 "Alice, 네게 빚진 돈 중 $3을 가져와 Charlie에게 지불해"라고 할 수 있습니다. Alice는 Bob로부터 일부 빚을 Charlie로 이전시킵니다. 최종적으로 신뢰선은 다음과 같이 조정됩니다.

<figure><img src="https://xrpl.org/img/noripple-02.svg" alt=""><figcaption></figcaption></figure>

우리는 이러한 과정을 두 주소가 서로에게 대금을 조정하는 것으로 "rippling"이라고 합니다. 이는 XRP Ledger의 유용하고 중요한 기능입니다. rippling은 동일한 [화폐 코드](../../references/xrp-ledger/undefined/undefined.md)를 사용하는 신뢰선으로 연결된 주소 간에 발생합니다. 발행자가 동일할 필요는 없으며, 사실 더 큰 체인은 항상 발행자를 변경하여 발생합니다.

## No Ripple 플래그&#x20;

rippling이 원하지 않은 발행되지 않은 계정(특히 서로 다른 수수료와 정책을 가진 발행자의 잔액을 보유하는 유동성 제공자)은 일반적으로 잔액이 예상치 않게 서로 다른 발행자 간에 이동하지 않도록 원하지 않습니다.

**No Ripple** 플래그는 신뢰선의 설정 중 하나입니다. 두 개 이상의 신뢰선이 동일한 주소로부터 No Ripple을 활성화한 경우, 제3자로부터의 결제는 해당 주소를 통해 해당 신뢰선을 통해 rippling할 수 없습니다. 이를 통해 동일한 통화 코드를 사용하는 서로 다른 발행자 간에 잔액이 예상치 않게 변경되는 것으로부터 유동성 제공자를 보호합니다.

계정은 개별 신뢰선에서 No Ripple을 비활성화할 수 있으며, 해당 신뢰선을 포함하는 모든 쌍을 통해 rippling할 수 있게 할 수도 있습니다. 또한 [기본 Ripple 플래그](rippling.md#ripple)를 활성화하여 기본적으로 rippling을 허용할 수 있습니다.

예를 들어, Emily가 다른 금융 기관에서 발행한 두 가지 다른 토큰을 가지고 있다고 가정해보겠습니다.

<figure><img src="https://xrpl.org/img/noripple-03.svg" alt=""><figcaption></figcaption></figure>

이제 Charlie는 Emily의 주소를 통해 Daniel에게 rippling하여 지불할 수 있습니다. 예를 들어, Charlie가 Daniel에게 $10을 지불한다면:

<figure><img src="https://xrpl.org/img/noripple-04.svg" alt=""><figcaption></figcaption></figure>

이는 Charlie와 Daniel을 모르는 Emily에게 놀라운 일일지도 모릅니다. 더 나쁜 일은 Institution A가 Institution B보다 높은 수수료를 부과하여 Emily에게 돈을 인출하는 데 비용이 발생할 수 있다는 점입니다. No Ripple 플래그는 이러한 상황을 피하기 위해 존재합니다. Emily가 두 개의 신뢰선에 대해 해당 플래그를 설정하면 해당 신뢰선을 통해 rippling할 수 없게 됩니다.

예를 들어:

<figure><img src="https://xrpl.org/img/noripple-05.svg" alt=""><figcaption></figcaption></figure>

이제 Charlie가 Emily의 주소를 통해 Daniel에게 rippling하여 지불하는 시나리오는 더 이상 가능하지 않습니다.

## 구체적인 사항&#x20;

No Ripple 플래그는 특정 경로를 유효하지 않게 만들어 결제에 사용할 수 없도록 합니다. 경로는 해당 주소에 대해 No Ripple이 활성화된 신뢰선을 통해 **들어가고 나가는** 경우에만 유효합니다. 경로가 주어진 주소로 들어가고 나가면서 No Ripple이 그 주소에 대해 활성화된 신뢰선을 사용하는 경우에만 해당 경로가 유효합니다.

<figure><img src="https://xrpl.org/img/noripple-06.svg" alt=""><figcaption></figcaption></figure>

## 기본 Ripple 플래그&#x20;

**기본 Ripple** 플래그는 기본적으로 모든 들어오는 신뢰선에 대해 rippling을 활성화하는 계정 설정입니다. 발행자는 고객이 서로 토큰을 전송할 수 있도록 이 플래그를 활성화해야 합니다.

계정의 기본 Ripple 설정은 사용자가 생성하는 신뢰선에 영향을 주지 않습니다. 다만, 다른 사람이 열어주는 신뢰선에만 영향을 줍니다. 계정의 기본 Ripple 설정을 변경하면 변경 전에 생성된 신뢰선은 기존의 No Ripple 설정을 유지합니다. [TrustSet 트랜잭션](../../references/xrp-ledger/undefined-1/undefined-1/trustset.md)을 사용하여 신뢰선의 No Ripple 설정을 주소의 새 기본값과 일치시킬 수 있습니다.

자세한 내용은 ['XRP Ledger 게이트웨이 되기'의 기본 Ripple을](../../tutorials/xrp-ledger/undefined.md) 참조하세요.

## No Ripple 사용하기&#x20;

### No Ripple 활성화/비활성화&#x20;

No Ripple 플래그는 주소가 해당 신뢰선의 양수 또는 0 잔액을 가진 경우에만 활성화할 수 있습니다. 이렇게 하면 기능을 남용하여 신뢰선 잔액의 의무를 무단으로 철회하는 것을 방지할 수 있습니다. (물론 주소를 버리는 것으로는 여전히 의무를 무단으로 철회할 수 있습니다.)

No Ripple 플래그를 활성화하려면 <mark style="background-color:yellow;">tfSetNoRipple</mark> 플래그를 사용하는 [TrustSet 트랜잭션](../../references/xrp-ledger/undefined-1/undefined-1/trustset.md)을 보내십시오. No Ripple 플래그를 비활성화하려면 <mark style="background-color:yellow;">tfClearNoRipple</mark> 플래그를 사용하면 됩니다.

## No Ripple 상태 확인하기&#x20;

상호적으로 신뢰하는 두 계정의 경우 No Ripple 플래그는 각 계정별로 따로 추적됩니다.

[HTTP/웹소켓 API](../../references/http-websocket-apis/) 또는 선호하는 [클라이언트 라이브러리](../../references/undefined/)를 사용하여 [account\_lines 메소드](../../references/http-websocket-apis/api-1/undefined/account\_lines.md)를 통해 신뢰선을 조회할 수 있습니다. 각 신뢰선에 대해 <mark style="background-color:yellow;">no\_ripple</mark> 필드는 현재 주소가 해당 신뢰선에 No Ripple 플래그를 활성화했는지 여부를 보여주며, <mark style="background-color:yellow;">no\_ripple\_peer</mark> 필드는 상대방이 No Ripple 플래그를 활성화했는지 여부를 보여줍니다.
