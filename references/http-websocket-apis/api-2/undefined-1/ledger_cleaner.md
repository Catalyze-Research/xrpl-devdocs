# ledger\_cleaner

ledger\_cleaner 명령은 rippled의 ledger 데이터베이스에서 손상을 찾아 복구할 수 있는 비동기 유지보수 프로세스인 [Ledger Cleaner](https://github.com/ripple/rippled/blob/f313caaa73b0ac89e793195dcc2a5001786f916f/src/ripple/app/ledger/README.md#the-ledger-cleaner)를 제어합니다.

ledger\_cleaner 메소드는 권한이 없는 사용자가 실행할 수 없는 관리자 메소드입니다.

## 요청 형식

요청 형식의 예시입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "command": "ledger_cleaner",
    "max_ledger": 13818756,
    "min_ledger": 13818000,
    "stop": false
}
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| Field        | 유형                                                                | 설명                                                                                      |
| ------------ | ----------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| ledger       | 번호 - [원장 색인](https://xrpl.org/basic-data-types.html#ledger-index) | _(선택사항)_ 제공된 경우 지정된 원장만 확인하고 수정합니다.                                                     |
| max\_ledger  | 번호 - [원장 색인](https://xrpl.org/basic-data-types.html#ledger-index) | _(선택 사항)_ 원장 인덱스가 이보다 작거나 같은 원장을 확인하도록 원장 클리너를 구성합니다.                                   |
| min\_ledger  | 번호 - [원장 색인](https://xrpl.org/basic-data-types.html#ledger-index) | _(선택 사항)_ 원장 인덱스가 이보다 높거나 같은 원장을 확인하도록 원장 클리너를 구성합니다.                                   |
| full         | Boolean                                                           | (선택 사항) true이면 지정된 원장의 원장 상태 개체와 트랜잭션을 수정합니다. 기본값은 false입니다. 원장이 제공되면 자동으로 true로 설정됩니다. |
| fix\_txns    | Boolean                                                           | (선택 사항) 참이면 지정된 원장의 트랜잭션을 수정합니다. 제공된 경우 full을 재정의합니다.                                   |
| check\_nodes | Boolean                                                           | (선택 사항) 참이면 지정된 원장의 원장 상태 개체를 수정합니다. 제공된 경우 full을 재정의합니다.                               |
| stop         | Boolean                                                           | (선택 사항) 참이면 원장 클리너를 비활성화합니다.                                                            |

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="JSON-RPC" %}
```json
200 OK

{
   "result" : {
      "message" : "Cleaner configured",
      "status" : "success"
   }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| Field   | 유형  | 설명                    |
| ------- | --- | --------------------- |
| message | 문자열 | Cleaner configured 성공 |

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* internal 매개 변수가 잘못 지정된 경우. (이것은 버그입니다. 의도된 오류 코드는 invalidParams입니다.)
