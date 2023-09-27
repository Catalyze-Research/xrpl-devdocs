# ripple\_path\_find

ripple\_path\_find 메소드는 바로 사용할 수 있는 결제 경로가 포함된 단일 응답을 제공하는 path\_find 메소드의 간소화된 버전입니다. 이 메소드는 웹소켓과 JSON-RPC API 모두에서 사용할 수 있습니다. 하지만 시간이 지나면 결과가 구식이 되는 경향이 있습니다. 최신 상태를 유지하기 위해 여러 번 호출하는 대신 가능하면 [path\_find](https://xrpl.org/path\_find.html) 메소드를 사용하여 지속적인 업데이트를 구독해야 합니다.

rippled 서버는 결제를 위해 가장 저렴한 경로 또는 경로 조합을 찾으려고 노력하지만, 이 메소드가 반환하는 경로가 실제로 가장 좋은 경로라는 보장은 없습니다.

{% hint style="info" %}
Caution:

신뢰할 수 없는 서버의 경로 찾기 결과에 주의하세요. 서버는 운영자에게 돈을 벌기 위해 최적 경로가 아닌 경로를 반환하도록 수정될 수 있습니다. 또한 서버가 과부하 상태일 때 잘못된 결과를 반환할 수도 있습니다. 경로 찾기를 신뢰할 수 있는 자체 서버가 없는 경우, 서로 다른 당사자가 운영하는 여러 서버의 경로 찾기 결과를 비교하여 단일 서버가 잘못된 결과를 반환하는 위험을 최소화해야 합니다.
{% endhint %}

## 요청 형식

요청 형식의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": 8,
    "command": "ripple_path_find",
    "source_account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
    "source_currencies": [
        {
            "currency": "XRP"
        },
        {
            "currency": "USD"
        }
    ],
    "destination_account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
    "destination_amount": {
        "value": "0.001",
        "currency": "USD",
        "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B"
    }
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "ripple_path_find",
    "params": [
        {
            "destination_account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
            "destination_amount": {
                "currency": "USD",
                "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                "value": "0.001"
            },
            "source_account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
            "source_currencies": [
                {
                    "currency": "XRP"
                },
                {
                    "currency": "USD"
                }
            ]
        }
    ]
}
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| `Field`               | 유형              | 설명                                                                                                                                                                                                             |
| --------------------- | --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `source_account`      | 문자열             | 거래에서 자금을 보낼 계좌의 고유 주소                                                                                                                                                                                          |
| `destination_account` | 문자열             | 거래에서 자금을 받을 계좌의 고유 주소                                                                                                                                                                                          |
| `destination_amount`  | 문자열 또는 객체       | 객체 통화 대상 계정이 거래에서 받게 될 금액입니다. 특수한 경우: 리플 0.30.0에 추가됨 "-1"(XRP의 경우)을 지정하거나 -1을 값 필드의 내용으로 제공할 수 있습니다(XRP가 아닌 통화의 경우). 이렇게 하면 send\_max에 지정된 금액(제공된 경우) 이상을 지출하지 않으면서 가능한 한 많은 금액을 전달할 경로를 요청합니다.                |
| `send_max`            | 문자열 또는 객체       | (선택 사항) 트랜잭션에서 지출할 통화 금액입니다. source\_currencies와 함께 사용할 수 없습니다.[![새로운 기능: 파문 0.30.0](https://img.shields.io/badge/New%20in-rippled%200.30.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.30.0) |
| `source_currencies`   | 배열              | (선택 사항) 소스 계정에서 지출할 수 있는 통화의 배열입니다. 배열의 각 항목은 통화 금액 지정 방법과 같이 필수 통화 필드와 선택적 발행자 필드가 있는 JSON 객체여야 합니다. 18개 이상의 소스 통화를 포함할 수 없습니다. 기본적으로 최대 88개의 서로 다른 통화/발행자 쌍까지 사용 가능한 모든 소스 통화를 사용합니다.                        |
| `ledger_hash`         | 문자열             | (선택 사항) 사용할 원장 버전에 대한 20바이트 16진수 문자열입니다. (원장 지정하기 참조)                                                                                                                                                          |
| `ledger_index`        | 문자열 또는 부호 없는 정수 | (선택 사항) 사용할 원장의 원장 인덱스 또는 원장을 자동으로 선택하기 위한 단축 문자열입니다. (원장 지정하기 참조)                                                                                                                                             |

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": 8,
    "status": "success",
    "type": "response",
    "result": {
        "alternatives": [
            {
                "paths_canonical": [],
                "paths_computed": [
                    [
                        {
                            "currency": "USD",
                            "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                            "type": 48,
                            "type_hex": "0000000000000030"
                        },
                        {
                            "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                            "type": 1,
                            "type_hex": "0000000000000001"
                        }
                    ],
                    [
                        {
                            "currency": "USD",
                            "issuer": "rrpNnNLKrartuEqfJGpqyDwPj1AFPg9vn1",
                            "type": 48,
                            "type_hex": "0000000000000030"
                        },
                        {
                            "account": "rrpNnNLKrartuEqfJGpqyDwPj1AFPg9vn1",
                            "type": 1,
                            "type_hex": "0000000000000001"
                        },
                        {
                            "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                            "type": 1,
                            "type_hex": "0000000000000001"
                        }
                    ],
                    [
                        {
                            "currency": "USD",
                            "issuer": "rrpNnNLKrartuEqfJGpqyDwPj1AFPg9vn1",
                            "type": 48,
                            "type_hex": "0000000000000030"
                        },
                        {
                            "account": "rrpNnNLKrartuEqfJGpqyDwPj1AFPg9vn1",
                            "type": 1,
                            "type_hex": "0000000000000001"
                        },
                        {
                            "account": "rLpq4LgabRfm1xEX5dpWfJovYBH6g7z99q",
                            "type": 1,
                            "type_hex": "0000000000000001"
                        },
                        {
                            "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                            "type": 1,
                            "type_hex": "0000000000000001"
                        }
                    ],
                    [
                        {
                            "currency": "USD",
                            "issuer": "rrpNnNLKrartuEqfJGpqyDwPj1AFPg9vn1",
                            "type": 48,
                            "type_hex": "0000000000000030"
                        },
                        {
                            "account": "rrpNnNLKrartuEqfJGpqyDwPj1AFPg9vn1",
                            "type": 1,
                            "type_hex": "0000000000000001"
                        },
                        {
                            "account": "rPuBoajMjFoDjweJBrtZEBwUMkyruxpwwV",
                            "type": 1,
                            "type_hex": "0000000000000001"
                        },
                        {
                            "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                            "type": 1,
                            "type_hex": "0000000000000001"
                        }
                    ]
                ],
                "source_amount": "256987"
            }
        ],
        "destination_account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
        "destination_currencies": [
            "015841551A748AD2C1F76FF6ECB0CCCD00000000",
            "JOE",
            "DYM",
            "EUR",
            "CNY",
            "MXN",
            "BTC",
            "USD",
            "XRP"
        ]
    }
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
200 OK

{
    "result": {
        "alternatives": [
            {
                "paths_canonical": [],
                "paths_computed": [
                    [
                        {
                            "currency": "USD",
                            "issuer": "rpDMez6pm6dBve2TJsmDpv7Yae6V5Pyvy2",
                            "type": 48,
                            "type_hex": "0000000000000030"
                        },
                        {
                            "account": "rpDMez6pm6dBve2TJsmDpv7Yae6V5Pyvy2",
                            "type": 1,
                            "type_hex": "0000000000000001"
                        },
                        {
                            "account": "rfDeu7TPUmyvUrffexjMjq3mMcSQHZSYyA",
                            "type": 1,
                            "type_hex": "0000000000000001"
                        },
                        {
                            "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                            "type": 1,
                            "type_hex": "0000000000000001"
                        }
                    ],
                    [
                        {
                            "currency": "USD",
                            "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                            "type": 48,
                            "type_hex": "0000000000000030"
                        },
                        {
                            "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                            "type": 1,
                            "type_hex": "0000000000000001"
                        }
                    ],
                    [
                        {
                            "currency": "USD",
                            "issuer": "rLEsXccBGNR3UPuPu2hUXPjziKC3qKSBun",
                            "type": 48,
                            "type_hex": "0000000000000030"
                        },
                        {
                            "account": "rLEsXccBGNR3UPuPu2hUXPjziKC3qKSBun",
                            "type": 1,
                            "type_hex": "0000000000000001"
                        },
                        {
                            "account": "raspZSGNiTKi5jmvFxUYCuYXPv1V8WhL5g",
                            "type": 1,
                            "type_hex": "0000000000000001"
                        },
                        {
                            "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                            "type": 1,
                            "type_hex": "0000000000000001"
                        }
                    ],
                    [
                        {
                            "currency": "USD",
                            "issuer": "rpHgehzdpfWRXKvSv6duKvVuo1aZVimdaT",
                            "type": 48,
                            "type_hex": "0000000000000030"
                        },
                        {
                            "account": "rpHgehzdpfWRXKvSv6duKvVuo1aZVimdaT",
                            "type": 1,
                            "type_hex": "0000000000000001"
                        },
                        {
                            "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                            "type": 1,
                            "type_hex": "0000000000000001"
                        }
                    ]
                ],
                "source_amount": "207414"
            }
        ],
        "destination_account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
        "destination_currencies": [
            "USD",
            "JOE",
            "BTC",
            "DYM",
            "CNY",
            "EUR",
            "015841551A748AD2C1F76FF6ECB0CCCD00000000",
            "MXN",
            "XRP"
        ],
        "status": "success"
    }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`                  | 유형  | 설명                                                                                                             |
| ------------------------ | --- | -------------------------------------------------------------------------------------------------------------- |
| `alternatives`           | 배열  | 아래 설명과 같이 취할 수 있는 경로가 있는 객체 배열입니다. 비어 있으면 원본 계정과 대상 계정을 연결하는 경로가 없습니다.                                         |
| `destination_account`    | 문자열 | 결제 거래를 받을 계좌의 고유 주소                                                                                            |
| `destination_currencies` | 배열  | 목적지에서 수락하는 통화를 나타내는 문자열 배열(예: "USD"와 같은 3글자 코드 또는 "015841551A748AD2C1F76FF6ECB0CCCD00000000"와 같은 40자 16진수)입니다. |

대안 배열의 각 요소는 하나의 가능한 소스 통화(시작 계정에서 보유)에서 대상 계정 및 통화까지의 경로를 나타내는 객체입니다. 이 객체에는 다음과 같은 필드가 있습니다:

| `Field`          | 유형        | 설명                                                   |
| ---------------- | --------- | ---------------------------------------------------- |
| `paths_computed` | 배열        | [결제 경로를](https://xrpl.org/paths.html) 정의하는 객체 배열의 배열 |
| `source_amount`  | 문자열 또는 객체 | 대상이 원하는 금액을 받기 위해 소스가 이 경로를 따라 보내야 하는 통화 금액입니다.      |

다음 필드는 더 이상 사용되지 않으므로 생략할 수 있습니다: PATHS\_CANONICAL 및 PATHS\_EXPANDED. 이러한 필드가 표시되면 무시해야 합니다.

## 발생 가능한 오류

* 모든 범용 오류 유형입니다.
* tooBusy - 서버에 너무 많은 부하가 걸려 경로를 계산할 수 없습니다. 관리자로 연결되어 있는 경우 반환되지 않습니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* srcActMissing - 요청에서 source\_account 필드가 생략되었습니다.
* srcActMalformed - 요청의 source\_account 필드의 형식이 올바르지 않습니다.
* dstActMissing - 요청에서 대상\_계정 필드가 생략되었습니다.
* dstActMalformed - 요청의 대상\_계정 필드의 형식이 올바르지 않습니다.
* srcCurMalformed - 원본\_통화 필드의 형식이 올바르지 않습니다.
* srcIsrMalformed - 요청에 있는 하나 이상의 통화 객체의 발행자 필드가 유효하지 않습니다.
