# 트랜잭션 메타데이터(Transaction Metadata)



[Currency Amount](https://xrpl.org/basic-data-types.html#specifying-currency-amounts)트랜잭션 메타데이터는 트랜잭션이 처리된 후 트랜잭션에 추가되는 데이터 섹션입니다. Ledger에 포함되는 모든 트랜잭션에는 성공 여부와 관계없이 메타데이터가 있습니다. 트랜잭션 메타데이터는 트랜잭션의 결과를 자세히 설명합니다.

{% hint style="danger" %}
**Warning**:

트랜잭션 메타데이터에 설명된 변경 사항은 트랜잭션이 검증된 Ledger 버전에 있는 경우에만 최종적입니다.
{% endhint %}

트랜잭션 메타데이터에 표시될 수 있는 일부 필드는 다음과 같습니다:

| 필드                                                                                 | 값                                                                                     | 설명                                                                                                                                                                                                                                                                |
| ---------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `AffectedNodes`                                                                    | Array                                                                                 | 이 트랜잭션에 의해 생성, 삭제 또는 수정된 원장 객체 목록 과 각 객체에 대한 특정 변경 사항입니다.                                                                                                                                                                                                         |
| `DeliveredAmount`                                                                  | [Currency Amount](https://xrpl.org/basic-data-types.html#specifying-currency-amounts) | _(생략가능)_ [부분결제](https://xrpl.org/partial-payments.html)의 경우 실제로 목적지까지 전달된 통화금액을 기록하는 필드입니다. 거래를 읽을 때 오류를 방지하려면 `delivered_amount`부분 여부에 관계없이 모든 결제 거래에 대해 제공되는 필드를 대신 사용하세요.                                                                                      |
| `TransactionIndex`                                                                 | Unsigned Integer                                                                      | 해당 거래가 포함된 원장 내 거래의 위치입니다. 이것은 0 인덱스입니다. (예를 들어 값 2는 해당 원장의 세 번째 거래임을 의미합니다.)                                                                                                                                                                                     |
| `TransactionResult`                                                                | String                                                                                | 트랜잭션의 성공 여부 또는 실패 방법을 나타내는 결과 코드입니다.                                                                                                                                                                                                                              |
| [`delivered_amount`](https://xrpl.org/transaction-metadata.html#delivered\_amount) | [Currency Amount](https://xrpl.org/basic-data-types.html#specifying-currency-amounts) | (비결제 거래의 경우 생략) 대상 계정에서 실제로 수령한 통화 금액입니다. 이 필드를 사용하여 거래가 부분 결제인지 여부에 관계없이 배송된 금액을 확인할 수 있습니다. 자세한 내용은 이 설명을 참조하세요.[![새로운 기능: 파문이 0.27.0](https://img.shields.io/badge/New%20in-rippled%200.27.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/0.27.0) |

## Metadata 예시

다음 JSON 객체는 [a complex cross-currency payment](https://xrpcharts.ripple.com/?\_\_hstc=78174987.eb834f4976cc9a9e484fa466d5c5acb0.1692504586144.1700629533999.1700631525411.29&\_\_hssc=78174987.8.1700631525411&\_\_hsfp=875642578#/transactions/8C55AFC2A2AA42B5CE624AEECDB3ACFDD1E5379D4E5BF74A8460C5E97EF8706B)에 대한 메타데이터를 보여줍니다:

```json
{
  "AffectedNodes": [
    {
      "ModifiedNode": {
        "FinalFields": {
          "Account": "r9ZoLsJHzMMJLpvsViWQ4Jgx17N8cz1997",
          "Balance": "77349986",
          "Flags": 0,
          "OwnerCount": 2,
          "Sequence": 9
        },
        "LedgerEntryType": "AccountRoot",
        "LedgerIndex": "1E7E658C2D3DF91EFAE5A12573284AD6F526B8F64DD12F013C6F889EF45BEA97",
        "PreviousFields": {
          "OwnerCount": 3
        },
        "PreviousTxnID": "55C11248ACEFC2EFD59755BF88867783AC18EA078517108F942069C2FBE4CF5C",
        "PreviousTxnLgrSeq": 35707468
      }
    },
    {
      "ModifiedNode": {
        "FinalFields": {
          "Balance": {
            "currency": "USD",
            "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
            "value": "2298.927882138068"
          },
          "Flags": 1114112,
          "HighLimit": {
            "currency": "USD",
            "issuer": "rLEsXccBGNR3UPuPu2hUXPjziKC3qKSBun",
            "value": "0"
          },
          "HighNode": "000000000000006B",
          "LowLimit": {
            "currency": "USD",
            "issuer": "rpvvAvaZ7TXHkNLM8UJwCTU6yBU2jDTJ1P",
            "value": "1000000000"
          },
          "LowNode": "0000000000000007"
        },
        "LedgerEntryType": "RippleState",
        "LedgerIndex": "220DDA7164F3F41F3C5223FA3125D4CD368EBB4FB954B5FBFFB6D1EA6DACDD5E",
        "PreviousFields": {
          "Balance": {
            "currency": "USD",
            "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
            "value": "2297.927882138068"
          }
        },
        "PreviousTxnID": "1DB2F9C67C3F42F7B8AB02BA2264254A78A201EC8A9974A1CACEFD51545B1263",
        "PreviousTxnLgrSeq": 43081739
      }
    },
    {
      "ModifiedNode": {
        "FinalFields": {
          "Balance": {
            "currency": "USD",
            "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
            "value": "33403.80553244443"
          },
          "Flags": 1114112,
          "HighLimit": {
            "currency": "USD",
            "issuer": "rhub8VRN55s94qWKDv6jmDy1pUykJzF3wq",
            "value": "0"
          },
          "HighNode": "0000000000001A40",
          "LowLimit": {
            "currency": "USD",
            "issuer": "rd5Sx93pCMgfxwBuofjen2csoFYmY8VrT",
            "value": "1000000000"
          },
          "LowNode": "0000000000000000"
        },
        "LedgerEntryType": "RippleState",
        "LedgerIndex": "38569918AF54B520463CFDDD00EB5ADD8768039BD94E61A5E25C387EA4FDC9A3",
        "PreviousFields": {
          "Balance": {
            "currency": "USD",
            "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
            "value": "33402.80752845242"
          }
        },
        "PreviousTxnID": "38A0E82ADC2DA6C6D59929B73E9812CD1E1384E452FD23D0717EA0037E2FC9E3",
        "PreviousTxnLgrSeq": 43251694
      }
    },
    {
      "ModifiedNode": {
        "FinalFields": {
          "Account": "rBndiPPKs9k5rjBb7HsEiqXKrz8AfUnqWq",
          "BookDirectory": "4627DFFCFF8B5A265EDBD8AE8C14A52325DBFEDAF4F5C32E5B09B13AC59DBA5E",
          "BookNode": "0000000000000000",
          "Flags": 0,
          "OwnerNode": "0000000000000000",
          "Sequence": 407556,
          "TakerGets": {
            "currency": "USD",
            "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
            "value": "75.1379833998197"
          },
          "TakerPays": "204986996"
        },
        "LedgerEntryType": "Offer",
        "LedgerIndex": "557BDD35E40EAFFE0AC98108A0F4AC4BB812A168CFD5B4E35475F42A60ABD9C8",
        "PreviousFields": {
          "TakerGets": {
            "currency": "USD",
            "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
            "value": "76.1399833998197"
          },
          "TakerPays": "207720593"
        },
        "PreviousTxnID": "961C575073788979815F103D065CEE449D2EA6EFE8FC8C33C26EC08586925D90",
        "PreviousTxnLgrSeq": 43251680
      }
    },
    {
      "ModifiedNode": {
        "FinalFields": {
          "Account": "r9KG7Du7aFmABzMvDnwuvPaEoMu4Eurwok",
          "Balance": "8080207629",
          "Flags": 0,
          "OwnerCount": 6,
          "Sequence": 1578765
        },
        "LedgerEntryType": "AccountRoot",
        "LedgerIndex": "5A667CB5FBAB4143EDEFBD6EDDD4B6D19C905209C8EE16486D5D7CD6CB083E78",
        "PreviousFields": {
          "Balance": "8080152531",
          "Sequence": 1578764
        },
        "PreviousTxnID": "E3CDFD288620871455634DC1E56439136AACA1DDBCE987BE12F97486AB477375",
        "PreviousTxnLgrSeq": 43251694
      }
    },
    {
      "DeletedNode": {
        "FinalFields": {
          "Account": "r9ZoLsJHzMMJLpvsViWQ4Jgx17N8cz1997",
          "BookDirectory": "A6D5D1C1CC92D56FDDFD4434FB10BD31F63EB991DA3C756653071AFD498D0000",
          "BookNode": "0000000000000000",
          "Flags": 0,
          "OwnerNode": "0000000000000000",
          "PreviousTxnID": "DB028A461E98B0398CAD65F2871B381A6D0B9A21662CA5B033438D83C518C0F2",
          "PreviousTxnLgrSeq": 35686129,
          "Sequence": 7,
          "TakerGets": {
            "currency": "EUR",
            "issuer": "rhub8VRN55s94qWKDv6jmDy1pUykJzF3wq",
            "value": "2.5"
          },
          "TakerPays": {
            "currency": "ETH",
            "issuer": "rcA8X3TVMST1n3CJeAdGk1RdRCHii7N2h",
            "value": "0.05"
          }
        },
        "LedgerEntryType": "Offer",
        "LedgerIndex": "6AA7E5121FEB456F0A899E3D6F25D62ABB408BB67B91C9270E13714401ED72B5"
      }
    },
    {
      "ModifiedNode": {
        "FinalFields": {
          "Account": "rd5Sx93pCMgfxwBuofjen2csoFYmY8VrT",
          "Balance": "8251028196",
          "Flags": 0,
          "OwnerCount": 4,
          "Sequence": 274
        },
        "LedgerEntryType": "AccountRoot",
        "LedgerIndex": "6F830A1B38F827CD4BEC946A40F1E2DF726FC22AFC3918FD621567AF17F49F3A",
        "PreviousFields": {
          "Balance": "8253816902"
        },
        "PreviousTxnID": "38A0E82ADC2DA6C6D59929B73E9812CD1E1384E452FD23D0717EA0037E2FC9E3",
        "PreviousTxnLgrSeq": 43251694
      }
    },
    {
      "ModifiedNode": {
        "FinalFields": {
          "Account": "rd5Sx93pCMgfxwBuofjen2csoFYmY8VrT",
          "BookDirectory": "79C54A4EBD69AB2EADCE313042F36092BE432423CC6A4F784E0CB6D74F25A336",
          "BookNode": "0000000000000000",
          "Flags": 0,
          "OwnerNode": "0000000000000000",
          "Sequence": 273,
          "TakerGets": "8246341599",
          "TakerPays": {
            "currency": "USD",
            "issuer": "rhub8VRN55s94qWKDv6jmDy1pUykJzF3wq",
            "value": "2951.147613535471"
          }
        },
        "LedgerEntryType": "Offer",
        "LedgerIndex": "7FD1EAAE17B7D68AE640FFC56CECC3999B4F938EFFF6EA6887B6CC8BD9DBDC63",
        "PreviousFields": {
          "TakerGets": "8249130305",
          "TakerPays": {
            "currency": "USD",
            "issuer": "rhub8VRN55s94qWKDv6jmDy1pUykJzF3wq",
            "value": "2952.145617527486"
          }
        },
        "PreviousTxnID": "38A0E82ADC2DA6C6D59929B73E9812CD1E1384E452FD23D0717EA0037E2FC9E3",
        "PreviousTxnLgrSeq": 43251694
      }
    },
    {
      "ModifiedNode": {
        "FinalFields": {
          "Balance": {
            "currency": "USD",
            "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
            "value": "-11.68225001668339"
          },
          "Flags": 131072,
          "HighLimit": {
            "currency": "USD",
            "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
            "value": "5000"
          },
          "HighNode": "0000000000000000",
          "LowLimit": {
            "currency": "USD",
            "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
            "value": "0"
          },
          "LowNode": "000000000000004A"
        },
        "LedgerEntryType": "RippleState",
        "LedgerIndex": "826CF5BFD28F3934B518D0BDF3231259CBD3FD0946E3C3CA0C97D2C75D2D1A09",
        "PreviousFields": {
          "Balance": {
            "currency": "USD",
            "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
            "value": "-10.68225001668339"
          }
        },
        "PreviousTxnID": "28B271F7C27C1A267F32FFCD8B1795C5D3B1DC761AD705E3A480139AA8B61B09",
        "PreviousTxnLgrSeq": 43237130
      }
    },
    {
      "ModifiedNode": {
        "FinalFields": {
          "Account": "rBndiPPKs9k5rjBb7HsEiqXKrz8AfUnqWq",
          "Balance": "8276201534",
          "Flags": 0,
          "OwnerCount": 5,
          "Sequence": 407558
        },
        "LedgerEntryType": "AccountRoot",
        "LedgerIndex": "880C6FB7B9C0083211F950E4449AD45895C0EC1114B5112CE1320AC7275E3237",
        "PreviousFields": {
          "Balance": "8273467937"
        },
        "PreviousTxnID": "CB4B54942F11510A47D2731C3260429093F24016B366CBF15D8EC4B705372F02",
        "PreviousTxnLgrSeq": 43251683
      }
    },
    {
      "ModifiedNode": {
        "FinalFields": {
          "Balance": {
            "currency": "USD",
            "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
            "value": "-6557.745685633666"
          },
          "Flags": 2228224,
          "HighLimit": {
            "currency": "USD",
            "issuer": "rBndiPPKs9k5rjBb7HsEiqXKrz8AfUnqWq",
            "value": "1000000000"
          },
          "HighNode": "0000000000000000",
          "LowLimit": {
            "currency": "USD",
            "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
            "value": "0"
          },
          "LowNode": "0000000000000512"
        },
        "LedgerEntryType": "RippleState",
        "LedgerIndex": "8A9FEE5192E334195314B5C162BC78F7452ADB14E06839D48943BAE05EE1967F",
        "PreviousFields": {
          "Balance": {
            "currency": "USD",
            "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
            "value": "-6558.747685633666"
          }
        },
        "PreviousTxnID": "961C575073788979815F103D065CEE449D2EA6EFE8FC8C33C26EC08586925D90",
        "PreviousTxnLgrSeq": 43251680
      }
    },
    {
      "ModifiedNode": {
        "FinalFields": {
          "Balance": {
            "currency": "GCB",
            "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
            "value": "9990651675.348776"
          },
          "Flags": 3211264,
          "HighLimit": {
            "currency": "GCB",
            "issuer": "rHaans8PtgwbacHvXAL3u6TG28gTAtCwr8",
            "value": "0"
          },
          "HighNode": "0000000000000000",
          "LowLimit": {
            "currency": "GCB",
            "issuer": "r9KG7Du7aFmABzMvDnwuvPaEoMu4Eurwok",
            "value": "10000000000"
          },
          "LowNode": "0000000000000000"
        },
        "LedgerEntryType": "RippleState",
        "LedgerIndex": "A2B41EE7818A5756B6A2276BDBB3CE0ED3A3B350787FD6B76E5EA1354A8F20D2",
        "PreviousFields": {
          "Balance": {
            "currency": "GCB",
            "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
            "value": "9990651678.137482"
          }
        },
        "PreviousTxnID": "961C575073788979815F103D065CEE449D2EA6EFE8FC8C33C26EC08586925D90",
        "PreviousTxnLgrSeq": 43251680
      }
    },
    {
      "DeletedNode": {
        "FinalFields": {
          "ExchangeRate": "53071AFD498D0000",
          "Flags": 0,
          "RootIndex": "A6D5D1C1CC92D56FDDFD4434FB10BD31F63EB991DA3C756653071AFD498D0000",
          "TakerGetsCurrency": "0000000000000000000000004555520000000000",
          "TakerGetsIssuer": "2ADB0B3959D60A6E6991F729E1918B7163925230",
          "TakerPaysCurrency": "0000000000000000000000004554480000000000",
          "TakerPaysIssuer": "06CC4A6D023E68AA3499C6DE3E9F2DC52B8BA254"
        },
        "LedgerEntryType": "DirectoryNode",
        "LedgerIndex": "A6D5D1C1CC92D56FDDFD4434FB10BD31F63EB991DA3C756653071AFD498D0000"
      }
    },
    {
      "ModifiedNode": {
        "FinalFields": {
          "Flags": 0,
          "Owner": "r9ZoLsJHzMMJLpvsViWQ4Jgx17N8cz1997",
          "RootIndex": "A83C1B192A27582EDB320EBD7A3FE58D7042CE04B67A2B3D87FDD63D871E12D7"
        },
        "LedgerEntryType": "DirectoryNode",
        "LedgerIndex": "A83C1B192A27582EDB320EBD7A3FE58D7042CE04B67A2B3D87FDD63D871E12D7"
      }
    },
    {
      "ModifiedNode": {
        "FinalFields": {
          "Balance": {
            "currency": "USD",
            "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
            "value": "0"
          },
          "Flags": 65536,
          "HighLimit": {
            "currency": "USD",
            "issuer": "rLEsXccBGNR3UPuPu2hUXPjziKC3qKSBun",
            "value": "0"
          },
          "HighNode": "0000000000000002",
          "LowLimit": {
            "currency": "USD",
            "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
            "value": "1"
          },
          "LowNode": "0000000000000000"
        },
        "LedgerEntryType": "RippleState",
        "LedgerIndex": "C493ABA2619D0FC6355BA862BC8312DF8266FBE76AFBA9636E857F7EAC874A99",
        "PreviousFields": {
          "Balance": {
            "currency": "USD",
            "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
            "value": "1"
          }
        },
        "PreviousTxnID": "28B271F7C27C1A267F32FFCD8B1795C5D3B1DC761AD705E3A480139AA8B61B09",
        "PreviousTxnLgrSeq": 43237130
      }
    },
    {
      "ModifiedNode": {
        "FinalFields": {
          "Account": "r9KG7Du7aFmABzMvDnwuvPaEoMu4Eurwok",
          "BookDirectory": "E6E8A9842EA2ED1FD5D0599343692CE1EBF977AEA751B7DC5B038D7EA4C68000",
          "BookNode": "0000000000000000",
          "Flags": 65536,
          "OwnerNode": "0000000000000000",
          "Sequence": 39018,
          "TakerGets": {
            "currency": "GCB",
            "issuer": "rHaans8PtgwbacHvXAL3u6TG28gTAtCwr8",
            "value": "9990651675.348776"
          },
          "TakerPays": "9990651675348776"
        },
        "LedgerEntryType": "Offer",
        "LedgerIndex": "C939B9B2C5803DD6D89B792E72470F79CBE9F9E999691789E0B68C3808BDDD8E",
        "PreviousFields": {
          "TakerGets": {
            "currency": "GCB",
            "issuer": "rHaans8PtgwbacHvXAL3u6TG28gTAtCwr8",
            "value": "9990651678.137482"
          },
          "TakerPays": "9990651678137482"
        },
        "PreviousTxnID": "961C575073788979815F103D065CEE449D2EA6EFE8FC8C33C26EC08586925D90",
        "PreviousTxnLgrSeq": 43251680
      }
    },
    {
      "ModifiedNode": {
        "FinalFields": {
          "Balance": {
            "currency": "USD",
            "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
            "value": "2963.413395452545"
          },
          "Flags": 65536,
          "HighLimit": {
            "currency": "USD",
            "issuer": "rhub8VRN55s94qWKDv6jmDy1pUykJzF3wq",
            "value": "0"
          },
          "HighNode": "0000000000001A97",
          "LowLimit": {
            "currency": "USD",
            "issuer": "rpvvAvaZ7TXHkNLM8UJwCTU6yBU2jDTJ1P",
            "value": "0"
          },
          "LowNode": "0000000000000007"
        },
        "LedgerEntryType": "RippleState",
        "LedgerIndex": "E4D1FBD5CB72A1D3EE38C21F3BCB13E454FCB469CD01C1366E0008A031E6A7FC",
        "PreviousFields": {
          "Balance": {
            "currency": "USD",
            "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
            "value": "2964.413395452545"
          }
        },
        "PreviousTxnID": "1DB2F9C67C3F42F7B8AB02BA2264254A78A201EC8A9974A1CACEFD51545B1263",
        "PreviousTxnLgrSeq": 43081739
      }
    }
  ],
  "DeliveredAmount": {
    "currency": "GCB",
    "issuer": "rHaans8PtgwbacHvXAL3u6TG28gTAtCwr8",
    "value": "2.788706"
  },
  "TransactionIndex": 38,
  "TransactionResult": "tesSUCCESS",
  "delivered_amount": {
    "currency": "GCB",
    "issuer": "rHaans8PtgwbacHvXAL3u6TG28gTAtCwr8",
    "value": "2.788706"
  }
}
```

## AffectedNodes

&#x20;`AffectedNodes`배열에는 이 트랜잭션이 어떤 식으로든 수정한 ledger의 객체 전체 목록이 포함됩니다. 이 배열의 각 항목은 어떤 유형인지 나타내는 최상위 필드 하나가 있는 객체입니다:&#x20;

* `CreatedNode`는 트랜잭션이 ledger에 새 개체를 생성했음을 나타냅니다.
* `DeletedNode`는 트랜잭션이 ledger에서 개체를 제거했음을 나타냅니다.
* `ModifiedNode`는 트랜잭션이 ledger에 있는 기존 개체를 수정했음을 나타냅니다.

이러한 각 필드의 값은 ledger 개체에 대한 변경 사항을 설명하는 JSON 객체입니다.

## CreatedNode Fields

&#x20;`CreatedNode`객체에는 다음 필드가 포함됩니다:

| 필드              | 값                                                              | 설명                                                                                                                                                                                                                  |
| --------------- | -------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| LedgerEntryType | String                                                         | 생성된 [ledger 객체의 유형입니다](https://xrpl.org/ledger-object-types.html) .                                                                                                                                                 |
| LedgerIndex     | String - [Hash](https://xrpl.org/basic-data-types.html#hashes) | ledger의 [상태 트리 에 있는 ](https://xrpl.org/ledgers.html)[이 원장 객체의 ID](https://xrpl.org/ledger-object-ids.html) 입니다 . 참고: 필드 이름이 매우 유사하더라도 이는 [원장 색인](https://xrpl.org/basic-data-types.html#ledger-index) 과 동일하지 않습니다 . |
| NewFields       | Object                                                         | 새로 생성된 원장 개체의 콘텐츠 필드입니다. 존재하는 필드는 생성된 원장 객체의 유형에 따라 달라집니다.                                                                                                                                                          |

## DeletedNode Fields

`DeletedNode`객체에는 다음 필드가 포함됩니다:

| 필드                | 값                                                              | 설명                                                                                                                                                                                                                    |
| ----------------- | -------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `LedgerEntryType` | String                                                         | 삭제된 [원장 객체의 유형입니다](https://xrpl.org/ledger-object-types.html).                                                                                                                                                        |
| `LedgerIndex`     | String - [Hash](https://xrpl.org/basic-data-types.html#hashes) | 원장의 [상태 트리 에 있는 ](https://xrpl.org/ledgers.html)[이 원장 객체의 ID](https://xrpl.org/ledger-object-ids.html) 입니다. **참고:** 필드 이름이 매우 유사하더라도 이는 [원장 색인](https://xrpl.org/basic-data-types.html#ledger-index) 과 **동일하지 않습니다.** |
| `FinalFields`     | Object                                                         | 삭제 직전 원장 개체의 콘텐츠 필드입니다. 존재하는 필드는 생성된 원장 객체의 유형에 따라 달라집니다.                                                                                                                                                             |

## ModifiedNode  Fields

&#x20;`ModifiedNode`객체에는 다음 필드가 포함됩니다:

| 필드                  | 값                                                                 | 설명                                                                                                                                                                                                                   |
| ------------------- | ----------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `LedgerEntryType`   | 문자열                                                               | 삭제된 [원장 객체의 유형입니다](https://xrpl.org/ledger-object-types.html).                                                                                                                                                       |
| `LedgerIndex`       | 문자열 - [해시](https://xrpl.org/basic-data-types.html#hashes)         | 원장의 [상태 트리 에 있는 ](https://xrpl.org/ledgers.html)[이 원장 객체의 ID](https://xrpl.org/ledger-object-ids.html) 입니다. **참고:** 필드 이름이 매우 유사하더라도 이는 [원장 색인](https://xrpl.org/basic-data-types.html#ledger-index)과 **동일하지 않습니다.** |
| `FinalFields`       | 객체                                                                | 이 트랜잭션의 변경 사항을 적용한 후 원장 개체의 콘텐츠 필드입니다. 존재하는 필드는 생성된 원장 객체의 유형에 따라 달라집니다. `PreviousTxnID`대부분의 원장 개체 유형에 및 `PreviousTxnLgrSeq`필드가 있지만 이 경우에는 및 필드가 생략됩니다.                                                              |
| `PreviousFields`    | 객체                                                                | 이 트랜잭션의 결과로 변경된 객체의 모든 필드에 대한 이전 값입니다. 트랜잭션이 객체에 필드 _만 추가한 경우 이 필드는 빈 객체입니다._                                                                                                                                        |
| `PreviousTxnID`     | 문자열 - [해시](https://xrpl.org/basic-data-types.html#hashes)         | (생략 가능) 이 원장 개체를 수정하기 위한 이전 트랜잭션의 식별 해시입니다. PreviousTxnID 필드가 없는 원장 객체 유형의 경우 생략됩니다.                                                                                                                                 |
| `PreviousTxnLgrSeq` | 번호 - [원장 색인](https://xrpl.org/basic-data-types.html#ledger-index) | (생략 가능) 이 원장 개체를 수정하기 위한 이전 트랜잭션이 포함된 원장 버전의 원장 인덱스입니다. PreviousTxnLgrSeq 필드가 없는 원장 객체 유형의 경우 생략됩니다.                                                                                                                 |

{% hint style="info" %}
**Note**:

수정된 Ledger 항목에 PreviousTxnID 및 PreviousTxnLgrSeq 필드가 있는 경우, 트랜잭션은 항상 트랜잭션의 자체 식별 해시 및 트랜잭션이 포함된 Ledger 버전의 인덱스로 해당 필드를 업데이트하지만, 이러한 필드의 새 값은 ModifiedNode 객체의 FinalFields에 나열되지 않으며 이전 값은 중첩된 PreviousFields 객체가 아닌 ModifiedNode 객체의 최상위 레벨에 나열됩니다.
{% endhint %}

## NFT Fields

NFT와 관련된 트랜잭션(tx 및 account\_tx)은 메타데이터에 다음 필드를 포함할 수 있습니다. 이러한 값은 요청 시점에 클리오서버에 의해 추가되며 해시된 바이너리 메타데이터에 저장되지 않습니다:&#x20;

| 필드            | 값   | 설명                                                                                                                  |
| ------------- | --- | ------------------------------------------------------------------------------------------------------------------- |
| `nftoken_id`  | 문자열 | 트랜잭션의 결과로 원장에 변경된 NFToken의 NFTokenID를 표시합니다. 트랜잭션이 NFTokenMint 또는 NFTokenAcceptOffer인 경우에만 표시됩니다. NFTokenID를 참조하세요. |
| `nftoken_ids` | 배열  | 트랜잭션의 결과로 원장에 변경된 NFToken에 대한 모든 NFTokenID를 표시합니다. 트랜잭션이 NFTokenCancelOffer인 경우에만 표시됩니다.                            |
| `offer_id`    | 문자열 | NFTokenCreateOffer 트랜잭션의 응답으로 새로운 NFTokenOffer의 오퍼 ID를 표시합니다.                                                       |

## delivered\_amount

결제 트랜잭션의 금액은 목적지에 전달할 금액을 나타내므로 트랜잭션이 성공하면 목적지는 해당 금액을 수령하게 됩니다(부분 결제인 경우 제외). (이 경우 금액까지 양수 금액이 도착했을 수 있습니다.) 금액 필드를 신뢰할지 여부를 선택하는 대신 메타데이터의 delivered\_amount 필드를 사용하여 실제로 목적지에 얼마나 많은 금액이 도착했는지 확인해야 합니다.

rippled 서버는 모든 성공적인 결제 트랜잭션에 대해 JSON 트랜잭션 메타데이터에 delivered\_amount 필드를 제공합니다. 이 필드는 일반 화폐 금액과 같은 형식입니다. 그러나 다음 두 가지 기준을 모두 충족하는 트랜잭션에는 전달된 금액을 사용할 수 없습니다:

* 부분 결제.
* 2014-01-20 이전에 확인된 ledger에 포함.

두 조건이 모두 참이면 delivered\_amount에 실제 금액 대신 사용할 수 없는 문자열 값이 포함됩니다. 이 경우 트랜잭션의 메타데이터에서 영향을 받는 노드를 읽어야만 실제 전달된 금액을 파악할 수 있습니다.

{% hint style="info" %}
**Note**:

&#x20;`delivered_amount` field는 요청에 따라 on-demand 방식으로 생성되며 거래 메타데이터의 바이너리 형식에 포함되지 않으며 거래 메타데이터의 해시를 계산할 때 사용되지도 않습니다. 반면, 2014-01-20 이후 부분 결제 트랜잭션의 경우 배달된 금액 필드가 바이너리 형식에 포함됩니다.
{% endhint %}

See also: [Partial Payments](https://xrpl.org/partial-payments.html)
