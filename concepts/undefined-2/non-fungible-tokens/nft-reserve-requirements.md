# NFT Reserve Requirements

NFT를 발행하고 보유하며 판매를 위해서는 reserve로 XRP를 보유해야 합니다. reserve 요금은 빠르게 누적될 수 있습니다. reserve requirements을 이해하면 비즈니스 케이스에 가장 적합한 접근 방식을 선택할 수 있습니다.

## 기본 Reserve

귀하의 계정은 현재 10 XRP의 기본 reserve을 보유해야 합니다. 기본 reserve XRP 금액은 변경될 수 있습니다. [기본 reserve 및 소유자 reserve](../../undefined-3/undefined/reserves.md)을 참조하세요.

## 소유자 Reserve&#x20;

XRP Ledger에서 귀하가 소유한 개체 당 소유자 reserve가 현재 2 XRP로 필요합니다. 이는 사용자가 필요 없는 데이터로 ledger를 스팸하게 하지 않도록 하고 더 이상 필요하지 않은 데이터를 정리하도록 유도하기 위한 것입니다. 소유자 reserve 금액은 변경될 수 있습니다. [기본 reserve 및 소유자 reserve](../../undefined-3/undefined/reserves.md)을 참조하세요.

NFT의 경우, 개체는 개별 NFT를 의미하는 것이 아닌 계정이 소유한 <mark style="background-color:yellow;">NFTokenPage</mark> 객체를 의미합니다. <mark style="background-color:yellow;">NFTokenPage</mark> 객체는 최대 32개의 NFT를 저장할 수 있습니다.

하지만 NFT는 공간 절약을 위해 페이지에 패킹되지 않습니다. 따라서 64개의 NFT를 소유한다고 해서 2개의 <mark style="background-color:yellow;">NFTokenPage</mark> 객체만 가지고 있는 것은 아닙니다.

평균적으로 각 NFTokenPage가 24개의 NFT를 저장한다는 것을 일반적인 기준으로 가정하는 것이 좋습니다. 따라서 N개의 NFT를 발행하거나 소유하는 데 필요한 reserve requirements은 (24N)/2 또는 각 NFT 당 1/12 XRP의 reserve requirements으로 추정할 수 있습니다.

다음 표는 소유한 NFT의 수와 해당 NFT를 저장하는 페이지 수에 따라 총 소유자 reserve가 얼마나 될 수 있는지의 예시를 제공합니다.

| 소유한 NFT    | 우수 사례  | 일반적인 경우 | 최악의 경우  |
| ---------- | ------ | ------- | ------- |
| 32 혹은 그 이하 | 2 XRP  | 2 XRP   | 2 XRP   |
| 50         | 4 XRP  | 6 XRP   | 8 XRP   |
| 200        | 14 XRP | 18 XRP  | 26 XRP  |
| 1000       | 64 XRP | 84 XRP  | 126 XRP |

## NFTokenOffer Reserve&#x20;

각 <mark style="background-color:yellow;">NFTokenOffer</mark> 객체는 제안을 게시하는 계정에게 한 단위의 reserve을 요구합니다. 현재로서는 이 reserve은 2 XRP입니다. reserve은 제안을 취소함으로써 회수할 수 있습니다. 또한 수락되어 해당 제안이 XRP Ledger에서 제거되면 reserve도 회수됩니다.

{% hint style="info" %}
Tip

NFT를 판매한 후에는 예의상 입찰자를 대신해 유효기간이 지난 <mark style="background-color:yellow;">NFTokenOffer</mark>를 취소하여 reserve를 돌려드리세요. NFTokenCancelOffer 트랜잭션으로 이를 수행할 수 있습니다.
{% endhint %}

## 실제적인 고려사항

NFT를 발행, 보유 및 구매, 판매할 때 reserve requirements은 빠르게 누적될 수 있습니다. 이로 인해 거래 중에 계정의 reserve requirements보다 낮아질 수 있습니다. requirements 미달 시 거래 능력이 제한될 수 있습니다. [reserve requirements 미달 시](../../undefined-3/undefined/reserves.md)를 참조하세요.

새로운 계정을 생성하고 NFT를 발행한 다음 XRP Ledger에 NFTokenSellOffer를 생성하는 경우, 최소 14 XRP의 reserve이 필요합니다.

| Reserve 종류     |     금액 |
| -------------- | -----: |
| Base           | 10 XRP |
| NFToken Page   |  2 XRP |
| NFToken Offers |  2 XRP |
| Total          | 14 XRP |

{% hint style="info" %}
Note

Reserve requirements는 아니지만, 각 발행 및 판매 프로세스에서 트랜잭션당 보통 12 드롭(약 0.000012 XRP)의 미미한 수수료를 커버하기 위해 reserve 이상으로 적어도 1 XRP를 보유해야 함을 기억하세요.
{% endhint %}

200개의 NFT를 발행하고 각각에 대해 <mark style="background-color:yellow;">NFTokenSellOffer</mark>를 생성하는 경우, 최대 436 XRP의 reserve가 필요합니다.

| Reserve 종류     |      금액 |
| -------------- | ------: |
| Base           |  10 XRP |
| NFToken Pages  |  26 XRP |
| NFToken Offers | 400 XRP |
| Total          | 436 XRP |

필요한 reserve가 편안하게 설정한 금액을 초과하는 경우, 일정 시간 동안 보유할 NFT 및 제안의 수를 줄이기 위해 온디맨드로 발행하는 모델을 고려해보십시오. [온디맨드 발행](batch-minting.md)을 참조하세요.

&#x20;
