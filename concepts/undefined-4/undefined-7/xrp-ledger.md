# XRP Ledger에 코드를 기여하는 방법

XRP Ledger를 구동하는 소프트웨어는 오픈 소스입니다. 누구나 다운로드하고 수정하고 확장하거나 탐색할 수 있습니다. 코드를 기여하려면 <mark style="background-color:yellow;">rippled</mark>에 코드를 추가하기 전에 커뮤니티와 협력하여 변경 사항의 사양을 정의하고 코드를 테스트하는 것이 중요합니다.

## XRP Ledger 표준&#x20;

<mark style="background-color:yellow;">rippled</mark>의 변경 사항은 XRP Ledger 표준 (XLS)에 의해 추적됩니다. XLS는 변경 사항을 식별하고 세부 사항을 설명하는 문서입니다. 개발을 시작하기 전에 [XRPL-Standards 저장소](https://github.com/XRPLF/XRPL-Standards/discussions)에서 토론을 시작해야 합니다. 이를 통해 커뮤니티가 변경 사항에 대해 토론하고 의견을 제공할 수 있습니다.

{% hint style="info" %}
Note:

버그 수정은 XLS가 필요하지 않지만, 수정안이 필요할 수 있습니다.
{% endhint %}

XLS를 생성하는 것은 별도의 프로세스가 있지만, 다음과 같이 요약할 수 있습니다:

1. 토론을 시작하고 피드백을 수집합니다.&#x20;
2. 표준 저장소에 XLS 초안을 작성합니다.&#x20;
3. XLS 초안을 후보 사양으로 게시합니다.&#x20;

자세한 내용은 [XLS 기여 가이드](https://github.com/XRPLF/XRPL-Standards/blob/master/CONTRIBUTING.md)를 참조하십시오.

## 수정안 구현&#x20;

XLS를 생성한 후 변경 사항이 수정안이 필요한지 여부를 결정해야 합니다. **트랜잭션 처리**에 영향을 주는 변경 사항은 특히 다음과 같은 변경 사항이 수정안이 필요합니다:

* Ledger 규칙을 수정하여 다른 결과를 가져옵니다.&#x20;
* 트랜잭션을 추가하거나 제거합니다.&#x20;
* 컨센서스에 영향을 줍니다.

{% hint style="info" %}
Note:\
수정안이 필요하지 않은 경우 코드 작성 및 배포로 직접 이동할 수 있습니다.
{% endhint %}

수정안으로 코드를 구현하려면 다음 파일에 수정안을 추가해야 합니다:

* **Feature.cpp:**&#x20;

개발이 완료될 때까지 <mark style="background-color:yellow;">Supported</mark> 매개변수를 <mark style="background-color:yellow;">no</mark>로 설정해야 합니다.

버그 수정의 경우 <mark style="background-color:yellow;">DefaultVote</mark> 매개변수를 <mark style="background-color:yellow;">yes</mark>로 설정해야 하며, 기타 모든 경우는 기본값으로 <mark style="background-color:yellow;">no</mark>로 설정됩니다.

* **Feature.h:** <mark style="background-color:yellow;">numFeatures</mark> 카운터를 증가시키고 <mark style="background-color:yellow;">extern uint256 const</mark> 변수를 선언합니다.&#x20;

## 코딩 및 배포&#x20;

일반적인 개발 경로는 다음과 같이 나눌 수 있습니다:

1. [<mark style="background-color:yellow;">rippled</mark> 저장소](https://github.com/XRPLF/rippled)에서 포크(fork) 또는 브랜치(branch)를 생성하여 코드를 개발합니다.

{% hint style="info" %}
Tip:

어떤 부분부터 시작해야 할지 모르는 경우 Dev Null Productions에서 상세하고 철저한 [rippled 소스 코드 가이드](https://xrpintel.com/source)를 제공합니다.
{% endhint %}

2. 단위 및 통합 테스트를 실행합니다. 서버를 _stand-alone 모드는_ 변경 사항을 격리된 환경에서 테스트하는 데 유용하지만, 광범위한 변경 사항의 경우 개인 네트워크를 구축하는 것이 좋습니다.
3. <mark style="background-color:yellow;">XRPLF:develop</mark>에 pull request를 생성합니다.\
   **수정안에 대한 참고 사항:** **Feature.cpp**에서 Supported 매개변수를 <mark style="background-color:yellow;">yes</mark>로 업데이트합니다.
4. pull request가 XRP Ledger 관리자에 의해 승인되면 코드가 <mark style="background-color:yellow;">develop</mark>에 통합되고 Devnet에서 추가적인 테스트를 수행할 수 있습니다.\
   **수정안에 대한 참고 사항:** <mark style="background-color:yellow;">DefaultVote</mark> 매개변수는 이제 잠겨 있습니다. - 수정안에 문제가 발견된 경우 수정 작업을 다시 시작하고 새로운 PR을 제출해야 합니다. 새로운 PR에서 기본 투표를 변경할 수 있습니다.
5. 분기별로 <mark style="background-color:yellow;">develop</mark>에서 승인된 PR을 사용하여 릴리스 후보를 빌드합니다. 패키지를 Testnet에 배포하고 Mainnet의 일부 노드에 배포합니다. 릴리스 후보에서 문제가 발견되지 않으면 코드가 <mark style="background-color:yellow;">master</mark>에 통합되고 Mainnet의 노드는 이 빌드로 업그레이드할 수 있습니다.
6. 새로운 수정안은 컨센서스 프로세스를 거치고 유효성 검증인들은 해당 수정안을 활성화할지에 대해 투표합니다.

## Code 흐름 차트

<figure><img src="https://xrpl.org/img/Contribute%20Code%20Flowchart.png" alt=""><figcaption></figcaption></figure>
