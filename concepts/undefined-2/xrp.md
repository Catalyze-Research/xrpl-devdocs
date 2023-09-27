# XRP 직접 결제

금융 시스템의 기반은 가치를 이동시키는 것, 다시 말해 결제입니다. XRP Ledger에서 가장 간단한 결제 유형은 XRP-to-XRP의 직접적인 결제로, XRP Ledger 내의 한 계정에서 다른 계정으로 XRP를 직접적으로 이동시킵니다.

## XRP-to-XRP 직접 결제에 대해

일반적으로 XRP Ledger의 어떤 주소든 다른 어떤 주소로 직접적으로 XRP를 보낼 수 있습니다. 받는 쪽 주소는 일반적으로 destination address라고 하며, 보내는 쪽 주소는 source address라고 합니다. XRP를 직접적으로 보내기 위해 송신자는 [Payment Transaction](../../references/xrp-ledger/undefined-1/undefined-1/payment.md)을 사용하며, 다음과 같이 간결할 수 있습니다.

```json
{
  "TransactionType": "Payment",
  "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
  "Destination": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
  "Amount": "13000000"
}
```

이 거래 명령은 다음을 의미합니다: <mark style="background-color:yellow;">rf1Bi...</mark>에서 <mark style="background-color:yellow;">ra5nK...</mark>로 정확히 13 XRP를 보내시오. 거래가 성공적으로 처리되면 정확히 그렇게 수행됩니다. 새로운 ledger 버전이 [유효화](../undefined-4/undefined.md)되기까지 일반적으로 약 4초가 소요되기 때문에, 거래가 성공적으로 생성되고 제출되며 실행되며 최종 결과가 나올 때까지 8초 이내에 완료될 수 있습니다. 현재 진행 중인 ledger 버전 이후에 큐에 들어간다고 하더라도 마찬가지입니다.

{% hint style="info" %}
[Payment Transaction 유형](../../references/xrp-ledger/undefined-1/undefined-1/payment.md)은 [교차 화폐 결제](undefined.md)와 [부분 결제](undefined-3.md)를 포함하여 일부 특수한 종류의 결제에도 사용될 수 있습니다. 부분 결제의 경우, 거래가 매우 작은 금액만 전달되었더라도 <mark style="background-color:yellow;">양</mark>에 큰 금액의 XRP가 표시될 수 있습니다. 고객에게 잘못된 금액이 입금되는 것을 방지하는 방법은 [부분 결제 악용](undefined-3.md#undefined-4)을 참조하세요.
{% endhint %}

직접 XRP-to-XRP 결제는 부분 결제가 불가능하지만, 부분 결제는 다른 출처 통화로부터 전환된 후 XRP를 전달할 수 있습니다.

## 펀딩 계정

수학적으로 유효한 모든 주소는 최소 계정 [reserve](../undefined-1/undefined/reserves.md)를 충족할 수 있는 충분한 XRP를 지급하는 한, XRP Ledger에 해당 주소에 대한 기록이 미리 존재하지 않더라도 지급을 받을 수 있습니다. 결제가 충분한 XRP를 전달하지 못하면 실패합니다.

더 많은 정보는 [계정](../undefined-1/undefined/)을 참조하세요.

## 주소 재사용

XRP Ledger에서 결제를 받을 수 있는 주소는 영구적이며, XRP로 예치된 사용할 수 없는 상당한 [reserve requirement](https://xrpl.org/reserves.html) 필요합니다. 이는 다른 블록체인 시스템과는 달리 각 거래마다 다른 일회용 주소를 사용하는 것은 좋은 생각이 아닙니다. XRP Ledger의 최선의 사용 방법은 여러 거래에 동일한 주소를 재사용하는 것입니다. 해당 주소를 정기적으로 사용하는 경우(특히 인터넷에 연결된 서비스에서 관리하는 경우) 정기적으로 키를 설정하고 [일반 키](../undefined-1/undefined/undefined.md)를 미리 변경하여 키 유출 위험을 줄여야 합니다.

송신자로서는 수령자가 이전 결제에서 사용한 주소와 동일한 주소를 사용한다고 가정하는 것이 좋지 않습니다. 때로는 보안 침해가 발생하여 개인 또는 기업이 주소를 변경해야 할 수 있습니다. 자금을 보내기 전에 수령자에게 현재의 수령 주소를 요청하여 침해당한 이전 주소를 통제한 악성 사용자에게 실수로 자금을 보내지 않도록 해야 합니다.

## XRP 직접 결제의 처리 방법

비교적 고수준에서 XRP Ledger의 거래 처리 엔진은 다음과 같이 XRP 직접 결제를 적용합니다:

1. [Payment Transaction](../../references/xrp-ledger/undefined-1/undefined-1/payment.md)의 매개변수를 유효성 검사합니다. 거래가 XRP를 보내고 전달하는 구조로 구성되어 있다면, 거래 처리 엔진은 직접적인 XRP-to-XRP 결제로 인식합니다. 유효성 검사에는 다음이 포함됩니다:

* 모든 필드가 올바르게 형식화되어 있는지 확인합니다. 예를 들어, XRP 직접 결제의 경우 <mark style="background-color:yellow;">양</mark> 필드는 XRP의 drop이어야 합니다.
* 송신 주소가 XRP Ledger에서 자금을 보유한 [계정](../undefined-1/undefined/)인지 확인합니다.
* 제공된 모든 서명이 송신 주소에 대해 유효한지 확인합니다.
* destination address가 송신 주소와 다른지 확인합니다. (다른 [데스티네이션 태그](../undefined-1/undefined-2/undefined-2.md)를 사용하여 동일한 주소로 보내는 것만으로는 충분하지 않습니다.)
* 송신자가 결제를 보내기에 충분한 XRP 잔액을 보유하고 있는지 확인합니다.
* 모든 검사 중 하나라도 실패하면 결제가 실패합니다.

2. 수령 주소가 자금을 보유하고 있는지 확인합니다.

* 수령 주소가 자금을 보유하고 있다면, 엔진은 입금 허가 또는 <mark style="background-color:yellow;">RequireDest</mark>와 같은 추가적인 결제 수령 요구 사항을 확인합니다. 결제가 이러한 추가 요구 사항 중 어느 하나를 충족하지 않으면 결제가 실패합니다.
* 수령 주소가 자금을 보유하고 있지 않으면, 결제가 최소 [계정 reserve](../undefined-1/undefined/reserves.md)를 충족시키기에 충분한 XRP를 전달하는지 확인합니다. 그렇지 않으면 결제가 실패합니다.

3. 엔진은 <mark style="background-color:yellow;">양</mark> 필드에 지정된 XRP 금액에 [트랜잭션 비용](../undefined-1/undefined-2/undefined.md)으로 파괴될 XRP를 더하여 송신 계정의 XRP를 차감하고 동일한 금액을 수신 계정에 입금합니다. 필요한 경우, 수신 주소에 대한 새로운 계정([AccountRoot 객체](../../references/xrp-ledger/ledger/ledger-1/accountroot.md))을 생성합니다. 새로운 계정의 시작 잔액은 결제의 <mark style="background-color:yellow;">Amount</mark>와 동일합니다. 엔진은 거래의 메타데이터에 전달된 금액 <mark style="background-color:yellow;">delivered\_amount</mark> 필드를 추가하여 얼마가 전달되었는지 나타냅니다. 받은 XRP의 양을 확인할 때는 <mark style="background-color:yellow;">양</mark> 필드가 아닌 delivered\_amount를 항상 사용해야 하며, (크로스 화폐 "부분 결제"는 Amount 필드에 명시된 것보다 적은 XRP를 전달할 수 있습니다.) 자세한 내용은 [부분 결제](undefined-3.md)를 참조하십시오.

## 다른 결제 유형과의 비교

* XRP **직접 결제**는 XRP를 하나의 거래로 보내고 받는 유일한 방법입니다. 속도, 간편성, 저렴한 비용의 좋은 균형을 제공합니다.
* [교차 화폐 결제](undefined.md)도 [Payment](../../references/xrp-ledger/undefined-1/undefined-1/payment.md) Transaction 유형을 사용하지만, XRP-to-XRP를 제외한 어떤 조합의 XRP 및 비-XRP [토큰](../undefined-3/)을 전송할 수 있습니다. 또한 [부분 결제](undefined-3.md)도 가능합니다. 교차 화폐 결제는 XRP로 표시되지 않은 결제나 [탈중앙화 거래소](../dex/)에서의 확정차익 기회를 활용하는 데 적합합니다.
* [수표](undefined-1.md)는 송신자가 즉시 자금을 이체하지 않고 의무를 설정할 수 있는 기능입니다. 수령자는 수표를 만료되기 전에 언제든지 현금화할 수 있지만, 금액은 보장되지 않습니다. 수표는 XRP 또는 [토큰](../undefined-3/)을 보낼 수 있습니다. 수표는 수령자가 결제를 요구할 권한을 갖도록 합니다.
* [에스크로](undefined-2.md)는 특정 조건이 충족될 때 지정된 XRP를 청구할 수 있는 기능입니다. XRP 금액은 완전히 보장되며, 에스크로가 만료되지 않는 한 송신자는 다른 용도로 사용할 수 없습니다. 에스크로는 대규모 스마트 계약에 적합합니다.
* [결제 채널](undefined-4.md)은 XRP를 보관합니다. 수신자는 서명된 인증서를 사용하여 채널에서 XRP를 대량으로 청구할 수 있습니다. 개별 인증서는 XRP Ledger 거래를 전송하지 않고도 확인할 수 있습니다. 결제 채널은 매우 높은 거래량의 소액 결제나 "스트리밍" 결제에 적합합니다.
