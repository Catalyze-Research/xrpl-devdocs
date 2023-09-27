# validator\_info

validator\_info 메소드는 서버가 유효성 검증인으로 구성된 경우 서버의 현재 유효성 검증인 설정을 반환합니다.

validator\_info 메소드는 권한이 없는 사용자가 실행할 수 없는 관리자 메소드입니다.

## 요청 형식

요청 형식의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "command": "validator_info"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```
{
    "method": "validator_info"
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
#Syntax: validator_info
rippled validator_info
```
{% endtab %}
{% endtabs %}

요청은 어떤 매개변수도 허용하지 않습니다.

## &#x20;응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "result": {
    "domain": "mduo13.com",
    "ephemeral_key": "n9KnrcCmL5psyKtk2KWP6jy14Hj4EXuZDg7XMdQJ9cSDoFSp53hu",
    "manifest": "JAAAAAFxIe002KClGBUlRA7h5J2Y5B7Xdlxn1Z5OxY7ZC2UmqUIikHMhAkVIeB7McBf4NFsBceQQlScTVUWMdpYzwmvs115SUGDKdkcwRQIhAJnKfYWnPsBsATIIRfgkAAK+HE4zp8G8AmOPrHmLZpZAAiANiNECVQTKktoD7BEoEmS8jaFBNMgRdcG0dttPurCAGXcKbWR1bzEzLmNvbXASQPjO6wxOfhtWsJ6oMWBg8Rs5STAGvQV2ArI5MG3KbpFrNSMxbx630Ars9d9j1ORsUS5v1biZRShZfg9180JuZAo=",
    "master_key": "nHBk5DPexBjinXV8qHn7SEKzoxh2W92FxSbNTPgGtQYBzEF4msn9",
    "seq": 1
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
    "domain": "mduo13.com",
    "ephemeral_key": "n9KnrcCmL5psyKtk2KWP6jy14Hj4EXuZDg7XMdQJ9cSDoFSp53hu",
    "manifest": "JAAAAAFxIe002KClGBUlRA7h5J2Y5B7Xdlxn1Z5OxY7ZC2UmqUIikHMhAkVIeB7McBf4NFsBceQQlScTVUWMdpYzwmvs115SUGDKdkcwRQIhAJnKfYWnPsBsATIIRfgkAAK+HE4zp8G8AmOPrHmLZpZAAiANiNECVQTKktoD7BEoEmS8jaFBNMgRdcG0dttPurCAGXcKbWR1bzEzLmNvbXASQPjO6wxOfhtWsJ6oMWBg8Rs5STAGvQV2ArI5MG3KbpFrNSMxbx630Ars9d9j1ORsUS5v1biZRShZfg9180JuZAo=",
    "master_key": "nHBk5DPexBjinXV8qHn7SEKzoxh2W92FxSbNTPgGtQYBzEF4msn9",
    "seq": 1,
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
   "result" : {
      "domain" : "mduo13.com",
      "ephemeral_key" : "n9KnrcCmL5psyKtk2KWP6jy14Hj4EXuZDg7XMdQJ9cSDoFSp53hu",
      "manifest" : "JAAAAAFxIe002KClGBUlRA7h5J2Y5B7Xdlxn1Z5OxY7ZC2UmqUIikHMhAkVIeB7McBf4NFsBceQQlScTVUWMdpYzwmvs115SUGDKdkcwRQIhAJnKfYWnPsBsATIIRfgkAAK+HE4zp8G8AmOPrHmLZpZAAiANiNECVQTKktoD7BEoEmS8jaFBNMgRdcG0dttPurCAGXcKbWR1bzEzLmNvbXASQPjO6wxOfhtWsJ6oMWBg8Rs5STAGvQV2ArI5MG3KbpFrNSMxbx630Ars9d9j1ORsUS5v1biZRShZfg9180JuZAo=",
      "master_key" : "nHBk5DPexBjinXV8qHn7SEKzoxh2W92FxSbNTPgGtQYBzEF4msn9",
      "seq" : 1,
      "status" : "success"
   }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| Field          | 유형  | 설명                                                                                                                                                                                 |
| -------------- | --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| domain         | 문자열 | (생략 가능) 이 유효성 검사기와 연결된 도메인 이름(구성된 경우)입니다.                                                                                                                                          |
| ephemeral\_key | 문자열 | (생략될 수 있음) 이 서버가 유효성 검사 메시지에 서명하는 데 사용하는 임시 키 쌍의 공개 키(base58)입니다. 유효성 검사기의 구성된 토큰이 변경되면 이 키가 변경됩니다.                                                                                |
| manifest       | 문자열 | (생략될 수 있음) 이 유효성 검사기의 구성된 토큰에 해당하는 공개 "매니페스트"는 바이너리로 직렬화되고 base64로 인코딩됩니다. 이 필드에는 개인 정보가 포함되어 있지 않습니다.                                                                             |
| master\_key    | 문자열 | base58에 있는 이 유효성 검사기의 마스터 키 쌍의 공개 키입니다 . 이 키는 유효성 검사기를 고유하게 식별하며 유효성 검사기가 임시 키를 순환하는 경우 동일하게 유지됩니다. \[validation\_seed]가 아닌 를 사용하여 서버를 구성한 경우 \[validator\_token]이는 응답의 유일한 필드입니다. |
| seq            | 숫자  | (생략 가능) 이 검증인이 구성한 검증 토큰 및 설정에 대한 시퀀스 번호입니다. 이 숫자는 유효성 검사기 운영자가 임시 키를 회전하거나 설정을 변경하기 위해 유효성 검사기 토큰을 업데이트할 때마다 증가합니다.                                                               |

유효성 검증인 토큰 및 키 순환에 대한 자세한 내용은 [validator-keys-tool Guide](https://github.com/ripple/validator-keys-tool/blob/master/doc/validator-keys-tool-guide.md)를 참조하세요.

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 서버가 유효성 검증인으로 구성되지 않은 경우 서버는 "error\_message" : "not a validator"과 함께 이 오류를 반환합니다.
