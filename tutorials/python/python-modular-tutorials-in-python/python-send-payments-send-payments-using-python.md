# python을 이용한 Send Payments(Send Payments Using Python)

XRP Ledger (XRPL)은 강력하고 안전하며 사용자 정의가 가능한 서비스입니다. 당신은 특정 비즈니스 요구 사항을 지원하도록 자신만의 인터페이스를 만들 수 있습니다.

이 빠른 시작은 XRP Ledger를 시도해 볼 수 있는 테스트 harness 인터페이스를 구축하는 방법을 설명합니다. 테스트 harness는 여러 계정을 표시하므로 한 계정에서 다른 계정으로 토큰을 전송하고 결과를 실시간으로 확인할 수 있습니다. 아래 이미지는 4단계 완료 시의 토큰 테스트 Harness를 보여줍니다.

<figure><img src="../../../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

많은 필드와 버튼이 모두 함께 작동하여 상당히 실질적인 작업을 수행합니다. 그러나 XRP Ledger를 시작하는 것은 그렇게 복잡하지 않습니다. 조금씩 한 발짝 한 발짝 나아가다 보면 어떤 작업도 어렵지 않습니다.

일반적으로 XRP Ledger와 상호작용하는 예제 함수는 네 단계를 포함합니다.

1. XRP Ledger에 연결하고 지갑을 인스턴스화합니다.
2. 트랜잭션을 사용하여 XRP Ledger에 변경 사항을 만듭니다.
3. 요청을 사용하여 XRP Ledger의 계정과 토큰의 상태를 얻습니다.
4. XRP Ledger에서 연결을 끊습니다.

각 교육과정은 토큰 테스트 Harness를 한 부분씩 구축하는 방법을 보여줍니다. 각 모듈에서는 완전한 Python 코드 샘플과 코드 연습을 통해 테스트 원장과 의미 있는 상호작용을 시도해볼 수 있습니다. 또한 각 섹션의 전체 소스 코드에 대한 링크가 있어 텍스트 편집기로 수정하고 Python 환경에서 실행할 수 있습니다. 예제는 어떤 순서로든 시도해 볼 수 있습니다.

이 빠른 시작 튜토리얼은 기능을 구현하고 XRP Ledger의 기능을 탐색하는 데 사용되는 API를 소개합니다. API의 모든 기능을 보여주는 것은 아니며, 이 예제는 프로덕션 또는 보안 결제용으로 사용할 수 없습니다.

대부분 가독성을 위해 간결함을 희생한 "브루트 포스" 코드입니다. 각 예제는 이전 단계를 기반으로 하며, 새로운 Python UI 파일과 모듈을 추가하여 레슨의 새로운 동작을 지원합니다. 여러분이 만든 애플리케이션은 이 예제를 통해 크게 개선될 것으로 기대합니다. 여러분의 피드백과 기여를 환영합니다.이 빠른 시작에서 당신 다음을 수행할 수 있습니다:

1. [계정 생성 및 XRP 전송](../1.-xrp-python.md)
2. [신뢰선 생성 및 화폐 전송.](python-send-payments-send-payments-using-python/currency-create-trust-line-and-send-currency-using-python.md)
3. [NFTs 발행 및 소각. ](python-nfts-nfts-using-python/nfts-mint-and-burn-nfts-using-python.md)
4. [NFTs 전송.](python-nfts-nfts-using-python/nfts-transfer-nfts-using-python.md)

또한 [NFT 판매 중개](python-nfts-nfts-using-python/nft-broker-an-nft-sale-using-python.md), [공인 발행자 지정](python-nfts-nfts-using-python/assign-an-authorized-minter-using-python.md) 방법을 보여주는 확장 레슨도 있습니다.

## 요구사항&#x20;

시작하려면 로컬 디스크에 새 폴더를 만들고 pip 사용하여 Python(xrpl-py) 라이브러리를 설치하세요.

```javascript
pip3 install xrpl-py
```

[Quickstart Samples](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/quickstart/js/) 다운로드 하세요.

{% hint style="info" %}
Note:

Quickstart Samples가 없다면, 이후 예제들을 시도할 수 없습니다.
{% endhint %}
