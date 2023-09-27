# 시간 보류 에스크로 보내기

시간 보류 에스크로 보내기 EscrowCreate 트랜잭션 유형은 특정 시간이 지났을 때만 해제 조건이 되는 에스크로를 만들 수 있습니다. 이렇게 하려면 FinishAfter 필드를 사용하고 Condition 필드를 생략합니다.

## 1. 릴리스 시간 계산하기&#x20;

Ripple 에포크 이후 시간을 전체 초 단위로 지정해야 하며, 이는 UNIX 에포크 이후 946684800초입니다. 예를 들어 2017년 11월 13일 자정(UTC)에 자금을 릴리스하는 경우입니다.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// JavaScript Date() is natively expressed in milliseconds; convert to seconds
const release_date_unix = Math.floor( new Date("2017-11-13T00:00:00Z") / 1000 );
const release_date_ripple = release_date_unix - 946684800;
console.log(release_date_ripple);
// 563846400
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Warning:

먼저 동등한 Ripple 시간으로 변환하지 않고 FinishAfter 필드에 UNIX 시간을 사용하면 잠금 해제 시간이 향후 30년으로 추가 설정됩니다!
{% endhint %}

## 2. EscrowCreate 트랜잭션 제출&#x20;

EscrowCreate 트랜잭션에 서명하고 제출합니다. 트랜잭션의 FinishAfter 필드를 보류된 결제를 해제해야 하는 시간으로 설정합니다. 보류된 결제를 해제하기 위한 유일한 조건으로 시간을 설정하려면 조건 필드를 생략합니다. 받는 사람을 발신자와 동일한 주소일 수 있는 수신자로 설정합니다. 금액을 에스크로할 총 XRP 금액(드롭 단위)으로 설정합니다.

{% hint style="info" %}
Caution:

제어하지 않는 서버에는 절대로 비밀 키를 제출하지 마세요. 네트워크를 통해 암호화되지 않은 비밀 키를 전송하지 마세요.&#x20;
{% endhint %}

요청:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 2,
  "command": "submit",
  "secret": "s████████████████████████████",
  "tx_json": {
      "Account": "rajgkBmMxmz161r8bWYH7CQAFZP5bA9oSG",
      "TransactionType": "EscrowCreate",
      "Amount": "10000",
      "Destination": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
      "FinishAfter": 557020800
  }
}
```
{% endtab %}
{% endtabs %}

응답:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 2,
  "status": "success",
  "type": "response",
  "result": {
    "engine_result": "tesSUCCESS",
    "engine_result_code": 0,
    "engine_result_message": "The transaction was applied. Only final in a validated ledger.",
    "tx_blob": "1200012280000000240000000120252133768061400000000000271068400000000000000A732103C3555B7339FFDDB43495A8371A3A87B4C66B67D49D06CB9BA1FDBFEEB57B6E437446304402203C9AA4C21E1A1A7427D41583283E7A513DDBDD967B246CADD3B2705D858A7A8E02201BEA7B923B18910EEB9F306F6DE3B3F53549BBFAD46335B62B4C34A6DCB4A47681143EEB46C355B04EE8D08E8EED00F422895C79EA6A83144B4E9C06F24296074F7BC48F92A97916C6DC5EA9",
    "tx_json": {
      "Account": "rajgkBmMxmz161r8bWYH7CQAFZP5bA9oSG",
      "Amount": "10000",
      "Destination": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
      "Fee": "10",
      "FinishAfter": 557020800,
      "Flags": 2147483648,
      "Sequence": 1,
      "SigningPubKey": "03C3555B7339FFDDB43495A8371A3A87B4C66B67D49D06CB9BA1FDBFEEB57B6E43",
      "TransactionType": "EscrowCreate",
      "TxnSignature": "304402203C9AA4C21E1A1A7427D41583283E7A513DDBDD967B246CADD3B2705D858A7A8E02201BEA7B923B18910EEB9F306F6DE3B3F53549BBFAD46335B62B4C34A6DCB4A476",
      "hash": "55B2057332F8999208C43BA1E7091B423A16E5ED2736C06300B4076085205263"
    }
  }
}
```
{% endtab %}
{% endtabs %}

트랜잭션이 검증된 ledger 버전에 포함되었을 때 최종 상태를 확인할 수 있도록 트랜잭션의 식별 해시값을 메모해 두세요.

## 3. 검증 대기&#x20;

라이브 네트워크(mainnet, testnet, devnet 포함)에서는 ledger가 자동으로 닫힐 때까지 4\~7초 정도 기다릴 수 있습니다.

Stand-alone에서 rippled을 실행하는 경우, ledger\_accept 메소드를 사용하여 ledger를 수동으로 닫아야 합니다.&#x20;

## 4. 에스크로가 생성되었는지 확인

트랜잭션의 식별 해시와 함께 tx 메소드를 사용해 최종 상태를 확인합니다. 트랜잭션 메타데이터에서 에스크로 ledger 객체를 생성했음을 나타내는 CreatedNode를 찾습니다.

요청:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 3,
  "command": "tx",
  "transaction": "55B2057332F8999208C43BA1E7091B423A16E5ED2736C06300B4076085205263"
}
```
{% endtab %}
{% endtabs %}

응답:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 3,
  "status": "success",
  "type": "response",
  "result": {
    "Account": "rajgkBmMxmz161r8bWYH7CQAFZP5bA9oSG",
    "Amount": "10000",
    "Destination": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "Fee": "10",
    "FinishAfter": 557020800,
    "Flags": 2147483648,
    "Sequence": 1,
    "SigningPubKey": "03C3555B7339FFDDB43495A8371A3A87B4C66B67D49D06CB9BA1FDBFEEB57B6E43",
    "TransactionType": "EscrowCreate",
    "TxnSignature": "304402203C9AA4C21E1A1A7427D41583283E7A513DDBDD967B246CADD3B2705D858A7A8E02201BEA7B923B18910EEB9F306F6DE3B3F53549BBFAD46335B62B4C34A6DCB4A476",
    "date": 557014081,
    "hash": "55B2057332F8999208C43BA1E7091B423A16E5ED2736C06300B4076085205263",
    "inLedger": 1828796,
    "ledger_index": 1828796,
    "meta": {
      "AffectedNodes": [
        {
          "ModifiedNode": {
            "LedgerEntryType": "AccountRoot",
            "LedgerIndex": "13F1A95D7AAB7108D5CE7EEAF504B2894B8C674E6D68499076441C4837282BF8",
            "PreviousTxnID": "613B28E0890FC975F2CBA3D700F75116F623B1E3FE48CB7CB2EB216EAD6F097D",
            "PreviousTxnLgrSeq": 1799920
          }
        },
        {
          "CreatedNode": {
            "LedgerEntryType": "Escrow",
            "LedgerIndex": "2B9845CB9DF686B9615BF04F3EC66095A334D985E03E71B893B90FCF6D4DC9E6",
            "NewFields": {
              "Account": "rajgkBmMxmz161r8bWYH7CQAFZP5bA9oSG",
              "Amount": "10000",
              "Destination": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
              "FinishAfter": 557020800
            }
          }
        },
        {
          "ModifiedNode": {
            "FinalFields": {
              "Account": "rajgkBmMxmz161r8bWYH7CQAFZP5bA9oSG",
              "Balance": "9999989990",
              "Flags": 0,
              "OwnerCount": 1,
              "Sequence": 2
            },
            "LedgerEntryType": "AccountRoot",
            "LedgerIndex": "AE5AB6584A76C37C7382B6880609FC7792D90CDA36FF362AF412EB914C1715D3",
            "PreviousFields": {
              "Balance": "10000000000",
              "OwnerCount": 0,
              "Sequence": 1
            },
            "PreviousTxnID": "F181D45FD094A7417926F791D9DF958B84CE4B7B3D92CC9DDCACB1D5EC59AAAA",
            "PreviousTxnLgrSeq": 1828732
          }
        },
        {
          "CreatedNode": {
            "LedgerEntryType": "DirectoryNode",
            "LedgerIndex": "D623EBEEEE701D4323D0ADA5320AF35EA8CC6520EBBEF69343354CD593DABC88",
            "NewFields": {
              "Owner": "rajgkBmMxmz161r8bWYH7CQAFZP5bA9oSG",
              "RootIndex": "D623EBEEEE701D4323D0ADA5320AF35EA8CC6520EBBEF69343354CD593DABC88"
            }
          }
        }
      ],
      "TransactionIndex": 3,
      "TransactionResult": "tesSUCCESS"
    },
    "validated": true
  }
}
```
{% endtab %}
{% endtabs %}

## 5. 릴리스 시간 대기

FinishAfter 시간이 있는 보류된 결제는 에스크로 노드의 FinishAfter 시간보다 늦은 close\_time 헤더 필드를 가진 ledger가 이미 마감될 때까지 완료할 수 없습니다.

가장 최근에 유효성을 검사한 ledger의 마감 시간은 ledger 메소드를 사용하여 확인할 수 있습니다:

요청:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 4,
  "command": "ledger",
  "ledger_index": "validated"
}
```
{% endtab %}
{% endtabs %}

응답:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 4,
  "status": "success",
  "type": "response",
  "result": {
    "ledger": {
      "accepted": true,
      "account_hash": "3B5A8FF5334F94F4D3D09F236F9D1B4C028FCAE30948ACC986D461DDEE1D886B",
      "close_flags": 0,
      "close_time": 557256670,
      "close_time_human": "2017-Aug-28 17:31:10",
      "close_time_resolution": 10,
      "closed": true,
      "hash": "A999223A80174A7CB39D766B625C9E476F24AD2F15860A712CD029EE5ED1C320",
      "ledger_hash": "A999223A80174A7CB39D766B625C9E476F24AD2F15860A712CD029EE5ED1C320",
      "ledger_index": "1908253",
      "parent_close_time": 557256663,
      "parent_hash": "6A70C5336ACFDA05760D827776079F7A544D2361CFD5B21BD55A92AA20477A61",
      "seqNum": "1908253",
      "totalCoins": "99997280690562728",
      "total_coins": "99997280690562728",
      "transaction_hash": "49A51DFB1CAB2F134D93D5D1C5FF55A15B12DA36DAF9F5862B17C47EE966647D"
    },
    "ledger_hash": "A999223A80174A7CB39D766B625C9E476F24AD2F15860A712CD029EE5ED1C320",
    "ledger_index": 1908253,
    "validated": true
  }
}
```
{% endtab %}
{% endtabs %}

## 6. EscrowFinish 트랜잭션 제출&#x20;

FinishAfter 시간이 지난 후 자금 출금을 실행하려면 EscrowFinish 트랜잭션에 서명하고 제출합니다. 트랜잭션의 소유자 필드를 EscrowCreate 트랜잭션의 계정 주소로 설정하고 오퍼 시퀀스를 EscrowCreate 트랜잭션의 시퀀스 번호로 설정합니다. 시간으로만 보류되는 에스크로의 경우 조건 및 주문 처리 필드를 생략합니다.

{% hint style="info" %}
Tip:

XRP Ledger의 상태는 트랜잭션에 의해서만 수정할 수 있기 때문에 EscrowFinish 트랜잭션이 필요합니다. 이 트랜잭션의 발신자는 에스크로의 수취인, 에스크로의 원래 발신자 또는 기타 XRP Ledger의 주소일 수 있습니다.
{% endhint %}

에스크로가 만료된 경우, 대신 에스크로를 취소할 수만 있습니다.

{% hint style="info" %}
Caution:

제어하지 않는 서버에 비밀 키를 제출하지 마세요. 네트워크를 통해 암호화되지 않은 비밀 키를 전송하지 마세요.&#x20;
{% endhint %}

요청:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 5,
  "command": "submit",
  "secret": "s████████████████████████████",
  "tx_json": {
    "Account": "rajgkBmMxmz161r8bWYH7CQAFZP5bA9oSG",
    "TransactionType": "EscrowFinish",
    "Owner": "rajgkBmMxmz161r8bWYH7CQAFZP5bA9oSG",
    "OfferSequence": 1
  }
}
```
{% endtab %}
{% endtabs %}

응답:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 5,
  "status": "success",
  "type": "response",
  "result": {
    "engine_result": "tesSUCCESS",
    "engine_result_code": 0,
    "engine_result_message": "The transaction was applied. Only final in a validated ledger.",
    "tx_blob": "1200022280000000240000000220190000000168400000000000000A732103C3555B7339FFDDB43495A8371A3A87B4C66B67D49D06CB9BA1FDBFEEB57B6E4374473045022100923B91BA4FD6450813F5335D71C64BA9EB81304A86859A631F2AD8571424A46502200CCE660D36781B84634C5F23619EB6CFCCF942709F54DCCF27CF6F499AE78C9B81143EEB46C355B04EE8D08E8EED00F422895C79EA6A82143EEB46C355B04EE8D08E8EED00F422895C79EA6A",
    "tx_json": {
      "Account": "rajgkBmMxmz161r8bWYH7CQAFZP5bA9oSG",
      "Fee": "10",
      "Flags": 2147483648,
      "OfferSequence": 1,
      "Owner": "rajgkBmMxmz161r8bWYH7CQAFZP5bA9oSG",
      "Sequence": 2,
      "SigningPubKey": "03C3555B7339FFDDB43495A8371A3A87B4C66B67D49D06CB9BA1FDBFEEB57B6E43",
      "TransactionType": "EscrowFinish",
      "TxnSignature": "3045022100923B91BA4FD6450813F5335D71C64BA9EB81304A86859A631F2AD8571424A46502200CCE660D36781B84634C5F23619EB6CFCCF942709F54DCCF27CF6F499AE78C9B",
      "hash": "41856A742B3CAF307E7B4D0B850F302101F0F415B785454F7501E9960A2A1F6B"
    }
  }
}
```
{% endtab %}
{% endtabs %}

트랜잭션이 검증된 ledger 버전에 포함되었을 때 최종 상태를 확인할 수 있도록 트랜잭션의 식별 해시값을 메모해 두세요.

## 7. 검증 대기&#x20;

라이브 네트워크(mainnet, testnet 또는 devnet 포함)에서는 ledger가 자동으로 닫힐 때까지 4\~7초 정도 기다릴 수 있습니다.

Stand-alone 모드에서 Ripple을 실행하는 경우, ledger\_accept 메소드를 사용해 ledger를수동으로 닫아야 합니다.&#x20;

## 8. 최종 결과 확인&#x20;

트랜잭션의 식별 해시와 함께 tx 메소드를 사용해 트랜잭션의 최종 상태를 확인합니다. 특히 트랜잭션 메타데이터에서 에스크로된 결제의 목적지에 대해 AccountRoot 유형의 ModifiedNode가 있는지 확인하세요. 개체의 FinalFields에 잔액 필드에 XRP의 증가가 표시되어야 합니다.

요청:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 21,
  "command": "tx",
  "transaction": "41856A742B3CAF307E7B4D0B850F302101F0F415B785454F7501E9960A2A1F6B"
}
```
{% endtab %}
{% endtabs %}

&#x20;응답:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 21,
  "status": "success",
  "type": "response",
  "result": {
    "Account": "rajgkBmMxmz161r8bWYH7CQAFZP5bA9oSG",
    "Fee": "10",
    "Flags": 2147483648,
    "OfferSequence": 1,
    "Owner": "rajgkBmMxmz161r8bWYH7CQAFZP5bA9oSG",
    "Sequence": 2,
    "SigningPubKey": "03C3555B7339FFDDB43495A8371A3A87B4C66B67D49D06CB9BA1FDBFEEB57B6E43",
    "TransactionType": "EscrowFinish",
    "TxnSignature": "3045022100923B91BA4FD6450813F5335D71C64BA9EB81304A86859A631F2AD8571424A46502200CCE660D36781B84634C5F23619EB6CFCCF942709F54DCCF27CF6F499AE78C9B",
    "date": 557256681,
    "hash": "41856A742B3CAF307E7B4D0B850F302101F0F415B785454F7501E9960A2A1F6B",
    "inLedger": 1908257,
    "ledger_index": 1908257,
    "meta": {
      "AffectedNodes": [
        {
          "ModifiedNode": {
            "FinalFields": {
              "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
              "Balance": "400210000",
              "Flags": 0,
              "OwnerCount": 0,
              "Sequence": 1
            },
            "LedgerEntryType": "AccountRoot",
            "LedgerIndex": "13F1A95D7AAB7108D5CE7EEAF504B2894B8C674E6D68499076441C4837282BF8",
            "PreviousFields": {
              "Balance": "400200000"
            },
            "PreviousTxnID": "55B2057332F8999208C43BA1E7091B423A16E5ED2736C06300B4076085205263",
            "PreviousTxnLgrSeq": 1828796
          }
        },
        {
          "DeletedNode": {
            "FinalFields": {
              "Account": "rajgkBmMxmz161r8bWYH7CQAFZP5bA9oSG",
              "Amount": "10000",
              "Destination": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
              "FinishAfter": 557020800,
              "Flags": 0,
              "OwnerNode": "0000000000000000",
              "PreviousTxnID": "55B2057332F8999208C43BA1E7091B423A16E5ED2736C06300B4076085205263",
              "PreviousTxnLgrSeq": 1828796
            },
            "LedgerEntryType": "Escrow",
            "LedgerIndex": "2B9845CB9DF686B9615BF04F3EC66095A334D985E03E71B893B90FCF6D4DC9E6"
          }
        },
        {
          "ModifiedNode": {
            "FinalFields": {
              "Account": "rajgkBmMxmz161r8bWYH7CQAFZP5bA9oSG",
              "Balance": "9999989980",
              "Flags": 0,
              "OwnerCount": 0,
              "Sequence": 3
            },
            "LedgerEntryType": "AccountRoot",
            "LedgerIndex": "AE5AB6584A76C37C7382B6880609FC7792D90CDA36FF362AF412EB914C1715D3",
            "PreviousFields": {
              "Balance": "9999989990",
              "OwnerCount": 1,
              "Sequence": 2
            },
            "PreviousTxnID": "55B2057332F8999208C43BA1E7091B423A16E5ED2736C06300B4076085205263",
            "PreviousTxnLgrSeq": 1828796
          }
        },
        {
          "ModifiedNode": {
            "FinalFields": {
              "Flags": 0,
              "Owner": "rajgkBmMxmz161r8bWYH7CQAFZP5bA9oSG",
              "RootIndex": "D623EBEEEE701D4323D0ADA5320AF35EA8CC6520EBBEF69343354CD593DABC88"
            },
            "LedgerEntryType": "DirectoryNode",
            "LedgerIndex": "D623EBEEEE701D4323D0ADA5320AF35EA8CC6520EBBEF69343354CD593DABC88"
          }
        }
      ],
      "TransactionIndex": 2,
      "TransactionResult": "tesSUCCESS"
    },
    "validated": true
  }
}
```
{% endtab %}
{% endtabs %}
