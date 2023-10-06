# 스테이블코인 발행인(Stablecoin Issuer)

**스테이블코인(Stablecoin)**은 외부 세계의 자산으로 뒷받침되는 토큰입니다. 스테이블코인을 통해 사용자는 익숙한 통화로 거래할 수 있으며, 블록체인을 통해 편리하게 자금을 입출금할 수 있습니다. 이러한 서비스를 제공하는 대가로 스테이블코인 발행자는 스테이블코인의 인출 또는 송금 수수료 등 다양한 방식으로 수익을 얻을 수 있습니다.

누구나 XRP Ledger에 어떤 통화 코드로든 토큰을 발행할 수 있지만, 스테이블코인의 가치는 해당 자산으로 상환될 수 있다는 약속에서 비롯됩니다. 스테이블코인 발행에는 관할권에 따라 달라지는 규제 의무도 수반될 수 있습니다. 이러한 이EUR 스테이블코인을 발행하려면 일반적으로 평판이 좋은 비즈니스가 필요합니다.

{% hint style="info" %}
**NOTE:**

_XRP Ledger의 스테이블코인 발행자_를 이전에는 "**gateways**"라고 불렀습니다.
{% endhint %}

이 문서에서는 스테이블코인을 발행하기 전에 알아야 할 정보를 제공하고, 스테이블코인 발행자를 설정하는 데 관련된 선택 사항을 요약하며, XRP Ledger와기술 통합을 구현하기 위한 리소스를 제공합니다.



## 배경 정보(Background Information)

### 신뢰선과 토큰(Trust Lines and Tokens) <a href="#trust-lines-and-tokens" id="trust-lines-and-tokens"></a>

기본 암호화폐인 XRP를 제외한 XRP Ledger의 모든 자산은 **토큰**으로 표시되며, 토큰의 의미를 정의하는 특정 발행자와 연결됩니다. XRP Ledger는 사용자가 원하는 토큰만 보유하고 받을 수 있도록 **신뢰선(Trustline)**이라고 하는 방향성 회계 관계 시스템을 갖추고 있습니다.

외부 시스템의 자금으로 뒷받침되는 토큰을 **스테이블코인**이라고 부르기도 합니다. 여기에는 은행 계좌의 법정 화폐, 다른 블록체인의 암호화폐 또는 다른 유형의 자산과 가치 형태로 뒷받침되는 토큰이 포함됩니다. "스테이블코인(stablecoin)"이라는 용어는 토큰과 토큰이 나타내는 자산 간의 환율이 1:1(수수료 제외)로 "안정적(stable)"이어야 한다는 개념에서 유래했습니다.

* 참조: [Trust Lines and Issuing](https://xrpl.org/trust-lines-and-issuing.html)

### XRP

XRP는 XRP Ledger의 기본 암호화폐입니다. XRP는 모든 XRP Ledger 주소에서 다른 주소로 직접 전송할 수 있습니다. 이는 XRP를 편리한 브릿지 화폐로 만드는 데 도움이 됩니다.

토큰 발행자는 XRP를 축적하거나 교환할 필요가 없습니다. Reserve 요건을 충족하고 네트워크를 통해 트랜잭션을 전송하는 데 드는 비용을 지불하기 위해 소량의 XRP 잔액만 보유하면 됩니다. 10달러에 해당하는 XRP는 바쁜 발행자의 경우 최소 1년간의 거래 비용으로 충분할 것입니다.

* 참조: [What is XRP?](https://xrpl.org/what-is-xrp.html), [Reserves](https://xrpl.org/reserves.html), and [Transaction Cost](https://xrpl.org/transaction-cost.html)

### 유동성 및 트레이딩(Liquidity and Trading)

XRP Ledger은 탈중앙화 거래소를 포함하고 있으며, 여기서 모든 사용자는 어떤 조합으로든 XRP와 토큰을 교환하기 위해 입찰을 하고 이행할 수 있습니다. 탈중앙화 거래소는 또한 원자 간 결제를 가능하게 하는 유동성을 제공합니다.

스테이블코인 발행자가 탈중앙화 거래소를 직접 사용할 필요는 없지만, 모든 토큰은 자동으로 거래에 사용할 수 있습니다. 토큰이 널리 사용되면 사용자들은 자연스럽게 토큰을 서로 거래하며 다른 인기 자산에 유동성을 공급할 것입니다. 발행자는 특히 토큰이 신규일 때 기준 금리로 XRP 또는 기타 인기 토큰에 유동성을 공급하고자 할 수 있습니다. 스테이블코인 발행자가 유동성을 공급하는 경우, 거래와 발행에 서로 다른 주소를 사용하는 것이 가장 좋습니다.

* 참조: [Decentralized Exchange](https://xrpl.org/decentralized-exchange.html)

### 권장하는 비즈니스 관행

XRP 원장에 있는 스테이블코인 발행자의 토큰 가치는 고객이 필요할 때 토큰을 상환할 수 있다는 신뢰에서 직접적으로 비롯됩니다. 비즈니스 중단의 위험을 줄이려면 다음 모범 사례를 따라야 합니다:

* 별도의 발급 주소와 운영 주소를 사용하여 네트워크에서 위험 프로필을 제한합니다.&#x20;
* 은행비밀보호법 등 관할 지역의 자금세탁 방지 규정을 준수합니다. 여기에는 일반적으로 "고객알기제도(KYC)" 정보 수집 요건이 포함됩니다.&#x20;
* XRP 레저 재단의 토큰 발행자 자체 평가를 완료합니다.&#x20;
* 모든 정책과 수수료를 공개합니다.&#x20;
* 클라이언트 애플리케이션이 귀하에 대한 관련 세부 정보를 표시할 수 있도록 도메인 확인이 포함된 xrp-ledger.toml 파일을 제공하세요.

### 핫월렛과 콜드월렛(Hot and Cold Wallets)

XRP Ledger에서 금융기관은 일반적으로 여러 개의 XRP Ledger 주소를 사용해 유출된 비밀 키와 관련된 위험을 최소화합니다. 업계 표준은 다음과 같이 역할을 분리하는 것입니다:

* "콜드 월렛"이라고도 하는 하나의 **발급 주소**. 이 주소는 Ledger에서 금융 기관의 회계 관계의 허브이지만 가능한 한 적은 수의 트랜잭션을 전송합니다.&#x20;
* "핫 지갑"이라고도 하는 하나 이상의 **운영 주소**. 인터넷에 연결된 자동화된 시스템은 이러한 주소의 비밀 키를 사용하여 고객 및 파트너에게 이체하는 등의 일상적인 비즈니스를 수행합니다.&#x20;
* "웜 월렛"이라고도 하는 **선택적 대기 주소**. 신뢰할 수 있는 사람이 이 주소를 사용하여 운영 주소로 돈을 이체합니다.&#x20;

주요 기사: [Issuing and Operational Addresses](https://xrpl.org/account-types.html)



### 수수료와 수익원(Fees and Revenue Sources)

스테이블코인 발행자는 다음과 같은 다양한 방법으로 수익을 얻을 수 있습니다:

* 출금 또는 입금 수수료. 발행자는 XRP Ledger로 돈을 옮기거나 빼는 서비스에 대해 소액의 수수료(예: 1%)를 부과할 수 있습니다. 이 수수료는 XRP Ledger가 아니라 발행자의 자체 시스템에서 사용자에게 얼마를 발행하거나 크레딧을 제공할지 결정할 때 부과됩니다.&#x20;
* 송금 수수료. 발행자는 사용자가 XRP 원장에서 스테이블코인을 전송할 때 부과할 수수료를 백분율로 설정할 수 있습니다. 이 금액은 사용자가 거래할 때마다 XRP Ledger에서 차감되며, 발행자가 원장 외부에 보유한 자산의 양을 줄이지 않고도 스테이블코인 발행자가 Ledger에서 사용자에게 갚아야 할 총 의무를 감소시킵니다.&#x20;
* 부가가치를 통한 간접 수익. 스테이블코인은 다른 인접 서비스의 채택을 용이하게 하는 편리한 기능을 제공합니다.&#x20;
* 담보(collateral)에 대한 이자. 발행자는 스테이블코인을 뒷받침하는 자산을 이자 수익 계좌에 보유할 수 있습니다. 물론, 고객의 인출을 처리할 수 있는 충분한 자금을 항상 확보하고 있어야 합니다.&#x20;
* 금융 거래소. 기업은 탈중앙화 거래소에서 자체 스테이블코인을 사고 팔 수 있으며, 교차 통화 거래에 유동성을 제공할 수 있습니다.

### 수수료율 선택하기(Choosing Fee Rates)

토큰 수수료는 선택 사항입니다. 수수료가 높을수록 해당 토큰이 사용될 때 더 많은 수익을 얻을 수 있습니다. 반면에 수수료가 높으면 고객이 서비스 사용을 꺼리게 됩니다. 다른 발행자, 특히 동일한 유형의 자산으로 뒷받침되는 토큰을 사용하는 다른 발행자가 부과하는 수수료와 전신 수수료와 같은 XRP Ledger를 벗어난 기존 결제 시스템에서 부과하는 수수료를 고려하세요. 적절한 수수료 구조를 선택하는 것은 시장에서 기꺼이 지불할 의사가 있는 가격과 가격의 균형을 맞추는 문제입니다.

### 규정 준수 가이드라인(Compliance Guidelines)

토큰 발행자는 현지 규정을 준수하고 해당 기관에 보고할 책임이 있습니다. 규정은 국가와 주마다 다르지만 다음 섹션에 설명된 보고 및 규정 준수 요건을 포함할 수 있습니다. 토큰을 발행하기 전에 관할권 및 사용 사례에 대한 요건에 대해 전문적인 법률 자문을 구해야 합니다. 다음 리소스는 유용한 배경 자료가 될 수 있습니다.

### 고객확인제도(KYC)

고객확인제도(KYC)는 금융기관이 범죄 활동에 금융기관을 이용하는 것을 방지하기 위해 고객의 신원을 파악하고 확인하기 위해 실시하는 실사 활동을 말합니다. 금융 관련 범죄 활동에는 자금 세탁, 테러 자금 조달, 금융 사기, 신원 도용 등이 포함될 수 있습니다. 고객은 개인, 중개자 또는 기업일 수 있습니다.

KYC 프로세스는 일반적으로 다음을 목표로 합니다:

* 고객 신원 확인(조직 및 비즈니스의 경우 모든 실소유주 확인)
* 비즈니스 관계의 목적과 의도된 성격 이해
* 예상되는 거래 활동을 이해합니다.

금융기관과 관련 비즈니스가 위험, 특히 법적 위험과 평판 위험을 완화하기 위해서는 KYC가 매우 중요합니다. KYC 프로그램이 부적절하거나 존재하지 않는 경우 기관 또는 직원 개인이 민형사상 처벌을 받을 수 있습니다.

* **참조:**
  * [(USA) Bank Secrecy Act / Anti-Money Laundering Examination Manual](https://bsaaml.ffiec.gov/manual/Introduction/01)
  * [The Non-US Standard on KYC set by the Financial Action Task Force (FATF)](http://www.fatf-gafi.org/publications/fatfrecommendations/documents/fatf-recommendations.html)

### 자금 세탁 방지(AML) 및 테러 자금 조달 방지(CFT)

자금 세탁은 합법적인 금융 채널과 신뢰할 수 있는 기관을 통해 합법적으로 자금에 접근하거나 유통할 수 있도록 자금의 출처, 성격 또는 소유권을 위장하여 불법 자금을 이동시키는 프로세스입니다. 간단히 말해, "더러운 돈(dirty money)"을 "깨끗한 돈(clean money)"으로 전환하는 것입니다. 자금 세탁 방지(AML)는 자금 세탁을 막기 위해 고안된 법률과 절차를 말합니다.

테러 자금 조달이란 테러 활동에 관여하는 조직이나 테러 및 그 확산을 지원하는 조직에 자금을 모집, 수집 또는 제공하는 것을 말합니다. 테러 자금 조달 방지(CFT)는 테러 자금 조달에 사용되는 자금의 흐름을 식별, 보고 및 차단하는 프로세스를 말합니다.

* **참조:**
  * [“International Standards on Combating Money Laundering and the Financing of Terrorism & Proliferation.” FATF, 2012](http://www.fatf-gafi.org/publications/fatfrecommendations/documents/fatf-recommendations.html)
  * [“Virtual Currencies: Key Definitions and Potential AML/CFT Risks.” FATF, 2014](http://www.fatf-gafi.org/publications/methodsandtrends/documents/virtual-currency-definitions-aml-cft-risk.html)

### 자금 출처(Source of Funds)

불법 자금이 시스템을 통과하는 것을 방지하기 위해 금융 기관은 고객 자금의 출처가 범죄 활동과 관련이 있는지 합리적인 범위 내에서 판단할 수 있어야 합니다.

모든 고객의 정확한 자금 출처를 파악하는 것은 행정적으로 불가능할 수 있습니다. 따라서 일부 규제 당국은 모든 계정에 대해 구체적인 규제나 지침을 제공하지 않을 수 있습니다. 그러나 특정 경우에는 당국이 금융 기관에 자금 출처를 파악하고 보고하도록 요구할 수 있습니다. 자금 세탁 또는 테러 자금 조달의 위험이 높은 경우(일반적으로 "위험기반접근법(Risk-Based Approach; RBA)"이라고 함) 금융 기관은 고객의 자금 출처를 파악하는 것을 포함하되 이에 국한되지 않는 강화된 실사를 수행할 것을 FATF의 지침에 따라 권장하고 있습니다.

### 의심스러운 활동 보고(Suspicious Activity Reporting)

금융기관에서 자금이 범죄 활동과 관련이 있다고 의심되는 경우 해당 금융기관은 해당 규제 기관에 의심스러운 활동 보고서(SAR)를 제출해야 합니다. 의심스러운 활동을 보고하지 않으면 해당 기관은 처벌을 받을 수 있습니다.

* 참조:
  * [Suspicious Activity Reporting Overview (USA FFIEC)](https://bsaaml.ffiec.gov/manual/RegulatoryRequirements/04\_ep)
  * [FATF Recommendation 16: Reporting of suspicious transactions and compliance](http://www.fatf-gafi.org/publications/fatfrecommendations/documents/fatf-recommendations.html)

### 트래불 룰(Travel rule)

트래불 룰은 자금 이체 금융기관이 자금 이체 금액이 미화 3,000달러 상당액 이상인 경우 다음 금융기관에 특정 정보를 전달하도록 요구하는 은행비밀보호법(BSA) 규정입니다. 송금 주문에는 다음 정보가 포함되어야 합니다:

* 송금인의 이름,
* 송금인의 계좌 번호(사용 중인 경우),
* 송금인의 주소,
* 송금인의 금융기관 신원,
* 송금 주문 금액,
* 송금 주문의 실행 날짜, 그리고
* 수취인의 금융기관 신원.



* 참조: [Funds “Travel” Regulations: Questions & Answers](https://www.fincen.gov/resources/statutes-regulations/guidance/funds-travel-regulations-questions-answers)

### 수수료 공개 및 자금 추적(Fee Disclosure and Tracing Fund)

* 미국의 도드 프랭크 1073 전자 자금 이체법(규정 E)에 따라 은행은 미국에서 발생하는 국제 결제에 대해 환율, 수수료, 외국에서 지정된 수취인이 수령할 금액 등 비용 및 배송 조건에 대한 정보를 제공해야 합니다. '결제 전 공개(Pre-payment disclosure)'는 국제 전자 결제를 요청할 때 소비자에게 제공되며, '영수증 공개(receipt disclosure)'는 소비자가 송금을 승인할 때 소비자에게 제공됩니다. See also:
  * [The Consumer Financial Protection Bureau description of the regulation and extensions for banks](https://www.consumerfinance.gov/rules-policy/final-rules/electronic-fund-transfers-regulation-e/#rule)
* 유럽연합(EU)에서는 자금 세탁 및 테러 자금 조달을 탐지, 조사 및 방지하기 위해 송금인 은행, 수취인 은행 및 모든 중개 은행이 거래 내역에 송금인과 수취인의 특정 세부 정보를 포함하도록 EU 자금 이체 규정에서 규정하고 있습니다. \
  See also:
  * [EU Regulation (EC) No 1781/2006 description](http://eur-lex.europa.eu/LexUriServ/LexUriServ.do?uri=OJ:L:2006:345:0001:0009:EN:PDF)
  * [Effective June 26, 2017: Regulation 2015/847 on information accompanying transfers of funds](http://eur-lex.europa.eu/legal-content/EN/ALL/?uri=CELEX%3A32015R0847)

### 해외자산통제국(OFAC)

해외자산통제실(OFAC)은 미국의 외교 정책 및 국가 안보 목표를 지원하기 위해 경제 및 무역 제재를 관리하고 집행하는 미국 재무부 산하 기관입니다. 모든 미국인, 미국 법인 및 그 해외 지사는 OFAC 규정을 준수해야 합니다. OFAC 규정에 따라 미국 금융기관은 OFAC가 승인하거나 법령에 의해 명시적으로 면제되지 않는 한 OFAC가 관리 및 시행하는 제재 또는 금수 프로그램에 따라 개인, 법인 또는 국가와 거래 및 기타 거래를 수행하는 것이 금지됩니다.

* **참조:**&#x20;
  * [A list of OFAC resources](https://www.treasury.gov/resource-center/faqs/Sanctions/Pages/ques\_index.aspx)

### 가상통화 및 자금 서비스업에 대한 규제 정리

* United States:
  * [FinCEN Guidance and Definitions around Virtual Currency, March 18, 2013](https://www.fincen.gov/resources/statutes-regulations/guidance/application-fincens-regulations-persons-administering)
  * [FinCEN Publishes Two Rulings on Virtual Currency Miners and Investors, January 30, 2014](https://www.fincen.gov/news/news-releases/fincen-publishes-two-rulings-virtual-currency-miners-and-investors)
* Europe:
  * [European Banking Authority Opinion on Virtual Currencies, July 4, 2014](http://www.eba.europa.eu/documents/10180/657547/EBA-Op-2014-08+Opinion+on+Virtual+Currencies.pdf)
* FATF Guidance for Money Service Businesses:
  * [Financial Action Task Force, July 2009](http://www.fatf-gafi.org/media/fatf/documents/reports/Guidance-RBA-money-value-transfer-services.pdf)
