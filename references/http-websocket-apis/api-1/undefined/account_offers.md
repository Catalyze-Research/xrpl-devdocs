# account\_offers

account\_offers 메소드는 특정 ledger 버전 기준으로 미결된 특정 계정의 오퍼 목록을 검색합니다.

## 요청 형식

요청 형식의 예시입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 9,
  "command": "account_offers",
  "account": "rpP2JgiMyTF5jR5hLG3xHCPi1knBb1v9cM"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "account_offers",
    "params": [
        {
            "account": "rpP2JgiMyTF5jR5hLG3xHCPi1knBb1v9cM"
        }
    ]
}
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함될 수 있습니다:

| 필드             | 유형                                                           | 필수의? | 설명                                                                                           |
| -------------- | ------------------------------------------------------------ | ---- | -------------------------------------------------------------------------------------------- |
| `account`      | 문자열 - [주소](https://xrpl.org/basic-data-types.html#addresses) | 예    | 이 계정에서 제공한 제안을 찾아보세요.                                                                        |
| `ledger_hash`  | 문자열                                                          | 아니요  | 사용할 원장 버전에 대한 20바이트 16진수 문자열입니다. (원장 지정하기 참조)                                                |
| `ledger_index` | 숫자 또는 문자열                                                    | 아니요  | 사용할 원장의 원장 인덱스 또는 원장을 자동으로 선택하기 위한 바로가기 문자열입니다. (원장 지정하기 참조)                                 |
| `limit`        | 숫자                                                           | 아니요  | 검색할 오퍼의 수를 제한합니다. 서버는 이보다 적은 수의 결과를 반환할 수 있습니다. 10에서 400까지의 포괄적인 범위 내에 있어야 합니다. 기본값은 200입니다. |
| `marker`       | marker                                                       | 아니요  | 이전 페이지 매김된 응답의 값입니다. 해당 응답이 중단된 부분부터 데이터 검색을 다시 시작합니다.                                       |

다음 매개변수는 더 이상 사용되지 않습니다: ledger, strict.

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 9,
  "status": "success",
  "type": "response",
  "result": {
    "account": "rpP2JgiMyTF5jR5hLG3xHCPi1knBb1v9cM",
    "ledger_current_index": 18539550,
    "offers": [
      {
        "flags": 0,
        "quality": "0.00000000574666765650638",
        "seq": 6577664,
        "taker_gets": "33687728098",
        "taker_pays": {
          "currency": "EUR",
          "issuer": "rhub8VRN55s94qWKDv6jmDy1pUykJzF3wq",
          "value": "193.5921774819578"
        }
      },
      {
        "flags": 0,
        "quality": "7989247009094510e-27",
        "seq": 6572128,
        "taker_gets": "2361918758",
        "taker_pays": {
          "currency": "XAU",
          "issuer": "rrh7rf1gV2pXAoqA8oYbpHd8TKv5ZQeo67",
          "value": "0.01886995237307572"
        }
      },
      ... trimmed for length ...
    ],
    "validated": false
  }
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
200 OK

{
    "result": {
        "account": "rpP2JgiMyTF5jR5hLG3xHCPi1knBb1v9cM",
        "ledger_current_index": 18539596,
        "offers": [{
            "flags": 0,
            "quality": "0.000000007599140009999998",
            "seq": 6578020,
            "taker_gets": "29740867287",
            "taker_pays": {
                "currency": "USD",
                "issuer": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
                "value": "226.0050145327418"
            }
        }, {
            "flags": 0,
            "quality": "7989247009094510e-27",
            "seq": 6572128,
            "taker_gets": "2361918758",
            "taker_pays": {
                "currency": "XAU",
                "issuer": "rrh7rf1gV2pXAoqA8oYbpHd8TKv5ZQeo67",
                "value": "0.01886995237307572"
            }
        }, {
            "flags": 0,
            "quality": "0.00000004059594001318974",
            "seq": 6576905,
            "taker_gets": "3892952574",
            "taker_pays": {
                "currency": "CNY",
                "issuer": "rKiCet8SdvWxPXnAgYarFUXMh1zCPz432Y",
                "value": "158.0380691682966"
            }
        },

        ...

        ],
        "status": "success",
        "validated": false
    }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`                | 유형                                                                | 설명                                                                                                                                                                                                                                                             |
| ---------------------- | ----------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `account`              | 문자열                                                               | 오퍼를 만든 계정을 식별하는 고유 주소.                                                                                                                                                                                                                                         |
| `offers`               | 배열                                                                | 객체의 배열로, 각 객체는 요청된 원장 버전 기준으로 이 계정이 만든 오퍼 중 미결 상태인 오퍼를 나타냅니다. 오퍼 수가 많으면 한 번에 한도까지만 반환합니다.                                                                                                                                                                      |
| `ledger_current_index` | 번호 - [원장 색인](https://xrpl.org/basic-data-types.html#ledger-index) | (ledger\_hash 또는 ledger\_index를 제공한 경우 생략) 현재 진행 중인 원장 버전의 원장 인덱스로, 이 데이터를 검색할 때 사용되었습니다. 변경:[![새로운 기능: Rippled 0.26.4-sp1](https://img.shields.io/badge/New%20in-rippled%200.26.4--sp1-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.26.4-sp1) |
| `ledger_index`         | 번호 - [원장 색인](https://xrpl.org/basic-data-types.html#ledger-index) | (ledger\_current\_index가 대신 제공된 경우 생략) 요청에 따라 이 데이터를 검색할 때 사용된 원장 버전의 원장 인덱스입니다. 변경:[![새로운 기능: Rippled 0.26.4-sp1](https://img.shields.io/badge/New%20in-rippled%200.26.4--sp1-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.26.4-sp1)          |
| `ledger_hash`          | 문자열 - [해시](https://xrpl.org/basic-data-types.html#hashes)         | (생략 가능) 이 데이터를 검색할 때 사용된 원장 버전의 식별 해시입니다.[![새로운 기능: Rippled 0.26.4-sp1](https://img.shields.io/badge/New%20in-rippled%200.26.4--sp1-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.26.4-sp1)                                                     |
| `marker`               | [채점자](https://xrpl.org/markers-and-pagination.html)               | (생략 가능) 응답의 페이지 매김을 나타내는 서버 정의 값입니다. 이 값을 다음 호출에 전달하여 이 호출이 중단된 부분부터 다시 시작합니다. 이 호출 이후에 정보 페이지가 없는 경우 생략됩니다.[![새로운 기능: 파문 0.26.4](https://img.shields.io/badge/New%20in-rippled%200.26.4-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.26.4)    |

&#x20;각 오퍼 객체에는 다음 필드가 포함됩니다:

| `Field`      | 유형        | 설명                                                                                                                                                                                                                                                                                                 |
| ------------ | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `flags`      | 부호 없는 정수  | 이 오퍼 항목에 대해 비트 플래그로 설정된 옵션입니다.                                                                                                                                                                                                                                                                     |
| `seq`        | 부호 없는 정수  | 이 항목을 생성한 트랜잭션의 시퀀스 번호입니다. (거래 [일련번호는](https://xrpl.org/basic-data-types.html#account-sequence) 계정과 관련이 있습니다.)                                                                                                                                                                                     |
| `taker_gets` | 문자열 또는 객체 | 객체 오퍼를 수락하는 계정이 받는 금액으로, XRP로 금액을 나타내는 문자열 또는 통화 지정 객체입니다. (통화 금액 지정하기 참조)                                                                                                                                                                                                                         |
| `taker_pays` | 문자열 또는 객체 | 객체 오퍼를 수락하는 계정이 제공하는 금액으로, XRP 단위의 금액을 나타내는 문자열 또는 통화 지정 객체입니다. (통화 금액 지정하기 참조)                                                                                                                                                                                                                    |
| `quality`    | 문자열       | 오퍼의 환율로, 원래 `taker_pays`를 원래 `taker_gets`으로 나눈 비율입니다. 오퍼를 실행할 때 가장 유리한(가장 낮은) 품질을 가진 오퍼가 먼저 소비되며, 동일한 품질을 가진 오퍼는 가장 오래된 것부터 최신 것 순으로 실행됩니다. 새로운 기능:[![새로운 기능: 파문 0.29.0](https://img.shields.io/badge/New%20in-rippled%200.29.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.29.0) |
| `expiration` | 부호 없는 정수  | (생략 가능) Ripple 에포크 이후 초 단위로 이 오퍼가 미펀딩으로 간주되는 시간입니다. 참조: 오퍼 만료. 신규:[![새로운 기능: 파문 0.30.1](https://img.shields.io/badge/New%20in-rippled%200.30.1-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.30.1)                                                                                  |

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* actNotFound - 요청의 계정 필드에 지정된 주소가 장부에 있는 계정과 일치하지 않습니다.
* lgrNotFound - ledger\_hash 또는 ledger\_index로 지정한 ledger이 존재하지 않거나, 존재하지만 서버에 해당 ledger이 없습니다.
* actMalformed - 제공된 마커 필드가 올바르지 않습니다.
