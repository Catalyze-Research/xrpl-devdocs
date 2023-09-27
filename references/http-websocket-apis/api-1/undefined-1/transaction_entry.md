# transaction\_entry

transaction\_entry 메소드는 특정 원장 버전에서 단일 트랜잭션에 대한 정보를 검색합니다. (반면 tx 메소드는 지정된 트랜잭션에 대해 모든 원장을 검색합니다. 이 메소드를 대신 사용하는 것이 좋습니다.)

## 요청 형식

요청 형식의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 4,
  "command": "transaction_entry",
  "tx_hash": "C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9",
  "ledger_index": 56865245
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "transaction_entry",
    "params": [
        {
            "tx_hash": "C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9",
            "ledger_index": 56865245
        }
    ]
}
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| `Field`        | 유형              | 설명                                                                                                                                                                             |
| -------------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `ledger_hash`  |                 | (선택 사항) 사용할 원장 버전에 대한 20바이트 16진수 문자열입니다. ([원장 지정](https://xrpl.org/basic-data-types.html#specifying-ledgers) 참조)                                                               |
| `ledger_index` | 문자열 또는 부호 없는 정수 | (선택) 사용할 원장의 [원장 인덱스](https://xrpl.org/basic-data-types.html#ledger-index) 또는 자동으로 원장을 선택하는 단축 문자열입니다. ([원장 지정](https://xrpl.org/basic-data-types.html#specifying-ledgers) 참조) |
| `tx_hash`      |                 | 찾고 있는 거래의 고유 해시                                                                                                                                                                |

{% hint style="info" %}
Note:

이 메소드는 현재 진행 중인 ledger의 정보 검색을 지원하지 않습니다. ledger\_index 또는 ledger\_hash에 ledger 버전을 지정해야 합니다.
{% endhint %}

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 4,
  "result": {
    "ledger_hash": "793E56131D8D4ABFB27FA383BFC44F2978B046E023FF46C588D7E0C874C2472A",
    "ledger_index": 56865245,
    "metadata": {
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
    "tx_json": {
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
      "hash": "C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9"
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
        "ledger_hash": "793E56131D8D4ABFB27FA383BFC44F2978B046E023FF46C588D7E0C874C2472A",
        "ledger_index": 56865245,
        "metadata": {
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
        "tx_json": {
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
            "hash": "C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9"
        },
        "validated": true
    }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`        | 유형                                                                | 설명                                                                    |
| -------------- | ----------------------------------------------------------------- | --------------------------------------------------------------------- |
| `ledger_index` | 번호 - [원장 색인](https://xrpl.org/basic-data-types.html#ledger-index) | 거래가 발견된 원장 버전의 원장 인덱스입니다. 이는 요청한 내용과 동일합니다.                           |
| `ledger_hash`  | 문자열 - [해시](https://xrpl.org/basic-data-types.html#hashes)         | _(생략 가능)_ 거래가 발견된 원장 버전의 식별 해시입니다. 이는 요청한 내용과 동일합니다.                  |
| `metadata`     | 객체                                                                | 거래의 정확한 결과를 자세히 보여주는 거래 메타데이터입니다.                                     |
| `tx_json`      | 객체                                                                | [Transaction 객체](https://xrpl.org/transaction-formats.html)의 JSON 표현. |

서버가 트랜잭션을 찾지 못할 수 있는 몇 가지 이유가 있습니다:

* 트랜잭션이 존재하지 않습니다.
* 트랜잭션이 존재하지만 지정된 ledger 버전이 아닙니다.
* 서버에 지정된 ledger 버전을 사용할 수 없습니다. 올바른 버전을 보유하고 있는 다른 서버에서 다른 응답을 받을 수 있습니다.

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* fieldNotFoundTransaction - 요청에서 tx\_hash 필드가 생략되었습니다.
* notYetImplemented - 요청에 ledger 버전이 지정되지 않았습니다.
* lgrNotFound - ledger\_hash 또는 ledger\_index로 지정된 ledger이 존재하지 않거나, 존재하지만 서버에 없습니다.
* transactionNotFound - 요청에 지정된 트랜잭션을 지정된 ledger에서 찾을 수 없습니다. (다른 ledger 버전에 있거나 아예 사용할 수 없을 수도 있습니다.)
