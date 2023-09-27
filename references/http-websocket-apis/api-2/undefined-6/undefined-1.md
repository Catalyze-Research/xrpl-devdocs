# 피어 크롤러

문피어 크롤러는 P2P 네트워크의 상태와 토폴로지를 보고하기 위한 특수한 피어 포트 방식입니다. 이 API 방법은 기본적으로 피어 프로토콜 포트를 통해 권한 없이 사용할 수 있으며, 컨센서스, ledger 기록 및 기타 필요한 정보에 대한 rippled 서버의 P2P 통신에도 사용됩니다.

피어 크롤러가 보고하는 정보는 사실상 공개되며, 전체 XRP Ledger 네트워크, 상태 및 토폴로지에 대해 보고하는 데 사용할 수 있습니다.

## 요청 형식

피어 크롤러 정보를 요청하려면 다음 HTTP 요청을 보내세요:

* 프로토콜: https
* HTTP 메소드: GET
* 호스트: (rippled 서버, 호스트 이름 또는 IP 주소 기준)
* 포트: (rippled 서버가 피어 프로토콜을 사용하는 포트 번호, 일반적으로 51235)
* 경로: /crawl
* 보안: 대부분의 rippled 서버는 자체 서명된 인증서를 사용하여 요청에 응답합니다. 기본적으로 대부분의 도구(웹 브라우저 포함)는 이러한 응답을 신뢰할 수 없는 것으로 플래그를 지정하거나 차단합니다. 이러한 서버의 응답을 표시하려면 인증서 검사를 무시해야 합니다(예: cURL을 사용하는 경우 --insecure 플래그 추가).

## 응답 형식

응답에는 상태 코드 200 OK와 메시지 본문에 JSON 객체가 있습니다.

JSON 객체에는 다음과 같은 필드가 있습니다:

| `Field`   | 값  | 설명                                                                                                                                                                                                                                                                                      |
| --------- | -- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `counts`  | 객체 | (생략할 수 있음) get\_counts 메서드의 응답과 유사한 이 서버의 상태에 대한 통계입니다. 기본 구성에서는 이 필드를 보고하지 않습니다. 보고되는 정보에는 원장 및 트랜잭션 데이터베이스의 크기, 애플리케이션 내 캐시에 대한 캐시 적중률, 메모리에 캐시된 다양한 유형의 개체 수 등이 포함됩니다. 메모리에 저장될 수 있는 오브젝트 유형에는 원장(Ledger), 트랜잭션(STTx), 유효성 검사 메시지(STValidation) 등이 포함됩니다.                            |
| `overlay` | 객체 | (생략 가능) 피어 메서드의 응답과 유사하게 현재 이 서버에 연결된 피어 서버에 대한 정보입니다. 객체의 배열인 활성 필드가 하나 포함되어 있습니다(아래 참조).                                                                                                                                                                                              |
| `server`  | 객체 | (생략 가능) 이 서버에 대한 정보입니다. 실행 중인 리플 버전(build\_version), 서버에서 사용 가능한 원장 버전(complete\_ledgers), 서버에 발생한 부하량 등 server\_state 메서드의 공개 필드를 포함합니다.[![업데이트: 파문 1.2.1](https://img.shields.io/badge/Updated%20in-rippled%201.2.1-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.2.1) |
| `unl`     | 객체 | (생략 가능) 유효성 검사기 메서드 및 유효성 검사기 목록 사이트 메서드의 응답과 유사하게 서버가 신뢰하도록 구성된 유효성 검사기 및 유효성 검사기 목록 사이트에 대한 정보입니다.[![업데이트: 파문 1.2.1](https://img.shields.io/badge/Updated%20in-rippled%201.2.1-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.2.1)                                      |
| `version` | 숫자 | 이 피어 크롤러 응답 형식의 버전을 나타냅니다. 현재 피어 크롤러 버전 번호는 2입니다.[![업데이트: 파문 1.2.1](https://img.shields.io/badge/Updated%20in-rippled%201.2.1-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.2.1)                                                                                         |

overlay.active 배열의 각 멤버는 다음 필드를 가진 객체입니다:

| `Field`            | 값                | 설명                                                                                                                                                                                                                                     |
| ------------------ | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `complete_ledgers` | 문자열              | 이 피어가 사용할 수 있는 원장 버전 범위입니다.                                                                                                                                                                                                            |
| `complete_shards`  | 문자열              | (생략 가능) 이 피어가 사용할 수 있는 원장 기록 샤드의 범위입니다.                                                                                                                                                                                                |
| `ip`               | 문자열(IPv4 주소)     | (생략 가능) 연결된 피어의 IP 주소입니다. 피어가 검증자 또는 프라이빗 피어로 구성된 경우 생략됩니다.[![업데이트: 파문 1.2.1](https://img.shields.io/badge/Updated%20in-rippled%201.2.1-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.2.1)                              |
| `port`             | 문자열(숫자)          | (생략 가능) RTXP를 제공하는 피어 서버의 포트 번호입니다. 일반적으로 51235입니다. 피어가 유효성 검사기 또는 비공개 피어로 구성된 경우 생략됩니다.[![업데이트: 파문 1.2.1](https://img.shields.io/badge/Updated%20in-rippled%201.2.1-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.2.1) |
| `public_key`       | 문자열(Base-64 인코딩) | 이 피어에서 RTXP 메시지에 서명하는 데 사용하는 ECDSA 키 쌍의 공개 키입니다. (이는 피어 서버의 server\_info 메서드가 보고한 pubkey\_node와 동일한 데이터입니다.)                                                                                                                           |
| `type`             | 문자열              | 피어에 대한 TCP 연결이 수신인지 또는 발신인지를 나타내는 값입니다.                                                                                                                                                                                                |
| `uptime`           | 숫자               | 서버가 이 피어에 연결된 시간(초)입니다.                                                                                                                                                                                                                |
| `version`          | 문자열              | 피어가 사용 중이라고 보고하는 `rippled` 버전 번호입니다.                                                                                                                                                                                                   |

## 예시

요청:

{% tabs %}
{% tab title="HTTP" %}
```json
GET https://localhost:51235/crawl
```
{% endtab %}

{% tab title="cURL" %}
```json
curl --insecure https://localhost:51235/crawl
```
{% endtab %}
{% endtabs %}

응답:

```json
200 OK

{
    "overlay": {
        "active": [
            {
                "complete_ledgers": "45498918-45500918",
                "ip": "88.99.137.170",
                "port": "51235",
                "public_key": "AkU+AY9FWh8AXMc43fAUM69SzfAMGat0d/N+qx3kD6Dg",
                "type": "out",
                "uptime": 208,
                "version": "rippled-1.2.1"
            },
            {
                "complete_ledgers": "45500790-45500918",
                "ip": "198.13.58.221",
                "port": "51235",
                "public_key": "AlQvJAlNDYtoBSaZCXM0pT5RWvdOW9QhMW5++mHswkej",
                "type": "out",
                "uptime": 208,
                "version": "rippled-1.2.0"
            },
            {
                "complete_ledgers": "45500662-45500918",
                "ip": "52.90.101.104",
                "port": "51235",
                "public_key": "AkA04ujnwMn8mRyfJg4K7vzcQSOG7FHq4wUg60OQWnCY",
                "type": "out",
                "uptime": 209,
                "version": "rippled-1.2.1"
            },
            {
                "complete_ledgers": "45500662-45500918",
                "ip": "54.202.12.93",
                "port": "51235",
                "public_key": "AxoekFvFYzELGty9cqiXZB+NsOWTZ0Qs9mFIw69CGb3d",
                "type": "out",
                "uptime": 209,
                "version": "rippled-1.2.1"
            },
            {
                "complete_ledgers": "45498918-45500918",
                "ip": "173.255.240.113",
                "port": "51235",
                "public_key": "A4lWBMIDEQrO8Eerp9Hj3rFacbV0FiID3wTIx8Aoplq2",
                "type": "out",
                "uptime": 209,
                "version": "rippled-1.1.0"
            },
            {
                "complete_ledgers": "45499894-45500918",
                "ip": "54.186.73.52",
                "port": "51235",
                "public_key": "AjikFnq0P2XybCyREr2KPiqXqJteqwPwVRVbVK+93+3o",
                "type": "out",
                "uptime": 209,
                "version": "rippled-1.2.1"
            },
            {
                "complete_ledgers": "45499894-45500918",
                "ip": "54.186.248.91",
                "port": "51235",
                "public_key": "A4A4TPA17KlUjstp7fcL0qaWd4X+fvZ5MTxG5P5AggHW",
                "type": "out",
                "uptime": 209,
                "version": "rippled-1.2.1"
            },
            {
                "complete_ledgers": "45490918-45500918",
                "ip": "162.243.114.118",
                "port": "51235",
                "public_key": "AufDkW4E1DOxjzRPj46Eu+AyJdsakUeJTz3xklv1kCfp",
                "type": "out",
                "uptime": 209,
                "version": "rippled-1.2.1"
            },
            {
                "complete_ledgers": "45498918-45500918",
                "ip": "::ffff:45.56.78.201",
                "port": "51235",
                "public_key": "AmsXz4UUqjlz6iy8HHhZdHmBHteEBwYZLOHCHA4puCwj",
                "type": "out",
                "uptime": 209,
                "version": "rippled-1.1.0"
            },
            {
                "complete_ledgers": "32570-45500918",
                "ip": "169.55.164.30",
                "port": "51235",
                "public_key": "Aw7J0CVhFKt0h6PDEpqu6t4LbPY0PsX8jCFbvSQFDOkW",
                "type": "out",
                "uptime": 209,
                "version": "rippled-1.2.1"
            }
        ]
    },
    "server": {
        "build_version": "1.2.1",
        "complete_ledgers": "45500881-45500888",
        "io_latency_ms": 1,
        "jq_trans_overflow": "0",
        "last_close": {
            "converge_time": 3002,
            "proposers": 25
        },
        "load_base": 256,
        "load_factor": 256,
        "load_factor_fee_reference": 256,
        "load_factor_server": 256,
        "peer_disconnects": "0",
        "peer_disconnects_resources": "0",
        "peers": 10,
        "pubkey_node": "n9MJZBu5HyxyEq8xPGBxXFTfT3uzdnNsvR6R1NyXxbEzt79SrZJE",
        "published_ledger": 45500888,
        "server_state": "full",
        "server_state_duration_us": "40756665",
        "state_accounting": {
            "connected": {
                "duration_us": "163459544",
                "transitions": 1
            },
            "disconnected": {
                "duration_us": "2539592",
                "transitions": 1
            },
            "full": {
                "duration_us": "40756665",
                "transitions": 1
            },
            "syncing": {
                "duration_us": "5071794",
                "transitions": 1
            },
            "tracking": {
                "duration_us": "1",
                "transitions": 1
            }
        },
        "time": "2019-Mar-02 01:48:50.912360",
        "uptime": 213,
        "validated_ledger": {
            "close_time": 604806530,
            "hash": "00415B0ECF1D31E8DC9A7DCB04CAF1FD47E61D4D9D047743C1508CDBD36576CE",
            "reserve_base": 20000000,
            "reserve_inc": 5000000,
            "seq": 45500918
        }
    },
    "unl": {
        "local_static_keys": [],
        "publisher_lists": [
            {
                "available": true,
                "expiration": "2019-Mar-06 00:00:00.000000000",
                "pubkey_publisher": "ED2677ABFFD1B33AC6FBC3062B71F1E8397C1505E1C42C64D11AD1B28FF73F4734",
                "seq": 47,
                "uri": "https://vl.ripple.com",
                "version": 1
            }
        ],
        "validator_list": {
            "count": 1,
            "expiration": "2019-Mar-06 00:00:00.000000000",
            "status": "active"
        },
        "validator_sites": [
            {
                "last_refresh_status": "accepted",
                "last_refresh_time": "2019-Mar-02 01:45:19.940242379",
                "next_refresh_time": "2019-Mar-02 01:50:19.568004480",
                "refresh_interval_min": 5,
                "uri": "https://vl.ripple.com"
            }
        ]
    },
    "version": 2
}
```

## 참조

* 피어 프로토콜.
* 피어 크롤러 구성.
* 유효성 검사 기록 서비스는 피어 크롤러를 사용하여 유효성 검사 관련 데이터를 수집, 집계, 저장 및 배포하는 서비스의 예입니다.
