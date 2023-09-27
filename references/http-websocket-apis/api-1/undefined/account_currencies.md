# account\_currencies

account\_currencies 커맨드라인은 신뢰선을 기준으로 계정이 보내거나 받을 수 있는 화폐 목록을 검색합니다. (이 목록은 완전히 확인된 목록은 아니지만 사용자 인터페이스를 채우는 데 사용할 수 있습니다.)

## 요청 형식

요청 형식의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "command": "account_currencies",
    "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
    "ledger_index": "validated"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "account_currencies",
    "params": [
        {
            "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
            "account_index": 0,
            "ledger_index": "validated"
        }
    ]
}
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| `Field`        | 유형                                                           | 필수 여부 | 설명                                                                                                                                                                                     |
| -------------- | ------------------------------------------------------------ | ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `account`      | 문자열 - [주소](https://xrpl.org/basic-data-types.html#addresses) | 예     | 이 계정에서 보내거나 받을 수 있는 화폐를 찾아보세요.[![업데이트: 파문 1.11.0](https://img.shields.io/badge/Updated%20in-rippled%201.11.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.11.0)        |
| `ledger_hash`  | 문자열                                                          | 아니요   | 사용할 원장 버전에 대한 20바이트 16진수 문자열입니다. ([Specifying Ledgers](https://xrpl.org/basic-data-types.html#specifying-ledgers) 참조)                                                                  |
| `ledger_index` | 숫자 또는 문자열                                                    | 아니요   | 사용할 원장의 [원장 인덱스](https://xrpl.org/basic-data-types.html#ledger-index) 또는 자동으로 원장을 선택하는 단축 문자열입니다. ([Specifying Ledgers](https://xrpl.org/basic-data-types.html#specifying-ledgers) 참조) |

다음 필드는 더 이상 사용되지 않으므로 제공하지 않아야 합니다: account\_index, strict.

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "result": {
        "ledger_index": 11775844,
        "receive_currencies": [
            "BTC",
            "CNY",
            "DYM",
            "EUR",
            "JOE",
            "MXN",
            "USD",
            "015841551A748AD2C1F76FF6ECB0CCCD00000000"
        ],
        "send_currencies": [
            "ASP",
            "BTC",
            "CHF",
            "CNY",
            "DYM",
            "EUR",
            "JOE",
            "JPY",
            "MXN",
            "USD"
        ],
        "validated": true
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
        "ledger_index": 11775823,
        "receive_currencies": [
            "BTC",
            "CNY",
            "DYM",
            "EUR",
            "JOE",
            "MXN",
            "USD",
            "015841551A748AD2C1F76FF6ECB0CCCD00000000"
        ],
        "send_currencies": [
            "ASP",
            "BTC",
            "CHF",
            "CNY",
            "DYM",
            "EUR",
            "JOE",
            "JPY",
            "MXN",
            "USD"
        ],
        "status": "success",
        "validated": true
    }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`              | 유형                                                                | 설명                                                |
| -------------------- | ----------------------------------------------------------------- | ------------------------------------------------- |
| `ledger_hash`        | 문자열 - [해시](https://xrpl.org/basic-data-types.html#hashes)         | (생략 가능) 이 데이터를 검색하는 데 사용되는 원장 버전의 식별 해시(16진수)입니다. |
| `ledger_index`       | 정수 - [원장 색인](https://xrpl.org/basic-data-types.html#ledger-index) | 이 데이터를 검색하는 데 사용되는 원장 버전의 원장 인덱스입니다.              |
| `receive_currencies` | 문자열 배열                                                            | 이 계정이 받을 수 있는 통화에 대한 화폐 코드 배열 입니다.                |
| `send_currencies`    | 문자열 배열                                                            | 이 계정이 보낼 수 있는 통화에 대한 화폐 코드 배열 입니다.                |
| `validated`          | Boolean                                                           | `true`이면 데이터는 검증된 원장에서 나온 것입니다.                   |

{% hint style="info" %}
Note:

계정이 송금하거나 받을 수 있는 통화는 신뢰선 확인을 기반으로 정의됩니다. 계정에 특정 통화에 대한 신뢰선이 있고 잔액을 늘릴 수 있는 충분한 여유가 있는 경우 해당 통화를 받을 수 있습니다. 신뢰선의 잔액이 감소할 수 있는 경우 계정은 해당 통화를 보낼 수 있습니다. 이 방법은 신뢰선이 동결되었는지 또는 승인되었는지는 확인하지 않습니다.
{% endhint %}

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* actNotFound - 요청의 계정 필드에 지정된 주소가 장부에 있는 계정과 일치하지 않습니다.
* lgrNotFound - ledger\_hash 또는 ledger\_index로 지정한 ledger이 존재하지 않거나, 존재하지만 서버에 없습니다.
