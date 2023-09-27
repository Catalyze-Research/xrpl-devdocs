# crawl\_shards

피어 서버에서 사용 가능한 기록 ledger 데이터 조각에 대한 정보를 요청합니다. [![New in: rippled 1.2.0](https://img.shields.io/badge/New%20in-rippled%201.2.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/1.2.0)

_`crawl_shards`_ 메소드는 권한이 없는 사용자가 실행할 수 없는 관리자 메소드입니다.

## 요청 형식

요청 형식의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "command": "crawl_shards",
  "public_key": true,
  "limit": 0
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
  "method": "crawl_shards",
  "params": [
    {
      "public_key": true,
      "limit": 0
    }
  ]
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Note:

이 메소드에 대한 커맨드라인 구문은 없습니다. 커맨드라인에서 이 메소드에 액세스하려면 json 메소드를 사용하세요.
{% endhint %}

요청에는 다음 필드가 포함됩니다:

| `Field`      | 유형      | 설명                                                                            |
| ------------ | ------- | ----------------------------------------------------------------------------- |
| `public_key` | Boolean | (선택 사항) true이면 응답에 크롤링된 서버의 노드 공개 키(P2P 통신용)가 포함됩니다. 기본값은 거짓입니다.              |
| `limit`      | 숫자      | (선택 사항) 검색할 홉 수입니다. 기본값은 0으로, 직접 피어만 검색합니다. 1로 제한하면 피어의 피어도 검색합니다. 최대값은 3입니다. |

{% hint style="info" %}
Caution:

한도가 증가함에 따라 잠재적으로 검색할 수 있는 피어 수가 기하급수적으로 증가합니다. 제한을 2 또는 3으로 설정하면 서버가 API 요청에 응답하는 데 몇 초가 걸릴 수 있습니다.
{% endhint %}

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "result": {
    "complete_shards": "1-2,5,8-9,584,1973,2358",
    "peers": [
      {
        "complete_shards": "1-2,8,47,371,464,554,653,857,1076,1402,1555,1708,1813,1867",
        "public_key": "n9LxFZiySnfDSvfh23N94UxsFkCjWyrchTeKHcYE6tJJQL5iejb2"
      },
      {
        "complete_shards": "8-9,584",
        "ip": "192.168.1.132",
        "public_key": "n9MN5xwYqbrj64rtfZAXQy7Y3sNxXZJeLt7Lj61a9DYEZ4SE2tQQ"
      }
    ]
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
    "complete_shards": "1-2,5,8-9,584,1973,2358",
    "peers": [
      {
        "complete_shards": "1-2,8,47,371,464,554,653,857,1076,1402,1555,1708,1813,1867",
        "public_key": "n9LxFZiySnfDSvfh23N94UxsFkCjWyrchTeKHcYE6tJJQL5iejb2"
      },
      {
        "complete_shards": "8-9,584",
        "ip": "192.168.1.132",
        "public_key": "n9MN5xwYqbrj64rtfZAXQy7Y3sNxXZJeLt7Lj61a9DYEZ4SE2tQQ"
      }
    ],
    "status": "success"
  }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`           | 유형  | 설명                                                                                                                                                                |
| ----------------- | --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `complete_shards` | 문자열 | (생략 가능) 로컬 서버에서 사용할 수 있는 기록 샤드의 범위입니다. 이 값은 빈 문자열이거나 연결되지 않은 범위일 수 있습니다. 예를 들어 1-2,5,7-9는 샤드 1, 2, 5, 7, 8, 9를 사용할 수 있음을 나타냅니다. 이 서버에 히스토리 샤딩이 활성화되지 않은 경우 생략됩니다. |
| `peers`           | 배열  | (생략 가능) 각 피어가 사용할 수 있는 히스토리 샤드를 설명하는 피어 샤드 오브젝트 목록(아래 참조). 제한으로 지정된 홉 수 내에 샤드가 있는 피어가 없는 경우 응답은 이 필드를 생략합니다.                                                      |

## 피어 샤드 오브젝트

응답의 피어 배열의 각 멤버는 P2P 네트워크에서 하나의 서버를 설명하는 객체입니다. 이 목록에는 사용 가능한 완전한 기록 샤드가 하나 이상 있는 피어만 포함됩니다. 배열의 각 객체에는 다음과 같은 필드가 있습니다:

| `Field`             | 유형  | 설명                                                                                                                                                                                                                                                                           |
| ------------------- | --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `complete_shards`   | 문자열 | 이 피어가 사용할 수 있는 전체 기록 샤드의 범위입니다. 이 범위는 분리되어 있을 수 있습니다. 예를 들어 1-2,5,7-9는 샤드 1, 2, 5, 7, 8, 9를 사용할 수 있음을 나타냅니다.                                                                                                                                                                 |
| `incomplete_shards` | 문자열 | (생략 가능) 이 피어가 부분적으로 다운로드한 히스토리 샤드의 쉼표로 구분된 목록과 각 샤드의 완료 비율입니다. 예를 들어, 1:50,2:25는 샤드 1이 50% 다운로드되고 샤드 2가 25% 다운로드되었음을 나타냅니다.[![새로운 기능: Rippled 1.8.1](https://img.shields.io/badge/New%20in-rippled%201.8.1-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.8.1) |
| `public_key`        | 문자열 | (요청에서 "공개키": true를 지정하지 않은 경우 생략) 이 피어가 P2P 통신에 사용하는 공개키, XRP Ledger의 base58 형식입니다.                                                                                                                                                                                          |

IP 필드는 더 이상 제공되지 않습니다.[![Removed in: rippled 1.8.1](https://img.shields.io/badge/Removed%20in-rippled%201.8.1-red.svg)](https://github.com/ripple/rippled/releases/tag/1.8.1)

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 요청에서 하나 이상의 필수 필드가 생략되었거나 제공된 필드가 잘못된 데이터 유형으로 지정되었습니다.
* reportingUnsupported - (리포팅 모드 서버만 해당) 이 메소드는 리포팅 모드에서 사용할 수 없습니다.
