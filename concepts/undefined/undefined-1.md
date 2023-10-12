# 소프트웨어 생태계

XRP Ledger는 가치의 인터넷을 구동하고 가능하게 하는 소프트웨어 프로젝트의 깊고 다층적인 생태계에 위치해 있습니다. XRP Ledger와 상호 작용하는 모든 프로젝트, 도구, 그리고 비즈니스를 나열하는 것은 불가능하므로, 이 페이지에서는 몇 가지 카테고리만을 나열하고 [xrpl.org](https://xrpl.org/)에서 문서화된 중요한 프로젝트들을 강조하여 소개합니다.

## 스택레벨&#x20;

<figure><img src="../../.gitbook/assets/Software Ecosystem_1 (1).png" alt=""><figcaption></figcaption></figure>

* [XRP Ledger의 기초](undefined-1.md#rippled)는 항상 연결된 서버들의 P2P 네트워크로, 이들 서버는 거래를 공유하며 [컨센서스](../undefined-4/undefined.md) 과정에 참여하고 [트랜잭션](../transactions/)을 처리합니다. XRP Ledger 생태계의 모든 것은 결국 이 P2P 네트워크 위에 직접적으로, 간접적으로 구축됩니다.
* [_프로그래밍 라이브러리_](undefined-1.md#undefined-1)는 상위 레벨의 소프트웨어에서 존재하며, 이들은 프로그램 코드에 직접 임포트되고 XRP Ledger에 접근하는 메소드를 포함합니다.
* [_미들웨어_](undefined-1.md#undefined-2)는 XRP Ledger 데이터에 대한 간접적인 접근을 제공합니다. 이 계층의 응용 프로그램들은 대부분 자체 데이터 저장 및 처리 기능을 가지고 있습니다.
* [_앱과 서비스_](undefined-1.md#undefined-3)는 사용자 레벨에서 XRP Ledger와의 상호 작용을 제공하거나, 더 높은 수준의 앱과 서비스에 대한 기반을 제공합니다.

## rippled: 핵심 서버

XRP Ledger의 중심인 P2P 네트워크는 합의 규칙과 거래 처리를 강제하는 높은 신뢰성과 효율성의 서버가 필요합니다. XRPL Foundation은 이 서버 소프트웨어의 참조 구현을 관리하고 출판하며, 이를 [rippled](../xrp-ledger/)(발음은 "ripple-dee")라고 합니다. 서버는 [허용적인 오픈 소스 라이선스](https://github.com/XRPLF/rippled/blob/develop/LICENSE.md) 하에 제공되므로, 누구나 자신의 서버 인스턴스를 검사하고 수정하고 제한 없이 재배포할 수 있습니다.

<mark style="background-color:yellow;">rippled</mark>의 모든 인스턴스는 같은 네트워크([testnet과 같은 병렬 네트워크](../xrp-ledger/parallel-networks.md)를 따르도록 설정하지 않는 한)와 동기화하며 네트워크 전반의 모든 통신에 접근할 수 있습니다. 네트워크상의 모든 <mark style="background-color:yellow;">rippled</mark> 서버는 전체 XRP Ledger의 최신 상태 데이터의 완전한 복사본을 유지하며, 최근의 거래와 그 거래가 만든 변화의 기록을 가지고 있고, 모든 서버는 모든 거래를 독립적으로 처리하면서 그 결과가 네트워크의 나머지 부분과 일치하는지 확인합니다. 서버는 더 많은 [ledger 역사](../xrp-ledger/ledger/)를 유지하고 [검증인](../xrp-ledger/rippled.md#undefined-2)으로서 합의 과정에 참여하도록 설정될 수 있습니다.

이 서버는 사용자가 데이터를 조회하고, 서버를 관리하고, 거래를 제출하기 위해 [rippled API](../../references/http-websocket-apis/)를 제공합니다.

## 프로그래밍 라이브러리

XRP Ledger 데이터에 접근하기 위해 [프로그래밍 라이브러리](https://xrpl.org/client-libraries.html)는 엄밀히 말해 필수적이지 않습니다. HTTP나 WebSocket을 사용하여 직접 [rippled API](../../references/http-websocket-apis/)에 연결할 수 있습니다. 라이브러리는 <mark style="background-color:yellow;">rippled</mark> API에 접근하는 일반적인 작업을 간소화하고, 데이터를 라이브러리의 프로그래밍 언어에서 더 쉽게 이해하고 프로그래밍할 수 있는 형태로 변환합니다.

[JavaScript용 xrpl.js](../../tutorials/undefined-1/javascript.md)(이전에 "ripple-lib"라고 불렸음)는 XRP Ledger에 접근하기 위한 가장 오래되고 가장 잘 지원되는 라이브러리입니다. 많은 [미들웨어 서비스](undefined-1.md#undefined-2)는 내부적으로 이런 프로그래밍 라이브러리를 사용합니다.

## 미들웨어

미들웨어 서비스는 한쪽에서 XRP Ledger API를 소비하고, 반대쪽에서 자신의 API를 제공하는 프로그램입니다. 이들은 공통 기능을 서비스로 제공하여 상위 수준 애플리케이션을 더 쉽게 구축할 수 있게 하는 추상화 계층을 제공합니다.

[프로그래밍 라이브러리](undefined-1.md#undefined-1)와 달리, 미들웨어 서비스는 일반적으로 무한정 실행되며, 자체 데이터베이스(SQL 관계형 데이터베이스 또는 기타)와 설정 파일을 가질 수 있습니다.

[Data API](https://xrpl.org/data-api.html)는 XRP Ledger 위의 미들웨어 서비스의 예입니다. Data API는 XRP Ledger 데이터를 수집하고 변환하여, 시간별로 쿼리하거나, 데이터 유형별로 필터링하거나, 데이터 분석을 수행할 수 있게 합니다.

[XRP-API](https://xpring-eng.github.io/xrp-api/)는 또 다른 미들웨어 서비스입니다. XRP-API는 비밀 키를 관리하고 모든 프로그래밍 언어의 앱에게 XRP Ledger에 대한 더 편리한 RESTful 인터페이스를 제공합니다.

## 앱과 서비스

스택의 꼭대기에서는 정말로 흥미로운 일들이 일어납니다. 앱과 서비스는 사용자와 디바이스가 XRP Ledger에 연결하는 방법을 제공합니다. 이 수준에서, 거래소는 [XRP를 상장하고](../../use-cases/decentralized-finance/xrp-list-xrp-as-an-exchange.md), [게이트웨이는 분산 거래에 사용되는 다른 통화를 발행하며](../../tutorials/xrp-ledger/undefined.md), 지갑은 XRP를 사고, 팔고, 보유하는 사용자 인터페이스를 제공합니다. 그 외에도 더 높은 수준에 추가 서비스가 층층이 쌓이는 등 많은 가능성이 있습니다.&#x20;

XRP뿐만 아니라 다른 많은 가치 표시 방법과 호환되는 애플리케이션을 구축하는 좋은 방법은 XRP에 정착하여 [Interledger Protocol](https://interledger.org/)을 사용하는 것입니다.

XRP와 인접 기술을 사용하여 사용자와 상호 작용하는 프로젝트의 다른 예시들이 많이 있습니다. 몇 가지 예시를 보려면 [비즈니스](https://xrpl.org/uses.html), [거래소](https://xrpl.org/exchanges.html), [지갑](https://xrpl.org/xrp-overview.html)을 참조하세요.

## 참고

* [RippleX ](https://ripplex.io/)
* [Technical FAQ](https://xrpl.org/technical-faq.html)
* [XRPChat Links & Resources ](https://www.xrpchat.com/links/) - 게이트웨이와 거래소, 지갑과 저장 공간, 앱 등의 최신 목록이 포함되어 있습니다.
