# node\_to\_shard

node\_to\_shard 메소드는 ledger 저장소에서 샤드 저장소로 데이터를 복사하는 작업을 관리합니다. 데이터 복사를 시작, 중지하거나 복사 상태를 확인할 수 있습니다.

node\_to\_shard 메소드는 권한이 없는 사용자가 실행할 수 없는 관리자 메소드입니다.

## 요청 형식

요청 형식의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "command": "node_to_shard",
    "action": "start"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "node_to_shard",
    "params": [{
        "action": "start"
    }]
}
```
{% endtab %}

{% tab title="Commandline" %}
```
#Syntax: node_to_shard start|stop|status
rippled node_to_shard start
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| `Field`   | 유형  | 설명                                           |
| --------- | --- | -------------------------------------------- |
| `message` | 문자열 | 명령에 대한 응답으로 수행된 작업을 나타내는 사람이 읽을 수 있는 메시지입니다. |

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "result": {
    "message": "Database import initiated..."
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
      "message" : "Database import initiated...",
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
      "message" : "Database import initiated...",
      "status" : "success"
   }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`   | 유형  | 설명                                            |
| --------- | --- | --------------------------------------------- |
| `message` | 문자열 | 명령에 대한 응답으로, 수행된 작업을 나타내는 사람이 읽을 수 있는 메시지입니다. |

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* internal - 복사본이 실행 중이 아닌데 복사본의 상태를 확인하는 등 잘못된 작업을 시도한 경우.
* notEnabled - 서버가 히스토리 샤드를 저장하도록 구성되지 않은 경우.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락된 경우.
