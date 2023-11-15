# 이체 수수료(Transfer Fees)

[토큰](./) 발행자는 사용자가 해당 토큰을 상호 교환할 때 발생하는 이체 수수료를 부과할 수 있습니다. 이체의 발신자는 이체 수수료에 기반한 추가 백분율을 차감하고, 이체의 수취인은 의도한 금액을 입금받습니다. 그 차이가 이체 수수료입니다.

일반 토큰의 경우, 이체 수수료로 지불된 토큰은 소각되어 XRP Ledger에서 더 이상 추적되지 않습니다. 토큰이 ledger 외부 자산으로 보증되는 경우, 이는 발행자가 XRP Ledger에서의 의무를 충족하기 위해 예비해야 하는 자산의 양을 줄입니다. 이체 수수료는 외부 자산으로 보증되지 않은 토큰에는 적합하지 않습니다.

대체 불가능한 토큰도 이체 수수료를 가질 수 있지만, 작동 방식이 다릅니다. 자세한 내용은 [대체 불가능한 토큰](non-fungible-tokens/)을 참조하십시오.

이체 수수료는 발행 계정으로부터 직접 송금하거나 수신할 때는 적용되지 않지만, [운영 주소](broken-reference)에서 다른 사용자로 이체할 때는 적용됩니다.

XRP는 발행자가 없기 때문에 이체 수수료가 없습니다.

## 예시(Example)

이 예시에서 ACME 은행은 XRP Ledger에서 EUR 스테이블코인을 발행합니다. ACME은행은 이체 수수료를 1%로 설정할 수 있습니다. 지불 수신자가 2 EUR.ACME를 받으려면 송신자는 2.02 EUR.ACME를 보내야 합니다. 거래 후에는 ACME의 XRP Ledger에서의 미지급 의무가 0.02€만큼 감소하므로 ACME은행은 EUR 안정화토큰을 보증하기 위해 해당 금액을 은행 계좌에 보유할 필요가 없어집니다.

다음 다이어그램은 1%의 이체 수수료를 가진 Alice로부터 Charlie로의 2 EUR.ACME의 XRP Ledger 송금을 보여줍니다:

<figure><img src="https://xrpl.org/img/transfer-fees.svg" alt=""><figcaption></figcaption></figure>

회계 관점에서 Alice, ACME 및 Charlie의 재무상태는 다음과 같이 변경될 수 있습니다:

<figure><img src="https://xrpl.org/img/transfer-fees-balance-sheets.svg" alt=""><figcaption></figcaption></figure>

## 이체 수수료와 지불 경로(Transfer Fees in Payment Paths)

이체 수수료는 개별 이체가 한 당사자에서 다른 당사자로 토큰을 이동할 때마다 적용됩니다 (발행 계정에 직접 송금/수신하는 경우를 제외하고). 더 복잡한 거래에서는 이체 수수료가 여러 번 발생할 수 있습니다. 이체 수수료는 끝에서부터 역순으로 적용되어, 결국 송금자는 모든 수수료를 감안하여 충분한 금액을 송금해야 합니다. 예를 들어:

<figure><img src="https://xrpl.org/img/transfer-fees-in-paths.svg" alt=""><figcaption></figcaption></figure>

이 시나리오에서 Salazar (송신자)는 ACME가 발행한 EUR을 보유하고 있으며, WayGate가 발행한 100 USD를 Rosa (수신자)에게 전달하려고 합니다. FXMaker는 주문서에서 최고의 거래를 할 수 있는 공급자로, 0.9 EUR.ACME 당 1 USD.WayGate의 환율을 제시합니다. 이체 수수료가 없다면 Salazar는 90 EUR을 보내어 Rosa에게 100 USD를 전달할 수 있습니다. 그러나 ACME은 1%의 이체 수수료를, WayGate은 0.2%의 이체 수수료를 부과합니다. 이는 다음과 같은 의미를 가지게 됩니다:

* FXMaker는 Rosa가 100 USD.WayGate을 수령하려면 100.20 USD.WayGate을 보내야 합니다.
* FXMaker의 현재 요청은 90.18 EUR.ACME를 100.20 USD.WayGate을 보내기 위해 사용합니다.
* &#x20;FXMaker가 90.18 EUR.ACME를 수령하려면 Salazar는 91.0818 EUR.ACME를 보내야 합니다.

## 기술적 세부 정보(Technical Details)

이체 수수료는 [발행 주소](broken-reference)의 설정으로 나타냅니다. 이체 수수료는 0% 미만 또는 100% 이상이 될 수 없으며, 가장 가까운 0.0000001%로 반올림됩니다. 이체 수수료는 동일한 계정에서 발행된 모든 토큰에 적용됩니다. 서로 다른 토큰에 대해 다른 이체 수수료를 설정하려면 여러 [발행 주소](broken-reference)를 사용하십시오.

XRP Ledger 프로토콜에서 이체 수수료는 <mark style="background-color:yellow;">TransferRate</mark>필드로 표시되며, 해당 토큰의 10억 단위를 수신받기 위해 보내야 하는 금액을 나타내는 정수로 표시됩니다. <mark style="background-color:yellow;">TransferRate</mark> <mark style="background-color:yellow;">1005000000</mark>은 0.5%의 이체 수수료에 해당합니다. 기본적으로 <mark style="background-color:yellow;">TransferRate</mark>는 수수료가 없음을 나타내는 값으로 설정됩니다. <mark style="background-color:yellow;">TransferRate</mark>의 값은 <mark style="background-color:yellow;">1000000000</mark>("0%" 수수료) 미만 또는 <mark style="background-color:yellow;">2000000000</mark>("100%" 수수료) 이상으로 설정할 수 없습니다. 값 <mark style="background-color:yellow;">0</mark>은 수수료가 없는 특수한 경우로, <mark style="background-color:yellow;">1000000000</mark>과 동일합니다.

토큰 발행자는 발행 주소에서 [AccountSet 트랜잭션](../../references/xrp-ledger/undefined/undefined-1/accountset.md)을 제출하여 모든 토큰의 <mark style="background-color:yellow;">TransferRate</mark>를 변경할 수 있습니다.

누구나 [account\_info 메소드](../../references/http-websocket-apis/api-1/undefined/account\_info.md)를 사용하여 계정의 <mark style="background-color:yellow;">TransferRate</mark>를 확인할 수 있습니다. <mark style="background-color:yellow;">TransferRate</mark>가 생략된 경우 수수료가 없음을 나타냅니다.

{% hint style="info" %}
Note:

<mark style="background-color:yellow;">rippled</mark>된 v0.80.0에서 도입된 fix1201 개정안은 최대 이체 수수료를 100% (<mark style="background-color:yellow;">TransferRate</mark>  <mark style="background-color:yellow;">2000000000</mark>)로 낮추었으며, 이는 이전에 약 329%의 효과적인 한계로 설정되어 있었습니다 (32비트 정수의 최대 크기에 기반). XRP Ledger에는 여전히 100%보다 높은 이체 수수료 설정이 포함된 계정이 있을 수 있습니다. 이전에 설정된 이체 수수료는 명시된 비율대로 계속 적용됩니다.
{% endhint %}

## 클라이언트 라이브러리 지원(Client Library Support)

일부 [클라이언트 라이브러리](../../references/undefined/)에는 <mark style="background-color:yellow;">TransferRate</mark>함수를 얻거나 설정하는 편의 기능이 있습니다.

**JavaScript**: <mark style="background-color:yellow;">xrpl.percentToTransferRate()</mark>를 사용하여 백분율 이체 수수료를 문자열에서 해당하는 <mark style="background-color:yellow;">TransferRate</mark> 값으로 변환할 수 있습니다.



### See Also <a href="#see-also" id="see-also"></a>

* **Concepts:**
  * [Fees (Disambiguation)](https://xrpl.org/fees.html)
  * [Transaction Cost](https://xrpl.org/transaction-cost.html)
  * [Paths](https://xrpl.org/paths.html)
* **References:**
  * [account\_lines method](https://xrpl.org/account\_lines.html)
  * [account\_info method](https://xrpl.org/account\_info.html)
  * [AccountSet transaction](https://xrpl.org/accountset.html)
  * [AccountRoot Flags](https://xrpl.org/accountroot.html#accountroot-flags)

\
