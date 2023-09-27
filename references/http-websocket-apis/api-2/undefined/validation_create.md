# validation\_create

문문validation\_create 명령을 사용해 rippled 서버가 네트워크에서 자신을 식별하는 데 사용할 수 있는 암호화 키를 생성합니다. wallet\_propose 메소드와 마찬가지로, 이 메소드는 적절한 형식의 키 세트만 생성합니다. XRP Ledger 데이터나 서버 구성은 변경하지 않습니다.

validation\_create 메소드는 권한이 없는 사용자가 실행할 수 없는 관리자 메소드입니다.

생성된 키 쌍을 사용하여 유효성 검사(유효성 검사 키 쌍) 또는 일반 피어 투 피어 통신(노드 키 쌍)에 서명하도록 서버를 구성할 수 있습니다.

{% hint style="info" %}
Tip:

강력한 유효성 검사기를 구성하려면 오프라인 마스터 키로 유효성 검사기 토큰(회전 가능)을 생성하기 위해 유효성 검사기-키 도구(rippled RPM에 포함됨)를 사용해야 합니다. 자세한 내용은 유효성 검사기 설정을 참조하세요.
{% endhint %}

## 요청 형식

요청 형식의 예시입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": 0,
    "command": "validation_create",
    "secret": "BAWL MAN JADE MOON DOVE GEM SON NOW HAD ADEN GLOW TIRE"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "validation_create",
    "params": [
        {
            "secret": "BAWL MAN JADE MOON DOVE GEM SON NOW HAD ADEN GLOW TIRE"
        }
    ]
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
#Syntax: validation_create [secret]
rippled validation_create "BAWL MAN JADE MOON DOVE GEM SON NOW HAD ADEN GLOW TIRE"
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| `Field`  | 유형  | 설명                                                                                                                                          |
| -------- | --- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `secret` | 문자열 | (선택 사항) 이 값을 시드로 사용하여 자격 증명을 생성합니다. 동일한 비밀은 항상 동일한 자격 증명을 생성합니다. RFC-1751 형식이나 XRP Ledger의 base58 형식 으로 시드를 제공할 수 있습니다. 생략하면 무작위 시드를 생성합니다. |

{% hint style="info" %}
Note:

유효성 검즈인의 보안은 시드의 엔트로피에 따라 달라집니다. 강력한 무작위성으로 생성된 것이 아니라면 실제 비즈니스 목적으로 비밀 값을 사용하지 마세요. Ripple은 새 자격 증명을 처음 생성할 때 비밀을 생략할 것을 권장합니다.
{% endhint %}

## 응답 형식

성공적인 응답의 예시입니다:

{% tabs %}
{% tab title="JSON-RPC" %}
```json
{
   "result" : {
      "status" : "success",
      "validation_key" : "FAWN JAVA JADE HEAL VARY HER REEL SHAW GAIL ARCH BEN IRMA",
      "validation_public_key" : "n9Mxf6qD4J55XeLSCEpqaePW4GjoCR5U1ZeGZGJUCNe3bQa4yQbG",
      "validation_seed" : "ssZkdwURFMBXenJPbrpE14b6noJSu"
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
      "validation_key" : "FAWN JAVA JADE HEAL VARY HER REEL SHAW GAIL ARCH BEN IRMA",
      "validation_public_key" : "n9Mxf6qD4J55XeLSCEpqaePW4GjoCR5U1ZeGZGJUCNe3bQa4yQbG",
      "validation_seed" : "ssZkdwURFMBXenJPbrpE14b6noJSu"
   }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`                 | 유형  | 설명                                                          |
| ----------------------- | --- | ----------------------------------------------------------- |
| `validation_key`        | 문자열 | 유효성 검사 자격 증명을 위한 비밀 키로, RFC-1751 형식입니다.                     |
| `validation_public_key` | 문자열 | 이 유효성 검사 자격 증명을 위한 공개 키로, XRP Ledger의 base58 인코딩 문자열 형식입니다. |
| `validation_seed`       | 문자열 | 유효성 검사 자격 증명을 위한 비밀 키로, XRP Ledger에 베이스58로 인코딩된 문자열 형식입니다.  |

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* badSeed - 요청이 잘못된 시드 값을 제공했습니다. 이는 일반적으로 시드 값이 계정 주소나 유효성 검사 공개 키와 같은 다른 형식의 유효한 문자열인 것처럼 보이는 것을 의미합니다.
