# Reserves

XRP Ledger는 스팸 또는 악의적인 사용으로 인해 과도하게 커지는 것을 방지하기 위해 XRP로 _reserve requirements_을 적용합니다. 목표는 현재의 상용 기계에서도 현재의 ledger를 항상 RAM에 맞출 수 있도록 기술의 발전과 함께 ledger의 성장을 제한하는 것입니다.

계정을 보유하려면 해당 주소는 공유 전역 ledger에 최소한의 XRP 금액을 보유해야 합니다. 새 주소에 자금을 지원하기 위해서는 해당 주소로 충분한 XRP를 받아 reserve requirements를 충족해야 합니다. reserved XRP를 다른 사람에게 보낼 수는 없지만 [계정을 삭제함](./#undefined-3)으로써 일부 XRP를 회수할 수 있습니다.

reserve requirements은 [수수료 투표](../../undefined-1/undefined-6.md) 과정을 통해 시간이 지남에 따라 변경될 수 있으며, 여기서 유효성 검증인 새로운 reserve 설정에 동의할 수 있습니다.

## 기본 Reserve과 소유자 Reserve&#x20;

reserve requirement는 두 부분으로 구성됩니다:

* **기본 Reserve**는 ledger의 각 주소에 필요한 최소한의 XRP 금액입니다.
* **소유자 Reserve**는 해당 주소가 ledger에서 소유하는 각 객체에 대한 reserve requirement을 증가시킵니다. 항목당 비용을 _incremental reserve_라고 합니다.

Mainnet에서 현재 reserve requirement은 다음과 같습니다:

* Base Reserve: **10 XRP**
* Owner Reserve: 각 항목당 **2 XRP**

다른 네트워크에서의 reserve 다를 수 있습니다.

## Owner Reserves&#x20;

ledger에는 특정 계정이 소유하는 여러 객체(ledger entries)가 있습니다. 일반적으로 객체를 생성한 계정이 소유자(owner)가 됩니다. 각 객체는 소유자의 총 reserve requirement를 owner reserve 만큼 증가시킵니다. 객체가 ledger에서 제거되면 예치 요구 사항에는 더 이상 포함되지 않습니다.

소유자의 reserve requirement에 포함되는 객체는 다음과 같습니다: [Checks](../../undefined-3/undefined-1.md), [Deposit Preauthorizations](undefined-3.md), [Escrows](../../undefined-3/undefined-2.md), [NFT Offers](../../undefined-4/non-fungible-tokens/xrp-ledger-nft.md), [NFT Pages](../../undefined-4/non-fungible-tokens/), [Offers](../../../references/xrp-ledger/ledger/ledger-1/offer.md), [Payment Channels](../../undefined-3/undefined-4.md), [Signer Lists](undefined-1.md), [Tickets](../../undefined-3/undefined-1.md), 그리고 [신뢰선](../../undefined-4/undefined.md).

일부 특수한 경우:

* Non-Fungible Tokens(NFTs)은 각각 최대 32개의 NFT를 포함하는 페이지로 그룹화되며, 소유자 reserve은 페이지 당 적용되며 NFT 당으로 적용되지 않습니다. 페이지를 분할하고 결합하는 메커니즘으로 인해 페이지당 실제 저장되는 NFT의 수가 다를 수 있습니다. 참고[: NFTokenPage 객체의 reserve](../../../references/xrp-ledger/ledger/ledger-1/nftokenoffer.md#nftokenoffer-reserve).
* 신뢰선(<mark style="background-color:yellow;">RippleState</mark> 항목)은 두 개의 계정 간에 공유됩니다. 소유자 reserve는 둘 중 하나 또는 둘 다에 적용될 수 있습니다. 대부분의 경우, 토큰 보유자는 reserve를 부담하고 발행자는 부담하지 않습니다. 참고: [RippleState: 소유자 Reserve에 기여](../../../references/xrp-ledger/ledger/ledger-1/ripplestate.md#reserve).
* 2019년 4월에 활성화된 [MultiSignReserve 수정안](../../xrp-ledger/amendments/undefined.md#multisignreserve) 이전에 생성된 서명자 목록(Signer lists)은 여러 개의 객체로 계산됩니다. 참고: [Signer Lists 와 Reserves](../../../references/xrp-ledger/ledger/ledger-1/signerlist.md#signerlist-reserve).
* [소유자 디렉터리](../../../references/xrp-ledger/ledger/ledger-1/directorynode.md)는 계정과 관련된 모든 객체, 즉 계정이 소유하는 모든 객체를 나열하는 ledger 항목입니다. 그러나 Owner Directory 자체는 reserve에는 포함되지 않습니다.

## Reserves 조회하기&#x20;

애플리케이션은 [server\_info 메소드](../../../references/http-websocket-apis/api-1/undefined-5/server\_info.md) 또는 [server\_state 메소드](../../../references/http-websocket-apis/api-1/server-info/server\_state.md)를 사용하여 현재 기본 및 증분 reserve 값을 조회할 수 있습니다:

<table><thead><tr><th width="153">메소드</th><th width="98">단위</th><th width="239">기본 예비 필드</th><th>증분 준비금 필드</th></tr></thead><tbody><tr><td>server_info method</td><td>소수점 XRP</td><td><mark style="background-color:yellow;">validated_ledger.reserve_base_xrp</mark></td><td><mark style="background-color:yellow;">validated_ledger.reserve_inc_xrp</mark></td></tr><tr><td>server_state method</td><td>XRP의 정수 드롭</td><td><mark style="background-color:yellow;">validated_ledger.reserve_base</mark></td><td><mark style="background-color:yellow;">validated_ledger.reserve_inc</mark></td></tr></tbody></table>

계정 소유자의 reserve를 확인하려면 증분 reserve를 계정이 소유한 객체 수로 곱하세요. 계정이 소유한 객체 수를 확인하려면 [account\_info 메소드](../../../references/http-websocket-apis/api-1/undefined/account\_info.md)를 호출하고 <mark style="background-color:yellow;">account\_data.OwnerCount</mark>를 가져옵니다.

주소의 총 reserve requirement를 계산하려면 <mark style="background-color:yellow;">OwnerCount</mark>를 <mark style="background-color:yellow;">reserve\_inc\_xrp</mark>로 곱한 다음 <mark style="background-color:yellow;">reserve\_base\_xrp</mark>를 더하세요. 이것은 [파이썬에서 이 계산을 보여주는 예시](../../../tutorials/apps/python.md)입니다.

## Reserve Requirement 아래로 내려가기&#x20;

거래 처리 중에, [트랜잭션 비용](../../transactions/transaction-cost.md)이 발송 주소의 XRP 잔액 일부를 소멸시킵니다. 이로 인해 주소의 XRP 잔액이 reserve requirement 아래로 내려갈 수 있습니다. 심지어 이런 방법으로 XRP를 모두 소멸시킬 수도 있습니다.

당신의 계정이 현재의 reserve requirement보다 적은 XRP를 보유하고 있다면, 다른 사람에게 XRP를 보내거나, 계정의 reserve requirement을 늘릴 수 있는 새로운 객체를 생성할 수 없습니다. 그럼에도 불구하고, 계정은 ledger에 계속 존재하고, 충분한 XRP를 가지고 있으면 이러한 것들을 하지 않는 거래를 계속 보낼 수 있습니다. 충분한 XRP를 받아서 reserve requirement 이상으로 돌아가거나, [reserve requirement이 당신이 가지고 있는 금액 아래로 줄어들](reserves.md#reserve-requirements) 경우에도 마찬가지입니다.

{% hint style="info" %}
Tip:

당신의 주소가 reserve requirement 이하라면, 더 많은 XRP를 획득하기 위해 [OfferCreate 트랜잭션](../../../references/xrp-ledger/undefined-1/undefined-1/offercreate.md)을 보내고 reserve requirement 이상으로 되돌릴 수 있습니다. 그러나, reserve requirement 이하일 동안 [ledger에 Offer 항목](../../../references/xrp-ledger/ledger/ledger-1/offer.md)을 생성할 수 없기 때문에, 이 거래는 오더북에 이미 있는 Offer만을 소비할 수 있습니다.
{% endhint %}

## Reserve Requirements 변경&#x20;

XRP Ledger에는 reserve requirement를 조정하는 메커니즘이 있습니다. 이런 조정은 예를 들어, XRP의 장기적인 가치 변동, 일반 기계 하드웨어의 용량 향상, 또는 서버 소프트웨어 구현의 효율성 향상 등을 고려할 수 있습니다. 어떤 변경사항이든 합의 과정을 통해 승인 받아야 합니다. 더 많은 정보는 [수수료 투표](../../undefined-1/undefined-6.md)를 참조하세요.
