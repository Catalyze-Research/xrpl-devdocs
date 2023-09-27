# download\_shard

서버가 외부 소스에서 특정 기록 ledger 데이터 샤드를 다운로드하도록 지시합니다. rippled 서버가 히스토리 샤드를 저장하도록 구성되어 있어야 합니다. [![Updated in: rippled 1.6.0](https://img.shields.io/badge/Updated%20in-rippled%201.6.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/1.6.0)

`download_shard` 메소드는 권한이 없는 사용자가 실행할 수 없는 관리자 메소드입니다.

외부 소스는 샤드를 HTTPS를 통해 제공되는 lz4 압축 타르 아카이브로 제공해야 합니다. 아카이브에는 샤드 디렉터리와 NuDB 형식의 데이터 파일이 포함되어야 합니다.

이 방법을 사용하여 샤드를 다운로드하고 가져오는 것이 일반적으로 P2P 네트워크에서 개별적으로 샤드를 가져오는 것보다 빠릅니다. 이 방법을 사용하여 서버에서 제공할 특정 범위 또는 샤드 세트를 선택할 수도 있습니다.

## 요청 형식

요청 형식의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "command": "download_shard",
  "shards": [
    {"index": 1, "url": "https://example.com/1.tar.lz4"},
    {"index": 2, "url": "https://example.com/2.tar.lz4"},
    {"index": 5, "url": "https://example.com/5.tar.lz4"}
  ]
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
  "method": "download_shard",
  "params": [
    {
      "shards": [
        {"index": 1, "url": "https://example.com/1.tar.lz4"},
        {"index": 2, "url": "https://example.com/2.tar.lz4"},
        {"index": 5, "url": "https://example.com/5.tar.lz4"}
      ]
    }
  ]
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
# Syntax: download_shard [[<index> <url>]]
rippled download_shard 1 https://example.com/1.tar.lz4 2 https://example.com/2.tar.lz4 5 https://example.com/5.tar.lz4
```
{% endtab %}
{% endtabs %}

요청에는 다음 필드가 포함됩니다:

| `Field`  | 유형 | 설명                                          |
| -------- | -- | ------------------------------------------- |
| `shards` | 배열 | 다운로드할 샤드와 다운로드 위치를 설명하는 샤드 설명자 객체 목록(아래 참조) |

유효성 검사 필드는 더 이상 사용되지 않으며 향후 버전에서 제거될 수 있습니다. (서버는 샤드를 가져올 때 항상 샤드의 무결성을 확인합니다.) [![Updated in: rippled 1.6.0](https://img.shields.io/badge/Updated%20in-rippled%201.6.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/1.6.0)

샤드 배열의 각 샤드 설명자 객체에는 다음과 같은 필드가 있습니다:

| `Field` | 유형  | 설명                                                                                                                                                                                                                                                                                                                                               |
| ------- | --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `index` | 숫자  | 검색할 샤드의 인덱스입니다. 프로덕션 XRP Ledger에서 가장 오래된 샤드는 인덱스 1을 가지며 원장 32750-32768을 포함합니다. 다음 샤드에는 인덱스 2가 있고 원장 32769-49152 등이 포함됩니다.                                                                                                                                                                                                                        |
| `url`   | 문자열 | 이 샤드를 다운로드할 수 있는 URL입니다. URL은 `http://`또는 로 시작 `https://`하고 끝나야 합니다 `.tar.lz4`(대소문자를 구분하지 않음). 이 다운로드를 제공하는 웹 서버는 신뢰할 수 있는 인증 기관(CA)에서 서명한 유효한 TLS 인증서를 사용해야 합니다. (`rippled`운영 체제의 CA 저장소를 사용합니다.)[![업데이트: 파문 1.7.0](https://img.shields.io/badge/Updated%20in-rippled%201.7.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.7.0) |

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "result": {
    "message": "downloading shards 1-2,5"
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
    "message": "downloading shards 1-2,5",
    "status": "success"
  }
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
Loading: "/etc/rippled.cfg"
Connecting to 127.0.0.1:5005

{
  "result": {
    "message": "downloading shards 1-2,5",
    "status": "success"
  }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`   | 유형  | 설명                                 |
| --------- | --- | ---------------------------------- |
| `message` | 문자열 | 이 요청에 대한 응답으로 수행된 작업을 설명하는 메시지입니다. |

{% hint style="info" %}
Tip:

서버에서 사용 가능한 샤드를 확인하려면 크롤링 샤드 메소드를 사용하세요. 또는 샤드 저장소에 대해 구성된 위치의 하위 폴더를 확인할 수 있습니다(rippled.cfg에서 \[shard\_db]의 경로 매개변수). 폴더의 이름은 샤드 번호와 일치하도록 지정되며, 최대 하나의 폴더에 샤드가 불완전함을 나타내는 control.txt 파일이 포함될 수 있습니다.
{% endhint %}

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* notEnabled - 서버에 샤드 저장소가 구성되어 있지 않습니다.
* tooBusy - 서버가 이미 P2P 네트워크에서 또는 이전 다운로드\_샤드 요청의 결과로 샤드를 다운로드하고 있습니다.
* invalidParams - 요청에서 하나 이상의 필수 필드가 생략되었거나 제공된 필드가 잘못된 데이터 유형으로 지정되었습니다.
* reportingUnsupported - (리포팅 모드 서버만 해당) 이 메소드는 리포팅 모드에서 사용할 수 없습니다.
