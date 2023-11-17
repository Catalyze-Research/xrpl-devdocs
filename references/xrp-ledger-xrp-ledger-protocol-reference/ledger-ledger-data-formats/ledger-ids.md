# Ledger 객체 IDs

ledger 상태 데이터의 각 객체에는 고유한 ID가 있습니다. ID는 네임스페이스 식별자와 함께 개체의 중요한 콘텐츠를 해시하여 파생됩니다. ledger 객체 유형에 따라 어떤 네임스페이스 식별자를 사용할지, 해시에 어떤 콘텐츠를 포함할지가 결정됩니다. 이렇게 하면 모든 ID가 고유합니다. rippled은 해시를 계산하기 위해 SHA-512를 사용한 다음 결과를 처음 256비트로 잘라냅니다. 비공식적으로 **SHA-512Half**라고 불리는 이 알고리즘은 SHA-256과 비슷한 수준의 보안을 제공하지만 64비트 프로세서에서 더 빠르게 실행됩니다.

일반적으로 ledger 개체의 ID는 개체의 콘텐츠와 동일한 수준에서 JSON의 인덱스 필드로 반환됩니다. 트랜잭션 메타데이터에서 JSON의 ledger 객체 ID는 LedgerIndex입니다.

{% hint style="info" %}
Tip:

ledger에 있는 개체의 인덱스 또는 LedgerIndex 필드는 ledger 개체 ID입니다. 이는 ledger 인덱스와 동일하지 않습니다.
{% endhint %}

<figure><img src="https://xrpl.org/img/ledger-object-ids.svg" alt=""><figcaption></figcaption></figure>

&#x20;
