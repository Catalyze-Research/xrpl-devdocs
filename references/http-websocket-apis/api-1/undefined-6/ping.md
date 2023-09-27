# ping

ping 명령은 클라이언트가 연결 상태와 대기 시간을 테스트할 수 있도록 확인을 반환합니다.

## 요청 형식

요청 형식의 예시입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": 1,
    "command": "ping"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "ping",
    "params": [
        {}
    ]
}
```
{% endtab %}
{% endtabs %}

요청에 매개 변수가 포함되어 있지 않습니다.

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": 1,
    "result": {},
    "status": "success",
    "type": "response"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
200 OK

{
    "result": {
        "status": "success"
    }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며 필드가 포함되지 않은 성공적인 결과입니다. 클라이언트는 요청에서 응답까지의 왕복 시간을 지연 시간으로 측정할 수 있습니다.

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
