# fee

수수료 명령은 거래 비용에 대한 공개 ledger 요구 사항의 현재 상태를 보고합니다. 이를 위해서는 수수료 확장 수정이 활성화되어 있어야 합니다. [![New in: rippled 0.31.0](https://img.shields.io/badge/New%20in-rippled%200.31.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/0.31.0)

권한이 없는 사용자가 사용할 수 있는 공개 커맨드라인 입니다. [![Updated in: rippled 0.32.0](https://img.shields.io/badge/Updated%20in-rippled%200.32.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.32.0)

## 요청 형식

요청 형식의 예시입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": "fee_websocket_example",
  "command": "fee"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "fee",
    "params": [{}]
}
```
{% endtab %}

{% tab title="Commandline" %}
```
#Syntax: fee
rippled fee
```
{% endtab %}
{% endtabs %}

요청에 매개변수가 포함되지 않습니다.

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": "fee_websocket_example",
  "status": "success",
  "type": "response",
  "result": {
    "current_ledger_size": "14",
    "current_queue_size": "0",
    "drops": {
      "base_fee": "10",
      "median_fee": "11000",
      "minimum_fee": "10",
      "open_ledger_fee": "10"
    },
    "expected_ledger_size": "24",
    "ledger_current_index": 26575101,
    "levels": {
      "median_level": "281600",
      "minimum_level": "256",
      "open_ledger_level": "256",
      "reference_level": "256"
    },
    "max_queue_size": "480"
  }
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
200 OK

{
    "result": {
        "current_ledger_size": "56",
        "current_queue_size": "11",
        "drops": {
            "base_fee": "10",
            "median_fee": "10000",
            "minimum_fee": "10",
            "open_ledger_fee": "2653937"
        },
        "expected_ledger_size": "55",
        "ledger_current_index": 26575101,
        "levels": {
            "median_level": "256000",
            "minimum_level": "256",
            "open_ledger_level": "67940792",
            "reference_level": "256"
        },
        "max_queue_size": "1100",
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
   "result" : {
      "current_ledger_size" : "16",
      "current_queue_size" : "2",
      "drops" : {
         "base_fee" : "10",
         "median_fee" : "11000",
         "minimum_fee" : "10",
         "open_ledger_fee" : "3203982"
      },
      "expected_ledger_size" : "15",
      "ledger_current_index": 26575101,
      "levels" : {
         "median_level" : "281600",
         "minimum_level" : "256",
         "open_ledger_level" : "82021944",
         "reference_level" : "256"
      },
      "max_queue_size" : "300",
      "status" : "success"
   }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`                    | 유형      | 설명                                                                                                                                                                              |
| -------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `current_ledger_size`      | 문자열(정수) | 진행 중인 원장에 임시로 포함된 거래 수입니다.                                                                                                                                                      |
| `current_queue_size`       | 문자열(정수) | 다음 원장을 위해 현재 대기 중인 트랜잭션 수입니다.                                                                                                                                                   |
| `drops`                    | 객체      | XRP로 표시되는 거래 비용(`Fee`거래 분야) 에 대한 다양한 정보입니다.                                                                                                                                     |
| `drops.base_fee`           | 문자열(정수) | 최소 부하 상태에서 참조 트랜잭션이 원장에 포함되는 데 필요한 트랜잭션 비용으로, XRP 드롭으로 표시됩니다.                                                                                                                   |
| `drops.median_fee`         | 문자열(정수) | 이전에 검증된 원장에 포함된 거래 중 평균 거래 비용의 근사치로 XRP 드롭으로 표시됩니다.                                                                                                                             |
| `drops.minimum_fee`        | 문자열(정수) | XRP 드롭으로 표시되는 이후 원장을 위해 대기열에 추가되는 참조 거래 에 대한 최소 거래 비용입니다. 보다 크면 `base_fee`트랜잭션 대기열이 가득 찼습니다.                                                                                    |
| `drops.open_ledger_fee`    | 문자열(정수) | XRP 드롭으로 표시되는 현재 공개 원장에 포함되기 위해 참조 거래가 지불해야 하는 최소 거래 비용입니다.                                                                                                                     |
| `expected_ledger_size`     | 문자열(정수) | 현재 원장에 포함될 것으로 예상되는 대략적인 거래 수입니다. 이는 이전 원장의 거래 수를 기반으로 합니다.                                                                                                                     |
| `ledger_current_index`     | 숫자      | 이 통계가 설명하는 현재 공개 원장의 원장 인덱스 입니다.[![새로운 기능: 파문 0.50.0](https://img.shields.io/badge/New%20in-rippled%200.50.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.50.0) |
| `levels`                   | 객체      | [수수료 수준별](https://xrpl.org/transaction-cost.html#fee-levels) 거래 비용에 대한 다양한 정보입니다. 수수료 수준의 비율은 특정 거래의 최소 비용을 기준으로 모든 거래에 적용됩니다.                                                  |
| `levels.median_level`      | 문자열(정수) | 이전에 검증된 원장의 거래 중 중간 거래 비용으로, 수수료 수준으로 표시됩니다.                                                                                                                                    |
| `levels.minimum_level`     | 문자열(정수) | 미래 원장을 위해 대기하는 데 필요한 최소 거래 비용으로, 수수료 수준으로 표시됩니다.                                                                                                                                |
| `levels.open_ledger_level` | 문자열(정수) | 현재 공개 원장에 포함되는 데 필요한 최소 거래 비용은 [수수료 수준](https://xrpl.org/transaction-cost.html#fee-levels) 으로 표시됩니다.                                                                            |
| `levels.reference_level`   | 문자열(정수) | [수수료 수준](https://xrpl.org/transaction-cost.html#fee-levels) 으로 표시되는 최소 거래 비용에 해당합니다.                                                                                            |
| `max_queue_size`           | 문자열(정수) | [트랜잭션 큐가](https://xrpl.org/transaction-cost.html#queued-transactions) 현재 보유할 수 있는 최대 트랜잭션 수입니다.                                                                                 |

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
