# connect

connect 명령은 rippled 서버가 특정 피어 rippled 서버에 연결하도록 강제합니다.

connect 메소드는 권한이 없는 사용자가 실행할 수 없는 관리자 메소드입니다!

## 요청 형식

요청 형식의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "command": "connect",
    "ip": "192.170.145.88",
    "port": 51235
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "connect",
    "params": [
        {
            "ip": "192.170.145.88",
            "port": 51235
        }
    ]
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
#Syntax: connect ip [port]
rippled connect 192.170.145.88 51235
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| `Field` | 유형  | 설명                                                                                                                                                                                        |
| ------- | --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ip`    | 문자열 | 연결할 서버의 IP 주소                                                                                                                                                                             |
| `port`  | 숫자  | (선택사항) 연결 시 사용할 포트 번호입니다. 기본값은 **2459**입니다.[![업데이트: 파문 1.6.0](https://img.shields.io/badge/Updated%20in-rippled%201.6.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.6.0) |

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="JSON-RPC" %}
```json
{
   "result" : {
      "message" : "connecting",
      "status" : "success"
   }
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
Loading: "/etc/rippled.cfg"
Connecting to 127.0.0.1:5005

{
   "result" : {
      "message" : "connecting",
      "status" : "success"
   }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`   | 유형  | 설명                           |
| --------- | --- | ---------------------------- |
| `message` | 문자열 | `connecting`명령이 성공한 경우 값입니다. |

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* stand-alone 모드에서 연결할 수 없음 - stand-alone 모드에서 네트워크 관련 명령을 사용할 수 없습니다.
* reportingUnsupported - (리포팅 모드 서버만 해당) 이 메소드는 리포팅 모드에서 사용할 수 없습니다.
