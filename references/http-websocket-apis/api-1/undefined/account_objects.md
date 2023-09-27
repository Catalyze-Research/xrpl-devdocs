# account\_objects

account\_objects 명령은 계정이 소유한 모든 ledger 항목에 대한 원시 ledger 형식을 반환합니다. 계정의 신뢰선과 잔액에 대한 상위 수준 보기를 보려면 대신 account\_lines 메소드를 참조하세요.

account\_objects 응답에 나타날 수 있는 객체 유형은 다음과 같습니다:

* 현재 진행 중이거나, 펀딩되지 않았거나, 만료되었지만 아직 제거되지 않은 주문에 대한 오퍼 항목입니다. (자세한 내용은 오퍼의 라이프사이클을 참조하세요.)
* 이 계정 측이 기본 상태가 아닌 신뢰선에 대한 Ripple 상태 항목.
* 계정에 다중 서명이 활성화된 경우 계정의 서명자 목록입니다.
* 아직 실행되거나 취소되지 않은 보류 결제에 대한 에스크로 항목.
* 열려 있는 결제 채널에 대한 PayChannel 항목입니다.
* 보류 중인 수표에 대한 수표 항목.
* 입금 사전 승인에 대한 입금 사전 승인 항목.
* 티켓에 대한 티켓 항목.
* NFT 토큰 구매 또는 판매 오퍼를 위한 NFTokenOffer 항목.
* NFT 토큰 컬렉션을 위한 NFTokenPage 항목.[![New in: rippled 1.11.0](https://img.shields.io/badge/New%20in-rippled%201.11.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.11.0)

## 요청 형식

요청 형식의 예시입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 8,
  "command": "account_objects",
  "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
  "ledger_index": "validated",
  "type": "state",
  "deletion_blockers_only": false,
  "limit": 10
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "account_objects",
    "params": [
        {
            "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
            "ledger_index": "validated",
            "type": "state",
            "deletion_blockers_only": false,
            "limit": 10
        }
    ]
}
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| `Field`                  | 유형                                                          | 필수 여부 | 설명                                                                                                                                                                                                       |
| ------------------------ | ----------------------------------------------------------- | ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `account`                | 문자열                                                         | 예     | 계정의 고유 식별자(가장 일반적으로 계정의 주소)입니다.                                                                                                                                                                          |
| `deletion_blockers_only` | Boolean                                                     | 아니요   | true인 경우 응답에는 이 계정이 삭제되는 것을 차단하는 개체만 포함됩니다. 기본값은 거짓입니다.[![새로운 기능: Rippled 1.4.0](https://img.shields.io/badge/New%20in-rippled%201.4.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.4.0) |
| `ledger_hash`            | [해시](https://xrpl.org/basic-data-types.html#hashes)         | 아니요   | 사용할 원장 버전에 대한 20바이트 16진수 문자열입니다. (원장 지정하기 참조)                                                                                                                                                            |
| `ledger_index`           | [원장지수](https://xrpl.org/basic-data-types.html#ledger-index) | 아니요   | 사용할 원장의 원장 인덱스 또는 원장을 자동으로 선택하기 위한 단축 문자열입니다. (원장 지정하기 참조)                                                                                                                                               |
| `limit`                  | 숫자                                                          | 아니요   | 결과에 포함할 최대 개체 수입니다. 관리자가 아닌 연결의 경우 10\~400의 포함 범위 내에 있어야 합니다. 기본값은 200입니다                                                                                                                                |
| `marker`                 | marker                                                      | 아니요   | 이전 페이지 매김된 응답의 값입니다. 해당 응답이 중단된 부분부터 데이터 검색을 다시 시작합니다.                                                                                                                                                   |
| `type`                   | 문자열                                                         | 아니요   | 원장 항목 유형으로 결과를 필터링합니다. 유효한 유형은 `heck`, `deposit_preauth`, `escrow`, `nft_offer`, `nft_page`, `offer`, `payment_channel`, `signer_list`, `state` (신뢰선), 와`ticket` 입니다.                                    |

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": 8,
    "status": "success",
    "type": "response",
    "result": {
        "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
        "account_objects": [
            {
                "Balance": {
                    "currency": "ASP",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "0"
                },
                "Flags": 65536,
                "HighLimit": {
                    "currency": "ASP",
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "0"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "ASP",
                    "issuer": "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
                    "value": "10"
                },
                "LowNode": "0000000000000000",
                "PreviousTxnID": "BF7555B0F018E3C5E2A3FF9437A1A5092F32903BE246202F988181B9CED0D862",
                "PreviousTxnLgrSeq": 1438879,
                "index": "2243B0B630EA6F7330B654EFA53E27A7609D9484E535AB11B7F946DF3D247CE9"
            },
            {
                "Balance": {
                    "currency": "XAU",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "0"
                },
                "Flags": 3342336,
                "HighLimit": {
                    "currency": "XAU",
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "0"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "XAU",
                    "issuer": "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
                    "value": "0"
                },
                "LowNode": "0000000000000000",
                "PreviousTxnID": "79B26D7D34B950AC2C2F91A299A6888FABB376DD76CFF79D56E805BF439F6942",
                "PreviousTxnLgrSeq": 5982530,
                "index": "9ED4406351B7A511A012A9B5E7FE4059FA2F7650621379C0013492C315E25B97"
            },
            {
                "Balance": {
                    "currency": "USD",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "0"
                },
                "Flags": 1114112,
                "HighLimit": {
                    "currency": "USD",
                    "issuer": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
                    "value": "0"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "USD",
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "5"
                },
                "LowNode": "0000000000000000",
                "PreviousTxnID": "6FE8C824364FB1195BCFEDCB368DFEE3980F7F78D3BF4DC4174BB4C86CF8C5CE",
                "PreviousTxnLgrSeq": 10555014,
                "index": "2DECFAC23B77D5AEA6116C15F5C6D4669EBAEE9E7EE050A40FE2B1E47B6A9419"
            },
            {
                "Balance": {
                    "currency": "MXN",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "481.992867407479"
                },
                "Flags": 65536,
                "HighLimit": {
                    "currency": "MXN",
                    "issuer": "rHpXfibHgSb64n8kK9QWDpdbfqSpYbM9a4",
                    "value": "0"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "MXN",
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "1000"
                },
                "LowNode": "0000000000000000",
                "PreviousTxnID": "A467BACE5F183CDE1F075F72435FE86BAD8626ED1048EDEFF7562A4CC76FD1C5",
                "PreviousTxnLgrSeq": 3316170,
                "index": "EC8B9B6B364AF6CB6393A423FDD2DDBA96375EC772E6B50A3581E53BFBDFDD9A"
            },
            {
                "Balance": {
                    "currency": "EUR",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "0.793598266778297"
                },
                "Flags": 1114112,
                "HighLimit": {
                    "currency": "EUR",
                    "issuer": "rLEsXccBGNR3UPuPu2hUXPjziKC3qKSBun",
                    "value": "0"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "EUR",
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "1"
                },
                "LowNode": "0000000000000000",
                "PreviousTxnID": "E9345D44433EA368CFE1E00D84809C8E695C87FED18859248E13662D46A0EC46",
                "PreviousTxnLgrSeq": 5447146,
                "index": "4513749B30F4AF8DA11F077C448128D6486BF12854B760E4E5808714588AA915"
            },
            {
                "Balance": {
                    "currency": "CNY",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "0"
                },
                "Flags": 2228224,
                "HighLimit": {
                    "currency": "CNY",
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "3"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "CNY",
                    "issuer": "rnuF96W4SZoCJmbHYBFoJZpR8eCaxNvekK",
                    "value": "0"
                },
                "LowNode": "0000000000000008",
                "PreviousTxnID": "2FDDC81F4394695B01A47913BEC4281AC9A283CC8F903C14ADEA970F60E57FCF",
                "PreviousTxnLgrSeq": 5949673,
                "index": "578C327DA8944BDE2E10C9BA36AFA2F43E06C8D1E8819FB225D266CBBCFDE5CE"
            },
            {
                "Balance": {
                    "currency": "DYM",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "1.336889190631542"
                },
                "Flags": 65536,
                "HighLimit": {
                    "currency": "DYM",
                    "issuer": "rGwUWgN5BEg3QGNY3RX2HfYowjUTZdid3E",
                    "value": "0"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "DYM",
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "3"
                },
                "LowNode": "0000000000000000",
                "PreviousTxnID": "6DA2BD02DFB83FA4DAFC2651860B60071156171E9C021D9E0372A61A477FFBB1",
                "PreviousTxnLgrSeq": 8818732,
                "index": "5A2A5FF12E71AEE57564E624117BBA68DEF78CD564EF6259F92A011693E027C7"
            },
            {
                "Balance": {
                    "currency": "CHF",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "-0.3488146605801446"
                },
                "Flags": 131072,
                "HighLimit": {
                    "currency": "CHF",
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "0"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "CHF",
                    "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                    "value": "0"
                },
                "LowNode": "000000000000008C",
                "PreviousTxnID": "722394372525A13D1EAAB005642F50F05A93CF63F7F472E0F91CDD6D38EB5869",
                "PreviousTxnLgrSeq": 2687590,
                "index": "F2DBAD20072527F6AD02CE7F5A450DBC72BE2ABB91741A8A3ADD30D5AD7A99FB"
            },
            {
                "Balance": {
                    "currency": "BTC",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "0"
                },
                "Flags": 131072,
                "HighLimit": {
                    "currency": "BTC",
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "3"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "BTC",
                    "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                    "value": "0"
                },
                "LowNode": "0000000000000043",
                "PreviousTxnID": "03EDF724397D2DEE70E49D512AECD619E9EA536BE6CFD48ED167AE2596055C9A",
                "PreviousTxnLgrSeq": 8317037,
                "index": "767C12AF647CDF5FEB9019B37018748A79C50EDAF87E8D4C7F39F78AA7CA9765"
            },
            {
                "Balance": {
                    "currency": "USD",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "-16.00534471983042"
                },
                "Flags": 131072,
                "HighLimit": {
                    "currency": "USD",
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "5000"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "USD",
                    "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                    "value": "0"
                },
                "LowNode": "000000000000004A",
                "PreviousTxnID": "CFFF5CFE623C9543308C6529782B6A6532207D819795AAFE85555DB8BF390FE7",
                "PreviousTxnLgrSeq": 14365854,
                "index": "826CF5BFD28F3934B518D0BDF3231259CBD3FD0946E3C3CA0C97D2C75D2D1A09"
            }
        ],
        "ledger_hash": "053DF17D2289D1C4971C22F235BC1FCA7D4B3AE966F842E5819D0749E0B8ECD3",
        "ledger_index": 14378733,
        "limit": 10,
        "marker": "F60ADF645E78B69857D2E4AEC8B7742FEABC8431BD8611D099B428C3E816DF93,94A9F05FEF9A153229E2E997E64919FD75AAE2028C8153E8EBDB4440BD3ECBB5",
        "validated": true
    }
}
```
{% endtab %}

{% tab title="JSON-RPC" %}

{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`                | 유형                                                                | 설명                                                                                                          |
| ---------------------- | ----------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| `account`              | 문자열                                                               | 이 요청이 해당하는 계정의 고유 주소입니다.                                                                                    |
| `account_objects`      | 배열                                                                | 이 계정이 소유한 오브젝트의 배열입니다. 각 개체는 원시 원장 형식입니다.                                                                   |
| `ledger_hash`          | 문자열                                                               | (생략 가능) 이 응답을 생성하는 데 사용된 원장의 식별 해시입니다.                                                                      |
| `ledger_index`         | 번호 - [원장 색인](https://xrpl.org/basic-data-types.html#ledger-index) | (생략 가능) 이 응답을 생성하는 데 사용된 원장 버전의 원장 인덱스입니다.                                                                  |
| `ledger_current_index` | 번호 - [원장 색인](https://xrpl.org/basic-data-types.html#ledger-index) | 원장 인덱스 (생략 가능) 이 응답을 생성하는 데 사용된 현재 진행 중인 원장 버전의 원장 인덱스입니다.                                                  |
| `limit`                | 숫자                                                                | (생략 가능) 이 요청에 사용된 한도(있는 경우)입니다.                                                                             |
| `marker`               | marker                                                            | 응답이 페이지 매김되었음을 나타내는 마커 서버 정의 값입니다. 이 값을 다음 호출에 전달하여 이 호출이 중단된 부분부터 다시 시작합니다. 이 페이지 이후에 추가 페이지가 없는 경우 생략됩니다. |
| `validated`            | Boolean                                                           | Boolean 값이 포함되고 참으로 설정되면 이 응답의 정보는 유효성이 검사된 원장 버전에서 가져옵니다. 그렇지 않으면 정보가 변경될 수 있습니다.                          |

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* actNotFound - 요청의 계정 필드에 지정된 주소가 장부에 있는 계정과 일치하지 않습니다.
* lgrNotFound - ledger\_hash 또는 ledger\_index로 지정한 ledger이 존재하지 않거나, 존재하지만 서버에 없습니다.
