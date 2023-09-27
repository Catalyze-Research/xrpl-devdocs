# ledger\_current

ledger\_current 메소드는 현재 진행 중인 ledger의 고유 식별자를 반환합니다. 이 명령은 반환되는 ledger이 아직 유동적이기 때문에 테스트에 주로 유용합니다.

## 요청 형식

요청 형식의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
   "id": 2,
   "command": "ledger_current"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "ledger_current",
    "params": [
        {}
    ]
}
```
{% endtab %}
{% endtabs %}

요청에 매개변수가 없습니다.

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
200 OK

{
    "result": {
        "ledger_current_index": 8696233,
        "status": "success"
    }
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
200 OK

{
    "result": {
        "ledger_current_index": 8696233,
        "status": "success"
    }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`                | 유형                                                                       | 설명                  |
| ---------------------- | ------------------------------------------------------------------------ | ------------------- |
| `ledger_current_index` | 부호 없는 정수 - [원장 인덱스](https://xrpl.org/basic-data-types.html#ledger-index) | 이 원장 버전의 원장 인덱스입니다. |

현재 ledger의 해시는 내용과 함께 지속적으로 변경되므로 ledger\_hash 필드는 제공되지 않습니다.

## 발생 가능한 오류

* 모든 범용 오류 유형입니다.
