# 수수료 투표

검증인들은 기본 [트랜잭션 비용](../transactions/transaction-cost.md)과 [reserve requirements](../undefined-2/reserves.md)에 대한 변경에 투표할 수 있습니다. 만약 검증인의 구성 설정이 네트워크의 현재 설정과 다르다면, 검증인은 주기적으로 네트워크에 자신의 선호도를 표현합니다. 만약 쿼럼을 형성하는 검증인들이 변경에 합의한다면, 그들은 이후에 적용되는 변경을 적용할 수 있습니다. 검증인들은 특히 XRP의 가치에 대한 장기적인 변화에 대응하기 위해 이를 수행할 수 있습니다.

[<mark style="background-color:yellow;">rippled</mark> 검증인](../../tutorials/rippled/rippled-1/rippled.md) 운영자들은 <mark style="background-color:yellow;">rippled.cfg</mark> 파일의 <mark style="background-color:yellow;">\[voting]</mark> 구절에서 거래 비용과 reserve requirements에 대한 선호도를 설정할 수 있습니다.

{% hint style="info" %}
Caution:

신뢰하는 검증인들의 컨센서스에 의해 채택된 충분하지 않은 요구 사항은 XRP Ledger의 P2P 네트워크를 서비스 거부 공격에 노출시킬 수 있습니다.
{% endhint %}

설정할 수 있는 매개 변수는 다음과 같습니다:

| 매개변수              | 설명                                                                                                                    | 권장 값                |
| ----------------- | --------------------------------------------------------------------------------------------------------------------- | ------------------- |
| `reference_fee`   | 참조 트랜잭션을 전송하기 위해 삭제해야 하는 XRP의 양(1 XRP = 100만 드롭), 가능한 가장 저렴한 트랜잭션입니다. 실제 트랜잭션 비용은 개별 서버의 로드에 따라 동적으로 조정되는 이 값의 배수입니다. | `10` (0.00001 XRP)  |
| `account_reserve` | 계정에 예약해야 하는 최소 XRP 양(드롭 단위)입니다. 이 금액은 원장의 새 계좌에 자금을 대기 위해 보낼 수 있는 최소 금액입니다.                                           | `10000000` (10 XRP) |
| `owner_reserve`   | 주소가 ledger에서 소유한 각 개체에 대해 보유해야 하는 XRP의 양(드롭 단위)입니다.                                                                   | `2000000` (2 XRP)   |

## 투표 프로세스&#x20;

256번째 ledger를 "플래그(flag)" ledger라고 합니다. (플래그 ledger는 <mark style="background-color:yellow;">ledger\_index</mark>를 <mark style="background-color:yellow;">256</mark>으로 나눈 나머지가 <mark style="background-color:yellow;">0</mark>인 ledger로 정의됩니다.) 플래그 ledger 바로 이전의 ledger에서는 현재 네트워크 설정과 다른 계정 예비 및 거래 비용 선호도를 가진 각 검증인은 그들이 선호하는 값을 나타내는 "투표" 메시지를 그들의 ledger 검증과 함께 전송합니다.

플래그 ledger 자체에서는 아무 일도 일어나지 않지만, 검증인들은 신뢰하는 다른 검증인들로부터의 투표를 받고 이를 기록합니다.

다른 검증인들의 투표를 계산한 후, 각 검증인은 자신의 선호도와 신뢰하는 검증인들의 대다수의 선호도 사이에서 타협을 시도합니다. (예를 들어, 한 검증자는 최소 거래 비용을 10에서 100으로 올리고 싶지만, 대다수의 검증자들은 10에서 20으로만 올리고 싶을 경우, 해당 검증인은 비용을 20으로 올리기로 변경에 합의합니다. 그러나 해당 검증인은 10보다 낮거나 100보다 높은 값으로 결정하지 않습니다.) 타협이 가능한 경우, 검증인은 플래그 ledger 다음의 ledger 제안에 SetFee 의사 트랜잭션을 삽입합니다. 같은 변경을 원하는 다른 검증인들은 동일한 [SetFee 의사 트랜잭션](../../references/xrp-ledger/undefined-1/pseudo-transactions/setfee.md)을 동일한 ledger의 제안에 삽입합니다. (선호도가 기존 네트워크 설정과 일치하는 검증자들은 아무것도 수행하지 않습니다.) SetFee 의사 트랜잭션이 컨센서스 프로세스를 통과하여 확인된 ledger에 포함된다면, SetFee 의사 트랜잭션에 의해 표시된 새로운 거래 비용 및 예비 설정은 다음 ledger부터 적용됩니다.

요약하면:

* **플래그 ledger -1:** 검증자가 투표를 제출합니다.&#x20;
* **플래그 ledger:** 검증자가 투표를 집계하고 포함할 SetFee를 결정합니다.&#x20;
* **플래그 ledger +1:** 검증자가 자신의 제안 ledger에 SetFee 의사 트랜잭션을 삽입합니다.&#x20;
* **플래그 ledger +2:** 컨센서스에 도달한 SetFee 의사 트랜잭션을 포함한 새로운 설정이 적용됩니다.&#x20;

## 최대 수수료&#x20;

값 수수료에 대한 최대 가능한 값은 FeeSettings ledger 객체에 저장된 내부 데이터 타입에 의해 제한됩니다. 이 값들은 다음과 같습니다:

| 매개변수              | 최대 값(드롭)  | 최대 값(XRP)                |
| ----------------- | --------- | ------------------------ |
| `reference_fee`   | 264       | (지금까지 존재했던 것보다 더 많은 XRP) |
| `account_reserve` | 232 drops | 대략 4294 XRP              |
| `owner_reserve`   | 232 drops |  대략4294 XRP              |
