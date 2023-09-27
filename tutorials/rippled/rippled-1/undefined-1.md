# 수정안 테스트

제안된 수정안이 프로덕션 네트워크에서 완전히 활성화되기 전에 rippled에 어떻게 작동하는지 테스트할 수 있습니다. 컨센서스 네트워크의 다른 구성원은 이 기능을 사용할 수 없으므로 서버를 stand-alone 모드로 실행하세요.

{% hint style="info" %}
Caution:

이 기능은 개발 목적으로만 사용됩니다.
{% endhint %}

기능을 강제로 사용 설정하려면 수정안의 짧은 이름을 가진 \[features] 구절을 rippled.cfg 파일에 추가하세요. 각 수정 사항에는 고유한 줄이 필요합니다.

{% tabs %}
{% tab title="Example" %}
```
[features]
MultiSign
TrustSetAuth
```
{% endtab %}
{% endtabs %}
