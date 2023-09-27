# account\_channels

(페이찬 개정에 의해 추가됨. [![New in: rippled 0.33.0](https://img.shields.io/badge/New%20in-rippled%200.33.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.33.0))

account\_channels 메소드는 계정의 결제 채널에 대한 정보를 반환합니다. 여기에는 지정된 계정이 채널의 소스가 목적지가 아닌 채널인 채널만 포함됩니다. (채널의 '소스'와 '소유자'는 동일합니다.) 검색된 모든 정보는 특정 버전의 ledger을 기준으로 합니다.

## 요청 형식

요청 형식의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 1,
  "command": "account_channels",
  "account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
  "destination_account": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
  "ledger_index": "validated"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "account_channels",
    "params": [{
        "account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
        "destination_account": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
        "ledger_index": "validated"
    }]
}
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| 필드                    | 유형                                                           | 필수 여부 | 설명                                                                                                                                                                                     |
| --------------------- | ------------------------------------------------------------ | ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `account`             | 문자열 - [주소](https://xrpl.org/basic-data-types.html#addresses) | 예     | 이 계정이 채널의 소유자/소스인 채널을 찾아보세요.                                                                                                                                                           |
| `destination_account` | 문자열 - [주소](https://xrpl.org/basic-data-types.html#addresses) | 아니요   | 두 번째 계정 제공된 경우 대상이 이 계정인 결제 채널로 결과를 필터링합니다.                                                                                                                                            |
| `ledger_hash`         | 문자열                                                          | 아니요   | 사용할 원장 버전에 대한 20바이트 16진수 문자열입니다. ([Specifying Ledgers](https://xrpl.org/basic-data-types.html#specifying-ledgers) 참조)                                                                  |
| `ledger_index`        | 숫자 또는 문자열                                                    | 아니요   | 사용할 원장의 [원장 인덱스](https://xrpl.org/basic-data-types.html#ledger-index) 또는 자동으로 원장을 선택하는 단축 문자열입니다. ([Specifying Ledgers](https://xrpl.org/basic-data-types.html#specifying-ledgers) 참조) |
| `limit`               | 숫자                                                           | 아니요   | 검색할 트랜잭션 수를 제한합니다. 10보다 작거나 400보다 클 수 없습니다. 기본값은 200입니다.                                                                                                                               |
| `marker`              | marker                                                       | 아니요   | 이전에 페이지를 매긴 응답의 값입니다. 해당 응답이 중단된 데이터 검색을 재개합니다.                                                                                                                                        |

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 1,
  "result": {
    "account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "channels": [
      {
        "account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
        "amount": "1000",
        "balance": "0",
        "channel_id": "C7F634794B79DB40E87179A9D1BF05D05797AE7E92DF8E93FD6656E8C4BE3AE7",
        "destination_account": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
        "public_key": "aBR7mdD75Ycs8DRhMgQ4EMUEmBArF8SEh1hfjrT2V9DQTLNbJVqw",
        "public_key_hex": "03CFD18E689434F032A4E84C63E2A3A6472D684EAF4FD52CA67742F3E24BAE81B2",
        "settle_delay": 60
      }
    ],
    "ledger_hash": "1EDBBA3C793863366DF5B31C2174B6B5E6DF6DB89A7212B86838489148E2A581",
    "ledger_index": 71766314,
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
    "account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "channels": [
      {
        "account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
        "amount": "1000",
        "balance": "0",
        "channel_id": "C7F634794B79DB40E87179A9D1BF05D05797AE7E92DF8E93FD6656E8C4BE3AE7",
        "destination_account": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
        "public_key": "aBR7mdD75Ycs8DRhMgQ4EMUEmBArF8SEh1hfjrT2V9DQTLNbJVqw",
        "public_key_hex": "03CFD18E689434F032A4E84C63E2A3A6472D684EAF4FD52CA67742F3E24BAE81B2",
        "settle_delay": 60
      }
    ],
    "ledger_hash": "27F530E5C93ED5C13994812787C1ED073C822BAEC7597964608F2C049C2ACD2D",
    "ledger_index": 71766343,
    "status": "success",
    "validated": true
  }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| 필드             | 유형       | 설명                                                                                                                                                                                                                           |
| -------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `account`      | 문자열      | 결제 채널의 출처/소유자의 주소입니다. 이는 `account`요청 필드에 해당합니다.                                                                                                                                                                              |
| `channels`     | 채널 객체 배열 | 이 계정이 소유한 결제 채널입니다.[![업데이트: 파문 1.5.0](https://img.shields.io/badge/Updated%20in-rippled%201.5.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.5.0)                                                            |
| `ledger_hash`  | 문자열      | (생략 가능) 이 응답을 생성하는 데 사용된 원장 버전의 식별 해시입니다.[![새로운 기능: Rippled 0.90.0](https://img.shields.io/badge/New%20in-rippled%200.90.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.90.0)                                |
| `ledger_index` | 숫자       | 이 응답을 생성하는 데 사용된 원장 버전의 원장 인덱스입니다.[![새로운 기능: Rippled 0.90.0](https://img.shields.io/badge/New%20in-rippled%200.90.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.90.0)                                       |
| `validated`    | Boolean  | (생략 가능) 참이면, 이 응답의 정보는 유효성 검사된 원장 버전에서 가져온 것입니다. 그렇지 않으면 정보가 변경될 수 있습니다.[![새로운 기능: Rippled 0.90.0](https://img.shields.io/badge/New%20in-rippled%200.90.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.90.0) |
| `limit`        | 숫자       | (생략 가능) 이 요청에서 실제로 반환된 채널 오브젝트 수에 대한 제한입니다.                                                                                                                                                                                  |
| `marker`       | marker   | (생략 가능) 페이지 매김을 위한 서버 정의 값입니다. 이 값을 다음 호출에 전달하여 이 호출이 중단된 부분부터 결과 가져오기를 재개합니다. 이 호출 이후에 추가 페이지가 없는 경우 생략됩니다.                                                                                                                 |

각 채널 객체에는 다음과 같은 필드가 있습니다:

| 필드                   | 유형       | 설명                                                                                                                                                                                            |
| -------------------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| account              | 문자열      | [Address](https://xrpl.org/basic-data-types.html#addresses) 와 같은 채널의 소유자입니다.                                                                                                                  |
| amount               | 문자열      | 이 채널에 할당된 [XRP](https://xrpl.org/basic-data-types.html#specifying-currency-amounts) 의 총량입니다.                                                                                                  |
| balance              | 문자열      | 사용된 원장 버전을 기준으로 이 채널에서 지급된 총 XRP 금액(드롭 단위)입니다. (금액에서 잔액을 빼면 채널에 남은 XRP의 양을 계산할 수 있습니다.)                                                                                                       |
| channel\_id          | 문자열      | 64자 16진수 문자열로 된 이 채널의 고유 ID입니다. 이는 ledger의 상태 데이터에 있는 [채널 개체의 ID 이기도 합니다.](https://xrpl.org/paychannel.html#paychannel-id-format)                                                             |
| destination\_account | 문자열      | [Address](https://xrpl.org/basic-data-types.html#addresses) 와 같은 채널의 대상 계정입니다 . 이 계정만 채널이 열려 있는 동안 XRP를 받을 수 있습니다.                                                                            |
| settle\_delay        | 부호 없는 정수 | 채널 소유자가 채널 폐쇄를 요청한 후 결제 채널이 열려 있어야 하는 시간(초)입니다.                                                                                                                                               |
| public\_key          | 문자열      | _(생략 가능) XRP Ledger의_ [base58](https://xrpl.org/base58-encodings.html) 형식 으로 된 결제 채널의 공개 키입니다 . 이 채널에 대해 서명된 클레임은 일치하는 키 쌍으로 상환되어야 합니다.                                                       |
| public\_key\_hex     | 문자열      | _(생략 가능)_ 채널 생성 시 지정된 경우 결제 채널의 16진수 형식 공개 키입니다. 이 채널에 대해 서명된 클레임은 일치하는 키 쌍으로 상환되어야 합니다.                                                                                                      |
| expiration           | 부호 없는 정수 | _(생략 가능)_ 이 채널이 만료되도록 설정된 Ripple 에포크 이후의 시간 (초)입니다. 이 만료일은 변경할 수 있습니다. 가장 최근에 검증된 ledger의 마감 시간 이전인 경우 채널이 만료됩니다.                                                                             |
| cancel\_after        | 부호 없는 정수 | _(생략 가능)_ 채널 생성 시 지정된 경우 이 채널의 불변 만료 시간( Ripple 에포크 이후 초)입니다. 가장 최근에 검증된 ledger의 마감 시간 이전인 경우 채널이 만료됩니다.                                                                                      |
| source\_tag          | 부호 없는 정수 | _(생략될 수 있음)_ 채널 생성 시 지정된 경우 이 결제 채널을 통한 결제에 대한 소스 태그로 사용할 32비트 부호 없는 정수입니다. 이는 결제 채널의 창시자 또는 원본 계정의 기타 목적을 나타냅니다. 일반적으로 이 채널에서 결제를 반송하는 경우 반품 결제의 DestinationTag에 이 값을 지정해야 합니다.              |
| destination\_tag     | 부호 없는 정수 | _(생략될 수 있음)_ 채널 생성 시 지정된 경우 이 채널을 통한 결제의 [대상 태그](https://xrpl.org/become-an-xrp-ledger-gateway.html#source-and-destination-tags)로 사용할 32비트 부호 없는 정수입니다. 이는 지불 채널의 수혜자 또는 대상 계정의 기타 목적을 나타냅니다. |

## 발생가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* actNotFound - 요청의 계정 필드에 지정된 주소가 장부에 있는 계정과 일치하지 않습니다.
* lgrNotFound - ledger\_hash 또는 ledger\_index로 지정한 ledger이 존재하지 않거나, 존재하지만 서버에 없습니다.
