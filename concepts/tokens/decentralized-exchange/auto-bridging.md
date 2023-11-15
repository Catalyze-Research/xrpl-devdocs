# Auto-Bridging

XRP Ledger의 [탈중앙화 거래소](./)에서 두 개의 비-XRP 화폐를 교환할 수 있는 모든 [제안](offers.md)은 합성 오더북에서 XRP를 중간 화폐로 사용할 수 있습니다. 이는 _auto-bridging_ 덕분이며, 이는 모든 화폐 쌍의 유동성을 향상시키기 위해 XRP를 직접 거래하는 것보다 더 저렴할 때 XRP를 사용합니다. 이는 XRP가 XRP Ledger의 기본 암호화폐라는 성질 때문에 가능합니다. 제안 실행은 최상의 총 환율을 달성하기 위해 직접 제안과 auto-bridged 제안의 조합을 사용할 수 있습니다.

_예: Anita가 GBP를 팔고 BRL을 사기 위한 제안을 제시합니다. 그녀는 이 드문 화폐 시장에 제안이 별로 없다는 것을 발견할 수 있습니다. 좋은 환율을 가진 제안이 하나 있지만, Anita의 거래를 만족시키기에는 수량이 부족합니다. 그러나 GBP와 BRL 모두 XRP에 대한 활발하고 경쟁력 있는 시장을 가지고 있습니다. XRP Ledger의 auto-bridging 시스템은 한 트레이더에게서 GBP로 XRP를 구매한 다음, 다른 트레이더에게 XRP를 팔아 BRL을 구매함으로써 Anita의 제안을 완료하는 방법을 찾습니다. Anita는 직접 GBP:BRL 시장에서의 작은 제안과 GBP:XRP 그리고 XRP:BRL 제안을 짝지어 더 나은 복합 환율을 생성함으로써 자동적으로 가능한 최고의 환율을 얻게 됩니다._&#x20;

Auto-bridging은 모든 [OfferCreate 트랜잭션](../../../references/xrp-ledger/undefined/undefined-1/offercreate.md)에서 자동적으로 발생합니다. [결제 트랜잭션](../../../references/xrp-ledger/undefined/undefined-1/payment.md)은 기본적으로 auto-bridging를 사용하지 않지만, [경로](../paths.md) 탐색은 같은 효과를 가진 경로를 찾을 수 있습니다.
