# Demurrage(과잉보유비용)

{% hint style="info" %}
Demurrage(과잉 보유 비용)는 지원이 중단된 기능으로, 이 페이지는 XRP Ledger 소프트웨어의 이전 버전의 역사적인 동작을 설명합니다.
{% endhint %}

[Demurrage](https://en.wikipedia.org/wiki/Demurrage\_\(currency\))(과잉 보유 비용)는 보유한 자산에 대한 비용을 나타내는 음의 이자율입니다. XRP Ledger에서 토큰의 과잉 보유 비용을 나타내기 위해 과잉 보유율을 표시하는 사용자 정의 [화폐 코드](../../references/xrp-ledger/undefined/undefined.md)를 사용할 수 있습니다. 이렇게 하면 각각 다른 과잉 보유율을 갖는 토큰의 별도 버전이 생성됩니다. 클라이언트 응용 프로그램은 통화 코드와 함께 연간 이율 백분율을 표시하여 과잉 보유 통화 코드를 지원할 수 있습니다. 예: "XAU (-0.5% 연간)".

## 과잉 보유 토큰 금액 표현&#x20;

XRP Ledger에서 모든 금액을 지속적으로 업데이트하는 대신, 이 접근 방식은 이자를 받는 토큰이나 과잉 보유하는 토큰의 금액을 "ledger 값"과 "표시 값"으로 나눕니다. "ledger 값"은 통화의 가치를 고정된 지점인 "Ripple 에포크"인 2000년 1월 1일 자정에 기록한 것입니다. "표시 값"은 Ripple 에포크부터 해당 시간까지의 연속적인 이자 또는 과잉 보유를 계산한 후의 시간(일반적으로 현재 시간)에서의 금액을 나타냅니다.

{% hint style="info" %}
Tip:

과잉 보유를 인플레이션과 비슷하게 생각할 수 있습니다. 이를 통해 영향을 받는 모든 자산의 가치가 시간이 지남에 따라 감소하지만, ledger는 항상 2000년 값을 유지합니다. 이는 실제 세계의 인플레이션을 보여주지 않습니다. 과잉 보유는 일정한 비율로 가상적인 인플레이션과 유사합니다.
{% endhint %}

따라서 클라이언트 소프트웨어는 두 가지 변환을 적용해야 합니다:

* 지정된 시간의 표시 값에서 ledger 값으로 변환하여 기록합니다.&#x20;
* ledger 값에서 지정된 시간의 표시 값으로 변환합니다.&#x20;

## 과잉 보유 계산&#x20;

과잉 보유를 계산하기 위한 전체 공식은 다음과 같습니다:

```
D = A × ( e ^ (t ÷ τ) )
```

* **D**는 과잉 보유 후의 금액입니다.&#x20;
* **A**는 전역 원장에 기록된 과잉 보유 전 금액입니다.&#x20;
* **e**는 오일러 수(Euler's number)입니다.&#x20;
* **t**는 Ripple 에포크(2000년 1월 1일 0시 0분 UTC)부터 현재까지의 초 단위 시간입니다.&#x20;
* **τ**는 e-폴딩 시간(e-folding time)입니다. [이 값은 원하는 이자율에서 계산됩니다.](demurrage.md#undefined-2)&#x20;

표시 금액과 ledger 금액 사이의 변환을 위해 다음 단계를 사용할 수 있습니다:

1. ( e ^ (t ÷ τ) )의 값을 계산합니다. 이 수를 "과잉 보유 계수"라고 합니다. 과잉 보유 계수는 항상 특정 시간(현재 시간 등)을 기준으로 상대적입니다.&#x20;
2. 변환할 금액에 적용:\
   \- 변환할 금액에 과잉 보유 계수를 곱하여 ledger 값으로 변환합니다. \
   \- 변환할 금액에 과잉 보유 계수를 나누어 표시 값으로 변환합니다.&#x20;
3. 필요한 경우 결과 값을 원하는 정확도로 표현할 수 있도록 조정합니다. XRP Ledger의 토큰 형식에 따라 ledger 값은 소수점 이하 15자리의 정밀도로 제한됩니다.

## 이자를 받는 화폐 코드 형식&#x20;

양수 이자 또는 음수 이자(과잉 보유)를 갖는 토큰은 [표준 화폐 코드 형식](../../references/xrp-ledger/undefined/undefined.md) 대신 다음 형식의 160비트 화폐 코드를 사용합니다:

<figure><img src="https://xrpl.org/img/demurrage-currency-code-format.png" alt=""><figcaption></figcaption></figure>

1. 첫 8비트는 <mark style="background-color:yellow;">0x01</mark>이어야 합니다.&#x20;
2. 다음 24비트는 3자리 ASCII 문자를 나타냅니다. 이는 ISO 4217 코드로 예상되며, 표준 형식의 ASCII 문자와 동일한 문자를 지원합니다.&#x20;
3. 다음 24비트는 모두 <mark style="background-color:yellow;">0</mark>이어야 합니다.
4. 다음 64비트는 통화의 이자율을 IEEE 754 배정밀도 형식으로 나타냅니다. "e-폴딩 시간"으로 표시됩니다.&#x20;
5. 다음 24비트는 예약되어 있으며, 모두 <mark style="background-color:yellow;">0</mark>이어야 합니다.

## e-폴딩 시간 계산&#x20;

Ledger 금액과 표시 금액 사이를 전환하거나 이자부 통화/디레싱 화폐의 화폐 코드를 계산하려면 이자율을 "e-폴딩 시간"으로 설정해야 합니다. e-폴딩 시간은 수량이 e의 계수(젤러 수)만큼 증가하는 데 걸리는 시간입니다. 관례에 따라, e-폴딩 시간은 공식에서 문자 τ로 쓰여집니다.

주어진 연 퍼센트 이자율에 대한 e-폴딩 시간을 계산하는 방법:

1. 연이자를 적용한 후 최초로 제시된 금액의 백분율을 구하려면 이자율을 100%로 추가합니다. 연체의 경우 마이너스 금리를 사용합니다. 예를 들어, 0.5% 연체율은 -0.5%의 금리로 **99.5%**가 남게 됩니다.&#x20;
2. 백분율을 십진수로 나타냅니다. 예를 들어 99.5%는 **0.995**가 됩니다.&#x20;
3. 그 숫자의 자연 로그를 확인해 보세요. 예를 들어, **ln(0.995) = -0.005012541823544286**입니다. (이 숫자는 초기 이자율이 양수이면 양수이고, 이자율이 음수이면 음수입니다.)&#x20;
4. 1년(31536000)의 초 수를 이전 단계의 자연 로그 결과로 나눕니다. 예를 들어 **31536000 µ -0.005012541823544286 = -6291418827.045599**입니다. 이 결과는 e-폴딩 시간(초)입니다.&#x20;

{% hint style="info" %}
Note:

관례상, XRP Ledger의 이자/지연 규칙은 연간 고정 초수(31536000)를 사용하며, 이는 윤일 또는 윤초에 대해 조정되지 않습니다.
{% endhint %}

## 클라이언트 지원&#x20;

이자를 받거나 과잉 보유하는 토큰을 지원하기 위해 클라이언트 응용 프로그램은 다음과 같은 기능을 구현해야 합니다:

* Ledger 또는 거래 데이터에서 검색된 과잉 보유 토큰의 금액을 표시 값에서 ledger 값으로 변환해야 합니다. (과잉 보유의 경우 표시 값이 ledger 값보다 작습니다.)
* 과잉 보유 토큰의 입력을 받을 때, 클라이언트는 표시 형식에서 ledger 형식으로 금액을 변환해야 합니다. (과잉 보유의 경우 ledger 값이 사용자 입력 값보다 큽니다.) 클라이언트는 지불, 거래 제안 및 기타 유형의 트랜잭션을 생성할 때 ledger 값 사용해야 합니다.
* 클라이언트는 이자 또는 과잉 보유가 있는 토큰과 그렇지 않은 토큰, 그리고 서로 다른 이자율 또는 과잉 보유율을 가진 토큰을 구분해야 합니다. 클라이언트는 이자가 있는 통화 코드 형식을 "XAU (-0.5% 연간)"와 같은 표시 형식으로 파싱할 수 있어야 합니다.

## ripple-lib 지원&#x20;

과잉 보유는 ripple-lib 버전 **0.7.37**부터 **0.12.9**까지 지원되었습니다. 그러나 대부분의 현대적인 라이브러리에서는 과잉 보유를 _**지원하지 않습니다**_.

다음 코드 예제는 호환되는 버전의 ripple-lib를 사용하여 ledger 값과 표시 값 간의 변환 방법을 보여줍니다. Ripple 과잉 보유 계산기도 참조하세요.

표시 값에서 ledger 값으로 변환하려면 <mark style="background-color:yellow;">Amount.from\_human()</mark>을 사용합니다:

```javascript
// create an Amount object for the display amount of the demurring currency
// and pass in a reference_date that represents the current date
// (in this case, ledger value 10 XAU with 0.5% annual demurrage,
//  at 2017-11-04T00:07:50Z.)
var demAmount = ripple.Amount.from_human('10 0158415500000000C1F76FF6ECB0BAC600000000',
                                  {reference_date:563069270});

// set the issuer
demAmount.set_issuer("rHb9CJAWyB4rj91VRWn96DkukG4bwdtyTh");

// get the JSON format for the ledger amount
console.log(demAmount.to_json());

// { "value": "10.93625123082769",
//   "currency": "0158415500000000C1F76FF6ECB0BAC600000000",
//   "issuer": "rHb9CJAWyB4rj91VRWn96DkukG4bwdtyTh" }
```

ledger 값에서 표시 값으로 변환합니다:

```javascript
// create an Amount object with the ledger value,
ledgerAmount = ripple.Amount.from_json({
  "currency": "015841551A748AD2C1F76FF6ECB0CCCD00000000",
  "issuer": "rHb9CJAWyB4rj91VRWn96DkukG4bwdtyTh",
  "value": "10.93625123082769"})

// apply interest up to the current time to get the display amount
var displayAmount = demAmount.applyInterest(new Date());

console.log(displayAmount.to_json());

// { "value": "9.999998874657716",
//   "currency": "0158415500000000C1F76FF6ECB0BAC600000000",
//   "issuer": "rHb9CJAWyB4rj91VRWn96DkukG4bwdtyTh" }
```
