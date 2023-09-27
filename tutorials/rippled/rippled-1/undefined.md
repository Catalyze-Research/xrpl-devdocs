# 수정안 투표 구성

검증인으로 구성된 서버는 기능 메소드를 사용하여 XRP ledger 프로토콜 수정에 투표할 수 있습니다. (이 방법은 관리자 액세스 권한이 필요합니다.)

예를 들어, "SHAMapV2" 수정안에 반대표를 던지려면 다음 명령을 실행합니다:

{% tabs %}
{% tab title="WebSocket" %}
```
{
  "id": "any_id_here",
  "command": "feature",
  "feature": "SHAMapV2",
  "vetoed": true
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```
{
    "method": "feature",
    "params": [
        {
            "feature": "SHAMapV2",
            "vetoed": true
        }
    ]
}
```
{% endtab %}

{% tab title="Commandline" %}
```
rippled feature SHAMapV2 reject
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Note:

수정안의 짧은 이름은 대소문자를 구분합니다. 대소문자를 구분하지 않는 16진수로 수정안 ID를 사용할 수도 있습니다.
{% endhint %}

## 구성 파일 사용

구성 파일을 사용하여 수정안 투표를 구성하려는 경우, \[rpc\_startup] 구에 한 줄을 추가하여 명시적으로 투표할 때마다 시작 시 자동으로 명령이 실행되도록 할 수 있습니다. \
예시:

```
[rpc_startup]
{ "command": "feature", "feature": "SHAMapV2", "vetoed": true }
```

변경 사항을 적용하려면 서버를 다시 시작해야 합니다.

{% hint style="info" %}
Caution:

서버가 시작될 때마다 \[rpc\_startup] 구절의 모든 명령이 실행되므로 서버가 실행되는 동안 구성한 투표 설정이 무시될 수 있습니다.
{% endhint %}
