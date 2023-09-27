# path\_find

_웹소켓 API 전용!_ path\_find 메소드는 트랜잭션이 발생할 수 있는 경로를 검색하고, 시간이 지남에 따라 경로가 변경되면 주기적으로 업데이트를 전송합니다. JSON-RPC에서 지원하는 더 간단한 버전은 ripple\_path\_find 메소드를 참조하세요. XRP로만 결제하는 경우에는 XRP를 모든 계정으로 직접 송금할 수 있으므로 경로를 찾을 필요가 없습니다.

path\_find 명령에는 세 가지 모드 또는 하위 명령이 있습니다. 하위 명령 매개변수로 원하는 모드를 지정하세요:

* create - 경로 찾기 정보 전송 시작.
* close - 경로 찾기 정보 전송 중지.
* status - 현재 열려 있는 경로 찾기 요청의 정보를 가져옵니다.

rippled 서버는 결제를 위해 가장 저렴한 경로 또는 경로 조합을 찾으려고 노력하지만, 이 방법으로 반환된 경로가 실제로 최적의 경로라고 보장할 수는 없습니다. 서버 부하로 인해 경로 찾기가 최상의 결과를 찾지 못할 수도 있습니다. 또한 신뢰할 수 없는 서버의 경로 찾기 결과에 주의해야 합니다. 서버는 운영자에게 돈을 벌기 위해 최적의 경로가 아닌 경로를 반환하도록 수정될 수 있습니다. 경로 찾기를 신뢰할 수 있는 자체 서버가 없는 경우, 서로 다른 당사자가 운영하는 여러 서버의 경로 찾기 결과를 비교하여 단일 서버가 잘못된 결과를 반환하는 위험을 최소화해야 합니다. (참고: 최적의 결과보다 낮은 결과를 반환하는 서버가 반드시 악의적인 행동의 증거는 아니며, 서버 과부하로 인한 증상일 수도 있습니다.)

## PATH\_FIND CREATE

create 하위 명령은 특정 계정에서 다른 계정으로 원하는 금액의 특정 통화를 받을 수 있도록 결제 거래가 이루어질 수 있는 가능한 경로를 찾기 위한 지속적인 요청을 생성합니다. 초기 응답에는 원하는 금액을 받을 수 있는 두 주소 사이의 추천 경로가 포함됩니다. 그 후 서버는 추가 메시지를 보내며, "type": "path\_find"를 사용하여 잠재적 경로에 대한 업데이트와 함께 추가 메시지를 보냅니다. 업데이트 빈도는 서버의 재량에 맡겨져 있지만, 일반적으로 새로운 ledger 버전이 있을 때 몇 초에 한 번씩 업데이트됩니다.

클라이언트는 한 번에 하나의 경로 찾기 요청만 열어둘 수 있습니다. 동일한 연결에 다른 경로 찾기 요청이 이미 열려 있으면 이전 요청은 자동으로 닫히고 새 요청으로 대체됩니다.

## 요청 형식

요청 형식의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": 8,
    "command": "path_find",
    "subcommand": "create",
    "source_account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
    "destination_account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
    "destination_amount": {
        "value": "0.001",
        "currency": "USD",
        "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B"
    }
}
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| `Field`               | 유형        | 설명                                                                                                                                                                                                         |
| --------------------- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `subcommand`          | 문자열       | create 하위 명령을 보내려면 "create"를 사용합니다.                                                                                                                                                                        |
| `source_account`      | 문자열       | 경로를 찾을 계정의 고유 주소입니다. (즉, 결제를 송금할 계정입니다.)                                                                                                                                                                   |
| `destination_account` | 문자열       | 경로를 찾을 계정의 고유 주소입니다. (즉, 결제를 받을 계정입니다.)                                                                                                                                                                    |
| `destination_amount`  | 문자열 또는 객체 | 통화 대상 계정이 트랜잭션에서 받을 금액입니다. 특수한 경우: 리플 0.30.0에 추가됨 "-1"(XRP의 경우)을 지정하거나 -1을 값 필드의 내용으로 제공할 수 있습니다(XRP가 아닌 통화의 경우). 이렇게 하면 send\_max에 지정된 금액(제공된 경우) 이상을 지출하지 않으면서 가능한 한 많은 금액을 전달할 경로를 요청합니다.               |
| `send_max`            | 문자열 또는 객체 | (선택 사항) 트랜잭션에서 지출할 통화 금액입니다. source\_currencies와 호환되지 않습니다.[![새로운 기능: 파문 0.30.0](https://img.shields.io/badge/New%20in-rippled%200.30.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.30.0) |
| `paths`               | 배열        | (선택 사항) 확인할 결제 경로를 나타내는 객체 배열 배열입니다. 이미 알고 있는 특정 경로의 변경 사항을 계속 업데이트하거나 특정 경로를 따라 결제하는 데 드는 전체 비용을 확인하는 데 사용할 수 있습니다.                                                                                       |

서버는 다음 필드도 인식하지만 사용 결과가 보장되지는 않습니다: source\_currencies, bridges. 이러한 필드는 나중에 사용하기 위해 예약된 것으로 간주해야 합니다.

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 1,
  "status": "success",
  "type": "response",
  "result": {
    "alternatives": [
      {
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
              "issuer": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
              "type": 48,
              "type_hex": "0000000000000030"
            },
            {
              "account": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
              "type": 1,
              "type_hex": "0000000000000001"
            }
          ],
          [
            {
              "currency": "USD",
              "issuer": "r9vbV3EHvXWjSkeQ6CAcYVPGeq7TuiXY2X",
              "type": 48,
              "type_hex": "0000000000000030"
            },
            {
              "account": "r9vbV3EHvXWjSkeQ6CAcYVPGeq7TuiXY2X",
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
            }
          ]
        ],
        "source_amount": "251686"
      },
      {
        "paths_computed": [
          [
            {
              "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
              "type": 1,
              "type_hex": "0000000000000001"
            },
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
              "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
              "type": 1,
              "type_hex": "0000000000000001"
            },
            {
              "currency": "USD",
              "issuer": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
              "type": 48,
              "type_hex": "0000000000000030"
            },
            {
              "account": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
              "type": 1,
              "type_hex": "0000000000000001"
            }
          ],
          [
            {
              "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
              "type": 1,
              "type_hex": "0000000000000001"
            },
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
            }
          ]
        ],
        "source_amount": {
          "currency": "BTC",
          "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
          "value": "0.000001541291269274307"
        }
      },
      {
        "paths_computed": [
          [
            {
              "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
              "type": 1,
              "type_hex": "0000000000000001"
            },
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
          ]
        ],
        "source_amount": {
          "currency": "CHF",
          "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
          "value": "0.0009211546262510451"
        }
      },
      {
        "paths_computed": [
          [
            {
              "account": "razqQKzJRdB4UxFPWf5NEpEG3WMkmwgcXA",
              "type": 1,
              "type_hex": "0000000000000001"
            },
            {
              "currency": "USD",
              "issuer": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
              "type": 48,
              "type_hex": "0000000000000030"
            },
            {
              "account": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
              "type": 1,
              "type_hex": "0000000000000001"
            }
          ],
          [
            {
              "account": "razqQKzJRdB4UxFPWf5NEpEG3WMkmwgcXA",
              "type": 1,
              "type_hex": "0000000000000001"
            },
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
          ]
        ],
        "source_amount": {
          "currency": "CNY",
          "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
          "value": "0.006293562"
        }
      },
      {
        "paths_computed": [
          [
            {
              "account": "rGwUWgN5BEg3QGNY3RX2HfYowjUTZdid3E",
              "type": 1,
              "type_hex": "0000000000000001"
            },
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
            }
          ],
          [
            {
              "account": "rGwUWgN5BEg3QGNY3RX2HfYowjUTZdid3E",
              "type": 1,
              "type_hex": "0000000000000001"
            },
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
              "account": "rGwUWgN5BEg3QGNY3RX2HfYowjUTZdid3E",
              "type": 1,
              "type_hex": "0000000000000001"
            },
            {
              "currency": "USD",
              "issuer": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
              "type": 48,
              "type_hex": "0000000000000030"
            },
            {
              "account": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
              "type": 1,
              "type_hex": "0000000000000001"
            }
          ]
        ],
        "source_amount": {
          "currency": "DYM",
          "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
          "value": "0.0007157142857142858"
        }
      },
      {
        "paths_computed": [
          [
            {
              "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
              "type": 1,
              "type_hex": "0000000000000001"
            },
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
              "account": "rLEsXccBGNR3UPuPu2hUXPjziKC3qKSBun",
              "type": 1,
              "type_hex": "0000000000000001"
            },
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
              "account": "rLEsXccBGNR3UPuPu2hUXPjziKC3qKSBun",
              "type": 1,
              "type_hex": "0000000000000001"
            },
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
            }
          ]
        ],
        "source_amount": {
          "currency": "EUR",
          "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
          "value": "0.0007409623616236163"
        }
      },
      {
        "paths_computed": [
          [
            {
              "account": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
              "type": 1,
              "type_hex": "0000000000000001"
            },
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
          ]
        ],
        "source_amount": {
          "currency": "JPY",
          "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
          "value": "0.103412412"
        }
      }
    ],
    "destination_account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
    "destination_amount": {
      "currency": "USD",
      "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
      "value": "0.001"
    },
    "id": 1,
    "source_account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
    "full_reply": false
  }
}
```
{% endtab %}
{% endtabs %}

초기 응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| 필드                    | 유형        | 설명                                                                                                                                                                                                                                                                                                                                   |
| --------------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `alternatives`        | 배열        | 아래 설명과 같이 제안 된 [경로](https://xrpl.org/paths.html)가 포함 된 개체 배열입니다. 비어 있으면 소스 계정과 대상 계정을 연결하는 경로가 발견되지 않은 것입니다.                                                                                                                                                                                                                         |
| `destination_account` | 문자열       | 거래를 받을 계정의 고유 주소입니다.                                                                                                                                                                                                                                                                                                                 |
| `destination_amount`  | 문자열 또는 객체 | 거래에서 목적지가 받게 될 통화 금액입니다.                                                                                                                                                                                                                                                                                                             |
| `source_account`      | 문자열       | 거래를 보내는 고유 주소입니다.                                                                                                                                                                                                                                                                                                                    |
| `full_reply`          | Boolean   | 거짓이면 검색이 완료되지 않은 결과입니다. 나중에 회신할 때 더 나은 경로가 있을 수 있습니다. 참이면 이 경로가 가장 좋은 경로입니다. (이론적으로는 더 나은 경로가 존재할 수 있지만, rippled은 이를 찾지 못합니다.) 경로 찾기 요청을 종료할 때까지 rippled은 새 원장이 닫힐 때마다 계속 업데이트를 보냅니다.[![새로운 기능: 파문 0.29.0](https://img.shields.io/badge/New%20in-rippled%200.29.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.29.0) |

대안 배열의 각 요소는 하나의 가능한 소스 통화(시작 계정에서 보유)에서 대상 계정 및 통화까지의 경로를 나타내는 객체입니다. 이 객체에는 다음과 같은 필드가 있습니다:

| 필드                   | 유형        | 설명                                                                                       |
| -------------------- | --------- | ---------------------------------------------------------------------------------------- |
| `paths_computed`     | 배열        | 결제 경로를 정의하는 객체 배열의 배열                                                                    |
| `source_amount`      | 문자열 또는 객체 | 대상이 원하는 금액을 받기 위해 소스가 이 경로를 따라 보내야 하는 통화 금액 입니다.                                         |
| `destination_amount` | 문자열 또는 객체 | (생략 가능) 통화 이 경로를 따라 목적지가 받게 될 금액입니다. `destination_amount`요청의 내용이 "-1" 특수 사례인 경우에만 포함됩니다. |

## 발생 가능한 오류

* 모든 범용오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* noEvents - 비동기 콜백을 지원하지 않는 프로토콜(예: JSON-RPC)을 사용하고 있습니다. (JSON-RPC와 호환되는 경로 찾기 메소드는 ripple\_path\_find 메소드를 참조하세요.)

## 비동기 후속 조치

서버는 초기 응답 외에도 유사한 형식의 메시지를 추가로 전송하여 시간이 지남에 따라 결제 경로의 상태를 업데이트합니다. 이러한 메시지에는 어떤 요청으로 인해 메시지가 전송되었는지 알 수 있도록 원래 웹소켓 요청의 ID와 "type" 필드가 포함됩니다: "경로 찾기" 필드는 추가 응답임을 나타냅니다. 다른 필드는 초기 응답과 동일한 방식으로 정의됩니다.

후속 응답에 "full\_reply": true가 포함되면 현재 ledger에서 리플이 찾을 수 있는 최적의 경로입니다.

다음은 path\_find 생성 요청의 비동기 후속 조치 예시입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": 1,
    "type": "path_find",
    "alternatives": [
        /* paths omitted from this example; same format as the initial response */
    ],
    "destination_account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
    "destination_amount": {
        "currency": "USD",
        "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
        "value": "0.001"
    },
    "source_account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59"
}
```
{% endtab %}
{% endtabs %}

## path\_find 닫기

path\_find의 close 하위 명령은 서버에 현재 열려 있는 경로 찾기 요청에 대한 정보 전송을 중지하도록 지시합니다.

## 요청 형식

요청 형식의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 57,
  "command": "path_find",
  "subcommand": "close"
}
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| 필드           | 유형  | 설명                              |
| ------------ | --- | ------------------------------- |
| `subcommand` | 문자열 | `"close"`닫기 하위 명령을 보내는 데 사용됩니다. |

## 응답 형식

경로 찾기 요청이 성공적으로 닫힌 경우 응답은 경로 찾기 생성에 대한 초기 응답과 동일한 형식에 다음 필드가 추가된 형식을 따릅니다:

| 필드       | 유형      | 설명                                        |
| -------- | ------- | ----------------------------------------- |
| `closed` | Boolean | true 값은 이 응답이 경로 찾기 닫기 명령에 대한 응답임을 나타냅니다. |

미종료된 경로 찾기 요청이 없는 경우 오류가 대신 반환됩니다.

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 필드가 잘못 지정되었거나 필수 필드가 누락된 경우.
* noEvents - 비동기 콜백을 지원하지 않는 프로토콜(예: JSON-RPC)에서 이 메소드를 사용하려고 시도한 경우. (JSON-RPC와 호환되는 경로 찾기 메소드는 ripple\_path\_find 메소드를 참조하세요.)
* noPathRequest - 열려 있는 요청이 없는데 경로 찾기 요청을 닫으려고 했습니다.

## path\_find 상태

status 하위 명령은 클라이언트의 현재 열려 있는 경로 찾기 요청에 대한 즉각적인 업데이트를 요청합니다.

## 요청 형식

요청 형식의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 58,
  "command": "path_find",
  "subcommand": "status"
}
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| 필드           | 유형  | 설명                             |
| ------------ | --- | ------------------------------ |
| `subcommand` | 문자열 | "status"를 사용하여 상태 하위 명령을 보냅니다. |

## 응답 형식

[`path_find create`](https://xrpl.org/path\_find.html#path\_find-create)이 열려 있는 경우 응답은 경로 찾기 생성에 대한 초기 응답과 동일한 형식에 다음 필드가 추가된 형식을 따릅니다:

| 필드       | 유형      | 설명                                               |
| -------- | ------- | ------------------------------------------------ |
| `status` | Boolean | 값이 true이면 이 응답이 path\_find 상태 명령에 대한 응답임을 나타냅니다. |

미결 경로 찾기 요청이 없는 경우 오류가 대신 반환됩니다.

## 발생 가능한 오류

* 모든 범용 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* noEvents - 비동기 콜백을 지원하지 않는 프로토콜(예: JSON-RPC)을 사용하고 있습니다. (JSON-RPC와 호환되는 경로 찾기 메소드는 ripple\_path\_find 메소드를 참조하세요.)
* noPathRequest - 열려 있는 경로 찾기 요청이 없을 때 경로 찾기 요청의 상태를 확인하려고 했습니다.
