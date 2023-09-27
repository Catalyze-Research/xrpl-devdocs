# channel\_authorize

문자열(페이찬 수정안에서 추가되어 활성화됨. 신규: 리플 0.33.0 )

channel\_authorize 메소드는 결제 채널에서 특정 금액의 XRP를 상환하는 데 사용할 수 있는 서명을 생성합니다.

## 요청 형식

요청 형식의 예시입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": "channel_authorize_example_id1",
    "command": "channel_authorize",
    "channel_id": "5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3",
    "seed": "s████████████████████████████",
    "key_type": "secp256k1",
    "amount": "1000000",
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
POST http://localhost:5005/
Content-Type: application/json

{
    "method": "channel_authorize",
    "params": [{
        "channel_id": "5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3",
        "seed": "s████████████████████████████",
        "key_type": "secp256k1",
        "amount": "1000000"
    }]
}
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| 필드           | 유형  | 설명                                                                                                                                                                                                                                                                                                                                   |
| ------------ | --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `channel_id` | 문자열 | 사용할 결제 채널의 고유 ID입니다.                                                                                                                                                                                                                                                                                                                 |
| `secret`     | 문자열 | (선택 사항) 클레임에 서명하는 데 사용할 비밀 키입니다. 채널에 지정된 공개 키와 동일한 키 쌍이어야 합니다. secret, seed, 또는 passphrase와 함께 사용할 수 없습니다[![업데이트: 파문 1.4.0](https://img.shields.io/badge/Updated%20in-rippled%201.4.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.4.0)                                                                              |
| `seed`       | 문자열 | (선택 사항) 클레임에 서명하는 데 사용할 비밀 시드입니다. 채널에 지정된 공개 키와 동일한 키 쌍이어야 합니다. XRP Ledger의 base58 형식이어야 합니다. 제공된 경우 키 유형도 지정해야 합니다. secret, seed, 또는 passphrase와 함께 사용할 수 없습니다[![새로운 기능: Rippled 1.4.0](https://img.shields.io/badge/New%20in-rippled%201.4.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/1.4.0)                      |
| `seed_hex`   | 문자열 | (선택 사항) 클레임에 서명하는 데 사용할 비밀 시드입니다. 채널에 지정된 공개 키와 동일한 키 쌍이어야 합니다. 16진수 형식이어야 합니다. 제공하는 경우 키 유형도 지정해야 합니다. secret, seed, 또는 passphrase와 함께 사용할 수 없습니다[![새로운 기능: Rippled 1.4.0](https://img.shields.io/badge/New%20in-rippled%201.4.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.4.0)                                  |
| `passphrase` | 문자열 | (선택 사항) 클레임에 서명하는 데 사용할 문자열 패스프레이즈입니다. 채널에 지정된 공개 키와 동일한 키 쌍이어야 합니다. 이 암호 구문에서 파생된 키는 채널에 지정된 공개 키와 일치해야 합니다. 제공된 경우 key\_type도 지정해야 합니다. secret, seed, 또는 seed\_hex와 함께 사용할 수 없습니다[![새로운 기능: Rippled 1.4.0](https://img.shields.io/badge/New%20in-rippled%201.4.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.4.0) |
| `key_type`   | 문자열 | (선택 사항) 제공된 암호화 키 쌍의 서명 알고리즘입니다. 유효한 유형은 secp256k1 또는 ed25519입니다. 기본값은 secp256k1입니다.[![새로운 기능: Rippled 1.4.0](https://img.shields.io/badge/New%20in-rippled%201.4.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.4.0)                                                                                                |
| `amount`     | 문자열 | 승인할 XRP의 누적 금액(드롭 단위)입니다. 수신자가 이미 이 채널에서 더 적은 양의 XRP를 받은 경우, 이 방법으로 생성한 서명으로 차액을 상환할 수 있습니다.                                                                                                                                                                                                                                         |

요청은 `secret`, `seed`, `seed_hex`, 또는`passphrase` 중 정확히 하나를 지정해야 합니다.

{% hint style="info" %}
Warning:

신뢰할 수 없는 서버나 보안되지 않은 네트워크 연결을 통해 비밀 키를 보내지 마세요. (여기에는 이 요청의 secret, seed, seed\_hex 또는 passphrase 필드가 포함됩니다.) 이 방법은 본인이 운영하거나 자금을 전적으로 신뢰하는 서버에 대한 안전한 암호화된 네트워크 연결에서만 사용해야 합니다. 그렇지 않으면 도청자가 비밀 키를 사용하여 클레임에 서명하고 이 결제 채널과 동일한 키 쌍을 사용하는 다른 모든 결제 채널에서 모든 금액을 가져갈 수 있습니다. 자세한 내용은 보안 서명 설정을 참조하세요.
{% endhint %}

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": "channel_authorize_example_id1",
    "status": "success"
    "result": {
        "signature": "304402204EF0AFB78AC23ED1C472E74F4299C0C21F1B21D07EFC0A3838A420F76D783A400220154FB11B6F54320666E4C36CA7F686C16A3A0456800BBC43746F34AF50290064",
    }
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
200 OK

{
    "result": {
        "signature": "304402204EF0AFB78AC23ED1C472E74F4299C0C21F1B21D07EFC0A3838A420F76D783A400220154FB11B6F54320666E4C36CA7F686C16A3A0456800BBC43746F34AF50290064",
        "status": "success"
    }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| 필드          | 유형  | 설명                                                                                                                                                                   |
| ----------- | --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `signature` | 문자열 | 이 청구의 서명(16진수 값)입니다. 청구를 처리하려면 결제 채널의 대상 계정 에서 이 서명, 정확한 채널 ID, XRP 금액 및 채널의 공개 키가 포함된 [PaymentChannelClaim 거래를 보내야 합니다.](https://xrpl.org/paymentchannelclaim.html) |

## 발생 가능한 오류

* 모든 범용 오류 유형입니다.
* badKeyType - 요청의 key\_type 매개변수가 유효한 키 유형이 아닙니다. (유효한 유형은 secp256k1 또는 ed25519입니다.) 새로운 기능: rippled 1.4.0
* badSeed - 요청에 포함된 비밀 키가 유효한 비밀 키가 아닙니다.
* channelAmtMalformed - 요청에 포함된 금액이 유효한 XRP 금액이 아닙니다.
* channelMalformed - 요청의 channel\_id가 유효한 채널 ID가 아닙니다. 채널 ID는 256비트(64자) 16진수 문자열이어야 합니다.

&#x20;
