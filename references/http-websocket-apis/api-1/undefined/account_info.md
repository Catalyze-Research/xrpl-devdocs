# account\_info

account\_info 커맨드라인은 계정, 계정 활동, XRP 잔액에 대한 정보를 검색합니다. 검색된 모든 정보는 특정 버전의 원장을 기준으로 합니다.

## 요청 형식

account\_info 요청의 예시입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 2,
  "command": "account_info",
  "account": "rG1QQv2nh2gr7RCZ1P8YYcBUKCCN633jCn",
  "ledger_index": "current",
  "queue": true
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "account_info",
    "params": [
        {
            "account": "rG1QQv2nh2gr7RCZ1P8YYcBUKCCN633jCn",
            "ledger_index": "current",
            "queue": true
        }
    ]
}
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| `Field`        | 유형                                                           | 필수 여부 | 설명                                                                                                                                                                                    |
| -------------- | ------------------------------------------------------------ | ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `account`      | 문자열 - [주소](https://xrpl.org/basic-data-types.html#addresses) | 예     | 조회할 계정입니다.[![업데이트: 파문 1.11.0](https://img.shields.io/badge/Updated%20in-rippled%201.11.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.11.0)                           |
| `ledger_hash`  | 배열                                                           | 아니요   | 사용할 원장 버전에 대한 20바이트 16진수 문자열입니다. ([Specifying Ledgers](https://xrpl.org/basic-data-types.html#specifying-ledgers) 참조)                                                                 |
| `ledger_index` | 숫자 또는 문자열                                                    | 아니요   | 사용할 원장의 [원장 인덱스](https://xrpl.org/basic-data-types.html#ledger-index) 또는 자동으로 원장을 선택하는 단축 문자열입니다.([Specifying Ledgers](https://xrpl.org/basic-data-types.html#specifying-ledgers) 참조) |
| `queue`        | Boolean                                                      | 아니요   | 참이면 이 계정에서 보낸 대기 중인 트랜잭션에 대한 통계를 반환합니다. 현재 열려 있는 원장의 데이터를 쿼리할 때만 사용할 수 있습니다. 보고 모드의 서버에서는 사용할 수 없습니다.                                                                                 |
| `signer_lists` | Boolean                                                      | 아니요   | true이면 이 계정과 연결된 모든 서명자 목록 객체를 반환합니다.                                                                                                                                                 |

다음 필드는 더 이상 사용되지 않으므로 제공해서는 안 됩니다: ident, ledger, strict.

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": 5,
    "status": "success",
    "type": "response",
    "result": {
        "account_data": {
            "Account": "rG1QQv2nh2gr7RCZ1P8YYcBUKCCN633jCn",
            "Balance": "999999999960",
            "Flags": 8388608,
            "LedgerEntryType": "AccountRoot",
            "OwnerCount": 0,
            "PreviousTxnID": "4294BEBE5B569A18C0A2702387C9B1E7146DC3A5850C1E87204951C6FDAA4C42",
            "PreviousTxnLgrSeq": 3,
            "Sequence": 6,
            "index": "92FA6A9FC8EA6018D5D16532D7795C91BFB0831355BDFDA177E86C8BF997985F"
        },
        "ledger_current_index": 4,
        "queue_data": {
            "auth_change_queued": true,
            "highest_sequence": 10,
            "lowest_sequence": 6,
            "max_spend_drops_total": "500",
            "transactions": [
                {
                    "auth_change": false,
                    "fee": "100",
                    "fee_level": "2560",
                    "max_spend_drops": "100",
                    "seq": 6
                },
                ... (trimmed for length) ...
                {
                    "LastLedgerSequence": 10,
                    "auth_change": true,
                    "fee": "100",
                    "fee_level": "2560",
                    "max_spend_drops": "100",
                    "seq": 10
                }
            ],
            "txn_count": 5
        },
        "validated": false
    }
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "result": {
        "account_data": {
            "Account": "rG1QQv2nh2gr7RCZ1P8YYcBUKCCN633jCn",
            "Balance": "999999999960",
            "Flags": 8388608,
            "LedgerEntryType": "AccountRoot",
            "OwnerCount": 0,
            "PreviousTxnID": "4294BEBE5B569A18C0A2702387C9B1E7146DC3A5850C1E87204951C6FDAA4C42",
            "PreviousTxnLgrSeq": 3,
            "Sequence": 6,
            "index": "92FA6A9FC8EA6018D5D16532D7795C91BFB0831355BDFDA177E86C8BF997985F"
        },
        "ledger_current_index": 4,
        "queue_data": {
            "auth_change_queued": true,
            "highest_sequence": 10,
            "lowest_sequence": 6,
            "max_spend_drops_total": "500",
            "transactions": [
                {
                    "auth_change": false,
                    "fee": "100",
                    "fee_level": "2560",
                    "max_spend_drops": "100",
                    "seq": 6
                },
                ... (trimmed for length) ...
                {
                    "LastLedgerSequence": 10,
                    "auth_change": true,
                    "fee": "100",
                    "fee_level": "2560",
                    "max_spend_drops": "100",
                    "seq": 10
                }
            ],
            "txn_count": 5
        },
        "status": "success",
        "validated": false
    }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 요청된 계정과 해당 데이터, 해당 계정이 적용되는 ledger이 다음 필드와 함께 포함됩니다:

| `Field`                | 유형      | 설명                                                                                                                                                                                                                                                                                                                                                                                  |
| ---------------------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `account_data`         | 객체      | 원장에 저장된 이 계정 정보가 포함된 AccountRoot 원장 개체입니다.                                                                                                                                                                                                                                                                                                                                          |
| `account_flags`        | 객체      | `Flags`계정 필드 에 따른 계정의 플래그 상태(아래 참조)입니다.[![새로운 기능: 잔물결 1.11.0](https://img.shields.io/badge/New%20in-rippled%201.11.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.11.0)                                                                                                                                                                                             |
| `signer_lists`         | 배열      | _(요청이 지정되지 않고 `signer_lists`하나 이상의 SignerList가 계정과 연결되어 있지 않으면 생략됩니다.)_ [다중 서명을](https://xrpl.org/multi-signing.html) 위해 이 계정과 연결된 SignerList 원장 개체 의 배열입니다. 계정은 최대 하나의 SignerList를 소유할 수 있으므로 이 배열에는 멤버가 있는 경우 정확히 하나의 멤버가 있어야 합니다.[![새로운 기능: 파문 0.31.0](https://img.shields.io/badge/New%20in-rippled%200.31.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.31.0) |
| `ledger_current_index` | 정수      | _`ledger_index`(대신 제공되는 경우 생략_) 이 정보를 검색할 때 사용된 현재 진행 중인 원장의 원장 인덱스입니다.                                                                                                                                                                                                                                                                                                             |
| `ledger_index`         | 정수      | _(`ledger_current_index`대신 제공되는 경우 생략)_ 이 정보를 검색할 때 사용된 원장 버전의 원장 인덱스입니다. 이 정보에는 이 버전보다 최신 원장 버전의 변경 사항이 포함되어 있지 않습니다.                                                                                                                                                                                                                                                              |
| `queue_data`           | 객체      | _(현재 공개 원장 `queue`으로 지정하여`true`_ 쿼리하지 않는 한 생략됩니다.) 이 계정에서 전송한 대기 중인 트랜잭션에 대한 정보입니다. 이 정보는 P2P XRP Ledger 네트워크`rippled` 의 다른 서버와 다를 수 있는 로컬 서버의 상태를 설명합니다. 대기열 메커니즘에 의해 값이 "느리게" 계산되므로 일부 필드는 생략될 수 있습니다.                                                                                                                                                                            |
| `validated`            | Boolean | 이 데이터가 검증된 원장 버전에서 나온 경우 참입니다. 생략하거나 false로 설정하면 이 데이터는 최종 데이터가 아닙니다.[![새로운 기능: 파문 0.26.0](https://img.shields.io/badge/New%20in-rippled%200.26.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.26.0)                                                                                                                                                                |

account\_flags 필드에는 다음과 같은 중첩 필드가 포함됩니다:

queue\_data 필드(있는 경우)에는 다음과 같은 중첩 필드가 포함됩니다:

| `Field`                 | 유형      | 설명                                                                                                                          |
| ----------------------- | ------- | --------------------------------------------------------------------------------------------------------------------------- |
| `txn_count`             | 정수      | 이 주소에서 대기 중인 트랜잭션 수 입니다.                                                                                                    |
| `auth_change_queued`    | Boolean | (생략될 수 있음) 대기열의 트랜잭션이 이 주소의 트랜잭션 승인 방식을 변경하는지 여부입니다. 이면 `true`이 주소는 해당 트랜잭션이 실행되거나 대기열에서 삭제될 때까지 더 이상 트랜잭션을 대기열에 넣을 수 없습니다. |
| `lowest_sequence`       | 정수      | (생략 가능) 이 주소에 대기 중인 트랜잭션 중 가장 낮은 Sequence Number입니다.                                                                        |
| `highest_sequence`      | 정수      | (생략 가능) 이 주소에 대기 중인 트랜잭션 중 가장 높은 Sequence Number입니다.                                                                        |
| `max_spend_drops_total` | 문자열     | (생략될 수 있음) 대기열의 모든 트랜잭션이 가능한 최대 XRP 금액을 소비하는 경우 이 주소에서 인출될 수 있는 XRP 드롭의 정수 금액입니다.                                           |
| `transactions`          | 배열      | (생략 가능) 이 주소에서 대기 중인 각 트랜잭션에 대한 정보입니다.                                                                                      |

queue\_data 트랜잭션 배열의 각 객체(있는 경우)에는 다음 필드 중 일부 또는 전부가 포함될 수 있습니다:

| `Field`           | 유형      | 설명                                          |
| ----------------- | ------- | ------------------------------------------- |
| `auth_change`     | Boolean | 이 거래가 이 주소의 거래 승인 방식을 변경하는지 여부입니다.          |
| `fee`             | 문자열     | 이 거래의 거래 비용 (XRP 드롭단위.)                     |
| `fee_level`       | 문자열     | 해당 거래 유형의 최소 비용 대비 해당 거래의 거래 비용(수수료 수준입니다.) |
| `max_spend_drops` | 문자열     | 이 거래가 보내거나 파괴할 수 있는 XRP의 최대 금액입니다.          |
| `seq`             | 정수      | 이 거래의 시퀀스 번호입니다.                            |

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다. 예를 들어, 요청에서 queue를 true로 지정했지만 현재 열려 있는 ledger이 아닌 ledger\_index를 지정했습니다.
* actNotFound - 요청의 계정 필드에 지정된 주소가 ledger의 계정과 일치하지 않습니다.
* lgrNotFound - ledger\_hash 또는 ledger\_index로 지정한 ledger이 존재하지 않거나, 존재하지만 서버에 없습니다.
