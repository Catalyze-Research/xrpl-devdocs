# ledger\_closed

ledger\_closed 메소드는 가장 최근에 닫힌 ledger의 고유 식별자를 반환합니다. (이 ledger은 아직 검증되지 않았으며 불변하지 않습니다.)

## 요청 형식

요청 형식의 예시입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
   "id": 2,
   "command": "ledger_closed"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "ledger_closed",
    "params": [
        {}
    ]
}
```
{% endtab %}
{% endtabs %}

이 메소드는 매개변수를 허용하지 않습니다.

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 1,
  "status": "success",
  "type": "response",
  "result": {
    "ledger_hash": "17ACB57A0F73B5160713E81FE72B2AC9F6064541004E272BD09F257D57C30C02",
    "ledger_index": 6643099
  }
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
200 OK

{
    "result": {
        "ledger_hash": "8B5A0C5F6B198254A6E411AF55C29EE40AA86251D2E78DD0BB17647047FA9C24",
        "ledger_index": 8696231,
        "status": "success"
    }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`        | 유형       | 설명                                                                         |
| -------------- | -------- | -------------------------------------------------------------------------- |
| `ledger_hash`  | 문자열      | 이 원장 버전의 고유 [해시](https://xrpl.org/basic-data-types.html#hashes) (16진수)입니다. |
| `ledger_index` | 부호 없는 정수 | 이 원장 버전의 원장 인덱스입니다.                                                        |

## 발생 가능한 오류

* 모든 범용 오류 유형입니다.
