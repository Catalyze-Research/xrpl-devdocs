# Software Ecosystem

XRP Ledger는 가치 인터넷을 지원하고 가능하게 하는 소프트웨어 프로젝트의 깊고 다층적인 생태계의 본거지입니다. XRP Ledger와 상호작용하는 모든 프로젝트, 도구, 비즈니스를 나열하는 것은 불가능하므로 이 페이지에서는 몇 가지 카테고리만 나열하고 이 웹사이트에 문서화된 몇 가지 핵심 프로젝트만 강조합니다.

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

## 스택 레벨(Stack Levels)

* 코어 서버([_Core Servers_](https://xrpl.org/software-ecosystem.html#core-servers)_)_는 항상 트랜잭션을 중계하고 처리하는 피어투피어 네트워크인 XRP 원장의 기초를 형성합니다.\

* 클라이언트 라이브러리([_Client Libraries_](https://xrpl.org/software-ecosystem.html#client-libraries)_)_는 상위 수준의 소프트웨어에 존재하며, 프로그램 코드로 직접 가져올 수 있고 XRP 원장에 액세스하는 메서드를 포함합니다.\

* 미들웨어([_Middleware_](https://xrpl.org/software-ecosystem.html#middleware)_)_는 XRP 원장 데이터에 대한 간접 액세스를 제공합니다. 이 계층의 애플리케이션은 종종 자체 데이터 저장 및 처리 기능을 가지고 있습니다.\

* 앱과 서비스(A[_pps and Services_](https://xrpl.org/software-ecosystem.html#apps-and-services))는 XRP 원장과 사용자 수준의 상호작용을 제공하거나 더 높은 수준의 앱과 서비스를 위한 기반을 제공합니다.

## 코어 서버(Core Servers)

XRP Ledger의 핵심인 P2P 네트워크는 합의 및 거래 처리 규칙을 시행하기 위해 매우 안정적이고 효율적인 서버를 필요로 합니다. XRP Ledger Foundation은 rippled("리플-디"로 발음)라고 하는 이 서버 소프트웨어의 참조 구현을 게시합니다. 이 서버는 허용된 [a permissive open-source license](https://github.com/XRPLF/rippled/blob/develop/LICENSE.md) 하에 제공되므로 누구나 자신의 서버 인스턴스를 검사하고 수정할 수 있으며, 제한 없이 다시 게시할 수 있습니다.

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

모든 코어 서버는 동일한 네트워크에 동기화되며([test network](https://xrpl.org/parallel-networks.html)를 따르도록 구성되지 않은 경우) 네트워크 전반의 모든 통신에 액세스할 수 있습니다. 네트워크의 모든 서버는 최근 트랜잭션 및 해당 트랜잭션의 변경 기록과 함께 전체 XRP Ledger에 대한 최신 상태 데이터의 전체 사본을 보관하며, 모든 서버는 모든 트랜잭션을 독립적으로 처리하면서 그 결과가 나머지 네트워크와 일치하는지 확인합니다. 서버는 더 많은 원장 기록([ledger history](https://xrpl.org/ledger-history.html))을 보관하고 합의 과정에 검증자([validator](https://xrpl.org/rippled-server-modes.html#validators))로 참여하도록 구성할 수 있습니다.



코어 서버는 사용자가 데이터를 조회하고, 서버를 관리하고, 트랜잭션을 제출할 수 있도록 [HTTP / WebSocket APIs](https://xrpl.org/http-websocket-apis.html)를 노출합니다. 일부 서버는 HTTP/웹소켓 API를 제공하지만 P2P 네트워크에 직접 연결하지 않고 트랜잭션을 처리하거나 합의에 참여하지도 않습니다. 리포팅 모드에서 실행되는 리플 서버와 Clio 서버와 같은 이러한 서버는 P2P 모드의 코어 서버에 의존하여 트랜잭션을 처리합니다.



## 클라이언트 라이브러리(Client Libraries)

라이브러리는 일반적으로 HTTP/WebSocket API를 통해 XRP Ledger에 액세스하는 몇 가지 일반적인 작업을 간소화합니다. 라이브러리는 데이터를 다양한 프로그래밍 언어에 더 친숙하고 편리한 형태로 변환하며, 일반적인 작업의 구현을 포함합니다.

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

대부분의 클라이언트 라이브러리의 핵심 기능 중 하나는 트랜잭션에 로컬로 서명하는 것이므로 사용자가 네트워크를 통해 개인 키를 전송할 필요가 없습니다.

많은 미들웨어 서비스가 내부적으로 클라이언트 라이브러리를 사용합니다.

현재 사용 가능한 [클라이언트 라이브러리](https://xrpl.org/client-libraries.html)에 대한 정보는 클라이언트 라이브러리를 참조하세요.



## 미들웨어(Middleware)

미들웨어 서비스는 한쪽에서는 XRP Ledger API를 소비하고 다른 쪽에서는 자체 API를 제공하는 프로그램입니다. 미들웨어 서비스는 몇 가지 일반적인 기능을 서비스로 제공함으로써 더 높은 수준의 애플리케이션을 더 쉽게 구축할 수 있도록 추상화 계층을 제공합니다.

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

새로 인스턴스화되고 이를 가져오는 프로그램과 함께 종료되는 클라이언트 라이브러리와 달리, 미들웨어 서비스는 일반적으로 무기한 실행 상태를 유지하며 자체 데이터베이스(관계형 SQL 데이터베이스 등) 및 구성 파일을 보유할 수 있습니다. 일부는 다양한 가격 또는 사용량 제한이 있는 클라우드 서비스로 제공됩니다.



## 앱과 서비스(Apps and Services)

스택 꼭대기에서 정말 흥미로운 일들이 일어납니다. 앱과 서비스는 사용자와 기기가 XRP Ledger에 연결할 수 있는 방법을 제공합니다. 사설 거래소, 토큰 발행자, 마켓 플레이스, 탈중앙화 거래소 인터페이스, 지갑과 같은 서비스는 XRP와 모든 종류의 토큰을 포함한 다양한 자산을 구매, 판매, 거래할 수 있는 사용자 인터페이스를 제공합니다. 그 외에도 더 높은 계층의 추가 서비스를 포함한 다양한 가능성이 존재합니다.

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

이 계층에서 또는 그 이상으로 구축할 수 있는 몇 가지 예는 [Use Cases](https://xrpl.org/use-cases.html)를 참조하세요.
