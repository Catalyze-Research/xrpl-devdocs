# account\_tx

account\_tx 메소드는 지정된 계정과 관련된 트랜잭션 목록을 검색합니다.

## 요청 형식

요청 형식의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 2,
  "command": "account_tx",
  "account": "rLNaPoKeeBjZe2qs6x52yVPZpZ8td4dc6w",
  "ledger_index_min": -1,
  "ledger_index_max": -1,
  "binary": false,
  "limit": 2,
  "forward": false
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "account_tx",
    "params": [
        {
            "account": "rLNaPoKeeBjZe2qs6x52yVPZpZ8td4dc6w",
            "binary": false,
            "forward": false,
            "ledger_index_max": -1,
            "ledger_index_min": -1,
            "limit": 2
        }
    ]
}
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| `Field`            | 유형              | 설명                                                                                                                                           |
| ------------------ | --------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| `account`          | 문자열             | 계정의 고유 식별자(가장 일반적으로 계정의 주소)입니다.                                                                                                              |
| `ledger_index_min` | 정수              | (선택 사항) 트랜잭션을 포함할 가장 최근의 원장을 지정하는 데 사용합니다. 값이 -1이면 서버가 사용 가능한 가장 오래된 유효성 검사된 원장 버전을 사용하도록 지시합니다.                                             |
| `ledger_index_max` | 정수              | (선택 사항) 트랜잭션을 포함할 가장 최근의 원장을 지정하는 데 사용합니다. 값이 -1이면 서버가 사용 가능한 가장 최근의 유효성이 검증된 원장 버전을 사용하도록 지시합니다.                                            |
| `ledger_hash`      | 문자열             | (선택 사항) 단일 원장에서만 트랜잭션을 찾을 때 사용합니다. (원장 지정하기 참조).                                                                                             |
| `ledger_index`     | 문자열 또는 부호 없는 정수 | _(선택 사항)_ 단일 원장에서만 거래를 찾는 데 사용합니다. ( [원장 지정을](https://xrpl.org/basic-data-types.html#specifying-ledgers) 참조하십시오 .)                           |
| `binary`           | Boolean         | (선택 사항) 기본값은 거짓입니다. true로 설정하면 트랜잭션을 JSON 대신 16진수 문자열로 반환합니다.                                                                                |
| `forward`          | Boolean         | (선택 사항) 기본값은 거짓입니다. true로 설정하면 가장 오래된 원장으로 인덱싱된 값을 먼저 반환합니다. 그렇지 않으면 최신 원장부터 인덱싱된 결과를 반환합니다. (결과의 각 페이지는 내부적으로 정렬되지 않을 수 있지만 전체 페이지는 정렬됩니다.) |
| `limit`            | 정수              | (선택 사항) 기본값은 다양합니다. 검색할 트랜잭션 수를 제한합니다. 서버가 이 값을 준수할 필요는 없습니다.                                                                                |
| `marker`           | marker          | 이전 페이지 매김된 응답의 마커 값입니다. 해당 응답이 중단된 부분부터 데이터 검색을 재개합니다. 이 값은 서버의 사용 가능한 원장 범위가 변경되더라도 안정적입니다.                                                 |

요청에 `ledger_index`, `ledger_hash`, `ledger_index_min`, or `ledger_index_max` 필드 중 하나 이상을 사용해야 합니다.

다음 레거시 필드는 더 이상 지원되지 않습니다: `offset`, `count`, `ledger_min`, `ledger_max`. [![Removed in: rippled 1.7.0](https://img.shields.io/badge/Removed%20in-rippled%201.7.0-red.svg)](https://github.com/ripple/rippled/releases/tag/1.7.0)

## 쿼리된 데이터에 대한 이터레이션

다른 페이지네이션 메소드와 마찬가지로 마커 필드를 사용하여 여러 페이지의 데이터를 반환할 수 있습니다.

요청 사이의 시간에서 "ledger\_index\_min": -1 및 "ledger\_index\_max": -1은 이전과 다른 ledger 버전을 참조하도록 변경될 수 있습니다. 마커 필드는 마커가 요청에 지정된 ledger 범위를 벗어난 지점을 나타내지 않는 한, 요청에서 ledger 범위가 변경되더라도 안전하게 페이지네이션할 수 있습니다.

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 2,
  "result": {
    "account": "rLNaPoKeeBjZe2qs6x52yVPZpZ8td4dc6w",
    "ledger_index_max": 57111999,
    "ledger_index_min": 55886305,
    "limit": 2,
    "marker": {
      "ledger": 57111981,
      "seq": 16
    },
    "transactions": [
      {
        "meta": {
          "AffectedNodes": [
            {
              "ModifiedNode": {
                "FinalFields": {
                  "Account": "rLNaPoKeeBjZe2qs6x52yVPZpZ8td4dc6w",
                  "Balance": "3732969177079",
                  "Flags": 131072,
                  "OwnerCount": 0,
                  "Sequence": 702817
                },
                "LedgerEntryType": "AccountRoot",
                "LedgerIndex": "140FA03FE8C39540CA8189BC7A7956795C712BC0A542C6409C041150703C8574",
                "PreviousFields": {
                  "Balance": "3713891690008"
                },
                "PreviousTxnID": "D58864C16344ADCC15995C7986CFC607CB693E88F84D2E019F0A35FB29749202",
                "PreviousTxnLgrSeq": 57111994
              }
            },
            {
              "ModifiedNode": {
                "FinalFields": {
                  "Account": "rw2ciyaNshpHe7bCHo4bRWq6pqqynnWKQg",
                  "Balance": "40010160",
                  "Flags": 131072,
                  "OwnerCount": 0,
                  "Sequence": 466334
                },
                "LedgerEntryType": "AccountRoot",
                "LedgerIndex": "CC20FEBEA6D2AF969EC46F2BD92684D9FBABC3F238E841B5E056FE4EBF4379A9",
                "PreviousFields": {
                  "Balance": "19117497271",
                  "Sequence": 466333
                },
                "PreviousTxnID": "F6B8274D3D419A95A59681E5F55578084C395FF9051924360CA3EA745F5581E8",
                "PreviousTxnLgrSeq": 57111993
              }
            }
          ],
          "TransactionIndex": 25,
          "TransactionResult": "tesSUCCESS",
          "delivered_amount": "19077487071"
        },
        "tx": {
          "Account": "rw2ciyaNshpHe7bCHo4bRWq6pqqynnWKQg",
          "Amount": "19077487071",
          "Destination": "rLNaPoKeeBjZe2qs6x52yVPZpZ8td4dc6w",
          "DestinationTag": 1,
          "Fee": "40",
          "Flags": 2147483648,
          "LastLedgerSequence": 57112020,
          "Sequence": 466333,
          "SigningPubKey": "0381575032E254BF4D699C3D8D6EFDB63B3A71F97475C6F6885BC7DAEEE55D9A01",
          "TransactionType": "Payment",
          "TxnSignature": "3045022100CFC5FD057C7C685C690637AD1E639E2642BBC00EFD8E06E3F6C72FA924BC99D40220317D0708E814F69F874D641B6732E37A53B1220B493B2B8390D9EF51E8062515",
          "date": 649200260,
          "hash": "46BF0B576677B0DEA2D94591424A57A2DE8E3D89383631E16F40D09A513C656C",
          "inLedger": 57111998,
          "ledger_index": 57111998
        },
        "validated": true
      },
      {
        "meta": {
          "AffectedNodes": [
            {
              "ModifiedNode": {
                "FinalFields": {
                  "Account": "rLNaPoKeeBjZe2qs6x52yVPZpZ8td4dc6w",
                  "Balance": "3713891690008",
                  "Flags": 131072,
                  "OwnerCount": 0,
                  "Sequence": 702817
                },
                "LedgerEntryType": "AccountRoot",
                "LedgerIndex": "140FA03FE8C39540CA8189BC7A7956795C712BC0A542C6409C041150703C8574",
                "PreviousFields": {
                  "Balance": "3714441690048",
                  "Sequence": 702816
                },
                "PreviousTxnID": "FDD5007913B39027BAF10B31144DBC1F7DC147528DF31FF048A06DC5D3108BD6",
                "PreviousTxnLgrSeq": 57111981
              }
            },
            {
              "ModifiedNode": {
                "FinalFields": {
                  "Account": "r9dU6Z7P2i7MrDi1VUZ7uyq6J77eg86YtB",
                  "Balance": "2629998983",
                  "Flags": 0,
                  "OwnerCount": 0,
                  "Sequence": 10
                },
                "LedgerEntryType": "AccountRoot",
                "LedgerIndex": "27B96FE681B33825CC95DA197358B30D3A1721F2125F2D76022D46B2418ABA0A",
                "PreviousFields": {
                  "Balance": "2079998983"
                },
                "PreviousTxnID": "44A47AC04C0C7237C32BE9A532B578D07641705D3A59DB9B3C5B6225001E39B7",
                "PreviousTxnLgrSeq": 56613857
              }
            }
          ],
          "TransactionIndex": 16,
          "TransactionResult": "tesSUCCESS",
          "delivered_amount": "550000000"
        },
        "tx": {
          "Account": "rLNaPoKeeBjZe2qs6x52yVPZpZ8td4dc6w",
          "Amount": "550000000",
          "Destination": "r9dU6Z7P2i7MrDi1VUZ7uyq6J77eg86YtB",
          "Fee": "40",
          "Flags": 2147483648,
          "LastLedgerSequence": 57112016,
          "Sequence": 702816,
          "SigningPubKey": "020A46D8D02AC780C59853ACA309EAA92E7D8E02DD72A0B6AC315A7D18A6C3276A",
          "TransactionType": "Payment",
          "TxnSignature": "3045022100D589029EF63F9E528F6100C7A36D26AFFF84085EC9AC16DA8E30E11F390D4E87022011466E0FE4A90B89142EE47E535545EEA4A2D65E0BD234DFB447721218B59C9B",
          "date": 649200241,
          "hash": "D58864C16344ADCC15995C7986CFC607CB693E88F84D2E019F0A35FB29749202",
          "inLedger": 57111994,
          "ledger_index": 57111994
        },
        "validated": true
      }
    ],
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
        "account": "rLNaPoKeeBjZe2qs6x52yVPZpZ8td4dc6w",
        "ledger_index_max": 57112019,
        "ledger_index_min": 56248229,
        "limit": 2,
        "marker": {
            "ledger": 57112007,
            "seq": 13
        },
        "status": "success",
        "transactions": [
            {
                "meta": {
                    "AffectedNodes": [
                        {
                            "ModifiedNode": {
                                "FinalFields": {
                                    "Account": "rLNaPoKeeBjZe2qs6x52yVPZpZ8td4dc6w",
                                    "Balance": "3732290013101",
                                    "Flags": 131072,
                                    "OwnerCount": 0,
                                    "Sequence": 702820
                                },
                                "LedgerEntryType": "AccountRoot",
                                "LedgerIndex": "140FA03FE8C39540CA8189BC7A7956795C712BC0A542C6409C041150703C8574",
                                "PreviousFields": {
                                    "Balance": "3732745656171",
                                    "Sequence": 702819
                                },
                                "PreviousTxnID": "7C031FD5B710E3C048EEF31254089BEEC505900BCC9A842257A0319453333998",
                                "PreviousTxnLgrSeq": 57112010
                            }
                        },
                        {
                            "ModifiedNode": {
                                "FinalFields": {
                                    "Account": "raLPjTYeGezfdb6crXZzcC8RkLBEwbBHJ5",
                                    "Balance": "4231510602153",
                                    "Flags": 0,
                                    "OwnerCount": 0,
                                    "Sequence": 96486
                                },
                                "LedgerEntryType": "AccountRoot",
                                "LedgerIndex": "39DC5D448DECEFC3CD20818788E3DA891CA943935E8D7B12FCB5B5871FCB1638",
                                "PreviousFields": {
                                    "Balance": "4231054959123"
                                },
                                "PreviousTxnID": "33D2014C832610293730028CA37857AC183BFCE3E42B9979C491FB8B82B3E9DC",
                                "PreviousTxnLgrSeq": 57112004
                            }
                        }
                    ],
                    "TransactionIndex": 12,
                    "TransactionResult": "tesSUCCESS",
                    "delivered_amount": "455643030"
                },
                "tx": {
                    "Account": "rLNaPoKeeBjZe2qs6x52yVPZpZ8td4dc6w",
                    "Amount": "455643030",
                    "Destination": "raLPjTYeGezfdb6crXZzcC8RkLBEwbBHJ5",
                    "DestinationTag": 18240312,
                    "Fee": "40",
                    "Flags": 2147483648,
                    "LastLedgerSequence": 57112037,
                    "Sequence": 702819,
                    "SigningPubKey": "020A46D8D02AC780C59853ACA309EAA92E7D8E02DD72A0B6AC315A7D18A6C3276A",
                    "TransactionType": "Payment",
                    "TxnSignature": "30450221008602B2E390C0C7B65182C6DBC86292052C1961B2BEFB79C2C8431722C0ADB911022024B74DCF910A4C8C95572CF662EB7F5FF67E1AC4D7B9B7BFE2A8EE851EC16576",
                    "date": 649200322,
                    "hash": "08EF5BDA2825D7A28099219621CDBECCDECB828FEA202DEB6C7ACD5222D36C2C",
                    "inLedger": 57112015,
                    "ledger_index": 57112015
                },
                "validated": true
            },
            {
                "meta": {
                    "AffectedNodes": [
                        {
                            "ModifiedNode": {
                                "FinalFields": {
                                    "Account": "rLNaPoKeeBjZe2qs6x52yVPZpZ8td4dc6w",
                                    "Balance": "3732745656171",
                                    "Flags": 131072,
                                    "OwnerCount": 0,
                                    "Sequence": 702819
                                },
                                "LedgerEntryType": "AccountRoot",
                                "LedgerIndex": "140FA03FE8C39540CA8189BC7A7956795C712BC0A542C6409C041150703C8574",
                                "PreviousFields": {
                                    "Balance": "3732246155784"
                                },
                                "PreviousTxnID": "CCBCCB528F602007C937C496F0828C118E073DF180084CCD3646EC1E414844E4",
                                "PreviousTxnLgrSeq": 57112007
                            }
                        },
                        {
                            "ModifiedNode": {
                                "FinalFields": {
                                    "Account": "rw2ciyaNshpHe7bCHo4bRWq6pqqynnWKQg",
                                    "Balance": "236476361",
                                    "Flags": 131072,
                                    "OwnerCount": 0,
                                    "Sequence": 466335
                                },
                                "LedgerEntryType": "AccountRoot",
                                "LedgerIndex": "CC20FEBEA6D2AF969EC46F2BD92684D9FBABC3F238E841B5E056FE4EBF4379A9",
                                "PreviousFields": {
                                    "Balance": "735976788",
                                    "Sequence": 466334
                                },
                                "PreviousTxnID": "C528B32DD588EFAE2FE833E8AA92E6AE2DF2C8DB3DB8C6C4F334AD37B253D72A",
                                "PreviousTxnLgrSeq": 57112010
                            }
                        }
                    ],
                    "TransactionIndex": 33,
                    "TransactionResult": "tesSUCCESS",
                    "delivered_amount": "499500387"
                },
                "tx": {
                    "Account": "rw2ciyaNshpHe7bCHo4bRWq6pqqynnWKQg",
                    "Amount": "499500387",
                    "Destination": "rLNaPoKeeBjZe2qs6x52yVPZpZ8td4dc6w",
                    "DestinationTag": 1,
                    "Fee": "40",
                    "Flags": 2147483648,
                    "LastLedgerSequence": 57112032,
                    "Sequence": 466334,
                    "SigningPubKey": "0381575032E254BF4D699C3D8D6EFDB63B3A71F97475C6F6885BC7DAEEE55D9A01",
                    "TransactionType": "Payment",
                    "TxnSignature": "3045022100C7EA1701FE48C75508EEBADBC9864CD3FFEDCEB48AB99AEA960BFA360AE163ED0220453C9577502924C9E1A9A450D4B950A44016813BC70E1F16A65A402528D730B7",
                    "date": 649200302,
                    "hash": "7C031FD5B710E3C048EEF31254089BEEC505900BCC9A842257A0319453333998",
                    "inLedger": 57112010,
                    "ledger_index": 57112010
                },
                "validated": true
            }
        ],
        "validated": true
    }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`            | 유형                                                                | 설명                                                                     |
| ------------------ | ----------------------------------------------------------------- | ---------------------------------------------------------------------- |
| `account`          | 문자                                                                | 계정을 식별하는 고유 주소                                                         |
| `ledger_index_min` | 정수 - [원장 색인](https://xrpl.org/basic-data-types.html#ledger-index) | 실제로 트랜잭션을 검색한 가장 이른 원장의 원장 인덱스입니다.                                     |
| `ledger_index_max` | 정수 - [원장 색인](https://xrpl.org/basic-data-types.html#ledger-index) | 실제로 트랜잭션을 검색한 가장 최근 원장의 원장 인덱스입니다.                                     |
| `limit`            | 정수                                                                | 요청에 사용된 제한 값입니다. (서버에서 적용하는 실제 제한 값과 다를 수 있습니다.)                       |
| `marker`           | marker                                                            | 응답의 페이지 매김을 나타내는 서버 정의 값입니다. 이 호출이 중단된 부분부터 다시 시작하려면 다음 호출에 전달합니다.     |
| `transactions`     | 정렬                                                                | 아래 설명된 대로 요청의 기준과 일치하는 트랜잭션의 배열입니다.                                    |
| `validated`        | Boolean                                                           | 포함되고 참으로 설정되면 이 응답의 정보는 유효성 검사된 원장 버전에서 가져옵니다. 그렇지 않으면 정보가 변경될 수 있습니다. |

{% hint style="info" %}
Note:

서버에 사용자가 지정한 버전이 없는 경우와 같이 서버가 요청에 제공한 것과 다른 ledger\_index\_min 및 ledger\_index\_max 값으로 응답할 수 있습니다.
{% endhint %}

각 트랜잭션 객체에는 JSON 또는 16진수 문자열("바이너리":true) 형식으로 요청되었는지 여부에 따라 다음과 같은 필드가 포함됩니다.

| `Field`        | 유형                    | 설명                                                                         |
| -------------- | --------------------- | -------------------------------------------------------------------------- |
| `ledger_index` | 정수                    | 이 트랜잭션이 포함된 원장 버전의 원장 인덱스입니다.                                              |
| `meta`         | 객체(JSON) 또는 문자열(바이너리) | 바이너리가 True이면 트랜잭션 메타데이터의 16진수 문자열입니다. 그렇지 않으면 트랜잭션 메타데이터가 JSON 형식으로 포함됩니다. |
| `tx`           | 객체                    | (JSON 모드만 해당) 트랜잭션을 정의하는 JSON 객체입니다.                                       |
| `tx_blob`      | 문자열                   | (바이너리 모드만 해당) 트랜잭션을 나타내는 고유 해시 문자열입니다.                                     |
| `validated`    | Boolean               | 트랜잭션이 유효한 원장에 포함되는지 여부입니다. 아직 검증 원장에 포함되지 않은 트랜잭션은 변경될 수 있습니다.             |

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* actMalformed - 요청의 계정 필드에 지정된 주소의 형식이 올바르지 않습니다.
* lgrIdxMalformed - ledger\_index\_min 또는 ledger\_index\_max에 지정된 ledger이 존재하지 않거나, 존재하지만 서버에 없는 경우입니다.
* lgrIdxsInvalid - 요청이 ledger\_index\_min보다 앞선 ledger\_index\_max를 지정하거나 서버가 네트워크와 동기화되지 않아 유효성이 검증된 ledger 범위를 가지고 있지 않은 경우입니다.

&#x20;
