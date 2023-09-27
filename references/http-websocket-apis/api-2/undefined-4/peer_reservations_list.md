# peer\_reservations\_list

peer\_reservations\_list 메소드는 피어 예약을 나열합니다.[![New in: rippled 1.4.0](https://img.shields.io/badge/New%20in-rippled%201.4.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/1.4.0)

peer\_reservations\_list 메소드는 권한이 없는 사용자가 실행할 수 없는 관리자 메소드입니다.

## 요청 형식

요청 형식의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": "peer_reservations_list_example_1",
  "command": "peer_reservations_list"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "peer_reservations_list"
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
#Syntax: peer_reservations_list
rippled peer_reservations_list
```
{% endtab %}
{% endtabs %}

이 요청은 매개변수를 받지 않습니다.

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": "peer_reservations_list_example_1",
  "result": {
    "reservations": [
      {
        "description": "Ripple s1 server 'WOOL'",
        "node": "n9Jt8awsPzWLjBCNKVEEDQnw4bQEPjezfcQ4gttD1UzbLT1FoG99"
      },
      {
        "node": "n9MZRo92mzYjjsa5XcqnPC7GFYAnENo9VfJzKmpcS9EFZvw5fgwz"
      }
    ]
  },
  "status": "success",
  "type": "response"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
  "result" : {
    "reservations" : [
       {
          "description" : "Ripple s1 server 'WOOL'",
          "node" : "n9Jt8awsPzWLjBCNKVEEDQnw4bQEPjezfcQ4gttD1UzbLT1FoG99"
       },
       {
          "node" : "n9MZRo92mzYjjsa5XcqnPC7GFYAnENo9VfJzKmpcS9EFZvw5fgwz"
       }
    ],
    "status" : "success"
  }
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
Loading: "/etc/rippled.cfg"
2019-Dec-27 21:56:07.253260422 HTTPClient:NFO Connecting to 127.0.0.1:5005

{
  "result" : {
    "reservations" : [
       {
          "description" : "Ripple s1 server 'WOOL'",
          "node" : "n9Jt8awsPzWLjBCNKVEEDQnw4bQEPjezfcQ4gttD1UzbLT1FoG99"
       },
       {
          "node" : "n9MZRo92mzYjjsa5XcqnPC7GFYAnENo9VfJzKmpcS9EFZvw5fgwz"
       }
    ],
    "status" : "success"
  }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`        | 유형 | 설명                                             |
| -------------- | -- | ---------------------------------------------- |
| `reservations` | 배열 | 기존 피어 예약 목록입니다. 각 구성원은 아래에 설명된 대로 피어 예약 객체입니다. |

## 피어 예약 객체

예약 배열의 각 멤버는 하나의 피어 예약을 설명하는 JSON 객체입니다. 이 객체에는 다음과 같은 필드가 있습니다:

| `Field`       | 유형  | 설명                                      |
| ------------- | --- | --------------------------------------- |
| `node`        | 문자열 | 이 예약의 대상인 피어 서버의 노드 공개 키(예: base58)입니다. |
| `description` | 문자열 | (생략 가능) 이 피어 예약과 함께 제공된 설명(있는 경우)입니다.   |

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* reportingUnsupported - (리포팅 모드 서버만 해당) 이 메소드는 리포팅 모드에서 사용할 수 없습니다.
