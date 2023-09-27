# peer\_reservations\_del

peer\_reservations\_del 메소드는 특정 피어 예약이 존재할 경우 이를 제거합니다. [![New in: rippled 1.4.0](https://img.shields.io/badge/New%20in-rippled%201.4.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/1.4.0)

peer\_reservations\_del 메소드는 권한이 없는 사용자가 실행할 수 없는 관리자 메소드입니다.

{% hint style="info" %}
Note:

피어 예약을 삭제해도 해당 피어가 연결되어 있는 경우 해당 피어와의 연결이 자동으로 해제되지는 않습니다.
{% endhint %}

## 요청 형식

요청 형식의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": "peer_reservations_del_example_1",
    "command": "peer_reservations_del",
    "public_key": "n9Jt8awsPzWLjBCNKVEEDQnw4bQEPjezfcQ4gttD1UzbLT1FoG99"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "peer_reservations_del",
    "params": [{
      "public_key": "n9Jt8awsPzWLjBCNKVEEDQnw4bQEPjezfcQ4gttD1UzbLT1FoG99"
    }]
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
#Syntax: peer_reservations_del <public_key>
rippled peer_reservations_del n9Jt8awsPzWLjBCNKVEEDQnw4bQEPjezfcQ4gttD1UzbLT1FoG99
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| `Field`      | 유형  | 설명                                 |
| ------------ | --- | ---------------------------------- |
| `public_key` | 문자열 | 제거할 피어 예약의 노드 공개 키 (base58 형식)입니다. |

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": "peer_reservations_del_example_1",
  "result": {
    "previous": {
      "description": "Ripple s1 server 'WOOL'",
      "node": "n9Jt8awsPzWLjBCNKVEEDQnw4bQEPjezfcQ4gttD1UzbLT1FoG99"
    }
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
      "previous" : {
         "description" : "Ripple s1 server 'WOOL'",
         "node" : "n9Jt8awsPzWLjBCNKVEEDQnw4bQEPjezfcQ4gttD1UzbLT1FoG99"
      },
      "status" : "success"
   }
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
Loading: "/etc/rippled.cfg"
Connecting to 127.0.0.1:5005

{
   "result" : {
      "previous" : {
         "description" : "Ripple s1 server 'WOOL'",
         "node" : "n9Jt8awsPzWLjBCNKVEEDQnw4bQEPjezfcQ4gttD1UzbLT1FoG99"
      },
      "status" : "success"
   }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`    | 유형 | 설명                                                                             |
| ---------- | -- | ------------------------------------------------------------------------------ |
| `previous` | 객체 | (생략 가능) 삭제하기 전 피어 예약의 마지막 상태인 피어 예약 객체입니다. 이 필드는 피어 예약이 성공적으로 삭제된 경우 항상 제공됩니다. |

{% hint style="info" %}
Note:

지정한 예약이 존재하지 않으면 이 명령은 빈 결과 객체와 함께 성공을 반환합니다. 이 경우 이전 필드는 생략됩니다.
{% endhint %}

## 피어 예약 객체

이전 필드가 제공되면 다음 필드와 함께 이 피어 예약의 이전 상태가 표시됩니다:

| `Field`       | 유형  | 설명                                      |
| ------------- | --- | --------------------------------------- |
| `node`        | 문자열 | 이 예약의 대상인 피어 서버의 노드 공개 키(예: base58)입니다. |
| `description` | 문자열 | (생략 가능) 이 피어 예약과 함께 제공된 설명(있는 경우)입니다.   |

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* publicMalformed - 요청의 공개 키 필드가 유효하지 않습니다. base58 형식의 유효한 노드 공개 키여야 합니다.
* reportingUnsupported - (리포팅 모드 서버만 해당) 이 메소드는 리포팅 모드에서 사용할 수 없습니다.
