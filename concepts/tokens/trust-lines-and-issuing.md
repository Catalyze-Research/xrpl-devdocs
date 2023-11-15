# 신뢰선과 발급(Trust Lines and Issuing)

신뢰선은 XRP Ledger에서 [토큰](./)을 보유하기 위한 구조입니다. 신뢰선은 XRP Ledger의 규칙인 '원하지 않는 토큰을 다른사람들에게 보유하게 할 수 없다'는 규칙을 강제합니다. 이 조치는 XRP Ledger의 [커뮤니티 크레딧 사용 사례](https://xrpl.org/tokens.html#community-credit) 등의 이점을 가능하게 하기 위해 필요합니다.

각 "신뢰선"은 다음을 포함하는 양방향 관계입니다:

* 신뢰선이 연결하는 [**두 계정**](../undefined-2/)의 식별자 입니다.
* 하나의 공유 **잔액**은 한 계정의 관점에서는 양(positive)이지만 다른 계정의 관점에서는 음(negative)입니다.
  * 음의(negative) 잔액을 가진 계정은 일반적으로 토큰의 "발행자"로 간주됩니다. 하지만 [API](../../tutorials/http-websocket-apis/http-websocket-api-get-started-using-http-websocket-apis.md)에서는 <mark style="background-color:yellow;">발행자</mark> 이름이 양쪽을 모두 가리킬 수 있습니다.
* 다양한 설정과 메타데이터. 각 계정은 신뢰선에 대한 자체 **설정**을 제어할 수 있습니다.
  * 가장 중요한 점은, 각 측이 신뢰선의 **한도**를 설정한다는 것이며, 이는 기본적으로 0입니다. 각 계정의 잔액(신뢰선의 입장에서)은 해당 계정의 한도를 넘어갈 수 없으며, 이는 [해당 계정의 행동](https://xrpl.org/trust-lines-and-issuing.html#going-above-the-limit)을 통해서만 가능합니다.

신뢰선은 주어진 화폐 코드에 특정합니다. 두 계정 사이에는 다른 [화폐 코드](../../references/xrp-ledger/basic-data-types/currency-formats.md)를 위한 신뢰선 여러 개 있을 수 있지만, 특정 화폐 코드에 대해서는 하나의 공유 신뢰선만 있을 수 있습니다.

## 생성&#x20;

어느 계정이든 다른 계정에 대해 토큰을 발행하도록 일방적으로 "신뢰"를 할 수 있습니다. 이는 [TrustSet 트랜잭션](../../references/xrp-ledger/undefined/undefined-1/trustset.md)을 통해 0이 아닌 한도와 자체 설정을 보내는 것으로, 이는 잔액이 0인 신뢰선을 생성하고 다른 쪽의 설정을 기본값으로 설정합니다.

신뢰선은 [탈중앙화 거래소](decentralized-exchange/)에서 토큰을 사는 것과 같은 일부 트랜잭션에 의해 암시적으로 생성될 수 있습니다. 이 경우, 신뢰선은 완전히 기본 설정을 사용합니다.

## 한도 초과&#x20;

신뢰선의 한도를 초과하여 잔액을 보유할 수 있는 세 가지 경우가 있습니다:

1. [거래](decentralized-exchange/)를 통해 해당 토큰을 더 획득할 때.
2. 양의 잔액이 있는 신뢰선의 한도를 줄일 때.
3. [수표를 현금화](../undefined-1/undefined-1.md)하여 해당 토큰을 더 획득할 때._(_[_CheckCashMakesTrustLine_](../xrp-ledger/amendments/undefined.md) _수정안이 필요.)_

## 신뢰선 설정

공유 잔액 외에도 각 계정은 신뢰선에 대한 자체 설정을 가지며, 이는 다음을 포함합니다:

* **한도**, 0에서 최대 토큰 금액까지의 수치입니다. 지불과 다른 계정의 행동은 신뢰선의 잔액(이 계정의 입장에서)이 한도를 초과하게 할 수 없습니다. 기본값은 0입니다.
* **승인됨**: 참/거짓 값으로, 이 계정이 발행하는 토큰을 다른 쪽이 보유하도록 허용하는 [승인된 신뢰선](authorized-trust-lines.md) 함께 사용됩니다. 기본값은 <mark style="background-color:yellow;">false</mark>입니다. 일단 <mark style="background-color:yellow;">true</mark>로 설정되면 이를 다시 변경할 수 없습니다.
* **No Ripple**: 이 신뢰선을 통해 토큰이 [ripple](rippling.md) 될 수 있는지를 제어하는 참/거짓 값입니다. 기본값은 계정의 "Default Ripple" 설정에 따라 다릅니다. 새 계정의 경우, "Default Ripple"이 꺼져 있어서 No Ripple의 기본값은 <mark style="background-color:yellow;">true</mark>입니다. 일반적으로, 발행자는 Ripple을 허용하고, 발행자가 아닌 사람들은 커뮤니티 크레딧에 대한 신뢰선을 사용하지 않는 한 Ripple을 비활성화해야 합니다.
* **동결**: 이 신뢰선에 대한 [개별 동결](freezing-tokens/)이 적용되어 있는지를 나타내는 참/거짓 값입니다. 기본값은 <mark style="background-color:yellow;">false</mark>입니다.
* **Quality In**과 **Quality Out** 설정: 이는 계정이 이 신뢰선에서 다른 계정이 발행한 토큰을 표면 가치보다 낮게(또는 높게) 평가할 수 있게 합니다. 예를 들어, 안정화폐 발행자가 오프-ledger 자산에 대한 토큰을 인출하는데 3%의 수수료를 부과한다면, 이 설정을 사용하여 해당 토큰을 표면 가치의 97%로 평가할 수 있습니다. 기본값인 0은 표면 가치를 나타냅니다.

## Reserves와  삭제&#x20;

신뢰선이 ledger에 공간을 차지하기 때문에, [신뢰선은 계정이 보유해야 하는 reserve-XRP를 증가시킵니다.](../undefined-2/reserves.md) 신뢰선에 있는 계정 중 하나 또는 둘 다가 신뢰선의 reserve를 부담할 수 있습니다. 이는 신뢰선의 상태에 따라 다릅니다: 설정이 기본 상태가 아니거나 양의 잔액을 보유하고 있으면, 이는 소유자 reserve에 대한 항목 한 개로 간주됩니다.

일반적으로 이는 신뢰선을 생성한 계정이 reserve를 부담하고 발행자는 부담하지 않는다는 것을 의미합니다.

신뢰선은 양쪽의 설정이 기본 상태에 있고 잔액이 0인 경우 자동으로 삭제됩니다. 이는 신뢰선을 삭제하려면 다음을 수행해야 함을 의미합니다:

1. 기본 설정으로 설정하려면 [TrustSet 트랜잭션](../../references/xrp-ledger/undefined/undefined-1/trustset.md)을 보냅니다.
2. 신뢰선에서 당신이 가진 양의(positive)잔액을 모두 없애야 합니다. 이를 위해 [지불](../../references/xrp-ledger/undefined/undefined-1/payment.md)을 보내거나 [탈중앙화 거래소](decentralized-exchange/)에서 화폐를 팔 수 있습니다.

당신의잔액이 음수(발행자인 경우)이거나 다른 쪽의 설정이 기본 상태가 아닌 경우, 신뢰선을 완전히 삭제할 수는 없지만, 동일한 단계를 따르면 소유자 reserve에 대한 부담을 줄일 수 있습니다.

**승인된** 설정은 한 번 작동하면 다시 멈출 수 없으므로, 신뢰선의 기본 상태에는 포함되지 않습니다.

## 무료 신뢰선

신뢰선은 XRP Ledger의 강력한 기능이므로, 계정의 처음 두 개의 신뢰선을 "무료"로 만드는 특별한 기능이 있습니다.

계정이 새로운 신뢰선을 생성할 때, 계정이 새로운 선을 포함하여 ledger에 최대 2개의 항목을 소유하고 있다면, 계정의 소유자 reserve 일반 금액 대신 0으로 취급됩니다. 이는 계정이 ledger의 객체를 소유하는 데 필요한 증가된 reserve requirement을 충족시키지 못하더라도 거래가 성공할 수 있도록 합니다.

한 계정이 ledger에 3개 이상의 객체를 소유하고 있는 경우, 전체 소유자 reserve가 적용됩니다.
