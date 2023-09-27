# peer\_reservations\_add

peer\_reservations\_add 메소드는 XRP Ledger P2P 네트워크에서 특정 피어 서버를 위한 예약 슬롯을 추가하거나 업데이트합니다. [![New in: rippled 1.4.0](https://img.shields.io/badge/New%20in-rippled%201.4.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/1.4.0)

peer\_reservations\_add 메소드는 권한이 없는 사용자가 실행할 수 없는 관리자 메소드입니다.

## 요청 형식

요청 형식의 예시입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": "peer_reservations_add_example_1",
    "command": "peer_reservations_add",
    "public_key": "n9Jt8awsPzWLjBCNKVEEDQnw4bQEPjezfcQ4gttD1UzbLT1FoG99",
    "description": "Ripple s1 server 'WOOL'"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "peer_reservations_add",
    "params": [{
      "public_key": "n9Jt8awsPzWLjBCNKVEEDQnw4bQEPjezfcQ4gttD1UzbLT1FoG99",
      "description": "Ripple s1 server 'WOOL'"
    }]
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
#Syntax: peer_reservations_add <public_key> [<description>]
rippled peer_reservations_add n9Jt8awsPzWLjBCNKVEEDQnw4bQEPjezfcQ4gttD1UzbLT1FoG99 "Ripple s1 server 'WOOL'"
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| `Field`       | 유형  | 설명                                                              |
| ------------- | --- | --------------------------------------------------------------- |
| `public_key`  | 문자열 | base58에서 예약을 추가할 피어 예약의 노드 공개 키입니다.                             |
| `description` | 문자열 | (선택 사항) 피어 예약에 대한 사용자 지정 설명입니다. 서버가 다시 시작되면 64자를 초과하는 설명은 잘립니다. |

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": "peer_reservations_add_example_1",
  "result": {
    "previous": {
      "description": "Maecenas atavis edite regibus, O et praesidium et dulce decus meum, Sunt quos curriculo pulverem Olympicum Collegisse iuvat metaque fervidis Evitata rotis palmaque nobilis Terrarum dominos evehit ad deos; Hunc, si mobilium turba Quiritium Certat tergeminis tollere honoribus; Illum, si proprio condidit horreo, Quidquid de Libycis verritur areis.",
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
  "result": {
    "previous": {
      "description": "Maecenas atavis edite regibus, O et praesidium et dulce decus meum, Sunt quos curriculo pulverem Olympicum Collegisse iuvat metaque fervidis Evitata rotis palmaque nobilis Terrarum dominos evehit ad deos; Hunc, si mobilium turba Quiritium Certat tergeminis tollere honoribus; Illum, si proprio condidit horreo, Quidquid de Libycis verritur areis.",
      "node": "n9Jt8awsPzWLjBCNKVEEDQnw4bQEPjezfcQ4gttD1UzbLT1FoG99"
    },
    "status": "success"
  }
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
Loading: "/etc/rippled.cfg"
Connecting to 127.0.0.1:5005

{
  "result": {
    "previous": {
      "description": "Maecenas atavis edite regibus, O et praesidium et dulce decus meum, Sunt quos curriculo pulverem Olympicum Collegisse iuvat metaque fervidis Evitata rotis palmaque nobilis Terrarum dominos evehit ad deos; Hunc, si mobilium turba Quiritium Certat tergeminis tollere honoribus; Illum, si proprio condidit horreo, Quidquid de Libycis verritur areis.",
      "node": "n9Jt8awsPzWLjBCNKVEEDQnw4bQEPjezfcQ4gttD1UzbLT1FoG99"
    },
    "status": "success"
  }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`    | 유형 | 설명                                                                                                                     |
| ---------- | -- | ---------------------------------------------------------------------------------------------------------------------- |
| `previous` | 객체 | (생략 가능) 동일한 노드 공개 키로 이미 예약이 있었던 경우, 동일한 노드 공개 키에 대한 이전 항목입니다. **이 개체는 아래 설명과 같이 Peer Reservation Object** 형식으로 지정됩니다 . |

동일한 노드 공개 키에 대한 이전 항목이 없는 경우 결과 객체는 비어 있습니다.

## 피어 예약 객체

이전 필드가 제공된 경우, 다음 필드와 함께 이 피어 예약의 이전 상태를 표시합니다:

| `Field`       | 유형  | 설명                                      |
| ------------- | --- | --------------------------------------- |
| `node`        | 문자열 | 이 예약을 위한 피어 서버의 노드 공개 키(예: base58) 입니다. |
| `description` | 문자열 | (생략 가능) 이 피어 예약과 함께 제공되는 설명(있는 경우)입니다.  |

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* publicMalformed - 요청의 공개 키 필드가 유효하지 않습니다. base58 형식의 유효한 노드 공개 키여야 합니다.
* reportingUnsupported - (리포팅 모드 서버만 해당) 이 메소드는 리포팅 모드에서 사용할 수 없습니다.
