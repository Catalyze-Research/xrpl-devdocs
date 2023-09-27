# manifest

manifest 메소드는 주어진 유효성 검증인 공개 키에 대한 현재 "manifest" 정보를 보고합니다. "manifest"는 유효성 검증인의 마스터 키 쌍의 서명으로 임시 서명 키를 인증하는 데이터 블록입니다. [![Updated in: rippled 1.7.0](https://img.shields.io/badge/Updated%20in-rippled%201.7.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/1.7.0)

## 요청 형식

요청 형식의 예시입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "command": "manifest",
    "public_key": "nHUFE9prPXPrHcG3SkwP1UzAQbSphqyQkQK9ATXLZsfkezhhda3p"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "manifest",
    "params": [{
        "public_key":"nHUFE9prPXPrHcG3SkwP1UzAQbSphqyQkQK9ATXLZsfkezhhda3p"
    }]
}
```
{% endtab %}

{% tab title="Commandline" %}
```
#Syntax: manifest public_key
rippled manifest nHUFE9prPXPrHcG3SkwP1UzAQbSphqyQkQK9ATXLZsfkezhhda3p
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| `Field`      | 유형  | 설명                                                                                                            |
| ------------ | --- | ------------------------------------------------------------------------------------------------------------- |
| `public_key` | 문자열 | 조회할 유효성 검사기의 base58로 인코딩된 공개 키 입니다.[ ](https://xrpl.org/base58-encodings.html)이는 마스터 공개 키 또는 임시 공개 키일 수 있습니다. |

{% hint style="info" %}
Note:

이 메소드의 커맨드라인 형식은 rippled v1.5.0에서 작동하지 않습니다. 자세한 내용은 이슈 #3317을 참조하세요.
{% endhint %}

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "result": {
    "details": {
      "domain": "",
      "ephemeral_key": "n9J67zk4B7GpbQV5jRQntbgdKf7TW6894QuG7qq1rE5gvjCu6snA",
      "master_key": "nHUFE9prPXPrHcG3SkwP1UzAQbSphqyQkQK9ATXLZsfkezhhda3p",
      "seq": 1
    },
    "manifest": "JAAAAAFxIe3AkJgOyqs3y+UuiAI27Ff3Mrfbt8e7mjdo06bnGEp5XnMhAhRmvCZmWZXlwShVE9qXs2AVCvhVuA/WGYkTX/vVGBGwdkYwRAIgGnYpIGufURojN2cTXakAM7Vwa0GR7o3osdVlZShroXQCIH9R/Lx1v9rdb4YY2n5nrxdnhSSof3U6V/wIHJmeao5ucBJA9D1iAMo7YFCpb245N3Czc0L1R2Xac0YwQ6XdGT+cZ7yw2n8JbdC3hH8Xu9OUqc867Ee6JmlXtyDHzBdY/hdJCQ==",
    "requested": "nHUFE9prPXPrHcG3SkwP1UzAQbSphqyQkQK9ATXLZsfkezhhda3p"
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
    "details": {
      "domain": "",
      "ephemeral_key": "n9J67zk4B7GpbQV5jRQntbgdKf7TW6894QuG7qq1rE5gvjCu6snA",
      "master_key": "nHUFE9prPXPrHcG3SkwP1UzAQbSphqyQkQK9ATXLZsfkezhhda3p",
      "seq": 1
    },
    "manifest": "JAAAAAFxIe3AkJgOyqs3y+UuiAI27Ff3Mrfbt8e7mjdo06bnGEp5XnMhAhRmvCZmWZXlwShVE9qXs2AVCvhVuA/WGYkTX/vVGBGwdkYwRAIgGnYpIGufURojN2cTXakAM7Vwa0GR7o3osdVlZShroXQCIH9R/Lx1v9rdb4YY2n5nrxdnhSSof3U6V/wIHJmeao5ucBJA9D1iAMo7YFCpb245N3Czc0L1R2Xac0YwQ6XdGT+cZ7yw2n8JbdC3hH8Xu9OUqc867Ee6JmlXtyDHzBdY/hdJCQ==",
    "requested": "nHUFE9prPXPrHcG3SkwP1UzAQbSphqyQkQK9ATXLZsfkezhhda3p",
    "status": "success"
  }
}
```
{% endtab %}

{% tab title="Commandline" %}
```
Loading: "/etc/rippled.cfg"
Connecting to 127.0.0.1:5005

{
  "result": {
    "details": {
      "domain": "",
      "ephemeral_key": "n9J67zk4B7GpbQV5jRQntbgdKf7TW6894QuG7qq1rE5gvjCu6snA",
      "master_key": "nHUFE9prPXPrHcG3SkwP1UzAQbSphqyQkQK9ATXLZsfkezhhda3p",
      "seq": 1
    },
    "manifest": "JAAAAAFxIe3AkJgOyqs3y+UuiAI27Ff3Mrfbt8e7mjdo06bnGEp5XnMhAhRmvCZmWZXlwShVE9qXs2AVCvhVuA/WGYkTX/vVGBGwdkYwRAIgGnYpIGufURojN2cTXakAM7Vwa0GR7o3osdVlZShroXQCIH9R/Lx1v9rdb4YY2n5nrxdnhSSof3U6V/wIHJmeao5ucBJA9D1iAMo7YFCpb245N3Czc0L1R2Xac0YwQ6XdGT+cZ7yw2n8JbdC3hH8Xu9OUqc867Ee6JmlXtyDHzBdY/hdJCQ==",
    "requested": "nHUFE9prPXPrHcG3SkwP1UzAQbSphqyQkQK9ATXLZsfkezhhda3p",
    "status": "success"
  }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`     | 유형  | 설명                                                                                                                                                         |
| ----------- | --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `details`   | 객체  | (생략 가능) 이 매니페스트에 포함된 데이터입니다. `public_key`서버에 요청에 대한 매니페스트가 없으면 생략됩니다. 해당 내용에 대한 전체 설명은 아래 **세부정보 개체를** 참조하세요.                                              |
| `manifest`  | 문자열 | (생략 가능) Base64 형식의 전체 매니페스트 데이터입니다. 이 데이터는 base64로 인코딩되기 전에 바이너리로 [직렬화 됩니다. ](https://xrpl.org/serialization.html)`public_key`서버에 요청에 대한 매니페스트가 없으면 생략됩니다. |
| `requested` | 문자열 | `public_key`요청의 입니다 .                                                                                                                                      |

## 세부 정보 객체

제공된 경우 세부 정보 객체에는 다음 필드가 포함됩니다:

| `Field`         | 유형  | 설명                                                                                          |
| --------------- | --- | ------------------------------------------------------------------------------------------- |
| `domain`        | 문자열 | 이 유효성 검사기가 연결되어 있다고 주장하는 도메인 이름입니다. 매니페스트에 도메인이 포함되지 않은 경우 이는 빈 문자열입니다.                     |
| `ephemeral_key` | 문자열 | [base58](https://xrpl.org/base58-encodings.html)에 있는 이 유효성 검사기의 임시 공개 키입니다.                 |
| `master_key`    | 문자열 | [base58](https://xrpl.org/base58-encodings.html)에 있는 이 유효성 검사기의 마스터 공개 키입니다.                |
| `seq`           | 숫자  | 이 매니페스트의 시퀀스 번호입니다. 이 숫자는 유효성 검사기 운영자가 임시 키를 회전하거나 설정을 변경하기 위해 유효성 검사기 토큰을 업데이트할 때마다 증가합니다. |

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 공개 키 필드가 누락되었거나 잘못 지정되었습니다.
* reportingUnsupported - (리포팅 모드 서버만 해당) 이 메소드는 리포팅 모드에서 사용할 수 없습니다.
