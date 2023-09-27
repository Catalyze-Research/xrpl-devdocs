# book\_offers

book\_offers 메소드는 오더북이라고도 하는 두 통화 간의 오퍼 목록을 검색합니다. 이 응답은 펀딩되지 않은 오퍼는 생략하고 남은 각 오퍼의 총액 중 현재 펀딩된 금액을 보고합니다.

## 요청 형식

요청 형식의 예시입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 4,
  "command": "book_offers",
  "taker": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
  "taker_gets": {
    "currency": "XRP"
  },
  "taker_pays": {
    "currency": "USD",
    "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B"
  },
  "limit": 10
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "book_offers",
    "params": [
        {
            "taker": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
            "taker_gets": {
                "currency": "XRP"
            },
            "taker_pays": {
                "currency": "USD",
                "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B"
            },
            "limit": 10
        }
    ]
}
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| `Field`        | 유형                                                          | 필수 여부 | 설명                                                                                                               |
| -------------- | ----------------------------------------------------------- | ----- | ---------------------------------------------------------------------------------------------------------------- |
| `taker_gets`   | 객체                                                          | 예     | 제안을 받는 계정이 금액이 없는 통화로 받게 될 자산입니다.                                                                                |
| `taker_pays`   | 객체                                                          | 예     | 제안을 받는 계정이 금액 없이 통화로 지불할 자산입니다.                                                                                  |
| `ledger_hash`  | [해시](https://xrpl.org/basic-data-types.html#hashes)         | 아니요   | 사용할 원장 버전에 대한 20바이트 16진수 문자열입니다. (원장 지정 참조)                                                                      |
| `ledger_index` | [원장지수](https://xrpl.org/basic-data-types.html#ledger-index) | 아니요   | 사용할 원장의 원장 인덱스 또는 자동으로 원장을 선택하는 단축 문자열입니다. (원장 지정 참조)                                                            |
| `limit`        | 숫자                                                          | 아니요   | 반환할 최대 제안 수입니다. 응답에는 더 적은 수의 결과가 포함될 수 있습니다.                                                                     |
| `taker`        | 문자열                                                         | 아니요   | 관점으로 사용할 계정의 주소 입니다. 자금이 조달되지 않은 경우에도 응답에는 이 계정의 제안이 포함됩니다. (이를 사용하여 오더북에서 귀하의 제안보다 높거나 낮은 제안이 무엇인지 확인할 수 있습니다.) |

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 11,
  "status": "success",
  "type": "response",
  "result": {
    "ledger_current_index": 7035305,
    "offers": [
      {
        "Account": "rM3X3QSr8icjTGpaF52dozhbT2BZSXJQYM",
        "BookDirectory": "7E5F614417C2D0A7CEFEB73C4AA773ED5B078DE2B5771F6D55055E4C405218EB",
        "BookNode": "0000000000000000",
        "Flags": 0,
        "LedgerEntryType": "Offer",
        "OwnerNode": "0000000000000AE0",
        "PreviousTxnID": "6956221794397C25A53647182E5C78A439766D600724074C99D78982E37599F1",
        "PreviousTxnLgrSeq": 7022646,
        "Sequence": 264542,
        "TakerGets": {
          "currency": "EUR",
          "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
          "value": "17.90363633316433"
        },
        "TakerPays": {
          "currency": "USD",
          "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
          "value": "27.05340557506234"
        },
        "index": "96A9104BF3137131FF8310B9174F3B37170E2144C813CA2A1695DF2C5677E811",
        "quality": "1.511056473200875"
      },
      {
        "Account": "rhsxKNyN99q6vyYCTHNTC1TqWCeHr7PNgp",
        "BookDirectory": "7E5F614417C2D0A7CEFEB73C4AA773ED5B078DE2B5771F6D5505DCAA8FE12000",
        "BookNode": "0000000000000000",
        "Flags": 131072,
        "LedgerEntryType": "Offer",
        "OwnerNode": "0000000000000001",
        "PreviousTxnID": "8AD748CD489F7FF34FCD4FB73F77F1901E27A6EFA52CCBB0CCDAAB934E5E754D",
        "PreviousTxnLgrSeq": 7007546,
        "Sequence": 265,
        "TakerGets": {
          "currency": "EUR",
          "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
          "value": "2.542743233917848"
        },
        "TakerPays": {
          "currency": "USD",
          "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
          "value": "4.19552633596446"
        },
        "index": "7001797678E886E22D6DE11AF90DF1E08F4ADC21D763FAFB36AF66894D695235",
        "quality": "1.65"
      }
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
        "ledger_current_index": 8696243,
        "offers": [],
        "status": "success",
        "validated": false
    }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`                | 유형                                                          | 설명                                                                              |
| ---------------------- | ----------------------------------------------------------- | ------------------------------------------------------------------------------- |
| `ledger_current_index` | [원장지수](https://xrpl.org/basic-data-types.html#ledger-index) | (ledger\_current\_index가 제공된 경우 생략) 이 정보를 검색하는 데 사용된 현재 진행 중인 원장 버전의 원장 인덱스입니다. |
| `ledger_index`         | [원장지수](https://xrpl.org/basic-data-types.html#ledger-index) | 요청에 따라 이 데이터를 검색할 때 사용된 원장 버전의 원장 인덱스(ledger\_current\_index가 제공된 경우 생략).       |
| `ledger_hash`          | [해시](https://xrpl.org/basic-data-types.html#hashes)         | _(생략 가능)_ 요청에 따라 이 데이터를 검색할 때 사용된 원장 버전의 식별 해시입니다.                              |
| `offers`               | 정렬                                                          | 오퍼 객체의 배열로, 각 오퍼 객체의 필드가 있습니다.                                                  |

표준 오퍼 필드 외에도 오퍼 배열의 멤버에 다음 필드가 포함될 수 있습니다:

| `Field`             | 유형                                                                          | 설명                                                                                                                                           |
| ------------------- | --------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| `owner_funds`       | 문자열                                                                         | `TakerGets`제안을 하는 쪽이 거래할 수 있는 통화 금액입니다. (XRP는 드롭으로 표시되며 다른 모든 통화는 소수 값으로 표시됩니다.) 거래자가 동일한 책에 여러 제안을 가지고 있는 경우 가장 높은 순위의 제안에만 이 필드가 포함됩니다.    |
| `taker_gets_funded` | [통화 금액](https://xrpl.org/basic-data-types.html#specifying-currency-amounts) | _(부분 자금 조달 제안에만 포함됨)_ 제안의 자금 조달 상태를 고려하여 테이커가 받을 수 있는 최대 통화 금액입니다.                                                                           |
| `taker_pays_funded` | [통화 금액](https://xrpl.org/basic-data-types.html#specifying-currency-amounts) | _(부분적으로 자금을 조달한 제안에만 포함됨)_ 제안의 자금 상태를 고려하여 테이커가 지불할 최대 통화 금액입니다.                                                                             |
| `quality`           | 문자열                                                                         | 환율은 `taker_pays`를 `taker_gets`으로 나눈 비율입니다. 공평성을 위해 동일한 품질의 오퍼는 자동으로 선입선출 방식으로 처리됩니다. (즉, 여러 사람이 같은 환율로 환전하겠다고 오퍼를 제출하면 가장 오래된 오퍼가 먼저 채택됩니다.) |

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* lgrNotFound - ledger\_hash 또는 ledger\_index에 지정된 ledger이 존재하지 않거나, 존재하지만 서버에 없습니다.
* srcCurMalformed - 요청의 taker\_pays 필드의 형식이 올바르지 않습니다.
* dstAmtMalformed - 요청의 taker\_gets 필드의 형식이 올바르지 않습니다.
* srcIsrMalformed - 요청의 taker\_pays 필드의 발급자 필드가 유효하지 않습니다.
* dstIsrMalformed - 요청의 taker\_gets 필드의 발행자 필드가 유효하지 않습니다.
* badMarket - 원하는 오더북이 존재하지 않습니다(예: 자체적으로 통화를 교환하는 오퍼).
