# 오류 형식

문오류가 발생할 수 있는 모든 가능한 방법을 나열하는 것은 불가능합니다. 일부는 전송 계층에서 발생할 수 있으며(예: 네트워크 연결 끊김), 이 경우 사용 중인 클라이언트와 전송에 따라 결과가 달라집니다. 그러나 rippled 서버가 요청을 성공적으로 수신하면 표준화된 오류 형식으로 응답하려고 시도합니다.

{% hint style="info" %}
Caution:

요청에 오류가 발생하면 오류를 디버깅할 수 있도록 전체 요청이 응답의 일부로 다시 복사됩니다. 그러나 여기에는 요청의 일부로 전달된 모든 비밀도 포함됩니다. 오류 메시지를 공유할 때는 실수로 중요한 계정 비밀번호가 다른 사람에게 노출되지 않도록 각별히 주의하세요.
{% endhint %}

몇 가지 오류 예시:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 3,
  "status": "error",
  "type": "response",
  "error": "ledgerIndexMalformed",
  "request": {
    "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
    "command": "account_info",
    "id": 3,
    "ledger_index": "-",
    "strict": true
  }
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
HTTP Status: 200 OK

{
    "result": {
        "error": "ledgerIndexMalformed",
        "request": {
            "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
            "command": "account_info",
            "ledger_index": "-",
            "strict": true
        },
        "status": "error"
    }
}
```
{% endtab %}
{% endtabs %}

## WebSocket 형식 <a href="#websocket-format" id="websocket-format"></a>

| `Field`       | 유형   | 설명                                                               |
| ------------- | ---- | ---------------------------------------------------------------- |
| `id`          | (변수) | 이 응답을 요청한 웹 소켓 요청에 제공된 ID.                                       |
| `status`      | 문자열  | `"error"`요청으로 인해 오류가 발생한 경우.                                     |
| `type`        | 문자열  | 일반적으로 `"response"`명령에 대한 성공적인 응답을 나타냅니다.                         |
| `error`       | 문자열  | 발생한 오류 유형에 대한 고유 코드.                                             |
| `request`     | 객체   | 이 오류를 발생시킨 요청의 사본(JSON 형식)입니다. **주의:** 요청에 비밀이 포함된 경우 여기에 복사됩니다! |
| `api_version` | 숫자   | (생략 가능)`api_version` 요청 시 명시한 내용 입니다.                            |

## JSON-RPC 형식

일부 JSON-RPC 요청은 HTTP 계층에서 오류 코드와 함께 응답합니다. 이러한 경우 응답 본문에는 일반 텍스트 설명이 포함됩니다. 예를 들어 메소드 매개변수에 명령을 지정하는 것을 잊어버린 경우 다음과 같은 응답이 표시됩니다:

```
HTTP Status: 400 Bad Request
Null method
```

HTTP 상태 코드 200 OK와 함께 반환되는 다른 오류의 경우 응답은 다음 필드를 포함하여 JSON 형식으로 형식이 지정됩니다:

| `Field`          | 유형  | 설명                                                                                                                     |
| ---------------- | --- | ---------------------------------------------------------------------------------------------------------------------- |
| `result`         | 객체  | 쿼리에 대한 응답을 포함하는 객체.                                                                                                    |
| `result.error`   | 문자열 | 발생한 오류 유형에 대한 고유 코드.                                                                                                   |
| `result.status`  | 문자열 | `"error"`요청으로 인해 오류가 발생한 경우.                                                                                           |
| `result.request` | 객체  | 이 오류를 발생시킨 요청의 사본(JSON 형식)입니다. **주의:** 요청에 계정 비밀이 포함된 경우 여기에 복사됩니다! **참고:** 요청은 수행된 요청에 관계없이 WebSocket 형식으로 다시 형식화됩니다. |

## 범용 오류

모든 메소드는 오류 코드에 대해 다음 값 중 하나를 반환할 수 있습니다:

* amendmentBlocked - 서버가 수정이 차단되었으며, XRP 레저 네트워크와 동기화 상태를 유지하려면 최신 버전으로 업데이트해야 합니다.
* failedToForward - (리포팅 모드 서버만 해당) 서버가 이 요청을 P2P 모드 서버로 전달하려고 했지만 연결이 실패했습니다.
* invalid\_API\_version - 서버가 요청의 API 버전 번호를 지원하지 않습니다.
* jsonInvalid - (웹소켓만 해당) 요청이 올바른 JSON 객체가 아닙니다.
  * 이 경우 JSON-RPC는 대신 400 잘못된 요청 HTTP 오류를 반환합니다.
* missingCommand - (WebSocket만 해당) 요청에 명령 필드가 지정되지 않았습니다.
  * 이 경우 JSON-RPC는 대신 400 잘못된 요청 HTTP 오류를 반환합니다.
* noClosed - 서버에 닫힌 ledger이 없습니다. 일반적으로 서버가 시작을 완료하지 않았기 때문입니다.
* noCurrent - 서버가 높은 부하, 네트워크 문제, 유효성 검사기 오류, 잘못된 구성 또는 기타 문제로 인해 현재 ledger이 무엇인지 알 수 없습니다.
* noNetwork - 서버가 나머지 XRP Ledger P2P 네트워크에 연결하는 데 문제가 있습니다(독립형 모드로 실행되지 않음).
* tooBusy - 서버가 지금 이 명령을 수행하기에는 너무 많은 부하를 받고 있습니다. 일반적으로 관리자로 연결되어 있는 경우 반환되지 않습니다.
* unknownCmd - 요청에 rippled 서버가 인식하는 명령이 포함되어 있지 않습니다.
* wsTextRequired - (웹소켓만 해당) 요청의 연산 코드가 텍스트가 아닙니다.
