# 다중 서명 트랜잭션 전송

다음 절차는 다중 서명된 트랜잭션을 생성, 서명 및 제출하는 방법을 보여줍니다.

## 요구 사항

* 주소에 대한 다중 서명이 이미 설정되어 있어야 합니다.
* 다중 서명을 사용할 수 있어야 합니다. 다중 서명은 2016년 6월 27일부터 XRP Ledger 컨센서스 프로토콜에 대한 수정안으로 활성화되었습니다.

## 1. 트랜잭션 생성 <a href="#1-create-the-transaction" id="1-create-the-transaction"></a>

제출하려는 트랜잭션을 나타내는 JSON 개체를 만듭니다. 수수료 및 순서를 포함하여 이 트랜잭션에 대한 모든 것을 지정해야 합니다. 또한 SigningPubKey 필드를 빈 문자열로 포함하여 트랜잭션이 다중 서명되었음을 나타냅니다.&#x20;

다중 서명 트랜잭션의 수수료는 정규 서명 트랜잭션보다 훨씬 높다는 점을 명심하세요. 정상 거래 비용의 최소 (N+1)배여야 합니다. 여기서 N은 제공하려는 서명 수입니다. 여러 소스에서 서명을 수집하는 데 시간이 걸리는 경우가 있으므로 해당 시간 동안 트랜잭션 비용이 증가하는 경우 현재 최소값보다 더 많이 지정하는 것이 좋습니다.&#x20;

다음은 다중 서명할 준비가 된 트랜잭션의 예입니다.

```json
{
    "TransactionType": "TrustSet",
    "Account": "rEuLyBCvcw4CFmzv8RepSiAoNgF8tTGJQC",
    "Flags": 262144,
    "LimitAmount": {
        "currency": "USD",
        "issuer": "rHb9CJAWyB4rj91VRWn96DkukG4bwdtyTh",
        "value": "100"
    },
    "Sequence": 2,
    "SigningPubKey": "",
    "Fee": "30000"
}
```

(이 거래는 rEuLyBCvcw4CFmzv8RepSiAoNgF8tTGJQC에서 rHb9CJAWyB4rj91VRWn96DkukG4bwdtyTh까지 최대 잔액이 100 USD인 회계 관계를 생성합니다.)

## 2. 하나의 서명 받기 <a href="#2-get-one-signature" id="2-get-one-signature"></a>

SignerList 구성원 중 하나의 비밀 키 및 주소와 함께 sign\_for 메소소드를 사용하여 해당 구성원에 대한 서명을 가져옵니다.

{% hint style="info" %}
Caution:

제어하지 않는 서버에 비밀 키를 제출하지 마세요. 네트워크를 통해 암호화되지 않은 비밀 키를 보내지 마세요.
{% endhint %}

```json
$ rippled sign_for rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW <rsA2L..'s secret> '{
>     "TransactionType": "TrustSet",
>     "Account": "rEuLyBCvcw4CFmzv8RepSiAoNgF8tTGJQC",
>     "Flags": 262144,
>     "LimitAmount": {
>         "currency": "USD",
>         "issuer": "rHb9CJAWyB4rj91VRWn96DkukG4bwdtyTh",
>         "value": "100"
>     },
>     "Sequence": 2,
>     "SigningPubKey": "",
>     "Fee": "30000"
> }'
Loading: "/etc/opt/ripple/rippled.cfg"
Connecting to 127.0.0.1:5005
{
   "result" : {
      "status" : "success",
      "tx_blob" : "1200142200040000240000000263D5038D7EA4C680000000000000000000000000005553440000000000B5F762798A53D543A014CAF8B297CFF8F2F937E868400000000000753073008114A3780F5CB5A44D366520FC44055E8ED44D9A2270F3E010732102B3EC4E5DD96029A647CFA20DA07FE1F85296505552CCAC114087E66B46BD77DF744730450221009C195DBBF7967E223D8626CA19CF02073667F2B22E206727BFE848FF42BEAC8A022048C323B0BED19A988BDBEFA974B6DE8AA9DCAE250AA82BBD1221787032A864E58114204288D2E47F8EF6C99BCC457966320D12409711E1F1",
      "tx_json" : {
         "Account" : "rEuLyBCvcw4CFmzv8RepSiAoNgF8tTGJQC",
         "Fee" : "30000",
         "Flags" : 262144,
         "LimitAmount" : {
            "currency" : "USD",
            "issuer" : "rHb9CJAWyB4rj91VRWn96DkukG4bwdtyTh",
            "value" : "100"
         },
         "Sequence" : 2,
         "Signers" : [
            {
               "Signer" : {
                  "Account" : "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW",
                  "SigningPubKey" : "02B3EC4E5DD96029A647CFA20DA07FE1F85296505552CCAC114087E66B46BD77DF",
                  "TxnSignature" : "30450221009C195DBBF7967E223D8626CA19CF02073667F2B22E206727BFE848FF42BEAC8A022048C323B0BED19A988BDBEFA974B6DE8AA9DCAE250AA82BBD1221787032A864E5"
               }
            }
         ],
         "SigningPubKey" : "",
         "TransactionType" : "TrustSet",
         "hash" : "A94A6417D1A7AAB059822B894E13D322ED3712F7212CE9257801F96DE6C3F6AE"
      }
   }
}
```

`tx_json`응답 필드를 저장하세요. `Signers`필드에 새 서명이 있습니다. `tx_blob`필드 값을 버릴 수 있습니다.

stand-alone 모드 또는 비생산 네트워크에 문제가 있는 경우 다중 서명이 활성화되어 있는지 확인하세요.

## 3. 추가 서명 받기 <a href="#3-get-additional-signatures" id="3-get-additional-signatures"></a>

병렬 또는 직렬로 추가 서명을 수집할 수 있습니다.

* 병렬: 트랜잭션에 대한 원본 JSON과 함께 sign\_for 명령을 사용합니다. 각 응답에는 `Signers` 배열에 단일 서명이 있습니다.&#x20;
* 직렬: 이전 sign\_for 응답의 tx\_json 값과 함께 sign\_for 명령을 사용합니다. 각 응답은 기존 `Signers` 배열에 새 서명을 추가합니다.

{% hint style="info" %}
Caution:

제어하지 않는 서버에 비밀 키를 제출하지 마세요. 네트워크를 통해 암호화되지 않은 비밀 키를 보내지 마세요.
{% endhint %}

```json
$ rippled sign_for rUpy3eEg8rqjqfUoLeBnZkscbKbFsKXC3v <rUpy..'s secret> '{
>    "Account" : "rEuLyBCvcw4CFmzv8RepSiAoNgF8tTGJQC",
>    "Fee" : "30000",
>    "Flags" : 262144,
>    "LimitAmount" : {
>       "currency" : "USD",
>       "issuer" : "rHb9CJAWyB4rj91VRWn96DkukG4bwdtyTh",
>       "value" : "100"
>    },
>    "Sequence" : 2,
>    "Signers" : [
>       {
>          "Signer" : {
>             "Account" : "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW",
>             "SigningPubKey" : "02B3EC4E5DD96029A647CFA20DA07FE1F85296505552CCAC114087E66B46BD77DF",
>             "TxnSignature" : "30450221009C195DBBF7967E223D8626CA19CF02073667F2B22E206727BFE848FF42BEAC8A022048C323B0BED19A988BDBEFA974B6DE8AA9DCAE250AA82BBD1221787032A864E5"
>          }
>       }
>    ],
>    "SigningPubKey" : "",
>    "TransactionType" : "TrustSet",
>    "hash" : "A94A6417D1A7AAB059822B894E13D322ED3712F7212CE9257801F96DE6C3F6AE"
> }'
Loading: "/etc/opt/ripple/rippled.cfg"
Connecting to 127.0.0.1:5005
{
   "result" : {
      "status" : "success",
      "tx_blob" : "1200142200040000240000000263D5038D7EA4C680000000000000000000000000005553440000000000B5F762798A53D543A014CAF8B297CFF8F2F937E868400000000000753073008114A3780F5CB5A44D366520FC44055E8ED44D9A2270F3E010732102B3EC4E5DD96029A647CFA20DA07FE1F85296505552CCAC114087E66B46BD77DF744730450221009C195DBBF7967E223D8626CA19CF02073667F2B22E206727BFE848FF42BEAC8A022048C323B0BED19A988BDBEFA974B6DE8AA9DCAE250AA82BBD1221787032A864E58114204288D2E47F8EF6C99BCC457966320D12409711E1E0107321028FFB276505F9AC3F57E8D5242B386A597EF6C40A7999F37F1948636FD484E25B744630440220680BBD745004E9CFB6B13A137F505FB92298AD309071D16C7B982825188FD1AE022004200B1F7E4A6A84BB0E4FC09E1E3BA2B66EBD32F0E6D121A34BA3B04AD99BC181147908A7F0EDD48EA896C3580A399F0EE78611C8E3E1F1",
      "tx_json" : {
         "Account" : "rEuLyBCvcw4CFmzv8RepSiAoNgF8tTGJQC",
         "Fee" : "30000",
         "Flags" : 262144,
         "LimitAmount" : {
            "currency" : "USD",
            "issuer" : "rHb9CJAWyB4rj91VRWn96DkukG4bwdtyTh",
            "value" : "100"
         },
         "Sequence" : 2,
         "Signers" : [
            {
               "Signer" : {
                  "Account" : "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW",
                  "SigningPubKey" : "02B3EC4E5DD96029A647CFA20DA07FE1F85296505552CCAC114087E66B46BD77DF",
                  "TxnSignature" : "30450221009C195DBBF7967E223D8626CA19CF02073667F2B22E206727BFE848FF42BEAC8A022048C323B0BED19A988BDBEFA974B6DE8AA9DCAE250AA82BBD1221787032A864E5"
               }
            },
            {
               "Signer" : {
                  "Account" : "rUpy3eEg8rqjqfUoLeBnZkscbKbFsKXC3v",
                  "SigningPubKey" : "028FFB276505F9AC3F57E8D5242B386A597EF6C40A7999F37F1948636FD484E25B",
                  "TxnSignature" : "30440220680BBD745004E9CFB6B13A137F505FB92298AD309071D16C7B982825188FD1AE022004200B1F7E4A6A84BB0E4FC09E1E3BA2B66EBD32F0E6D121A34BA3B04AD99BC1"
               }
            }
         ],
         "SigningPubKey" : "",
         "TransactionType" : "TrustSet",
         "hash" : "BD636194C48FD7A100DE4C972336534C8E710FD008C0F3CF7BC5BF34DAF3C3E6"
      }
   }
}
```

구성한 SignerList에 따라 필요한 모든 당사자로부터 서명을 받으려면 이 단계를 여러 번 반복해야 할 수 있습니다.

### 4. 서명 결합 및 제출 <a href="#4-combine-signatures-and-submit" id="4-combine-signatures-and-submit"></a>

서명을 직렬로 수집한 경우 마지막 `sign_for` 응답의 `tx_json`에 모든 서명이있으므로 이를 submit\_multisigned 메소드의 인수로 사용할 수 있습니다.&#x20;

서명을 병렬로 수집한 경우 모든 서명이 포함된 `tx_json` 개체를 수동으로 생성해야 합니다. 모든 sign\_for 응답에서 서명자 배열을 가져오고 해당 내용을 각 서명이 있는 단일 서명자 배열로 결합합니다. 결합된 서명자 배열을 원래 트랜잭션 JSON 값에 추가하고 이를 submit\_multisigned 메소드에 대한 인수로 사용합니다.

```json
$ rippled submit_multisigned '{
>              "Account" : "rEuLyBCvcw4CFmzv8RepSiAoNgF8tTGJQC",
>              "Fee" : "30000",
>              "Flags" : 262144,
>              "LimitAmount" : {
>                 "currency" : "USD",
>                 "issuer" : "rHb9CJAWyB4rj91VRWn96DkukG4bwdtyTh",
>                 "value" : "100"
>              },
>              "Sequence" : 2,
>              "Signers" : [
>                 {
>                    "Signer" : {
>                       "Account" : "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW",
>                       "SigningPubKey" : "02B3EC4E5DD96029A647CFA20DA07FE1F85296505552CCAC114087E66B46BD77DF",
>                       "TxnSignature" : "30450221009C195DBBF7967E223D8626CA19CF02073667F2B22E206727BFE848FF42BEAC8A022048C323B0BED19A988BDBEFA974B6DE8AA9DCAE250AA82BBD1221787032A864E5"
>                    }
>                 },
>                 {
>                    "Signer" : {
>                       "Account" : "rUpy3eEg8rqjqfUoLeBnZkscbKbFsKXC3v",
>                       "SigningPubKey" : "028FFB276505F9AC3F57E8D5242B386A597EF6C40A7999F37F1948636FD484E25B",
>                       "TxnSignature" : "30440220680BBD745004E9CFB6B13A137F505FB92298AD309071D16C7B982825188FD1AE022004200B1F7E4A6A84BB0E4FC09E1E3BA2B66EBD32F0E6D121A34BA3B04AD99BC1"
>                    }
>                 }
>              ],
>              "SigningPubKey" : "",
>              "TransactionType" : "TrustSet",
>              "hash" : "BD636194C48FD7A100DE4C972336534C8E710FD008C0F3CF7BC5BF34DAF3C3E6"
>           }'
Loading: "/etc/opt/ripple/rippled.cfg"
Connecting to 127.0.0.1:5005
{
    "result": {
        "engine_result": "tesSUCCESS",
        "engine_result_code": 0,
        "engine_result_message": "The transaction was applied. Only final in a validated ledger.",
        "status": "success",
        "tx_blob": "1200142200040000240000000263D5038D7EA4C680000000000000000000000000005553440000000000B5F762798A53D543A014CAF8B297CFF8F2F937E868400000000000753073008114A3780F5CB5A44D366520FC44055E8ED44D9A2270F3E010732102B3EC4E5DD96029A647CFA20DA07FE1F85296505552CCAC114087E66B46BD77DF744730450221009C195DBBF7967E223D8626CA19CF02073667F2B22E206727BFE848FF42BEAC8A022048C323B0BED19A988BDBEFA974B6DE8AA9DCAE250AA82BBD1221787032A864E58114204288D2E47F8EF6C99BCC457966320D12409711E1E0107321028FFB276505F9AC3F57E8D5242B386A597EF6C40A7999F37F1948636FD484E25B744630440220680BBD745004E9CFB6B13A137F505FB92298AD309071D16C7B982825188FD1AE022004200B1F7E4A6A84BB0E4FC09E1E3BA2B66EBD32F0E6D121A34BA3B04AD99BC181147908A7F0EDD48EA896C3580A399F0EE78611C8E3E1F1",
        "tx_json": {
            "Account": "rEuLyBCvcw4CFmzv8RepSiAoNgF8tTGJQC",
            "Fee": "30000",
            "Flags": 262144,
            "LimitAmount": {
                "currency": "USD",
                "issuer": "rHb9CJAWyB4rj91VRWn96DkukG4bwdtyTh",
                "value": "100"
            },
            "Sequence": 2,
            "Signers": [{
                "Signer": {
                    "Account": "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW",
                    "SigningPubKey": "02B3EC4E5DD96029A647CFA20DA07FE1F85296505552CCAC114087E66B46BD77DF",
                    "TxnSignature": "30450221009C195DBBF7967E223D8626CA19CF02073667F2B22E206727BFE848FF42BEAC8A022048C323B0BED19A988BDBEFA974B6DE8AA9DCAE250AA82BBD1221787032A864E5"
                }
            }, {
                "Signer": {
                    "Account": "rUpy3eEg8rqjqfUoLeBnZkscbKbFsKXC3v",
                    "SigningPubKey": "028FFB276505F9AC3F57E8D5242B386A597EF6C40A7999F37F1948636FD484E25B",
                    "TxnSignature": "30440220680BBD745004E9CFB6B13A137F505FB92298AD309071D16C7B982825188FD1AE022004200B1F7E4A6A84BB0E4FC09E1E3BA2B66EBD32F0E6D121A34BA3B04AD99BC1"
                }
            }],
            "SigningPubKey": "",
            "TransactionType": "TrustSet",
            "hash": "BD636194C48FD7A100DE4C972336534C8E710FD008C0F3CF7BC5BF34DAF3C3E6"
        }
    }
}
```

`hash`나중에 트랜잭션 결과를 확인할 수 있도록 응답의 값을 기록해 둡니다. (이 경우 해시는`BD636194C48FD7A100DE4C972336534C8E710FD008C0F3CF7BC5BF34DAF3C3E6`입니다.)

## 5. Ledger 닫기 <a href="#5-close-the-ledger" id="5-close-the-ledger"></a>

라이브 네트워크를 사용하는 경우 ledger가 자동으로 닫힐 때까지 4-7초를 기다릴 수 있습니다.

`rippled`stand-alone 모드에서 실행 중인 경우 ledger\_accept 메소드를 사용하여 ledger를 수동으로 닫습니다:

```json
$ rippled ledger_accept
Loading: "/etc/opt/ripple/rippled.cfg"
Connecting to 127.0.0.1:5005
{
   "result" : {
      "ledger_current_index" : 7,
      "status" : "success"
   }
}
```

## 6. 거래 결과 확인 <a href="#6-confirm-transaction-results" id="6-confirm-transaction-results"></a>

submit\_multisigned 명령에 대한 응답의 해시 값을 사용하여 tx 메소드를 사용하여 트랜잭션을 조회합니다. 특히 TransactionResult가 문자열 tesSUCCESS인지 확인하세요.

라이브 네트워크에서 검증된 필드가 boolean이 true로 설정되었는지도 확인해야 합니다. 필드가 참이 아닌 경우 컨센서스 프로세스가 완료될 때까지 더 오래 기다려야 할 수 있습니다. 또는 거래가 어떤 이유로 ledger에 포함되지 않을 수 있습니다.

stand-alone 모드에서 서버는 ledger가 수동으로 닫힌 경우 자동으로 유효성이 검증된 것으로 간주합니다.

```json
$ rippled tx BD636194C48FD7A100DE4C972336534C8E710FD008C0F3CF7BC5BF34DAF3C3E6
Loading: "/etc/opt/ripple/rippled.cfg"
Connecting to 127.0.0.1:5005
{
    "result": {
        "Account": "rEuLyBCvcw4CFmzv8RepSiAoNgF8tTGJQC",
        "Fee": "30000",
        "Flags": 262144,
        "LimitAmount": {
            "currency": "USD",
            "issuer": "rHb9CJAWyB4rj91VRWn96DkukG4bwdtyTh",
            "value": "100"
        },
        "Sequence": 2,
        "Signers": [{
            "Signer": {
                "Account": "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW",
                "SigningPubKey": "02B3EC4E5DD96029A647CFA20DA07FE1F85296505552CCAC114087E66B46BD77DF",
                "TxnSignature": "30450221009C195DBBF7967E223D8626CA19CF02073667F2B22E206727BFE848FF42BEAC8A022048C323B0BED19A988BDBEFA974B6DE8AA9DCAE250AA82BBD1221787032A864E5"
            }
        }, {
            "Signer": {
                "Account": "rUpy3eEg8rqjqfUoLeBnZkscbKbFsKXC3v",
                "SigningPubKey": "028FFB276505F9AC3F57E8D5242B386A597EF6C40A7999F37F1948636FD484E25B",
                "TxnSignature": "30440220680BBD745004E9CFB6B13A137F505FB92298AD309071D16C7B982825188FD1AE022004200B1F7E4A6A84BB0E4FC09E1E3BA2B66EBD32F0E6D121A34BA3B04AD99BC1"
            }
        }],
        "SigningPubKey": "",
        "TransactionType": "TrustSet",
        "date": 512172510,
        "hash": "BD636194C48FD7A100DE4C972336534C8E710FD008C0F3CF7BC5BF34DAF3C3E6",
        "inLedger": 6,
        "ledger_index": 6,
        "meta": {
            "AffectedNodes": [{
                "ModifiedNode": {
                    "LedgerEntryType": "AccountRoot",
                    "LedgerIndex": "2B6AC232AA4C4BE41BF49D2459FA4A0347E1B543A4C92FCEE0821C0201E2E9A8",
                    "PreviousTxnID": "B7E1D33DB7DEA3BB65BFAB2C80E02125F47FCCF6C957A7FDECD915B3EBE0C1DD",
                    "PreviousTxnLgrSeq": 4
                }
            }, {
                "CreatedNode": {
                    "LedgerEntryType": "RippleState",
                    "LedgerIndex": "93E317B32022977C77810A2C558FBB28E30E744C68E73720622B797F957EC5FA",
                    "NewFields": {
                        "Balance": {
                            "currency": "USD",
                            "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                            "value": "0"
                        },
                        "Flags": 2162688,
                        "HighLimit": {
                            "currency": "USD",
                            "issuer": "rHb9CJAWyB4rj91VRWn96DkukG4bwdtyTh",
                            "value": "0"
                        },
                        "LowLimit": {
                            "currency": "USD",
                            "issuer": "rEuLyBCvcw4CFmzv8RepSiAoNgF8tTGJQC",
                            "value": "100"
                        }
                    }
                }
            }, {
                "ModifiedNode": {
                    "FinalFields": {
                        "Account": "rEuLyBCvcw4CFmzv8RepSiAoNgF8tTGJQC",
                        "Balance": "999960000",
                        "Flags": 0,
                        "OwnerCount": 6,
                        "Sequence": 3
                    },
                    "LedgerEntryType": "AccountRoot",
                    "LedgerIndex": "A6B1BA6F2D70813100908EA84ABB7783695050312735E2C3665259F388804EA0",
                    "PreviousFields": {
                        "Balance": "999990000",
                        "OwnerCount": 5,
                        "Sequence": 2
                    },
                    "PreviousTxnID": "8FDC18960455C196A8C4DE0D24799209A21F4A17E32102B5162BD79466B90222",
                    "PreviousTxnLgrSeq": 5
                }
            }, {
                "ModifiedNode": {
                    "FinalFields": {
                        "Flags": 0,
                        "Owner": "rEuLyBCvcw4CFmzv8RepSiAoNgF8tTGJQC",
                        "RootIndex": "C2728175908D82FB1DE6676F203D8D3C056995A9FA9B369EF326523F1C65A1DE"
                    },
                    "LedgerEntryType": "DirectoryNode",
                    "LedgerIndex": "C2728175908D82FB1DE6676F203D8D3C056995A9FA9B369EF326523F1C65A1DE"
                }
            }, {
                "CreatedNode": {
                    "LedgerEntryType": "DirectoryNode",
                    "LedgerIndex": "D8120FC732737A2CF2E9968FDF3797A43B457F2A81AA06D2653171A1EA635204",
                    "NewFields": {
                        "Owner": "rHb9CJAWyB4rj91VRWn96DkukG4bwdtyTh",
                        "RootIndex": "D8120FC732737A2CF2E9968FDF3797A43B457F2A81AA06D2653171A1EA635204"
                    }
                }
            }],
            "TransactionIndex": 0,
            "TransactionResult": "tesSUCCESS"
        },
        "status": "success",
        "validated": true
    }
}
```
