# server\_state

server\_state 명령은 rippled 서버의 현재 상태에 대해 기계가 읽을 수 있는 다양한 정보를 서버에 요청합니다. 응답은 server\_info 메소드와 거의 동일하지만 읽기 쉬운 단위 대신 처리하기 쉬운 단위를 사용합니다. (예를 들어, XRP 값은 과학적 표기법이나 십진수 값 대신 정수 방울로 제공되며, 시간은 초 대신 밀리초로 제공됩니다.)

## 요청 형식

요청 형식의 예시입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 2,
  "command": "server_state"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "server_state",
    "params": [
        {}
    ]
}
```
{% endtab %}

{% tab title="Commandline" %}
```
#Syntax: server_state
rippled server_state
```
{% endtab %}
{% endtabs %}

요청은 매개변수를 받지 않습니다.

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 1,
  "result": {
    "state": {
      "build_version": "1.7.2",
      "complete_ledgers": "64572720-65887201",
      "io_latency_ms": 1,
      "jq_trans_overflow": "0",
      "last_close": {
        "converge_time": 3005,
        "proposers": 41
      },
      "load_base": 256,
      "load_factor": 256,
      "load_factor_fee_escalation": 256,
      "load_factor_fee_queue": 256,
      "load_factor_fee_reference": 256,
      "load_factor_server": 256,
      "peer_disconnects": "365006",
      "peer_disconnects_resources": "336",
      "peers": 216,
      "pubkey_node": "n9MozjnGB3tpULewtTsVtuudg5JqYFyV3QFdAtVLzJaxHcBaxuXD",
      "server_state": "full",
      "server_state_duration_us": "3588969453592",
      "state_accounting": {
        "connected": {
          "duration_us": "301410595",
          "transitions": "2"
        },
        "disconnected": {
          "duration_us": "1207534",
          "transitions": "2"
        },
        "full": {
          "duration_us": "3589171798767",
          "transitions": "2"
        },
        "syncing": {
          "duration_us": "6182323",
          "transitions": "2"
        },
        "tracking": {
          "duration_us": "43",
          "transitions": "2"
        }
      },
      "time": "2021-Aug-24 20:44:43.466048 UTC",
      "uptime": 3589480,
      "validated_ledger": {
        "base_fee": 10,
        "close_time": 683153081,
        "hash": "B52AC3876412A152FE9C0442801E685D148D05448D0238587DBA256330A98FD3",
        "reserve_base": 20000000,
        "reserve_inc": 5000000,
        "seq": 65887201
      },
      "validation_quorum": 33
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

Headers

{
  "result": {
    "state": {
      "build_version": "1.7.2",
      "complete_ledgers": "65844785-65887184",
      "io_latency_ms": 3,
      "jq_trans_overflow": "580",
      "last_close": {
        "converge_time": 3012,
        "proposers": 41
      },
      "load_base": 256,
      "load_factor": 134022,
      "load_factor_fee_escalation": 134022,
      "load_factor_fee_queue": 256,
      "load_factor_fee_reference": 256,
      "load_factor_server": 256,
      "peer_disconnects": "792367",
      "peer_disconnects_resources": "7273",
      "peers": 72,
      "pubkey_node": "n9LNvsFiYfFf8va6pma2PHGJKVLSyZweN1iBAkJQSeHw4GjM8gvN",
      "server_state": "full",
      "server_state_duration_us": "422128665555",
      "state_accounting": {
        "connected": {
          "duration_us": "172799714",
          "transitions": "1"
        },
        "disconnected": {
          "duration_us": "309059",
          "transitions": "1"
        },
        "full": {
          "duration_us": "6020429212246",
          "transitions": "143"
        },
        "syncing": {
          "duration_us": "413813232",
          "transitions": "152"
        },
        "tracking": {
          "duration_us": "266553605",
          "transitions": "152"
        }
      },
      "time": "2021-Aug-24 20:43:43.043406 UTC",
      "uptime": 6021282,
      "validated_ledger": {
        "base_fee": 10,
        "close_time": 683153020,
        "hash": "ABEF3D24015E8B6B7184B4ABCEDC0E0E3AA4F0677FAB91C40B1E500707C1F3E5",
        "reserve_base": 20000000,
        "reserve_inc": 5000000,
        "seq": 65887184
      },
      "validation_quorum": 33
    },
    "status": "success"
  }
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
Loading: "/etc/opt/ripple/rippled.cfg"
2020-Mar-24 01:30:08.646201720 UTC HTTPClient:NFO Connecting to 127.0.0.1:5005

Headers

{
  "result": {
    "state": {
      "build_version": "1.7.2",
      "complete_ledgers": "65844785-65887184",
      "io_latency_ms": 3,
      "jq_trans_overflow": "580",
      "last_close": {
        "converge_time": 3012,
        "proposers": 41
      },
      "load_base": 256,
      "load_factor": 134022,
      "load_factor_fee_escalation": 134022,
      "load_factor_fee_queue": 256,
      "load_factor_fee_reference": 256,
      "load_factor_server": 256,
      "peer_disconnects": "792367",
      "peer_disconnects_resources": "7273",
      "peers": 72,
      "pubkey_node": "n9LNvsFiYfFf8va6pma2PHGJKVLSyZweN1iBAkJQSeHw4GjM8gvN",
      "server_state": "full",
      "server_state_duration_us": "422128665555",
      "state_accounting": {
        "connected": {
          "duration_us": "172799714",
          "transitions": "1"
        },
        "disconnected": {
          "duration_us": "309059",
          "transitions": "1"
        },
        "full": {
          "duration_us": "6020429212246",
          "transitions": "143"
        },
        "syncing": {
          "duration_us": "413813232",
          "transitions": "152"
        },
        "tracking": {
          "duration_us": "266553605",
          "transitions": "152"
        }
      },
      "time": "2021-Aug-24 20:43:43.043406 UTC",
      "uptime": 6021282,
      "validated_ledger": {
        "base_fee": 10,
        "close_time": 683153020,
        "hash": "ABEF3D24015E8B6B7184B4ABCEDC0E0E3AA4F0677FAB91C40B1E500707C1F3E5",
        "reserve_base": 20000000,
        "reserve_inc": 5000000,
        "seq": 65887184
      },
      "validation_quorum": 33
    },
    "status": "success"
  }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 상태 객체가 유일한 필드로 포함된 성공적인 결과입니다.

상태 객체에는 다음과 같은 필드가 배열되어 있을 수 있습니다:

| `Field`                          | 유형       | 설명                                                                                                                                                                                                                                                                                                     |
| -------------------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `amendment_blocked`              | Boolean  | (생략 가능) 참이면 이 서버는 수정이 차단된 서버입니다. 서버가 수정이 차단되지 않은 경우 응답은 이 필드를 생략합니다.[![새로운 기능: Rippled 0.80.0](https://img.shields.io/badge/New%20in-rippled%200.80.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.80.0)                                                                               |
| `build_version`                  | 문자열      | 실행 중인 `rippled` 버전의 버전 번호입니다.                                                                                                                                                                                                                                                                          |
| `complete_ledgers`               | 문자열      | 로컬 리`ripple`가 데이터베이스에 가지고 있는 원장 버전의 시퀀스 번호를 나타내는 범위 표현식입니다. "2500-5000,32570-7695432"와 같이 분리되지 않은 시퀀스일 수 있습니다. 서버에 완전한 원장이 없는 경우(예: 최근에 네트워크와 동기화를 시작한 경우) 이 문자열은 비어 있습니다.                                                                                                                             |
| `closed_ledger`                  | 객체       | (생략 가능) 합의에 의해 검증되지 않은 가장 최근에 닫힌 원장에 대한 정보입니다. 가장 최근에 유효성이 검증된 원장을 사용할 수 있는 경우, 응답은 이 필드를 생략하고 대신 validated\_ledger를 포함합니다. 멤버 필드는 validated\_ledger 필드와 동일합니다.                                                                                                                                        |
| `io_latency_ms`                  | 숫자       | I/O 작업을 기다리는 데 소요된 시간(밀리초)입니다. 이 수치가 매우 낮지 않으면 rippled 서버에 심각한 부하 문제가 있을 수 있습니다.                                                                                                                                                                                                                       |
| `jq_trans_overflow`              | 문자열 - 숫자 | 이 서버가 한 번에 250개 이상의 트랜잭션을 처리하기 위해 대기 중인 횟수입니다. 숫자가 크면 서버가 XRP 레저 네트워크의 트랜잭션 부하를 처리할 수 없다는 의미일 수 있습니다. 미래 대비 서버 사양에 대한 자세한 권장 사항은 용량 계획을 참조하세요.[![새로운 기능: Rippled 0.90.0](https://img.shields.io/badge/New%20in-rippled%200.90.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.90.0)     |
| `last_close`                     | 객체       | 합의에 도달하는 데 걸린 시간, 참여한 신뢰할 수 있는 검증자 수 등 서버가 마지막으로 원장을 닫은 시간에 대한 정보입니다.                                                                                                                                                                                                                                  |
| `last_close.converge_time`       | 숫자       | 가장 최근에 검증한 원장 버전에서 합의에 도달하는 데 걸린 시간(밀리초)입니다.                                                                                                                                                                                                                                                           |
| `last_close.proposers`           | 숫자       | 가장 최근에 검증한 원장 버전에 대한 합의 프로세스에서 서버가 고려한 신뢰할 수 있는 검증자(검증자로 구성된 경우 자신 포함) 수입니다.                                                                                                                                                                                                                           |
| `load`                           | 객체       | (관리자 전용) 서버의 현재 로드 상태에 대한 자세한 정보입니다.                                                                                                                                                                                                                                                                   |
| `load.job_types`                 | 배열       | (관리자 전용) 서버가 수행 중인 다양한 유형의 작업 비율과 각 작업에 소요되는 시간에 대한 정보입니다.                                                                                                                                                                                                                                             |
| `load.threads`                   | 숫자       | (관리자 전용) 서버의 기본 작업 풀에 있는 스레드 수입니다.                                                                                                                                                                                                                                                                     |
| `load_base`                      | 정수       | 트랜잭션 비용 계산에 사용되는 기준 서버 부하량입니다. load\_factor가 load\_base와 같으면 기본 트랜잭션 비용만 적용됩니다. load\_factor가 load\_base보다 크면 트랜잭션 비용에 두 값의 비율을 곱합니다. 예를 들어 load\_factor가 load\_base의 두 배이면 트랜잭션 비용이 두 배가 됩니다.                                                                                                         |
| `load_factor`                    | 숫자       | 서버가 현재 적용 중인 로드 팩터입니다. 이 값과 load\_base 사이의 비율에 따라 트랜잭션 비용의 배수가 결정됩니다. 로드 팩터는 개별 서버의 로드 팩터, 클러스터의 로드 팩터, 공개 원장 비용, 전체 네트워크의 로드 팩터 중 가장 높은 값에 의해 결정됩니다.[![업데이트: 파문 0.33.0](https://img.shields.io/badge/Updated%20in-rippled%200.33.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.33.0) |
| `load_factor_fee_escalation`     | 숫자       | 수수료 수준에서 오픈 원장에 진입하기 위한 트랜잭션 비용에 대한 현재 승수입니다. [![새로운 기능: 파문 0.32.0](https://img.shields.io/badge/New%20in-rippled%200.32.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.32.0)                                                                                                          |
| `load_factor_fee_queue`          | 숫자       | _(생략될 수 있음)_ 대기열이 꽉 찼을 때 대기열에 들어가기 위한 트랜잭션 비용의 현재 승수(수수료 수준)입니다. [![새로운 기능: 파문 0.32.0](https://img.shields.io/badge/New%20in-rippled%200.32.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.32.0)                                                                                       |
| `load_factor_fee_reference`      | 숫자       | 로드 스케일링이 없는 트랜잭션 비용(수수료 수준).[![새로운 기능: 파문 0.32.0](https://img.shields.io/badge/New%20in-rippled%200.32.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.32.0)                                                                                                                            |
| `load_factor_server`             | 숫자       | (생략 가능) 서버가 적용하는 부하율로, 공개 원장 비용은 포함하지 않습니다.[![새로운 기능: 파문 0.33.0](https://img.shields.io/badge/New%20in-rippled%200.33.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.33.0)                                                                                                             |
| `peers`                          | 숫자       | (리포팅 모드 서버에서는 생략) 이 서버가 현재 연결되어 있는 다른 리플 서버의 수입니다.                                                                                                                                                                                                                                                     |
| `pubkey_node`                    | 문자열      | P2P 통신을 위해 이 서버를 확인하는 데 사용되는 공개 키입니다. 이 노드 키 쌍은 서버를 처음 시작할 때 서버가 자동으로 생성합니다. (삭제하면 서버가 새 키 쌍을 생성할 수 있습니다.) 클러스터링에 유용한 \[node\_seed] 구성 옵션을 사용하여 구성 파일에서 영구값을 설정할 수 있습니다.                                                                                                                               |
| `pubkey_validator`               | 문자열      | (관리자 전용) 이 노드가 원장 유효성 검사에 서명하는 데 사용하는 공개 키입니다. 이 유효성 검사 키 쌍은 \[validator\_token] 또는 \[validation\_seed] 구성 필드에서 파생됩니다.                                                                                                                                                                                 |
| `reporting`                      | 객체       | (리포 모드 서버만 해당) 이 서버의 보고 모드별 구성에 대한 정보입니다.                                                                                                                                                                                                                                                              |
| `reporting.etl_sources`          | 배열       | (리포팅 모드 서버만 해당) 이 리포팅 모드에서 데이터를 검색하는 P2P 모드 서버의 목록입니다. 이 배열의 각 항목은 ETL 소스 객체입니다.                                                                                                                                                                                                                       |
| `reporting.is_writer`            | Boolean  | (리포팅 모드 서버만 해당) 참이면 이 서버가 외부 데이터베이스에 원장 데이터를 쓰고 있습니다. 거짓이면 다른 보고 모드 서버가 현재 공유 데이터베이스를 채우고 있거나 읽기 전용으로 구성되어 있기 때문에 현재 쓰고 있지 않습니다.                                                                                                                                                                       |
| `reporting.last_publish_time`    | 문자열      | (리포팅 모드 서버만 해당) 이 서버가 P2P 모드 소스에서 새 유효성 검사된 원장을 마지막으로 본 시간을 나타내는 ISO 8601 타임스탬프입니다.                                                                                                                                                                                                                    |
| `server_state`                   | 문자열      | 서버가 네트워크에 어느 정도 참여하고 있는지를 나타내는 문자열입니다. 자세한 내용은 가능한 서버 상태를 참조하세요.                                                                                                                                                                                                                                       |
| `server_state_duration_us`       | 숫자       | 서버가 현재 상태에 있었던 연속 마이크로초 수입니다.[![새로운 기능: Rippled 1.2.0](https://img.shields.io/badge/New%20in-rippled%201.2.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.2.0)                                                                                                                         |
| `state_accounting`               | 객체       | 서버가 각 상태에서 보낸 시간에 대한 정보가 포함된 다양한 서버 상태의 맵입니다. 서버의 네트워크 연결 상태를 장기적으로 추적하는 데 유용할 수 있습니다.[![새로운 기능: 파문 0.30.1](https://img.shields.io/badge/New%20in-rippled%200.30.1-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.30.1)                                                                  |
| `state_accounting.*.duration_us` | 문자열      | 서버가 이 상태에서 보낸 시간(마이크로초)입니다. (서버가 다른 상태로 전환될 때마다 업데이트됩니다.)[![새로운 기능: 파문 0.30.1](https://img.shields.io/badge/New%20in-rippled%200.30.1-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.30.1)                                                                                               |
| `state_accounting.*.transitions` | 문자열      | 서버가 이 상태로 변경된 횟수입니다.[![새로운 기능: 파문 0.30.1](https://img.shields.io/badge/New%20in-rippled%200.30.1-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.30.1)                                                                                                                                    |
| `time`                           | 문자열      | 서버의 시계에 따른 UTC 기준 현재 시간입니다.[![업데이트: 파문 1.5.0](https://img.shields.io/badge/Updated%20in-rippled%201.5.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.5.0)                                                                                                                              |
| `uptime`                         | 숫자       | 서버가 작동한 연속 시간(초) 수입니다.[![새로운 기능: 파문 0.30.1](https://img.shields.io/badge/New%20in-rippled%200.30.1-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.30.1)                                                                                                                                  |
| `validated_ledger`               | 객체       | (생략 가능) 가장 최근에 완전히 검증된 원장에 대한 정보입니다. 가장 최근에 유효성이 검증된 원장을 사용할 수 없는 경우, 응답은 이 필드를 생략하고 대신 closed\_ledger를 포함합니다.                                                                                                                                                                                         |
| `validated_ledger.base_fee`      | 숫자       | 트랜잭션을 네트워크에 전파하는 데 드는 기본 수수료(XRP 단위)입니다.                                                                                                                                                                                                                                                               |
| `validated_ledger.close_time`    | 숫자       | [Ripple ](https://xrpl.org/basic-data-types.html#specifying-time)에포크 이후 이 원장이 마감된 시간(초)입니다.                                                                                                                                                                                                            |
| `validated_ledger.hash`          | 문자열      | 이 원장 버전의 고유 해시(16진수)입니다.                                                                                                                                                                                                                                                                               |
| `validated_ledger.reserve_base`  | 숫자       | 가장 최근에 검증된 원장 버전 기준의 최소 [계정 준비금입니다.](https://xrpl.org/reserves.html)                                                                                                                                                                                                                                   |
| `validated_ledger.reserve_inc`   | 숫자       | 가장 최근에 유효성이 검사된 원장 버전을 기준으로 계정이 소유한 각 항목에 대한 소유자 예치금입니다.                                                                                                                                                                                                                                               |
| `validated_ledger.seq`           | 숫자       | 가장 최근에 유효성을 검사한 원장 버전의 원장 인덱스입니다.                                                                                                                                                                                                                                                                      |
| `validation_quorum`              | 숫자       | 원장 버전을 검증하는 데 필요한 신뢰할 수 있는 최소 검증 횟수입니다. 상황에 따라 서버에서 더 많은 유효성 검사를 요구할 수 있습니다.                                                                                                                                                                                                                           |
| `validator_list_expires`         | 숫자       | (관리자 전용) 현재 검증자 목록이 만료될 때, Ripple 에포크 이후 초 단위로, 서버가 아직 게시된 검증자 목록을 로드하지 않은 경우 0입니다.[![새로운 기능: Rippled 0.80.1](https://img.shields.io/badge/New%20in-rippled%200.80.1-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.80.1)                                                                |

## ETL 소스 객체

보고 모드 서버에서 etl\_sources 필드의 각 멤버는 다음 필드를 가진 객체입니다:

| 필드                          | 유형      | 설명                                                                                                                      |
| --------------------------- | ------- | ----------------------------------------------------------------------------------------------------------------------- |
| `connected`                 | Boolean | true이면 리포팅 모드 서버가 이 P2P 모드 서버에 연결됩니다. false이면 서버가 연결되지 않은 것입니다. 이는 구성이 잘못되었거나 네트워크가 중단되었거나 P2P 모드 서버가 다운되었기 때문일 수 있습니다. |
| `grpc_port`                 | 문자열     | 이 리포팅 모드 서버가 gRPC를 통해 연결하고 원장 데이터를 검색하도록 구성된 P2P 모드 서버의 포트 번호입니다.                                                       |
| `ip`                        | 문자열     | P2P 모드 서버의 IP 주소(IPv4 또는 IPv6)입니다.                                                                                      |
| `last_message_arrival_time` | 문자열     | 리포팅 모드 서버가 이 P2P 서버로부터 메시지를 수신한 가장 최근 시간을 나타내는 ISO 8601 타임스탬프입니다.                                                       |
| `validated_ledgers_range`   | 문자열     | 이 P2P 모드 서버가 사용 가능하다고 보고하는 유효성 검사된 원장 버전의 범위로, complete\_ledgers와 동일한 형식입니다.                                            |
| `websocket_port`            | 문자열     | 리포팅 모드에서 직접 제공할 수 없는 WebSocket 요청을 전달하도록 이 보고 모드 서버가 구성되어 있는 P2P 서버의 포트 번호입니다.                                          |

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
