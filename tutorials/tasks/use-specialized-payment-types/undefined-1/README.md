# 결제 채널 사용

결제 채널은 아주 작은 단위로 나눠서 나중에 정산할 수 있는 "비동기" XRP 결제를 전송하는 고급 기능입니다. 이 튜토리얼에서는 로컬 rippled 서버의 JSON-RPC API를 사용하는 예시를 통해 결제 채널 사용의 전체 과정을 안내합니다.

이 튜토리얼을 단계별로 진행하려면 두 사람이 각각 펀딩된 XRP Ledger 계정의 키를 가지고 있는 것이 가장 이상적입니다. 하지만, 한 사람이 두 개의 XRP Ledger 주소를 관리하는 상태로 튜토리얼을 진행할 수도 있습니다.

## 예제 값

이 튜토리얼에 사용된 예시 주소는 다음과 같습니다:

| **결제자 주소**                                       | `rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH`                                 |
| ------------------------------------------------ | -------------------------------------------------------------------- |
| **채널에 사용되는 공개 키(XRP Ledger의 base58 인코딩 문자열 형식)** | `aB44YfzW24VDEJQ2UuLPV2PvqcPCSoLnL7y5M1EzhdW4LnK5xMS3`               |
| **채널에 사용되는 공개 키(16진수)**                          | `023693F15967AE357D0327974AD46FE3C127113B1110D6044FD41E723689F81CC6` |
| **수취인 주소**                                       | `rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn`                                 |

{% hint style="info" %}
Tip:

이 예제에서 채널의 공개 키는 결제자 마스터 키 쌍의 공개 키입니다. 이는 완벽하게 안전하고 유효합니다. 결제자만 해당 키 쌍의 공개키와 비밀키를 알고 있다면 다른 키 쌍을 사용하는 것도 완벽하게 안전하고 유효합니다.
{% endhint %}

또한 트랜잭션을 전송할 rippled 서버가 필요합니다. 이 튜토리얼의 예시에서는 rippled 서버가 테스트 머신(로컬 호스트)에서 포트 5005의 암호화되지 않은 JSON-RPC API 엔드포인트로 실행되고 있다고 가정합니다.

실제 XRP를 전송하지 않고 테스트하려면 testnet XRP와 함께 XRP Ledger testnet 주소를 사용할 수 있습니다. testnet을 사용하는 경우 http://localhost:5005/ 대신 https://api.altnet.rippletest.net:51234 에 연결하여 testnet 서버의 JSON-RPC API를 사용할 수 있습니다.

결제 채널에는 원하는 금액의 XRP를 사용할 수 있습니다. 이 튜토리얼의 예시 값은 결제 채널에 최소 1일 동안 100RP(100000000드롭)를 따로 설정했습니다.

## 흐름 다이어그램

다음 다이어그램은 결제 채널의 수명 주기를 요약한 것입니다:

<figure><img src="https://xrpl.org/img/paychan-flow.png" alt=""><figcaption></figcaption></figure>

이 다이어그램의 번호가 매겨진 단계를 이 튜토리얼의 단계와 일치시킬 수 있습니다.

1. 결제자: 채널 생성
2. 수취인: 채널 확인
3. 지급인 청구 서명
4. 수취인: 수취인에게 청구 보내기
5. 수취인: 청구 확인
6. 수취인 상품 또는 서비스 제공
7. 원하는 대로 3\~6단계를 반복합니다.
8. 수취인: 클레임 상환
9. 지급인: 채널 폐쇄 요청
10. 지급인(또는 다른 사람): 만료된 채널 닫기

## 1. 결제자가 특정 수신자에 대한 결제 채널을 생성합니다.

이것이 PaymentChannelCreate 트랜잭션입니다. 이 프로세스의 일부로 결제자는 채널의 청구에 대한 보증에 영향을 주는 만료 시간 및 결제 지연과 같은 채널의 특정 세부 정보를 설정합니다. 또한 지급자는 채널에 대한 청구를 확인하는 데 사용할 공개 키도 설정합니다.

{% hint style="info" %}
Tip:

'정산 지연'은 정산을 지연시키지 않으며, ledger버전이 닫히는 시간(3\~5초)만큼 빠르게 정산이 이루어질 수 있습니다. '정산 지연'은 수취인이 정산을 완료할 수 있도록 채널을 닫을 때 강제적으로 지연되는 것입니다.&#x20;
{% endhint %}

다음 예시는 JSON-RPC API로 로컬 rippled 서버에 제출하여 결제 채널을 생성하는 예시입니다. 결제 채널은 예시 지급인(rN7n7...)으로부터 예시 수취인(rf1Bi...)에게 1일의 결제 지연을 두고 100개의 XRP를 할당합니다. 공개 키는 예시 지급인의 마스터 공개 키(16진수)입니다.

{% hint style="info" %}
Note:

결제 채널은 결제자의 소유자 reserve에 하나의 객체로 계산됩니다. 소유자는 결제 채널에 할당된 XRP를 차감한 후에도 준비금을 충족할 수 있는 최소한의 XRP를 보유해야 합니다.
{% endhint %}

요청:

````json
POST http://localhost:5005/
Content-Type: application/json

{
    "method": "submit",
    "params": [{
        "secret": "s████████████████████████████",
        "tx_json": {
            "Account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
            "TransactionType": "PaymentChannelCreate",
            "Amount": "100000000",
            "Destination": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
            "SettleDelay": 86400,
            "PublicKey": "023693F15967AE357D0327974AD46FE3C127113B1110D6044FD41E723689F81CC6",
            "DestinationTag": 20170428
        },
        "fee_mult_max": 1000
    }]
}
```json

Response:

```json
200 OK

{
    "result": {
        "engine_result": "tesSUCCESS",
        "engine_result_code": 0,
        "engine_result_message": "The transaction was applied. Only final in a validated ledger.",
        ...
        "tx_json": {
            ...
            "TransactionType": "PaymentChannelCreate",
            "hash": "3F93C482C0BC2A1387D9E67DF60BECBB76CC2160AE98522C77AF0074D548F67D"
        }
    }
}
````

제출 요청에 대한 즉각적인 응답에는 트랜잭션의 식별 해시값이 포함된 잠정 결과가 포함됩니다. 결제자는 검증된 ledger에서 트랜잭션의 최종 결과를 확인하고 메타데이터에서 채널 ID를 가져와야 합니다. 이 작업은 tx 명령으로 수행할 수 있습니다:

요청:

```json
POST http://localhost:5005/
Content-Type: application/json

{
    "method": "tx",
    "params": [{
        "transaction": "3F93C482C0BC2A1387D9E67DF60BECBB76CC2160AE98522C77AF0074D548F67D"
    }]
}
```

&#x20;응답:

```json
200 OK

{
    "result": {
        "Account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "Amount": "100000000",
        "Destination": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
        ...
        "TransactionType": "PaymentChannelCreate",
        ...
        "hash": "3F93C482C0BC2A1387D9E67DF60BECBB76CC2160AE98522C77AF0074D548F67D",
        "inLedger": 29380080,
        "ledger_index": 29380080,
        "meta": {
            "AffectedNodes": [
                ...
                {
                    "CreatedNode": {
                        "LedgerEntryType": "PayChannel",
                        "LedgerIndex": "5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3",
                        "NewFields": {
                            "Account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
                            "Amount": "100000000",
                            "Destination": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
                            "DestinationTag": 20170428,
                            "PublicKey": "023693F15967AE357D0327974AD46FE3C127113B1110D6044FD41E723689F81CC6",
                            "SettleDelay": 86400
                        }
                    }
                },
                ...
            ],
            "TransactionIndex": 16,
            "TransactionResult": "tesSUCCESS"
        },
        "status": "success",
        "validated": true
    }
}
```

JSON-RPC의 응답에서 결제자는 다음을 찾아야 합니다:

* 트랜잭션의 메타 필드에서 트랜잭션 결과가 tesSUCCESS인지 확인합니다.
* 응답이 "validated":true인지 확인하여 데이터가 유효성 검증된 ledger에서 가져온 것임을 표시합니다. (유효성이 검증된 ledger 버전에 나타나는 경우에만 tesSUCCESS 결과가 최종적입니다.)
* 트랜잭션 메타 필드의 AffectedNodes 배열에서 LedgerEntryType이 PayChannel인 CreatedNode 객체를 찾습니다. CreatedNode 객체의 LedgerIndex 필드는 채널 ID를 나타냅니다. (위 예제에서는 "5DB0..."로 시작하는 16진수 문자열입니다.) 채널 ID는 나중에 청에 서명하는 데 필요합니다. PayChannel ledger 객체 유형에 대한 자세한 내용은 PayChannel ledger 객체를 참조하세요.

## 2. 수취인이 결제 채널의 세부 정보를 확인합니다.

다음 예제에서와 같이 채널의 수취인을 사용하여 [account\_channels](https://xrpl.org/account\_channels.html) 메소드로 결제 채널을 조회할 수 있습니다(JSON-RPC API 사용):

요청:

```json
POST http://localhost:5005/
Content-Type: application/json

{
    "method": "account_channels",
    "params": [{
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "destination_account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
        "ledger_index": "validated"
    }]
}
```

&#x20;응답:

```json
200 OK

{
    "result": {
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "channels": [{
            "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
            "amount": "100000000",
            "balance": "0",
            "channel_id": "5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3",
            "destination_account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
            "destination_tag": 20170428,
            "public_key": "aB44YfzW24VDEJQ2UuLPV2PvqcPCSoLnL7y5M1EzhdW4LnK5xMS3",
            "public_key_hex": "023693F15967AE357D0327974AD46FE3C127113B1110D6044FD41E723689F81CC6",
            "settle_delay": 86400
        }],
        "status": "success"
    }
}
```

수취인은 결제 채널의 매개변수가 다음 사항을 모두 포함하여 특정 사용 사례에 적합한지 확인해야 합니다:

* destination\_account 필드에 수취인의 주소가 올바른지 확인합니다.&#x20;
* 수취인이 미결제 청구를 상환할 수 있는 충분한 시간을 제공하는 결제 지연 시간(초)이 settle\_delay 필드에 입력되어 있는지 확인합니다.&#x20;
* cancel\_after(변경 불가능한 만료) 및 expiration(변경 가능한 만료) 필드가 있는 경우 너무 이르지 않은지 확인합니다. 수취인은 이 시간을 기록해 두었다가 그 전에 청구를 상환할 수 있도록 해야 합니다.&#x20;
* 공개 키와 채널 ID 필드를 기록해 두세요. 나중에 클레임을 확인하고 상환하는 데 필요합니다.&#x20;
* (선택 사항) destination\_tag 필드가 존재하고 원하는 데스티네이션태그가 있는지 확인합니다.&#x20;

동일한 두 당사자 간에 여러 채널이 있을 수 있으므로 수취인이 올바른 채널의 자격을 확인하는 것이 중요합니다. 혼동할 가능성이 있는 경우, 수취인은 사용할 채널의 채널 ID(channel\_id)를 명확히 해야 합니다.

## 3. 결제자가 채널에서 XRP에 대해 하나 이상의 서명된 _청구_를 생성합니다.

이러한 청구 금액은 결제자가 지불하고자 하는 특정 상품이나 서비스에 따라 다릅니다.

각 청구는 누적 금액에 대한 청구여야 합니다. 즉, 각각 10XRP로 두 개의 상품을 구매하려면 첫 번째 청구 금액은 10XRP이고 두 번째 청구의 금액은 20XRP여야 합니다. 청구 금액은 채널에 할당된 총 XRP 금액을 초과할 수 없습니다. ([PaymentChannelFund](https://xrpl.org/paymentchannelfund.html) 트랜잭션은 채널에 할당된 총 XRP 금액을 늘릴 수 있습니다.)

channel\_authorize 메소드로 클레임을 생성할 수 있습니다. 다음 예시는 채널에서 1 XRP를 승인하는 예시입니다:

요청:

```json
POST http://localhost:5005/
Content-Type: application/json

{
    "method": "channel_authorize",
    "params": [{
        "channel_id": "5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3",
        "secret": "s████████████████████████████",
        "amount": "1000000"
    }]
}
```

&#x20;응답:

```json
{
    "result": {
        "signature": "304402204EF0AFB78AC23ED1C472E74F4299C0C21F1B21D07EFC0A3838A420F76D783A400220154FB11B6F54320666E4C36CA7F686C16A3A0456800BBC43746F34AF50290064",
        "status": "success"
    }
}
```

## 4. 지급인이 수취인에게 상품이나 서비스에 대한 대금으로 청구를 보냅니다.

이 통신은 지급인과 수취인이 동의할 수 있는 모든 통신 시스템에서 "ledger 외부"에서 이루어집니다. 이를 위해 보안 통신을 사용해야 하지만 반드시 보안 통신을 사용해야 하는 것은 아닙니다. 채널의 지급인 또는 수취인만 해당 채널에 대한 청구를를 상환할 수 있습니다.

청구의 정확한 형식은 다음 정보를 전달하는 한 중요하지 않습니다:

| Field                   | Example                                                                                                                                                                                                                                                 |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Channel ID              | `5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3`                                                                                                                                                                                      |
| Amount of XRP, in drops | `1000000`                                                                                                                                                                                                                                               |
| Signature               | <p><code>304402204EF0AFB78AC23ED1C472E74F4299C0C21F1B21D07EFC0A3838A420F76D783A</code><br><code>400220154FB11B6F54320666E4C36CA7F686C16A3A0456800BBC43746F34AF50290064</code> <em>(Note: this long string has been broken to fit on one line.)</em></p> |

수취인은 채널과 연결된 공개 키를 알고 있어야 하며, 이 공개 키는 채널의 수명이 다할 때까지 동일합니다.

## 5. 수취인이 청구를 확인합니다.

channel\_verify 메소드를 사용하여 청구를 확인할 수 있습니다. 수취인은 청구 금액이 제공된 상품 및 서비스의 총 가격보다 크거나 같은지 확인해야 합니다. (금액은 누적 금액이므로 지금까지 구매한 모든 상품 및 서비스의 총 가격입니다.)

JSON-RPC API로 channel\_verify를 사용하는 예시입니다:

요청:

```json
POST http://localhost:5005/
Content-Type: application/json

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

응답:

```json
200 OK

{
    "result": {
        "signature_verified":true,
        "status":"success"
    }
}
```

응답에 "signature\_verified": true가 표시되면 청구 서명이 진짜인 것입니다. 또한 수취인은 채널에 클레임을 처리할 수 있는 충분한 XRP가 있는지 확인해야 합니다. 이를 위해 수취인은 [account\_channels](https://xrpl.org/account\_channels.html) 메소드를 사용하여 결제 채널의 가장 최근 유효성이 확인된 상태를 확인합니다.

요청:

```json
POST http://localhost:5005/
Content-Type: application/json

{
    "method": "account_channels",
    "params": [{
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "destination_account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
        "ledger_index": "validated"
    }]
}
```

&#x20;응답:

```json
200 OK

{
    "result": {
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "channels": [{
            "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
            "amount": "100000000",
            "balance": "0",
            "channel_id": "5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3",
            "destination_account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
            "destination_tag": 20170428,
            "public_key": "aB44YfzW24VDEJQ2UuLPV2PvqcPCSoLnL7y5M1EzhdW4LnK5xMS3",
            "public_key_hex": "023693F15967AE357D0327974AD46FE3C127113B1110D6044FD41E723689F81CC6",
            "settle_delay": 86400
        }],
        "status": "success"
    }
}
```

수취인은 다음을 확인해야 합니다:

* 채널 배열에서 channel\_id가 청구의 채널 ID와 일치하는 개체를 찾습니다. 동일한 당사자 간에도 여러 결제 채널을 보유할 수 있지만, 청구는 ID가 일치하는 채널에 대해서만 사용할 수 있습니다.
* 채널의 만료(변경 가능한 만료)가 있는 경우 너무 이르지 않은지 확인합니다. 수취인은 이 시간 이전에 청구를 상환해야 합니다.
* 청구 금액이 채널 금액과 같거나 적은지 확인합니다. 청구 금액이 더 높으면 지급자가 PaymentChannelFund 트랜잭션을 사용하여 채널에서 사용할 수 있는 총 XRP 금액을 늘리지 않는 한 청구 금액을 상환할 수 없습니다.
* 채널 잔액이 수취인이 채널에서 이미 받았을 것으로 예상되는 금액과 일치하는지 확인합니다. 일치하지 않는 경우 수취인은 채널의 거래 내역을 다시 확인해야 합니다. 불일치에 대한 몇 가지 가능한 설명은 다음과 같습니다:
  * 송금인이 PaymentChannelClaim 트랜잭션을 사용하여 채널에서 수취인에게 XRP를 전달했지만 수취인이 수신 트랜잭션을 알아채지 못하고 기록하지 않은 경우입니다.
  * 수취인의 기록에 '전송 중'이거나 최신 검증 ledger 버전에 아직 포함되지 않은 트랜잭션이 포함된 경우. 수취인은 tx 메소드를 사용하여 개별 거래의 상태를 조회하여 이를 확인할 수 있습니다.
  * account\_channels 요청에 올바른 ledger 버전이 지정되지 않았습니다. (최신 유효성이 검증된 ledger 버전을 가져오려면 "ledger\_index": "validated"를 사용하세요.)
  * 수취인이 이전에 XRP를 상환했지만 기록하는 것을 잊었습니다.
  * 수취인이 XRP 상환을 시도하고 잠정 결과를 기록했지만 거래의 최종 유효성 검사 결과가 동일하지 않아 수취인이 최종 유효성 검사 결과를 기록하지 않은 경우.
  * 수취인이 쿼리한 rippled 서버가 나머지 네트워크와 동기화되지 않았거나 알 수 없는 버그가 발생했습니다. server\_info 메소드를 사용하여 서버의 상태를 확인하세요. (이 상황을 재현할 수 있는 경우 문제를 신고해 주세요.)

서명과 결제 채널의 현재 상태를 모두 확인한 후, 수취인은 아직 XRP를 수령하지 않았지만 채널이 만료되기 전에 트랜잭션이 처리되기만 하면 XRP를 사용할 수 있다는 것을 확신합니다.

## 6. 수취인이 상품 또는 서비스를 제공합니다.

이 시점에서 수취인은 이미 결제가 확실하다는 것을 알기 때문에 송금인에게 상품과 서비스를 제공할 수 있습니다.

이 튜토리얼의 목적상 수취인은 결제자에게 하이파이브 또는 이에 상응하는 온라인 메시지를 "상품 및 서비스"로 제공할 수 있습니다.

## 7. 원하는 대로 3\~6단계를 반복합니다.

지급인과 수취인은 XRP Ledger 기다릴 필요 없이 3\~6단계(상품과 서비스에 대한 대가 청구 생성, 전송, 확인)를 원하는 만큼 자주 반복할 수 있습니다. 이 과정의 두 가지 주요 한계는 다음과 같습니다:

* 결제 채널에 있는 XRP의 양. (필요한 경우, 결제자는 결제 채널에서 사용할 수 있는 총 XRP 양을 늘리기 위해  [PaymentChannelFund](https://xrpl.org/paymentchannelfund.html) 트랜잭션을 전송할 수 있습니다).
* 결제 채널의 불변 만료(설정된 경우). (account\_channels 메소드에 대한 응답의 cancel\_after 필드에 표시됩니다.)

## 8. 준비가 되면 수취인이 승인된 금액에 대한 청구를 상환합니다.

수취인이 채널에서 최종적으로 XRP를 받는 시점입니다.

잔액, 금액, 서명, 공개키 필드가 제공된 [PaymentChannelClaim](https://xrpl.org/paymentchannelclaim.html) 트랜잭션입니다. 청구 값은 누적되므로 수취인은 가장 큰(가장 최근의) 청구만 상환하면 전체 금액을 받을 수 있습니다. 수취인이 승인된 전체 금액에 대해 클레임을 상환할 필요는 없습니다.

수취인은 원하는 경우 이 작업을 여러 번 수행하여 비즈니스를 계속 진행하면서 부분적으로 정산할 수 있습니다.

채널에서 XRP를 청구하는 예시입니다:

요청:

```json
POST http://localhost:5005/
Content-Type: application/json

{
    "method": "submit",
    "params": [{
            "secret": "s████████████████████████████",
            "tx_json": {
                "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
                "TransactionType": "PaymentChannelClaim",
                "Amount": "1000000",
                "Balance": "1000000",
                "Channel": "5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3",
                "PublicKey": "023693F15967AE357D0327974AD46FE3C127113B1110D6044FD41E723689F81CC6",
                "Signature": "304402204EF0AFB78AC23ED1C472E74F4299C0C21F1B21D07EFC0A3838A420F76D783A400220154FB11B6F54320666E4C36CA7F686C16A3A0456800BBC43746F34AF50290064"
            },
            "fee_mult_max": 1000
        }]
}
```

&#x20;응답:

```json
200 OK

{
    "result": {
        "engine_result": "tesSUCCESS",
        "engine_result_code": 0,
        "engine_result_message": "The transaction was applied. Only final in a validated ledger.",
        "status": "success",
        "tx_blob": "12000F2280000000240000017450165DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB36140000000000F42406240000000000F424068400000000000000A7121023693F15967AE357D0327974AD46FE3C127113B1110D6044FD41E723689F81CC6732103AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB7447304502210096B933BC24DA77D8C4057B4780B282BA66C668DFE1ACF4EEC063AD6661725797022037C8823669CE91AACA8CC754C9F041359F85B0B32384AEA141EBC3603798A24C7646304402204EF0AFB78AC23ED1C472E74F4299C0C21F1B21D07EFC0A3838A420F76D783A400220154FB11B6F54320666E4C36CA7F686C16A3A0456800BBC43746F34AF5029006481144B4E9C06F24296074F7BC48F92A97916C6DC5EA9",
        "tx_json": {
            "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
            "Amount": "1000000",
            "Balance": "1000000",
            "Channel": "5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3",
            "Fee": "10",
            "Flags": 2147483648,
            "PublicKey": "023693F15967AE357D0327974AD46FE3C127113B1110D6044FD41E723689F81CC6",
            "Sequence": 372,
            "Signature": "304402204EF0AFB78AC23ED1C472E74F4299C0C21F1B21D07EFC0A3838A420F76D783A400220154FB11B6F54320666E4C36CA7F686C16A3A0456800BBC43746F34AF50290064",
            "SigningPubKey": "03AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB",
            "TransactionType": "PaymentChannelClaim",
            "TxnSignature": "304502210096B933BC24DA77D8C4057B4780B282BA66C668DFE1ACF4EEC063AD6661725797022037C8823669CE91AACA8CC754C9F041359F85B0B32384AEA141EBC3603798A24C",
            "hash": "C9FE08FC88CF76C3B06622ADAA47AE99CABB3380E4D195E7751274CFD87910EB"
        }
    }
}
```

수취인은 검증된 ledger에서 이 거래가 성공했음을 확인해야 합니다. 자세한 내용은 신뢰할 수 있는 트랜잭션 제출을 참조하세요.

## 9. 송금인과 수취인이 거래를 완료하면 송금인은 채널 폐쇄를 요청합니다.

이는 tfClose 플래그가 설정된 PaymentChannelClaim 트랜잭션 또는 Expiration 필드가 설정된 PaymentChannelFund 트랜잭션입니다. (흐름 다이어그램의 9a).

결제자가 채널 폐쇄를 요청할 때 채널에 XRP가 남아 있지 않으면 채널이 즉시 폐쇄됩니다.

채널에 XRP가 남아 있는 경우, 채널 폐쇄 요청은 수취인에게 미결제 청구를 즉시 상환하라는 최종 경고로 작용합니다. 수취인은 채널이 닫히기 전까지 결제 지연 시간만큼의 시간을 가질 수 있습니다. 정확한 시간(초)은 ledger의 마감 시간에 따라 약간씩 달라집니다.

수취인은 클레임 처리 후 즉시 결제 채널을 닫을 수도 있습니다(흐름 다이어그램의 9b).

채널 폐쇄를 요청하는 트랜잭션 제출 예시:

```json
{
    "method": "submit",
    "params": [{
        "secret": "s████████████████████████████",
        "tx_json": {
            "Account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
            "TransactionType": "PaymentChannelClaim",
            "Channel": "5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3",
            "Flags": 2147614720
        },
        "fee_mult_max": 1000
    }]
}
```

트랜잭션이 검증된 원장에 포함된 후, 양쪽 당사자는 [account\_channels](https://xrpl.org/account\_channels.html) 메소드를 사용하여 채널의 현재 예정된 만료를 조회할 수 있습니다. 반드시 "ledger\_index": "validated"를 지정하여 최신 유효성이 검증된 ledger 버전에서 데이터를 가져와야 합니다.

account\_channels 응답 예시:&#x20;

```json
{
    "result": {
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "channels": [
            {
                "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
                "amount": "100000000",
                "balance": "1000000",
                "channel_id": "5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3",
                "destination_account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
                "destination_tag": 20170428,
                "expiration": 547073182,
                "public_key": "aB44YfzW24VDEJQ2UuLPV2PvqcPCSoLnL7y5M1EzhdW4LnK5xMS3",
                "public_key_hex": "023693F15967AE357D0327974AD46FE3C127113B1110D6044FD41E723689F81CC6",
                "settle_delay": 86400
            }
        ],
        "status": "success"
    }
}
```

이 예시에서 만료 값 547073182는 Ripple 에포크 이후 초 단위로 2017-05-02T20:46:22Z에 매핑되므로, 해당 시간까지 상환되지 않은 청구는 더 이상 유효하지 않습니다.

## 10. 만료된 채널은 누구나 폐쇄할 수 있습니다.

결제 지연 시간이 지나거나 채널이 예정된 만료 시간에 도달하면 채널이 만료됩니다. 채널에 영향을 줄 수 있는 추가 트랜잭션은 채널만 닫을 수 있으며, 미청구 XRP는 지급자에게 반환됩니다.

채널은 만료된 상태로 ledger에 무기한 남아있을 수 있습니다. ledger는 랜잭션의 결과 외에는 변경할 수 없기 때문에 만료된 채널을 닫으려면 누군가 트랜잭션을 보내야 합니다. 채널이 ledger에 남아 있는 한, 채널은 소유자 reserve의 목적상 결제자가 소유한 객체로 간주됩니다.

Ripple은 결제자가 이를 위해 tfClose 플래그가 포함된 두 번째 PaymentChannelClaim 트랜잭션을 전송할 것을 권장합니다. 그러나 결제 채널에 관여하지 않은 계정이라도 다른 계정으로 인해 만료된 채널이 닫힐 수 있습니다.

트랜잭션을 제출하는 명령은 채널 만료를 요청하는 이전 예제와 동일합니다. (그러나 자동으로 채워지는 시퀀스 번호, 서명 및 식별 해시는 고유합니다.)

만료된 채널을 폐쇄하기 위해 트랜잭션을 제출하는 예제입니다:

```json
{
    "method": "submit",
    "params": [{
        "secret": "s████████████████████████████",
        "tx_json": {
            "Account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
            "TransactionType": "PaymentChannelClaim",
            "Channel": "5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3",
            "Flags": 2147614720
        },
        "fee_mult_max": 1000
    }]
}
```

트랜잭션이 검증된 ledger에 포함된 경우, 트랜잭션의 메타데이터를 확인하여 채널을 삭제하고 발신자에게 XRP를 반환했는지 확인할 수 있습니다.

tx 메소드를 사용하여 이 단계의 트랜잭션을 조회했을 때의 응답 예시입니다:

```json
{
    "result": {
        "Account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "Channel": "5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3",
        "Fee": "5606",
        "Flags": 2147614720,
        "Sequence": 41,
        "SigningPubKey": "023693F15967AE357D0327974AD46FE3C127113B1110D6044FD41E723689F81CC6",
        "TransactionType": "PaymentChannelClaim",
        "TxnSignature": "3044022008922FEB6F7D35D42006685BCBB007103D2A40AFAA69A7CFC10DF529F94BB6A402205D67816F50BBAEE0A2709AA3A93707304EC21133550FD2FF7436AD0C3CA6CE27",
        "date": 547091262,
        "hash": "9C0CAAC3DD1A74461132DA4451F9E53BDF4C93DFDBEFCE1B10021EC569013B33",
        "inLedger": 29480670,
        "ledger_index": 29480670,
        "meta": {
            "AffectedNodes": [
                {
                    "ModifiedNode": {
                        "LedgerEntryType": "AccountRoot",
                        "LedgerIndex": "13F1A95D7AAB7108D5CE7EEAF504B2894B8C674E6D68499076441C4837282BF8",
                        "PreviousTxnID": "C9FE08FC88CF76C3B06622ADAA47AE99CABB3380E4D195E7751274CFD87910EB",
                        "PreviousTxnLgrSeq": 29385089
                    }
                },
                {
                    "DeletedNode": {
                        "FinalFields": {
                            "Account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
                            "Amount": "100000000",
                            "Balance": "1000000",
                            "Destination": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
                            "DestinationTag": 20170428,
                            "Expiration": 547073182,
                            "Flags": 0,
                            "OwnerNode": "0000000000000000",
                            "PreviousTxnID": "C5C70B2BCC515165B7F62ACC8126F8F8B655EB6E1D949A49B2358262BDA986B4",
                            "PreviousTxnLgrSeq": 29451256,
                            "PublicKey": "023693F15967AE357D0327974AD46FE3C127113B1110D6044FD41E723689F81CC6",
                            "SettleDelay": 86400
                        },
                        "LedgerEntryType": "PayChannel",
                        "LedgerIndex": "5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3"
                    }
                },
                {
                    "ModifiedNode": {
                        "FinalFields": {
                            "Account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
                            "Balance": "1041862844",
                            "Flags": 0,
                            "OwnerCount": 2,
                            "Sequence": 42
                        },
                        "LedgerEntryType": "AccountRoot",
                        "LedgerIndex": "B1CB040A17F9469BC00376EC8719535655824AD16CB5F539DD5765FEA88FDBE3",
                        "PreviousFields": {
                            "Balance": "942868450",
                            "OwnerCount": 3,
                            "Sequence": 41
                        },
                        "PreviousTxnID": "C5C70B2BCC515165B7F62ACC8126F8F8B655EB6E1D949A49B2358262BDA986B4",
                        "PreviousTxnLgrSeq": 29451256
                    }
                },
                {
                    "ModifiedNode": {
                        "FinalFields": {
                            "Flags": 0,
                            "Owner": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
                            "RootIndex": "E590FC40B4F24D18341569BD3702A2D4E07E7BC04D11CE63608B67979E67030C"
                        },
                        "LedgerEntryType": "DirectoryNode",
                        "LedgerIndex": "E590FC40B4F24D18341569BD3702A2D4E07E7BC04D11CE63608B67979E67030C"
                    }
                }
            ],
            "TransactionIndex": 7,
            "TransactionResult": "tesSUCCESS"
        },
        "status": "success",
        "validated": true
    }
}
```

트랜잭션의 메타데이터에서 다음을 찾습니다:

* "LedgerEntryType": "PayChannel"가 있는 DeletedNode 항목. LedgerIndex 필드는 채널 ID와 일치해야 합니다. 이는 채널이 삭제되었음을 나타냅니다.
* "LedgerEntryType": "AccountRoot"가 있는 ModifiedNode 항목. 이전 필드와 최종 필드에 있는 잔액 필드의 변경 사항은 결제자에게 반환되는 미사용 XRP를 반영합니다.

해당 필드는 결제 채널이 폐쇄되었음을 나타냅니다.

## 결론

이것으로 결제 채널 사용 튜토리얼을 마쳤습니다. Ripple은 사용자가 결제 채널의 속도와 편리함을 최대한 활용할 수 있는 독특하고 흥미로운 사용 사례를 찾아보시길 권장합니다.
