# 부분 결제

기본 설정에서 XRP Ledger의 [결제 트랜잭션](../../references/xrp-ledger/undefined-1/undefined-1/payment.md)에서 <mark style="background-color:yellow;">양</mark> 필드는 환율 및 이체 수수료를 고려한 정확히 전달할 금액을 지정합니다. "부분 결제" 플래그(<mark style="background-color:yellow;">tfPartialPayment</mark>)는 송금한 금액을 증가시키는 대신 수신한 금액을 감소시켜 결제를 성공시킬 수 있도록 합니다. 부분 결제는 자신에게 추가 비용을 부과하지 않고 [결제를 반환](../../tutorials/xrp-ledger/undefined.md)하는 데 유용합니다.

트랜잭션 비용으로 사용되는 XRP 금액은 어떤 종류의 트랜잭션인지에 관계없이 항상 송신자의 계정에서 공제됩니다.

부분 결제는 XRP Ledger와의 느슨한 통합을 이용하여 거래소와 게이트웨이로부터 돈을 훔치는 데 이용될 수 있습니다. 이 문서의 "[부분 결제 악용](undefined-3.md#undefined-4)" 섹션에서 이 악용 방법이 작동하는 방식과 그것을 피하는 방법에 대해 설명하고 있습니다.

## 의미론적 측면&#x20;

## 부분 결제 사용하지 않을 때

부분 결제 플래그를 사용하지 않는 결제를 보낼 때, 트랜잭션의 <mark style="background-color:yellow;">양</mark> 필드는 전달할 정확한 금액을 지정하고, <mark style="background-color:yellow;">SendMax</mark> 필드는 보낼 최대 금액과 화를 지정합니다. 결제가 <mark style="background-color:yellow;">SendMax</mark> 매개변수를 초과하지 않고도 전체 <mark style="background-color:yellow;">양</mark>을 전달할 수 없거나 다른 이유로 전체 금액을 전달할 수 없는 경우, 트랜잭션은 실패합니다. 트랜잭션 명령에서 <mark style="background-color:yellow;">SendMax</mark> 필드가 생략된 경우, <mark style="background-color:yellow;">양</mark>과 동일하다고 간주됩니다. 이 경우, 총 수수료 금액이 0인 경우에만 결제가 성공할 수 있습니다.

다시 말해:

```
Amount + (fees) = (sent amount) ≤ SendMax
```

이 공식에서 "수수료"는 송금 수수료와 환율을 의미합니다. "송금 금액"과 전달된 금액(<mark style="background-color:yellow;">양</mark>)은 다른 통화로 표시될 수 있으며, XRP Ledger의 탈중앙화 거래소에서 오퍼를 소비하여 변환할 수 있습니다.

{% hint style="info" %}
Note:

트랜잭션의 <mark style="background-color:yellow;">수수료</mark> 필드는 XRP [트랜잭션 비용](../undefined-1/undefined-2/undefined.md)을 나타내며, 이는 네트워크로 트랜잭션을 전달하기 위해 소멸됩니다. 지정된 정확한 거래 비용은 항상 송신자에서 공제되며, 어떤 유형의 결제에 대한 수수료 계산과 완전히 별개입니다.
{% endhint %}

## 부분결제를 사용할 때

부분 결제를 사용하는 결제를 보낼 때, 트랜잭션의 <mark style="background-color:yellow;">양</mark> 필드는 전달할 최대 금액을 지정합니다. 부분 결제는 수수료, 충분한 유동성 부족, 수신 계정의 신뢰 선에 충분한 공간 부족 등의 제한이 있음에도 불구하고 일부 목표 금액을 송금하는 데 성공할 수 있습니다.

선택 사항인 <mark style="background-color:yellow;">DeliverMin</mark> 필드는 배송할 최소 금액을 지정합니다. <mark style="background-color:yellow;">SendMax</mark> 필드는 부분 결제가 아닌 경우와 동일하게 작동합니다. 부분 결제 트랜잭션은 <mark style="background-color:yellow;">SendMax</mark> 금액을 초과하지 않고 <mark style="background-color:yellow;">DeliverMin</mark> 필드와 같거나 더 큰 금액을 전달하면 성공합니다. <mark style="background-color:yellow;">DeliverMin</mark>필드를 지정하지 않으면 양수 금액을 전달하여 부분 결제에 성공할 수 있습니다.

다시 말해:

```
Amount ≥ (Delivered Amount) = SendMax - (Fees) ≥ DeliverMin > 0
```

## 부분 결제 제약

부분 결제에는 다음과 같은 제한 사항이 있습니다:

* 부분 결제는 주소를 자금으로 공급할 수 없습니다. 이 경우 결과 코드 <mark style="background-color:yellow;">telNO\_DST\_PARTIAL</mark>가 반환됩니다.&#x20;
* 직접적인 XRP-to-XRP 결제는 부분 결제가 될 수 없습니다. 이 경우 결과 코드 <mark style="background-color:yellow;">temBAD\_SEND\_XRP\_PARTIAL</mark>가 반환됩니다.&#x20;
  * 하지만 XRP가 하나의 통화로 참여하는 교차 화폐 결제는 부분 결제가 될 수 있습니다.&#x20;

## delivered\_mount 필드

전달된 금액 필드 부분 결제가 실제로 얼마나 전달되었는지 이해하기 위해 성공한 Payment 트랜잭션의 메타데이터에는 <mark style="background-color:yellow;">delivered\_amount</mark> 필드가 포함됩니다. 이 필드는 전달된 실제 금액을 <mark style="background-color:yellow;">양</mark> 필드와 <mark style="background-color:yellow;">동일한 형식</mark>으로 설명합니다.

부분 결제가 아닌 경우, 트랜잭션 메타데이터의 <mark style="background-color:yellow;">delivered\_amount</mark> 필드는 트랜잭션의 <mark style="background-color:yellow;">양</mark> 필드와 동일합니다. 결제가 토큰을 전달할 때, <mark style="background-color:yellow;">delivered\_amount</mark>은 반올림으로 인해 <mark style="background-color:yellow;">양</mark> 필드와 약간 다를 수 있습니다.

다음 두 가지 기준을 **모두** 충족하는 트랜잭션에는 전달된 금액을 **사용할 수 없습니다**:

* 부분 결제인 경우.&#x20;
* 2014-01-20 이전에 확인된 분산 원장에 포함된 경우.&#x20;

이러한 조건이 모두 참이면 <mark style="background-color:yellow;">delivered\_amount</mark> 필드에 실제 금액 대신 <mark style="background-color:yellow;">unavailable</mark>이라는 문자열 값이 포함됩니다. 이 경우, 실제 전달된 금액은 트랜잭션의 메타데이터에 있는 <mark style="background-color:yellow;">AffectedNodes</mark>를 읽어 확인할 수 있습니다. 트랜잭션이 토큰을 전달하고 Amount의 <mark style="background-color:yellow;">발행자</mark>가 <mark style="background-color:yellow;">목적지 주소</mark>와 동일한 계정인 경우, 전달된 <mark style="background-color:yellow;">금액</mark>은 서로 다른 상대방에 대한 신뢰 선을 나타내는 여러 개의 <mark style="background-color:yellow;">AffectedNodes</mark> 멤버에 분할될 수 있습니다.

<mark style="background-color:yellow;">delivered\_amount</mark> 필드는 다음 위치에서 찾을 수 있습니다:

| API                                                               | Method                                                             | Field                                                                                                                                                                                                            |
| ----------------------------------------------------------------- | ------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [JSON-RPC / WebSocket](https://xrpl.org/http-websocket-apis.html) | [account\_tx ](https://xrpl.org/account\_tx.html)메소드               | `result.transactions` 배열 멤버의`meta.delivered_amount`                                                                                                                                                              |
| [JSON-RPC / WebSocket](https://xrpl.org/http-websocket-apis.html) | [tx ](https://xrpl.org/tx.html)메소드                                 | `result.meta.delivered_amount`                                                                                                                                                                                   |
| [JSON-RPC / WebSocket](https://xrpl.org/http-websocket-apis.html) | [transaction\_entry ](https://xrpl.org/transaction\_entry.html)메소드 | `result.metadata.delivered_amount`                                                                                                                                                                               |
| [JSON-RPC / WebSocket](https://xrpl.org/http-websocket-apis.html) | [ledger ](https://xrpl.org/ledger.html)메소드 (트랜잭션이 확장된 경우)          | `result.ledger.transactions` 배열 멤버의`metaData.delivered_amount` [![New in: rippled 1.2.1](https://img.shields.io/badge/New%20in-rippled%201.2.1-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.2.1) |
| [WebSocket](https://xrpl.org/http-websocket-apis.html)            | 트랜잭션 구독                                                            | `meta.delivered_amount` 의 구독 메세지[![New in: rippled 1.2.1](https://img.shields.io/badge/New%20in-rippled%201.2.1-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.2.1)                                |
| ripple-lib v1.x                                                   | `getTransaction`메소드                                                | `outcome.deliveredAmount`                                                                                                                                                                                        |
| ripple-lib v1.x                                                   | `getTransaction`메소드                                                | array members' `outcome.deliveredAmount`                                                                                                                                                                         |

## 부분 결제 악용

만약 금융 기관의 XRP Ledger와의 통합이 Payment의 <mark style="background-color:yellow;">금액</mark> 필드가 항상 전달된 전체 금액이라고 가정한다면, 악의적인 사용자는 그 가정을 악용하여 기관으로부터 돈을 훔칠 수 있습니다. 이 악용은 게이트웨이, 거래소 또는 상인에 대해 사용될 수 있으며, 해당 기관의 소프트웨어가 부분 결제를 올바르게 처리하지 않는 경우에만 사용할 수 있습니다.

**들어오는 Payment 트랜잭션을 처리하는 올바른 방법은 Amount 필드가 아닌 **<mark style="background-color:yellow;">**delivered\_amount**</mark>** 메타데이터 필드를 사용하는 것입니다**. 이렇게 하면 기관은 _실제로_ 받은 <mark style="background-color:yellow;">금액</mark>에 대해 오해하지 않게 됩니다.

## 악용 시나리오 단계&#x20;

취약한 금융 기관을 악용하기 위해 악의적인 사용자는 다음과 같은 작업을 수행합니다:

1. 악의적인 사용자는 기관에 대해 Payment 트랜잭션을 보냅니다. 이 트랜잭션에는 큰 <mark style="background-color:yellow;">금액</mark> 필드가 있으며 <mark style="background-color:yellow;">**tfPartialPayment**</mark> 플래그가 활성화되어 있습니다.&#x20;
2. 부분 결제가 성공합니다 (결과 코드 <mark style="background-color:yellow;">tesSUCCESS</mark>)만 실제로는 지정된 통화의 매우 작은 <mark style="background-color:yellow;">금액</mark>을 전달합니다. 취약한 기관은 <mark style="background-color:yellow;">플래그</mark> 필드나 delivered\_amount 메타데이터 필드를 확인하지 않고 트랜잭션의 Amount 필드를 읽습니다.&#x20;
3. 취약한 기관은 <mark style="background-color:yellow;">플래그</mark> 필드나 <mark style="background-color:yellow;">delivered\_amount</mark> 메타데이터 필드를 확인하지 않고 트랜잭션의 <mark style="background-color:yellow;">금액</mark> 필드를 읽습니다.&#x20;
4. 취약한 기관은 기관 자체 ledger과 같은 외부 시스템에서 악의적인 행위자에게 전체 <mark style="background-color:yellow;">금액</mark>에 대한 크레딧을 제공하지만, XRP Ledger에 훨씬 적은 <mark style="background-color:yellow;">delivered\_amount를 받</mark>게 됩니다.
5. 악의적인 사용자는 취약한 기관이 불일치에 대해 알아차리기 전에 잔액을 가능한 한 다른 시스템으로 인출합니다.&#x20;
   1. 블록체인 거래는 일반적으로 되돌릴 수 없기 때문에 악의적인 행위자는 일반적으로 잔액을 비트코인 등 다른 암호화폐로 전환하는 것을 선호합니다. 법정 화폐 시스템으로 인출할 경우, 금융기관은 거래가 처음 실행된 후 며칠이 지나면 거래를 되돌리거나 취소할 수 있습니다.
   2. 거래소의 경우, 악의적인 사용자는 XRP 잔액을 XRP Ledger로 직접 인출할 수도 있습니다.&#x20;

판매자의 경우 작업 순서는 약간 다르지만 개념은 동일합니다:

1. 악의적인 사용자는 대량의 상품이나 서비스를 구매하도록 요청합니다.&#x20;
2. 취약한 상인은 악의적인 사용자에게 그 상품과 서비스의 가격에 해당하는 금액으로 청구서를 보냅니다.&#x20;
3. 악의적인 사용자는 상인에게 Payment 트랜잭션을 보냅니다. 이 트랜잭션에는 큰 <mark style="background-color:yellow;">금액</mark> 필드가 있으며 <mark style="background-color:yellow;">**tfPartialPayment**</mark> 플래그가 활성화되어 있습니다.&#x20;
4. 부분 결제가 성공합니다 (결과 코드 <mark style="background-color:yellow;">tesSUCCESS</mark>)만 실제로는 지정된 통화의 매우 작은 금액만 전달합니다.&#x20;
5. 취약한 상인은 Flags 필드나 <mark style="background-color:yellow;">delivered\_amount</mark> 메타데이터 필드를 확인하지 않고 트랜잭션의 <mark style="background-color:yellow;">금액</mark> 필드를 읽습니다.&#x20;
6. 취약한 상인은 XRP Ledger에서 훨씬 작은 <mark style="background-color:yellow;">delivered\_amount</mark>를 받았음에도 불구하고 <mark style="background-color:yellow;">금액</mark>이 지불되었다고 간주하고 악의적인 사용자에게 상품이나 서비스를 제공합니다.&#x20;
7. 악의적인 사용자는 상인이 불일치에 대해 알아차리기 전에 상품과 서비스를 사용하거나 판매하거나 탈취합니다.&#x20;

## 추가적인 완화 조치&#x20;

들어오는 트랜잭션을 처리할 때 <mark style="background-color:yellow;">delivered\_amount</mark> 필드를 사용하는 것만으로도 이 악용을 피할 수 있습니다. 그러나 추가적인 적극적인 비즈니스 관행을 통해 이와 유사한 악용의 가능성을 회피하거나 완화할 수도 있습니다. 예를 들어:

* 출금 처리에 대한 비즈니스 로직에 추가적인 정상성 검사를 추가하십시오. XRP Ledger에서 보유한 총 잔액이 예상 자산 및 채무와 일치하지 않는 경우 출금을 처리하지 마십시오.&#x20;
* "고객을 알아라" 가이드라인을 따르고 고객의 신원을 엄격히 확인하십시오. 사전에 악의적인 사용자를 인식하고 차단하거나 시스템을 악용한 악의적인 사용자에 대해 법적 조치를 취할 수 있을 수 있습니다.
