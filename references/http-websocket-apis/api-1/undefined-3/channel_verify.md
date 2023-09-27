# channel\_verify

(페이찬 수정안에서 추가되어 활성화됨. [![New in: rippled 0.33.0](https://img.shields.io/badge/New%20in-rippled%200.33.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/0.33.0)_)_

channel\_verify 메소드는 결제 채널에서 특정 금액의 XRP를 교환하는 데 사용할 수 있는 서명의 유효성을 확인합니다.

## 요청 형식

요청 형식의 예시입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": 1,
    "command": "channel_verify",
    "channel_id": "5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3",
    "signature": "304402204EF0AFB78AC23ED1C472E74F4299C0C21F1B21D07EFC0A3838A420F76D783A400220154FB11B6F54320666E4C36CA7F686C16A3A0456800BBC43746F34AF50290064",
    "public_key": "aB44YfzW24VDEJQ2UuLPV2PvqcPCSoLnL7y5M1EzhdW4LnK5xMS3",
    "amount": "1000000"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "channel_verify",
    "params": [{
        "channel_id": "5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3",
        "signature": "304402204EF0AFB78AC23ED1C472E74F4299C0C21F1B21D07EFC0A3838A420F76D783A400220154FB11B6F54320666E4C36CA7F686C16A3A0456800BBC43746F34AF50290064",
        "public_key": "aB44YfzW24VDEJQ2UuLPV2PvqcPCSoLnL7y5M1EzhdW4LnK5xMS3",
        "amount": "1000000"
    }]
}
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| 필드           | 유형  | 설명                                                                                                                                                                                                               |
| ------------ | --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `amount`     | 문자열 | 제공된 서명이 승인하는 XRP의 양(드롭 단위)입니다.                                                                                                                                                                                   |
| `channel_id` | 문자열 | XRP를 제공하는 채널의 채널 ID입니다. 이는 64자의 16진수 문자열입니다.                                                                                                                                                                     |
| `public_key` | 문자열 | 채널의 공개 키와 서명을 생성하는 데 사용된 키 쌍(16진수 또는 XRP Ledger의 base58 형식)입니다.[![업데이트: 파문 0.90.0](https://img.shields.io/badge/Updated%20in-rippled%200.90.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.90.0) |
| `signature`  | 문자열 | 확인할 서명(16진수)입니다.                                                                                                                                                                                                 |

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": 1,
    "status": "success",
    "type": "response",
    "result": {
        "signature_verified":true
    }
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
200 OK

{
    "result": {
        "signature_verified":true,
        "status":"success"
    }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| 필드                   | 유형      | 설명                                          |
| -------------------- | ------- | ------------------------------------------- |
| `signature_verified` | Boolean | `true`인 경우 서명은 명시된 금액, 채널 및 공개 키에 대해 유효합니다. |

{% hint style="info" %}
Caution:

채널에 충분한 XRP가 할당되었는지 확인한다는 의미는 아닙니다. 청구를 유효한 것으로 간주하기 전에 최신 검증 ledger에서 채널을 조회하여 채널이 열려 있고 해당 채널의 금액이 청구 금액과 같거나 더 큰지 확인해야 합니다. 이를 확인하려면 account\_channel 메소드를 사용합니다.
{% endhint %}

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* publicMalformed - 요청의 public\_key 필드가 올바른 형식의 유효한 공개 키가 아닙니다. 공개 키는 33바이트이며 base58 또는 16진수로 표시해야 합니다. 베이스58로 표시되는 계정 공개 키는 문자 a로 시작하며, 16진수로 표시되는 공개 키의 길이는 66자입니다.
* channelMalformed - 요청의 channel\_id 필드가 유효한 채널 ID가 아닙니다. 채널 ID는 256비트(64자) 16진수 문자열이어야 합니다.
* channelAmtMalformed - 금액 필드에 지정된 값이 유효한 XRP 금액이 아닙니다.
