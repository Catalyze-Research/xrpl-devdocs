# 구독 취소

배열구독 취소 명령은 서버에 특정 구독 또는 구독 집합에 대한 메시지 전송을 중지하도록 지시합니다.

## 요청 형식

요청 형식의 예시입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": "Unsubscribe a lot of stuff",
    "command": "unsubscribe",
    "streams": ["ledger","server","transactions","transactions_proposed"],
    "accounts": ["rrpNnNLKrartuEqfJGpqyDwPj1AFPg9vn1"],
    "accounts_proposed": ["rrpNnNLKrartuEqfJGpqyDwPj1AFPg9vn1"],
    "books": [
        {
            "taker_pays": {
                "currency": "XRP"
            },
            "taker_gets": {
                "currency": "USD",
                "issuer": "rUQTpMqAF5jhykj4FExVeXakrZpiKF6cQV"
            },
            "both": true
        }
    ]
}
```
{% endtab %}
{% endtabs %}

요청의 매개변수는 종료할 구독을 대신 정의하는 데 사용된다는 점을 제외하면 구독 메소드의 매개변수와 거의 동일하게 지정됩니다. 매개 변수는 다음과 같습니다:

| `Field`             | 유형 | 필수 여부 | 설명                                                                                                                                                         |
| ------------------- | -- | ----- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `streams`           | 배열 | 아니요   | `ledger`, `server`, `transactions`,  `transactions_proposed` 등 구독을 취소할 일반 스트림의 문자열 이름 배열입니다.                                                               |
| `accounts`          | 배열 | 아니요   | [XRP Ledger](https://xrpl.org/base58-encodings.html)의 base58 형식으로 되어 있습니다. (이전에 해당 계정을 특별히 구독한 경우에만 해당 메시지를 중지합니다. 일반 트랜잭션 스트림에서 계정을 필터링하는 데는 사용할 수 없습니다.) |
| `accounts_proposed` | 배열 | 아니요   | `accounts`과 유사하지만 아직 검증되지 않은 트랜잭션이 포함된 `accounts_propose` 구독에 대한 배열입니다.                                                                                    |
| `books`             | 배열 | 아니요   | 아래에 설명된 대로 구독을 취소할 오더북을 정의하는 객체 배열입니다.                                                                                                                     |

rt\_accounts 및 url 매개변수와 rt\_transactions 스트림 이름은 더 이상 사용되지 않으며 별도의 통지 없이 제거될 수 있습니다.

책 배열의 객체는 모든 필드가 없다는 점을 제외하면 구독의 객체와 거의 비슷하게 정의됩니다. 포함되는 필드는 다음과 같습니다:

| `Field`      | 유형      | 필수의? | 설명                                                               |
| ------------ | ------- | ---- | ---------------------------------------------------------------- |
| `taker_gets` | 객체      | 아니요  | 오퍼를 받는 계정이 받을 통화를 통화 및 발행자 필드가 있는 객체로 지정합니다. XRP의 경우 발행자는 생략합니다. |
| `taker_pays` | 객체      | 아니요  | 오퍼를 받는 계정이 받을 통화를 통화 및 발행자 필드가 있는 객체로 지정합니다.                     |
| `both`       | Boolean | 아니요  | 참이면 호가창의 양쪽 모두에 대한 청약이 제거됩니다.                                    |

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": "Unsubscribe a lot of stuff",
    "result": {},
    "status": "success",
    "type": "response"
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며 필드가 포함되지 않은 성공적인 결과입니다.

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* 권한 없음 - 요청에 URL 필드가 포함되어 있지만 관리자로 연결되어 있지 않습니다.
* 잘못된 스트림 - 요청의 스트림 필드 형식이 올바르지 않습니다.
* 잘못된 계정 - 요청의 계정 또는 계정\_제안 필드에 있는 주소 중 하나가 올바른 형식의 XRP 레저 주소가 아닙니다.

{% hint style="info" %}
Note:

글로벌 ledger에 아직 항목이 없는 주소의 스트림을 구독하여 해당 주소에 펀딩이 이루어질 때 메시지를 받을 수 있습니다.
{% endhint %}

* srcCurMalformed - 요청에 있는 도서 필드의 하나 이상의 테이커\_페이즈 하위 필드의 형식이 올바르게 지정되지 않았습니다.
* dstAmtMalformed - 요청에 있는 도서 필드의 하나 이상의 taker\_gets 하위 필드 형식이 올바르지 않습니다.
* srcIsrMalformed - 요청에 있는 도서 필드의 하나 이상의 taker\_pays 하위 필드 중 발행자 필드가 유효하지 않습니다.
* dstIsrMalformed - 요청에 있는 도서 필드의 하나 이상의 taker\_gets 하위 필드 중 하나 이상의 발행자 필드가 유효하지 않습니다.
* badMarket - 예약 필드에 있는 하나 이상의 원하는 오더북이 존재하지 않습니다(예: 자체 통화 교환 오퍼).
