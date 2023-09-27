# 구독

문자열문문자열구독 메소드는 특정 이벤트가 발생하면 서버에 주기적으로 알림을 요청합니다.

## 요청 형식

요청 형식의 예시입니다:

{% tabs %}
{% tab title="Subscribe to accounts" %}
```json
{
  "id": "Example watch Bitstamp's hot wallet",
  "command": "subscribe",
  "accounts": ["rrpNnNLKrartuEqfJGpqyDwPj1AFPg9vn1"]
}
```
{% endtab %}

{% tab title="Subscribe to order book" %}
```json
{
    "id": "Example subscribe to XRP/GateHub USD order book",
    "command": "subscribe",
    "books": [
        {
            "taker_pays": {
                "currency": "XRP"
            },
            "taker_gets": {
                "currency": "USD",
                "issuer": "rhub8VRN55s94qWKDv6jmDy1pUykJzF3wq"
            },
            "snapshot": true
        }
    ]
}
```
{% endtab %}

{% tab title="Subscribe to ledger steram" %}
```json
{
  "id": "Example watch for new validated ledgers",
  "command": "subscribe",
  "streams": ["ledger"]
}
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개 변수가 포함됩니다:

| `Field`             | 유형  | 설명                                                                                                                               |
| ------------------- | --- | -------------------------------------------------------------------------------------------------------------------------------- |
| `streams`           | 배열  | _(선택 사항)_ 아래 설명된 대로 구독할 일반 스트림의 문자열 이름 배열                                                                                        |
| `accounts`          | 배열  | _(선택 사항)_ 검증된 거래를 모니터링할 고유한 계정 주소가 포함된 배열입니다. 주소는 XRP Ledger의 base58 형식 이어야 합니다. 서버는 이러한 계정 중 하나 이상에 영향을 미치는 모든 거래에 대한 알림을 보냅니다. |
| `accounts_proposed` | 배열  | (선택 사항) 계정과 비슷하지만 아직 완료되지 않은 거래를 포함합니다.                                                                                          |
| `books`             | 배열  | _(선택 사항)_ 아래에 자세히 설명된 대로 업데이트를 모니터링할 오더북을 정의하는 개체 배열입니다.                                                                         |
| `url`               | 문자열 | (웹소켓의 경우 선택 사항, 그렇지 않은 경우 필수) 서버가 각 이벤트에 대해 JSON-RPC 콜백을 전송하는 URL입니다. _관리자 전용_.                                                  |
| `url_username`      | 문자열 | _(선택 사항)_ 콜백 URL에서 기본 인증을 제공하기 위한 사용자 이름입니다.                                                                                     |
| `url_password`      | 문자열 | _(선택 사항)_ 콜백 URL에서 기본 인증을 제공하기 위한 비밀번호입니다.                                                                                       |

사용자, 비밀번호, rt\_accounts 매개변수는 더 이상 사용되지 않으며 별도의 통지 없이 제거될 수 있습니다.

스트림 매개변수는 다음과 같은 기본 정보 스트림에 대한 액세스를 제공합니다:

* consensus - 서버가 컨센서스 주기에서 단계가 변경될 때마다 메시지를 보냅니다.
* ledger - 컨센서스 프로세스가 새로운 검증 ledger을 선언할 때마다 메시지를 보냅니다.
* manifests - 서버가 검증자의 임시 서명 키에 대한 업데이트를 받을 때마다 메시지를 보냅니다.
* peer\_status - (관리자만 해당) 연결된 피어 rippled 서버에 대한 정보, 특히 컨센서스 프로세스와 관련된 정보입니다.
* transactions - 트랜잭션이 폐쇄 ledger에 포함될 때마다 메시지를 보냅니다.
* transactions\_proposed- 트랜잭션이 폐쇄 ledger에 포함될 때마다 메시지를 전송하며, 아직 검증 ledger에 포함되지 않았고 앞으로도 포함되지 않을 수도 있는 일부 트랜잭션도 포함됩니다. 제안된 모든 트랜잭션이 검증 전에 표시되는 것은 아닙니다. 참고: 성공하지 못한 일부 트랜잭션도 스팸 방지 트랜잭션 수수료를 받기 때문에 검증 ledger에 포함됩니다.
* server - rippled 서버의 상태(예: 네트워크 연결)가 변경될 때마다 메시지를 보냅니다.
* validations - 서버가 유효성 검증인을 신뢰하는지 여부에 관계없이 서버가 유효성 검사 메시지를 수신할 때마다 메시지를 보냅니다. (서버가 최소 정족수 이상의 신뢰할 수 있는 검증인으로부터 유효성 검사 메시지를 수신하면 개별 Ripple은 ledger이 유효하다고 선언합니다.)

{% hint style="info" %}
Note:

보고 모드의 서버에서는 서버, 피어 상태, 컨센서스 스트림을 사용할 수 없습니다. 리포팅 모드 서버는 이러한 스트림 중 하나를 요청하면 보고 모드 서버에서 보고 지원되지 않음 오류를 반환합니다. [![Updated in: rippled 1.8.1](https://img.shields.io/badge/Updated%20in-rippled%201.8.1-blue.svg)](https://github.com/ripple/rippled/releases/tag/1.8.1)
{% endhint %}

books 배열의 각 멤버는 제공된 경우 다음 필드를 가진 객체입니다:

| `Field`      | 유형      | 설명                                                                                                                                                                  |
| ------------ | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `taker_gets` | 객체      | [금액이 없는 통화 개체](https://xrpl.org/currency-formats.html#specifying-without-amounts) 로 제안을 받는 계정이 받게 될 통화를 지정합니다.                                                      |
| `taker_pays` | 객체      | [금액이 없는 통화 개체](https://xrpl.org/currency-formats.html#specifying-without-amounts) 로 제안을 받는 계정에서 지불할 통화를 지정합니다.                                                      |
| `taker`      | 문자열     | [XRP Ledger의 base58](https://xrpl.org/base58-encodings.html) 형식으로 제안을 보기 위한 관점으로 사용할 고유 계정 주소입니다. (이는 자금 조달 상태 및 [제안](https://xrpl.org/offers.html) 수수료에 영향을 미칩니다.) |
| `snapshot`   | Boolean | (선택 사항) true인 경우 업데이트를 보내기 전에 구독할 때 주문서의 현재 상태를 한 번 반환합니다. 기본값은 false입니다.                                                                                           |
| `both`       | Boolean | (선택 사항) 참이면 오더북의 양쪽을 모두 반환합니다. 기본값은 false입니다.                                                                                                                       |

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": "Example watch Bitstamp's hot wallet",
  "status": "success",
  "type": "response",
  "result": {}
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따릅니다. 응답에 포함된 필드는 요청에 포함된 구독에 따라 달라집니다.

* accounts 및 accounts\_proposed - 반환되는 필드가 없습니다.
* 스트림: 서버 - load\_base(서버의 현재 부하 수준), random(무작위로 생성된 값) 등 서버 상태에 대한 정보(변경될 수 있음).
* 스트림: 트랜잭션, 스트림: 트랜잭션\_제안, 스트림: 유효성 검사 및 스트림: 컨센서스 - 반환되는 필드가 없습니다.
* 스트림: ledger - 보유 중인 ledger과 현재 수수료 일정에 대한 정보입니다. 여기에는 ledger 스트림 메시지와 동일한 필드가 포함되지만, 유형과 txn\_count 필드가 생략됩니다.
* books - 기본적으로 반환되는 필드가 없습니다. 요청에 "스냅샷": true가 설정되어 있으면 오퍼(오더북을 정의하는 오퍼 정의 객체 배열)를 반환합니다.

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* noPermission - 요청에 URL 필드가 포함되어 있지만 관리자로 연결되어 있지 않습니다.
* unknownStream - 요청의 스트림 필드에 있는 하나 이상의 멤버가 유효한 스트림 이름이 아닙니다.
* malformedStream - 요청의 스트림 필드 형식이 올바르지 않습니다.
* malformedAccount - 요청의 계정 또는 계정\_제안 필드에 있는 주소 중 하나가 올바른 형식의 XRP 레저 주소가 아닙니다. (참고:: 글로벌 ledger에 아직 항목이 없는 주소의 스트림을 구독하여 해당 주소가 펀딩될 때 메시지를 받을 수 있습니다.)
* srcCurMalformed - 요청에 있는 도서 필드의 하나 이상의 테이커\_페이즈 하위 필드의 형식이 올바르지 않습니다.
* dstAmtMalformed - 요청에 있는 도서 필드의 하나 이상의 taker\_gets 하위 필드 형식이 올바르지 않습니다.
* srcIsrMalformed - 요청에 있는 도서 필드의 하나 이상의 taker\_pays 하위 필드 중 발행자 필드가 유효하지 않습니다.
* dstIsrMalformed - 요청에 있는 도서 필드의 하나 이상의 taker\_gets 하위 필드 중 하나 이상의 발행자 필드가 유효하지 않습니다.
* badMarket - 호가창 필드에 있는 하나 이상의 원하는 오더북이 존재하지 않습니다(예: 자체 통화 교환 오퍼).

특정 스트림을 구독하면 구독을 취소하거나 웹소켓 연결을 닫을 때까지 해당 스트림에서 주기적으로 응답을 받습니다. 이러한 응답의 내용은 구독한 스트림에 따라 다릅니다. 다음은 몇 가지 예입니다:

## Ledger 스트림

ledger 스트림은 컨센서스 프로세스가 새로운 검증 ledger을 선언할 때만 ledgerClosed 메시지를 전송합니다. 이 메시지는 ledger을 식별하고 ledger의 내용에 대한 몇 가지 정보를 제공합니다.

```json
{
  "type": "ledgerClosed",
  "fee_base": 10,
  "fee_ref": 10,
  "ledger_hash": "687F604EF6B2F67319E8DCC8C66EF49D84D18A1E18F948421FC24D2C7C3DB464",
  "ledger_index": 7125358,
  "ledger_time": 455751310,
  "reserve_base": 20000000,
  "reserve_inc": 5000000,
  "txn_count": 7,
  "validated_ledgers": "32570-7125358"
}
```

ledger 스트림 메시지의 필드는 다음과 같습니다:

| `Field`             | 유형                                                                | 설명                                                                                                                                                                       |
| ------------------- | ----------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `type`              | 문자열                                                               | ledgerClosed는 원장 스트림에서 가져온 것임을 나타냅니다.                                                                                                                                    |
| `fee_base`          | 숫자                                                                | 이 원장 버전 기준의 기준 트랜잭션 비용(XRP 단위)입니다. 이 원장 버전에 SetFee 의사 트랜잭션이 포함된 경우 다음 원장 버전부터 새로운 트랜잭션 비용이 적용됩니다.                                                                        |
| `fee_ref`           | 숫자                                                                | "수수료 단위"로 표시된 참조 트랜잭션 비용입니다.                                                                                                                                             |
| `ledger_hash`       | 문자열 - [해시](https://xrpl.org/basic-data-types.html#hashes)         | 폐쇄된 원장 버전의 식별 해시입니다.                                                                                                                                                     |
| `ledger_index`      | 번호 - [원장 색인](https://xrpl.org/basic-data-types.html#ledger-index) | 폐쇄된 원장 버전의 식별 해시입니다.                                                                                                                                                     |
| `ledger_time`       | 숫자                                                                | [Ripple 에포크 이후](https://xrpl.org/basic-data-types.html#specifying-time) 이 원장이 닫힌 시간( 초)입니다.                                                                              |
| `reserve_base`      | 숫자                                                                | 계정에 필요한 최소 예치금(XRP 단위)입니다. 이 원장 버전에 SetFee 의사 트랜잭션이 포함된 경우 다음 원장 버전부터 새로운 기본 준비금이 적용됩니다.                                                                                 |
| `reserve_inc`       | 숫자                                                                | 계정이 원장에 소유하고 있는 각 개체에 대한 소유자 준비금(XRP 단위)입니다. 원장에 SetFee 의사 트랜잭션이 포함된 경우 새 소유자 준비금은 이 원장 이후부터 적용됩니다.                                                                      |
| `txn_count`         | 숫자                                                                | 원장 버전에 포함된 새 트랜잭션의 수입니다.                                                                                                                                                 |
| `validated_ledgers` | 문자열                                                               | (생략 가능) 서버에서 사용할 수 있는 원장의 범위입니다. 24900901-24900984,24901116-24901158과 같이 분리되지 않은 시퀀스일 수 있습니다. 서버가 네트워크에 연결되어 있지 않거나 연결되어 있지만 아직 네트워크에서 원장을 가져오지 않은 경우에는 이 필드가 반환되지 않습니다. |

## 검증 스트림

[![New in: rippled 0.29.0](https://img.shields.io/badge/New%20in-rippled%200.29.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/0.29.0)

검증 스트림은 검증 메시지가 신뢰할 수 있는 검증자가 보낸 것인지 여부와 관계없이 검증 투표라고도 하는 검증 메시지를 수신할 때마다 메시지를 보냅니다. 메시지는 다음과 같습니다:

```json
{
    "type": "validationReceived",
    "amendments":[
        "42426C4D4F1009EE67080A9B7965B44656D7714D104A72F9B4369F97ABF044EE",
        "4C97EBA926031A7CF7D7B36FDE3ED66DDA5421192D63DE53FFB46E43B9DC8373",
        "6781F8368C4771B83E8B821D88F580202BCB4228075297B19E4FDC5233F1EFDC",
        "C1B8D934087225F509BEB5A8EC24447854713EE447D277F69545ABFA0E0FD490",
        "DA1BD556B42D85EA9C84066D028D355B52416734D3283F85E216EA5DA6DB7E13"
    ],
    "base_fee":10,
    "flags":2147483649,
    "full":true,
    "ledger_hash":"EC02890710AAA2B71221B0D560CFB22D64317C07B7406B02959AD84BAD33E602",
    "ledger_index":"6",
    "load_fee":256000,
    "master_key": "nHUon2tpyJEHHYGmxqeGu37cvPYHzrMtUNQFVdCgGNvEkjmCpTqK",
    "reserve_base":20000000,
    "reserve_inc":5000000,
    "signature":"3045022100E199B55643F66BC6B37DBC5E185321CF952FD35D13D9E8001EB2564FFB94A07602201746C9A4F7A93647131A2DEB03B76F05E426EC67A5A27D77F4FF2603B9A528E6",
    "signing_time":515115322,
    "validation_public_key":"n94Gnc6svmaPPRHUAyyib1gQUov8sYbjLoEwUBYPH39qHZXuo8ZT"
}
```

유효성 검증 스트림 메시지의 필드는 다음과 같습니다:

| Field                   | 유형       | 설명                                                                                                                                                                                                             |
| ----------------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| type                    | 문자열      | 값은 validationReceived이것이 검증 스트림에서 나온 것임을 나타냅니다.                                                                                                                                                                |
| amendments              | 문자열 배열   | (생략 가능) 이 서버가 프로토콜에 추가하려는 수정 사항 입니다.                                                                                                                                                                           |
| base\_fee               | 정수       | (생략 가능) 이 서버가 수수료 투표를reference\_fee 통해 설정하려는 실제 거래 비용(가치)입니다.                                                                                                                                                  |
| cookie                  | 문자열 - 숫자 | _(생략 가능)_ 서버 시작 시 서버가 선택한 임의의 값입니다. 동일한 검증 키 쌍이 동시에 다른 쿠키로 검증에 서명하는 경우 이는 일반적으로 여러 서버가 동일한 검증 키 쌍을 사용하도록 잘못 구성되었음을 나타냅니다.                                                                                      |
| flags                   | 숫자       | 이 검증 메시지에 플래그의 비트 마스크가 추가되었습니다. 플래그는 0x80000000유효성 검사 서명이 완전 표준임을 나타냅니다. 플래그는 0x00000001이것이 완전한 검증임을 나타냅니다. 그렇지 않으면 부분 검증입니다. 부분 검증은 특정 원장에 투표하기 위한 것이 아닙니다. 부분 검증은 검증인이 여전히 온라인 상태이지만 합의를 따라잡지 못하고 있음을 나타냅니다. |
| full                    | Boolean  | true이면 이는 완전한 검증입니다. 그렇지 않으면 부분 검증입니다. 부분 검증은 특정 원장에 투표하기 위한 것이 아닙니다. 부분 검증은 검증인이 여전히 온라인 상태이지만 합의를 따라잡지 못하고 있음을 나타냅니다.                                                                                        |
| ledger\_hash            | 문자열      | 제안된 원장의 식별 해시가 검증되고 있습니다.                                                                                                                                                                                      |
| ledger\_index           | 문자열 - 숫자 | 제안된 원장의 원장 인덱스입니다.                                                                                                                                                                                             |
| load\_fee               | 정수       | _(생략 가능)_ 이 검증인이 현재 시행하고 있는 로컬 로드 스케일 거래 비용(수수료 단위)입니다.                                                                                                                                                        |
| master\_key             | 문자열      | _(생략 가능)_ 검증인이 검증인 토큰을 사용하는 경우 XRP Ledger의 base58 형식인 검증인의 마스터 공개 키입니다. (참조: 서버 에서 유효성 검사 활성화rippled.)                                                                                                         |
| reserve\_base           | 정수       | (생략 가능) 이 검증인이 수수료 투표를account\_reserve 통해 설정하려는 최소 준비금 요구 사항( 값)입니다.                                                                                                                                           |
| reserve\_inc            | 정수       | _(생략 가능) 이 검증인이_ 수수료 투표를owner\_reserve 통해 설정하려는 준비금 요구 사항(가치)의 증가분입니다 .                                                                                                                                        |
| server\_version         | 문자열 - 숫자 | _(생략 가능)_ 검증 서버의 버전 번호를 인코딩하는 64비트 정수입니다. 예를 들어, "1745990410175512576". 256개의 원장마다 한 번만 제공됩니다.                                                                                                                 |
| signature               | 문자열      | 검증인이 이 원장에 대한 투표에 서명하는 데 사용한 서명입니다.                                                                                                                                                                            |
| signing\_time           | 숫자       | 이 검증 투표가 서명되었을 때, [Ripple 에포크 이후 몇 초](https://xrpl.org/basic-data-types.html#specifying-time) 만에 기록되었습니다.                                                                                                      |
| validated\_hash         | 문자열      | 이 검증이 적용되는 제안된 원장의 고유 해시입니다.                                                                                                                                                                                   |
| validation\_public\_key | 문자열      | 유효성 검사자가 메시지에 서명하는 데 사용한 키 쌍의 공개 키로, XRP Ledger의 base58 형식입니다. 이는 메시지를 전송하는 검증자를 식별하며 서명을 확인하는 데 사용할 수도 있습니다. 유효성 검사자가 토큰을 사용하는 경우, 이 키는 임시 공개 키입니다.                                                           |

## 트랜잭션 스트림

많은 구독에서 다음과 같은 트랜잭션에 대한 메시지가 생성됩니다:

* 트랜잭션 스트림
* transactions\_proposed 스트림
* 계정 구독
* accounts\_proposed 구독
* book (오더북) 구독

트랜잭션 스트림은 엄밀히 말하면 트랜잭션 스트림의 상위 스트림으로, 검증된 모든 트랜잭션과 아직 검증 ledger에 포함되지 않았고 앞으로도 포함되지 않을 수 있는 일부 제안 트랜잭션이 포함됩니다. 이러한 '진행 중인' 트랜잭션은 필드를 통해 식별할 수 있습니다:

* 유효성 검증된 필드가 누락되었거나 값이 거짓입니다.
* 메타 또는 메타데이터 필드가 없습니다.
* 트랜잭션이 어떤 ledger 버전에서 완료되었는지를 나타내는 ledger\_hash 및 ledger\_index 필드 대신, 현재 어떤 ledger 버전에서 제안되었는지를 나타내는 ledger\_current\_index 필드가 있습니다.

그렇지 않으면, 트랜잭션 제안 스트림의 메시지는 트랜잭션 스트림의 메시지와 동일합니다.

계좌나 오더북을 수정할 수 있는 것은 트랜잭션뿐이므로 특정 계좌나 장부를 구독한 결과로 전송되는 메시지 역시 트랜잭션 스트림의 메시지와 동일한 트랜잭션 메시지 형태를 취합니다. 유일한 차이점은 보고 있는 계좌 또는 오더북에 영향을 주는 거래에 대한 메시지만 수신한다는 것입니다.

accounts\_proposed 구독도 같은 방식으로 작동하지만, 감시 중인 계좌에 대해 transactions\_proposed 스트림과 같이 미확인 트랜잭션도 포함된다는 점이 다릅니다.

```json
{
  "status": "closed",
  "type": "transaction",
  "engine_result": "tesSUCCESS",
  "engine_result_code": 0,
  "engine_result_message": "The transaction was applied.",
  "ledger_hash": "989AFBFD65D820C6BD85301B740F5D592F060668A90EEF5EC1815EBA27D58FE8",
  "ledger_index": 7125442,
  "meta": {
    "AffectedNodes": [
      {
        "ModifiedNode": {
          "FinalFields": {
            "Flags": 0,
            "IndexPrevious": "0000000000000000",
            "Owner": "rRh634Y6QtoqkwTTrGzX66UYoCAvgE6jL",
            "RootIndex": "ABD8CE2D1205D0C062876E9E1F3CBDC902ED8EF4E8D3D071B962C7ED0E113E68"
          },
          "LedgerEntryType": "DirectoryNode",
          "LedgerIndex": "0BBDEE7D0BE120F7BF27640B5245EBFE0C5FD5281988BA823C44477A70262A4D"
        }
      },
      {
        "DeletedNode": {
          "FinalFields": {
            "Account": "rRh634Y6QtoqkwTTrGzX66UYoCAvgE6jL",
            "BookDirectory": "892E892DC63D8F70DCF5C9ECF29394FF7DD3DC6F47DB8EB34A03920BFC5E99BE",
            "BookNode": "0000000000000000",
            "Flags": 0,
            "OwnerNode": "000000000000006E",
            "PreviousTxnID": "58A17D95770F8D07E08B81A85896F4032A328B6C2BDCDEC0A00F3EF3914DCF0A",
            "PreviousTxnLgrSeq": 7125330,
            "Sequence": 540691,
            "TakerGets": "4401967683",
            "TakerPays": {
              "currency": "BTC",
              "issuer": "rNPRNzBB92BVpAhhZr4iXDTveCgV5Pofm9",
              "value": "0.04424"
            }
          },
          "LedgerEntryType": "Offer",
          "LedgerIndex": "386B7803A9210747941B0D079BB408F31ACB1CB98832184D0287A1CBF4FE6D00"
        }
      },
      {
        "DeletedNode": {
          "FinalFields": {
            "ExchangeRate": "4A03920BFC5E99BE",
            "Flags": 0,
            "RootIndex": "892E892DC63D8F70DCF5C9ECF29394FF7DD3DC6F47DB8EB34A03920BFC5E99BE",
            "TakerGetsCurrency": "0000000000000000000000000000000000000000",
            "TakerGetsIssuer": "0000000000000000000000000000000000000000",
            "TakerPaysCurrency": "0000000000000000000000004254430000000000",
            "TakerPaysIssuer": "92D705968936C419CE614BF264B5EEB1CEA47FF4"
          },
          "LedgerEntryType": "DirectoryNode",
          "LedgerIndex": "892E892DC63D8F70DCF5C9ECF29394FF7DD3DC6F47DB8EB34A03920BFC5E99BE"
        }
      },
      {
        "ModifiedNode": {
          "FinalFields": {
            "Account": "rRh634Y6QtoqkwTTrGzX66UYoCAvgE6jL",
            "Balance": "11133297300",
            "Flags": 0,
            "OwnerCount": 9,
            "Sequence": 540706
          },
          "LedgerEntryType": "AccountRoot",
          "LedgerIndex": "A6C2532E1008A513B3F822A92B8E5214BD0D413DC20AD3631C1A39AD6B36CD07",
          "PreviousFields": {
            "Balance": "11133297310",
            "OwnerCount": 10,
            "Sequence": 540705
          },
          "PreviousTxnID": "484D57DFC4E446DA83B4540305F0CE836D4E007361542EC12CC0FFB5F0A1BE3A",
          "PreviousTxnLgrSeq": 7125358
        }
      }
    ],
    "TransactionIndex": 1,
    "TransactionResult": "tesSUCCESS"
  },
  "transaction": {
    "Account": "rRh634Y6QtoqkwTTrGzX66UYoCAvgE6jL",
    "Fee": "10",
    "Flags": 2147483648,
    "OfferSequence": 540691,
    "Sequence": 540705,
    "SigningPubKey": "030BB49C591C9CD65C945D4B78332F27633D7771E6CF4D4B942D26BA40748BB8B4",
    "TransactionType": "OfferCancel",
    "TxnSignature": "30450221008223604A383F3AED25D53CE7C874700619893A6EEE4336508312217850A9722302205E0614366E174F2DFF78B879F310DB0B3F6DA1967E52A32F65E25DCEC622CD68",
    "date": 455751680,
    "hash": "94CF924C774DFDBE474A2A7E40AEA70E7E15D130C8CBEF8AF1D2BE97A8269F14"
  },
  "validated": true
}
```

트랜잭션 스트림 메시지에는 다음과 같은 필드가 있습니다:

| `Field`                 | 유형                                                                | 설명                                                                         |
| ----------------------- | ----------------------------------------------------------------- | -------------------------------------------------------------------------- |
| `type`                  | 문자열                                                               | 트랜잭션은 여러 스트림에서 발생할 수 있는 트랜잭션의 알림임을 나타냅니다.                                  |
| `engine_result`         | 문자열                                                               | 문자열 트랜잭션 결과 코드                                                             |
| `engine_result_code`    | 숫자                                                                | 숫자 트랜잭션 응답 코드(해당되는 경우).                                                    |
| `engine_result_message` | 문자열                                                               | 트랜잭션 응답에 대한 사람이 읽을 수 있는 설명                                                 |
| `ledger_current_index`  | 번호 - [원장 색인](https://xrpl.org/basic-data-types.html#ledger-index) | (검증되지 않은 트랜잭션만 해당) 트랜잭션이 현재 제안된 현재 진행 중인 원장 버전에 대한 원장 인덱스입니다.              |
| `ledger_hash`           | 문자열 - [해시](https://xrpl.org/basic-data-types.html#hashes)         | (검증한 트랜잭션만 해당) 이 트랜잭션이 포함된 원장 버전의 식별 해시입니다.                                |
| `ledger_index`          | 번호 - [원장 색인](https://xrpl.org/basic-data-types.html#ledger-index) | (검증된 트랜잭션만 해당) 이 트랜잭션이 포함된 원장 버전의 원장 인덱스입니다.                               |
| `meta`                  | 객체                                                                | (검증한 트랜잭션만 해당) 트랜잭션의 정확한 결과를 자세히 보여주는 트랜잭션 메타데이터입니다.                       |
| `transaction`           | 객체                                                                | JSON 형식의 트랜잭션 정의입니다.                                                       |
| `validated`             | Boolean                                                           | 참이면 이 트랜잭션은 유효성 검사된 원장에 포함되며 그 결과는 최종적입니다. 트랜잭션 스트림의 응답은 항상 유효성을 검사해야 합니다. |

## 피어 상태 스트림

관리자 전용 피어 상태 스트림은 이 서버가 연결된 다른 rippled 서버의 활동, 특히 컨센서스 과정에서의 상태에 대한 많은 양의 정보를 보고합니다.

피어 상태 스트림 메시지 예시:

```json
{
    "action": "CLOSING_LEDGER",
    "date": 508546525,
    "ledger_hash": "4D4CD9CD543F0C1EF023CC457F5BEFEA59EEF73E4552542D40E7C4FA08D3C320",
    "ledger_index": 18853106,
    "ledger_index_max": 18853106,
    "ledger_index_min": 18852082,
    "type": "peerStatusChange"
}
```

피어 상태 스트림 메시지는 피어 rippled 서버의 상태가 변경된 이벤트를 나타냅니다. 이러한 메시지는 다음 필드를 가진 JSON 객체입니다:

| `Field`            | 값   | 설명                                             |
| ------------------ | --- | ---------------------------------------------- |
| `type`             | 문자열 | 피어 상태 변경은 피어 상태 스트림에서 온 메시지임을 나타냅니다.           |
| `action`           | 문자열 | 이 메시지를 표시한 이벤트 유형입니다. 가능한 값은 피어 상태 이벤트를 참조하세요. |
| `date`             | 숫자  | 이 이벤트가 발생한 시간(Ripple 에포크 이후 초 단위)입니다.          |
| `ledger_hash`      | 문자열 | (생략 가능) 이 메시지와 관련된 원장 버전의 식별 해시입니다.            |
| `ledger_index`     | 숫자  | (생략 가능) 이 메시지와 관련된 원장 버전의 원장 인덱스입니다.           |
| `ledger_index_max` | 숫자  | (생략 가능) 피어가 현재 사용할 수 있는 최대 레저 인덱스입니다.          |
| `ledger_index_min` | 숫자  | 생략 가능) 피어가 현재 사용할 수 있는 가장 작은 원장 인덱스입니다.        |

## 피어 상태 이벤트

피어 상태 스트림 메시지의 작업 필드에는 다음과 같은 값이 있을 수 있습니다:

| `Value`           | 의미                                                                                                                        |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------- |
| `CLOSING_LEDGER`  | 피어는 이 [Ledger Index를](https://xrpl.org/basic-data-types.html#ledger-index) 사용하여 원장 버전을 마감했습니다. 이는 일반적으로 합의가 곧 시작됨을 의미합니다. |
| `ACCEPTED_LEDGER` | 피어는 합의 라운드의 결과로 이 원장 버전을 구축했습니다. **참고:** 이 원장은 여전히 ​​불변적으로 검증될지 확실하지 않습니다.                                                |
| `SWITCHED_LEDGER` | 피어는 네트워크의 나머지 부분을 따르지 않는다고 결론을 내리고 다른 원장 버전으로 전환했습니다.                                                                     |
| `LOST_SYNC`       | 피어는 검증된 원장 버전과 합의가 진행 중인 버전을 추적하는 데 있어 네트워크의 나머지 부분보다 뒤쳐졌습니다                                                              |

## 오더북 스트림

오더북 필드를 사용하여 하나 이상의 오더북을 구독하면 해당 오더북에 영향을 주는 모든 트랜잭션을 반환받습니다.

오더북 스트림 메시지 예시:

```json
{
    "engine_result": "tesSUCCESS",
    "engine_result_code": 0,
    "engine_result_message": "The transaction was applied. Only final in a validated ledger.",
    "ledger_hash": "08547DD866F099CCB3666F113116B7AA2DF520FA2E3011DD1FF9C9C04A6C7C3E",
    "ledger_index": 18852105,
    "meta": {
        "AffectedNodes": [{
            "ModifiedNode": {
                "FinalFields": {
                    "Account": "rfCFLzNJYvvnoGHWQYACmJpTgkLUaugLEw",
                    "AccountTxnID": "D295E2BE50E3B78AED24790D7B9096996DAF43F095BF17DB83EEACC283D14050",
                    "Balance": "3070332374272",
                    "Flags": 0,
                    "OwnerCount": 23,
                    "RegularKey": "r9S56zu6QeJD5d8A7QMfLAeYavgB9dhaX4",
                    "Sequence": 12142921
                },
                "LedgerEntryType": "AccountRoot",
                "LedgerIndex": "2880A9B4FB90A306B576C2D532BFE390AB3904642647DCF739492AA244EF46D1",
                "PreviousFields": {
                    "AccountTxnID": "3CA3422B0E42D76A7A677B0BA0BE72DFCD93676E0C80F8D2EB27C04BD8457A0F",
                    "Balance": "3070332385272",
                    "Sequence": 12142920
                },
                "PreviousTxnID": "3CA3422B0E42D76A7A677B0BA0BE72DFCD93676E0C80F8D2EB27C04BD8457A0F",
                "PreviousTxnLgrSeq": 18852102
            }
        }, {
            "ModifiedNode": {
                "FinalFields": {
                    "Flags": 0,
                    "IndexPrevious": "00000000000022D2",
                    "Owner": "rfCFLzNJYvvnoGHWQYACmJpTgkLUaugLEw",
                    "RootIndex": "F435FBBEC9654204D7151A01E686BAA8CB325A472D7B61C7916EA58B59355767"
                },
                "LedgerEntryType": "DirectoryNode",
                "LedgerIndex": "29A543B6681AD7FC8AFBD1386DAE7385F02F9B8C4756A467DF6834AB54BBC9DB"
            }
        }, {
            "ModifiedNode": {
                "FinalFields": {
                    "ExchangeRate": "4C1BA999A513EF78",
                    "Flags": 0,
                    "RootIndex": "79C54A4EBD69AB2EADCE313042F36092BE432423CC6A4F784C1BA999A513EF78",
                    "TakerGetsCurrency": "0000000000000000000000000000000000000000",
                    "TakerGetsIssuer": "0000000000000000000000000000000000000000",
                    "TakerPaysCurrency": "0000000000000000000000005553440000000000",
                    "TakerPaysIssuer": "2ADB0B3959D60A6E6991F729E1918B7163925230"
                },
                "LedgerEntryType": "DirectoryNode",
                "LedgerIndex": "79C54A4EBD69AB2EADCE313042F36092BE432423CC6A4F784C1BA999A513EF78"
            }
        }, {
            "CreatedNode": {
                "LedgerEntryType": "Offer",
                "LedgerIndex": "92E235EE80D2B28A89BEE2C905D4545C2A004FD5D4097679C8A3FB25507FD9EB",
                "NewFields": {
                    "Account": "rfCFLzNJYvvnoGHWQYACmJpTgkLUaugLEw",
                    "BookDirectory": "79C54A4EBD69AB2EADCE313042F36092BE432423CC6A4F784C1BA999A513EF78",
                    "Expiration": 508543674,
                    "OwnerNode": "00000000000022F4",
                    "Sequence": 12142920,
                    "TakerGets": "6537121438",
                    "TakerPays": {
                        "currency": "USD",
                        "issuer": "rhub8VRN55s94qWKDv6jmDy1pUykJzF3wq",
                        "value": "50.9"
                    }
                }
            }
        }, {
            "DeletedNode": {
                "FinalFields": {
                    "Account": "rfCFLzNJYvvnoGHWQYACmJpTgkLUaugLEw",
                    "BookDirectory": "79C54A4EBD69AB2EADCE313042F36092BE432423CC6A4F784C1BA999A513EF78",
                    "BookNode": "0000000000000000",
                    "Expiration": 508543133,
                    "Flags": 0,
                    "OwnerNode": "00000000000022F4",
                    "PreviousTxnID": "58B3279C2D56AAC3D9B06106E637C01E3D911E9D31E2FE4EA0D886AC9F4DEE1E",
                    "PreviousTxnLgrSeq": 18851945,
                    "Sequence": 12142889,
                    "TakerGets": "6537121438",
                    "TakerPays": {
                        "currency": "USD",
                        "issuer": "rhub8VRN55s94qWKDv6jmDy1pUykJzF3wq",
                        "value": "50.9"
                    }
                },
                "LedgerEntryType": "Offer",
                "LedgerIndex": "D3436CE21925E1CB12C5C444963B47D7EA0CD9A0E387926DC76B23FE5CD1C15F"
            }
        }],
        "TransactionIndex": 26,
        "TransactionResult": "tesSUCCESS"
    },
    "status": "closed",
    "transaction": {
        "Account": "rfCFLzNJYvvnoGHWQYACmJpTgkLUaugLEw",
        "Expiration": 508543674,
        "Fee": "11000",
        "Flags": 2147483648,
        "LastLedgerSequence": 18852106,
        "OfferSequence": 12142889,
        "Sequence": 12142920,
        "SigningPubKey": "034841BF24BD72C7CC371EBD87CCBF258D8ADB05C18DE207130364A97D8A3EA524",
        "TakerGets": "6537121438",
        "TakerPays": {
            "currency": "USD",
            "issuer": "rhub8VRN55s94qWKDv6jmDy1pUykJzF3wq",
            "value": "50.9"
        },
        "TransactionType": "OfferCreate",
        "TxnSignature": "3045022100B9AD678A773FB61F8F9B565713C80CBF187A2F9EB8E9CE0DAC7B839CA6F4B04C02200613D173A0636CD9BE13F2E3EBD13A16932B5B7D8A96BB5F6D561CA5CDBC4AD3",
        "date": 508543090,
        "hash": "D295E2BE50E3B78AED24790D7B9096996DAF43F095BF17DB83EEACC283D14050",
        "owner_funds": "3070197374272"
    },
    "type": "transaction",
    "validated": true
}
```

오더북 스트림 메시지의 형식은 거래 스트림 메시지의 형식과 동일하지만 오퍼 생성 트랜잭션에 다음 필드도 포함된다는 점이 다릅니다:

| `Field`                   | 값   | 설명                                                                                                                                   |
| ------------------------- | --- | ------------------------------------------------------------------------------------------------------------------------------------ |
| `transaction.owner_funds` | 문자열 | 이 거래를 실행한 후 이 OfferCreate 거래를 보내는 `TakerGets`통화 의 숫자 금액입니다 . `Account`통화 금액이 [동결](https://xrpl.org/freezes.html) 되었는지 여부는 확인하지 않습니다. |

## 컨센서스 스트림

[![New in: rippled 1.4.0](https://img.shields.io/badge/New%20in-rippled%201.4.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/1.4.0)

컨센서스 스트림은 컨센서스 프로세스가 페이즈를 변경할 때 컨센서스 페이즈 메시지를 보냅니다. 이 메시지에는 서버가 속한 새로운 컨센서스 단계가 포함됩니다.

```json
{
  "type": "consensusPhase",
  "consensus": "accepted"
}
```

컨센서스 스트림 메시지의 필드는 다음과 같습니다:

| `Field`     | 유형  | 설명                                                                    |
| ----------- | --- | --------------------------------------------------------------------- |
| `type`      | 문자열 | `consensusPhase`이는 합의 흐름에서 나온 것임을 나타냅니다.                              |
| `consensus` | 문자열 | 서버가 현재 진행 중인 새로운 합의 단계입니다. 가능한 값은 `open`, `establish`및 `accepted`입니다. |
