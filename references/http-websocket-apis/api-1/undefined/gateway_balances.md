# gateway\_balances

gateway\_balances 명령은 특정 계정에서 발행된 총 잔액을 계산하며, 운영 주소에서 보유한 금액은 선택적으로 제외합니다. [![New in: rippled 0.28.2](https://img.shields.io/badge/New%20in-rippled%200.28.2-blue.svg)](https://github.com/ripple/rippled/releases/tag/0.28.2)

{% hint style="info" %}
Caution:

일부 퍼블릭 서버에서는 많은 양의 처리가 필요할 수 있으므로 이 API 메소드를 비활성화합니다.
{% endhint %}

## 요청 형식

요청 형식의 예입니다:

{% hint style="info" %}
Note:

이 메소드에 대한 커맨드라인 구문은 없습니다. 대신 json 메소드를 사용하여 커맨드라인에서 이 메소드에 액세스할 수 있습니다.
{% endhint %}

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": "example_gateway_balances_1",
    "command": "gateway_balances",
    "account": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
    "strict": true,
    "hotwallet": ["rKm4uWpg9tfwbVSeATv4KxDe6mpE9yPkgJ","ra7JkEzrgeKHdzKgo4EUUVBnxggY4z37kt"],
    "ledger_index": "validated"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "gateway_balances",
    "params": [
        {
            "account": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
            "hotwallet": [
                "rKm4uWpg9tfwbVSeATv4KxDe6mpE9yPkgJ",
                "ra7JkEzrgeKHdzKgo4EUUVBnxggY4z37kt"
            ],
            "ledger_index": "validated",
            "strict": true
        }
    ]
}jso
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| `Field`        | 유형              | 설명                                                                      |
| -------------- | --------------- | ----------------------------------------------------------------------- |
| `account`      | 문자열             | 확인할 주소입니다. 발급 주소여야 합니다.                                                 |
| `strict`       | Boolean         | (선택 사항) 참이면 계정 매개변수에 대해 주소 또는 공개 키만 허용합니다. 기본값은 거짓입니다.                  |
| `hotwallet`    | 문자열 또는 배열       | (선택 사항) 발급된 잔액에서 제외할 운영 주소 또는 해당 주소의 배열입니다.                             |
| `ledger_hash`  | 문자열             | (선택 사항) 사용할 원장 버전에 대한 20바이트 16진수 문자열입니다. (원장 지정하기 참조)                   |
| `ledger_index` | 문자열 또는 부호 없는 정수 | (선택 사항) 사용할 원장 버전의 원장 인덱스 또는 원장을 자동으로 선택하기 위한 바로가기 문자열입니다. (원장 지정하기 참조) |

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 3,
  "status": "success",
  "type": "response",
  "result": {
    "account": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
    "assets": {
      "r9F6wk8HkXrgYWoJ7fsv4VrUBVoqDVtzkH": [
        {
          "currency": "BTC",
          "value": "5444166510000000e-26"
        }
      ],
      "rPFLkxQk6xUGdGYEykqe7PR25Gr7mLHDc8": [
        {
          "currency": "EUR",
          "value": "4000000000000000e-27"
        }
      ],
      "rPU6VbckqCLW4kb51CWqZdxvYyQrQVsnSj": [
        {
          "currency": "BTC",
          "value": "1029900000000000e-26"
        }
      ],
      "rpR95n1iFkTqpoy1e878f4Z1pVHVtWKMNQ": [
        {
          "currency": "BTC",
          "value": "4000000000000000e-30"
        }
      ],
      "rwmUaXsWtXU4Z843xSYwgt1is97bgY8yj6": [
        {
          "currency": "BTC",
          "value": "8700000000000000e-30"
        }
      ]
    },
    "balances": {
      "rKm4uWpg9tfwbVSeATv4KxDe6mpE9yPkgJ": [
        {
          "currency": "EUR",
          "value": "29826.1965999999"
        }
      ],
      "ra7JkEzrgeKHdzKgo4EUUVBnxggY4z37kt": [
        {
          "currency": "USD",
          "value": "13857.70416"
        }
      ]
    },
    "ledger_hash": "61DDBF304AF6E8101576BF161D447CA8E4F0170DDFBEAFFD993DC9383D443388",
    "ledger_index": 14483195,
    "obligations": {
      "BTC": "5908.324927635318",
      "EUR": "992471.7419793958",
      "GBP": "4991.38706013193",
      "USD": "1997134.20229482"
    },
    "validated": true
  }
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
200 OK
{
    "result": {
        "account": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
        "assets": {
            "r9F6wk8HkXrgYWoJ7fsv4VrUBVoqDVtzkH": [
                {
                    "currency": "BTC",
                    "value": "5444166510000000e-26"
                }
            ],
            "rPFLkxQk6xUGdGYEykqe7PR25Gr7mLHDc8": [
                {
                    "currency": "EUR",
                    "value": "4000000000000000e-27"
                }
            ],
            "rPU6VbckqCLW4kb51CWqZdxvYyQrQVsnSj": [
                {
                    "currency": "BTC",
                    "value": "1029900000000000e-26"
                }
            ],
            "rpR95n1iFkTqpoy1e878f4Z1pVHVtWKMNQ": [
                {
                    "currency": "BTC",
                    "value": "4000000000000000e-30"
                }
            ],
            "rwmUaXsWtXU4Z843xSYwgt1is97bgY8yj6": [
                {
                    "currency": "BTC",
                    "value": "8700000000000000e-30"
                }
            ]
        },
        "balances": {
            "rKm4uWpg9tfwbVSeATv4KxDe6mpE9yPkgJ": [
                {
                    "currency": "EUR",
                    "value": "29826.1965999999"
                }
            ],
            "ra7JkEzrgeKHdzKgo4EUUVBnxggY4z37kt": [
                {
                    "currency": "USD",
                    "value": "13857.70416"
                }
            ]
        },
        "ledger_hash": "980FECF48CA4BFDEC896692C31A50D484BDFE865EC101B00259C413AA3DBD672",
        "ledger_index": 14483212,
        "obligations": {
            "BTC": "5908.324927635318",
            "EUR": "992471.7419793958",
            "GBP": "4991.38706013193",
            "USD": "1997134.20229482"
        },
        "status": "success",
        "validated": true
    }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`                | 유형                                                                | 설명                                                                              |
| ---------------------- | ----------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| `account`              | 문자열 - [주소](https://xrpl.org/basic-data-types.html#addresses)      | 주소 잔액을 발행한 계정의 주소입니다.                                                           |
| `obligations`          | 객체                                                                | (비어 있는 경우 생략) 제외되지 않은 주소에 발행된 총 금액으로, 발행된 총 금액에 대한 통화 맵입니다.                     |
| `balances`             | 객체                                                                | (비어 있는 경우 생략) 요청에서 핫월렛 주소로 발행된 금액입니다. 키는 주소이고 값은 해당 주소가 보유한 통화 금액의 배열입니다.       |
| `assets`               | 객체                                                                | (비어 있는 경우 생략) 다른 사람이 발행한 총 보유 금액입니다. 권장 구성에서는 발행 주소가 없어야 합니다.                   |
| `ledger_hash`          | 문자열 - [해시](https://xrpl.org/basic-data-types.html#hashes)         | (생략 가능) 이 응답을 생성하는 데 사용된 원장 버전의 식별 해시입니다.                                       |
| `ledger_index`         | 번호 - [원장 색인](https://xrpl.org/basic-data-types.html#ledger-index) | (생략 가능) 이 응답을 생성하는 데 사용된 원장 버전의 원장 인덱스입니다.                                      |
| `ledger_current_index` | 번호 - [원장 색인](https://xrpl.org/basic-data-types.html#ledger-index) | (ledger\_current\_index가 제공된 경우 생략) 이 정보를 검색하는 데 사용된 현재 진행 중인 원장 버전의 원장 인덱스입니다. |

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* invalidHotWallet - 핫월렛 필드에 지정된 주소 중 하나 이상이 요청에서 계정이 발행한 통화를 보유한 계정의 주소가 아닙니다.
* actNotFound - 요청의 계정 필드에 지정된 주소가 ledger에 있는 계정과 일치하지 않습니다.
* lgrNotFound - ledger\_hash 또는 ledger\_index로 지정한 ledger이 존재하지 않거나, 존재하지만 서버에 없습니다.
