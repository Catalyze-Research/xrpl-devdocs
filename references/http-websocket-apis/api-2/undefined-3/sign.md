# sign

sign 메소드는 JSON 형식의 트랜잭션과 시드 값을 받아 트랜잭션의 서명된 바이너리 표현을 반환합니다. 다중 서명된 트랜잭션에 하나의 서명을 제공하려면 sign\_for 메소드를 대신 사용하세요.

기본적으로 이 메소드는 관리자 전용입니다. 서버 관리자가 공개 서명을 사용하도록 설정한 경우 공개 메소드로 사용할 수 있습니다.

{% hint style="info" %}
Caution:

rippled 서버를 직접 실행하지 않는 한 이 명령을 사용하는 대신 클라이언트 라이브러리를 사용하여 로컬 서명을 수행해야 합니다. 자세한 내용은 보안 서명 설정을 참조하세요.
{% endhint %}

## 요청 형식

요청 형식의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 2,
  "command": "sign",
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
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "sign",
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

{% tab title="Commandline" %}
```json
#Syntax: sign secret tx_json [offline]
rippled sign s████████████████████████████ '{"TransactionType": "Payment", "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn", "Destination": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX", "Amount": { "currency": "USD", "value": "1", "issuer" : "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn" }, "Sequence": 360, "Fee": "10000"}' offline
```
{% endtab %}
{% endtabs %}

트랜잭션에 서명하려면 트랜잭션을 인증할 수 있는 비밀 키를 제공해야 합니다. 일반적으로 서버가 비밀 키를 도출하는 시드 값을 제공합니다. 몇 가지 방법으로 이 작업을 수행할 수 있습니다:

* 비밀 필드에 시드를 입력하고 key\_type 필드를 생략합니다. 이 값은 XRP Ledger base58 시드, RFC-1751, 16진수 또는 문자열 암호 구문으로 포맷할 수 있습니다. (SECP256K1 키만 해당)
* key\_type 값과 `eed`, `seed_hex`, `passphrase` 중 정확히 한 가지를 입력합니다. 비밀 필드는 생략합니다. (커맨드라인 구문에서는 지원되지 않습니다.)

요청에는 다음 매개변수가 포함됩니다:

| Field          | 유형      | 설명                                                                                                                                                                                 |
| -------------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| tx\_json       | 객체      | JSON 형식의 [트랜잭션 정의](https://xrpl.org/transaction-formats.html)                                                                                                                      |
| secret         | 문자열     | (선택 사항) 트랜잭션에 서명하는 데 사용되는 트랜잭션을 제공하는 계정의 비밀 시드입니다. 신뢰할 수 없는 서버나 보안되지 않은 네트워크 연결을 통해 비밀번호를 보내지 마세요. `key_type`, `seed`, `seed_hex` 또는 `passphrase`와 함께 사용할 수 없습니다.                  |
| seed           | 문자열     | (선택 사항) 트랜잭션에 서명하는 데 사용되는 트랜잭션을 제공하는 계정의 비밀 시드입니다. XRP 원장의 base58 형식이어야 합니다. 제공된 경우 키 유형도 지정해야 합니다.`ecret`, `seed_hex` 또는 `passphrase`과 함께 사용할 수 없습니다.                             |
| seed\_hex      | 문자열     | (선택 사항) 트랜잭션에 서명하는 데 사용되는 트랜잭션을 제공하는 계정의 비밀 시드입니다. 16진수 형식이어야 합니다. 제공하는 경우 key\_type도 지정해야 합니다. `secret`, `seed` 또는 `passphrase` 구문과 함께 사용할 수 없습니다.                                |
| passphrase     | 문자열     | (선택 사항) 트랜잭션에 서명하는 데 사용되는 트랜잭션을 제공하는 계정의 비밀 시드를 문자열 비밀번호 구문으로 입력합니다. 제공하는 경우 key\_type도 지정해야 합니다. secret, seed 또는 seed\_hex와 함께 사용할 수 없습니다.                                        |
| key\_type      | 문자열     | (선택 사항) 제공된 암호화 키 쌍의 서명 알고리즘입니다. 유효한 유형은 secp256k1 또는 ed25519입니다. 기본값은 secp256k1입니다. secret과 함께 사용할 수 없습니다.                                                                        |
| offline        | Boolean | (선택 사항) 참이면 트랜잭션을 구성할 때 트랜잭션 세부 정보를 자동으로 채우려고 시도하지 않습니다. 기본값은 false입니다.                                                                                                            |
| build\_path    | Boolean | (선택 사항) 이 필드를 제공하면 서버가 서명하기 전에 결제 트랜잭션의 경로 필드를 자동으로 채웁니다. 트랜잭션이 직접 XRP 결제이거나 결제 유형 트랜잭션이 아닌 경우 이 필드를 생략해야 합니다. 주의: 서버는 이 필드의 값이 아니라 이 필드의 유무를 확인합니다. 이 동작은 변경될 수 있습니다. (이슈 #3272 ) |
| fee\_mult\_max | 정수      | (선택 사항) 자동 입력된 수수료 값이 기준 거래 비용 × fee\_mult\_max ÷ fee\_div\_max보다 크면 rpcHIGH\_FEE 오류와 함께 서명이 실패합니다. 트랜잭션의 수수료 필드를 명시적으로 지정하면 이 필드는 영향을 미치지 않습니다. 기본값은 10입니다.                       |
| fee\_div\_max  | 정수      | (선택 사항) 자동 입력된 수수료 값이 기준 트랜잭션 비용 × fee\_mult\_max ÷ fee\_div\_max보다 크면 rpcHIGH\_FEE 오류와 함께 서명이 실패합니다. 트랜잭션의 수수료 필드를 명시적으로 지정하면 이 필드는 영향을 미치지 않습니다. 기본값은 1입니다.                      |

## 자동 채우기 필드

tx\_json(트랜잭션 객체)의 특정 필드를 생략하면 서버가 자동으로 채우려고 시도합니다. 요청에서 오프라인을 true로 지정하지 않는 한 서버는 서명하기 전에 다음 필드를 제공합니다:

* 시퀀스 - 서버는 발신자의 계정 정보에서 다음 시퀀스 번호를 자동으로 사용합니다.

{% hint style="info" %}
Caution:

계정의 다음 시퀀스 번호는 이 트랜잭션이 적용될 때까지 증가하지 않습니다. 각 트랜잭션에 대한 응답을 제출하고 기다리지 않고 여러 트랜잭션에 서명하는 경우 첫 번째 트랜잭션 이후에 각 트랜잭션에 대해 올바른 시퀀스 번호를 수동으로 입력해야 합니다.
{% endhint %}

* Fee - Fee 매개변수를 생략하면 서버가 적절한 트랜잭션 비용을 자동으로 입력하려고 시도합니다. 프로덕션 XRP Ledger에서는 적절한 fee\_mult\_max 값을 제공하지 않으면 rpcHIGH\_FEE로 실패합니다.
  * fee\_mult\_max과 fee\_div\_max 매개변수는 참조 트랜잭션 비용에 적용되는 로드 스케일링 승수에 따라 자동으로 제공되는 트랜잭션 비용이 얼마나 높을 수 있는지를 제한합니다. 기본 설정은 자동 제공된 값이 10배 이상의 승수를 사용하는 경우 오류를 반환합니다. 그러나 프로덕션 XRP Ledger은 일반적으로 1000배의 로드 승수를 사용합니다.
  * 커맨드라인 구문은 fee\_mult\_max 및 fee\_div\_max를 지원하지 않습니다. 프로덕션 XRP Ledger의 경우 수수료 값을 제공해야 합니다.

{% hint style="info" %}
Caution:

악의적인 서버는 fee\_mult\_max와 fee\_div\_max 값을 무시하고 지나치게 높은 트랜잭션 비용을 지정할 수 있습니다.
{% endhint %}

* Paths - 결제 유형 트랜잭션(XRP에서 XRP로 이체 제외)의 경우, ripple\_path\_find 메소드를 사용한 것처럼 경로 필드를 자동으로 채울 수 있습니다. 빌드 경로가 제공된 경우에만 채워집니다.

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 2,
  "status": "success",
  "type": "response",
  "result": {
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
        "status": "success",
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
      "tx_blob" : "1200002280000000240000016861D4838D7EA4C6800000000000000000000000000055534400000000004B4E9C06F24296074F7BC48F92A97916C6DC5EA9684000000000002710732103AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB7447304502210094D24C795CFFA8E46FE338AF63421DA5CE5E171ED56F8E4CE70FFABA15D3CFA2022063994C52BF0393C8157EBFFCDE6A7E7EDC7B16A462CA53214F64CC8FCBB5E54A81144B4E9C06F24296074F7BC48F92A97916C6DC5EA983143E9D4A2B8AA0780F682D136F7A56D6724EF53754",
      "tx_json" : {
         "Account" : "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
         "Amount" : {
            "currency" : "USD",
            "issuer" : "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
            "value" : "1"
         },
         "Destination" : "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
         "Fee" : "10000",
         "Flags" : 2147483648,
         "Sequence" : 360,
         "SigningPubKey" : "03AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB",
         "TransactionType" : "Payment",
         "TxnSignature" : "304502210094D24C795CFFA8E46FE338AF63421DA5CE5E171ED56F8E4CE70FFABA15D3CFA2022063994C52BF0393C8157EBFFCDE6A7E7EDC7B16A462CA53214F64CC8FCBB5E54A",
         "hash" : "DE80DA6FF9F93FE4CE87C99441F403E0290E35867FF48382204CB89975BF343E"
      }
   }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| Field    | 유형  | 설명                                                                                      |
| -------- | --- | --------------------------------------------------------------------------------------- |
| tx\_blob | 문자열 | 정규화되고 서명된 트랜잭션을 16진수로 한바이너리 표현                                                          |
| tx\_json | 객체  | 자동으로 채워진 모든 필드를 포함하여 서명된 [전체 트랜잭션](https://xrpl.org/transaction-formats.html) 의 JSON 사양 |

{% hint style="info" %}
Caution:

이 명령으로 인해 오류 메시지가 표시되는 경우 요청에 제공된 비밀 값이 메시지에 포함될 수 있습니다. 이러한 오류가 다른 사람에게 표시되지 않도록 하세요.
{% endhint %}

* 여러 사람이 볼 수 있는 로그 파일에 이 오류를 기록하지 마세요.
* 디버깅을 위해 이 오류를 공개 장소에 붙여넣지 마세요.
* 실수로라도 웹사이트에 오류 메시지를 표시하지 마세요.

## 발생 가능한 오류 유형

* 일반적인 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* highFee - 트랜잭션 비용에 대한 현재 로드 기반 승수가 자동으로 제공되는 트랜잭션 비용의 한도를 초과합니다. 요청에 더 높은 fee\_mult\_max(최소 1000)를 지정하거나 tx\_json의 수수료 필드에 값을 수동으로 입력합니다.
* tooBusy - 트랜잭션에 경로가 포함되지 않았지만 서버가 너무 바빠서 지금 경로 찾기를 수행하지 못합니다. 관리자로 연결되어 있는 경우에는 발생하지 않습니다.
* noPath - 트랜잭션에 경로가 포함되지 않았으며 서버에서 이 결제가 발생할 수 있는 경로를 찾을 수 없습니다.
