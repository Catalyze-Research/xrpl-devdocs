# 응답 형식

응답 형식은 메소드가 WebSocket, JSON-RPC 또는 Commandline 인터페이스로 호출되었는지 여부에 따라 약간씩 달라집니다. Commandline 인터페이스는 JSON-RPC를 호출하기 때문에 Commandline 인터페이스와 JSON-RPC 인터페이스는 동일한 형식을 사용합니다.

성공적인 응답의 필드에는 다음이 포함됩니다:

| Field         | 유형      | 설명                                                                                                                                       |
| ------------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| id            | (변수)    | (웹소켓만 해당) 이 응답을 유발한 요청에 제공된 ID입니다.                                                                                                       |
| status        | 문자열     | (웹소켓만 해당) 성공 값은 서버가 요청을 성공적으로 수신하고 이해했음을 나타냅니다. 일부 클라이언트 라이브러리는 성공 시 이 필드를 생략합니다.                                                        |
| result.status | 문자열     | (JSON-RPC 및 명령줄) 값은 success요청이 서버에서 성공적으로 수신되고 이해되었음을 나타냅니다. 일부 [클라이언트 라이브러리는](https://xrpl.org/client-libraries.html) 성공 시 이 필드를 생략합니다. |
| type          | 문자열     | 웹소켓만 해당) 응답 값은 API 요청에 대한 직접 응답을 나타냅니다. 비동기 알림은 ledgerClosed 또는 트랜잭션과 같은 다른 값을 사용합니다.                                                    |
| result        | 객체      | 쿼리 결과입니다. 내용은 명령에 따라 다릅니다.                                                                                                               |
| warning       | 문자열     | (생략 가능) 이 필드가 제공되면 값은 문자열 로드입니다. 이는 클라이언트가 서버가 이 클라이언트의 연결을 끊는 속도 제한 임계값에 접근하고 있음을 의미합니다.                                                |
| warnings      | 배열      | (생략 가능) 이 필드가 제공되면 중요한 경고가 있는 하나 이상의 경고 개체가 포함됩니다. 자세한 내용은 API 경고를 참조하세요.                                                                |
| forwarded     | Boolean | 참이면, 요청에 리포팅 모드에서 사용할 수 없는 데이터가 필요하기 때문에 이 요청과 응답이 리포팅 모드 서버에서 P2P 모드 서버로(또는 그 반대로) 전달되었습니다. 기본값은 거짓입니다.                                 |

## 성공적인 응답 예시

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 2,
  "status": "success",
  "type": "response",
  "result": {
    "account_data": {
      "Account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
      "Balance": "27389517749",
      "Flags": 0,
      "LedgerEntryType": "AccountRoot",
      "OwnerCount": 18,
      "PreviousTxnID": "B6B410172C0B65575D89E464AF5B99937CC568822929ABF87DA75CBD11911932",
      "PreviousTxnLgrSeq": 6592159,
      "Sequence": 1400,
      "index": "4F83A2CF7E70F77F79A307E6A472BFC2585B806A70833CCD1C26105BAE0D6E05"
    },
    "ledger_index": 6760970
  }
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
HTTP Status: 200 OK

{
    "result": {
        "account_data": {
            "Account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
            "Balance": "27389517749",
            "Flags": 0,
            "LedgerEntryType": "AccountRoot",
            "OwnerCount": 18,
            "PreviousTxnID": "B6B410172C0B65575D89E464AF5B99937CC568822929ABF87DA75CBD11911932",
            "PreviousTxnLgrSeq": 6592159,
            "Sequence": 1400,
            "index": "4F83A2CF7E70F77F79A307E6A472BFC2585B806A70833CCD1C26105BAE0D6E05"
        },
        "ledger_index": 6761012,
        "status": "success"
    }
}
```
{% endtab %}
{% endtabs %}

## API 경고

응답에 경고 배열이 포함된 경우 배열의 각 멤버는 서버의 개별 경고를 나타냅니다. 이러한 각 경고 객체에는 다음 필드가 포함됩니다:

| Field   | 유형  | 설명                                                                                                        |
| ------- | --- | --------------------------------------------------------------------------------------------------------- |
| id      | 숫자  | 이 경고 메시지에 대한 고유 숫자 코드입니다.                                                                                 |
| message | 문자열 | 이 메시지의 원인을 설명하는 사람이 읽을 수 있는 문자열입니다. 이 메시지의 내용에 의존하는 소프트웨어를 작성하지 마시고 대신 ID(해당되는 경우 세부 정보)를 사용하여 경고를 식별하세요. |
| details | 객체  | 세부 정보 객체(생략할 수 있음) 이 경고에 대한 추가 정보입니다. 내용은 경고 유형에 따라 다릅니다.                                                 |

다음 참조에서는 가능한 모든 경고에 대해 설명합니다.

## 1001. 지원되지 않는 수정안이 과반수에 도달했습니다.

경고 예시입니다:

```json
"warnings" : [
  {
    "details" : {
      "expected_date" : 864030,
      "expected_date_UTC" : "2000-Jan-11 00:00:30.0000000 UTC"
    },
    "id" : 1001,
    "message" : "One or more unsupported amendments have reached majority. Upgrade to the latest version before they are activated to avoid being amendment blocked."
  }
]
```

이 경고는 XRP Ledger 프로토콜에 대한 하나 이상의 수정이 활성화될 예정이지만 현재 서버에 해당 수정에 대한 구현이 없음을 나타냅니다. 해당 수정안이 활성화되면 현재 서버는 수정안이 차단되므로 가능한 한 빨리 최신 리플 버전으로 업그레이드해야 합니다.

서버는 클라이언트가 관리자로 연결된 경우에만 이 경고를 보냅니다.

이 경고에는 다음 필드가 있는 세부 정보 필드가 포함됩니다:

| 필드                  | 값   | 설명                                                      |
| ------------------- | --- | ------------------------------------------------------- |
| expected\_date      | 숫자  | Ripple 에포크 이후 지원되지 않는 첫 번째 수정안이 활성화될 것으로 예상되는 시간(초)입니다. |
| expected\_date\_UTC | 문자열 | 지원되지 않는 첫 번째 수정안이 활성화될 것으로 예상되는 UTC 기준의 타임스탬프입니다.       |

ledger 마감 시간은 다양하기 때문에 대략적인 시간입니다. 수정안이 지정된 시간까지 검증인의 80% 이상의 지지를 유지하지 못하여 예상 시간에 활성화되지 않을 수도 있습니다. 지원되지 않는 수정안이 활성화되지 않는 한 서버는 수정안이 차단되지 않습니다.

## 1002. 이 서버는 수정이 차단됨

경고 예시:

```json
"warnings" : [
  {
    "id" : 1002,
    "message" : "This server is amendment blocked, and must be updated to be able to stay in sync with the network."
  }
]
```

이 경고는 서버가 수정이 차단되어 더 이상 XRP Ledger과 동기화 상태를 유지할 수 없음을 나타냅니다.

서버 관리자는 활성화된 수정안을 지원하는 버전으로 [`rippled`](https://xrpl.org/install-rippled.html)`를` 업그레이드해야 합니다.

## 1003. 리포팅 서버입니다.

[![New in: rippled 1.7.0](https://img.shields.io/badge/New%20in-rippled%201.7.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.7.0)

경고 예시:

```json
"warnings" : [
  {
    "id" : 1003,
    "message" : "This is a reporting server. The default behavior of a reporting server is to only return validated data. If you are looking for not yet validated data, include \"ledger_index : current\" in your request, which will cause this server to forward the request to a p2p node. If the forward is successful the response will include \"forwarded\" : \"true\""
  }
]
```

이 경고는 요청에 응답하는 서버가 리포팅 모드를 실행 중임을 나타냅니다. 리포팅 모드는 P2P 네트워크에 연결하지 않고 아직 검증되지 않은 ledger 데이터를 추적하지 않기 때문에 특정 API 메소드를 사용할 수 없거나 다르게 동작할 수 있습니다.

일반적으로 이 경고는 무시해도 안전합니다.

{% hint style="info" %}
Caution:

ledger 버전을 명시적으로 지정하지 않고 ledger 데이터를 요청하는 경우, 보고 모드는 현재 진행 중인 ledger 대신 기본적으로 유효성이 검증된 최신 ledger을 사용합니다.
{% endhint %}
