# validation\_seed

validation\_seed 명령은 rippled 유효성 검사에 서명할 때 사용하는 비밀 값을 일시적으로 설정합니다. 이 값은 서버를 다시 시작할 때 구성 파일에 따라 재설정됩니다. [![Disabled since: rippled 0.29.1](https://img.shields.io/badge/Disabled%20since-rippled%200.29.1-red.svg)](https://github.com/ripple/rippled/releases/tag/0.29.1-rc1)

validation\_seed 메소드는 권한이 없는 사용자가 실행할 수 없는 관리자 메소드입니다!

## 요청 형식

요청 형식의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": "set_seed_1",
    "command": "validation_seed",
    "secret": "BAWL MAN JADE MOON DOVE GEM SON NOW HAD ADEN GLOW TIRE"
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
#Syntax: validation_seed [secret]
rippled validation_seed 'BAWL MAN JADE MOON DOVE GEM SON NOW HAD ADEN GLOW TIRE'
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| `Field`  | 유형  | 설명                                                                                                                                       |
| -------- | --- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| `secret` | 문자열 | (선택 사항) 이 값이 있는 경우 이 값을 유효성 검사 키 쌍의 비밀 값으로 사용합니다. 유효한 형식으로는 XRP Ledger의 base58 형식, RFC-1751 또는 암호문이 있습니다. 생략하면 네트워크에 유효성 검사를 제안할 수 없습니다. |

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="JSON-RPC" %}
```json
200 OK

{
   "result" : {
      "status" : "success",
      "validation_key" : "BAWL MAN JADE MOON DOVE GEM SON NOW HAD ADEN GLOW TIRE",
      "validation_public_key" : "n9Jx6RS6zSgqsgnuWJifNA9EqgjTKAywqYNReK5NRd1yLBbfC3ng",
      "validation_seed" : "snjJkyBGogTem5dFGbcRaThKq2Rt3"
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
      "status" : "success",
      "validation_key" : "BAWL MAN JADE MOON DOVE GEM SON NOW HAD ADEN GLOW TIRE",
      "validation_public_key" : "n9Jx6RS6zSgqsgnuWJifNA9EqgjTKAywqYNReK5NRd1yLBbfC3ng",
      "validation_seed" : "snjJkyBGogTem5dFGbcRaThKq2Rt3"
   }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`                 | 유형  | 설명                                                                          |
| ----------------------- | --- | --------------------------------------------------------------------------- |
| `validation_key`        | 문자열 | (제안을 비활성화한 경우 생략) 유효성 검사 자격 증명을 위한 비밀 키(RFC-1751 형식)입니다.                    |
| `validation_public_key` | 문자열 | (제안을 비활성화한 경우 생략) 유효성 검사 자격 증명의 공개 키로, XRP 레저의 base58 인코딩 문자열 형식입니다.        |
| `validation_seed`       | 문자열 | (제안을 비활성화한 경우 생략) 유효성 검사 자격 증명을 위한 비밀 키로, XRP Ledger의 base58 인코딩 문자열 형식입니다. |

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* badSeed - 요청이 잘못된 비밀값을 제공했습니다. 이는 일반적으로 비밀번호 값이 계정 주소나 유효성 검사 공개 키와 같은 다른 형식의 유효한 문자열인 것처럼 보이는 것을 의미합니다.

&#x20;
