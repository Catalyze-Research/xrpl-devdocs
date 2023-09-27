# noripple\_check

noripple\_check 명령은 계정의 기본 Ripple 필드 상태와 신뢰선의 No Ripple 플래그를 권장 설정과 비교하여 빠르게 확인할 수 있는 방법을 제공합니다.

## 요청 형식

요청 형식의 예시입니다:

{% hint style="info" %}
Note:

이 메소드에 대한 커맨드라인 구문은 없습니다. 대신 json 메소드를 사용하여 커맨드라인에서 이 메소드에 액세스할 수 있습니다.
{% endhint %}

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": 0,
    "command": "noripple_check",
    "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
    "role": "gateway",
    "ledger_index": "current",
    "limit": 2,
    "transactions": true
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "noripple_check",
    "params": [
        {
            "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
            "ledger_index": "current",
            "limit": 2,
            "role": "gateway",
            "transactions": true
        }
    ]
}
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| `Field`        | 유형              | 설명                                                                                                                                                                         |
| -------------- | --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `account`      | 문자열             | 계정의 고유 식별자(가장 일반적으로 계정의 주소)입니다.                                                                                                                                            |
| `role`         | 문자열             | 주소가 게이트웨이 또는 사용자를 가리키는지 여부입니다. 권장 사항은 계정의 역할에 따라 다릅니다. 발급자는 기본 Ripple을 사용하도록 설정해야 하며 모든 신뢰선에서 No Ripple을 비활성화해야 합니다. 사용자는 기본 Ripple을 비활성화하고 모든 신뢰선에서 No Ripple을 활성화해야 합니다. |
| `transactions` | Boolean         | (선택 사항) 참이면 문제를 해결하기 위해 서명하고 제출할 수 있는 제안 트랜잭션 배열을 JSON 객체로 포함합니다. 기본값은 거짓입니다.                                                                                              |
| `limit`        | 부호 없는 정수        | (선택 사항) 결과에 포함할 신뢰 줄 문제의 최대 수입니다. 기본값은 300입니다.                                                                                                                             |
| `ledger_hash`  | 문자열             | (선택 사항) 사용할 원장 버전에 대한 20바이트 16진수 문자열입니다. (원장 지정하기 참조)                                                                                                                      |
| `ledger_index` | 문자열 또는 부호 없는 정수 | (선택 사항) 사용할 원장의 원장 인덱스 또는 원장을 자동으로 선택하기 위한 바로가기 문자열입니다. (원장 지정하기 참조)                                                                                                       |

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 0,
  "status": "success",
  "type": "response",
  "result": {
    "ledger_current_index": 14342939,
    "problems": [
      "You should immediately set your default ripple flag",
      "You should clear the no ripple flag on your XAU line to r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
      "You should clear the no ripple flag on your USD line to rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q"
    ],
    "transactions": [
      {
        "Account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
        "Fee": 10000,
        "Sequence": 1406,
        "SetFlag": 8,
        "TransactionType": "AccountSet"
      },
      {
        "Account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
        "Fee": 10000,
        "Flags": 262144,
        "LimitAmount": {
          "currency": "XAU",
          "issuer": "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
          "value": "0"
        },
        "Sequence": 1407,
        "TransactionType": "TrustSet"
      },
      {
        "Account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
        "Fee": 10000,
        "Flags": 262144,
        "LimitAmount": {
          "currency": "USD",
          "issuer": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
          "value": "5"
        },
        "Sequence": 1408,
        "TransactionType": "TrustSet"
      }
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
        "ledger_current_index": 14380381,
        "problems": [
            "You should immediately set your default ripple flag",
            "You should clear the no ripple flag on your XAU line to r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
            "You should clear the no ripple flag on your USD line to rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q"
        ],
        "status": "success",
        "transactions": [
            {
                "Account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                "Fee": 10000,
                "Sequence": 1406,
                "SetFlag": 8,
                "TransactionType": "AccountSet"
            },
            {
                "Account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                "Fee": 10000,
                "Flags": 262144,
                "LimitAmount": {
                    "currency": "XAU",
                    "issuer": "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
                    "value": "0"
                },
                "Sequence": 1407,
                "TransactionType": "TrustSet"
            },
            {
                "Account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                "Fee": 10000,
                "Flags": 262144,
                "LimitAmount": {
                    "currency": "USD",
                    "issuer": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
                    "value": "5"
                },
                "Sequence": 1408,
                "TransactionType": "TrustSet"
            }
        ],
        "validated": false
    }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`                | 유형 | 설명                                                                                                                                                              |
| ---------------------- | -- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ledger_current_index` | 숫자 | 이 결과를 계산하는 데 사용된 원장의 원장 인덱스 번호입니다.                                                                                                                              |
| `problems`             | 배열 | 사람이 읽을 수 있는 문제 설명이 포함된 문자열 배열입니다. 여기에는 계정의 기본 Ripple 설정이 권장되지 않는 경우 최대 하나의 항목과 No Ripple 설정이 권장되지 않는 신뢰선에 대한 최대 제한 항목이 포함됩니다.                                   |
| `transactions`         | 배열 | (생략 가능) 요청에서 트랜잭션을 참으로 지정한 경우, 이것은 JSON 객체 배열이며, 각 객체는 설명된 문제 중 하나를 해결해야 하는 트랜잭션의 JSON 형식입니다. 이 배열의 길이는 문제 배열과 동일하며 각 항목은 해당 배열의 동일한 인덱스에 설명된 문제를 해결하기 위한 것입니다. |

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* actNotFound - 요청의 계정 필드에 지정된 주소가 장부에 있는 계정과 일치하지 않습니다.
* lgrNotFound - ledger\_hash 또는 ledger\_index로 지정한 ledger이 존재하지 않거나, 존재하지만 서버에 없습니다.
