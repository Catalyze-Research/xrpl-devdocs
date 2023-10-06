# 트랜잭션 비용(Transaction Cost)

스팸 및 서비스 거부 공격으로 인해 XRP Ledger가 방해받는 것을 방지하기 위해, 각 거래는 약간의 XRP를 파괴해야 합니다. 이 거래 비용은 네트워크 부하와 함께 증가하여 네트워크를 고의적이거나 실수로 과부하 상태로 만드는 것을 매우 비싸게 만드는 것이 목표입니다.

모든 거래는 거래 비용을 지불하기 위해 [얼마나 많은 XRP를 파괴할지](transaction-cost.md#undefined-7) 명시해야 합니다.

## 현재 거래 비용(Current Transaction Cost)

표준 거래에 대해 네트워크에서 요구하는 현재 최소 거래 비용은 **0.00001 XRP** (10 드롭)입니다. 이는 평소보다 높은 부하 때문에 때때로 증가합니다.

<mark style="background-color:yellow;">rippled</mark>에서 현재 거래 비용을 조회할 수도 있습니다.

## 특수 거래 비용(Special Transaction Costs)

일부 거래는 다른 거래 비용을 가집니다:

| 트랜잭션                                                                                       | 부하 확장 전 비용                             |
| ------------------------------------------------------------------------------------------ | -------------------------------------- |
| [Reference Transaction](https://xrpl.org/transaction-cost.html#reference-transaction-cost) | 10 drops                               |
| [Key Reset Transaction](https://xrpl.org/transaction-cost.html#key-reset-transaction)      | 0                                      |
| [Multi-signed Transaction](https://xrpl.org/multi-signing.html)                            | 10 drops × (1 + 제공된 서명 수)              |
| [EscrowFinish Transaction with Fulfillment](https://xrpl.org/escrowfinish.html)            | 10 drops × (33 + (주문 처리 크기(바이트) ÷ 16)) |
| [AccountDelete Transaction](https://xrpl.org/accounts.html#deletion-of-accounts)           | 2,000,000 drops                        |

## 거래 비용의 수혜자(Beneficiaries of the Transaction Cost)

거래 비용은 어떠한 당사자에게도 지불되지 않습니다. XRP는 되돌릴 수 없게 파괴됩니다.

## 부하 비용과 개방 ledger 비용(Load Cost and Open Ledger Cost)

거래 비용에는 두 개의 임계치가 있습니다:

* 거래 비용이 <mark style="background-color:yellow;">rippled</mark> 서버의 부하 기반 거래 비용 임계치를 충족시키지 않으면, 서버는 거래를 완전히 무시합니다.
* 거래 비용이 <mark style="background-color:yellow;">rippled</mark> 서버의 개방 ledger 비용 임계치를 충족시키지 않으면, 서버는 거래를 나중의 ledger 위해 대기열에 넣습니다.

이것은 거래를 대략 세 가지 범주로 나눕니다:

* 거래 비용이 부하 기반 거래 비용에 의해 거부되는 너무 낮은 거래 비용을 명시하는 거래.
* 현재 개방 ledger에 포함될 충분히 높은 거래 비용을 명시하는 거래.
* 그 사이에 있는 트랜잭션은 나중에 l[edger 버전을 위해 대기열](transaction-cost.md#undefined-3)에 추가됩니다.

## Local 부하 비용(Local Load Cost)

각 <mark style="background-color:yellow;">rippled</mark> 서버는 현재 부하를 기반으로 한 비용 임계치를 유지합니다. <mark style="background-color:yellow;">수수료</mark> 값이 <mark style="background-color:yellow;">rippled</mark> 서버의 현재 부하 기반 거래 비용보다 낮은 거래를 제출하면, 해당 서버는 거래를 적용하거나 전달하지 않습니다. (참고: 관리자 연결을 통해 거래를 제출하면, 거래가 un-scaled 최소 거래 비용을 충족하는 한 서버가 거래를 적용하고 전달합니다.) 거래의 Fee 값이 대부분의 서버의 요구 사항을 충족하지 않으면, 거래가 합의 과정에서 생존할 가능성은 매우 낮습니다.

## 개방 ledger 비용(Open Ledger Cost)

<mark style="background-color:yellow;">rippled</mark> 서버에는 거래 비용을 강제하는 두 번째 메커니즘이 있으며, 이를 개방 _ledger_ 비용이라고 합니다. 거래가 개방 ledger 비용 요구 사항을 XRP로 충족해야만 개방 ledger에 포함될 수 있습니다. 개방 ledger 비용을 충족하지 않는 거래는 대신 [다음 ledger에 대기](transaction-cost.md#undefined-3)할 수 있습니다.

새 ledger 버전마다 서버는 이전 ledger의 트랜잭션 수를 기준으로 개방 ledger에 포함할 트랜잭션 수에 대한 소프트 한도를 선택합니다. 개방 ledger 비용은 개방 ledger의 트랜잭션 수가 소프트 한도와 같아질 때까지는 스케일링되지 않은 최소 트랜잭션 비용과 같습니다. 그 이후에는 개방 ledger에 포함된 각 트랜잭션에 대해 개방 ledger 비용이 기하급수적으로 증가합니다. 다음 ledger에 대해 서버는 현재 ledger에 소프트 한도보다 많은 트랜잭션이 포함되어 있으면 소프트 한도를 증가시키고, 컨센서스 프로세스가 5초 이상 걸리면 소프트 한도를 감소시킵니다.

개방 ledger 비용 요구 사항은 [거래의 정상 비용에 비례](transaction-cost.md#undefined-5)하며, 절대 거래 비용이 아닙니다. 일반적으로 더 높은 요구 사항을 가진 거래 유형, 예를 들어 [다중 서명 거래](../undefined-1/undefined/undefined-1.md)는 최소 거래 비용 요구 사항을 가진 거래보다 개방 ledger 비용을 충족하기 위해 더 많이 지불해야 합니다.

참고:  [Fee Escalation explanation in rippled repository ](https://github.com/ripple/rippled/blob/release/src/ripple/app/misc/FeeEscalation.md).

## 대기열에 있는 거래(Queued Transactions)&#x20;

<mark style="background-color:yellow;">rippled</mark>가 서버의 로컬 로드 비용은 충족하지만 개방 ledger 비용은 충족하지 않는 트랜잭션을 수신하면, 서버는 해당 트랜잭션이 추후 ledger에 '포함될 가능성이 있는지' 추정합니다. 그렇다면 서버는 트랜잭션을 트랜잭션 대기열에 추가하고 네트워크의 다른 구성원에게 트랜잭션을 릴레이합니다. 그렇지 않으면 서버는 트랜잭션을 삭제합니다. [트랜잭션 비용은 트랜잭션이 검증된 ledger에 포함된 경우에만 적용되므로](transaction-cost.md#undefined-9) 서버는 트랜잭션 비용을 지불하지 않는 트랜잭션으로 인한 네트워크 부하를 최소화하려고 노력합니다.

대기 중인 트랜잭션에 대한 자세한 내용은 [트랜잭션 대기열](transaction-queue.md)을 참조하세요.

## 참조 거래 비용(Reference Transaction Cost)

"참조 거래"는 부하 스케일링 전에 필요한 [트랜잭션 비용](transaction-cost.md) 측면에서 가장 저렴한(무료가 아닌) 거래입니다. 대부분의 거래는 참조 거래와 동일한 비용을 갖습니다. [다중 서명](../undefined-1/undefined/undefined-1.md) 트랜잭션과 같은 일부 거래는 이 비용의 배수를 요구합니다. 개방 ledger 비용이 상승하면, 요구 사항은 거래의 기본 비용에 비례합니다.

## 수수료 수준(Fee Levels)

수수료 _수준_은 최소 비용과 실제 거래 비용 사이의 비례적 차이를 나타냅니다. [개방 ledger 비용](transaction-cost.md#ledger-1)은 절대 비용이 아닌 수수료 수준으로 측정됩니다. 아래 표를 참조하여 비교하세요:

| Transaction                                   | Minimum cost in drops | Minimum cost in Fee levels | Double cost in drops | Double cost in fee levels |
| --------------------------------------------- | --------------------- | -------------------------- | -------------------- | ------------------------- |
| 참조 트랜잭션(대부분의 트랜잭션)                            | 10                    | 256                        | 20                   | 512                       |
| 4개의 서명이 있는 다중 서명 트랜잭션                         | 50                    | 256                        | 100                  | 512                       |
| 키 재설정 트랜잭션                                    | 0                     | (사실상 무한)                   | N/A                  | (Effectively infinite)    |
| 32바이트 preimage를 사용하여 EscrowFinish 트랜잭션을 마칩니다. | 350                   | 256                        | 700                  | 512                       |

## 트랜잭션 비용 쿼리(Querying the Transaction Cost)

<mark style="background-color:yellow;">rippled</mark> APIs는 <mark style="background-color:yellow;">server\_info</mark> 명령(사람용)과 <mark style="background-color:yellow;">server\_state</mark> 명령(기계용)의 두 가지 방법으로 로컬 로드 기반 트랜잭션 비용을 쿼리할 수 있습니다.

[수수료 방법](fees.md)을 사용하여 오픈 ledger 비용을 확인할 수 있습니다.

## server\_info&#x20;

[server\_info 메소드](../../references/http-websocket-apis/api-1/undefined-5/server\_info.md)는 이전 ledger 기준으로 스케일링되지 않은 최소 XRP 비용을 <mark style="background-color:yellow;">validated\_ledger.base\_fee\_xrp</mark>로 소수점 XRP 형식으로 보고합니다. 트랜잭션을 릴레이하는 데 필요한 실제 비용은 동일한 응답에서 서버의 현재 부하 수준을 나타내는 <mark style="background-color:yellow;">load\_factor</mark> 매개변수에 해당 <mark style="background-color:yellow;">base\_fee\_xrp</mark> 값을 곱하여 스케일링됩니다. 다시 말해:

**현재 XRP 거래 비용 = base\_fee\_xrp × load\_factor**

## server\_state&#x20;

[server\_state 메소드](../../references/http-websocket-apis/api-1/server-info/server\_state.md)는 <mark style="background-color:yellow;">rippled</mark>의 내부 부하 계산의 직접적인 표현을 반환합니다. 이 경우, 효과적인 부하 비율은 현재의 <mark style="background-color:yellow;">load\_factor</mark>와 <mark style="background-color:yellow;">load\_base</mark>의 비율입니다. <mark style="background-color:yellow;">validated\_ledger.base\_fee</mark> 매개변수는 XRP의 drop 단위로 최소 거래 비용을 보고합니다. 이 설계는 rippled가 거래 비용을 정수 수학만 사용하여 계산하면서도 서버 부하에 대한 합리적인 세부 조정을 허용합니다. 거래 비용의 실제 계산은 다음과 같습니다:

**현재 drop 단위의 거래 비용 = (base\_fee × load\_factor) ÷ load\_base**

## 거래 비용 지정하기(Specifying the Transaction Cost)

모든 서명된 거래는 거래 비용을 [<mark style="background-color:yellow;">수수료</mark> 필드](fees.md)에 포함해야 합니다. 서명된 거래의 모든 필드와 마찬가지로, 이 필드는 서명을 무효화하지 않고 변경할 수 없습니다.

일반적으로, XRP Ledger는 거래를 서명된 그대로 실행합니다. (그렇지 않을 경우 분산 합의 네트워크를 조정하는 것은 적어도 어렵습니다.) 이 때문에, <mark style="background-color:yellow;">수수료</mark> 필드에서 지정된 정확한 XRP 양이 모든 네트워크 부분의 현재 최소 거래 비용보다 훨씬 많을지라도 모든 거래는 XRP를 파괴합니다. 거래 비용은 계정의 [reserve requirement](../undefined-1/undefined/reserves.md)을 위해 별도로 설정될 XRP까지 파괴할 수 있습니다.

거래에 서명하기 전에, 우리는 현재의 부하 기반 거래 비용을 조회하는 것을 추천합니다. 부하 스케일링 때문에 거래 비용이 높다면, 감소할 때까지 기다리고 싶을 수 있습니다. 거래를 즉시 제출할 계획이 없다면, 거래 비용의 미래 부하 기반 변동을 고려하여 약간 더 높은 거래 비용을 지정하는 것을 추천합니다.

## 자동으로 거래 비용 지정하기(Automatically Specifying the Transaction Cost)

<mark style="background-color:yellow;">수수료</mark> 필드는 거래를 생성할 때 자동으로 채울 수 있는 것들 중 하나입니다. 이 경우, 자동 채우기 소프트웨어는 P2P 네트워크의 현재 부하에 기반하여 적절한 <mark style="background-color:yellow;">수수료</mark> 값을 제공합니다. 그러나 이 방식으로 거래 비용을 자동으로 채우는 것에는 여러 가지 단점과 제한 사항이 있습니다:

* 네트워크의 거래 비용이 자동 채우기와 거래 제출 사이에 상승하면, 거래가 확인되지 않을 수 있습니다.&#x20;
  * 거래가 확정적으로 확인되거나 거부되지 않은 상태에 머무르지 않도록 하려면, <mark style="background-color:yellow;">LastLedgerSequence</mark> 매개변수를 제공하여 거래가 결국 만료되도록 하십시오. 또는, 동일한 <mark style="background-color:yellow;">시퀀스</mark> 번호를 재사용하여 거래를 취소해 볼 수 있습니다. 최선의 관행에 대해서는 신뢰할 수 있는 거래 제출을 참조하십시오.&#x20;
* 자동으로 제공된 값이 너무 높지 않게 주의해야 합니다. 작은 거래를 보내기 위해 큰 수수료를 낭비하고 싶지 않을 것입니다.&#x20;
  * 만약 당신이 rippled를 사용한다면, [sign 메소드](../../references/http-websocket-apis/api-2/undefined-3/sign.md)의 <mark style="background-color:yellow;">fee\_mult\_max</mark>와 <mark style="background-color:yellow;">fee\_div\_max</mark> 매개 변수를 사용하여 서명할 의향이 있는 부하 스케일링에 대한 한계를 설정할 수 있습니다.&#x20;
  * 일부 클라이언트 라이브러리(예: xrpl.js, xrpl-py)는 최대 Fee 값을 설정할 수 있으며, Fee 값이 최대값보다 높은 거래에 서명하는 대신 오류를 발생시킵니다.&#x20;
* 오프라인 머신에서나 다중 서명할 때는 자동 채우기를 할 수 없습니다.&#x20;

## 거래 비용과 실패한 거래(Transaction Costs and Failed Transactions)

거래 비용의 목적은 XRP Ledger의 P2P 네트워크를 과도한 부하로부터 보호하는 것이므로, 이는 네트워크에 분산되는 어떤 거래에든 적용되어야 합니다, 성공 여부에 상관없이. 그러나 공유 전역 ledger에 영향을 미치려면 거래는 검증된 ledger에 포함되어야 합니다. 따라서 <mark style="background-color:yellow;">rippled</mark> 서버는 거래가 실패하더라도 ledger에 포함시키려고 노력하며, <mark style="background-color:yellow;">tec</mark> 상태 코드를 가지는 거래("tec"는 "Transaction Engine - Claimed fee only"를 의미)를 포함시킵니다.

거래 비용은 거래가 실제로 검증된 ledger에 포함될 때만 보내는 사람의 XRP 잔액에서 차감됩니다. 이것은 거래가 성공적으로 간주되거나 <mark style="background-color:yellow;">tec</mark> 코드로 실패하더라도 마찬가지입니다.

거래의 실패가 최종적인 경우, <mark style="background-color:yellow;">rippled</mark> 서버는 그것을 네트워크에 전달하지 않습니다. 거래가 검증된 ledger에 포함되지 않으므로, 누구의 XRP 잔액에도 영향을 미칠 수 없습니다.

## XRP 부족(Insufficient XRP)

<mark style="background-color:yellow;">rippled</mark> 서버가 처음으로 거래를 평가할 때, 보내는 계정이 XRP 거래 비용을 지불하기에 충분한 XRP 잔액이 없다면 서버는 <mark style="background-color:yellow;">terINSUF\_FEE\_B</mark> 오류 코드로 거래를 거부합니다. 이것은 <mark style="background-color:yellow;">ter</mark> (Retry) 코드이므로, <mark style="background-color:yellow;">rippled</mark> 서버는 거래의 결과가 최종적일 때까지 거래를 네트워크에 전달하지 않고 다시 시도합니다.

거래가 이미 네트워크에 분산되었지만 계정에 거래 비용을 지불하기에 충분한 XRP가 없는 경우, 결과 코드 <mark style="background-color:yellow;">tecINSUFF\_FEE</mark>가 발생합니다. 이 경우, 계정은 가능한 모든 XRP를 지불하고, 0 XRP로 끝납니다. 이것은 <mark style="background-color:yellow;">rippled</mark>가 거래를 네트워크에 전달할지 여부를 진행 중인 ledger를 기준으로 결정하기 때문에 발생할 수 있습니다. 그러나 거래는 합의 ledger 만들 때 삭제되거나 재정렬될 수 있습니다.

## 키 재설정 거래(Key Reset Transaction)

특별한 경우로, 계정의 [lsfPasswordSpent 플래그](../../references/xrp-ledger/ledger/ledger-1/accountroot.md)가 비활성화된 상태에서 거래 비용이 0인 [SetRegularKey](../../references/xrp-ledger/undefined-1/undefined-1/setregularkey.md) 트랜잭션을 보낼 수 있습니다. 이 트랜잭션은 계정의 마스터 키 쌍으로 서명해야 합니다. 이러한 트랜잭션을 보내면 lsfPasswordSpent 플래그가 활성화됩니다.

이 기능은 RegularKey가 손상된 경우 계정을 복구할 수 있도록 설계되었습니다, 그리고 그럴 때 손상된 계정에 XRP가 얼마나 있는지에 대해 걱정할 필요가 없습니다. 이렇게 하면, 더 많은 XRP를 그 계정에 보내기 전에 계정을 다시 통제할 수 있습니다.

[lsfPasswordSpent 플래그](../../references/xrp-ledger/ledger/ledger-1/accountroot.md)는 처음에 비활성화됩니다. 이 플래그는 마스터 키 쌍으로 서명한 SetRegularKey 트랜잭션을 보낼 때 활성화됩니다. 계정이 XRP [결제](../../references/xrp-ledger/undefined-1/undefined-1/payment.md)를 받을 때 다시 비활성화됩니다.

<mark style="background-color:yellow;">rippled</mark>는 키 재설정 트랜잭션의 명목상의 트랜잭션 비용이 0임에도 불구하고 다른 트랜잭션보다 키 재설정 트랜잭션을 우선시합니다.

## 거래 비용 변경(Changing the Transaction Cost)

XRP Ledger는 XRP의 가치가 장기적으로 변하는 것을 반영하기 위해 최소 거래 비용을 변경하는 메커니즘이 있습니다. 어떤 변경이든 컨센서스 과정을 통해 승인받아야 합니다. 더 자세한 정보는 [수수료 투표](../undefined-4/undefined-6.md)를 참조하십시오.



## 참고

* **Concepts:**
  * [Reserves](https://xrpl.org/reserves.html)
  * [Fee Voting](https://xrpl.org/fee-voting.html)
  * [Transaction Queue](https://xrpl.org/transaction-queue.html)
* **Tutorials:**
  * [Reliable Transaction Submission](https://xrpl.org/reliable-transaction-submission.html)
* **References:**
  * [fee method](https://xrpl.org/fee.html)
  * [server\_info method](https://xrpl.org/server\_info.html)
  * [FeeSettings object](https://xrpl.org/feesettings.html)
  * [SetFee pseudo-transaction](https://xrpl.org/setfee.html)
