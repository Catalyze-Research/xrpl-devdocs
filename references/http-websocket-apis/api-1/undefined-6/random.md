# random

문랜덤 명령은 클라이언트가 난수 생성을 위한 엔트로피 소스로 사용할 난수를 제공합니다.

## 요청 형식

요청 형식의 예시입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": 1,
    "command": "random"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "random",
    "params": [
        {}
    ]
}
```
{% endtab %}
{% endtabs %}

요청에 매개변수가 포함되어 있지 않습니다.

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": 1,
    "result": {
        "random": "8ED765AEBBD6767603C2C9375B2679AEC76E6A8133EF59F04F9FC1AAA70E41AF"
    },
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
        "random": "4E57146AA47BC6E88FDFE8BAA235B900126C916B6CC521550996F590487B837A",
        "status": "success"
    }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`  | 유형  | 설명                   |
| -------- | --- | -------------------- |
| `random` | 문자열 | 임의의 256비트 16진수 값입니다. |

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* 내부 - 난수 생성기와 관련된 내부 오류가 발생했습니다.
