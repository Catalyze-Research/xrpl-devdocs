# 계정 삭제

XRP 원장의 계정 소유자는 [AccountDelete 트랜잭션](../../references/xrp-ledger/undefined/undefined-1/accountdelete.md)을 보내서 원장에서 계정 및 관련 항목을 삭제할 수 있으며, 계정의 대부분의 남아있는 XRP 잔액을 다른 계정으로 전송합니다. 계정의 무분별한 생성 및 삭제를 방지하기 위해, 계정을 삭제하려면 [트랜잭션 비용](../transactions/transaction-cost.md)으로 평소보다 많은 XRP를 소모해야 합니다.

계정과 관련된 몇몇 유형의 원장 항목은 계정이 삭제되는 것을 차단합니다. 예를 들어, 대체 가능한 토큰의 발행자는 해당 토큰의 잔액이 0이 아닌 동안 삭제될 수 없습니다.

계정이 삭제된 후에는 계정을 생성하는 일반적인 방법으로 원장에서 다시 생성할 수 있습니다. 삭제되었다가 다시 생성된 계정은 처음 생성된 계정과 다르지 않습니다.

## 요구사항

삭제하기 위해 계정은 다음의 요구사항을 충족해야 합니다:

* 계정의 <mark style="background-color:yellow;">Sequence</mark> 번호와 256을 더한 값이 현재 [Ledger Index](../../references/xrp-ledger/basic-data-types/)보다 작아야 합니다.
* 계정이 아래의 [ledger 객체 유형](../../references/xrp-ledger/ledger-ledger-data-formats/ledger/)들과 연결되어 있지 않아야 합니다(발신자 또는 수신자로서):
  * <mark style="background-color:yellow;">Escrow.</mark>
  * <mark style="background-color:yellow;">PayChannel.</mark>
  * <mark style="background-color:yellow;">RippleState.</mark>
  * <mark style="background-color:yellow;">Check.</mark>
* ledger에서 계정이 소유하는 객체가 1000개 미만이어야 합니다.
* [AccountDelete 트랜잭션](../../references/xrp-ledger/undefined/undefined-1/accountdelete.md)은 한 개의 항목에 대한 [소유자 reserve](reserves.md)(현재 2 XRP) 이상의 특별한 [트랜잭션 비용](../transactions/transaction-cost.md)을 지불해야 합니다.

계정이 삭제된 후에는, [계정을 생성](undefined-3.md#undefined)하는 일반적인 방법을 통해 ledger에 다시 생성될 수 있습니다. 삭제되었다가 다시 생성된 계정은 처음 생성된 계정과 다르지 않습니다.

{% hint style="info" %}
Warning:

[AccountDelete 트랜잭션](../../references/xrp-ledger/undefined/undefined-1/accountdelete.md)의 트랜잭션 비용은 계정이 삭제될 수 있는 요구사항을 충족하지 않아 트랜잭션이 실패하더라도 검증된 ledger에 트랜잭션이 포함되면 항상 적용됩니다. 계정을 삭제할 수 없는 경우에 고비용의 트랜잭션 비용을 지불할 가능성을 크게 줄이기 위해, <mark style="background-color:yellow;">fail\_hard</mark> 옵션이 활성화된 상태로 [트랜잭션을 제출](../../references/http-websocket-apis/api-1/undefined-1/submit.md)하세요.
{% endhint %}

비트코인과 많은 다른 암호화폐와는 달리, XRP Ledger의 공개 ledger 체인의 각 새 버전에는 ledger의 전체 상태가 포함되어 있으며, 이는 각 새 계정에 따라 크기가 증가합니다. 이러한 이유로, 필요한 경우가 아니라면 새로운 XRP Ledger 계정을 생성하지 않는 것이 좋습니다. 계정을 삭제함으로써 계정의 10 XRP [reserve](reserves.md) 중 일부를 회수할 수 있지만, 그렇게 하기 위해서는 최소한 2 XRP를 소멸시켜야 합니다.

많은 사용자를 대신해서 가치를 송수신하는 기관은 [**출발 태그**와 **데스티네이션 태그**](../transactions/source-and-destination-tags.md)를 사용하여 고객에게 보내고 받는 결제를 구분할 수 있으며, 이를 통해 XRP Ledger에서 한 개(또는 소수)의 계정만을 사용할 수 있습니다.
