# ledger\_entry

ledger\_entry 메서드는 XRP Ledger에서 단일 ledger 개체를 원시 형식으로 반환합니다. 검색할 수 있는 다양한 유형의 객체에 대한 정보는 ledger 형식을 참조하세요.

## 요청 형식

이 메소드는 여러 가지 유형의 데이터를 검색할 수 있습니다. 아래 나열된 일반 필드와 유형별 필드로 구성된 적절한 파라미터를 전달하고 표준 요청 형식을 따라 검색할 항목 유형을 선택할 수 있습니다. (예를 들어 WebSocket 요청에는 항상 command 필드와 선택적으로 id 필드가 있으며, JSON-RPC 요청에는 메소드 및 파라미터 필드가 사용됩니다.)

{% hint style="info" %}
Note:

이 메소드에 대한 커맨드라인 구문은 없습니다. 대신 json 메소드를 사용하여 커맨드라인에서 이 메소드에 액세스할 수 있습니다.
{% endhint %}

## 일반 필드

| 필드             | 유형              | 설명                                                                                                                                                                                                                                                       |
| -------------- | --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `binary`       | Boolean         | (선택 사항) 참이면 요청된 원장 개체의 내용을 XRP Ledger의 바이너리 형식의 16진수 문자열로 반환합니다. 그렇지 않으면 JSON 형식으로 데이터를 반환합니다. 기본값은 거짓입니다.[![업데이트: 파문 1.2.0](https://img.shields.io/badge/Updated%20in-rippled%201.2.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.2.0) |
| `ledger_hash`  | 문자열             | (선택 사항) 사용할 원장 버전에 대한 20바이트 16진수 문자열입니다. (원장 지정하기 참조)                                                                                                                                                                                                    |
| `ledger_index` | 문자열 또는 부호 없는 정수 | (선택 사항) 사용할 원장의 원장 인덱스 또는 원장을 자동으로 선택하기 위한 단축 문자열(예: "validated", "closed", "current"). (원장 지정하기 참조)                                                                                                                                                     |

generator 및 ledger 매개변수는 더 이상 사용되지 않으며 별도의 통지 없이 제거될 수 있습니다.

위의 일반 필드 외에도 다음 필드 중 정확히 1개 필드를 지정하여 검색할 객체 유형과 해당 하위 필드를 적절히 지정해야 합니다. 유효한 필드는 다음과 같습니다:

* [`index`](https://xrpl.org/ledger\_entry.html#get-ledger-object-by-id)
* [`account_root`](https://xrpl.org/ledger\_entry.html#get-accountroot-object)
* [`directory`](https://xrpl.org/ledger\_entry.html#get-directorynode-object)
* [`offer`](https://xrpl.org/ledger\_entry.html#get-offer-object)
* [`ripple_state`](https://xrpl.org/ledger\_entry.html#get-ripplestate-object)
* [`check`](https://xrpl.org/ledger\_entry.html#get-check-object)
* [`escrow`](https://xrpl.org/ledger\_entry.html#get-escrow-object)
* [`payment_channel`](https://xrpl.org/ledger\_entry.html#get-paychannel-object)
* [`deposit_preauth`](https://xrpl.org/ledger\_entry.html#get-depositpreauth-object)
* [`ticket`](https://xrpl.org/ledger\_entry.html#get-ticket-object)
* [`nft_page`](https://xrpl.org/ledger\_entry.html#get-nft-page)

{% hint style="info" %}
Caution:

요청에 이러한 유형별 필드를 1개 이상 지정하면 서버는 그 중 1개에 대한 결과만 검색합니다. 서버가 어떤 필드를 선택하는지는 정의되어 있지 않으므로 이 방법은 피해야 합니다.
{% endhint %}

## ID로 ledger 객체 가져오기

고유 ID로 모든 유형의 ledger 객체를 검색합니다.

| 필드      | 유형  | 설명                                              |
| ------- | --- | ----------------------------------------------- |
| `index` | 문자열 | 원장에서 검색할 단일 객체의 객체 ID(64자(256비트) 16진수 문자열) 입니다. |

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "command": "ledger_entry",
  "index": "7DB0788C020F02780A673DC74757F23823FA3014C1866E72CC4CD8B226CD6EF4",
  "ledger_index": "validated"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "ledger_entry",
    "params": [
        {
            "index": "7DB0788C020F02780A673DC74757F23823FA3014C1866E72CC4CD8B226CD6EF4",
            "ledger_index": "validated"
        }
    ]
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Tip: 이 유형의 요청은 ID가 항상 동일하므로 ledger 데이터에 존재하는 모든 싱글톤 객체를 가져오는 데 사용할 수 있습니다. 예시:

* [`Amendments`](https://xrpl.org/amendments-object.html) - `7DB0788C020F02780A673DC74757F23823FA3014C1866E72CC4CD8B226CD6EF4`
* [`FeeSettings`](https://xrpl.org/feesettings.html) - `4BC50C9B0D8515D3EAAE1E74B29A95804346C491EE1A95BF25E4AAB854A6A651`
* [Recent History `LedgerHashes`](https://xrpl.org/ledgerhashes.html) - `B4979A36CDC7F3D3D5C31A4EAE2AC7D7209DDA877588B9AFC66799692AB0D66B`
* [`NegativeUNL`](https://xrpl.org/negativeunl.html) - `2E8A59AA9D3B5B186B0B9E0F62E6C02587CA74A4D778938E957B6357D364B244`
{% endhint %}

## AccountRoot 객체 가져오기

주소로 AccountRoot 객체를 검색합니다. 이는 [account\_info](https://xrpl.org/account\_info.html) 메소드와 거의 동일합니다.

| 필드             | 유형                                                           | 설명                                                                  |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------------- |
| `account_root` | 문자열 - [주소](https://xrpl.org/basic-data-types.html#addresses) | 검색할 [AccountRoot 객체](https://xrpl.org/accountroot.html)의 클래식 주소입니다. |

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": "example_get_accountroot",
  "command": "ledger_entry",
  "account_root": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
  "ledger_index": "validated"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "ledger_entry",
    "params": [
        {
            "account_root": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
            "ledger_index": "validated"
        }
    ]
}
```
{% endtab %}
{% endtabs %}

## DirectoryNode 객체 조회

다른 ledger 객체 목록이 포함된 DirectoryNode를 검색합니다. 문자열(디렉터리의 객체 ID) 또는 객체로 제공될 수 있습니다.

| 필드                    | 유형        | 설명                                                                                                                               |
| --------------------- | --------- | -------------------------------------------------------------------------------------------------------------------------------- |
| `directory`           | 객체 또는 문자열 | 검색할 DirectoryNode 입니다. 문자열인 경우 16진수 형식의 디렉터리 개체 ID 여야 합니다. 객체인 경우 `dir_root`또는 `owner`하위 필드가 필요하며 선택적으로 `sub_index`하위 필드도 필요합니다. |
| `directory.sub_index` | 부호 없는 정수  | _(선택 사항) 제공된 경우_ DirectoryNode의 이후 "페이지"로 이동합니다.                                                                                 |
| `directory.dir_root`  | 문자열       | _(선택 사항)_ 검색할 디렉터리를 16진수 문자열로 식별하는 고유 인덱스입니다.                                                                                    |
| `directory.owner`     | 문자열       | _(선택 사항)_ 이 디렉터리와 연결된 계정의 고유 주소입니다.                                                                                              |

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 3,
  "command": "ledger_entry",
  "directory": {
    "owner": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "sub_index": 0
  },
  "ledger_index": "validated"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "ledger_entry",
    "params": [
        {
            "directory": {
              "owner": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
              "sub_index": 0
            },
            "ledger_index": "validated"
        }
    ]
}
```
{% endtab %}
{% endtabs %}

## 오퍼 오브젝트 조회

환전 오퍼를 정의하는 오퍼 오브젝트를 검색합니다. 문자열(오퍼의 고유 인덱스) 또는 객체로 제공될 수 있습니다.

| 필드              | 유형                                                           | 설명                                                                                                       |
| --------------- | ------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------- |
| `offer`         | 객체 또는 문자열                                                    | 검색할 Offer 개체 입니다. 문자열인 경우 제안에 대한 고유한 개체 ID 로 해석됩니다. 개체인 경우 하위 필드가 필요하며 `account`제안 `seq`을 고유하게 식별해야 합니다. |
| `offer.account` | 문자열 - [주소](https://xrpl.org/basic-data-types.html#addresses) | _`offer`(객체로 지정된 경우 필수_) 제안을 제출한 계정입니다.                                                                  |
| `offer.seq`     | 부호 없는 정수                                                     | _`offer`(객체로 지정된 경우 필수_) Offer 객체를 생성한 거래의 시퀀스 번호입니다 .                                                   |



{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": "example_get_offer",
  "command": "ledger_entry",
  "offer": {
    "account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "seq": 359
  },
  "ledger_index": "validated"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
  "method": "ledger_entry",
  "params": [
    {
      "offer": {
        "account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
        "seq": 359
      },
      "ledger_index": "validated"
    }
  ]
}
```
{% endtab %}
{% endtabs %}

## RippleState 객체 가져오기

두 계정 간의 (비XRP) 통화 잔액을 추적하는 [RippleState](https://xrpl.org/ripplestate.html) 객체를 검색합니다.

| 필드                      | 유형  | 설명                                                                                                    |
| ----------------------- | --- | ----------------------------------------------------------------------------------------------------- |
| `ripple_state`          | 객체  | 검색할 RippleState(신뢰선) 개체를 지정하는 체입니다. 검색할 RippleState 항목을 고유하게 지정하려면 `accounts`및`currency`하위 필드가 필요합니다. |
| `ripple_state.accounts` | 배열  | _`ripple_state`(지정된 경우 필수)_이 RippleState 객체 에 의해 연결된 두 계정을 정의하는 계정 주소의 2길이 배열입니다.                     |
| `ripple_state.currency` | 문자열 | _(`ripple_state`지정된 경우 필수)_ 검색할 RippleState 개체 의 통화 코드입니다.                                            |

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": "example_get_ripplestate",
  "command": "ledger_entry",
  "ripple_state": {
    "accounts": [
      "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
      "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW"
    ],
    "currency": "USD"
  },
  "ledger_index": "validated"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
  "method": "ledger_entry",
  "params": [{
    "ripple_state": {
      "accounts": [
        "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
        "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW"
      ],
      "currency": "USD"
    },
    "ledger_index": "validated"
  }]
}
```
{% endtab %}
{% endtabs %}

## 수표 객체 가져오기

수취인이 현금화할 수 있는 잠재적 결제수단인 수표 객체를 검색합니다. [![New in: rippled 1.0.0](https://img.shields.io/badge/New%20in-rippled%201.0.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/1.0.0)

| 필드      | 유형  | 설명                       |
| ------- | --- | ------------------------ |
| `check` | 문자열 | 검색할 Check 개체 의 개체 ID입니다. |

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": "example_get_check",
  "command": "ledger_entry",
  "check": "C4A46CCD8F096E994C4B0DEAB6CE98E722FC17D7944C28B95127C2659C47CBEB",
  "ledger_index": "validated"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
  "method": "ledger_entry",
  "params": [{
    "check": "C4A46CCD8F096E994C4B0DEAB6CE98E722FC17D7944C28B95127C2659C47CBEB",
    "ledger_index": "validated"
  }]
}
```
{% endtab %}
{% endtabs %}

## 에스크로 객체 가져오기

특정 시간이나 조건이 충족될 때까지 XRP를 보관하는 에스크로 객체를 검색합니다. 문자열(에스크로의 객체 ID) 또는 객체로 제공될 수 있습니다. [![New in: rippled 1.0.0](https://img.shields.io/badge/New%20in-rippled%201.0.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/1.0.0))

| 필드             | 유형                                                           | 설명                                                                                                                         |
| -------------- | ------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------- |
| `escrow`       | 객체 또는 문자열                                                    | 검색할 에스크로 [개체입니다](https://xrpl.org/escrow-object.html). 문자열인 경우 에스크로의 객체 ID (16진수)여야 합니다. 객체인 경우 필수 `owner`및 `seq`하위 필드입니다. |
| `escrow.owner` | 문자열 - [주소](https://xrpl.org/basic-data-types.html#addresses) | _`escrow`(객체로 지정된 경우 필수_) 에스크로 객체의 소유자(발신자)입니다.                                                                            |
| `escrow.seq`   | 부호 없는 정수                                                     | _`escrow`(객체로 지정된 경우 필수_) 에스크로 객체를 생성한 트랜잭션의 시퀀스 번호입니다.                                                                    |

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": "example_get_escrow",
  "command": "ledger_entry",
  "escrow": {
    "owner": "rL4fPHi2FWGwRGRQSH7gBcxkuo2b9NTjKK",
    "seq": 126
  },
  "ledger_index": "validated"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
  "method": "ledger_entry",
  "params": [{
    "escrow": {
      "owner": "rL4fPHi2FWGwRGRQSH7gBcxkuo2b9NTjKK",
      "seq": 126
    },
    "ledger_index": "validated"
  }]
}
```
{% endtab %}
{% endtabs %}

## 페이채널 객체 가져오기

비동기 결제를 위해 XRP를 보관하는 페이채널 객체를 검색합니다. [![New in: rippled 1.0.0](https://img.shields.io/badge/New%20in-rippled%201.0.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/1.0.0)

| 필드                | 유형  | 설명                            |
| ----------------- | --- | ----------------------------- |
| `payment_channel` | 문자열 | 검색할 PayChannel 개체 의 개체 ID입니다. |

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": "example_get_paychannel",
  "command": "ledger_entry",
  "payment_channel": "C7F634794B79DB40E87179A9D1BF05D05797AE7E92DF8E93FD6656E8C4BE3AE7",
  "ledger_index": "validated"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
  "method": "ledger_entry",
  "params": [{
    "payment_channel": "C7F634794B79DB40E87179A9D1BF05D05797AE7E92DF8E93FD6656E8C4BE3AE7",
    "ledger_index": "validated"
  }]
}
```
{% endtab %}
{% endtabs %}

## DepositPreauth 오브젝트 가져오기

입금 승인이 필요한 계정에 대한 결제에 대한 사전 승인을 추적하는 DepositPreauth 객체를 검색합니다. 문자열(입금 사전 인증의 객체 ID) 또는 객체로 제공할 수 있습니다. [![New in: rippled 1.1.0](https://img.shields.io/badge/New%20in-rippled%201.1.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/1.1.0)

| 필드                           | 유형                                                           | 설명                                                                                                                                                                   |
| ---------------------------- | ------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `deposit_preauth`            | 객체 또는 문자열                                                    | 검색할 [DepositPreauth 객체를](https://xrpl.org/depositpreauth-object.html) 지정합니다. 문자열인 경우 DepositPreauth 개체의 개체 ID (16진수)여야 합니다. 객체인 경우 필수 `owner`및 `authorized`하위 필드입니다. |
| `deposit_preauth.owner`      | 문자열 - [주소](https://xrpl.org/basic-data-types.html#addresses) | _(`deposit_preauth`객체로 지정된 경우 필수)_ 사전 승인을 제공한 계정입니다.                                                                                                                 |
| `deposit_preauth.authorized` | 문자열 - [주소](https://xrpl.org/basic-data-types.html#addresses) | _(`deposit_preauth`객체로 지정한 경우 필수)_ 사전 승인을 받은 계정입니다.                                                                                                                  |

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": "example_get_deposit_preauth",
  "command": "ledger_entry",
  "deposit_preauth": {
    "owner": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "authorized": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX"
  },
  "ledger_index": "validated"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
  "method": "ledger_entry",
  "params": [{
    "deposit_preauth": {
      "owner": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
      "authorized": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX"
    },
    "ledger_index": "validated"
  }]
}
```
{% endtab %}
{% endtabs %}

## 티켓 객체 가져오기

나중에 사용하기 위해 따로 보관된 시퀀스 번호를 나타내는 티켓 객체를 검색합니다. 문자열(티켓의 객체 ID) 또는 객체로 제공될 수 있습니다. (티켓 배치 개정으로 추가됨)

| 필드                  | 유형                                                           | 설명                                                                                                          |
| ------------------- | ------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------- |
| `ticket`            | 객체 또는 문자열                                                    | 검색할 티켓 개체입니다. 문자열인 경우 16진수로 티켓의 개체 ID 여야 합니다. 객체인 경우 티켓 항목을 고유하게 지정하려면 `account`및`ticket_seq` 하위 필드가 필요합니다. |
| `ticket.account`    | 문자열 - [주소](https://xrpl.org/basic-data-types.html#addresses) | _(`ticket`객체로 지정된 경우 필수)_ 티켓 객체의 소유자입니다.                                                                    |
| `ticket.ticket_seq` | 숫자                                                           | _(`ticket`객체로 지정된 경우 필수)_ 검색할 티켓 항목의 티켓 시퀀스 번호입니다.                                                          |

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": "example_get_ticket",
  "command": "ledger_entry",
  "ticket": {
    "account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "ticket_seq": 389
  },
  "ledger_index": "validated"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
  "method": "ledger_entry",
  "params": [{
    "ticket": {
      "account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
      "ticket_seq": 389
    },
    "ledger_index": "validated"
  }]
}
```
{% endtab %}
{% endtabs %}

## NFT 페이지 가져오기

NFT 페이지를 원시 ledger 형식으로 반환합니다.

| `Field`    | 유형  | 설명                      |
| ---------- | --- | ----------------------- |
| `nft_page` | 문자열 | 검색할 NFT 페이지 의 개체 ID입니다. |

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": "example_get_nft_page",
    "command": "ledger_entry",
    "nft_page": "255DD86DDF59D778081A06D02701E9B2C9F4F01DFFFFFFFFFFFFFFFFFFFFFFFF",
    "ledger_index": "validated"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
  "method": "ledger_entry",
  "params": [{
    "nft_page": "255DD86DDF59D778081A06D02701E9B2C9F4F01DFFFFFFFFFFFFFFFFFFFFFFFF",
    "ledger_index": "validated"
  }]
}
```
{% endtab %}
{% endtabs %}

## 응답 형식

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| 필드             | 유형       | 설명                                                                   |
| -------------- | -------- | -------------------------------------------------------------------- |
| `index`        | 문자열      | 이 원장 객체 의 고유 ID입니다.                                                  |
| `ledger_index` | 부호 없는 정수 | 이 데이터를 검색할 때 사용된 원장의 원장 인덱스입니다.                                      |
| `node`         | 객체       | _(`"binary": true`지정하는 경우 생략)_ 원장 형식 에 따라 해당 원장 개체의 데이터를 포함하는 개체입니다. |
| `node_binary`  | 문자열      | _(`"binary": true`지정하는 경우 생략)_ 원장 객체를 16진수로 표현한 이진수 입니다.             |

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": "example_get_accountroot",
  "result": {
    "index": "13F1A95D7AAB7108D5CE7EEAF504B2894B8C674E6D68499076441C4837282BF8",
    "ledger_hash": "31850E8E48E76D1064651DF39DF4E9542E8C90A9A9B629F4DE339EB3FA74F726",
    "ledger_index": 61966146,
    "node": {
      "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
      "AccountTxnID": "4E0AA11CBDD1760DE95B68DF2ABBE75C9698CEB548BEA9789053FCB3EBD444FB",
      "Balance": "424021949",
      "Domain": "6D64756F31332E636F6D",
      "EmailHash": "98B4375E1D753E5B91627516F6D70977",
      "Flags": 9568256,
      "LedgerEntryType": "AccountRoot",
      "MessageKey": "0000000000000000000000070000000300",
      "OwnerCount": 12,
      "PreviousTxnID": "4E0AA11CBDD1760DE95B68DF2ABBE75C9698CEB548BEA9789053FCB3EBD444FB",
      "PreviousTxnLgrSeq": 61965653,
      "RegularKey": "rD9iJmieYHn8jTtPjwwkW2Wm9sVDvPXLoJ",
      "Sequence": 385,
      "TransferRate": 4294967295,
      "index": "13F1A95D7AAB7108D5CE7EEAF504B2894B8C674E6D68499076441C4837282BF8"
    },
    "validated": true
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
    "index": "13F1A95D7AAB7108D5CE7EEAF504B2894B8C674E6D68499076441C4837282BF8",
    "ledger_hash": "395946243EA36C5092AE58AF729D2875F659812409810A63096AC006C73E656E",
    "ledger_index": 61966165,
    "node": {
      "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
      "AccountTxnID": "4E0AA11CBDD1760DE95B68DF2ABBE75C9698CEB548BEA9789053FCB3EBD444FB",
      "Balance": "424021949",
      "Domain": "6D64756F31332E636F6D",
      "EmailHash": "98B4375E1D753E5B91627516F6D70977",
      "Flags": 9568256,
      "LedgerEntryType": "AccountRoot",
      "MessageKey": "0000000000000000000000070000000300",
      "OwnerCount": 12,
      "PreviousTxnID": "4E0AA11CBDD1760DE95B68DF2ABBE75C9698CEB548BEA9789053FCB3EBD444FB",
      "PreviousTxnLgrSeq": 61965653,
      "RegularKey": "rD9iJmieYHn8jTtPjwwkW2Wm9sVDvPXLoJ",
      "Sequence": 385,
      "TransferRate": 4294967295,
      "index": "13F1A95D7AAB7108D5CE7EEAF504B2894B8C674E6D68499076441C4837282BF8"
    },
    "status": "success",
    "validated": true
  }
}
```
{% endtab %}
{% endtabs %}

## 발생 가능한 오류

* 모든 범용 오류 유형입니다.
* deprecatedFeature - 요청에서 제거된 필드(예: 생성기)를 지정했습니다.
* entryNotFound - 요청된 ledger 객체가 ledger에 존재하지 않습니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* lgrNotFound - ledger\_hash 또는 ledger\_index로 지정한 ledger이 존재하지 않거나, 존재하지만 서버에 없습니다.
* malformedAddress - 요청이 주소 필드를 잘못 지정했습니다.
* malformedCurrency - 요청이 통화 코드 필드를 잘못 지정했습니다.
* malformedOwner - 요청이 escrow.owner 하위 필드를 잘못 지정했습니다.
* malformedRequest - 요청이 잘못된 필드 조합을 제공했거나 하나 이상의 필드에 대해 잘못된 유형을 제공했습니다.
* unknownOption - 요청에 제공된 필드가 예상 요청 형식과 일치하지 않습니다.
