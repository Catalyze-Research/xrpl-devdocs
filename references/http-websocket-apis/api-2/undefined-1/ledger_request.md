# ledger\_request

ledger\_request 명령은 서버가 연결된 피어에서 특정 ledger 버전을 가져오도록 지시합니다. 이 명령은 서버에 즉시 연결된 피어 중 하나에 해당 ledger이 있는 경우에만 작동합니다. ledger을 완전히 가져오려면 명령을 여러번 실행해야 할 수도 있습니다.

ledger\_request 메소드는 권한이 없는 사용자가 실행할 수 없는 관리자 메소드입니다!

## 요청 형식

요청 형식의 예시입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": 102,
    "command": "ledger_request",
    "ledger_index": 13800000
}
```
{% endtab %}

{% tab title="Commandline" %}
```
rippled ledger_request 13800000
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| `Field`        | 유형  | 설명                                                                                            |
| -------------- | --- | --------------------------------------------------------------------------------------------- |
| `ledger_index` | 숫자  | (선택 사항) [Ledger Index](https://xrpl.org/basic-data-types.html#ledger-index) 로 지정된 원장을 검색합니다 . |
| `ledger_hash`  | 문자열 | (선택 사항) 식별 Hash 로 지정된 원장을 검색합니다 .                                                             |

ledger\_index 또는 ledger\_hash 중 하나만 제공해야 하며 둘 다 제공하면 안 됩니다.

## 응답 형식

응답은 표준 형식을 따릅니다. 그러나 요청이 rippled 서버에 ledger 검색을 시작하도록 성공적으로 지시했더라도 지정된 ledger이 없는 경우 실패 응답을 반환합니다.

{% hint style="info" %}
Note:

ledger을 검색하려면 rippled 서버의 기록에 해당 ledger이 있는 직접 피어가 있어야 합니다. 요청된 ledger을 가진 피어가 없는 경우, 연결 메소드 또는 구성 파일의 fixed\_ips 섹션을 사용하여 s2.ripple.com에 Ripple의 전체 히스토리 서버를 추가한 다음 ledger\_request를 다시 요청할 수 있습니다.
{% endhint %}

실패 응답은 ledger을 가져오는 중이라는 상태를 나타냅니다. 성공 응답은 ledger 메소드와 유사한 형식으로 ledger에 대한 정보를 포함합니다.

{% tabs %}
{% tab title="Commandline (failure)" %}
```json
Loading: "/etc/rippled.cfg"
Connecting to 127.0.0.1:5005

{
   "result" : {
      "acquiring" : {
         "hash" : "01DDD89B6605E20338B8EEB8EB2B0E0DD2F685A2B164F3790C4D634B5734CC26",
         "have_header" : false,
         "peers" : 2,
         "timeouts" : 0
      },
      "error" : "lgrNotFound",
      "error_code" : 20,
      "error_message" : "acquiring ledger containing requested index",
      "request" : {
         "command" : "ledger_request",
         "ledger_index" : 18851277
      },
      "status" : "error"
   }
}
```
{% endtab %}

{% tab title="Commandline (in-progress)" %}
```json
Loading: "/etc/rippled.cfg"
Connecting to 127.0.0.1:5005

{
   "result" : {
      "acquiring" : {
         "hash" : "01DDD89B6605E20338B8EEB8EB2B0E0DD2F685A2B164F3790C4D634B5734CC26",
         "have_header" : false,
         "peers" : 2,
         "timeouts" : 0
      },
      "error" : "lgrNotFound",
      "error_code" : 20,
      "error_message" : "acquiring ledger containing requested index",
      "request" : {
         "command" : "ledger_request",
         "ledger_index" : 18851277
      },
      "status" : "error"
   }
}
```
{% endtab %}

{% tab title="Commandline (success)" %}
```json
Loading: "/etc/rippled.cfg"
Connecting to 127.0.0.1:5005

{
   "result" : {
      "ledger" : {
         "accepted" : true,
         "account_hash" : "84EBB27D9510AD5B9A3A328201921B3FD418D4A349E85D3DC69E33C7B506407F",
         "close_time" : 486691300,
         "close_time_human" : "2015-Jun-04 00:01:40",
         "close_time_resolution" : 10,
         "closed" : true,
         "hash" : "DCF5D723ECEE1EF56D2B0024CD9BDFF2D8E3DC211BD2B9460165922564ACD863",
         "ledger_hash" : "DCF5D723ECEE1EF56D2B0024CD9BDFF2D8E3DC211BD2B9460165922564ACD863",
         "ledger_index" : "13840000",
         "parent_hash" : "8A3F6FBC62C11DE4538D969F9C7966234635FE6CEB1133DDC37220978F8100A9",
         "seqNum" : "13840000",
         "totalCoins" : "99999022883526403",
         "total_coins" : "99999022883526403",
         "transaction_hash" : "3D759EF3AF1AE2F78716A8CCB2460C3030F82687E54206E883703372B9E1770C"
      },
      "ledger_index" : 13840000,
      "status" : "success"
   }
}
```
{% endtab %}
{% endtabs %}

가능한 세 가지 응답 형식은 다음과 같습니다:

1. lgrNotFound 오류를 반환하는 경우, 응답에는 P2P 네트워크에서 ledger을 가져오는 진행 상황을 나타내는 ledger 요청 객체가 포함된 필드가 있습니다.
2. 응답에 서버가 현재 ledger을 가져오는 중이라고 표시되면 결과 본문은 P2P 네트워크에서 ledger을 가져오는 진행 상황을 나타내는 ledger 요청 객체입니다.
3. ledger을 완전히 사용할 수 있게 되면 응답은 ledger 헤더를 나타냅니다.

## ledger 요청 객체

서버가 ledger을 가져오는 중이지만 아직 완료되지 않은 경우, rippled 서버는 ledger을 가져오는 진행 상황을 나타내는 ledger 요청 객체를 반환합니다. 이 객체에는 다음과 같은 필드가 있습니다:

| `Field`                     | 유형      | 설명                                                |
| --------------------------- | ------- | ------------------------------------------------- |
| `hash`                      | 문자열     | (생략 가능) 서버가 알고 있는 경우 요청한 원장의 해시입니다.               |
| `have_header`               | Boolean | 서버에 요청된 원장의 헤더 섹션이 있는지 여부.                        |
| `have_state`                | Boolean | (생략 가능) 요청한 원장의 account-state 섹션이 서버에 있는지 여부.     |
| `have_transactions`         | Boolean | (생략가능) 요청한 원장의 거래부분이 서버에 있는지 여부.                  |
| `needed_state_hashes`       | 문자열 배열  | (생략 가능) 서버가 여전히 검색해야 하는 상태 트리 의 객체 해시 최대 16개입니다.  |
| `needed_transaction_hashes` | 문자열 배열  | (생략 가능) 서버가 여전히 검색해야 하는 트랜잭션 트리의 객체 해시 최대 16개입니다. |
| `peers`                     | 숫자      | 이 원장을 찾기 위해 서버가 쿼리하는 피어 수입니다.                     |
| `timeouts`                  | 숫자      | 지금까지 이 원장을 가져오는 횟수가 시간 초과되었습니다.                   |

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다. 이 오류는 현재 진행 중인 ledger과 같거나 더 높은 ledger 인덱스를 지정한 경우에도 발생할 수 있습니다.
* lgrNotFound - ledger을 아직 사용할 수 없는 경우. 이는 서버가 ledger을 가져오기 시작했음을 나타내지만, 연결된 피어 중 요청된 ledger이 없는 경우 실패할 수 있습니다. (이전에는 이 오류 대신 ledgerNotFound 코드를 사용했습니다.) [![Updated in: rippled 0.30.1](https://img.shields.io/badge/Updated%20in-rippled%200.30.1-blue.svg)](https://github.com/ripple/rippled/releases/tag/0.30.1)
* reportingUnsupported - (리포팅 모드 서버만 해당) 이 메소드는 리포팅 모드에서 사용할 수 없습니다.
