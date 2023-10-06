# 일괄 발행

일괄적으로 NFToken 개체를 발행하는 두 가지 일반적인 방법이 있습니다: 온디맨드 발행과 스크립트를 사용한 발행입니다.

## 온디맨드 발행 (지연 발행)&#x20;

온디맨드 발행 모델을 사용할 때, XRP Ledger에서 초기 판매를 위한 NFToken 개체의 매수 또는 매도 제안을 당신과 잠재적인 고객이 생성합니다. 초기 판매를 시작할 준비가 되었을 때, 토큰을 발행하고 판매 제안을 생성하거나 매수 제안을 수락한 후 거래를 완료합니다.

## 장점&#x20;

* 미판매된 NFToken 개체를 보유하기 위한 reserve requirement이 없습니다.&#x20;
* 판매될 것으로 알고 있는 시점에서 실시간으로 NFToken 개체를 발행할 수 있습니다.&#x20;

## 단점&#x20;

NFToken 개체의 초기 판매 이전에 시장 활동은 XRP Ledger에 기록되지 않습니다. 이는 일부 애플리케이션에는 문제가 되지 않을 수 있습니다.

## 스크립트를 사용한 발행&#x20;

프로그램 또는 스크립트를 사용하여 한 번에 많은 토큰을 발행합니다. 한 그룹에서 최대 200개의 트랜잭션을 병렬로 제출할 수 있는 [Tickets](../../../references/xrp-ledger/ledger/ledger-1/ticket.md)가 도움이 될 수 있습니다.

실제적인 예는 [일괄 발행 NFTokens](../../../tutorials/undefined/javascript/nfts.md) 튜토리얼을 참조하세요.

## 장점&#x20;

* NFToken 객체가 미리 발행됩니다.&#x20;
* NFToken 객체의 초기 판매를 위한 시장 활동이 ledger에 기록됩니다.&#x20;

## 단점&#x20;

발행하는 모든 NFToken 객체에 대해 [reserve requirement](../../undefined-4/undefined/reserves.md)을 충족해야 합니다. 대략적으로 현재 reserve 비율에 따라 <mark style="background-color:yellow;">NFToken</mark> 객체당 약 1/12 XRP입니다. 충분한 reserve XRP가 없는 경우, 발행 트랜잭션이 실패하게 됩니다.
