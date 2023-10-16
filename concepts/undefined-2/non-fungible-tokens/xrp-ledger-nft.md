# XRP Ledger에서 NFT 토큰 거래

XRP Ledger에서는 <mark style="background-color:yellow;">NFToken</mark> 객체를 계정 간에 이전할 수 있습니다. <mark style="background-color:yellow;">NFToken</mark>을 구매하거나 판매하기 위해 구매 또는 판매 제안을 할 수 있습니다. 또한 소유한 <mark style="background-color:yellow;">NFToken</mark>을 구매하고자 하는 다른 계정의 제안을 수락할 수도 있습니다. 심지어 <mark style="background-color:yellow;">NFToken</mark>을 0 가격으로 판매 제안하여 무료로 양도할 수도 있습니다. 모든 제안은 [NFTokenCreateOffer 트랜잭션](../../../references/xrp-ledger/undefined-1/undefined-1/nftokencreateoffer.md)을 사용하여 생성됩니다.

_(_[_NonFungibleTokensV1\_1 수정안_](../../xrp-ledger/amendments/undefined.md)_으로 추가됨.)_

## Reserve Requirements

모든 NFTokenOffer 개체는 현재 <mark style="background-color:yellow;">NFTokenSellOffer</mark>당 2XRP, <mark style="background-color:yellow;">NFTokenBuyOffer</mark>당 2XRP의 소유자 reserve를 늘려야 합니다. 이는 계정이 완료할 의도가 없는 제안으로 원장을 스팸 처리하는 것을 방지하기 위한 것입니다.

&#x20;[NFT Reserve Requirements](https://xrpl.org/nft-reserve-requirements.html)을 참조하십시오.

## 판매 제안&#x20;

## 판매 제안 생성&#x20;

<mark style="background-color:yellow;">NFToken</mark> 객체의 소유자로서, tfSellToken 플래그를 사용하여 [NFTokenCreateOffer 트랜잭션](../../../references/xrp-ledger/undefined-1/undefined-1/nftokencreateoffer.md)을 사용하여 판매 제안을 생성할 수 있습니다. <mark style="background-color:yellow;">NFTokenID</mark>와 지불로 받을 <mark style="background-color:yellow;">금액</mark>을 제공합니다. 선택적으로 제안의 유효 기간인 만료 날짜와 <mark style="background-color:yellow;">NFToken</mark>을 구매할 수 있는 유일한 계정인 대상 계정을 지정할 수도 있습니다.

## 판매 제안 수락&#x20;

판매용으로 제공되는 <mark style="background-color:yellow;">NFToken</mark>을 구매하려면 <mark style="background-color:yellow;">NFTokenAcceptOffer</mark> 트랜잭션을 사용합니다. 소유자 계정을 제공하고 수락할 <mark style="background-color:yellow;">NFTokenOffer</mark> 객체의 <mark style="background-color:yellow;">NFTokenOfferID</mark>를 지정합니다.

## 구매 제안&#x20;

## 구매 제안 생성&#x20;

어떤 계정이든 <mark style="background-color:yellow;">NFToken</mark>을 구매하기 위해 제안을 할 수 있습니다. <mark style="background-color:yellow;">tfSellToken</mark> 플래그 없이 NFTokenCreateOffer를 사용하여 구매 제안을 생성할 수 있습니다. <mark style="background-color:yellow;">소유자</mark> 계정, <mark style="background-color:yellow;">NFTokenID</mark> 및 제안 <mark style="background-color:yellow;">금액</mark>을 제공합니다.

## 구매 제안 수락&#x20;

<mark style="background-color:yellow;">NFTokenAcceptOffer</mark> 트랜잭션을 사용하여 <mark style="background-color:yellow;">NFToken</mark>을 이전합니다. <mark style="background-color:yellow;">NFTokenOfferID</mark> 및 소유자 계정을 제공하여 거래를 완료합니다.

## 거래 모드&#x20;

<mark style="background-color:yellow;">NFToken</mark>을 거래할 때, 구매자와 판매자 간의 직접 거래 또는 중개 거래(제 삼자 계정이 판매 및 구매 제안을 매칭하여 거래를 조정) 중 선택할 수 있습니다.

직접 모드에서 거래하면 판매자가 전송을 제어할 수 있습니다. 판매자는 누구나 구매할 수 있도록 <mark style="background-color:yellow;">NFToken</mark>을 판매할 수 있도록 게시하거나 특정 계정에 <mark style="background-color:yellow;">NFToken</mark>을 판매할 수 있습니다. 판매자는 <mark style="background-color:yellow;">NFToken</mark>의 전체 가격을 받습니다.

중개 모드에서 판매자는 제 삼자 계정이 <mark style="background-color:yellow;">NFToken</mark>의 판매를 중개하도록 허용합니다. 중개자 계정은 합의된 요율로 이체 수수료를 수집합니다. 이는 중개자가 사전 투자 없이 구매자의 자금에서 중개자와 판매자에게 지불되는 단일 거래로 이루어집니다.

## 중개 모드 사용 시기&#x20;

<mark style="background-color:yellow;">NFToken</mark>생성자가 적절한 구매자를 찾는 데 시간과 인내심이 있다면, 생성자는 판매 수익을 모두 보유할 수 있습니다. 이는 구매 가격이 다양한 <mark style="background-color:yellow;">NFToken</mark> 개체를 판매하는 생성자에게 적합합니다.

반면에 생성자는 시간을 소비하여 작품을 판매하는 대신 작품을 만드는 데 시간을 할애하고 싶어할 수도 있습니다. 각각의 개별 판매을 처리하는 대신, 판매 작업을 제 삼자 중개인 계정에게 맡길 수 있습니다.

중개인을 사용하면 여러 가지 이점이 있습니다. 예를 들어 다음과 같습니다:

* 중개인은 <mark style="background-color:yellow;">NFToken</mark>의 판매 가격을 극대화하기 위해 에이전트로 작동할 수 있습니다. 중개인은 판매 가격의 일부로 중개 수수료를 지불받으므로 가격이 높을수록 중개인의 수익도 증가합니다.
* 중개인은 큐레이터 역할로서, 특정 시장, 가격대 또는 기타 기준에 따라 <mark style="background-color:yellow;">NFToken을</mark> 선별하여 중개할 수 있습니다. 이는 잠재 고객들의 구매를 유도할 수 있습니다.
* 중개인은 Opensea.io와 유사한 마켓플레이스로서 응용 프로그램 계층에서 경매 프로세스를 처리할 수 있습니다.&#x20;

## 중개 판매 워크플로우&#x20;

가장 직관적인 워크플로우에서는 창작자가 새로운 <mark style="background-color:yellow;">NFToken</mark>을 발행합니다. 창작자는 최소 허용 판매 가격을 입력하고 중개인을 대상으로 설정하여 판매 제안을 시작합니다. 잠재적인 구매자들은 <mark style="background-color:yellow;">NFToken</mark>에 대한 입찰을 하며 입찰의 대상으로 중개인을 설정합니다. 중개인은 최종적으로 선정된 입찰을 선택하고 거래를 완료하며 중개 수수료를 받습니다. 중개인은 남아있는 <mark style="background-color:yellow;">NFToken</mark>에 대한 구매 제안을 취소하는 것이 좋습니다.

<figure><img src="https://xrpl.org/img/nft-brokered-mode-with-reserve.png" alt=""><figcaption></figcaption></figure>

다른 워크플로우에서는 창작자가 판매에 대한 더 많은 통제권을 가질 수 있습니다. 이 워크플로우에서는 창작자가 새로운 <mark style="background-color:yellow;">NFToken</mark>을 발행합니다. 입찰자들은 중개인을 대상으로 설정하여 자신의 제안을 생성합니다. 중개인은 최종적으로 선정된 입찰을 선택하고 중개 수수료를 제외한 제안에 대해 창작자의 서명을 요청하기 위해 <mark style="background-color:yellow;">NFTokenCreateOffer</mark>를 사용합니다. 창작자는 요청된 제안에 서명하여 중개인을 대상으로 설정합니다. 중개인은 중개 수수료를 보유하며 <mark style="background-color:yellow;">NFTokenAcceptOffer</mark>를 사용하여 판매를 완료합니다. 중개인은 <mark style="background-color:yellow;">NFTokenCancelOffer</mark>를 사용하여 남아있는 <mark style="background-color:yellow;">NFToken</mark>에 대한 입찰을 취소합니다.

<figure><img src="https://xrpl.org/img/nft-brokered-mode-without-reserve.png" alt=""><figcaption></figcaption></figure>

소유자가 다른 계정에서 생성한 <mark style="background-color:yellow;">NFToken</mark>을 재판매할 때도 동일한 워크플로를 사용할 수 있습니다.\
