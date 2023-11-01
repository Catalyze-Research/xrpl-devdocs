# JavaScript를 이용한 Send Payments(Send Payments Using JavaScript)

XRP Ledger(XRPL)는 강력하고 안전하며 사용자 정의가 가능한 서비스입니다. 자신만의 인터페이스를 만들어 기능을 시험해보고 특정 비즈니스 요구 사항을 지원할 수 있습니다.

이 빠른 시작에서는 XRP Ledger를 사용해 볼 수 있는 test harness interface에 대해 설명합니다. test harness에는 여러 계정이 표시되므로 한 계정에서 다른 계정으로 토큰을 전송하고 실시간으로 결과를 확인할 수 있습니다. 아래 이미지는 4단계가 완료된 Token Test Harness를 보여줍니다.

<figure><img src="../../../../.gitbook/assets/quickstart1 (2).png" alt=""><figcaption></figcaption></figure>

이는 많은 필드와 버튼이 있으며, 모두 함께 작동하여 몇 가지 중요한 실용적인 작업을 수행합니다. 하지만 XRP 레저를 시작하는 것은 그리 복잡하지 않습니다. 코끼리를 한 입씩 먹어치우면 어떤 작업도 어렵지 않습니다.

일반적으로 XRP 원장과 상호작용하기 위한 예시 기능은 네 단계로 구성됩니다.

1. Connect to the XRP Ledger and instantiate your wallet.
2. Make changes to the XRP Ledger using transactions.
3. Get the state of accounts and tokens on the XRP Ledger using requests.
4. Disconnect from the XRP Ledger.

각 레슨은 토큰 테스트 하네스를 한 번에 한 섹션씩 구축하는 방법을 보여줍니다. 각 모듈을 통해 테스트 원장과 의미 있는 상호작용을 시도해볼 수 있으며, 완전한 자바스크립트 및 HTML 코드 샘플과 코드 워크스루가 제공됩니다. 텍스트 편집기로 수정하고 브라우저에서 실행할 수 있는 각 섹션의 전체 소스 코드에 대한 링크도 있습니다. 기다릴 수 없다면 아래의 전제 조건을 충족한 다음 4강, NFT 전송으로 이동하여 전체 테스트 하네스를 바로 사용해볼 수 있습니다.

이 빠른 시작 튜토리얼에서는 기능을 구현하고 XRP 레저의 기능을 살펴보는 데 사용되는 API를 소개합니다. 이 튜토리얼은 API의 모든 기능을 보여주지는 않으며, 이 예시는 프로덕션이나 보안 결제에 사용하기 위한 것이 아닙니다.

대부분 가독성을 위해 간결함을 희생한 "brute force(무차별 대입)" 코드입니다. 각 예제는 이전 예제를 기반으로 새로운 JavaScript 파일과 지원 UI를 추가합니다. 여러분이 구축하는 애플리케이션이 이 예제를 기반으로 크게 개선되기를 기대합니다. 여러분의 피드백과 기여를 환영합니다.

## 전제 조건

시작하려면 로컬 디스크에 새 폴더를 만들고 npm을 사용하여 JavaScript 라이브러리를 설치하세요.

```
    npm install xrpl
```

[Quickstart Samples](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/quickstart/js/)을 다운로드하여 활용하는 것도 좋은 방법입니다.&#x20;
