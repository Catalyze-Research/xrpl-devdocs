# rippled 서버 모드(rippled Server Modes)

rippled 서버 소프트웨어는 설정에 따라 여러 가지 모드로 실행될 수 있습니다. 주요 모드는 다음과 같습니다:

* [**P2P 모드**](rippled-rippled-server-modes.md#p2p) - 이것이 서버의 주요 모드입니다: P2P 네트워크를 따르며 트랜잭션을 처리하고 [**ledger 히스토리**](ledger/) 일부를 유지합니다. 이 모드는 다음과 같은 역할을 수행할 수 있도록 구성할 수 있습니다:
  * [**유효성 검증인**](rippled-rippled-server-modes.md#undefined-2) - 컨센서스에 참여하여 네트워크를 안전하게 유지합니다.
  * [**API 서버**](rippled-rippled-server-modes.md#api) - 공유된 ledger에서 데이터를 읽고 트랜잭션을 제출하며 ledger의 활동을 감시하는 데에 API 액세스를 제공합니다. 선택적으로 완전한 히스토리 서버가 될 수 있으며, 이는 트랜잭션 및 ledger 히스토리의 완전한 기록을 유지합니다.
  * [**허브 서버**](rippled-rippled-server-modes.md#undefined-1) - 다른 많은 P2P 네트워크 구성원 간의 메시지 중계를 담당합니다.
* [**리포팅 모드**](rippled-rippled-server-modes.md#undefined-3) - 관계형 데이터베이스에서 API 요청을 제공하기 위한 특수 모드입니다. P2P네트워크에 참여하지 않으므로 P2P 모드 서버를 실행하고 신뢰할 수 있는 gRPC 연결을 통해 리포팅 모드 서버에 연결해야 합니다. [![New in: rippled 1.7.0](https://img.shields.io/badge/New%20in-rippled%201.7.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/1.7.0)
* [**Stand-alone 모드**](rippled-rippled-server-modes.md#stand-alone) - 테스트용 오프라인 모드입니다. P2P 네트워크에 연결되지 않으며 컨센서스를 사용하지 않습니다.

<mark style="background-color:yellow;">rippled</mark> 실행 파일은 로컬에서 [<mark style="background-color:yellow;">rippled</mark> API](../../references/http-websocket-apis/)에 액세스하기 위한 클라이언트 어플리케이으로 실행될 수도 있습니다. (이 경우 동일한 바이너리의 두 인스턴스가 함께 실행될 수 있습니다. 하나는 서버로, 다른 하나는 일시적으로 클라이언트로 실행되고 종료됩니다.)

각 모드에서 <mark style="background-color:yellow;">rippled</mark>를 실행하는 데 필요한 명령에 대한 자세한 정보는 [Commandline Reference](https://xrpl.org/commandline-usage.html)를 참조하십시오.

## P2P 모드(P2P Mode)

P2P 모드는 <mark style="background-color:yellow;">rippled</mark> 서버의 주요이자 기본 모드이며, 서버가 수행해야 할 대부분의 작업을 처리할 수 있습니다. 이러한 서버들은 트랜잭션을 처리하고 XRP ledger의 공유 상태를 유지하는 P2P 네트워크를 형성합니다. 트랜잭션을 제출하거나 ledger 데이터를 읽거나 네트워크에 참여하는 경우, 요청은 언젠가는 P2P 모드 서버를 통해 이루어져야 합니다.

P2P 모드 서버는 추가 기능을 제공할 수 있도록 구성할 수 있습니다. 최소한의 수정이 가해진 구성 파일로 실행되는 P2P 모드 서버는 스톡 서버라고도 합니다. 다른 구성 옵션으로는 다음이 있습니다:

* [검증인.](rippled-rippled-server-modes.md#undefined-2)
* [API 서버. ](rippled-rippled-server-modes.md#api)
* [퍼블릭 허브.](rippled-rippled-server-modes.md#undefined-1)

P2P 모드 서버는 기본적으로 [Mainnet](https://xrpl.org/parallel-networks.html)에 연결합니다.

## API 서버(API Servers)

모든 P2P 모드 서버는 트랜잭션 제출, 잔액 및 설정 확인, 서버 관리와 같은 목적으로 [API](../../references/http-websocket-apis/)를 제공합니다. 비즈니스에 사용되는 트랜잭션 조회나 제출 등의 작업을 위해 자체 서버를 운영하는 것 유용할 수 있습니다.

## 전체 히스토리 서버(Full History Servers)

다른 일부 블록체인과는 달리 XRP Ledger는 서버가 현재 상태를 알고 새로운 트랜잭션을 처리하기 위해 완전한 트랜잭션 히스토리를 보유할 필요가 없습니다. 서버 운영자는 얼마나 많은 [ledger 역사](ledger/)를 동시에 저장할지 결정할 수 있습니다. 그러나 P2P 모드 서버는 로컬로 사용 가능한 ledger 히스토리를 사용하여 API 요청에 응답할 수 있습니다. 예를 들어, 6개월 동안의 히스토리를 유지한다면, 서버는 1년 전의 트랜잭션 결과를 설명할 수 없을 것입니다. [전체 역사](ledger/)를 가진 API 서버는 XRP Ledger 시작 이후의 모든 트랜잭션과 잔액을 보고할 수 있습니다.

## 퍼블릭 허브(Public Hubs)

허브 서버는 많은 수의 [피어 프로토콜 연결](peer-protocol.md)을 가진 P2P 모드 서버입니다. 특히 인터넷에서 연결을 허용하는 _퍼블릭 허브_는 XRP Ledger 네트워크의 효율적인 연결을 유지하는 데 도움이 됩니다. 성공적인 퍼블릭 허브는 다음과 같은 특징을 갖추고 있습니다:

* 좋은 대역폭.
* 신뢰할 수 있는 피어와의 연결.
* 신뢰성 있는 메시지 중계 능력.

서버를 허브로 구성하려면 허용되는 최대 피어 수를 늘리고 방화벽과 NAT(네트워크 주소 변환) 라우터를 통해 적절한 포트를 전달해야 합니다.

## 검증인(Validators)

XRP ledger의 견고성은 각각 일부 다른 검증인 서로가 공모하지 않을 것이라는 신뢰를 가지고 있는 연결된 검증인 웹에 의존합니다. 다른 P2P 모드 서버와 마찬가지로 각 트랜잭션을 처리하고 ledger 상태를 계산하는 것 외에, 검증인 [컨센서스 프로토콜](../consensus-protocol/consensus-structure.md)에 적극적으로 참여합니다. XRP ledger에 의존하는 경우 검증 과정에 참여하여 컨센서스 프로세스에 기여하는 것이 중요합니다.

유효성 검사는 작은 계산 리소스만 사용하지만, 단일 엔티티나 조직이 여러 개의 유효성 검증인들을 운영하는 것은 공모에 대한 더 많은 보호를 제공하지 않기 때문에 큰 이점이 없습니다. 각 검증인은 고유한 암호키 쌍으로 자신을 식별하며 이를 주의 깊게 관리해야 합니다. 여러 검증인들은 키 쌍을 공유해서는 안 됩니다. 이러한 이유로 유효성 검사는 기본적으로 비활성화되어 있습니다.

다른 용도로 사용되는 서버에서 유효성 검사를 안전하게 활성화할 수 있으며, 이러한 유형의 구성 "다목적 서버"라고 합니다. 또는 다른 작업을 수행하지 않는 전용 검증인 실행하거나 P2P 모드 <mark style="background-color:yellow;">rippled</mark> 서버와 클러스터로 실행할 수도 있습니다.

검증인 실행에 대한 자세한 내용은 [rippled를 검증인으로 실행하기](../../tutorials/rippled/rippled-1/rippled.md)를 참조하세요.

## 리포팅 모드(Reporting Mode)

[![New in: rippled 1.7.0](https://img.shields.io/badge/New%20in-rippled%201.7.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/1.7.0)

리포팅 모드는 API 요청을 더 효율적으로 처리하기 위한 특수한 모드입니다. 이 모드에서 서버는 P2P 모드로 실행 중인 별도의 <mark style="background-color:yellow;">rippled</mark> 서버로부터 최신 유효한 ledger 데이터를 [gRPC](../../tutorials/rippled/rippled-1/grpc.md)를 통해 받아와 관계형 데이터베이스 ([PostgreSQL](https://www.postgresql.org/))에 로드합니다. 리포팅 모드 서버는 직접적으로 P2P 네트워크에 참여하지 않지만, 트랜잭션 제출과 같은 요청은 사용하는 P2P 모드 서버로 전달할 수 있습니다.

여러 리포팅 모드 서버는 PostgreSQL 데이터베이스와 [Apache Cassandra](https://cassandra.apache.org/\_/index.html) 클러스터에 대한 액세스를 공유하여 각 서버가 모든 데이터의 중복 사본을 필요로 하지 않고도 대량의 히스토리를 제공할 수 있습니다. 리포팅 모드 서버는 동일한 [rippledAPIs](../../references/http-websocket-apis/)를 통해 이 데이터를 제공하며, 기반이 되는 데이터의 저장 방식에 대한 차이점을 수용하기 위해 약간의 변경 사항이 있습니다.

가장 중요한 점으로, 리포팅 모드 서버는 대기 중이거나 유효하지 않은 ledger 데이터나 트랜잭션을 보고하지 않는다는 것입니다. 이 제한 사항은 [탈중앙화 거래소](../tokens/dex/)에서의 효율적인 액세스와 같이 변동 중인 데이터에 빠르게 액세스해야 하는 특정 사용 사례에 관련이 있습니다.

## Stand-Alone 모드&#x20;

stand-alone 모드에서 서버는 네트워크에 연결되지 않고 컨센서스 프로세스에 참여하지 않고 작동합니다. 컨센서스 프로세스가 없으므로 ledger를 수동으로 진행해야 하며 "닫힌" ledger와 "유효한" ledger 사이에 구분이 없습니다. 그러나 서버는 여전히 API 액세스를 제공하고 트랜잭션을 처리합니다. 이를 통해 다음과 같은 작업을 수행할 수 있습니다:

* 수정 사항이 탈중앙 네트워크 전체에 적용되기 전에 수정안의 효과를 테스트합니다.
* 처음부터 새로운 genesis  ledger를 생성합니다.
* 기존의 ledger 버전을 디스크에서 로드한 다음, 특정 트랜잭션을 다시 실행하여 결과를 재생산하거나 다른 가능성을 테스트합니다.

### 참고 <a href="#see-also" id="see-also"></a>

* **Tutorials:**
  * [Configure `rippled`](https://xrpl.org/configure-rippled.html)
  * [Use rippled in Stand-Alone Mode](https://xrpl.org/use-stand-alone-mode.html)

\
