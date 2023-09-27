# ledger

공개 ledger에 대한 정보를 검색합니다.

## 요청 형식

요청 형식의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": 14,
    "command": "ledger",
    "ledger_index": "validated",
    "full": false,
    "accounts": false,
    "transactions": false,
    "expand": false,
    "owner_funds": false
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "ledger",
    "params": [
        {
            "ledger_index": "validated",
            "accounts": false,
            "full": false,
            "transactions": false,
            "expand": false,
            "owner_funds": false
        }
    ]
}
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함될 수 있습니다:

| `Field`        | 유형                                                          | 필수 여부 | 설명                                                                                                                                                                                                                                             |
| -------------- | ----------------------------------------------------------- | ----- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ledger_hash`  | [해시](https://xrpl.org/basic-data-types.html#hashes)         | 아니요   | 사용할 원장 버전에 대한 20바이트 16진수 문자열입니다. (원장 지정하기 참조).                                                                                                                                                                                                 |
| `ledger_index` | [원장지수](https://xrpl.org/basic-data-types.html#ledger-index) | 아니요   | 사용할 원장의 원장 인덱스 또는 원장을 자동으로 선택하기 위한 바로가기 문자열입니다. (원장 지정하기 참조)                                                                                                                                                                                   |
| `full`         | Boolean                                                     | 아니요   | 관리자만 참이면 전체 원장에 대한 전체 정보를 반환합니다. 원장 버전을 지정하지 않으면 무시됩니다. 기본값은 거짓입니다. (트랜잭션, 계정, 확장을 활성화하는 것과 동일합니다.) C클리오 서버는 이 필드를 지원하지 않습니다. 주의: 메인넷에서는 기가바이트 상당의 데이터가 될 수 있으므로 요청이 시간 초과될 수 있습니다.                                                            |
| `accounts`     | Boolean                                                     | 아니요   | 관리자 전용입니다. 참이면 원장의 전체 상태 데이터를 반환합니다. 원장 버전을 지정하지 않으면 무시됩니다. 기본값은 false입니다. 주의: 메인넷에서는 이 데이터가 기가바이트에 달할 수 있으므로 요청이 시간 초과될 수 있습니다. 페이지가 지정된 여러 요청에 걸쳐 상태 데이터를 가져오려면 ledger\_data를 대신 사용하세요.                                                      |
| `transactions` | Boolean                                                     | 아니요   | true이면 지정된 원장 버전의 트랜잭션에 대한 정보를 반환합니다. 기본값은 거짓입니다. 원장 버전을 지정하지 않으면 무시됩니다.                                                                                                                                                                       |
| `expand`       | Boolean                                                     | 아니요   | 트랜잭션/계정 정보에 대해 해시 대신 전체 JSON 형식의 정보를 제공합니다. 기본값은 거짓입니다. 트랜잭션, 계정 또는 둘 다 요청하지 않으면 무시됩니다.                                                                                                                                                        |
| `owner_funds`  | Boolean                                                     | 아니요   | true이면 오퍼 크리에이트 트랜잭션의 메타데이터에 소유자\_자금 필드를 응답에 포함시킵니다. 기본값은 false입니다. 트랜잭션이 포함되지 않고 expand가 true인 경우 무시됩니다.                                                                                                                                      |
| `binary`       | Boolean                                                     | 아니요   | true이고 트랜잭션과 확장도 모두 true인 경우 트랜잭션 정보를 JSON 형식 대신 이진 형식(16진수 문자열)으로 반환합니다.                                                                                                                                                                      |
| `queue`        | Boolean                                                     | 아니요   | true이고 명령이 현재 원장을 요청하는 경우, 대기 중인 트랜잭션 배열을 결과에 포함합니다.                                                                                                                                                                                           |
| `type`         | 문자열                                                         | 아니요   | 원장 항목 유형으로 필터링합니다. 유효한 유형은 `account`, `amendments`, `amm` , `check`, `deposit_preauth`, `directory`, `escrow`, `fee`, `hashes`, `nft_offer`, `offer`, `payment_channel`, `signer_list`, `state` (신뢰선), 과 `ticket`. 계정(상태 데이터)을 요청하지 않으면 무시됩니다. |

ledger 필드는 더 이상 사용되지 않으며 추가 공지 없이 제거될 수 있습니다.

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 14,
  "result": {
    "ledger": {
      "accepted": true,
      "account_hash": "53BD4650A024E27DEB52DBB6A52EDB26528B987EC61C895C48D1EB44CEDD9AD3",
      "close_flags": 0,
      "close_time": 638329241,
      "close_time_human": "2020-Mar-24 01:40:41.000000000 UTC",
      "close_time_resolution": 10,
      "closed": true,
      "hash": "1723099E269C77C4BDE86C83FA6415D71CF20AA5CB4A94E5C388ED97123FB55B",
      "ledger_hash": "1723099E269C77C4BDE86C83FA6415D71CF20AA5CB4A94E5C388ED97123FB55B",
      "ledger_index": "54300932",
      "parent_close_time": 638329240,
      "parent_hash": "DF68B3BCABD31097634BABF0BDC87932D43D26E458BFEEFD36ADF2B3D94998C0",
      "seqNum": "54300932",
      "totalCoins": "99991024049648900",
      "total_coins": "99991024049648900",
      "transaction_hash": "50B3A8FE2C5620E43AA57564209AEDFEA3E868CFA2F6E4AB4B9E55A7A62AAF7B"
    },
    "ledger_hash": "1723099E269C77C4BDE86C83FA6415D71CF20AA5CB4A94E5C388ED97123FB55B",
    "ledger_index": 54300932,
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
    "ledger": {
      "accepted": true,
      "account_hash": "B258A8BB4743FB74CBBD6E9F67E4A56C4432EA09E5805E4CC2DA26F2DBE8F3D1",
      "close_flags": 0,
      "close_time": 638329271,
      "close_time_human": "2020-Mar-24 01:41:11.000000000 UTC",
      "close_time_resolution": 10,
      "closed": true,
      "hash": "3652D7FD0576BC452C0D2E9B747BDD733075971D1A9A1D98125055DEF428721A",
      "ledger_hash": "3652D7FD0576BC452C0D2E9B747BDD733075971D1A9A1D98125055DEF428721A",
      "ledger_index": "54300940",
      "parent_close_time": 638329270,
      "parent_hash": "AE996778246BC81F85D5AF051241DAA577C23BCA04C034A7074F93700194520D",
      "seqNum": "54300940",
      "totalCoins": "99991024049618156",
      "total_coins": "99991024049618156",
      "transaction_hash": "FC6FFCB71B2527DDD630EE5409D38913B4D4C026AA6C3B14A3E9D4ED45CFE30D"
    },
    "ledger_hash": "3652D7FD0576BC452C0D2E9B747BDD733075971D1A9A1D98125055DEF428721A",
    "ledger_index": 54300940,
    "status": "success",
    "validated": true
  }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드를 포함하여 ledger에 대한 정보가 포함됩니다:

| `Field`                        | 유형      | 설명                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ------------------------------ | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ledger`                       | 객체      | 이 원장의 전체 헤더 데이터입니다.                                                                                                                                                                                                                                                                                                                                                                                                     |
| `ledger.account_hash`          | 문자열     | 이 원장에 있는 모든 계정 상태 정보의 해시(16진수)                                                                                                                                                                                                                                                                                                                                                                                          |
| `ledger.accountState`          | 배열      | (요청하지 않는 한 생략) 이 원장의 모든 [계정 상태 정보입니다.](https://xrpl.org/ledger-data-formats.html)                                                                                                                                                                                                                                                                                                                                       |
| `ledger.close_flags`           | 정수      | [이 원장의 마감과 관련된 플래그](https://xrpl.org/ledger-header.html#close-flags) 의 비트맵입니다.                                                                                                                                                                                                                                                                                                                                          |
| `ledger.close_time`            | 정수      | [Ripple Epoch 이후](https://xrpl.org/basic-data-types.html#specifying-time) 이 원장이 닫힌 시간(초)입니다.                                                                                                                                                                                                                                                                                                                            |
| `ledger.close_time_human`      | 문자열     | 이 원장이 닫힌 시간으로, 사람이 읽을 수 있는 형식입니다. 항상 UTC 시간대를 사용합니다.[![업데이트: 파문 1.5.0](https://img.shields.io/badge/Updated%20in-rippled%201.5.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.5.0)                                                                                                                                                                                                                      |
| `ledger.close_time_resolution` | 정수      | 원장 마감 시간은 이 초 이내로 반올림됩니다.                                                                                                                                                                                                                                                                                                                                                                                               |
| `ledger.closed`                | Boolean | 이 원장의 폐쇄 여부                                                                                                                                                                                                                                                                                                                                                                                                             |
| `ledger.ledger_hash`           | 문자열     | 전체 원장의 고유 식별 해시입니다.                                                                                                                                                                                                                                                                                                                                                                                                     |
| `ledger.ledger_index`          | 문자열     | 인용된 정수로 표시된 이 원장의 [원장](https://xrpl.org/basic-data-types.html#ledger-index) 색인                                                                                                                                                                                                                                                                                                                                          |
| `ledger.parent_close_time`     | 정수      | 이전 원장이 마감된 시간입니다.                                                                                                                                                                                                                                                                                                                                                                                                       |
| `ledger.parent_hash`           | 문자열     | 이 원장 바로 앞에 나온 원장의 고유 식별 해시입니다.                                                                                                                                                                                                                                                                                                                                                                                          |
| `ledger.total_coins`           | 문자열     | 네트워크의 총 XRP 드롭 수(인용된 정수) (이는 거래 비용이 XRP를 파괴함에 따라 감소합니다.)                                                                                                                                                                                                                                                                                                                                                                |
| `ledger.transaction_hash`      | 문자열     | 이 원장에 포함된 거래 정보의 해시(16진수)                                                                                                                                                                                                                                                                                                                                                                                               |
| `ledger.transactions`          | 배열      | (요청하지 않으면 생략) 이 원장 버전에 적용된 거래입니다. 기본적으로 멤버는 트랜잭션을 식별하는 해시 [문자열](https://xrpl.org/basic-data-types.html#hashes) 입니다. 요청이 true로 지정된 경우 멤버는 요청이 true로 `expand`지정되었는지 여부에 따라 JSON 또는 `binary`로 트랜잭션의 전체 표현이 됩니다.                                                                                                                                                                                                            |
| `ledger_hash`                  | 문자열     | 전체 원장의 고유 식별 해시입니다.                                                                                                                                                                                                                                                                                                                                                                                                     |
| `ledger_index`                 | 숫자      | 이 원장의 [원장 색인입니다](https://xrpl.org/basic-data-types.html#ledger-index).                                                                                                                                                                                                                                                                                                                                                  |
| `validated`                    | Boolean | _(생략 가능)_ 인 경우 `true`검증된 원장 버전입니다. 생략하거나 로 설정하면 `false`이 원장의 데이터가 최종 데이터가 아닙니다.                                                                                                                                                                                                                                                                                                                                         |
| `queue_data`                   | 배열      | _(매개변수와 함께 요청하지 않는 한 생략 `queue`)_ 대기열과 동일한 순서로 대기 중인 트랜잭션을 설명하는 객체 배열입니다. 요청이 true로 지정된 경우 멤버에는 요청이 true로 `expand`지정되었는지 여부에 따라 JSON 또는 바이너리 형식으로 트랜잭션의 전체 표현이 포함됩니다. [FeeEscalation 수정](https://xrpl.org/known-amendments.html#feeescalation)`binary` 안으로 추가되었습니다.[![새로운 기능: Rippled 0.70.0](https://img.shields.io/badge/New%20in-rippled%200.70.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/0.70.0) |

다음 필드는 더 이상 사용되지 않으며 추가 공지 없이 제거될 수 있습니다: accepted, hash(대신 ledger\_hash 사용), seqNum(대신 ledger\_index 사용), totalCoins(대신 total\_coins 사용).

queue\_data 배열의 각 멤버는 대기열에 있는 트랜잭션 하나를 나타냅니다. 이 객체의 일부 필드는 아직 계산되지 않았기 때문에 생략될 수 있습니다. 이 객체의 필드는 다음과 같습니다:

| 필드                  | 값         | 설명                                                                                                                                                                                                                                                                                |
| ------------------- | --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `account`           | 문자열       | 이 대기 중인 트랜잭션에 대한 보낸 사람의 주소 [입니다.](https://xrpl.org/basic-data-types.html#addresses)                                                                                                                                                                                               |
| `tx`                | 문자열 또는 객체 | 기본적으로 이는 트랜잭션의 [식별 해시가](https://xrpl.org/basic-data-types.html#hashes) 포함된 문자열입니다. `tx_blob`트랜잭션이 이진 형식으로 확장된 경우 이는 트랜잭션의 이진 형식을 10진수 문자열로 포함하는 필드만 있는 개체입니다. 트랜잭션이 JSON 형식으로 확장된 경우 필드 에 트랜잭션 식별 해시를 포함하는 [트랜잭션 개체가](https://xrpl.org/transaction-formats.html)`hash` 포함된 개체입니다. |
| `retries_remaining` | 숫자        | 이 트랜잭션이 삭제되기 전까지 재시도할 수 있는 횟수입니다.                                                                                                                                                                                                                                                 |
| `preflight_result`  | 문자열       | 사전 거래 확인의 잠정 결과입니다. 이것은 항상 `tesSUCCESS`.                                                                                                                                                                                                                                          |
| `last_result`       | 문자열       | _(생략 가능) 이 트랜잭션이_ [재시도 가능한( `ter`) 결과를](https://xrpl.org/ter-codes.html) 얻은 후 대기열에 남아 있는 경우 `ter`얻은 정확한 결과 코드입니다.                                                                                                                                                                 |
| `auth_change`       | Boolean   | _(생략 가능)_ 이 거래가 이 주소의 [거래 승인 방식을](https://xrpl.org/transaction-basics.html#authorizing-transactions) 변경하는지 여부.                                                                                                                                                                    |
| `fee`               | 문자열       | _(생략 가능)_ 이 거래의 거래 비용( XRP [방울 ](https://xrpl.org/transaction-cost.html)[단위](https://xrpl.org/basic-data-types.html#specifying-currency-amounts) ).                                                                                                                               |
| `fee_level`         | 문자열       | _(생략될 수 있음)_ 이 거래 유형의 최소 비용을 기준으로 한 이 거래의 거래 비용( [수수료 수준)입니다](https://xrpl.org/transaction-cost.html#fee-levels).                                                                                                                                                                 |
| `max_spend_drops`   | 문자열       | _(생략될 수 있음)_ 이 거래 [에서 잠재적으로 전송되거나 파괴될 수 있는 XRP](https://xrpl.org/basic-data-types.html#specifying-currency-amounts) 의 최대 금액입니다.                                                                                                                                                   |

요청에 "owner\_funds": true 및 확장 트랜잭션이 지정된 경우, 응답에는 각 [OfferCreate](https://xrpl.org/offercreate.html) 트랜잭션의 메타데이터 객체에 owner\_funds 필드가 있습니다. 이 필드의 목적은 유효성이 검증된 새로운 ledger이 추가될 때마다 오퍼의 펀딩 상태를 더 쉽게 추적할 수 있도록 하기 위한 것입니다. 이 필드는 오더북 가입 스트림의 이 필드 버전과 약간 다르게 정의됩니다:

| `Field`       | 값   | 설명                                                                                                           |
| ------------- | --- | ------------------------------------------------------------------------------------------------------------ |
| `owner_funds` | 문자열 | 이 원장의 모든 트랜잭션이 실행된 후 이 OfferCreate 트랜잭션을 보내는 `TakerGets`통화 의 숫자 금액입니다. `Account`통화 금액이 동결 되었는지 여부는 확인하지 않습니다 |

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* lgrNotFound - ledger\_hash 또는 ledger\_index로 지정한 ledger이 존재하지 않거나, 존재하지만 서버에 없습니다.
* noPermission - 전체 또는 계정을 true로 지정했지만 관리자로 서버에 연결되어 있지 않은 경우(일반적으로 관리자는 로컬 포트에 연결해야 함).

&#x20;
