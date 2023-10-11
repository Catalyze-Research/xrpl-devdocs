# 알고리즘 트레이딩(Algorithmic Trading)

XRP Ledger는 탈중앙화된 거래소로 알고리즘 트레이딩을 통해 수익을 창출할 수 있는 기회를 제공하며, 이는 컴퓨터 프로그램을 실행하여 수익성 있는 거래 기회를 자동으로 찾아서 취하는 것을 의미합니다. 알고리즘 트레이딩에서는 일반적으로 정량적 요소를 기반으로 많은 거래를 수행하여 꾸준하고 작은 수익을 얻습니다. 이는 시장 펀더멘털을 기반으로 몇 번의 장기 투자를 하고 시간이 지나면서 큰 수익을 얻기 위해 기다리는 기존의 수동 트레이딩과는 다릅니다. 일반적으로 암호화폐의 높은 변동성으로 인해 기존의 "매수 후 보유" 투자에 적합하지 않기 때문에 블록체인은 수동 거래보다 알고리즘 거래에 더 적합한 경우가 많으며, XRP Ledger는 여러 가지 이유로 알고리즘 거래에 특히 적합합니다:



* 탈중앙화 거래소 데이터는 공개되어 있으며 자유롭게 이용할 수 있습니다.
* 거래는 몇 초 만에 정산되므로 특별한 장비 없이도 자주 거래할 수 있습니다.
* XRP Ledger의 거래 비용이 낮습니다.



## 트레이딩 전략(Trading Strategies)

알고리즘 트레이딩은 다양한 전략을 통해 수익을 창출할 수 있으며, 알고리즘 트레이딩의 도전 과제(또는 재미) 중 하나는 자신만의 고유한 접근 방식을 설계하고 구현하는 것입니다. 알고리즘 트레이딩 접근법은 크게 다음 범주에 속하는 경우가 많습니다:

**차익거래(Arbitrage)**: 자산을 매수한 후 즉시 매도하여 시세 차익을 노리는 것입니다. 여기에는 가격이 일치하지 않는 3개 이상의 자산 세트를 찾거나, 가격이 다른 사설 거래소 간에 자산을 이동하기 위해 XRP Ledger을 사용하는 것이 포함될 수 있습니다.

**퀀트 트레이딩(Quantitative Trading)**: 과거 가격 변동, 외부 데이터 또는 두 가지 모두에 기반해 미래 가격 변동을 예측하고 이를 활용하는 것입니다. 캔들스틱 패턴, 자산의 가격 변동과 다른 자산의 상관관계, 소셜 미디어의 감정 분석 등을 예로 들 수 있습니다.

**선행 매매(Front-running)**: 보류 중인 거래, 특히 대규모 거래를 이용하여 해당 거래가 매수하는 자산을 매수한 후 즉시 재판매하는 행위입니다. 프런트 러닝은 유동성을 추가하지 않고 다른 트레이더로부터 이익을 취하거나 다른 방법으로는 발생하지 않을 교환을 가능하게 하기 때문에 종종 눈살을 찌푸리게 합니다. XRP 원장의 표준 거래 순서는 프런트 러닝을 어렵게 만들지만, 불가능하지는 않습니다.



## 차익거래 예시(Arbitrage Examples)

차익거래를 수행하는 방법에는 여러 가지가 있으며, XRP Ledger 내 또는 인접한 곳에서 모두 가능합니다. 다음 예시는 잠재적인 전략을 설명하기 위한 것이지만, 다른 방법도 가능합니다.

순환 결제를 사용해 여러 자산 거래를 완료하고 수익을 얻을 수 있습니다. XRP Ledger는 자산 쌍 간의 겹치는 거래는 물론, XRP가 중간에 자산인 3개의 자산 세트도 자동으로 연결합니다. 그러나 XRP Ledger 프로토콜은 더 길거나 복잡한 다른 경로의 거래를 자동으로 찾아서 경쟁하지는 않습니다. (최적의 경로를 찾는 것은 계산 집약적인 것으로 알려진 문제 범주에 속합니다). 따라서 직접 경로를 찾으면 이와 같은 수익성 있는 차익거래 기회를 찾을 수 있으며, 이러한 경로를 결제 트랜잭션에 명시적으로 지정할 수 있습니다. 예를 들어 각각 발행자가 다른 세 가지 토큰인 FOO, BAR, TST가 있다고 가정해 보겠습니다. 1 FOO를 사용해 BAR 2개를 구매하고, 2 BAR를 사용해 TST 3개를 구매하고, 마지막으로 3 TST를 사용해 1.1 FOO를 구매할 수 있다면, 관련 토큰의 전송 수수료 등 거래 비용을 제외한 0.1 FOO의 수익을 얻을 수 있습니다.

자산 가격이 다른 여러 사설 거래소에 계정이 있는 경우 교차 거래소 차익거래를 수행할 수 있습니다. 예를 들어, ACME 거래소에서 1XRP당 0.45달러에 XRP를 매수한 다음 WayGate 거래소로 이동하여 1XRP당 0.50달러에 매도할 수 있다면, 거래소의 출금 및 입금 수수료를 포함하여 관련 거래의 거래 및 송금 비용을 제외한 XRP당 0.05달러의 이익을 얻을 수 있습니다. 좀 더 복잡한 예로, ACME 거래소에서 BTC:ETH 가격이 변동하여 ETH가 BTC에 비해 더 저렴해진다면, 한 거래소에서 ETH→XRP를 매도한 다음 XRP를 ACME 거래소로 이동하여 XRP→BTC→ETH를 거래하여 이익을 얻을 수 있습니다. XRP Ledger 거래는 몇 초 안에 정산되지만 이더리움 거래는 몇 분, 비트코인 거래는 몇 시간이 걸릴 수 있기 때문에 XRP를 브릿지 통화로 사용하면 ACME 거래소에서 단순히 ETH→BTC, BTC→ETH를 거래하는 것보다 이 기회를 더 빨리 활용할 수 있습니다. (물론 이는 유동성이 충분하고 스프레드가 충분히 타이트하여 XRP로 교환하는 데 수익보다 더 많은 비용이 들지 않는 경우에만 가능합니다.)



## 배경 자료(Background Reading)

일반적으로 다음 자료를 읽어보면 알고리즘 트레이딩에 익숙해질 수 있습니다:

* 인베스토피디아: 알고리즘 트레이딩의 기초: 개념 및 예시\
  ([Investopedia: Basics of Algorithmic Trading: Concepts and Examples](https://www.investopedia.com/articles/active-trading/101014/basics-algorithmic-trading-concepts-and-examples.asp))
* 플래시 보이즈: 마이클 루이스의 월스트리트 반란\
  ([_Flash Boys: A Wall Street Revolt_ by Michael Lewis ](https://wwnorton.com/books/Flash-Boys/))
* 인베스토피디아: 차익거래가 투자에서 작동하는 방식\
  ([Investopedia: How Arbitraging Works in Investing, With Examples](https://www.investopedia.com/terms/a/arbitrage.asp))

예시 포함 다음 페이지에서는 XRP Ledger의 탈중앙화 거래소가 어떻게 작동하는지에 대한 핵심 요소를 설명합니다:

* 토큰([Tokens](https://xrpl.org/tokens.html))
* 탈중앙화 거래소([Decentralized Exchange](https://xrpl.org/decentralized-exchange.html))
* 오퍼([Offers](https://xrpl.org/offers.html))

### 테스트 및 일반적인 실수(Testing and Common Mistakes) <a href="#testing-and-common-mistakes" id="testing-and-common-mistakes"></a>

다른 유형의 트레이딩과 마찬가지로 알고리즘 트레이딩도 수익을 낼 수 있는 확실한 방법은 아니며 손실을 볼 수 있는 방법은 많습니다. 수동 트레이딩과 비교하면 알고리즘 트레이딩은 실수할 여지가 훨씬 적습니다. 작은 실수를 저질렀지만 그 실수가 많은 수의 거래로 확대되면 문제를 해결할 기회도 갖기 전에 손실이 빠르게 누적될 수 있습니다. 따라서 다양한 테스트를 통해 트레이딩 전략이 실제로 수익을 낼 수 있는지 확인하는 것이 현명합니다. 다음 중 일부 또는 전부를 수행하여 전략 또는 전략의 실제 구현(흔히 봇이라고 함)을 테스트할 수 있습니다:

현재 Ledger 상태 또는 과거 거래를 기준으로 잠재 수익률을 수동으로 계산합니다. 과거 데이터를 기록하여 봇에 입력한 다음, 봇이 어떤 행동을 취했을지 기록하고 그 결과를 실제 과거 가격 변동과 비교합니다. 여러 가지 그럴듯한 미래 시나리오에서 접근 방식의 결과를 모델링하거나 예측합니다. 이러한 계산이나 봇을 구축할 때 흔히 범할 수 있는 실수는 다음과 같습니다:

* 반올림 오류(Rounding errors). 계산이 정확하지 않거나 블록체인이 사용하는 정밀도와 일치하지 않으면 거래 결과를 부정확하게 예측하여 손실을 입거나 거래가 전혀 실행되지 않을 수 있습니다. XRP Ledger는 토큰과 XRP 금액에 서로 다른 정밀도를 사용하므로, 둘을 서로 거래할 때 예상치 못한 곳에서 반올림이 발생할 수 있습니다. 프로토콜에서 사용되는 정밀도에 대한 자세한 내용은 통화 형식을 참조하세요.
  * 토큰 발행자는 토큰과 관련된 환율의 정밀도를 더 제한할 수 있다는 점에 유의하세요. 자세한 내용은 틱 크기를 참조하세요.
  * 일반적으로 조회 시점과 거래 체결 시점 사이의 반올림 또는 가격 변동에 따른 잠재적 차이를 고려하기 위해 금액을 약간의 비율로 조정해야 합니다. 이 금액을 슬리피지라고 하며, 적절한 금액을 설정하는 것이 중요합니다. 슬리피지가 너무 낮으면 거래가 전혀 체결되지 않을 수 있지만, 너무 높으면 선행 체결에 취약하며 슬리피지가 높을수록 가격 변동으로 인해 일반적으로 수익이 더 많이 줄어들 수 있습니다.
* 추가 비용과 딜레이는 무시하면 됩니다. 예를 들어 두 스테이블코인이 모두 미국 달러로 완전히 뒷받침되지만 한 발행사가 0.5%의 이체 수수료를 부과하고 다른 발행사가 0.25%의 이체 수수료를 부과하는 경우, 해당 스테이블코인의 유효 거래 가격은 0.25% 정도 차이가 날 것으로 예상해야 합니다. 일반적으로 적은 금액이더라도 트랜잭션을 전송하는 데 드는 비용이나 기타 잠재적인 딜레이로 인한 결과도 잊지 마세요. 예를 들어, Ledger 외 사설 거래소에서 현재 유리한 가격이 표시되더라도 해당 거래소에서 입금을 처리하는 데 몇 시간 또는 며칠이 걸리면 가격이 변동될 수 있으므로 해당 거래소에서 유동성을 이미 보유하고 있지 않다면 이를 활용할 수 없습니다.
* 이례적인 이벤트를 고려하지 않습니다. 전례가 없는('black swan') 이벤트를 제쳐두더라도 개별 이상값으로 인해 계산이 왜곡될 수 있습니다. 한 예(실화)로, 한 트레이더는 특정 시간대에 특정 전략의 잠재적 수익을 계산할 때 수익의 80% 이상이 다른 사용자가 실수로 가격에 0을 추가한 단일 "팻 핑거" 거래에서 발생했다고 보고했습니다. 같은 전략을 이상 거래가 포함되지 않은 시간 범위로 계산했을 때는 수익성이 훨씬 낮았습니다.
* 트랜잭션 플래그를 읽지 않음. XRP Ledger 트랜잭션의 플래그는 트랜잭션이 처리되는 방식과 프로토콜이 트랜잭션을 "성공"으로 표시하는 시점에 상당한 영향을 미칠 수 있습니다. 예를 들어, "오퍼" 트랜잭션의 플래그는 전액을 즉시 받을 수 있는 경우에만 거래하는 "채우기 또는 죽이기" 주문을 만들 수 있으며, "결제" 트랜잭션의 플래그는 의도한 목적지에 전액을 전달하지 못하더라도 부분 결제를 성공으로 만들 수 있습니다. 트랜잭션의 플래그 필드를 구문 분석하려면 비트 단위의 수학을 수행해야 하지만, 이 과정을 건너뛰면 예상이 완전히 틀릴 수 있습니다.



## 세금 및 라이선스(Taxes and Licensing)

블록체인 거래에 대한 법적 요건은 관할권에 따라 다릅니다. 대부분의 경우 시작하는 데 라이선스나 기타 법적 장벽이 없지만, 특히 손익이 일정 기준을 초과하는 경우 세금 목적으로 수익을 신고해야 할 수도 있습니다. 미국에서는 일반적으로 거래로 인한 이익(또는 손실)을 자본 이득으로 신고하므로, 자산을 취득할 당시의 원가 기준을 계산해야 합니다. 개별 상황에 따라 거래 활동을 추적하거나 적절한 세금 양식을 작성하는 데 도움이 되는 다양한 도구가 시중에 나와 있습니다. 거래하는 자산과 트레이딩 전략에 따라 세부 사항은 달라질 수 있습니다. 알고리즘 트레이딩을 시작하기 전에 반드시 조사를 하거나 세무 전문가와 상담하시기 바랍니다.



## 기술적 세부 사항(Technical Details)

### 거래하기(Placing Trades)

XRP Ledger의 탈중앙화 거래소에서 대체 가능한 토큰과 XRP를 사고 팔려면 일반적으로 오퍼크레잇 트랜잭션([OfferCreate transactions](https://xrpl.org/offercreate.html))을 전송해야 합니다. 이러한 방식으로 거래하는 코드와 기술 단계에 대한 자세한 안내는  [Trade in the Decentralized Exchang](https://xrpl.org/trade-in-the-decentralized-exchange.html)를 참조하세요. 결제 트랜잭션 유형([Payment transaction type](https://xrpl.org/payment.html))을 사용하여 통화를 교환할 수도 있습니다. 다른 사용자에게 다른 통화로 결제금([cross-currency payment](https://xrpl.org/cross-currency-payments.html))을 보내거나, 차익거래 기회를 하나의 작업으로 연결하는 긴 경로([path](https://xrpl.org/paths.html))를 사용해 본인에게 다시 보낼 수도 있습니다.

NFT은 다르게 작동합니다. NFT를 거래하는 코드와 기술 단계는 [Transfer NFTokens Using JavaScript](https://xrpl.org/transfer-nfts-using-javascript.html)조하세요.



## 거래 데이터 읽기(Reading Trade Data)

XRP Ledger의 거래 활동에 대한 정보 출처는 많습니다. 거래 전략과 사용 사례에 따라 [Public Servers](https://xrpl.org/public-servers.html)를 통해 XRP Ledger에 연결할 수도 있지만, 종종 자체 서버를 실행하는 것이 더 유용할 수 있으며 일부 사용 사례는 그렇게 하지 않고는 실용적이지 않을 수 있습니다. P2P 모드에서 코어 서버를 설정하는 방법에 대한 지침은 [Install `rippled`](https://xrpl.org/install-rippled.html) 를 참조하세요.

다른 트랜잭션 활동을 추적하는 방식을 사용하는 경우, 트랜잭션의 상세 메타데이터를 읽어야 거래 금액을 정확히 알 수 있습니다. 오퍼는 부분적으로 실행될 수 있으며 여러 매칭 오퍼를 소비할 수 있습니다. 트랜잭션 메타데이터를 해석하는 방법에 대한 자세한 설명은 트랜잭션 결과 조회하기([Look Up Transaction Results](https://xrpl.org/look-up-transaction-results.html))를 참조하세요.

수익 창출 기회에 대응할 시간을 최대한 확보하려면 [Open Ledger](https://xrpl.org/open-closed-validated-ledgers.html)의 보류 중인 데이터를 살펴보거나 제안된 트랜잭션을 모니터링하는 것도 좋습니다. 웹소켓에 연결되어 있다면 `transactions_proposed` 스트림과 함께 [subscribe method](https://xrpl.org/subscribe.html) 을 사용해 합의에 의해 검증되기 전 트랜잭션을 볼 수 있으며, `accounts_proposed` 매개변수를 사용해 구독하여 특정 계정(예: 거래하려는 토큰의 발행자)에 영향을 미치는 트랜잭션의 하위 집합으로 제한할 수도 있습니다.



## 추가 개발 사항

Ripple은 기존의 중앙 지정가 주문 기반(CLOB) 탈중앙화 거래소와 함께 작동하는 기본 자동화된 시장 메이커(AMM) 설계로 XRP Ledger 프로토콜을 확장할 것을 제안했습니다. 이 제안이 받아들여져 개정안으로 활성화되면, AMM은 XRP Ledger 거래에서 중요한 요소가 될 것입니다. 자세한 내용은 다음 링크에서 확인하실 수 있습니다:

* [XLS-30d: Automated Market Maker standards proposal ](https://github.com/XRPLF/XRPL-Standards/discussions/78)
* [AMM documentation (Ripple Open Source site) ](https://opensource.ripple.com/docs/xls-30d-amm/automated-market-makers/?\_\_hstc=78174987.2efa7ff4419289bab26a42d2f43d166e.1692594357676.1696984456638.1696989014591.99&\_\_hssc=78174987.4.1696989014591&\_\_hsfp=864247990)



## 참고 자료

다음 아티클에서는 이러한 전략이 다른 블록체인에서 어떻게 작동하는지에 대한 좀 더 구체적인 예시와 흥미로운 정보를 제공합니다. 이러한 정보는 시작하기 위해 반드시 필요한 것은 아니지만, 관점을 제공하는 데 도움이 될 수 있습니다.

* [Ethereum is a Dark Forest ](https://www.paradigm.xyz/2020/08/ethereum-is-a-dark-forest)
* [Flash Boys 2.0: Frontrunning, Transaction Reordering, and Consensus Instability in Decentralized Exchanges (PDF) ](https://arxiv.org/pdf/1904.05234.pdf)
* [Slippage in AMM Markets ](https://papers.ssrn.com/sol3/papers.cfm?abstract\_id=4133897)
* [Frontrunner Jones and the Raiders of the Dark Forest: An Empirical Study of Frontrunning on the Ethereum Blockchain ](https://www.usenix.org/conference/usenixsecurity21/presentation/torres)
* [SoK: Transparent Dishonesty: front-running attacks on Blockchain (PDF) ](https://arxiv.org/pdf/1902.05164)

