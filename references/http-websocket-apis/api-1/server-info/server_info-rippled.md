# server\_info (rippled)

server\_info 명령은 쿼리 중인 rippled 서버에 대한 다양한 정보를 사람이 읽을 수 있는 버전으로 서버에 요청합니다. 클리오 서버의 경우, 대신 server\_info(클리오)를 참조하세요.

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

{% tab title="Commandline" %}
```
#Syntax: server_info
rippled server_info
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
  "id": 1,
  "result": {
    "info": {
      "build_version": "1.9.4",
      "complete_ledgers": "32570-75801736",
      "hostid": "ARMY",
      "initial_sync_duration_us": "273518294",
      "io_latency_ms": 1,
      "jq_trans_overflow": "2282",
      "last_close": {
        "converge_time_s": 3.002,
        "proposers": 35
      },
      "load_factor": 1,
      "network_id": 0,
      "peer_disconnects": "3194",
      "peer_disconnects_resources": "3",
      "peers": 20,
      "pubkey_node": "n9KKBZvwPZ95rQi4BP3an1MRctTyavYkZiLpQwasmFYTE6RYdeX3",
      "server_state": "full",
      "server_state_duration_us": "69205850392",
      "state_accounting": {
        "connected": {
          "duration_us": "141058919",
          "transitions": "7"
        },
        "disconnected": {
          "duration_us": "514136273",
          "transitions": "3"
        },
        "full": {
          "duration_us": "4360230140761",
          "transitions": "32"
        },
        "syncing": {
          "duration_us": "50606510",
          "transitions": "30"
        },
        "tracking": {
          "duration_us": "40245486",
          "transitions": "34"
        }
      },
      "time": "2022-Nov-16 21:50:22.711679 UTC",
      "uptime": 4360976,
      "validated_ledger": {
        "age": 1,
        "base_fee_xrp": 0.00001,
        "hash": "3147A41F5F013209581FCDCBBB7A87A4F01EF6842963E13B2B14C8565E00A22B",
        "reserve_base_xrp": 10,
        "reserve_inc_xrp": 2,
        "seq": 75801736
      },
      "validation_quorum": 28
    }
  },
  "status": "success",
  "type": "response"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
200 OK

{
  "result": {
    "info": {
      "build_version": "1.9.4",
      "complete_ledgers": "32570-75801747",
      "hostid": "ABET",
      "initial_sync_duration_us": "566800928",
      "io_latency_ms": 1,
      "jq_trans_overflow": "47035",
      "last_close": {
        "converge_time_s": 3,
        "proposers": 35
      },
      "load_factor": 1,
      "network_id": 0,
      "peer_disconnects": "542",
      "peer_disconnects_resources": "1",
      "peers": 24,
      "pubkey_node": "n9LvjJyaYHBWUvQUat632RrnpS7UHVLW2tLUGXSZ2yXouh4goDHX",
      "server_state": "full",
      "server_state_duration_us": "3612238603",
      "state_accounting": {
        "connected": {
          "duration_us": "125498126",
          "transitions": "67"
        },
        "disconnected": {
          "duration_us": "473415516",
          "transitions": "2"
        },
        "full": {
          "duration_us": "4365855930299",
          "transitions": "337"
        },
        "syncing": {
          "duration_us": "1383837914",
          "transitions": "311"
        },
        "tracking": {
          "duration_us": "518995710",
          "transitions": "374"
        }
      },
      "time": "2022-Nov-16 21:51:03.737667 UTC",
      "uptime": 4368357,
      "validated_ledger": {
        "age": 1,
        "base_fee_xrp": 0.00001,
        "hash": "D54A94D5EF620DC212EAB5958D592EC641FC94ED92146477A04DCE5B006DFF05",
        "reserve_base_xrp": 10,
        "reserve_inc_xrp": 2,
        "seq": 75801747
      },
      "validation_quorum": 28
    },
    "status": "success"
  }
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
Loading: "/etc/opt/ripple/rippled.cfg"
2020-Mar-24 01:28:22.288484766 UTC HTTPClient:NFO Connecting to 127.0.0.1:5005

{
  "result": {
    "info": {
      "build_version": "1.9.4",
      "complete_ledgers": "32570-75801747",
      "hostid": "ABET",
      "initial_sync_duration_us": "566800928",
      "io_latency_ms": 1,
      "jq_trans_overflow": "47035",
      "last_close": {
        "converge_time_s": 3,
        "proposers": 35
      },
      "load_factor": 1,
      "network_id": 0,
      "peer_disconnects": "542",
      "peer_disconnects_resources": "1",
      "peers": 24,
      "pubkey_node": "n9LvjJyaYHBWUvQUat632RrnpS7UHVLW2tLUGXSZ2yXouh4goDHX",
      "server_state": "full",
      "server_state_duration_us": "3612238603",
      "state_accounting": {
        "connected": {
          "duration_us": "125498126",
          "transitions": "67"
        },
        "disconnected": {
          "duration_us": "473415516",
          "transitions": "2"
        },
        "full": {
          "duration_us": "4365855930299",
          "transitions": "337"
        },
        "syncing": {
          "duration_us": "1383837914",
          "transitions": "311"
        },
        "tracking": {
          "duration_us": "518995710",
          "transitions": "374"
        }
      },
      "time": "2022-Nov-16 21:51:03.737667 UTC",
      "uptime": 4368357,
      "validated_ledger": {
        "age": 1,
        "base_fee_xrp": 0.00001,
        "hash": "D54A94D5EF620DC212EAB5958D592EC641FC94ED92146477A04DCE5B006DFF05",
        "reserve_base_xrp": 10,
        "reserve_inc_xrp": 2,
        "seq": 75801747
      },
      "validation_quorum": 28
    },
    "status": "success"
  }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 정보 객체가 유일한 필드로 포함됩니다.

정보 객체에는 다음 필드가 일부 배열되어 있을 수 있습니다:

| `Field`                             | 유형       | 설명                                                                                                                                                                                                                                                                                                                                                                               |
| ----------------------------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `amendment_blocked`                 | Boolean  | (생략 가능) 참이면 이 서버는 수정이 차단된 서버입니다. 서버가 수정이 차단되지 않은 경우 응답은 이 필드를 생략합니다.[![새로운 기능: Rippled 0.80.0](https://img.shields.io/badge/New%20in-rippled%200.80.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.80.0)                                                                                                                                                         |
| `build_version`                     | 문자열      | 실행 중인 rippled 서버의 버전 번호입니다.                                                                                                                                                                                                                                                                                                                                                      |
| `closed_ledger`                     | 객체       | (생략 가능) 합의에 의해 검증되지 않은 가장 최근에 폐쇄된 원장에 대한 정보입니다. 가장 최근에 유효성이 검증된 원장을 사용할 수 있는 경우, 응답은 이 필드를 생략하고 대신 validated\_ledger를 포함합니다. 멤버 필드는 validated\_ledger 필드와 동일합니다.                                                                                                                                                                                                                 |
| `complete_ledgers`                  | 문자열      | 로컬 rippled가 데이터베이스에 가지고 있는 원장 버전의 시퀀스 번호를 나타내는 문자열 범위 표현식입니다. 24900901-24900984,24901116-24901158과 같이 분리되지 않은 시퀀스일 수 있습니다. 서버에 완전한 원장이 없는 경우(예: 최근에 네트워크와 동기화를 시작한 경우) 이 문자열은 비어 있습니다.                                                                                                                                                                                           |
| `hostid`                            | 문자열      | 관리자 요청 시 rippled 인스턴스를 실행하는 서버의 호스트 이름을 반환하고, 그렇지 않으면 노드 공개키를 기반으로 한 단일 RFC-1751 단어를 반환합니다.                                                                                                                                                                                                                                                                                      |
| `io_latency_ms`                     | 숫자       | I/O 작업을 기다리는 데 소요된 시간(밀리초)입니다. 이 수치가 매우 낮지 않으면 rippled 서버에 심각한 부하 문제가 있을 가능성이 높습니다.                                                                                                                                                                                                                                                                                              |
| `jq_trans_overflow`                 | 문자열 - 숫자 | 이 서버가 한 번에 250개 이상의 트랜잭션을 처리하기 위해 대기 중인 횟수(시작 이후)입니다. 숫자가 크면 서버가 XRP 레저 네트워크의 트랜잭션 부하를 처리할 수 없음을 의미할 수 있습니다. 미래 대비 서버 사양에 대한 자세한 권장 사항은 용량 계획을 참조하세요.[![새로운 기능: Rippled 0.90.0](https://img.shields.io/badge/New%20in-rippled%200.90.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.90.0)                                                                        |
| `last_close`                        | 객체       | 합의에 도달하는 데 걸린 시간과 참여한 신뢰할 수 있는 검증자 수를 포함해 서버가 마지막으로 원장을 닫은 시간에 대한 정보입니다.                                                                                                                                                                                                                                                                                                         |
| `last_close.converge_time_s`        | 숫자       | 가장 최근에 검증한 원장 버전에서 합의에 도달하는 데 걸린 시간(초)입니다.                                                                                                                                                                                                                                                                                                                                       |
| `last_close.proposers`              | 숫자       | 가장 최근에 검증한 원장 버전에 대한 합의 프로세스에서 서버가 고려한 신뢰할 수 있는 검증자 수(검증자로 구성된 경우 자신 포함).                                                                                                                                                                                                                                                                                                        |
| `load`                              | 객체       | (관리자만 해당) 서버의 현재 로드 상태에 대한 자세한 정보입니다.                                                                                                                                                                                                                                                                                                                                            |
| `load.job_types`                    | 배열       | (관리자만 해당) 서버가 수행 중인 다양한 유형의 작업 비율과 각 작업에 소요되는 시간에 대한 정보입니다.                                                                                                                                                                                                                                                                                                                      |
| `load.threads`                      | 숫자       | (관리자만 해당) 서버의 기본 작업 풀에 있는 스레드 수입니다.                                                                                                                                                                                                                                                                                                                                              |
| `load_factor`                       | 숫자       | 기본 트랜잭션 비용에 대한 승수로서 서버가 현재 적용 중인 로드 스케일 오픈 레저 트랜잭션 비용입니다. 예를 들어 로드 계수가 1000이고 기준 트랜잭션 비용이 10드롭의 XRP인 경우, 로드 스케일 트랜잭션 비용은 10,000드롭(0.01 XRP)입니다. 로드 팩터는 개별 서버의 로드 팩터, 클러스터의 로드 팩터, 공개 원장 비용, 전체 네트워크의 로드 팩터 중 가장 높은 값에 의해 결정됩니다.[![업데이트: 파문 0.33.0](https://img.shields.io/badge/Updated%20in-rippled%200.33.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.33.0) |
| `load_factor_local`                 | 숫자       | (생략 가능) 이 서버에 대한 로드를 기준으로 한 트랜잭션 비용에 대한 현재 승수입니다.                                                                                                                                                                                                                                                                                                                                |
| `load_factor_net`                   | 숫자       | (생략 가능) 나머지 네트워크에서 사용 중인 트랜잭션 비용에 대한 현재 승수 (다른 서버의 보고된 로드 값에서 추정).                                                                                                                                                                                                                                                                                                               |
| `load_factor_cluster`               | 숫자       | (생략 가능) 이 클러스터 내 서버에 대한 부하를 기준으로 한 트랜잭션 비용의 현재 승수입니다.                                                                                                                                                                                                                                                                                                                            |
| `load_factor_fee_escalation`        | 숫자       | (생략 가능) 트랜잭션이 오픈 원장에 들어가기 위해 지불해야 하는 트랜잭션 비용에 대한 현재 승수입니다.[![새로운 기능: 파문 0.32.0](https://img.shields.io/badge/New%20in-rippled%200.32.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.32.0)                                                                                                                                                                        |
| `load_factor_fee_queue`             | 숫자       | (생략 가능) 대기열이 가득 찬 경우 트랜잭션이 대기열에 들어가기 위해 지불해야 하는 트랜잭션 비용에 대한 현재 승수입니다.[![새로운 기능: 파문 0.32.0](https://img.shields.io/badge/New%20in-rippled%200.32.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.32.0)                                                                                                                                                             |
| `load_factor_server`                | 숫자       | (생략 가능) 서버가 적용하는 부하율로, 공개 원장 비용은 포함하지 않습니다.[![새로운 기능: 파문 0.33.0](https://img.shields.io/badge/New%20in-rippled%200.33.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.33.0)                                                                                                                                                                                       |
| `peers`                             | 숫자       | (리포팅 모드 서버에서는 생략) 이 서버가 현재 연결된 다른 rippled 서버의 수입니다.                                                                                                                                                                                                                                                                                                                              |
| `pubkey_node`                       | 문자열      | P2P 통신을 위해 이 서버를 확인하는 데 사용되는 공개 키입니다. 이 노드 키 쌍은 서버를 처음 시작할 때 서버가 자동으로 생성합니다. (삭제하면 서버가 새 키 쌍을 생성할 수 있습니다.) 클러스터링에 유용한 \[node\_seed] 구성 옵션을 사용하여 구성 파일에서 영구값을 설정할 수 있습니다.                                                                                                                                                                                                         |
| `pubkey_validator`                  | 문자열      | _(관리자 전용)_ 이 노드가 원장 유효성 검사에 서명하는 데 사용하는 공개 키입니다. 이 유효성 검사 키 쌍은 \[validator\_token] 또는 \[validation\_seed] 구성 필드에서 파생됩니다.                                                                                                                                                                                                                                                         |
| `reporting`                         | 객체       | (리포팅 모드 서버만 해당) 이 서버의 보고 모드별 구성에 대한 정보입니다.                                                                                                                                                                                                                                                                                                                                       |
| `reporting.etl_sources`             | 배열       | (리포팅 모드 서버만 해당) 이 리포팅 모드에서 데이터를 검색하는 P2P 모드 서버의 목록입니다. 이 배열의 각 항목은 ETL 소스 객체입니다.                                                                                                                                                                                                                                                                                                 |
| `reporting.is_writer`               | Boolean  | (리포팅 모드 서버만 해당) 참이면 이 서버가 외부 데이터베이스에 원장 데이터를 쓰고 있습니다. 거짓이면 다른 보고 모드 서버가 현재 공유 데이터베이스를 채우고 있거나 읽기 전용으로 구성되어 있기 때문에 현재 쓰고 있지 않습니다.                                                                                                                                                                                                                                                 |
| `reporting.last_publish_time`       | 문자열      | (리포팅 모드 서버만 해당) 이 서버가 마지막으로 구독 스트림에 유효성이 검사된 원장을 게시한 시점을 나타내는 ISO 8601 타임스탬프입니다.                                                                                                                                                                                                                                                                                                 |
| `server_state`                      | 문자열      | 서버가 네트워크에 어느 정도 참여하고 있는지를 나타내는 문자열입니다. 자세한 내용은 가능한 서버 상태를 참조하세요.                                                                                                                                                                                                                                                                                                                 |
| `server_state_duration_us`          | 숫자       | 서버가 현재 상태에 있었던 연속 마이크로초 수입니다.[![새로운 기능: Rippled 1.2.0](https://img.shields.io/badge/New%20in-rippled%201.2.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.2.0)                                                                                                                                                                                                   |
| `state_accounting`                  | 객체       | 서버가 각 상태에서 보낸 시간에 대한 정보가 포함된 다양한 서버 상태의 맵입니다. 서버의 네트워크 연결 상태를 장기적으로 추적하는 데 유용할 수 있습니다.[![새로운 기능: 파문 0.30.1](https://img.shields.io/badge/New%20in-rippled%200.30.1-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.30.1)                                                                                                                                            |
| `state_accounting.*.duration_us`    | 문자열      | 서버가 이 상태에서 보낸 시간(마이크로초)입니다. (서버가 다른 상태로 전환될 때마다 업데이트됩니다.)[![새로운 기능: 파문 0.30.1](https://img.shields.io/badge/New%20in-rippled%200.30.1-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.30.1)                                                                                                                                                                         |
| `state_accounting.*.transitions`    | 문자열      | 서버가 이 상태로 변경된 횟수입니다.[![새로운 기능: 파문 0.30.1](https://img.shields.io/badge/New%20in-rippled%200.30.1-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.30.1)                                                                                                                                                                                                              |
| `time`                              | 문자열      | 서버의 시계에 따른 UTC 기준 현재 시간입니다.[![업데이트: 파문 1.5.0](https://img.shields.io/badge/Updated%20in-rippled%201.5.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.5.0)                                                                                                                                                                                                        |
| `uptime`                            | 숫자       | 서버가 작동한 연속 시간(초) 수입니다.[![새로운 기능: 파문 0.30.1](https://img.shields.io/badge/New%20in-rippled%200.30.1-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.30.1)                                                                                                                                                                                                            |
| `validated_ledger`                  | 객체       | (생략 가능) 가장 최근에 완전히 검증된 원장에 대한 정보입니다. 가장 최근에 유효성이 검증된 원장을 사용할 수 없는 경우, 응답은 이 필드를 생략하고 대신 closed\_ledger를 포함합니다.                                                                                                                                                                                                                                                                   |
| `validated_ledger.age`              | 숫자       | 원장이 폐쇄된 이후 시간(초)입니다.                                                                                                                                                                                                                                                                                                                                                             |
| `validated_ledger.base_fee_xrp`     | 숫자       | 기본 수수료(XRP). 0.00001의 경우 1e-05와 같은 과학적 표기법으로 표시할 수 있습니다.                                                                                                                                                                                                                                                                                                                         |
| `validated_ledger.hash`             | 문자열      | 원장의 고유 해시(16진수)입니다.                                                                                                                                                                                                                                                                                                                                                              |
| `validated_ledger.reserve_base_xrp` | 숫자       | 모든 계정이 준비금으로 보유하는 데 필요한 최소 XRP(드롭이 아님) 수량입니다.                                                                                                                                                                                                                                                                                                                                    |
| `validated_ledger.reserve_inc_xrp`  | 숫자       | 원장에서 소유하고 있는 각 객체에 대해 계정 준비금에 추가되는 XRP(드롭이 아님) 수량입니다.                                                                                                                                                                                                                                                                                                                            |
| `validated_ledger.seq`              | 숫자       | 가장 최근에 유효성이 검증된 원장의 원장 인덱스입니다.                                                                                                                                                                                                                                                                                                                                                   |
| `validation_quorum`                 | 숫자       | 원장 버전을 검증하는 데 필요한 신뢰할 수 있는 검증의 최소 횟수입니다. 상황에 따라 서버가 더 많은 유효성 검사를 요구할 수 있습니다.                                                                                                                                                                                                                                                                                                     |
| `validator_list_expires`            | 문자열      | (관리자 전용) 현재 유효성 검사기 목록이 만료되는 UTC 기준 사람이 읽을 수 있는 시간, 서버가 아직 게시된 유효성 검사기 목록을 로드하지 않은 경우 알 수 없는 문자열 또는 서버가 정적 유효성 검사기 목록을 사용하는 경우 절대 문자열입니다. [![업데이트: 파문 1.5.0](https://img.shields.io/badge/Updated%20in-rippled%201.5.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.5.0)                                                                                         |

{% hint style="info" %}
Note:

closed\_ledger 필드가 존재하고 seq 값이 작은 경우(8자리 미만), 이는 rippled가 현재 P2P 네트워크에서 유효성이 검증된 ledger의 사본을 가지고 있지 않음을 나타냅니다. 이는 서버가 아직 동기화 중이라는 의미일 수 있습니다. 일반적으로 네트워크와 동기화하는 데는 연결 속도와 하드웨어 사양에 따라 약 5분 정도 소요됩니다.
{% endhint %}

## ETL 소스 객체

보고 모드 서버에서 etl\_sources 필드의 각 멤버는 다음 필드를 가진 객체입니다:

| 필드                          | 유형      | 설명                                                                                                                      |
| --------------------------- | ------- | ----------------------------------------------------------------------------------------------------------------------- |
| `connected`                 | Boolean | true이면 리포팅 모드 서버가 이 P2P 모드 서버에 연결됩니다. false이면 서버가 연결되지 않은 것입니다. 이는 구성이 잘못되었거나 네트워크가 중단되었거나 P2P 모드 서버가 다운되었기 때문일 수 있습니다. |
| `grpc_port`                 | 문자열     | 이 리포팅 모드 서버가 gRPC를 통해 연결하고 원장 데이터를 검색하도록 구성된 P2P 모드 서버의 포트 번호입니다.                                                       |
| `ip`                        | 문자열     | P2P 모드 서버의 IP 주소(IPv4 또는 IPv6)입니다.                                                                                      |
| `last_message_arrival_time` | 문자열     | 리포팅 모드 서버가 이 P2P 서버로부터 메시지를 수신한 가장 최근 시간을 나타내는 ISO 8601 타임스탬프입니다.                                                       |
| `validated_ledgers_range`   | 문자열     | 이 P2P 모드 서버가 사용 가능하다고 보고하는 유효성 검사된 원장 버전의 범위로, complete\_ledgers와 동일한 형식입니다.                                            |
| `websocket_port`            | 문자열     | 리포팅 모드에서 직접 제공할 수 없는 웹소켓 요청을 전달하도록 이 보고 모드 서버가 구성되어 있는 P2P 서버의 포트 번호입니다.                                                |

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
