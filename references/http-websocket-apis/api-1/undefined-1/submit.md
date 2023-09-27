# submit

제출 방법은 트랜잭션을 적용하고 네트워크에 전송하여 확인하여 향후 원장에 포함시킵니다.

이 명령에는 두 가지 모드가 있습니다:

* Submit-only 모드는 서명되고 직렬화된 트랜잭션을 바이너리 blob으로 가져와서 그대로 네트워크에 submit합니다. 서명된 트랜잭션 객체는 변경할 수 없으므로, submit 후 트랜잭션의 어떤 부분도 수정하거나 자동으로 채울 수 없습니다.
* Sign-and-submit 모드는 JSON 형식의 트랜잭션 개체를 가져와서 서명 방법과 동일한 방식으로 트랜잭션을 완성하고 서명한 다음 서명된 트랜잭션을 submit합니다. 이 모드는 테스트 및 개발용으로만 사용하는 것이 좋습니다.

트랜잭션을 최대한 강력하게 전송하려면 트랜잭션을 미리 구성하고 서명하여 정전 후에도 액세스할 수 있는 곳에 유지한 다음 tx\_blob으로 submit해야 합니다. submit 후 tx method 명령으로 네트워크를 모니터링하여 트랜잭션이 성공적으로 적용되었는지 확인하고, 재시작 또는 기타 문제가 발생하면 이전 트랜잭션과 시퀀스 번호가 동일하므로 두 번 적용되지 않으므로 안전하게 tx\_blob 트랜잭션을 다시 submit할 수 있습니다.

## Submit-Only 모드

submit-only 요청에는 다음 매개변수가 포함됩니다:

| `Field`     | 유형      | 필수 여부 | 설명                                                                      |
| ----------- | ------- | ----- | ----------------------------------------------------------------------- |
| `tx_blob`   | 문자열     | 예     | 제출할 서명된 트랜잭션의 16진수 표현입니다. 이는 다중 서명된 트랜잭션 일 수 있습니다.                      |
| `fail_hard` | Boolean | 아니요   | true이고 트랜잭션이 로컬에서 실패하면 트랜잭션을 다시 시도하거나 다른 서버로 릴레이하지 않습니다. 기본값은 false입니다. |

## 요청 형식

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": 3,
    "command": "submit",
    "tx_blob": "1200002280000000240000001E61D4838D7EA4C6800000000000000000000000000055534400000000004B4E9C06F24296074F7BC48F92A97916C6DC5EA968400000000000000B732103AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB7447304502210095D23D8AF107DF50651F266259CC7139D0CD0C64ABBA3A958156352A0D95A21E02207FCF9B77D7510380E49FF250C21B57169E14E9B4ACFD314CEDC79DDD0A38B8A681144B4E9C06F24296074F7BC48F92A97916C6DC5EA983143E9D4A2B8AA0780F682D136F7A56D6724EF53754"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "submit",
    "params": [
        {
            "tx_blob": "1200002280000000240000000361D4838D7EA4C6800000000000000000000000000055534400000000004B4E9C06F24296074F7BC48F92A97916C6DC5EA968400000000000000A732103AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB74473045022100D184EB4AE5956FF600E7536EE459345C7BBCF097A84CC61A93B9AF7197EDB98702201CEA8009B7BEEBAA2AACC0359B41C427C1C5B550A4CA4B80CF2174AF2D6D5DCE81144B4E9C06F24296074F7BC48F92A97916C6DC5EA983143E9D4A2B8AA0780F682D136F7A56D6724EF53754"
        }
    ]
}
```
{% endtab %}
{% endtabs %}

## Sign-and-Submit 모드

이 모드는 트랜잭션에 서명하고 즉시 submit합니다. 이 모드는 테스트용으로 사용할 수 있습니다. 다중 서명 트랜잭션에는 이 모드를 사용할 수 없습니다.

기본적으로 _sign-and-submit_ 모드는 관리자 전용입니다. 서버에서 공개 서명을 사용하도록 설정한 경우 공개 방법으로 사용할 수 있습니다.

다음과 같은 방법으로 트랜잭션에 서명하는 데 사용되는 비밀 키를 제공할 수 있습니다:

* 비밀값을 입력하고 key\_type 필드를 생략합니다. 이 값은 XRP Ledger 베이스58 시드, RFC-1751, 16진수 또는 문자열 암호 구문으로 형식화할 수 있습니다. (SECP256K1 키만 해당)
* 키 유형 값과 `seed`, `seed_hex` 또는 passphrase 중 정확히 하나를 입력합니다. 비밀 필드는 생략합니다. (명령줄 구문에서는 지원되지 않습니다.)

요청에는 다음 매개변수가 포함됩니다:

| `Field`        | 유형      | 설명                                                                                                                                                                                 |
| -------------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `tx_json`      | 객체      | JSON 형식의 트랜잭션 정의로, 선택적으로 자동 입력 가능한 필드를 생략할 수 있습니다.                                                                                                                                 |
| `secret`       | 문자열     | (선택 사항) 트랜잭션에 서명하는 데 사용되는 트랜잭션을 제공하는 계정의 비밀 키입니다. 신뢰할 수 없는 서버나 보안되지 않은 네트워크 연결을 통해 비밀번호를 보내지 마세요. `key_type`, `seed`, `seed_hex`, 또는 `passphrase`와 함께 사용할 수 없습니다.                  |
| `seed`         | 문자열     | (선택 사항) 트랜잭션에 서명하는 데 사용되는 트랜잭션을 제공하는 계정의 비밀 키입니다. XRP 레저의 base58 형식이어야 합니다. 제공하는 경우 키 유형도 지정해야 합니다. `secret`, `seed_hex`, 또는`passphrase` 함께 사용할 수 없습니다.                            |
| `seed_hex`     | 문자열     | (선택 사항) 트랜잭션에 서명하는 데 사용되는 트랜잭션을 제공하는 계정의 비밀 키입니다. 16진수 형식이어야 합니다. 제공하는 경우 키 유형도 지정해야 합니다. 비밀, 시드 또는 비밀번호와 함께 사용할 수 없습니다.                                                           |
| `passphrase`   | 문자열     | (선택 사항) 트랜잭션에 서명하는 데 사용되는 트랜잭션을 제공하는 계정의 비밀 키(문자열 암호 구문)를 입력합니다. 제공하는 경우 key\_type도 지정해야 합니다. secret, seed 또는 seed\_hex와 함께 사용할 수 없습니다.                                            |
| `key_type`     | 문자열     | (선택 사항) 이 요청에 제공된 암호화 키의 유형입니다. 유효한 유형은 secp256k1 또는 ed25519입니다. 기본값은 secp256k1입니다. secret와 함께 사용할 수 없습니다. 주의: Ed25519 지원은 실험 단계입니다.                                               |
| `fail_hard`    | Boolean | (선택 사항) true이고 트랜잭션이 로컬에서 실패하면 트랜잭션을 다시 시도하거나 다른 서버로 릴레이하지 않습니다. 기본값은 거짓입니다.                                                                                                       |
| `offline`      | Boolean | (선택 사항) true이면 트랜잭션을 구성할 때 자동으로 값을 채우거나 유효성을 검사하지 않습니다. 기본값은 거짓입니다.                                                                                                                |
| `build_path`   | Boolean | (선택 사항) 이 필드를 제공하면 서버가 서명하기 전에 결제 트랜잭션의 경로 필드를 자동으로 채웁니다. 트랜잭션이 직접 XRP 결제이거나 결제 유형 트랜잭션이 아닌 경우 이 필드를 생략해야 합니다. 주의: 서버는 이 필드의 값이 아니라 이 필드의 유무를 확인합니다. 이 동작은 변경될 수 있습니다. (이슈 #3272 ) |
| `fee_mult_max` | 정수      | (선택 사항) 자동 입력된 수수료 값이 기준 거래 비용 × fee\_mult\_max ÷ fee\_div\_max보다 큰 경우 서명 및 제출이 실패하고 rpcHIGH\_FEE 오류가 발생합니다. 트랜잭션의 수수료 필드를 명시적으로 지정하면 이 필드는 영향을 미치지 않습니다. 기본값은 10입니다.              |
| `fee_div_max`  | 정수      | (선택 사항) 자동 입력된 수수료 값이 기준 트랜잭션 비용 × fee\_mult\_max ÷ `fee_div_max`보다 크면 서명 및 제출이 실패하고 오류 rpcHIGH\_FEE가 표시됩니다. 트랜잭션의 수수료 필드를 명시적으로 지정하면 이 필드는 영향을 미치지 않습니다. 기본값은 1입니다.               |

서버가 특정 필드를 자동으로 채우는 방법에 대한 자세한 내용은 서명 메소드를 참조하세요.

## 요청 형식

요청 형식의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 2,
  "command": "submit",
  "tx_json" : {
      "TransactionType" : "Payment",
      "Account" : "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
      "Destination" : "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
      "Amount" : {
         "currency" : "USD",
         "value" : "1",
         "issuer" : "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn"
      }
   },
   "secret" : "s████████████████████████████",
   "offline": false,
   "fee_mult_max": 1000
}jso
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "submit",
    "params": [
        {
            "offline": false,
            "secret": "s████████████████████████████",
            "tx_json": {
                "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
                "Amount": {
                    "currency": "USD",
                    "issuer": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
                    "value": "1"
                },
                "Destination": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
                "TransactionType": "Payment"
            },
            "fee_mult_max": 1000
        }
    ]
}
```
{% endtab %}
{% endtabs %}

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
    "accepted" : true,
    "account_sequence_available" : 362,
    "account_sequence_next" : 362,
    "applied" : true,
    "broadcast" : true,
    "engine_result": "tesSUCCESS",
    "engine_result_code": 0,
    "engine_result_message": "The transaction was applied. Only final in a validated ledger.",
    "kept" : true,
    "open_ledger_cost": "10",
    "queued" : false,
    "tx_blob": "1200002280000000240000016861D4838D7EA4C6800000000000000000000000000055534400000000004B4E9C06F24296074F7BC48F92A97916C6DC5EA9684000000000002710732103AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB7446304402200E5C2DD81FDF0BE9AB2A8D797885ED49E804DBF28E806604D878756410CA98B102203349581946B0DDA06B36B35DBC20EDA27552C1F167BCF5C6ECFF49C6A46F858081144B4E9C06F24296074F7BC48F92A97916C6DC5EA983143E9D4A2B8AA0780F682D136F7A56D6724EF53754",
    "tx_json": {
      "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
      "Amount": {
        "currency": "USD",
        "issuer": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
        "value": "1"
      },
      "Destination": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
      "Fee": "10000",
      "Flags": 2147483648,
      "Sequence": 360,
      "SigningPubKey": "03AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB",
      "TransactionType": "Payment",
      "TxnSignature": "304402200E5C2DD81FDF0BE9AB2A8D797885ED49E804DBF28E806604D878756410CA98B102203349581946B0DDA06B36B35DBC20EDA27552C1F167BCF5C6ECFF49C6A46F8580",
      "hash": "4D5D90890F8D49519E4151938601EF3D0B30B16CD6A519D9C99102C9FA77F7E0"
    },
    "validated_ledger_index" : 21184416
  }
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "result": {
        "accepted" : true,
        "account_sequence_available" : 362,
        "account_sequence_next" : 362,
        "applied" : true,
        "broadcast" : true,
        "engine_result": "tesSUCCESS",
        "engine_result_code": 0,
        "engine_result_message": "The transaction was applied. Only final in a validated ledger.",
        "status": "success",
        "kept" : true,
        "open_ledger_cost": "10",
        "queued" : false,
        "tx_blob": "1200002280000000240000016961D4838D7EA4C6800000000000000000000000000055534400000000004B4E9C06F24296074F7BC48F92A97916C6DC5EA9684000000000002710732103AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB74473045022100A7CCD11455E47547FF617D5BFC15D120D9053DFD0536B044F10CA3631CD609E502203B61DEE4AC027C5743A1B56AF568D1E2B8E79BB9E9E14744AC87F38375C3C2F181144B4E9C06F24296074F7BC48F92A97916C6DC5EA983143E9D4A2B8AA0780F682D136F7A56D6724EF53754",
        "tx_json": {
            "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
            "Amount": {
                "currency": "USD",
                "issuer": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
                "value": "1"
            },
            "Destination": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
            "Fee": "10000",
            "Flags": 2147483648,
            "Sequence": 361,
            "SigningPubKey": "03AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB",
            "TransactionType": "Payment",
            "TxnSignature": "3045022100A7CCD11455E47547FF617D5BFC15D120D9053DFD0536B044F10CA3631CD609E502203B61DEE4AC027C5743A1B56AF568D1E2B8E79BB9E9E14744AC87F38375C3C2F1",
            "hash": "5B31A7518DC304D5327B4887CD1F7DC2C38D5F684170097020C7C9758B973847"
        }
    },
    "validated_ledger_index" : 21184416
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`                      | 유형      | 설명                                                                                                                                                                                                                                                                                                                                    |
| ---------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `engine_result`              | 문자열     | 트랜잭션의 예비 결과를 나타내는 텍스트 결과 코드(예: tesSUCCESS)                                                                                                                                                                                                                                                                                            |
| `engine_result_code`         | 정수      | 결과 코드의 숫자 버전. 권장하지 않습니다.                                                                                                                                                                                                                                                                                                              |
| `engine_result_message`      | 문자열     | 트랜잭션의 예비 결과에 대한 사람이 읽을 수 있는 설명입니다.                                                                                                                                                                                                                                                                                                    |
| `tx_blob`                    | 문자열     | 16진수 문자열 형식의 전체 트랜잭션                                                                                                                                                                                                                                                                                                                  |
| `tx_json`                    | 객체      | JSON 형식의 전체 트랜잭션 객체                                                                                                                                                                                                                                                                                                                   |
| `accepted`                   | Boolean | (서명 후 제출 모드에서는 생략) 참 값은 트랜잭션이 적용, 큐에 대기, 브로드캐스트 또는 나중에 보류되었음을 나타냅니다. 거짓 값은 이러한 일이 발생하지 않았음을 나타내므로 트랜잭션을 다시 제출하지 않고 다른 시간에 이미 제출하지 않는 한 트랜잭션이 성공할 수 없습니다[![새로운 기능: Rippled 1.5.0](https://img.shields.io/badge/New%20in-rippled%201.5.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.5.0)                              |
| `account_sequence_available` | 숫자      | (서명 후 제출 모드에서는 생략) 보류 중이거나 대기 중인 모든 트랜잭션 이후에 보내는 계정에서 사용할 수 있는 다음 시퀀스 번호입니다.[![새로운 기능: Rippled 1.5.0](https://img.shields.io/badge/New%20in-rippled%201.5.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.5.0)                                                                                                         |
| `account_sequence_next`      | 숫자      | (서명 후 제출 모드에서는 생략) 대기열에 있는 트랜잭션이 아닌, 잠정적으로 적용된 모든 트랜잭션 이후에 보내는 계정의 다음 시퀀스 번호입니다.[![새로운 기능: Rippled 1.5.0](https://img.shields.io/badge/New%20in-rippled%201.5.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.5.0)                                                                                                     |
| `applied`                    | Boolean | (서명 후 제출 모드에서는 생략) 참 값은 이 트랜잭션이 열린 원장에 적용되었음을 나타냅니다. 이 경우 트랜잭션이 다음 원장 버전에서 검증될 가능성이 높지만 보장되지는 않습니다.[![새로운 기능: Rippled 1.5.0](https://img.shields.io/badge/New%20in-rippled%201.5.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.5.0)                                                                                  |
| `broadcast`                  | Boolean | (서명 후 제출 모드에서는 생략) 참 값은 이 트랜잭션이 P2P XRP 레저 네트워크의 피어 서버에 브로드캐스트되었음을 나타냅니다. (참고: 독립형 모드와 같이 서버에 피어가 없는 경우, 서버는 트랜잭션을 브로드캐스트했을 경우에 참 값을 사용합니다). 거짓 값은 트랜잭션이 다른 서버에 브로드캐스트되지 않았음을 나타냅니다.[![새로운 기능: Rippled 1.5.0](https://img.shields.io/badge/New%20in-rippled%201.5.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.5.0) |
| `kept`                       | Boolean | (서명 후 제출 모드에서는 생략) 참 값은 트랜잭션이 나중에 재시도할 수 있도록 보관되었음을 나타냅니다.[![새로운 기능: Rippled 1.5.0](https://img.shields.io/badge/New%20in-rippled%201.5.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.5.0)                                                                                                                           |
| `queued`                     | Boolean | (서명 후 제출 모드에서는 생략) 참 값은 트랜잭션이 트랜잭션 대기열에 저장되었음을 나타내며, 이는 향후 원장 버전에 포함될 가능성이 있음을 의미합니다.[![새로운 기능: Rippled 1.5.0](https://img.shields.io/badge/New%20in-rippled%201.5.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.5.0)                                                                                                |
| `open_ledger_cost`           | 문자열     | (서명 후 제출 모드에서는 생략) 이 트랜잭션을 처리하기 전의 현재 오픈 원장 비용입니다. 비용이 낮은 트랜잭션이 대기열에 오를 가능성이 높습니다.[![새로운 기능: Rippled 1.5.0](https://img.shields.io/badge/New%20in-rippled%201.5.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.5.0)                                                                                                   |
| `validated_ledger_index`     | 정수      | (서명 후 제출 모드에서는 생략) 제출 시점에 가장 최근에 유효성이 검증된 원장의 원장 인덱스입니다. 이는 이 요청의 결과로 트랜잭션이 나타날 수 있는 원장 버전에 대한 하한선을 제공합니다. (트랜잭션이 이전에 이미 제출된 적이 있다면 이 원장 버전 또는 그 이전 버전에서만 유효성을 검사할 수 있습니다.)                                                                                                                                                           |

{% hint style="info" %}
Warning:

웹소켓 응답에 "상태":"성공"이 있어 명령이 성공적으로 수신되었음을 나타내더라도 트랜잭션이 성공적으로 실행되었음을 나타내는 것은 아닙니다. 결제에서 두 계정을 연결하는 신뢰 라인이 부족하거나 트랜잭션이 생성된 이후 ledger의 상태가 변경되는 등 여러 가지 상황으로 인해 트랜잭션이 성공적으로 처리되지 않을 수 있습니다. 문제가 없더라도 트랜잭션이 포함된 ledger 버전을 닫고 유효성을 검사하는 데 몇 초가 걸릴 수 있습니다. 자세한 내용은 트랜잭션 응답의 전체 목록을 참조하고, 트랜잭션의 결과가 검증된 ledger 버전에 표시될 때까지 최종적인 것으로 간주하지 마세요.
{% endhint %}

{% hint style="info" %}
Caution:

이 명령으로 오류 메시지가 표시되는 경우 요청의 비밀 키가 메시지에 포함될 수 있습니다. (서명 후 submit 모드에서만 발생할 수 있습니다.) 이러한 오류가 다른 사람에게 보이지 않는지 확인하세요.
{% endhint %}

* 여러 사람이 볼 수 있는 로그 파일에 비밀 키를 포함한 오류를 기록하지 마세요.
* 디버깅을 위해 비밀키를 포함한 오류를 공개 장소에 붙여넣지 마세요.
* 실수로라도 웹사이트에 비밀 키를 포함한 오류 메시지를 표시하지 마세요.

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* amendmentBlocked - rippled 서버가 수정 차단되어 트랜잭션을 네트워크에 submit할 수 없습니다.
* highFee - fee\_mult\_max 매개변수가 지정되었지만 서버의 현재 수수료 승수가 지정된 승수를 초과합니다. (서명 및 submit 모드만 해당)
* internalJson - 트랜잭션을 JSON으로 직렬화할 때 내부 오류가 발생했습니다. 서명이 잘못되었거나 일부 필드가 잘못되는 등 트랜잭션의 여러 측면으로 인해 발생할 수 있습니다.
* internalSubmit- 트랜잭션을 submit할 때 내부 오류가 발생했습니다. 서명이 잘못되었거나 일부 필드가 잘못되는 등 트랜잭션의 여러 측면으로 인해 발생할 수 있습니다.
* internalTransaction - 트랜잭션을 처리하는 동안 내부 오류가 발생했습니다. 서명이 잘못되었거나 일부 필드가 잘못되는 등 트랜잭션의 여러 측면으로 인해 발생할 수 있습니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* invalidTransaction - 트랜잭션이 잘못되었거나 유효하지 않습니다.
* noPath - 트랜잭션에 경로가 포함되어 있지 않으며 서버에서 이 결제가 발생할 수 있는 경로를 찾을 수 없습니다. (서명 및 submit 모드만 해당)
* tooBusy - 트랜잭션에 경로가 포함되어 있지 않지만 서버가 지금 너무 바빠서 경로 찾기를 수행할 수 없습니다. 관리자로 연결되어 있는 경우에는 발생하지 않습니다. (서명 후 submit 모드만 해당)
* notSupported - 이 서버에서는 서명이 지원되지 않습니다(서명 및 submit 모드만 해당). 서버 관리자인 경우 관리자로 연결되어도 서명에 액세스할 수 있거나 공개 서명을 활성화할 수 있습니다. 새로운 기능: 리플 1.1.0
