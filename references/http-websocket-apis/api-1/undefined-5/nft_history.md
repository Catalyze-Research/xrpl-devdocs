# nft\_history

배열문자nft\_history 명령은 조회 중인 NFT의 과거 트랜잭션 메타데이터를 클리오 서버에 요청합니다. [![New in: Clio v1.1.0](https://img.shields.io/badge/New%20in-Clio%20v1.1.0-blue.svg)](https://github.com/XRPLF/clio/releases/tag/1.1.0)

{% hint style="info" %}
Note:

nft\_history는 NFT와 관련된 성공적인 트랜잭션만 반환합니다.
{% endhint %}

## 요청 형식

요청 형식의 예시입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 1,
  "command": "nft_history",
  "nft_id": "00080000B4F4AFC5FBCBD76873F18006173D2193467D3EE70000099B00000000"
}
```
{% endtab %}

{% tab title="Second Tab" %}
```json
{
    "method": "nft_history",
    "params": [
      {
          "nft_id": "00080000B4F4AFC5FBCBD76873F18006173D2193467D3EE70000099B00000000"
      }
    ]
}
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| Field              | 유형              | 설명                                                                                                                                                                                                                                                                                                        |
| ------------------ | --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| nft\_id            | 문자열             | 대체 불가능한 토큰(NFT)의 고유 식별자입니다.                                                                                                                                                                                                                                                                               |
| ledger\_index\_min | 정수              | _(선택 사항)_ NFT를 포함할 최초 원장을 지정하는 데 사용됩니다. 값은 -1서버가 사용 가능한 가장 먼저 검증된 원장 버전을 사용하도록 지시합니다.                                                                                                                                                                                                                     |
| ledger\_index\_max | 정수              | _(선택 사항)_ NFT를 포함할 가장 최근 원장을 지정하는 데 사용됩니다. 값은 -1서버가 사용 가능한 가장 최근에 검증된 원장 버전을 사용하도록 지시합니다.                                                                                                                                                                                                                 |
| ledger\_hash       | 문자열             | _(선택 사항)_ 사용할 원장 버전에 대한 20바이트 16진수 문자열입니다. ([원장 지정](https://xrpl.org/basic-data-types.html#specifying-ledgers) 참조)                                                                                                                                                                                        |
| ledger\_index      | 문자열 또는 부호 없는 정수 | _(선택 사항)_ 사용할 원장의 [원장 인덱스](https://xrpl.org/basic-data-types.html#ledger-index) 또는 자동으로 원장을 선택하는 단축 문자열입니다. ledger\_indexas closed또는 를 지정하지 마십시오 current. 그렇게 하면 요청이 P2P rippled서버 로 전달되며 nft\_historyAPI는 에서 사용할 수 없습니다 rippled. ([원장 지정](https://xrpl.org/basic-data-types.html#specifying-ledgers) 참조) |
| binary             | Boolean         | (선택 사항) 기본값은 입니다 false로 설정된 경우 trueJSON 대신 16진수 문자열로 트랜잭션을 반환합니다.                                                                                                                                                                                                                                         |
| forward            | Boolean         | (선택 사항) 기본값은 입니다 false. 로 설정된 경우 true가장 오래된 원장으로 인덱싱된 값을 먼저 반환합니다. 그렇지 않으면 결과가 최신 원장으로 먼저 인덱싱됩니다. (결과의 각 페이지는 내부적으로 배열되지 않을 수 있지만 페이지는 전체적으로 배열됩니다.)                                                                                                                                                      |
| limit              | UInt32          | (선택 사항) 검색할 NFT 수를 제한합니다. 서버는 이 값을 준수할 필요가 없습니다.                                                                                                                                                                                                                                                          |
| marker             | marker          | 이전에 페이지를 매긴 응답의 값입니다. 해당 응답이 중단된 데이터 검색을 재개합니다. 서버의 사용 가능한 원장 범위가 변경되면 이 값은 안정적이지 않습니다. "검증된" 원장을 쿼리하는 경우 페이징 중에 새로운 NFT가 생성될 수 있습니다.                                                                                                                                                                     |

{% hint style="info" %}
Note:

ledger 버전을 지정하지 않으면 클리오는 검증된 최신 ledger을 사용합니다.
{% endhint %}

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 0,
  "type": "response",
  "result": {
    "ledger_index_min": 21377274,
    "ledger_index_max": 27876163,
    "transactions": [
      {
        "meta": {
          "AffectedNodes": [
            {
              "CreatedNode": {
                "LedgerEntryType": "NFTokenPage",
                "LedgerIndex": "97707A94B298B50334C39FB46E245D4744C0F5B5FFFFFFFFFFFFFFFFFFFFFFFF",
                "NewFields": {
                  "NFTokens": [
                    {
                      "NFToken": {
                        "NFTokenID": "0008271097707A94B298B50334C39FB46E245D4744C0F5B50000099B00000000",
                        "URI": "697066733A2F2F62616679626569676479727A74357366703775646D37687537367568377932366E6634646675796C71616266336F636C67747179353566627A6469"
                      }
                    }
                  ]
                }
              }
            },
            {
              "ModifiedNode": {
                "FinalFields": {
                  "Account": "rNoj836fhDm1eXaHHefPKs7iDb4gwzS7nc",
                  "Balance": "999999988",
                  "Flags": 0,
                  "MintedNFTokens": 1,
                  "OwnerCount": 1,
                  "Sequence": 27876155
                },
                "LedgerEntryType": "AccountRoot",
                "LedgerIndex": "AC0A2AD29B67B5E6DA1C5DE696440F59BCD8DEA0A4CF7AFD683D1489AAB1ED24",
                "PreviousFields": {
                  "Balance": "1000000000",
                  "OwnerCount": 0,
                  "Sequence": 27876154
                },
                "PreviousTxnID": "B483F0F7100658380E42BCF1B15AD59B71C4082635AD53B78D08A5198BBB6939",
                "PreviousTxnLgrSeq": 27876154
              }
            }
          ],
          "TransactionIndex": 0,
          "TransactionResult": "tesSUCCESS"
        },
        "tx": {
          "Account": "rNoj836fhDm1eXaHHefPKs7iDb4gwzS7nc",
          "Fee": "12",
          "Flags": 8,
          "LastLedgerSequence": 27876176,
          "NFTokenTaxon": 0,
          "Sequence": 27876154,
          "SigningPubKey": "EDDC20C6791F9FB13AFDCE2C717BE8779DD451BB556243F1FDBAA3CD159D68A9F6",
          "TransactionType": "NFTokenMint",
          "TransferFee": 10000,
          "TxnSignature": "EF657AB47E86FDC112BA054D90587DFE64A61604D9EDABAA7B01B61B56433E3C2AC5BF5AD2E8F5D2A9EAC22778F289094AC383A3F172B2304157A533E0C79802",
          "URI": "697066733A2F2F62616679626569676479727A74357366703775646D37687537367568377932366E6634646675796C71616266336F636C67747179353566627A6469",
          "hash": "E0774E1B8628E397C6E88F67D4424E55E4C81324607B19318255310A6FBAA4A2",
          "ledger_index": 27876158,
          "date": 735167200
        },
        "validated": true
      }
    ],
    "nft_id": "0008271097707A94B298B50334C39FB46E245D4744C0F5B50000099B00000000",
    "validated": true
  },
  "warnings": [
    {
      "id": 2001,
      "message": "This is a clio server. clio only serves validated data. If you want to talk to rippled, include 'ledger_index':'current' in your request"
    }
  ]
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
  "result": {
    "ledger_index_min": 21377274,
    "ledger_index_max": 27876163,
    "transactions": [
      {
        "meta": {
          "AffectedNodes": [
            {
              "CreatedNode": {
                "LedgerEntryType": "NFTokenPage",
                "LedgerIndex": "97707A94B298B50334C39FB46E245D4744C0F5B5FFFFFFFFFFFFFFFFFFFFFFFF",
                "NewFields": {
                  "NFTokens": [
                    {
                      "NFToken": {
                        "NFTokenID": "0008271097707A94B298B50334C39FB46E245D4744C0F5B50000099B00000000",
                        "URI": "697066733A2F2F62616679626569676479727A74357366703775646D37687537367568377932366E6634646675796C71616266336F636C67747179353566627A6469"
                      }
                    }
                  ]
                }
              }
            },
            {
              "ModifiedNode": {
                "FinalFields": {
                  "Account": "rNoj836fhDm1eXaHHefPKs7iDb4gwzS7nc",
                  "Balance": "999999988",
                  "Flags": 0,
                  "MintedNFTokens": 1,
                  "OwnerCount": 1,
                  "Sequence": 27876155
                },
                "LedgerEntryType": "AccountRoot",
                "LedgerIndex": "AC0A2AD29B67B5E6DA1C5DE696440F59BCD8DEA0A4CF7AFD683D1489AAB1ED24",
                "PreviousFields": {
                  "Balance": "1000000000",
                  "OwnerCount": 0,
                  "Sequence": 27876154
                },
                "PreviousTxnID": "B483F0F7100658380E42BCF1B15AD59B71C4082635AD53B78D08A5198BBB6939",
                "PreviousTxnLgrSeq": 27876154
              }
            }
          ],
          "TransactionIndex": 0,
          "TransactionResult": "tesSUCCESS"
        },
        "tx": {
          "Account": "rNoj836fhDm1eXaHHefPKs7iDb4gwzS7nc",
          "Fee": "12",
          "Flags": 8,
          "LastLedgerSequence": 27876176,
          "NFTokenTaxon": 0,
          "Sequence": 27876154,
          "SigningPubKey": "EDDC20C6791F9FB13AFDCE2C717BE8779DD451BB556243F1FDBAA3CD159D68A9F6",
          "TransactionType": "NFTokenMint",
          "TransferFee": 10000,
          "TxnSignature": "EF657AB47E86FDC112BA054D90587DFE64A61604D9EDABAA7B01B61B56433E3C2AC5BF5AD2E8F5D2A9EAC22778F289094AC383A3F172B2304157A533E0C79802",
          "URI": "697066733A2F2F62616679626569676479727A74357366703775646D37687537367568377932366E6634646675796C71616266336F636C67747179353566627A6469",
          "hash": "E0774E1B8628E397C6E88F67D4424E55E4C81324607B19318255310A6FBAA4A2",
          "ledger_index": 27876158,
          "date": 735167200
        },
        "validated": true
      }
    ],
    "nft_id": "0008271097707A94B298B50334C39FB46E245D4744C0F5B50000099B00000000",
    "validated": true
  },
  "warnings": [
    {
      "id": 2001,
      "message": "This is a clio server. clio only serves validated data. If you want to talk to rippled, include 'ledger_index':'current' in your request"
    }
  ]
}
```
{% endtab %}
{% endtabs %}

바이너리 매개변수를 true로 설정하면 16진수 문자열을 사용하는 간결한 응답을 받습니다. 사람이 읽을 수는 없지만 훨씬 더 간결합니다.

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 0,
  "type": "response",
  "result": {
    "ledger_index_min": 21377274,
    "ledger_index_max": 27876275,
    "transactions": [
      {
        "meta": "201C00000000F8E31100505697707A94B298B50334C39FB46E245D4744C0F5B5FFFFFFFFFFFFFFFFFFFFFFFFE8FAEC5A0008271097707A94B298B50334C39FB46E245D4744C0F5B50000099B000000007542697066733A2F2F62616679626569676479727A74357366703775646D37687537367568377932366E6634646675796C71616266336F636C67747179353566627A6469E1F1E1E1E51100612501A95B3A55B483F0F7100658380E42BCF1B15AD59B71C4082635AD53B78D08A5198BBB693956AC0A2AD29B67B5E6DA1C5DE696440F59BCD8DEA0A4CF7AFD683D1489AAB1ED24E62401A95B3A2D0000000062400000003B9ACA00E1E722000000002401A95B3B2D00000001202B0000000162400000003B9AC9F4811497707A94B298B50334C39FB46E245D4744C0F5B5E1E1F1031000",
        "tx_blob": "12001914271022000000082401A95B3A201B01A95B50202A0000000068400000000000000C7321EDDC20C6791F9FB13AFDCE2C717BE8779DD451BB556243F1FDBAA3CD159D68A9F67440EF657AB47E86FDC112BA054D90587DFE64A61604D9EDABAA7B01B61B56433E3C2AC5BF5AD2E8F5D2A9EAC22778F289094AC383A3F172B2304157A533E0C798027542697066733A2F2F62616679626569676479727A74357366703775646D37687537367568377932366E6634646675796C71616266336F636C67747179353566627A6469811497707A94B298B50334C39FB46E245D4744C0F5B5",
        "ledger_index": 27876158,
        "date": 735167200,
        "validated": true
      }
    ],
    "nft_id": "0008271097707A94B298B50334C39FB46E245D4744C0F5B50000099B00000000",
    "validated": true
  },
  "warnings": [
    {
      "id": 2001,
      "message": "This is a clio server. clio only serves validated data. If you want to talk to rippled, include 'ledger_index':'current' in your request"
    }
  ]
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
  "result": {
    "ledger_index_min": 21377274,
    "ledger_index_max": 27876275,
    "transactions": [
      {
        "meta": "201C00000000F8E31100505697707A94B298B50334C39FB46E245D4744C0F5B5FFFFFFFFFFFFFFFFFFFFFFFFE8FAEC5A0008271097707A94B298B50334C39FB46E245D4744C0F5B50000099B000000007542697066733A2F2F62616679626569676479727A74357366703775646D37687537367568377932366E6634646675796C71616266336F636C67747179353566627A6469E1F1E1E1E51100612501A95B3A55B483F0F7100658380E42BCF1B15AD59B71C4082635AD53B78D08A5198BBB693956AC0A2AD29B67B5E6DA1C5DE696440F59BCD8DEA0A4CF7AFD683D1489AAB1ED24E62401A95B3A2D0000000062400000003B9ACA00E1E722000000002401A95B3B2D00000001202B0000000162400000003B9AC9F4811497707A94B298B50334C39FB46E245D4744C0F5B5E1E1F1031000",
        "tx_blob": "12001914271022000000082401A95B3A201B01A95B50202A0000000068400000000000000C7321EDDC20C6791F9FB13AFDCE2C717BE8779DD451BB556243F1FDBAA3CD159D68A9F67440EF657AB47E86FDC112BA054D90587DFE64A61604D9EDABAA7B01B61B56433E3C2AC5BF5AD2E8F5D2A9EAC22778F289094AC383A3F172B2304157A533E0C798027542697066733A2F2F62616679626569676479727A74357366703775646D37687537367568377932366E6634646675796C71616266336F636C67747179353566627A6469811497707A94B298B50334C39FB46E245D4744C0F5B5",
        "ledger_index": 27876158,
        "date": 735167200,
        "validated": true
      }
    ],
    "nft_id": "0008271097707A94B298B50334C39FB46E245D4744C0F5B50000099B00000000",
    "validated": true
  },
  "warnings": [
    {
      "id": 2001,
      "message": "This is a clio server. clio only serves validated data. If you want to talk to rippled, include 'ledger_index':'current' in your request"
    }
  ]
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| Field              | 유형                                                                | 설명                                                                      |
| ------------------ | ----------------------------------------------------------------- | ----------------------------------------------------------------------- |
| nft\_id            | 문자열                                                               | 대체 불가능한 토큰(NFT)의 고유 식별자입니다.                                             |
| ledger\_index\_min | 정수 - [원장 색인](https://xrpl.org/basic-data-types.html#ledger-index) | 최초 원장의 원장 색인은 실제로 거래를 검색한 것입니다.                                         |
| ledger\_index\_max | 정수 - [원장 색인](https://xrpl.org/basic-data-types.html#ledger-index) | 가장 최근 원장의 원장 색인이 실제로 거래를 검색한 것입니다.                                      |
| limit              | 정수                                                                | limit요청에 사용된 값입니다 . (실제 서버에서 적용하는 제한값과 다를 수 있습니다.)                      |
| marker             | [marker](https://xrpl.org/markers-and-pagination.html)            | 응답이 페이지로 매겨졌음을 나타내는 서버 정의 값입니다. 이 통화가 중단된 곳에서 재개하려면 이를 다음 통화에 전달하세요.    |
| transactions       | 배열                                                                | 아래 설명된 대로 요청 기준과 일치하는 트랜잭션 배열입니다.                                       |
| validated          | Boolean                                                           | 포함되고 true로 설정된 경우, 이 응답의 정보는 검증된 원장 버전에서 가져옵니다. 그렇지 않으면 정보가 변경될 수 있습니다. |

{% hint style="info" %}
Note:

서버에 사용자가 지정한 버전이 없는 경우와 같이 서버가 요청에 제공한 것과 다른 ledger\_index\_min 및 ledger\_index\_max 값으로 응답할 수 있습니다.
{% endhint %}

각 트랜잭션 객체에는 JSON 또는 16진수 문자열 "binary":true 형식으로 요청되었는지 여부에 따라 다음과 같은 필드가 포함됩니다.

| Field         | 유형                    | 설명                                                                                 |
| ------------- | --------------------- | ---------------------------------------------------------------------------------- |
| ledger\_index | 정수                    | 이 거래가 포함된 원장 버전의 [원장 인덱스입니다](https://xrpl.org/basic-data-types.html#ledger-index). |
| meta          | 객체(JSON) 또는 문자열(바이너리) | True인 경우 binary이는 트랜잭션 메타데이터의 16진수 문자열입니다. 그렇지 않으면 트랜잭션 메타데이터가 JSON 형식으로 포함됩니다.    |
| tx            | 객체                    | (JSON 모드만 해당) 트랜잭션을 정의하는 JSON 객체                                                   |
| tx\_blob      | 문자열                   | (바이너리 모드에만 해당) 트랜잭션을 나타내는 고유한 해시 문자열입니다.                                           |
| validated     | Boolean               | 거래가 검증된 원장에 포함되어 있는지 여부입니다. 아직 검증된 원장에 포함되지 않은 거래는 변경될 수 있습니다.                     |

tx 객체에 반환되는 필드에 대한 정의는 트랜잭션 메타데이터를 참조하세요.

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* actMalformed - 요청의 계정 필드에 지정된 주소의 형식이 올바르지 않습니다.
* lgrIdxMalformed - ledger\_index\_min 또는 ledger\_index\_max에 지정된 ledger이 존재하지 않거나, 존재하지만 서버에 없는 경우입니다.
* lgrIdxsInvalid - 요청이 ledger\_index\_min보다 앞선 ledger\_index\_max를 지정하거나 서버가 네트워크와 동기화되지 않아 유효성이 검증된 ledger 범위를 가지고 있지 않은 경우입니다.
