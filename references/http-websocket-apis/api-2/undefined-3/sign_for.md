# sign\_for

sign\_for 명령은 다중 서명된 트랜잭션에 대해 하나의 서명을 제공합니다.

기본적으로 이 메소드는 관리자 전용입니다. 서버 관리자가 공개 서명을 사용하도록 설정한 경우 공개 방법으로 사용할 수 있습니다.

이 명령을 사용하려면 다중 서명 수정이 활성화되어 있어야 합니다. [![New in: rippled 0.31.0](https://img.shields.io/badge/New%20in-rippled%200.31.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/0.31.0)

## 요청 형식

요청 형식의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": "sign_for_example",
    "command": "sign_for",
    "account": "rLFd1FzHMScFhLsXeaxStzv3UC97QHGAbM",
    "seed": "s████████████████████████████",
    "key_type": "ed25519",
    "tx_json": {
        "TransactionType": "TrustSet",
        "Account": "rEuLyBCvcw4CFmzv8RepSiAoNgF8tTGJQC",
        "Flags": 262144,
        "LimitAmount": {
            "currency": "USD",
            "issuer": "rHb9CJAWyB4rj91VRWn96DkukG4bwdtyTh",
            "value": "100"
        },
        "Sequence": 2,
        "SigningPubKey": "",
        "Fee": "30000"
    }
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "sign_for",
    "params": [{
        "account": "rLFd1FzHMScFhLsXeaxStzv3UC97QHGAbM",
        "seed": "s████████████████████████████",
        "key_type": "ed25519",
        "tx_json": {
            "TransactionType": "TrustSet",
            "Account": "rEuLyBCvcw4CFmzv8RepSiAoNgF8tTGJQC",
            "Flags": 262144,
            "LimitAmount": {
                "currency": "USD",
                "issuer": "rHb9CJAWyB4rj91VRWn96DkukG4bwdtyTh",
                "value": "100"
            },
            "Sequence": 2,
            "SigningPubKey": "",
            "Fee": "30000"
        }
    }]
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
#Syntax: rippled sign_for <signer_address> <signer_secret> [offline]
rippled sign_for rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW s████████████████████████████ '{
    "TransactionType": "TrustSet",
    "Account": "rEuLyBCvcw4CFmzv8RepSiAoNgF8tTGJQC",
    "Flags": 262144,
    "LimitAmount": {
        "currency": "USD",
        "issuer": "rHb9CJAWyB4rj91VRWn96DkukG4bwdtyTh",
        "value": "100"
    },
    "Sequence": 2,
    "SigningPubKey": "",
    "Fee": "30000"
}'
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| Field      | 유형                                                           | 설명                                                                                                                                                                    |
| ---------- | ------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| account    | 문자열 - [주소](https://xrpl.org/basic-data-types.html#addresses) | 서명을 제공하는 주소입니다.                                                                                                                                                       |
| tx\_json   | 객체                                                           | 서명할 트랜잭션입니다. 서명 메서드를 사용할 때와 달리 수수료 및 시퀀스를 포함하여 트랜잭션의 모든 필드를 제공해야 합니다. 트랜잭션에는 빈 문자열이 값으로 포함된 SigningPubKey 필드가 포함되어야 합니다. 객체는 선택적으로 이전에 수집한 서명이 있는 서명자 배열을 포함할 수 있습니다. |
| secret     | 문자열                                                          | (선택 사항) 트랜잭션에 서명하는 데 사용되는 트랜잭션을 제공하는 계정의 비밀 키입니다. 신뢰할 수 없는 서버나 보안되지 않은 네트워크 연결을 통해 비밀 키를 보내지 마세요. 키 유형, 시드, 시드\_헥스 또는 비밀번호와 함께 사용할 수 없습니다.                            |
| seed       | 문자열                                                          | (선택 사항) 트랜잭션에 서명하는 데 사용되는 트랜잭션을 제공하는 계정의 비밀 키입니다. XRP 원장의 base58 형식이어야 합니다. 제공된 경우 키 유형도 지정해야 합니다. secret, seed\_hex 또는 암호문과 함께 사용할 수 없습니다.                           |
| seed\_hex  | 문자열                                                          | (선택 사항) 트랜잭션에 서명하는 데 사용되는 트랜잭션을 제공하는 계정의 비밀 키입니다. 16진수 형식이어야 합니다. 제공하는 경우 key\_type도 지정해야 합니다. 비밀, 시드 또는 비밀번호와 함께 사용할 수 없습니다.                                         |
| passphrase | 문자열                                                          | (선택 사항) 트랜잭션을 제공하는 계정의 비밀 키로, 서명하는 데 사용되는 문자열 비밀번호 구문입니다. 제공하는 경우 key\_type도 지정해야 합니다. secret, seed 또는 seed\_hex와 함께 사용할 수 없습니다.                                      |
| key\_type  | 문자열                                                          | (선택 사항) 이 요청에 제공된 암호화 키의 유형입니다. 유효한 유형은 secp256k1 또는 ed25519입니다. 기본값은 secp256k1입니다. secret와 함께 사용할 수 없습니다. 주의: Ed25519 지원은 실험 단계입니다.                                  |

다음 중 하나에 해당하는 비밀 키를 정확히 1개의 필드에 제공해야 합니다:

* 비밀값을 제공하고 key\_type 필드를 생략합니다. 이 값의 형식은 XRP 레저 베이스58 시드, RFC-1751, 16진수 또는 문자열 암호 구문으로 지정할 수 있습니다. (SECP256K1 키만 해당)
* key\_type 값과 `seed`, `seed_hex`, or `passphrase` 중 정확히 한 가지를 입력합니다. 비밀 필드는 생략합니다. (커맨드라인 구문에서는 지원되지 않습니다.)

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": "sign_for_example",
  "status": "success",
  "type": "response",
  "result": {
    "tx_blob": "1200142200040000240000000263D5038D7EA4C680000000000000000000000000005553440000000000B5F762798A53D543A014CAF8B297CFF8F2F937E868400000000000753073008114A3780F5CB5A44D366520FC44055E8ED44D9A2270F3E0107321EDDF4ECB8F34A168143B928D48EFE625501FB8552403BBBD3FC038A5788951D7707440C3DCA3FEDE6D785398EEAB10A46B44047FF1B0863FC4313051FB292C991D1E3A9878FABB301128FE4F86F3D8BE4706D53FA97F5536DBD31AF14CD83A5ACDEB068114D96CB910955AB40A0E987EEE82BB3CEDD4441AAAE1F1",
    "tx_json": {
      "Account": "rEuLyBCvcw4CFmzv8RepSiAoNgF8tTGJQC",
      "Fee": "30000",
      "Flags": 262144,
      "LimitAmount": {
        "currency": "USD",
        "issuer": "rHb9CJAWyB4rj91VRWn96DkukG4bwdtyTh",
        "value": "100"
      },
      "Sequence": 2,
      "Signers": [
        {
          "Signer": {
            "Account": "rLFd1FzHMScFhLsXeaxStzv3UC97QHGAbM",
            "SigningPubKey": "EDDF4ECB8F34A168143B928D48EFE625501FB8552403BBBD3FC038A5788951D770",
            "TxnSignature": "C3DCA3FEDE6D785398EEAB10A46B44047FF1B0863FC4313051FB292C991D1E3A9878FABB301128FE4F86F3D8BE4706D53FA97F5536DBD31AF14CD83A5ACDEB06"
          }
        }
      ],
      "SigningPubKey": "",
      "TransactionType": "TrustSet",
      "hash": "5216A13A3E3CF662352F0B430C7D82B7450415B6883DD428B5EC1DF1DE45DD8C"
    }
  }
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
200 OK

{
   "result" : {
      "status" : "success",
      "tx_blob" : "1200142200040000240000000263D5038D7EA4C680000000000000000000000000005553440000000000B5F762798A53D543A014CAF8B297CFF8F2F937E868400000000000753073008114A3780F5CB5A44D366520FC44055E8ED44D9A2270F3E010732102B3EC4E5DD96029A647CFA20DA07FE1F85296505552CCAC114087E66B46BD77DF744730450221009C195DBBF7967E223D8626CA19CF02073667F2B22E206727BFE848FF42BEAC8A022048C323B0BED19A988BDBEFA974B6DE8AA9DCAE250AA82BBD1221787032A864E58114204288D2E47F8EF6C99BCC457966320D12409711E1F1",
      "tx_json" : {
         "Account" : "rEuLyBCvcw4CFmzv8RepSiAoNgF8tTGJQC",
         "Fee" : "30000",
         "Flags" : 262144,
         "LimitAmount" : {
            "currency" : "USD",
            "issuer" : "rHb9CJAWyB4rj91VRWn96DkukG4bwdtyTh",
            "value" : "100"
         },
         "Sequence" : 2,
         "Signers" : [
            {
               "Signer" : {
                  "Account" : "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW",
                  "SigningPubKey" : "02B3EC4E5DD96029A647CFA20DA07FE1F85296505552CCAC114087E66B46BD77DF",
                  "TxnSignature" : "30450221009C195DBBF7967E223D8626CA19CF02073667F2B22E206727BFE848FF42BEAC8A022048C323B0BED19A988BDBEFA974B6DE8AA9DCAE250AA82BBD1221787032A864E5"
               }
            }
         ],
         "SigningPubKey" : "",
         "TransactionType" : "TrustSet",
         "hash" : "A94A6417D1A7AAB059822B894E13D322ED3712F7212CE9257801F96DE6C3F6AE"
      }
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
      "tx_blob" : "1200142200040000240000000263D5038D7EA4C680000000000000000000000000005553440000000000B5F762798A53D543A014CAF8B297CFF8F2F937E868400000000000753073008114A3780F5CB5A44D366520FC44055E8ED44D9A2270F3E010732102B3EC4E5DD96029A647CFA20DA07FE1F85296505552CCAC114087E66B46BD77DF744730450221009C195DBBF7967E223D8626CA19CF02073667F2B22E206727BFE848FF42BEAC8A022048C323B0BED19A988BDBEFA974B6DE8AA9DCAE250AA82BBD1221787032A864E58114204288D2E47F8EF6C99BCC457966320D12409711E1F1",
      "tx_json" : {
         "Account" : "rEuLyBCvcw4CFmzv8RepSiAoNgF8tTGJQC",
         "Fee" : "30000",
         "Flags" : 262144,
         "LimitAmount" : {
            "currency" : "USD",
            "issuer" : "rHb9CJAWyB4rj91VRWn96DkukG4bwdtyTh",
            "value" : "100"
         },
         "Sequence" : 2,
         "Signers" : [
            {
               "Signer" : {
                  "Account" : "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW",
                  "SigningPubKey" : "02B3EC4E5DD96029A647CFA20DA07FE1F85296505552CCAC114087E66B46BD77DF",
                  "TxnSignature" : "30450221009C195DBBF7967E223D8626CA19CF02073667F2B22E206727BFE848FF42BEAC8A022048C323B0BED19A988BDBEFA974B6DE8AA9DCAE250AA82BBD1221787032A864E5"
               }
            }
         ],
         "SigningPubKey" : "",
         "TransactionType" : "TrustSet",
         "hash" : "A94A6417D1A7AAB059822B894E13D322ED3712F7212CE9257801F96DE6C3F6AE"
      }
   }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| Field    | 유형  | 설명                                                                                                   |
| -------- | --- | ---------------------------------------------------------------------------------------------------- |
| tx\_blob | 문자열 | 새로 추가된 서명을 포함하여 서명된 트랜잭션의 16진수 표현입니다. 서명이 충분한 경우 제출 메서드를 사용하여 이 문자열을 제출할 수 있습니다.                     |
| tx\_json | 객체  | 배열에 새로 추가된 서명이 포함된 JSON 형식의 트랜잭션 명세서 객체입니다. 서명이 충분하면 submit\_multisigned 메서드를 사용하여 이 객체를 제출할 수 있습니다. |

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* srcActNotFound - 트랜잭션의 계정이 ledger에 있는 자금이 입금된 주소가 아닌 경우.
* srcActMalformed - 요청의 서명 주소(계정 필드)가 올바르게 형성되지 않은 경우.
* badSeed - 제공된 시드 값의 형식이 잘못되었습니다.
* badSecret - 제공된 비밀 값의 형식이 잘못되었습니다.
