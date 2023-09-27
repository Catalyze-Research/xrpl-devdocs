# submit\_multisigned

submit\_multisigned 명령은 다중 서명 트랜잭션을 적용하고 향후 ledger에 포함되도록 네트워크에 전송합니다. (submit 전용 모드에서 submit 명령을 사용하여 다중 서명 트랜잭션을 바이너리 형식으로 submit할 수도 있습니다.)

이 명령을 사용하려면 다중 서명 수정이 활성화되어 있어야 합니다. [![New in: rippled 0.31.0](https://img.shields.io/badge/New%20in-rippled%200.31.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/0.31.0)

## 요청 형식

요청 형식의 예시입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": "submit_multisigned_example",
    "command": "submit_multisigned",
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
        "Signers": [{
            "Signer": {
                "Account": "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW",
                "SigningPubKey": "02B3EC4E5DD96029A647CFA20DA07FE1F85296505552CCAC114087E66B46BD77DF",
                "TxnSignature": "30450221009C195DBBF7967E223D8626CA19CF02073667F2B22E206727BFE848FF42BEAC8A022048C323B0BED19A988BDBEFA974B6DE8AA9DCAE250AA82BBD1221787032A864E5"
            }
        }, {
            "Signer": {
                "Account": "rUpy3eEg8rqjqfUoLeBnZkscbKbFsKXC3v",
                "SigningPubKey": "028FFB276505F9AC3F57E8D5242B386A597EF6C40A7999F37F1948636FD484E25B",
                "TxnSignature": "30440220680BBD745004E9CFB6B13A137F505FB92298AD309071D16C7B982825188FD1AE022004200B1F7E4A6A84BB0E4FC09E1E3BA2B66EBD32F0E6D121A34BA3B04AD99BC1"
            }
        }],
        "SigningPubKey": "",
        "TransactionType": "TrustSet",
        "hash": "BD636194C48FD7A100DE4C972336534C8E710FD008C0F3CF7BC5BF34DAF3C3E6"
    }
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "submit_multisigned",
    "params": [
        {
            "tx_json": {
                "Account": "rEuLyBCvcw4CFmzv8RepSiAoNgF8tTGJQC",
                "Fee": "30000",
                "Flags": 262144,
                "LimitAmount": {
                    "currency": "USD",
                    "issuer": "rHb9CJAWyB4rj91VRWn96DkukG4bwdtyTh",
                    "value": "0"
                },
                "Sequence": 4,
                "Signers": [
                    {
                        "Signer": {
                            "Account": "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW",
                            "SigningPubKey": "02B3EC4E5DD96029A647CFA20DA07FE1F85296505552CCAC114087E66B46BD77DF",
                            "TxnSignature": "3045022100CC9C56DF51251CB04BB047E5F3B5EF01A0F4A8A549D7A20A7402BF54BA744064022061EF8EF1BCCBF144F480B32508B1D10FD4271831D5303F920DE41C64671CB5B7"
                        }
                    },
                    {
                        "Signer": {
                            "Account": "raKEEVSGnKSD9Zyvxu4z6Pqpm4ABH8FS6n",
                            "SigningPubKey": "03398A4EDAE8EE009A5879113EAA5BA15C7BB0F612A87F4103E793AC919BD1E3C1",
                            "TxnSignature": "3045022100FEE8D8FA2D06CE49E9124567DCA265A21A9F5465F4A9279F075E4CE27E4430DE022042D5305777DA1A7801446780308897699412E4EDF0E1AEFDF3C8A0532BDE4D08"
                        }
                    }
                ],
                "SigningPubKey": "",
                "TransactionType": "TrustSet",
                "hash": "81A477E2A362D171BB16BE17B4120D9F809A327FA00242ABCA867283BEA2F4F8"
            }
        }
    ]
}
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| `Field`     | 유형      | 필수 여부 | 설명                                                                    |
| ----------- | ------- | ----- | --------------------------------------------------------------------- |
| `tx_json`   | 객체      | 예     | 서명자 배열이 포함된 JSON 형식의 트랜잭션입니다. 성공하려면 서명의 가중치가 서명자 목록의 쿼럼과 같거나 높아야 합니다. |
| `fail_hard` | Boolean | 아니요   | 참이면 트랜잭션이 로컬에서 실패하면 트랜잭션을 다시 시도하거나 다른 서버로 릴레이하지 않습니다. 기본값은 false입니다.  |

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": "submit_multisigned_example",
  "status": "success",
  "type": "response",
  "result": {
    "engine_result": "tesSUCCESS",
    "engine_result_code": 0,
    "engine_result_message": "The transaction was applied. Only final in a validated ledger.",
    "tx_blob": "1200142200040000240000000263D5038D7EA4C680000000000000000000000000005553440000000000B5F762798A53D543A014CAF8B297CFF8F2F937E868400000000000753073008114A3780F5CB5A44D366520FC44055E8ED44D9A2270F3E010732102B3EC4E5DD96029A647CFA20DA07FE1F85296505552CCAC114087E66B46BD77DF744730450221009C195DBBF7967E223D8626CA19CF02073667F2B22E206727BFE848FF42BEAC8A022048C323B0BED19A988BDBEFA974B6DE8AA9DCAE250AA82BBD1221787032A864E58114204288D2E47F8EF6C99BCC457966320D12409711E1E0107321028FFB276505F9AC3F57E8D5242B386A597EF6C40A7999F37F1948636FD484E25B744630440220680BBD745004E9CFB6B13A137F505FB92298AD309071D16C7B982825188FD1AE022004200B1F7E4A6A84BB0E4FC09E1E3BA2B66EBD32F0E6D121A34BA3B04AD99BC181147908A7F0EDD48EA896C3580A399F0EE78611C8E3E1F1",
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
            "Account": "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW",
            "SigningPubKey": "02B3EC4E5DD96029A647CFA20DA07FE1F85296505552CCAC114087E66B46BD77DF",
            "TxnSignature": "30450221009C195DBBF7967E223D8626CA19CF02073667F2B22E206727BFE848FF42BEAC8A022048C323B0BED19A988BDBEFA974B6DE8AA9DCAE250AA82BBD1221787032A864E5"
          }
        },
        {
          "Signer": {
            "Account": "rUpy3eEg8rqjqfUoLeBnZkscbKbFsKXC3v",
            "SigningPubKey": "028FFB276505F9AC3F57E8D5242B386A597EF6C40A7999F37F1948636FD484E25B",
            "TxnSignature": "30440220680BBD745004E9CFB6B13A137F505FB92298AD309071D16C7B982825188FD1AE022004200B1F7E4A6A84BB0E4FC09E1E3BA2B66EBD32F0E6D121A34BA3B04AD99BC1"
          }
        }
      ],
      "SigningPubKey": "",
      "TransactionType": "TrustSet",
      "hash": "BD636194C48FD7A100DE4C972336534C8E710FD008C0F3CF7BC5BF34DAF3C3E6"
    }
  }
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
200 OK

{
    "result": {
        "engine_result": "tesSUCCESS",
        "engine_result_code": 0,
        "engine_result_message": "The transaction was applied. Only final in a validated ledger.",
        "status": "success",
        "tx_blob": "120014220004000024000000046380000000000000000000000000000000000000005553440000000000B5F762798A53D543A014CAF8B297CFF8F2F937E868400000000000753073008114A3780F5CB5A44D366520FC44055E8ED44D9A2270F3E010732102B3EC4E5DD96029A647CFA20DA07FE1F85296505552CCAC114087E66B46BD77DF74473045022100CC9C56DF51251CB04BB047E5F3B5EF01A0F4A8A549D7A20A7402BF54BA744064022061EF8EF1BCCBF144F480B32508B1D10FD4271831D5303F920DE41C64671CB5B78114204288D2E47F8EF6C99BCC457966320D12409711E1E010732103398A4EDAE8EE009A5879113EAA5BA15C7BB0F612A87F4103E793AC919BD1E3C174473045022100FEE8D8FA2D06CE49E9124567DCA265A21A9F5465F4A9279F075E4CE27E4430DE022042D5305777DA1A7801446780308897699412E4EDF0E1AEFDF3C8A0532BDE4D0881143A4C02EA95AD6AC3BED92FA036E0BBFB712C030CE1F1",
        "tx_json": {
            "Account": "rEuLyBCvcw4CFmzv8RepSiAoNgF8tTGJQC",
            "Fee": "30000",
            "Flags": 262144,
            "LimitAmount": {
                "currency": "USD",
                "issuer": "rHb9CJAWyB4rj91VRWn96DkukG4bwdtyTh",
                "value": "0"
            },
            "Sequence": 4,
            "Signers": [
                {
                    "Signer": {
                        "Account": "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW",
                        "SigningPubKey": "02B3EC4E5DD96029A647CFA20DA07FE1F85296505552CCAC114087E66B46BD77DF",
                        "TxnSignature": "3045022100CC9C56DF51251CB04BB047E5F3B5EF01A0F4A8A549D7A20A7402BF54BA744064022061EF8EF1BCCBF144F480B32508B1D10FD4271831D5303F920DE41C64671CB5B7"
                    }
                },
                {
                    "Signer": {
                        "Account": "raKEEVSGnKSD9Zyvxu4z6Pqpm4ABH8FS6n",
                        "SigningPubKey": "03398A4EDAE8EE009A5879113EAA5BA15C7BB0F612A87F4103E793AC919BD1E3C1",
                        "TxnSignature": "3045022100FEE8D8FA2D06CE49E9124567DCA265A21A9F5465F4A9279F075E4CE27E4430DE022042D5305777DA1A7801446780308897699412E4EDF0E1AEFDF3C8A0532BDE4D08"
                    }
                }
            ],
            "SigningPubKey": "",
            "TransactionType": "TrustSet",
            "hash": "81A477E2A362D171BB16BE17B4120D9F809A327FA00242ABCA867283BEA2F4F8"
        }
    }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`                 | 유형  | 설명                                                                     |
| ----------------------- | --- | ---------------------------------------------------------------------- |
| `engine_result`         | 문자열 | 트랜잭션의 예비 결과를 나타내는 코드(예: tesSUCCESS)                                    |
| `engine_result_code`    | 정수  | engine\_result\_code 트랜잭션의 예비 결과를 나타내는 숫자 코드로, 엔진 결과와 직접적으로 연관되어 있습니다. |
| `engine_result_message` | 문자열 | 사람이 읽을 수 있는 예비 트랜잭션 결과에 대한 설명                                          |
| `tx_blob`               | 문자열 | 16진수 문자열 형식의 전체 트랜잭션                                                   |
| `tx_json`               | 객체  | JSON 형식의 전체 트랜잭션 객체                                                    |

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* srcActMalformed - tx\_json의 계정 필드가 잘못되었거나 누락되었습니다.
* internal - 내부 오류가 발생했습니다. 여기에는 제공된 트랜잭션 JSON에 서명이 유효하지 않은 경우가 포함됩니다.
