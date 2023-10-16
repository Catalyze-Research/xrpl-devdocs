# NFT 경매 진행하기

경매를 진행하는 방법은 몇 가지가 있으며 각각 장단점이 있습니다.

## XRPL 외부에서 경매 진행하고 XRPL에서 구매 완료하기&#x20;

이 방법은 가장 간단한 접근 방식입니다. <mark style="background-color:yellow;">NFTokenOffer</mark> 객체는 항상 생성자가 취소할 수 있으므로 구속력 있는 제안을 구현할 수 없습니다.

* 입찰 내역을 개인 데이터베이스에 저장합니다.&#x20;
* 당신은 최고 입찰액의 일부를 가져갑니다.&#x20;
* 구매자/판매자에게 XRPL 트랜잭션을 보내 구매를 완료합니다.&#x20;

## Reserve를 사용하여 중개 모드로 경매 실행&#x20;

중개자 모드에서 reserve가 있는 경매.

<figure><img src="https://xrpl.org/img/nft-auction1.png" alt=""><figcaption></figcaption></figure>

1. 판매자는 NFT를 생성하고, <mark style="background-color:yellow;">NFTokenCreateOffer</mark>를 사용하여 reserve 경매 가격을 설정하며, 중개자 계정을 목적지로 지정합니다.&#x20;
2. 입찰자들은 <mark style="background-color:yellow;">NFTokenCreateOffer</mark>를 사용하여 입찰을 하고, 중개자 계정을 목적지로 설정합니다.&#x20;
3. 중개자는 당첨 입찰을 선택하고, <mark style="background-color:yellow;">NFTokenCreateOffer</mark>를 사용하여 판매를 완료하고 중개수수료를 수집합니다. 그 후 중개자는 <mark style="background-color:yellow;">NFTokenCreateOffer</mark>를 사용하여 패배한 입찰을 취소합니다.&#x20;

### 장점:

* 경매 전체가 XRP Ledger에서 이루어집니다, 중개 수수료 포함.&#x20;
* 판매자는 체인 상에 reserve 가격을 표시합니다.&#x20;
* 이것은 구매 측에서 바인딩 제안에 _가깝습니다_.&#x20;

### 단점:

* 판매자와 중개자 사이에는 중개자가 사전에 합의된 비율 이상을 받지 않을 것이라는 암묵적인 신뢰가 있어야 합니다. 만약 reserve가 1 XRP이었고 당첨 입찰이 1000 XRP였다면, 중개자가 999 XRP를 이익으로 가져가 판매자에게는 reserve 이익만 남겨두는 것을 방지하는 체인 상의 메커니즘이 없습니다.&#x20;

이 단점의 주요 완화 요인은 이러한 행동이 발생하면 중개자들이 시장 점유율을 모두 잃을 것이라는 점이며, 판매자들은 이를 이해해야 합니다.

## 중개자 모드에서 reserve 없이 경매 실행&#x20;

이는 세 가지 워크플로우 중 가장 복잡한 것입니다.

<figure><img src="https://xrpl.org/img/nft-auction2.png" alt=""><figcaption></figcaption></figure>

1. 판매자는 <mark style="background-color:yellow;">NFTokenMint</mark>를 사용하여 NFT를 생성합니다.&#x20;
2. 입찰자들은 <mark style="background-color:yellow;">NFTokenCreateOffer</mark>를 사용하여 입찰을 하고, 중개자를 목적지로 설정합니다.&#x20;
3. 중개자는 당첨 입찰을 선택하고, 수수료로 수집될 금액을 뺀 다음, 판매자에게 <mark style="background-color:yellow;">NFTokenCreateOffer</mark>를 통해 이 금액에 대한 판매 제안을 서명하도록 요청합니다.&#x20;
4. 판매자는 요청된 제안에 서명하고, 중개자를 목적지로 설정합니다.&#x20;
5. 중개자는 <mark style="background-color:yellow;">NFTokenAcceptOffer</mark>를 사용하여 판매를 완료하고, 중개 수수료를 받습니다.&#x20;
6. 중개자는 <mark style="background-color:yellow;">NFTokenCreateOffer</mark>를 사용하여 남은 입찰을 취소합니다.&#x20;

### 장점:

* 이 흐름은 참가자들 사이에 절대적인 신뢰가 필요하지 않아, 블록체인에서 대부분의 사람들이 기대하는 옵션입니다.&#x20;
* 판매자들은 중개자가 얼마나 많은 수수료를 취하는지 정확히 알고, 체인 상에서 이에 동의해야 합니다.&#x20;

### 단점:

* 경매가 완료된 후에는 판매가 최종 입찰 금액과 중개 수수료 금액에 따라 결정됩니다. 이는 판매자들이 완전히 완료된 경매에서 철회할 수 있거나, 판매자들이 산만하거나 알림을 보지 못해 결산을 지연할 수 있다는 의미입니다.&#x20;
* 경매가 완료된 후에 판매자는 당첨 입찰을 거절하고, 대신 다른 사람에게 판매할 수 있습니다.
