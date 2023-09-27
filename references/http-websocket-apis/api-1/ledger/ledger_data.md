# ledger\_data

ledger\_data 메서드는 지정된 원장의 내용을 검색합니다. 여러 번의 호출을 반복하여 단일 ledger 버전의 전체 내용을 검색할 수 있습니다.

## 요청 형식

요청 형식의 예입니다:

{% hint style="info" %}
Note:

이 메소드에 대한 커맨드라인 구문은 없습니다. 대신 json 메소드를 사용하여 커맨드라인에서 이 메소드에 액세스할 수 있습니다.
{% endhint %}

{% tabs %}
{% tab title="WebSocket" %}
```json
{
   "id": 2,
   "ledger_hash": "842B57C1CC0613299A686D3E9F310EC0422C84D3911E5056389AA7E5808A93C8",
   "command": "ledger_data",
   "limit": 5,
   "binary": true
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "ledger_data",
    "params": [
        {
            "binary": true,
            "ledger_hash": "842B57C1CC0613299A686D3E9F310EC0422C84D3911E5056389AA7E5808A93C8",
            "limit": 5
        }
    ]
}
```
{% endtab %}
{% endtabs %}

요청에는 다음 필드가 포함될 수 있습니다:

| `Field`        | 유형                                                          | 필수 여부 | 설명                                                                                                                                                                                                                           |
| -------------- | ----------------------------------------------------------- | ----- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ledger_hash`  | [해시](https://xrpl.org/basic-data-types.html#hashes)         | 아니요   | 사용할 원장 버전을 식별하는 20바이트 16진수 문자열입니다.                                                                                                                                                                                           |
| `ledger_index` | [원장지수](https://xrpl.org/basic-data-types.html#ledger-index) | 아니요   | 사용할 원장의 원장 인덱스 또는 자동으로 원장을 선택하는 단축 문자열입니다. ([원장 지정](https://xrpl.org/basic-data-types.html#specifying-ledgers) 참조)                                                                                                           |
| `binary`       | Boolean                                                     | 아니요   | `true`인 경우 원장 항목을 JSON 대신 16진수 문자열로 반환합니다. 기본값은 `false`입니다.                                                                                                                                                                  |
| `limit`        | 숫자                                                          | 아니요   | 검색할 원장 항목 수를 제한합니다. 서버는 이 수보다 적은 수의 항목을 반환할 수 있습니다. 2048(바이너리 요청 시) 또는 256(JSON 요청 시)을 초과할 수 없습니다. 이 범위를 벗어나는 양수 값은 가장 가까운 유효한 옵션으로 대체됩니다. 기본값은 최대값입니다.                                                                      |
| `marker`       | marker                                                      | 아니요   | 이전에 페이지를 매긴 응답의 값입니다. 해당 응답이 중단된 데이터 검색을 재개합니다.                                                                                                                                                                              |
| `type`         | Boolean                                                     | 아니요   | 특정 유형의 원장 항목으로 결과를 필터링합니다. 유효한 유형은 , `account`, `amendments`, `amm` , `check`, `deposit_preauth`, `directory`, `escrow`, `fee`, `hashes`, `nft_offer`, `offer`, `payment_channel`, `signer_list`, `state` (신뢰선), 과 `ticket`. |

ledger 필드는 더 이상 사용되지 않으며 별도의 통지 없이 제거될 수 있습니다.

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket (binary:true)" %}
```json
{
    "id": 2,
    "result": {
        "ledger_hash": "842B57C1CC0613299A686D3E9F310EC0422C84D3911E5056389AA7E5808A93C8",
        "ledger_index": "6885842",
        "marker": "0002A590029B53BE7857EFF9985F770EC792CE483720EB5E963C4D6A607D43DF",
        "state": [
            {
                "data": "11006122000000002400000001250062FEA42D0000000055C204A65CF2542946289A3358C67D991B5E135FABFA89F271DBA7A150C08CA0466240000000354540208114C909F42250CFE8F12A7A1A0DFBD3CBD20F32CD79",
                "index": "00001A2969BE1FC85F1D7A55282FA2E6D95C71D2E4B9C0FDD3D9994F3C00FF8F"
            },
            {
                "data": "11006F22000000002400000003250035788533000000000000000034000000000000000055555B93628BF3EC318892BB7C7CDCB6732FF53D12B6EEC4FAF60DD1AEE1C6101F501071633D7DE1B6AEB32F87F1A73258B13FC8CC32942D53A66D4F038D7EA4C6800064D4838D7EA4C68000000000000000000000000000425443000000000035DD7DF146893456296BF4061FBE68735D28F3286540000000000F42408114A4B8F5F7B644AEDC3447F9459C132EEB016A133B",
                "index": "000037C6659BB98F8D09F2F4CFEB27DE8EFEAFE54DD9E1C13AECDF5794B0C0F5"
            },
            {
                "data": "11006F2200020000240000000A250067395C33000000000000000034000000000000000055A160BC41A45B6BB118DF23D77E4FF23C723431B917F50DCB41319ECC2821F34C5010DFA3B6DDAB58C7E8E5D944E736DA4B7046C30E4F460FD9DE4C1AA535D3D0C00064D554C88B43EFA00000000000000000000000000055534400000000000A20B3C85F482532A9578DBB3950B85CA06594D165400000B59B9F780081148366FB9ACD2A0FD822E31112D2EB6F98C317C2C1",
                "index": "0000A8791F78CC9B39200E12A9BDAACCF40A72A512FA815525CFC9BA772990F7"
            },
            {
                "data": "1100612200000000240000000125003E742F2D0000000055286498B513710CFEB2D723A554C7557983D1952DF4DEE342C40DCB43067C9A21624000000306DC42008114225BAB89C4A4B94624BB069D6DB3C819F934991C",
                "index": "0000B717320558E2DE1A3B9FDB24E9A695BF05D1A44E4A4683212BB1DD0FBA23"
            },
            {
                "data": "110072220002000025000B65783700000000000000003800000000000000005587591A63051645F37B85D1FBA55EE69B1C96BFF16904F5C99F03FB93D42D03756280000000000000000000000000000000000000004254430000000000000000000000000000000000000000000000000166800000000000000000000000000000000000000042544300000000000A20B3C85F482532A9578DBB3950B85CA06594D167D4C38D7EA4C680000000000000000000000000004254430000000000C795FDF8A637BCAAEDAD1C434033506236C82A2D",
                "index": "000103996A3BAD918657F86E12A67D693E8FC8A814DA4B958A244B5F14D93E58"
            }
        ]
    },
    "status": "success",
    "type": "response"
}
```
{% endtab %}

{% tab title="WebSocket (binary:false)" %}
```json
{
    "id": 2,
    "result": {
        "ledger_hash": "842B57C1CC0613299A686D3E9F310EC0422C84D3911E5056389AA7E5808A93C8",
        "ledger_index": "6885842",
        "marker": "0002A590029B53BE7857EFF9985F770EC792CE483720EB5E963C4D6A607D43DF",
        "state": [
            {
                "Account": "rKKzk9ghA2iuy3imqMXUHJqdRPMtNDGf4c",
                "Balance": "893730848",
                "Flags": 0,
                "LedgerEntryType": "AccountRoot",
                "OwnerCount": 0,
                "PreviousTxnID": "C204A65CF2542946289A3358C67D991B5E135FABFA89F271DBA7A150C08CA046",
                "PreviousTxnLgrSeq": 6487716,
                "Sequence": 1,
                "index": "00001A2969BE1FC85F1D7A55282FA2E6D95C71D2E4B9C0FDD3D9994F3C00FF8F"
            },
            {
                "Account": "rGryPmNWFognBgMtr9k4quqPbbEcCrhNmD",
                "BookDirectory": "71633D7DE1B6AEB32F87F1A73258B13FC8CC32942D53A66D4F038D7EA4C68000",
                "BookNode": "0000000000000000",
                "Flags": 0,
                "LedgerEntryType": "Offer",
                "OwnerNode": "0000000000000000",
                "PreviousTxnID": "555B93628BF3EC318892BB7C7CDCB6732FF53D12B6EEC4FAF60DD1AEE1C6101F",
                "PreviousTxnLgrSeq": 3504261,
                "Sequence": 3,
                "TakerGets": "1000000",
                "TakerPays": {
                    "currency": "BTC",
                    "issuer": "rnuF96W4SZoCJmbHYBFoJZpR8eCaxNvekK",
                    "value": "1"
                },
                "index": "000037C6659BB98F8D09F2F4CFEB27DE8EFEAFE54DD9E1C13AECDF5794B0C0F5"
            },
            {
                "Account": "rUy8tW38MW9ma7kSjRgB2GHtTkQAFRyrN8",
                "BookDirectory": "DFA3B6DDAB58C7E8E5D944E736DA4B7046C30E4F460FD9DE4C1AA535D3D0C000",
                "BookNode": "0000000000000000",
                "Flags": 131072,
                "LedgerEntryType": "Offer",
                "OwnerNode": "0000000000000000",
                "PreviousTxnID": "A160BC41A45B6BB118DF23D77E4FF23C723431B917F50DCB41319ECC2821F34C",
                "PreviousTxnLgrSeq": 6764892,
                "Sequence": 10,
                "TakerGets": "780000000000",
                "TakerPays": {
                    "currency": "USD",
                    "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                    "value": "5850"
                },
                "index": "0000A8791F78CC9B39200E12A9BDAACCF40A72A512FA815525CFC9BA772990F7"
            },
            {
                "Account": "rh3C81VfNDhhWPQWCU8ZGgknvdgNUvRtM9",
                "Balance": "13000000000",
                "Flags": 0,
                "LedgerEntryType": "AccountRoot",
                "OwnerCount": 0,
                "PreviousTxnID": "286498B513710CFEB2D723A554C7557983D1952DF4DEE342C40DCB43067C9A21",
                "PreviousTxnLgrSeq": 4092975,
                "Sequence": 1,
                "index": "0000B717320558E2DE1A3B9FDB24E9A695BF05D1A44E4A4683212BB1DD0FBA23"
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
                    "issuer": "rKUK9omZqVEnraCipKNFb5q4tuNTeqEDZS",
                    "value": "10"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "BTC",
                    "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                    "value": "0"
                },
                "LowNode": "0000000000000000",
                "PreviousTxnID": "87591A63051645F37B85D1FBA55EE69B1C96BFF16904F5C99F03FB93D42D0375",
                "PreviousTxnLgrSeq": 746872,
                "index": "000103996A3BAD918657F86E12A67D693E8FC8A814DA4B958A244B5F14D93E58"
            }
        ]
    },
    "status": "success",
    "type": "response"
}
```
{% endtab %}

{% tab title="JSON-RPC (binary:true)" %}
```json
200 OK

{
    "result": {
        "ledger_hash": "842B57C1CC0613299A686D3E9F310EC0422C84D3911E5056389AA7E5808A93C8",
        "ledger_index": "6885842",
        "marker": "0002A590029B53BE7857EFF9985F770EC792CE483720EB5E963C4D6A607D43DF",
        "state": [
            {
                "data": "11006122000000002400000001250062FEA42D0000000055C204A65CF2542946289A3358C67D991B5E135FABFA89F271DBA7A150C08CA0466240000000354540208114C909F42250CFE8F12A7A1A0DFBD3CBD20F32CD79",
                "index": "00001A2969BE1FC85F1D7A55282FA2E6D95C71D2E4B9C0FDD3D9994F3C00FF8F"
            },
            {
                "data": "11006F22000000002400000003250035788533000000000000000034000000000000000055555B93628BF3EC318892BB7C7CDCB6732FF53D12B6EEC4FAF60DD1AEE1C6101F501071633D7DE1B6AEB32F87F1A73258B13FC8CC32942D53A66D4F038D7EA4C6800064D4838D7EA4C68000000000000000000000000000425443000000000035DD7DF146893456296BF4061FBE68735D28F3286540000000000F42408114A4B8F5F7B644AEDC3447F9459C132EEB016A133B",
                "index": "000037C6659BB98F8D09F2F4CFEB27DE8EFEAFE54DD9E1C13AECDF5794B0C0F5"
            },
            {
                "data": "11006F2200020000240000000A250067395C33000000000000000034000000000000000055A160BC41A45B6BB118DF23D77E4FF23C723431B917F50DCB41319ECC2821F34C5010DFA3B6DDAB58C7E8E5D944E736DA4B7046C30E4F460FD9DE4C1AA535D3D0C00064D554C88B43EFA00000000000000000000000000055534400000000000A20B3C85F482532A9578DBB3950B85CA06594D165400000B59B9F780081148366FB9ACD2A0FD822E31112D2EB6F98C317C2C1",
                "index": "0000A8791F78CC9B39200E12A9BDAACCF40A72A512FA815525CFC9BA772990F7"
            },
            {
                "data": "1100612200000000240000000125003E742F2D0000000055286498B513710CFEB2D723A554C7557983D1952DF4DEE342C40DCB43067C9A21624000000306DC42008114225BAB89C4A4B94624BB069D6DB3C819F934991C",
                "index": "0000B717320558E2DE1A3B9FDB24E9A695BF05D1A44E4A4683212BB1DD0FBA23"
            },
            {
                "data": "110072220002000025000B65783700000000000000003800000000000000005587591A63051645F37B85D1FBA55EE69B1C96BFF16904F5C99F03FB93D42D03756280000000000000000000000000000000000000004254430000000000000000000000000000000000000000000000000166800000000000000000000000000000000000000042544300000000000A20B3C85F482532A9578DBB3950B85CA06594D167D4C38D7EA4C680000000000000000000000000004254430000000000C795FDF8A637BCAAEDAD1C434033506236C82A2D",
                "index": "000103996A3BAD918657F86E12A67D693E8FC8A814DA4B958A244B5F14D93E58"
            }
        ],
        "status": "success"
    }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`        | 유형                                                                       | 설명                                                                   |
| -------------- | ------------------------------------------------------------------------ | -------------------------------------------------------------------- |
| `ledger_index` | 부호 없는 정수 - [원장 인덱스](https://xrpl.org/basic-data-types.html#ledger-index) | 이 원장 버전의 원장 인덱스입니다.                                                  |
| `ledger_hash`  | 문자열 - [해시](https://xrpl.org/basic-data-types.html#hashes)                | 이 원장 버전의 고유 식별 해시입니다.                                                |
| `state`        | 배열                                                                       | 아래 정의된 대로 원장 상태 트리의 데이터를 포함하는 JSON 객체 배열입니다.                         |
| `marker`       | marker                                                                   | 응답이 페이지로 매겨졌음을 나타내는 서버 정의 값입니다. 이 통화가 중단된 곳에서 재개하려면 이를 다음 화폐에 전달하세요. |

요청에 유형 필드가 언급된 경우, 배열 객체의 첫 번째 집합이 요청된 유형과 일치하지 않으면 상태 배열이 비어 있습니다. 이러한 경우 이 응답의 마커를 사용하여 페이지네이션을 하고 추가 데이터를 검색할 수 있습니다.

상태 배열의 각 객체 형식은 요청에서 바이너리가 참으로 설정되었는지 여부에 따라 달라집니다. 각 상태 객체에는 다음 필드가 포함될 수 있습니다:

| `Field`           | 유형   | 설명                                                                                                                                    |
| ----------------- | ---- | ------------------------------------------------------------------------------------------------------------------------------------- |
| `data문`           | 문자열  | _(`"binary":true`인 경우에만 포함됨)_ 요청된 데이터의 16진수 표현                                                                                        |
| `LedgerEntryType` | 문자열  | _(`"binary":true`인 경우에만 포함됨)_ 이 객체가 나타내는 원장 객체의 유형을 나타내는 문자열입니다. 전체 목록은 [원장 개체 유형을](https://xrpl.org/ledger-object-types.html) 참조하세요. |
| (추가 필드)           | (변수) | _(`"binary":true`인 경우에만 포함됨)_ [원장 개체 유형](https://xrpl.org/ledger-object-types.html) 에 따라 이 개체를 설명하는 추가 필드입니다.                         |
| `index`           | 문자열  | 이 원장 항목의 고유 식별자(16진수)입니다.                                                                                                             |

## 발생 가능한 오류

* 모든 범용 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* lgrNotFound - ledger\_hash 또는 ledger\_index에 지정된 ledger이 존재하지 않거나, 존재하지만 서버에 없습니다.
