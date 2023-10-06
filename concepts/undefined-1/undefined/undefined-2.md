# 티켓

_(_[_TicketBatch_](../../xrp-ledger/amendments/undefined.md#ticketbatch)[ _수정안_](../../xrp-ledger/amendments/undefined.md#ticketbatch)_에 의해 추가됨.)_

XRP Ledger의 티켓은 거래의 [일련 번호](../../../references/xrp-ledger/undefined/#undefined-3)를 당장 보내지 않고 미리 설정해두는 방법입니다. 티켓을 사용하면 거래를 일반적인 순서 외에 보낼 수 있습니다. 이에 대한 한 가지 사용 사례는 필요한 서명을 모으는 데 시간이 걸릴 수 있는 [다중 서명](undefined-1.md) 거래를 허용하는 것입니다: 티켓을 사용하는 거래에 대해 서명을 수집하는 동안에도, 다른 거래를 계속 보낼 수 있습니다.

## 배경

[트랜잭션](../../transactions/)에는 일련 번호가 있어서 주어진 거래는 한 번 이상 실행되지 않도록 합니다. 일련 번호는 또한 주어진 거래가 유일하게 하도록 합니다: 같은 금액의 돈을 동일한 사람에게 여러 번 보낼 경우, 일련 번호는 매번 다르게 보장되는 한 가지 상세 정보입니다. 마지막으로, 일련 번호는 일부 거래가 네트워크 전체에 걸쳐 보내질 때 순서가 뒤섞여 도착하더라도 거래를 일관된 순서로 놓는 우아한 방법을 제공합니다.

그러나, 일련 번호가 너무 제한적인 일부 상황이 있습니다. 예를 들어:

* 둘 이상의 사용자가 각각 독립적으로 거래를 보낼 수 있는 권한을 가지고 계정에 대한 접근을 공유합니다. 이 사용자들이 먼저 협조하지 않고 거의 동시에 거래를 보내려고 시도하면, 그들은 각각 다른 거래에 대해 같은 일련 번호를 사용하려고 시도할 수 있고, 성공할 수 있는 것은 하나 뿐입니다.&#x20;
* 미리 거래를 준비하고 서명한 후에 이를 안전한 저장소에 저장하여 특정 이벤트가 발생할 경우 언제든지 실행할 수 있게 하려고 할 수 있습니다. 그러나, 그 사이에 계정을 평소와 같이 계속 사용하려는 경우, 미리 설정된 거래가 필요로 할 일련 번호를 알 수 없습니다.&#x20;
* [여러 사람이 거래를 유효하게 하기 위해 서명](undefined-1.md)해야 할 때, 한 번에 둘 이상의 거래를 계획하는 것이 어려울 수 있습니다. 거래에 개별 일련 번호를 부여하면, 이전 거래에 모든 사람이 서명한 후에야 나중에 번호가 매겨진 거래를 보낼 수 있습니다. 하지만 만약 보류 중인 각 거래에 같은 일련 번호를 사용한다면, 성공할 수 있는 거래는 하나 뿐입니다.&#x20;

티켓은 이러한 문제를 모두 해결하기 위해 일련 번호를 미리 설정하여 나중에, 그들의 보통 순서 외에 사용할 수 있도록 하지만, 그래도 각각 한 번 이상은 아니게 하는 방법을 제공합니다.

## 티켓은 예약된 일련 번호입니다&#x20;

티켓은 나중에 사용하기 위해 일련 번호가 설정되었음을 나타내는 기록입니다. 계정은 먼저 [TicketCreate 트랜잭션](../../../references/xrp-ledger/undefined-1/undefined-1/ticketcreate.md)을 보내서 하나 이상의 일련 번호를 티켓으로 설정해놓습니다. 이는 [ledger의 상태 데이터](../ledgers.md)에 각 예약된 일련 번호에 대한 기록을 만들어, [티켓 객체](../../../references/xrp-ledger/ledger/ledger-1/ticket.md) 형태로 저장합니다.

티켓은 그것들을 생성하기 위해 설정해 놓은 일련 번호를 사용하여 번호가 매겨집니다. 예를 들어, 계정의 현재 일련 번호가 101이고 3개의 티켓을 생성하면, 그 티켓들은 티켓 일련 번호 102, 103, 104를 가지게 됩니다. 이렇게 하면 계정의 일련 번호가 105로 증가하게 됩니다.

<figure><img src="https://xrpl.org/img/ticket-creation.svg" alt=""><figcaption></figcaption></figure>

나중에 일련 번호 대신 특정 티켓을 사용하여 거래를 보낼 수 있습니다. 이렇게 하면 ledger의 상태 데이터에서 해당 티켓이 제거되고 계정의 일반적인 일련 번호는 변경되지 않습니다. 또한 티켓을 사용하지 않고 일반 일련 번호를 사용하여 거래를 보낼 수도 있습니다. 언제든지 어떤 순서로든 이용 가능한 티켓을 사용할 수 있지만, 각 티켓은 한 번만 사용할 수 있습니다.

<figure><img src="https://xrpl.org/img/ticket-usage.svg" alt=""><figcaption></figcaption></figure>

위의 예를 계속하면, 105번 일련 번호 또는 생성한 세 개의 티켓 중 하나를 사용하여 거래를 보낼 수 있습니다. 티켓 103을 사용하여 거래를 보내면, 이를 통해 ledger에서 티켓 103이 삭제됩니다. 그 후에는 105번 일련 번호, 티켓 102, 또는 티켓 104를 사용하여 다음 거래를 할 수 있습니다.

{% hint style="info" %}
Caution:\
각 티켓은 [소유자 reserve](reserves.md)에 대해 별도의 항목으로 계산되므로 각 티켓에 대해 2개의 XRP를 설정해야 합니다. (티켓을 사용한 후에 XRP는 다시 사용 가능해집니다.) 한 번에 많은 수의 티켓을 생성하면 이 비용이 빠르게 누적될 수 있습니다.
{% endhint %}

일련 번호와 마찬가지로, 거래가 컨센서스에 의해 확인된 경우에만 티켓이 소비됩니다. 그러나 원래 의도한 작업을 수행하지 못한 거래도 [tec 클래스 결과 코드](../../../references/xrp-ledger/undefined-1/pseudo-transactions/undefined/tec-codes.md)를 가진 [컨센서스](../../undefined-4/undefined.md)에 의해 확인될 수 있습니다.

계정이 사용할 수 있는 티켓을 조회하려면 [account\_objects 메소드](../../../references/http-websocket-apis/api-1/undefined/account\_objects.md)를 사용하십시오.

## 제한 사항&#x20;

어떤 계정도 티켓을 생성하고 어떤 종류의 거래에도 사용할 수 있습니다. 그러나 일부 제한사항이 적용됩니다:

* 각 티켓은 한 번만 사용할 수 있습니다. 같은 티켓 일련 번호를 사용할 수 있는 여러 다른 후보 거래가 있을 수 있지만, 그 후보 중 하나만 컨센서스에 의해 검증될 수 있습니다.
* 각 계정은 ledger에 한 번에 250개 이상의 티켓을 가질 수 없습니다. 한 번에 250개 이상의 티켓을 생성할 수도 없습니다.
* 티켓을 사용하여 더 많은 티켓을 생성할 수 있습니다. 그렇게 하면 사용한 티켓은 한 번에 가질 수 있는 티켓의 총 수에 포함되지 않습니다.&#x20;
* 각 티켓은 [소유자 reserve](reserves.md)로 계산되므로, 아직 사용하지 않은 각 티켓에 대해 2개의 XRP를 설정해야 합니다. 티켓을 사용한 후 XRP를 다시 사용할 수 있습니다.
* 개별 ledger 내에서, 티켓을 사용하는 거래는 동일한 발신자의 다른 거래 이후에 실행됩니다. 계정이 같은 ledger 버전에서 티켓을 사용하는 여러 거래를 가지고 있다면, 그 티켓들은 티켓 일련 번호가 가장 낮은 것부터 가장 높은 것까지 순서대로 실행됩니다. (자세한 정보는 컨센서스의 [규범 순서](../../undefined-4/undefined.md)에 대한 문서를 참조하십시오.)&#x20;
* 티켓을 "취소"하려면, 티켓을 사용해 [no-op](../../undefined-4/undefined-4.md) [AccountSet 트랜잭션](../../../references/xrp-ledger/undefined-1/undefined-1/accountset.md)을 수행하기 위해 티켓을 사용하십시오. 이렇게 하면 그 reserve requirement를충족할 필요 없이 티켓이 삭제됩니다.

## 참고 <a href="#see-also" id="see-also"></a>

* **Concepts:**
  * [Multi-Signing](https://xrpl.org/multi-signing.html)
* **Tutorials:**
  * [Use Tickets](https://xrpl.org/use-tickets.html)
* **References:**
  * [TicketCreate transaction](https://xrpl.org/ticketcreate.html)
  * [Transaction Common Fields](https://xrpl.org/transaction-common-fields.html)
  * [Ticket object](https://xrpl.org/ticket.html)
  * [account\_objects method](https://xrpl.org/account\_objects.html)

