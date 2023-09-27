# tx

tx 메소드는 식별 해시를 통해 단일 트랜잭션에 대한 정보를 검색합니다.

## 요청 형식

요청 형식의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 1,
  "command": "tx",
  "transaction": "C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9",
  "binary": false
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "tx",
    "params": [
        {
            "transaction": "C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9",
            "binary": false
        }
    ]
}
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개 변수가 포함됩니다:

| `Field`       | 유형      | 설명                                                                                                                             |
| ------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------ |
| `transaction` | 문자열     | 트랜잭션의 256비트 해시(16진수)입니다.                                                                                                       |
| `binary`      | Boolean | (선택 사항) true이면 트랜잭션 데이터와 메타데이터를 16진수 문자열로 직렬화된 바이너리로 반환합니다. false이면 트랜잭션 데이터와 메타데이터를 JSON으로 반환합니다. 기본값은 false입니다.              |
| `min_ledger`  | 숫자      | (선택 사항) 최대 원장과 함께 사용하여 이 원장부터 시작하여 최대 1000개의 원장 인덱스 범위를 지정할 수 있습니다(포함). 서버가 트랜잭션을 찾을 수 없는 경우, 이 범위의 모든 원장을 검색할 수 있는지 확인합니다.    |
| `max_ledger`  | 숫자      | (선택 사항) 이 원장으로 끝나는 최대 1000개의 원장 인덱스 범위를 지정하려면 min\_ledger와 함께 사용합니다(포함). 서버가 트랜잭션을 찾을 수 없는 경우, 요청된 범위의 모든 원장을 검색할 수 있는지 확인합니다. |

{% hint style="info" %}
Caution:

이 명령은 최소 ledger부터 최대 ledger까지 범위를 벗어난 ledger에 트랜잭션이 포함되어 있어도 성공적으로 트랜잭션을 찾을 수 있습니다.
{% endhint %}

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 1,
  "result": {
    "Account": "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
    "Fee": "12",
    "Flags": 0,
    "LastLedgerSequence": 56865248,
    "OfferSequence": 5037708,
    "Sequence": 5037710,
    "SigningPubKey": "03B51A3EDF70E4098DA7FB053A01C5A6A0A163A30ED1445F14F87C7C3295FCB3BE",
    "TakerGets": "15000000000",
    "TakerPays": {
      "currency": "CNY",
      "issuer": "rKiCet8SdvWxPXnAgYarFUXMh1zCPz432Y",
      "value": "20160.75"
    },
    "TransactionType": "OfferCreate",
    "TxnSignature": "3045022100A5023A0E64923616FCDB6D664F569644C7C9D1895772F986CD6B981B515B02A00220530C973E9A8395BC6FE2484948D2751F6B030FC7FB8575D1BFB406368AD554D9",
    "date": 648248020,
    "hash": "C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9",
    "inLedger": 56865245,
    "ledger_index": 56865245,
    "meta": {
      "AffectedNodes": [
        {
          "ModifiedNode": {
            "FinalFields": {
              "ExchangeRate": "4F04C66806CF7400",
              "Flags": 0,
              "RootIndex": "02BAAC1E67C1CE0E96F0FA2E8061020536CEDD043FEB0FF54F04C66806CF7400",
              "TakerGetsCurrency": "0000000000000000000000000000000000000000",
              "TakerGetsIssuer": "0000000000000000000000000000000000000000",
              "TakerPaysCurrency": "000000000000000000000000434E590000000000",
              "TakerPaysIssuer": "CED6E99370D5C00EF4EBF72567DA99F5661BFB3A"
            },
            "LedgerEntryType": "DirectoryNode",
            "LedgerIndex": "02BAAC1E67C1CE0E96F0FA2E8061020536CEDD043FEB0FF54F04C66806CF7400"
          }
        },
        {
          "ModifiedNode": {
            "FinalFields": {
              "Account": "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
              "Balance": "10404767991",
              "Flags": 0,
              "OwnerCount": 3,
              "Sequence": 5037711
            },
            "LedgerEntryType": "AccountRoot",
            "LedgerIndex": "1DECD9844E95FFBA273F1B94BA0BF2564DDF69F2804497A6D7837B52050174A2",
            "PreviousFields": {
              "Balance": "10404768003",
              "Sequence": 5037710
            },
            "PreviousTxnID": "4DC47B246B5EB9CCE92ABA8C482479E3BF1F946CABBEF74CA4DE36521D5F9008",
            "PreviousTxnLgrSeq": 56865244
          }
        },
        {
          "DeletedNode": {
            "FinalFields": {
              "Account": "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
              "BookDirectory": "02BAAC1E67C1CE0E96F0FA2E8061020536CEDD043FEB0FF54F04C66806CF7400",
              "BookNode": "0000000000000000",
              "Flags": 0,
              "OwnerNode": "0000000000000000",
              "PreviousTxnID": "8F5FF57B404827F12BDA7561876A13C3E3B3095CBF75334DBFB5F227391A660C",
              "PreviousTxnLgrSeq": 56865244,
              "Sequence": 5037708,
              "TakerGets": "15000000000",
              "TakerPays": {
                "currency": "CNY",
                "issuer": "rKiCet8SdvWxPXnAgYarFUXMh1zCPz432Y",
                "value": "20160.75"
              }
            },
            "LedgerEntryType": "Offer",
            "LedgerIndex": "26AAE6CA8D29E28A47C92ADF22D5D96A0216F0551E16936856DDC8CB1AAEE93B"
          }
        },
        {
          "ModifiedNode": {
            "FinalFields": {
              "Flags": 0,
              "IndexNext": "0000000000000000",
              "IndexPrevious": "0000000000000000",
              "Owner": "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
              "RootIndex": "47FAF5D102D8CE655574F440CDB97AC67C5A11068BB3759E87C2B9745EE94548"
            },
            "LedgerEntryType": "DirectoryNode",
            "LedgerIndex": "47FAF5D102D8CE655574F440CDB97AC67C5A11068BB3759E87C2B9745EE94548"
          }
        },
        {
          "CreatedNode": {
            "LedgerEntryType": "Offer",
            "LedgerIndex": "8BAEE3C7DE04A568E96007420FA11ABD0BC9AE44D35932BB5640E9C3FB46BC9B",
            "NewFields": {
              "Account": "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
              "BookDirectory": "02BAAC1E67C1CE0E96F0FA2E8061020536CEDD043FEB0FF54F04C66806CF7400",
              "Sequence": 5037710,
              "TakerGets": "15000000000",
              "TakerPays": {
                "currency": "CNY",
                "issuer": "rKiCet8SdvWxPXnAgYarFUXMh1zCPz432Y",
                "value": "20160.75"
              }
            }
          }
        }
      ],
      "TransactionIndex": 0,
      "TransactionResult": "tesSUCCESS"
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
{
    "result": {
        "Account": "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
        "Fee": "12",
        "Flags": 0,
        "LastLedgerSequence": 56865248,
        "OfferSequence": 5037708,
        "Sequence": 5037710,
        "SigningPubKey": "03B51A3EDF70E4098DA7FB053A01C5A6A0A163A30ED1445F14F87C7C3295FCB3BE",
        "TakerGets": "15000000000",
        "TakerPays": {
            "currency": "CNY",
            "issuer": "rKiCet8SdvWxPXnAgYarFUXMh1zCPz432Y",
            "value": "20160.75"
        },
        "TransactionType": "OfferCreate",
        "TxnSignature": "3045022100A5023A0E64923616FCDB6D664F569644C7C9D1895772F986CD6B981B515B02A00220530C973E9A8395BC6FE2484948D2751F6B030FC7FB8575D1BFB406368AD554D9",
        "date": 648248020,
        "hash": "C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9",
        "inLedger": 56865245,
        "ledger_index": 56865245,
        "meta": {
            "AffectedNodes": [
                {
                    "ModifiedNode": {
                        "FinalFields": {
                            "ExchangeRate": "4F04C66806CF7400",
                            "Flags": 0,
                            "RootIndex": "02BAAC1E67C1CE0E96F0FA2E8061020536CEDD043FEB0FF54F04C66806CF7400",
                            "TakerGetsCurrency": "0000000000000000000000000000000000000000",
                            "TakerGetsIssuer": "0000000000000000000000000000000000000000",
                            "TakerPaysCurrency": "000000000000000000000000434E590000000000",
                            "TakerPaysIssuer": "CED6E99370D5C00EF4EBF72567DA99F5661BFB3A"
                        },
                        "LedgerEntryType": "DirectoryNode",
                        "LedgerIndex": "02BAAC1E67C1CE0E96F0FA2E8061020536CEDD043FEB0FF54F04C66806CF7400"
                    }
                },
                {
                    "ModifiedNode": {
                        "FinalFields": {
                            "Account": "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
                            "Balance": "10404767991",
                            "Flags": 0,
                            "OwnerCount": 3,
                            "Sequence": 5037711
                        },
                        "LedgerEntryType": "AccountRoot",
                        "LedgerIndex": "1DECD9844E95FFBA273F1B94BA0BF2564DDF69F2804497A6D7837B52050174A2",
                        "PreviousFields": {
                            "Balance": "10404768003",
                            "Sequence": 5037710
                        },
                        "PreviousTxnID": "4DC47B246B5EB9CCE92ABA8C482479E3BF1F946CABBEF74CA4DE36521D5F9008",
                        "PreviousTxnLgrSeq": 56865244
                    }
                },
                {
                    "DeletedNode": {
                        "FinalFields": {
                            "Account": "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
                            "BookDirectory": "02BAAC1E67C1CE0E96F0FA2E8061020536CEDD043FEB0FF54F04C66806CF7400",
                            "BookNode": "0000000000000000",
                            "Flags": 0,
                            "OwnerNode": "0000000000000000",
                            "PreviousTxnID": "8F5FF57B404827F12BDA7561876A13C3E3B3095CBF75334DBFB5F227391A660C",
                            "PreviousTxnLgrSeq": 56865244,
                            "Sequence": 5037708,
                            "TakerGets": "15000000000",
                            "TakerPays": {
                                "currency": "CNY",
                                "issuer": "rKiCet8SdvWxPXnAgYarFUXMh1zCPz432Y",
                                "value": "20160.75"
                            }
                        },
                        "LedgerEntryType": "Offer",
                        "LedgerIndex": "26AAE6CA8D29E28A47C92ADF22D5D96A0216F0551E16936856DDC8CB1AAEE93B"
                    }
                },
                {
                    "ModifiedNode": {
                        "FinalFields": {
                            "Flags": 0,
                            "IndexNext": "0000000000000000",
                            "IndexPrevious": "0000000000000000",
                            "Owner": "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
                            "RootIndex": "47FAF5D102D8CE655574F440CDB97AC67C5A11068BB3759E87C2B9745EE94548"
                        },
                        "LedgerEntryType": "DirectoryNode",
                        "LedgerIndex": "47FAF5D102D8CE655574F440CDB97AC67C5A11068BB3759E87C2B9745EE94548"
                    }
                },
                {
                    "CreatedNode": {
                        "LedgerEntryType": "Offer",
                        "LedgerIndex": "8BAEE3C7DE04A568E96007420FA11ABD0BC9AE44D35932BB5640E9C3FB46BC9B",
                        "NewFields": {
                            "Account": "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
                            "BookDirectory": "02BAAC1E67C1CE0E96F0FA2E8061020536CEDD043FEB0FF54F04C66806CF7400",
                            "Sequence": 5037710,
                            "TakerGets": "15000000000",
                            "TakerPays": {
                                "currency": "CNY",
                                "issuer": "rKiCet8SdvWxPXnAgYarFUXMh1zCPz432Y",
                                "value": "20160.75"
                            }
                        }
                    }
                }
            ],
            "TransactionIndex": 0,
            "TransactionResult": "tesSUCCESS"
        },
        "status": "success",
        "validated": true
    }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 트랜잭션 객체의 필드와 다음과 같은 추가 필드가 포함됩니다:

| `Field`        | 유형                    | 설명                                                                                                              |
| -------------- | --------------------- | --------------------------------------------------------------------------------------------------------------- |
| `date`         | 숫자                    | 트랜잭션이 적용된 ledger의 마감 시간을 나타내는 2000년 1월 1일(00:00 UTC) 이후의 시간(초)입니다. 이 값은 실제 시간과 정확한 관계가 없으며 마감 시간 분해능에 따라 달라집니다. |
| `hash`         | 문자열                   | 거래의 SHA-512 해시                                                                                                  |
| `inLedger`     | 숫자                    | (더 이상 사용되지 않음) ledger\_index의 별칭입니다.                                                                            |
| `ledger_index` | 숫자                    | 해당 거래가 포함된 원장의 원장 색인입니다.                                                                                        |
| `meta`         | 객체(JSON) 또는 문자열(바이너리) | 트랜잭션 결과를 설명하는 트랜잭션 메타데이터 입니다.                                                                                   |
| `validated`    | Boolean               | 참이면 이 데이터는 검증된 원장 버전에서 가져온 것이고, 생략하거나 거짓으로 설정하면 이 데이터는 최종 버전이 아닙니다.                                             |
| (다양한)          | (변수)                  | 트랜잭션 객체의 기타 필드                                                                                                  |

{% hint style="info" %}
Note:

rippled 1.7.0에는 요청이 바이너리를 요청하더라도 메타 필드에 JSON이 포함되는 알려진 문제가 있습니다. (#3791 )
{% endhint %}

Not Found 응답

서버가 트랜잭션을 찾지 못하면 두 가지 의미로 txnNotFound 오류를 반환합니다:

* 트랜잭션이 ledger 버전에 포함되지 않았으며 실행되지 않았습니다.
* 트랜잭션이 서버에서 사용할 수 없는 ledger 버전에 포함되었습니다.

즉, txnNotFound만으로는 트랜잭션의 최종 결과를 알기에 충분하지 않습니다.

가능성을 더 좁히려면 요청에 최소 ledger 및 최대 ledger 필드를 사용하여 검색할 ledger 범위를 제공할 수 있습니다. 이 두 필드를 모두 제공하면 txnNotFound 응답에 다음 필드가 포함됩니다:

| `Field`        | 유형      | 설명                                                                                                                                                                                                                                                                                                                 |
| -------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `searched_all` | Boolean | (요청에 최소 원장 및 최대 원장이 제공되지 않은 경우 생략) 참이면 서버가 지정된 모든 원장 버전을 검색할 수 있으며 트랜잭션이 그 중 어느 버전에도 없습니다. false이면 서버가 지정된 모든 원장 버전을 사용할 수 없으므로 그 중 하나에 트랜잭션이 포함될 수 있는지 확실하지 않습니다.[![새로운 기능: Rippled 1.5.0](https://img.shields.io/badge/New%20in-rippled%201.5.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/1.5.0) |

요청된 범위의 ledger을 모두 검색한 txnNotFound 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "error": "txnNotFound",
  "error_code": 29,
  "error_message": "Transaction not found.",
  "id": 1,
  "request": {
    "binary": false,
    "command": "tx",
    "id": 1,
    "max_ledger": 54368673,
    "min_ledger": 54368573,
    "transaction": "E08D6E9754025BA2534A78707605E0601F03ACE063687A0CA1BDDACFCD1698C7"
  },
  "searched_all": true,
  "status": "error",
  "type": "response"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
200 OK

{
  "result": {
    "error": "txnNotFound",
    "error_code": 29,
    "error_message": "Transaction not found.",
    "request": {
      "binary": false,
      "command": "tx",
      "max_ledger": 54368673,
      "min_ledger": 54368573,
      "transaction": "E08D6E9754025BA2534A78707605E0601F03ACE063687A0CA1BDDACFCD1698C7"
    },
    "searched_all": true,
    "status": "error"
  }
}
```
{% endtab %}
{% endtabs %}

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* txnNotFound - 트랜잭션이 존재하지 않거나 리플이 사용할 수 없는 ledger 버전의 일부였습니다.
* excessiveLgrRange - 요청의 최소 ledger 필드와 최대 ledger 필드의 간격이 1000 이상입니다.
* invalidLgrRange - 지정된 최소값이 최대값보다 크거나 해당 매개변수 중 하나가 유효한 ledger 인덱스가 아닙니다.
