# get\_counts

get\_counts 명령은 서버의 상태에 대한 다양한 통계, 주로 서버가 현재 메모리에 보유하고 있는 다양한 유형의 오브젝트 수를 제공합니다.

get\_counts 메소드는 권한이 없는 사용자가 실행할 수 없는 관리자 메소드입니다.

## 요청 형식

요청 형식의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": 90,
    "command": "get_counts",
    "min_count": 100
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "get_counts",
    "params": [
        {
            "min_count": 100
        }
    ]
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
#Syntax: get_counts [min_count]
rippled get_counts 100
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| `Field`     | 유형           | 설명                      |
| ----------- | ------------ | ----------------------- |
| `min_count` | 숫자(부호 없는 정수) | 이보다 높은 값이 있는 필드만 반환합니다. |

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="JSON-RPC" %}
```json
{
   "result" : {
      "AL_hit_rate" : 48.36725616455078,
      "HashRouterEntry" : 3048,
      "Ledger" : 46,
      "NodeObject" : 10417,
      "SLE_hit_rate" : 64.62035369873047,
      "STArray" : 1299,
      "STLedgerEntry" : 646,
      "STObject" : 6987,
      "STTx" : 4104,
      "STValidation" : 610,
      "Transaction" : 4069,
      "dbKBLedger" : 10733,
      "dbKBTotal" : 39069,
      "dbKBTransaction" : 26982,
      "fullbelow_size" : 0,
      "historical_perminute" : 0,
      "ledger_hit_rate" : 71.0565185546875,
      "node_hit_rate" : 3.808214902877808,
      "node_read_bytes" : 393611911,
      "node_reads_hit" : 1283098,
      "node_reads_total" : 679410,
      "node_writes" : 1744285,
      "node_written_bytes" : 794368909,
      "status" : "success",
      "treenode_cache_size" : 6650,
      "treenode_track_size" : 598631,
      "uptime" : "3 hours, 50 minutes, 27 seconds",
      "write_load" : 0
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
      "AL_hit_rate" : 48.36725616455078,
      "HashRouterEntry" : 3048,
      "Ledger" : 46,
      "NodeObject" : 10417,
      "SLE_hit_rate" : 64.62035369873047,
      "STArray" : 1299,
      "STLedgerEntry" : 646,
      "STObject" : 6987,
      "STTx" : 4104,
      "STValidation" : 610,
      "Transaction" : 4069,
      "dbKBLedger" : 10733,
      "dbKBTotal" : 39069,
      "dbKBTransaction" : 26982,
      "fullbelow_size" : 0,
      "historical_perminute" : 0,
      "ledger_hit_rate" : 71.0565185546875,
      "node_hit_rate" : 3.808214902877808,
      "node_read_bytes" : 393611911,
      "node_reads_hit" : 1283098,
      "node_reads_total" : 679410,
      "node_writes" : 1744285,
      "node_written_bytes" : 794368909,
      "status" : "success",
      "treenode_cache_size" : 6650,
      "treenode_track_size" : 598631,
      "uptime" : "3 hours, 50 minutes, 27 seconds",
      "write_load" : 0
   }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따릅니다. 결과에 포함된 필드 목록은 사전 통지 없이 변경될 수 있지만 다음 중 어느 것이든 포함될 수 있습니다:

| `Field`       | 유형  | 설명                          |
| ------------- | --- | --------------------------- |
| `Transaction` | 숫자  | `Transaction`메모리에 있는 객체 의 수 |
| `Ledger`      | 숫자  | 메모리에 있는 원장 수                |
| `uptime`      | 문자열 | 이 서버가 중단 없이 실행된 시간입니다.      |

대부분의 다른 항목의 경우 값은 현재 메모리에 있는 해당 유형의 객체 수를 나타냅니다.

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
