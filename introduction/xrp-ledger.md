# XRP Ledger란?



XRP Ledger는 자체 디지털 통화를 사용하여 금융 거래를 처리하고 기록하는 탈중앙화된 블록체인입니다.

## 블록체인이란?

블록체인은 지속적으로 증가하는 일련의 기록입니다. 블록체인은 데이터 블록에서 시작됩니다.

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

A group of trusted validator nodes reach consensus that the data is valid.

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

이 블록은 매우 복잡한 64진수 컴퓨터 생성 암호화 해시 번호로 고유하게 식별됩니다.

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

블록은 생성 시간이 표시된 타임스탬프로도 식별할 수 있습니다.

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

각 검증자 노드는 데이터 블록의 자체 복사본을 가져옵니다. 단일한 중앙권한은 없습니다. 모든 사본은 동일하게 유효합니다.

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

각 블록에는 이전 블록에 대한 링크인 해시 포인터가 포함되어 있습니다. 또한 타임스탬프, 새 데이터, 고유한 해시 번호가 있습니다.

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

이 구조를 사용하면 각 블록은 체인에서 명확한 위치를 가지며 이전 데이터 블록으로 다시 연결됩니다. 이렇게 하면 불변의 블록 체인이 생성됩니다. 이전 블록을 역추적하여 체인의 모든 최신 정보를 언제든지 확인할 수 있습니다.

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

블록체인은 설계상 데이터 수정에 강합니다. 모든 원장 노드는 블록체인의 똑같은 사본을 받습니다.

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

이렇게 하면 당사자 간의 거래를 효율적이고 검증 가능하며 영구적인 방식으로 기록하는 개방형 분산 원장이 생성됩니다.

일단 기록된 특정 블록의 데이터는 검증자 과반수가 변경에 동의하지 않는 한 소급하여 변경할 수 없습니다. 그렇게 되면 이후의 모든 블록은 동일한 방식으로 변경됩니다(매우 드물고 예외적인 경우).



## 연합 합의 프로세스는 어떻게 진행되나요? <a href="#how-does-the-federated-consensus-process-work" id="how-does-the-federated-consensus-process-work"></a>

XRPL에 있는 대부분의 \*rippled 서버는 트랜잭션을 모니터링하거나 제안합니다. 서버의 중요한 하위 집합은 검증자로 실행됩니다. 이러한 신뢰할 수 있는 서버는 새로운 트랜잭션 목록을 새로운 가능한 원장 인스턴스(블록체인의 새로운 블록)에 축적합니다.

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

_\*"rippled servers"는 XRPL (XRP Ledger)의 서버를 가리킵니다. XRPL 네트워크에는 여러 대의 서버가 운영되며 네트워크 상태를 유지하고 거래를 처리합니다._

* _"ripple"은 이 기술을 개발한 회사의 이름으로, XRPL은 그들이 만든 블록체인입니다. 회사명과 블록체인 이름이 비슷해서 혼동하기 쉽습니다._
* _"rippled"는 XRPL 노드 소프트웨어의 이름입니다. XRPL 네트워크에서 서버 역할을 하는 컴퓨터는 모두 rippled 소프트웨어를 실행합니다._

_따라서 "rippled servers"는 XRPL 네트워크의 서버 노드들을 가리키며, 이러한 서버들은 XRPL 네트워크를 유지하고 거래를 처리하는 역할을 합니다. 이 중 일부 서버는 "validators"로 알려져 있으며 신뢰할 수 있는 서버로서 새로운 거래를 처리하고 새로운 레저 인스턴스(블록체인의 새로운 블록)에 추가합니다. 이러한 validators는 네트워크의 안정성과 신뢰성을 유지하는 데 중요한 역할을 합니다._



이들은 자신의 리스트를 다른 모든 검증자와 공유합니다. 검증자는 서로가 제안한 변경 사항을 통합하고 새 버전의 원장 제안서를 배포합니다.

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

검증자의 80%가 일련의 트랜잭션에 동의하면 체인 끝에 새 원장 인스턴스를 생성하고 프로세스를 다시 시작합니다. 이 합의 과정은 4\~6초 정도 소요됩니다. [https://livenet.xrpl.org/](https://livenet.xrpl.org/) 을 방문하여 실시간으로 원장 인스턴스가 생성되는 과정을 모니터링할 수 있습니다.

## 어떤 네트워크를 이용할 수 있나요?

XRPL은 rippled 서버의 인스턴스를 직접 설정하고 연결하고자 하는 누구에게나 열려 있습니다. 노드는 네트워크를 모니터링하거나 트랜잭션을 수행하거나 검증자가 될 수 있습니다. 활성 XRPL 네트워크는 일반적으로 메인넷이라고 합니다.

자신의 자금을 투자하지 않고 XRPL의 기능을 시험해보고 싶은 개발자나 신규 사용자를 위해 테스트넷과 데브넷이라는 두 가지 개발자 환경이 있습니다. 사용자는 1,000 (가짜) XRP로 자금을 조달한 계정을 생성하고 두 환경 중 하나에 연결하여 XRPL과 상호 작용할 수 있습니다.\


Next: XRP란?





