# 수정안

수정안 시스템은 XRP Ledger에서 트랜잭션 처리에 영향을 주는 변경 사항을 승인하기 위해 컨센서스 프로세스를 사용합니다. 완전한 기능을 갖춘 트랜잭션 처리 변경 사항은 수정안으로 도입되며, 그 후 유효성 검증인이 이러한 변경 사항에 대해 투표합니다. 수정안이 2주 동안 80% 이상의 지지를 받으면 수정안이 통과되고 해당 변경 사항이 이후의 모든 ledger 버전에 영구적으로 적용됩니다. 통과된 수정안을 비활성화하려면 새로운 수정안이 필요합니다.

{% hint style="info" %}
Note:

트랜잭션 처리를 변경하는 버그 수정도 수정안이 필요합니다.
{% endhint %}

## 수정안 프로세스

매 256번째 ledger를 플래그 ledger라고 합니다. **플래그** ledger에는 특별한 내용이 없지만 수정안 프로세스가 이루어집니다.

1. **플래그 ledger -1:** <mark style="background-color:yellow;">rippled</mark> 검증인이 유효성 검증 메시지를 보낼 때 수정안 투표도 제출합니다.
2. **플래그 ledger:** 서버는 신뢰할 수 있는 검증인의 투표를 해석합니다.
3. **플래그 ledger +1:** 서버는 발생한 내용에 따라 <mark style="background-color:yellow;">EnableAmendment</mark> 의사 트랜잭션과 플래그를 삽입합니다:

* <mark style="background-color:yellow;">tfGotMajority</mark> 플래그는 수정안이 80% 이상의 지지를 받았음을 의미합니다.
* <mark style="background-color:yellow;">tfLostMajority</mark> 플래그는 수정안에 대한 지지가 80% 이하로 감소했음을 의미합니다.
* 플래그가 없으면 수정안이 활성화됩니다.

{% hint style="info" %}
Note:&#x20;

수정안이 요구하는 2주 기간에 80% 지지를 잃을 수도 있습니다. 이 경우, 두 가지 시나리오에 대해 <mark style="background-color:yellow;">EnableAmendment</mark> 의사 트랜잭션이 추가됩니다. 그러나 수정안은 최종적으로 활성화됩니다.
{% endhint %}

4. **플래그 ledger +2:** 활성화된 수정안은 이 ledger 이후의 트랜잭션에 적용됩니다.

## 수정안 투표&#x20;

각 <mark style="background-color:yellow;">rippled</mark> 버전은 알려진 수정안 목록과 해당 수정안을 구현하는 코드로 컴파일됩니다. <mark style="background-color:yellow;">rippled</mark> 검증인의 운영자는 서버를 각 수정안에 대해 투표하도록 구성할 수 있으며, 언제든지 변경할 수 있습니다. 운영자가 투표를 선택하지 않으면 서버는 소스 코드에서 정의된 기본 투표를 사용합니다.

{% hint style="info" %}
Note:

기본 투표는 소프트웨어 버전 간에 변경될 수 있습니다. [![Updated in: rippled 1.8.1](https://img.shields.io/badge/Updated%20in-rippled%201.8.1-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.8.1)
{% endhint %}

수정안은 신뢰할 수 있는 검증인의 80% 이상의 지지를 2주간 유지해야 활성화됩니다. 지지가 80% 미만으로 떨어지면 수정안은 일시적으로 거부되며, 2주 기간이 다시 시작됩니다. 수정안은 영구적으로 활성화되기 전에 여러 차례 과반수를 얻거나 잃을 수 있습니다.

소스 코드가 제거되고 활성화되지 않은 수정안은 네트워크에서 **거부된 것**으로 간주됩니다.

## 수정안 차단 서버

수정안 차단은 XRP Ledger 데이터의 정확성을 보호하기 위한 보안 기능입니다. 수정안이 활성화되면 해당 수정안의 소스 코드가 없는 이전 버전의 <mark style="background-color:yellow;">rippled</mark>를 실행하는 서버는 네트워크의 규칙을 이해할 수 없게 됩니다. ledger 데이터를 추측하고 잘못 해석하지 않기 위해 이러한 서버는 **수정안 차단 상태**가 되어 다음 작업을 수행할 수 없습니다:

* ledger의 유효성을 결정할 수 없습니다.&#x20;
* 트랜잭션을 제출하거나 처리할 수 없습니다.&#x20;
* 컨센서스 프로세스에 참여할 수 없습니다.&#x20;
* 향후 수정안에 대해 투표할 수 없습니다.&#x20;

<mark style="background-color:yellow;">rippled</mark> 서버의 투표 구성은 수정안 차단 여부에 영향을 주지 않습니다. <mark style="background-color:yellow;">rippled</mark> 서버는 항상 네트워크의 나머지에서 활성화된 수정안을 따르므로 차단은 규칙 변경을 이해할 코드를 보유하고 있는지 여부에만 기초합니다. 이는 서버를 병렬 네트워크에 연결하고 다른 수정안이 활성화된 경우에도 수정안 차단 상태가 될 수 있다는 것을 의미합니다. 예를 들어, XRP Ledger Devnet은 일반적으로 실험적인 수정안이 활성화되어 있습니다. 최신 제품 릴리스를 사용하는 경우 서버에는 해당 실험적인 수정안을 위한 코드가 없을 가능성이 높습니다.

최신 버전의 <mark style="background-color:yellow;">rippled</mark>로 업그레이드하여 수정안 차단 상태인 서버를 해제할 수 있습니다.

## 수정안 폐기&#x20;

수정안이 활성화되면 이전 버전의 <mark style="background-color:yellow;">rippled</mark>에 대한 사전 수정안 동작의 소스 코드가 유지됩니다. 이전 코드를 보존하는 등의 사용 사례가 있지만, 수정안과 레거시 코드를 추적하면 시간이 지남에 따라 복잡성이 증가합니다.

[XRP Ledger Standard 11d](https://github.com/XRPLF/XRPL-Standards/discussions/19)에서는 오래된 수정안과 관련된 사전 수정안 코드를 폐기하는 프로세스를 정의합니다. Mainnet에서 2년 동안 수정안이 활성화된 후에는 해당 수정안을 폐기할 수 있습니다. 수정안을 폐기하면 해당 수정안이 핵심 프로토콜의 일부가 되며, 더 이상 추적되거나 수정안으로 처리되지 않으며, 모든 사전 수정안 코드가 제거됩니다.
