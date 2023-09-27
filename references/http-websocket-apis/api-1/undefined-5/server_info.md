# server\_info

server\_info 명령은 조회 중인 클리오 서버에 대한 다양한 정보를 사람이 읽을 수 있는 버전으로 클리오 서버에 요청합니다. rippled 서버의 경우, 대신 server\_info(rippled)를 참조하세요. [![New in: Clio v1.0.0](https://img.shields.io/badge/New%20in-Clio%20v1.0.0-blue.svg) ](https://github.com/XRPLF/clio/releases/tag/1.0.0)

## 요청 형식

요청 형식의 예시입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 1,
  "command": "server_info"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "server_info",
    "params": [
        {}
    ]
}
```
{% endtab %}
{% endtabs %}

요청은 매개변수를 받지 않습니다.

## 응답 형식

클라이언트가 로컬호스트를 통해 클리오 서버에 연결하면 응답에 카운터와 etl 객체가 포함됩니다. 클라이언트가 동일한 서버에 있지 않으므로 로컬호스트를 통해 연결하지 않는 경우 이러한 객체는 응답에서 생략됩니다.

클라이언트가 로컬 호스트를 통해 연결할 때 성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": 1,
    "result": {
        "info": {
            "complete_ledgers": "19499132-19977628",
            "counters": {
                "rpc": {
                    "account_objects": {
                        "started": "1",
                        "finished": "1",
                        "errored": "0",
                        "forwarded": "0",
                        "duration_us": "991"
                    },
                    "account_tx": {
                        "started": "1",
                        "finished": "1",
                        "errored": "0",
                        "forwarded": "0",
                        "duration_us": "91633"
                    },
                    "account_lines": {
                        "started": "5",
                        "finished": "5",
                        "errored": "0",
                        "forwarded": "0",
                        "duration_us": "4915159"
                    },
                    "submit_multisigned": {
                        "started": "2",
                        "finished": "2",
                        "errored": "0",
                        "forwarded": "2",
                        "duration_us": "4823"
                    },
                    "ledger_entry": {
                        "started": "1",
                        "finished": "1",
                        "errored": "0",
                        "forwarded": "0",
                        "duration_us": "17806"
                    },
                    "server_info": {
                        "started": "5",
                        "finished": "5",
                        "errored": "0",
                        "forwarded": "0",
                        "duration_us": "2375580"
                    },
                    "account_info": {
                        "started": "5",
                        "finished": "5",
                        "errored": "0",
                        "forwarded": "5",
                        "duration_us": "9256"
                    },
                    "account_currencies": {
                        "started": "4",
                        "finished": "4",
                        "errored": "0",
                        "forwarded": "0",
                        "duration_us": "517302"
                    },
                    "noripple_check": {
                        "started": "1",
                        "finished": "1",
                        "errored": "0",
                        "forwarded": "1",
                        "duration_us": "2218"
                    },
                    "tx": {
                        "started": "1",
                        "finished": "1",
                        "errored": "0",
                        "forwarded": "0",
                        "duration_us": "562"
                    },
                    "gateway_balances": {
                        "started": "6",
                        "finished": "6",
                        "errored": "0",
                        "forwarded": "0",
                        "duration_us": "1395156"
                    },
                    "channel_authorize": {
                        "started": "1",
                        "finished": "1",
                        "errored": "0",
                        "forwarded": "1",
                        "duration_us": "2017"
                    },
                    "manifest": {
                        "started": "1",
                        "finished": "1",
                        "errored": "0",
                        "forwarded": "1",
                        "duration_us": "1707"
                    },
                    "subscribe": {
                        "started": "6",
                        "finished": "6",
                        "errored": "0",
                        "forwarded": "0",
                        "duration_us": "116"
                    },
                    "random": {
                        "started": "1",
                        "finished": "1",
                        "errored": "0",
                        "forwarded": "0",
                        "duration_us": "111"
                    },
                    "ledger_data": {
                        "started": "14",
                        "finished": "3",
                        "errored": "11",
                        "forwarded": "0",
                        "duration_us": "6179145"
                    },
                    "ripple_path_find": {
                        "started": "1",
                        "finished": "1",
                        "errored": "0",
                        "forwarded": "1",
                        "duration_us": "1409563"
                    },
                    "account_channels": {
                        "started": "14",
                        "finished": "14",
                        "errored": "0",
                        "forwarded": "0",
                        "duration_us": "1062692"
                    },
                    "submit": {
                        "started": "6",
                        "finished": "6",
                        "errored": "0",
                        "forwarded": "6",
                        "duration_us": "11383"
                    },
                    "transaction_entry": {
                        "started": "8",
                        "finished": "5",
                        "errored": "3",
                        "forwarded": "0",
                        "duration_us": "494131"
                    }
                },
                "subscriptions": {
                    "ledger": 0,
                    "transactions": 0,
                    "transactions_proposed": 0,
                    "manifests": 2,
                    "validations": 2,
                    "account": 0,
                    "accounts_proposed": 0,
                    "books": 0
                }
            },
            "load_factor": 1,
            "clio_version": "0.3.0-b2",
            "validation_quorum": 8,
            "rippled_version": "1.9.1-rc1",
            "validated_ledger": {
                "age": 4,
                "hash": "4CD25FB70D45646EE5822E76E58B66D39D5AE6BA0F70491FA803DA0DA218F434",
                "seq": 19977628,
                "base_fee_xrp": 1E-5,
                "reserve_base_xrp": 1E1,
                "reserve_inc_xrp": 2E0
            }
        },
        "cache": {
            "size": 8812733,
            "is_full": true,
            "latest_ledger_seq": 19977629
        },
        "etl": {
            "etl_sources": [
                {
                    "validated_range": "19405538-19977629",
                    "is_connected": "1",
                    "ip": "52.36.182.38",
                    "ws_port": "6005",
                    "grpc_port": "50051",
                    "last_msg_age_seconds": "0"
                }
            ],
            "is_writer": true,
            "read_only": false,
            "last_publish_age_seconds": "2"
        },
        "validated": true
    },
    "status": "success",
    "type": "response",
    "warnings": [
        {
            "id": 2001,
            "message": "This is a clio server. clio only serves validated data. If you want to talk to rippled, include ledger_index:current in your request"
        },
        {
            "id": 2002,
            "message": "This server may be out of date"
        }
    ]
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
200 OK

{
    "result": {
        "info": {
            "complete_ledgers": "19499132-19977628",
            "counters": {
                "rpc": {
                    "account_objects": {
                        "started": "1",
                        "finished": "1",
                        "errored": "0",
                        "forwarded": "0",
                        "duration_us": "991"
                    },
                    "account_tx": {
                        "started": "1",
                        "finished": "1",
                        "errored": "0",
                        "forwarded": "0",
                        "duration_us": "91633"
                    },
                    "account_lines": {
                        "started": "5",
                        "finished": "5",
                        "errored": "0",
                        "forwarded": "0",
                        "duration_us": "4915159"
                    },
                    "submit_multisigned": {
                        "started": "2",
                        "finished": "2",
                        "errored": "0",
                        "forwarded": "2",
                        "duration_us": "4823"
                    },
                    "ledger_entry": {
                        "started": "1",
                        "finished": "1",
                        "errored": "0",
                        "forwarded": "0",
                        "duration_us": "17806"
                    },
                    "server_info": {
                        "started": "5",
                        "finished": "5",
                        "errored": "0",
                        "forwarded": "0",
                        "duration_us": "2375580"
                    },
                    "account_info": {
                        "started": "5",
                        "finished": "5",
                        "errored": "0",
                        "forwarded": "5",
                        "duration_us": "9256"
                    },
                    "account_currencies": {
                        "started": "4",
                        "finished": "4",
                        "errored": "0",
                        "forwarded": "0",
                        "duration_us": "517302"
                    },
                    "noripple_check": {
                        "started": "1",
                        "finished": "1",
                        "errored": "0",
                        "forwarded": "1",
                        "duration_us": "2218"
                    },
                    "tx": {
                        "started": "1",
                        "finished": "1",
                        "errored": "0",
                        "forwarded": "0",
                        "duration_us": "562"
                    },
                    "gateway_balances": {
                        "started": "6",
                        "finished": "6",
                        "errored": "0",
                        "forwarded": "0",
                        "duration_us": "1395156"
                    },
                    "channel_authorize": {
                        "started": "1",
                        "finished": "1",
                        "errored": "0",
                        "forwarded": "1",
                        "duration_us": "2017"
                    },
                    "manifest": {
                        "started": "1",
                        "finished": "1",
                        "errored": "0",
                        "forwarded": "1",
                        "duration_us": "1707"
                    },
                    "subscribe": {
                        "started": "6",
                        "finished": "6",
                        "errored": "0",
                        "forwarded": "0",
                        "duration_us": "116"
                    },
                    "random": {
                        "started": "1",
                        "finished": "1",
                        "errored": "0",
                        "forwarded": "0",
                        "duration_us": "111"
                    },
                    "ledger_data": {
                        "started": "14",
                        "finished": "3",
                        "errored": "11",
                        "forwarded": "0",
                        "duration_us": "6179145"
                    },
                    "ripple_path_find": {
                        "started": "1",
                        "finished": "1",
                        "errored": "0",
                        "forwarded": "1",
                        "duration_us": "1409563"
                    },
                    "account_channels": {
                        "started": "14",
                        "finished": "14",
                        "errored": "0",
                        "forwarded": "0",
                        "duration_us": "1062692"
                    },
                    "submit": {
                        "started": "6",
                        "finished": "6",
                        "errored": "0",
                        "forwarded": "6",
                        "duration_us": "11383"
                    },
                    "transaction_entry": {
                        "started": "8",
                        "finished": "5",
                        "errored": "3",
                        "forwarded": "0",
                        "duration_us": "494131"
                    }
                },
                "subscriptions": {
                    "ledger": 0,
                    "transactions": 0,
                    "transactions_proposed": 0,
                    "manifests": 2,
                    "validations": 2,
                    "account": 0,
                    "accounts_proposed": 0,
                    "books": 0
                }
            },
            "load_factor": 1,
            "clio_version": "0.3.0-b2",
            "validation_quorum": 8,
            "rippled_version": "1.9.1-rc1",
            "validated_ledger": {
                "age": 4,
                "hash": "4CD25FB70D45646EE5822E76E58B66D39D5AE6BA0F70491FA803DA0DA218F434",
                "seq": 19977628,
                "base_fee_xrp": 1E-5,
                "reserve_base_xrp": 1E1,
                "reserve_inc_xrp": 2E0
            }
        },
        "cache": {
            "size": 8812733,
            "is_full": true,
            "latest_ledger_seq": 19977629
        },
        "etl": {
            "etl_sources": [
                {
                    "validated_range": "19405538-19977629",
                    "is_connected": "1",
                    "ip": "52.36.182.38",
                    "ws_port": "6005",
                    "grpc_port": "50051",
                    "last_msg_age_seconds": "0"
                }
            ],
            "is_writer": true,
            "read_only": false,
            "last_publish_age_seconds": "2"
        },
        "validated": true,
    },
    "status": "success",
    "type": "response",
    "warnings": [
        {
            "id": 2001,
            "message": "This is a clio server. clio only serves validated data. If you want to talk to rippled, include ledger_index:current in your request"
        },
        {
            "id": 2002,
            "message": "This server may be out of date"
        }
    ]
}
```
{% endtab %}
{% endtabs %}

클라이언트가 로컬 호스트를 통해 연결하지 않을 때 성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": 1,
    "result": {
        "info": {
            "complete_ledgers":"32570-73737719",
            "load_factor":1,
            "clio_version":"1.0.2",
            "validation_quorum":28,
            "rippled_version":"1.9.1",
            "validated_ledger": {
                "age":7,
                "hash":"4ECDEAF9E6F8B37EFDE297953168AAB42DEED1082A565639EBB2D29E047341B4",
                "seq":73737719,
                "base_fee_xrp":1E-5,
                "reserve_base_xrp":1E1,
                "reserve_inc_xrp":2E0
            },
            "cache": {
                "size":15258947,
                "is_full":true,
                "latest_ledger_seq":73737719
            }
        },
        "validated":true,
        "status":"success"
    },
    "warnings": [
        {
            "id":2001,
            "message":"This is a clio server. clio only serves validated data. If you want to talk to rippled, include 'ledger_index':'current' in your request"
        }
    ]
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
200 OK

{
    "result": {
        "info": {
            "complete_ledgers":"32570-73737719",
            "load_factor":1,
            "clio_version":"1.0.2",
            "validation_quorum":28,
            "rippled_version":"1.9.1",
            "validated_ledger": {
                "age":7,
                "hash":"4ECDEAF9E6F8B37EFDE297953168AAB42DEED1082A565639EBB2D29E047341B4",
                "seq":73737719,
                "base_fee_xrp":1E-5,
                "reserve_base_xrp":1E1,
                "reserve_inc_xrp":2E0
            },
            "cache": {
                "size":15258947,
                "is_full":true,
                "latest_ledger_seq":73737719
            }
        },
        "validated":true,
        "status":"success"
    },
    "warnings": [
        {
            "id":2001,
            "message":"This is a clio server. clio only serves validated data. If you want to talk to rippled, include 'ledger_index':'current' in your request"
        }
    ]
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 정보 객체가 유일한 필드로 포함됩니다.

정보 객체에는 다음과 같은 필드가 배열되어 있을 수 있습니다:

| Field                                    | 유형      | 설명                                                                                                                                                                                                                                |
| ---------------------------------------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| complete\_ledgers                        | 문자열     | 로컬 rippled이 데이터베이스에 가지고 있는 원장 버전의 시퀀스 번호를 나타내는 범위 표현식입니다. 24900901-24900984,24901116-24901158과 같은 불연속적인 시퀀스일 수 있습니다. 서버에 완전한 원장이 없는 경우(예: 최근에 네트워크와 동기화를 시작한 경우) 이 문자열은 비어 있습니다.                                                  |
| counters                                 | 객체      | (생략가능) 서버 시작 이후 처리된 API 호출에 대한 통계입니다. 클라이언트가 로컬호스트를 통해 클리오 서버에 연결한 경우에만 존재합니다.                                                                                                                                                    |
| rpc                                      | 객체      | _(생략가능)_ 시작 이후 클리오 서버가 처리한 각 API 호출에 대한 통계입니다. 카운터 객체 내에 중첩되어 있으므로 클라이언트가 로컬호스트를 통해 클리오 서버에 연결한 경우에만 존재합니다.                                                                                                                       |
| rpc.\*.started                           | 숫자      | 클리오 서버가 시작 이후 처리를 시작한 이 유형의 API 호출 수입니다.                                                                                                                                                                                          |
| rpc.\*.finished                          | 숫자      | 클리오 서버가 시작 이후 처리를 완료한 이 유형의 API 호출 수입니다.                                                                                                                                                                                          |
| rpc.\*.errored                           | 숫자      | 시작 이후 어떤 종류의 오류를 초래한 이 유형의 API 호출 수입니다.                                                                                                                                                                                           |
| rpc.\*.forwarded                         | 숫자      | 클리오 서버가 rippled P2P 서버로 전달한 이 유형의 API 호출 수입니다.                                                                                                                                                                                    |
| rpc.\*.duration\_us                      | 숫자      | 시작 이후 이 유형의 API 호출을 처리하는 데 소요된 총 마이크로초 수입니다.                                                                                                                                                                                      |
| subscriptions                            | 객체      | (생략 가능) 각 스트림 유형에 대한 현재 구독자 수입니다. 카운터 객체 내에 중첩되어 있으므로 클라이언트가 로컬호스트를 통해 클리오 서버에 연결한 경우에만 존재합니다.                                                                                                                                    |
| subscriptions.ledger                     |         |                                                                                                                                                                                                                                   |
| subscriptions.transactions               |         |                                                                                                                                                                                                                                   |
| subscriptions.transactions\_proposed     |         |                                                                                                                                                                                                                                   |
| subscriptions.manifests                  |         |                                                                                                                                                                                                                                   |
| subscriptions.validations                |         |                                                                                                                                                                                                                                   |
| subscriptions.account                    |         |                                                                                                                                                                                                                                   |
| subscriptions.accounts\_proposed         |         |                                                                                                                                                                                                                                   |
| subscriptions.books                      |         |                                                                                                                                                                                                                                   |
| load\_factor                             | 숫자      | 서버가 현재 적용하고 있는 로드 스케일 공개 원장 트랜잭션 비용으로, 기본 트랜잭션 비용에 승수를 곱한 값입니다. 예를 들어, 부하 계수가 1000이고 기준 트랜잭션 비용이 10드롭인 경우, 부하 확장 트랜잭션 비용은 10,000드롭(0.01XRP)이 됩니다. 로드 팩터는 개별 서버의 로드 팩터, 클러스터의 로드 팩터, 공개 원장 비용, 전체 네트워크의 로드 팩터 중 가장 높은 값에 의해 결정됩니다. |
| clio\_version                            | 문자열     | 실행 중인 클리오 서버의 버전 번호입니다.                                                                                                                                                                                                           |
| validation\_quorum                       | 숫자      | (생략 가능) 원장 버전을 검증하는 데 필요한 신뢰할 수 있는 검증의 최소 수입니다. 상황에 따라 서버에 더 많은 유효성 검사가 필요할 수 있습니다. 이 값은 rippled에서 가져옵니다. 클리오 서버가 어떤 이유로 rippled에 연결할 수 없는 경우 이 필드는 응답에서 생략될 수 있습니다.                                                              |
| rippled\_version                         | 문자열     | _(생략 가능)_ 클리오 서버가 연결되어 있는 실행 중인 리플드 서버의 버전 번호입니다. 클리오 서버가 어떤 이유로 rippled에 연결할 수 없는 경우 이 필드는 응답에서 생략될 수 있습니다.                                                                                                                      |
| validated\_ledger                        | 객체      | (생략 가능) 가장 최근에 완전히 검증된 원장에 대한 정보입니다. 가장 최근의 유효성 검사된 원장을 사용할 수 없는 경우, 응답은 이 필드를 생략하고 대신 closed\_ledger를 포함합니다.                                                                                                                     |
| validated\_ledger.age                    | 숫자      | 원장이 폐쇄된 이후 시간(초)입니다.                                                                                                                                                                                                              |
| validated\_ledger.base\_fee\_xrp         | 숫자      | 기본 수수료(XRP). 0.00001의 경우 1e-05와 같은 과학적 표기법으로 표시할 수 있습니다.                                                                                                                                                                          |
| validated\_ledger.hash                   | 문자열     | 원장의 고유 해시(16진수)입니다.                                                                                                                                                                                                               |
| validated\_ledger.reserve\_base\_xrp     | 숫자      | 모든 계정이 예비로 보유하는 데 필요한 최소 XRP(드롭이 아님) 수량입니다. 0.00001의 경우 1e-05와 같은 과학적 표기법으로 표시할 수 있습니다.                                                                                                                                           |
| validated\_ledger.reserve\_inc\_xrp      | 숫자      | 계정이 원장에서 소유하고 있는 각 개체에 대해 계정 준비금에 추가되는 XRP(드롭 제외)의 양입니다. 0.00001의 경우 1e-05와 같은 과학적 표기법으로 표시할 수 있습니다.                                                                                                                              |
| validated\_ledger.seq                    | 숫자      | 가장 최근에 유효성이 검증된 원장의 원장 인덱스입니다.                                                                                                                                                                                                    |
| validator\_list\_expires                 | 문자열     | (관리자 전용) 현재 유효성 검사기 목록이 만료되는 UTC 기준 사람이 읽을 수 있는 시간, 서버가 아직 게시된 유효성 검사기 목록을 로드하지 않은 경우 알 수 없음 문자열 또는 서버가 정적 유효성 검사기 목록을 사용하는 경우 없음 문자열입니다.                                                                                         |
| cache                                    | 객체      | 클리오의 상태 데이터 캐시에 대한 오브젝트 정보입니다.                                                                                                                                                                                                    |
| cache.size                               | 숫자      | 현재 캐시에 있는 상태 데이터 오브젝트 수입니다.                                                                                                                                                                                                       |
| cache.is\_full                           | Boolean | 캐시에 특정 원장에 대한 모든 상태 데이터가 포함되어 있으면 참, 그렇지 않으면 거짓입니다. book\_offers 메서드와 같은 일부 API 호출은 캐시가 가득 차면 훨씬 빠르게 처리됩니다.                                                                                                                       |
| cache.latest\_ledger\_seq                | 숫자      | 캐시에 저장된 가장 최근에 유효성이 검증된 원장의 원장 인덱스입니다.                                                                                                                                                                                            |
| etl                                      | 객체      | 객체 클리오 서버가 연결된 rippled 소스(ETL 소스)입니다. 클라이언트가 로컬호스트를 통해 클리오 서버에 연결한 경우에만 존재합니다.                                                                                                                                                    |
| etl.etl\_sources                         | 객체 배열   | 클리오 서버가 연결되어 데이터를 추출하는 리플리드 소스(ETL 소스)를 나열합니다.                                                                                                                                                                                    |
| etl.etl\_sources.validated\_range        | 문자열     | P2P rippled 서버에서 검색한 유효성 검사된 원장 범위 문자열입니다.                                                                                                                                                                                        |
| etl.etl\_sources.is\_connected           | Boolean | 클리오가 웹소켓을 통해 이 소스에 연결되면 참, 그렇지 않으면 거짓입니다. 여기서 값이 거짓이면 네트워크 문제가 있거나 리플이 실행되고 있지 않음을 나타낼 수 있습니다.                                                                                                                                    |
| etl.etl\_sources.ip                      | 숫자      | rippled 서버의 IP 번호입니다.                                                                                                                                                                                                             |
| etl.etl\_sources.ws\_port                | 숫자      | rippled 서버의 웹소켓 포트 번호입니다.                                                                                                                                                                                                         |
| etl.etl\_sources.grpc\_port              | 숫자      | 클리오 서버가 연결된 P2P rippled 서버의 gRPC 연결 포트입니다.                                                                                                                                                                                        |
| etl.etl\_sources.last\_msg\_age\_seconds | 숫자      | 클리오가 마지막으로 리플드된 메시지를 수신한 후 경과한 총 시간(초)입니다. 이 값은 8보다 크지 않아야 합니다.                                                                                                                                                                   |
| etl.is\_writer                           | Boolean | 이 클리오 서버가 현재 데이터베이스에 데이터를 쓰고 있으면 참, 그렇지 않으면 거짓입니다.                                                                                                                                                                                |
| etl.read\_only                           | Boolean | 이 클리오 서버가 읽기 전용 모드로 구성되어 있으면 참, 그렇지 않으면 거짓입니다.                                                                                                                                                                                    |
| etl.last\_publish\_age\_seconds          | 숫자      | 클리오 서버가 마지막으로 원장을 게시한 후 경과한 시간(초)입니다. 이 값은 8을 넘지 않아야 합니다.                                                                                                                                                                         |
| validated                                | Boolean | 참이면 응답이 합의에 의해 유효성이 검증된 원장 버전을 사용함을 나타냅니다. Clio는 검증된 원장 데이터를 저장하고 반환하므로 항상 참입니다. 요청이 리플에 전달되었고 서버가 현재 데이터를 반환하는 경우, 누락 또는 거짓 값은 이 원장의 데이터가 최종 데이터가 아님을 나타냅니다.                                                                     |
| status                                   | 문자열     | API 요청의 상태를 반환합니다. 요청이 성공적으로 완료되면 성공입니다.                                                                                                                                                                                          |

### 발생 가능한 오류

* 일반적인 오류 유형입니다.
