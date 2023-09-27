# account\_lines

account\_lines 메소드는 계정의 신뢰선에 대한 정보를 반환하며, 여기에는 XRP가 아닌 모든 통화와 자산의 잔액이 포함되어 있습니다. 검색된 모든 정보는 특정 버전의 ledger을 기준으로 합니다.

## 요청 형식

요청 형식의 예시입니다:

{% tabs %}
{% tab title="WebSoket" %}
```json
{
  "id": 1,
  "command": "account_lines",
  "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "account_lines",
    "params": [
        {
            "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59"
        }
    ]
}
```
{% endtab %}
{% endtabs %}

이 요청은 다음 매개변수를 허용합니다:

| `Field`        | 유형                                                           | 설명                                                                                                            |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------- |
| `account`      | 문자열 - [주소](https://xrpl.org/basic-data-types.html#addresses) | 이 계좌에 연결된 신뢰선을 찾아보세요.                                                                                         |
| `ledger_hash`  | 문자열                                                          | _(선택 사항)_ 사용할 원장 버전에 대한 20바이트 16진수 문자열입니다. (원장 지정 참조)                                                         |
| `ledger_index` | 숫자 또는 문자열                                                    | _(선택)_ 사용할 원장의 원장 인덱스 또는 자동으로 원장을 선택하는 단축 문자열입니다. (원장 지정 참조)                                                  |
| `peer`         | 문자열 - [주소](https://xrpl.org/basic-data-types.html#addresses) | _(선택사항)_ 두 번째 계정 제공된 경우 두 계정을 연결하는 신뢰선으로 결과를 필터링합니다.                                                          |
| `limit`        | 숫자                                                           | _(선택 사항)_ 검색할 신뢰선 수를 제한합니다. 결과 페이지가 더 많더라도 서버는 지정된 제한보다 적은 양을 반환할 수 있습니다. 10\~400 범위 내에 있어야 합니다. 기본값은 200입니다. |
| `marker`       | marker                                                       | _(선택 사항)_ 페이지를 매긴 이전 응답의 값입니다. 해당 응답이 중단된 데이터 검색을 재개합니다.                                                      |

다음 매개변수는 더 이상 사용되지 않으며 추후 공지 없이 제거될 수 있습니다: ledger 및 peer\_index.

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
        "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
        "lines": [
            {
                "account": "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
                "balance": "0",
                "currency": "ASP",
                "limit": "0",
                "limit_peer": "10",
                "quality_in": 0,
                "quality_out": 0
            },
            {
                "account": "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
                "balance": "0",
                "currency": "XAU",
                "limit": "0",
                "limit_peer": "0",
                "no_ripple": true,
                "no_ripple_peer": true,
                "quality_in": 0,
                "quality_out": 0
            },
            {
                "account": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
                "balance": "3.497605752725159",
                "currency": "USD",
                "limit": "5",
                "limit_peer": "0",
                "no_ripple": true,
                "quality_in": 0,
                "quality_out": 0
            }
        ]
    }
}
```
{% endtab %}

{% tab title="Second Tab" %}
```json
200 OK

{
    "result": {
        "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
        "lines": [
            {
                "account": "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
                "balance": "0",
                "currency": "ASP",
                "limit": "0",
                "limit_peer": "10",
                "quality_in": 0,
                "quality_out": 0
            },
            {
                "account": "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
                "balance": "0",
                "currency": "XAU",
                "limit": "0",
                "limit_peer": "0",
                "no_ripple": true,
                "no_ripple_peer": true,
                "quality_in": 0,
                "quality_out": 0
            },
            {
                "account": "rs9M85karFkCRjvc6KMWn8Coigm9cbcgcx",
                "balance": "0",
                "currency": "015841551A748AD2C1F76FF6ECB0CCCD00000000",
                "limit": "10.01037626125837",
                "limit_peer": "0",
                "no_ripple": true,
                "quality_in": 0,
                "quality_out": 0
            }
        ],
        "status": "success"
    }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 계정 주소와 신뢰줄 객체 배열이 포함됩니다. 구체적으로 결과 객체에는 다음 필드가 포함됩니다:

| `Field`                | 유형                                                                | 설명                                                                                                                                                                                                                                                    |
| ---------------------- | ----------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `account`              | 문자열                                                               | 이 요청에 해당하는 계정의 고유 주소입니다. 이는 신뢰선의 목적을 위한 "관점 계정"입니다.                                                                                                                                                                                                   |
| `lines`                | 배열                                                                | 아래에 설명된 대로 신뢰 라인 객체의 배열입니다. 신뢰 라인 수가 많은 경우 limit한 번에 최대 라인만 반환합니다.                                                                                                                                                                                    |
| `ledger_current_index` | 정수 - [원장 색인](https://xrpl.org/basic-data-types.html#ledger-index) | _(제공되거나 있는 경우 생략 `ledger_hash`) `ledger_index`해당_ 정보를 조회할 때 사용된 현재 오픈 원장의 원장 인덱스입니다.[![새로운 기능: Rippled 0.26.4-sp1](https://img.shields.io/badge/New%20in-rippled%200.26.4--sp1-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.26.4-sp1) |
| `ledger_index`         | 정수 - [원장 색인](https://xrpl.org/basic-data-types.html#ledger-index) | _( `ledger_current_index`대신 제공되는 경우 생략)_ 이 데이터를 검색할 때 사용된 원장 버전의 원장 인덱스입니다.[![새로운 기능: Rippled 0.26.4-sp1](https://img.shields.io/badge/New%20in-rippled%200.26.4--sp1-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.26.4-sp1)          |
| `ledger_hash`          | 문자열 - [해시](https://xrpl.org/basic-data-types.html#hashes)         | _(생략 가능)_ 이 데이터를 검색할 때 사용된 원장 버전을 식별하는 해시입니다.[![새로운 기능: Rippled 0.26.4-sp1](https://img.shields.io/badge/New%20in-rippled%200.26.4--sp1-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.26.4-sp1)                                        |
| `marker`               | marker                                                            | 응답이 페이지로 매겨졌음을 나타내는 서버 정의 값입니다. 이 통화가 중단된 곳에서 재개하려면 이를 다음 통화에 전달하세요. 이 페이지 이후에 추가 페이지가 없으면 생략됩니다.[![새로운 기능: 파문 0.26.4](https://img.shields.io/badge/New%20in-rippled%200.26.4-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.26.4)      |

각 신뢰 객체에는 다음 필드의 일부 조합이 있습니다:

| `Field`           | 유형       | 설명                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ----------------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `account`         | 문자열      | 이 신뢰선에 대한 상대방의 고유 주소입니다.                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `balance`         | 문자열      | 이 라인에 대해 현재 보유하고 있는 숫자 잔액을 나타냅니다. 양수 잔액은 관점 계정에 가치가 있음을 의미합니다. 마이너스 잔액은 관점 계정에 가치가 있다는 것을 의미합니다.                                                                                                                                                                                                                                                                                                                                          |
| `currency`        | 문자열      | 이 신뢰선이 보유할 수 있는 통화를 식별하는 통화 코드 입니다.                                                                                                                                                                                                                                                                                                                                                                                                       |
| `limit`           | 문자열      | 이 계정이 동료 계정에 지불할 의사가 있는 특정 통화의 최대 금액입니다.                                                                                                                                                                                                                                                                                                                                                                                                  |
| `limit_peer`      | 문자열      | 상대방 계정이 관점 계정에 지불할 의사가 있는 최대 통화 금액입니다.                                                                                                                                                                                                                                                                                                                                                                                                    |
| `quality_in`      | 부호 없는 정수 | 계정이 이 신뢰선에 들어오는 잔액을 평가하는 비율(10억 단위당 이 값의 비율)입니다. (예를 들어 5억이라는 값은 0.5:1 비율을 나타냅니다.) 특별한 경우로 0은 1:1 비율로 처리됩니다.                                                                                                                                                                                                                                                                                                                              |
| `quality_out`     | 부호 없는 정수 | 계정이 이 신뢰선이 나가는 잔액을 평가하는 비율(10억 단위당 이 값의 비율)입니다. (예를 들어 5억이라는 값은 0.5:1 비율을 나타냅니다.) 특별한 경우로 0은 1:1 비율로 처리됩니다.                                                                                                                                                                                                                                                                                                                               |
| `no_ripple`       | Boolean  | (생략 가능) 참이면 이 계정이 이 신뢰선에 대해 [No Ripple](https://xrpl.org/rippling.html)를 활성화한 것입니다. 거짓이면 이 계정은 [No Ripple](https://xrpl.org/rippling.html) 플래그를 비활성화했지만 기본 No Ripple 플래그도 비활성화 되어 있으므로 기본 상태로 간주되지 않습니다. 생략된 경우 계정은 이 신뢰 관계에 대해 No Ripple 플래그를 비활성화하고 기본 리플을 활성화한 상태입니다.[![업데이트: 파문 1.7.0](https://img.shields.io/badge/Updated%20in-rippled%201.7.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.7.0)                    |
| `no_ripple_peer`  | Boolean  | <p>(생략 가능) 참이면 피어 계정이 이  신뢰선에 대해 리플 없음 플래그를 활성화한 것입니다. 존재하고 거짓인 경우, 이 계정은 <a href="https://xrpl.org/rippling.html">No Ripple</a> 플래그를 비활성화했지만 기본 Ripple 플래그도 비활성화되어 있으므로 기본 상태로 간주되지 않습니다. 생략된 경우 계정은 이 신뢰 관계에 대해 리플 없음 플래그를 비활성화하고 기본 Ripple을 활성화한 상태입니다.<br><a href="https://github.com/ripple/rippled/releases/tag/1.7.0"><img src="https://img.shields.io/badge/Updated%20in-rippled%201.7.0-blue.svg" alt="업데이트: 파문 1.7.0"> </a></p> |
| `authorized`      | Boolean  | (생략 가능) 참이면 이 계정이 이 신뢰선을 승인한 것입니다. 기본값은 거짓입니다.                                                                                                                                                                                                                                                                                                                                                                                            |
| `peer_authorized` | Boolean  | (생략 가능) 참이면 피어 계정이 이 신뢰선을 승인한 것입니다. 기본값은 거짓입니다.                                                                                                                                                                                                                                                                                                                                                                                           |
| `freeze`          | Boolean  | (생략 가능) true이면 이 계정이 이 신뢰선을 동결했습니다. 기본값은 거짓입니다.                                                                                                                                                                                                                                                                                                                                                                                           |
| `freeze_peer`     | Boolean  | (생략 가능) true이면 피어 계정이 이 신뢰선을 동결했습니다. 기본값은 거짓입니다.                                                                                                                                                                                                                                                                                                                                                                                          |

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* actNotFound - 요청의 계정 필드에 지정된 주소가 장부에 있는 계정과 일치하지 않습니다.
* lgrNotFound - ledger\_hash 또는 ledger\_index로 지정한 ledger이 존재하지 않거나, 존재하지만 서버에 없는 경우입니다.
* actMalformed - 제공된 마커 필드가 허용되지 않는 경우.
