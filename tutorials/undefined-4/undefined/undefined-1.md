# 조건부 보류 에스크로 보내기

## 1. 조건 및 이행 생성&#x20;

XRP ledger 에스크로에는 PREIMAGE-SHA-256 암호화 조건이 필요합니다. 적절한 형식으로 조건과 이행을 계산하려면 5개 종 조건과 같은 암호화 조건 라이브러리를 사용해야 합니다. 이행을 생성합니다:

* 암호학적으로 안전한 무작위 소스를 사용하여 최소 32바이트의 무작위 바이트를 생성합니다.&#x20;
* Interledger 프로토콜의 PSK 사양을 따르고 ILP 패킷의 HMAC-SHA-256을 이행으로 사용합니다.&#x20;

무작위 이행 및 조건에 대한 코드 예시:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const cc = require('five-bells-condition')
const crypto = require('crypto')

const preimageData = crypto.randomBytes(32)
const fulfillment = new cc.PreimageSha256()
fulfillment.setPreimage(preimageData)

const condition = fulfillment.getConditionBinary().toString('hex').toUpperCase()
console.log('Condition:', condition)

// Keep secret until you want to finish the escrow
const fulfillment_hex = fulfillment.serializeBinary().toString('hex').toUpperCase()
console.log('Fulfillment:', fulfillment_hex)
```
{% endtab %}

{% tab title="Python" %}
```python
from os import urandom
from cryptoconditions import PreimageSha256

secret = urandom(32)

fulfillment = PreimageSha256(preimage=secret)

print("Condition", fulfillment.condition_binary.hex().upper())

# Keep secret until you want to finish the escrow
print("Fulfillment", fulfillment.serialize_binary().hex().upper())
```
{% endtab %}
{% endtabs %}

나중에 사용할 수 있도록 조건과 주문 처리를 저장합니다. 보류된 결제 처리를 완료할 때까지 주문 처리를 비밀로 유지해야 합니다. 이행을 아는 사람은 누구나 에스크로를 완료하여 보류된 금액을 원하는 목적지로 릴리스할 수 있습니다.

## 2. 해제 또는 취소 시간 계산&#x20;

조건부 에스크로 거래에는 취소 후 또는 완료 후 필드 또는 둘 다 포함되어야 합니다. 취소 후 필드를 사용하면 지정된 시간까지 조건이 충족되지 않으면 XRP가 발신자에게 반환됩니다. FinishAfter 필드는 누군가가 올바른 이행을 보내더라도 에스크로가 실행될 수 없는 시간을 지정합니다. 어떤 필드를 입력하든 지정하는 시간은 미래여야 합니다.

취소 후 시간을 24시간 후로 설정하는 예시입니다:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const rippleOffset = 946684800
const CancelAfter = Math.floor(Date.now() / 1000) + (24*60*60) - rippleOffset
console.log(CancelAfter)
// Example: 556927412
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Warning:

XRP ledger에서 시간을 Ripple 에포크 이후 초 단위로 지정해야 합니다. 취소 후 또는 완료 후 필드에 변환하지 않고 UNIX 시간을 사용하면 잠금 해제 시간이 30년 후로 추가 설정됩니다!
{% endhint %}

## EscrowCreate 트랜잭션 제출

EscrowCreate 트랜잭션에 서명하고 제출합니다. 트랜잭션의 조건 필드를 보류된 결제가 해제되어야 하는 시간으로 설정합니다. 수신자를 발신자와 동일한 주소로 설정합니다. 이전 단계에서 계산한 취소 후 또는 완료 후 시간을 포함합니다. 금액을 에스크로할 총 XRP 금액(드롭 단위)으로 설정합니다.

{% hint style="info" %}
Caution:

제어하지 않는 서버에는 절대로 비밀 키를 제출하지 마세요. 네트워크를 통해 암호화되지 않은 비밀 키를 전송하지 마세요.
{% endhint %}

요청:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 1,
  "command": "submit",
  "secret": "s████████████████████████████",
  "tx_json": {
    "Account": "rEhw9vD98ZrkY4tZPvkZst5H18RysqFdaB",
    "TransactionType": "EscrowCreate",
    "Amount": "100000",
    "Destination": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "Condition": "A0258020E24D9E1473D4DF774F6D8E089067282034E4FA7ECACA2AD2E547953B2C113CBD810120",
    "CancelAfter": 556927412
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
  "id": 1,
  "status": "success",
  "type": "response",
  "result": {
    "engine_result": "tesSUCCESS",
    "engine_result_code": 0,
    "engine_result_message": "The transaction was applied. Only final in a validated ledger.",
    "tx_blob": "120001228000000024000000052024213209B46140000000000186A068400000000000000A732103E498E35BC1E109C5995BD3AB0A6D4FFAB61B853C8F6010FABC5DABAF34478B61744730450221008AC8BDC2151D5EF956197F0E6E89A4F49DEADC1AC38367870E444B1EA8D88D97022075E31427B455DFF87F0F22B849C71FC3987A91C19D63B6D0242E808347EC8A8F701127A0258020E24D9E1473D4DF774F6D8E089067282034E4FA7ECACA2AD2E547953B2C113CBD81012081149A2AA667E1517EFA8A6B552AB2EDB859A99F26B283144B4E9C06F24296074F7BC48F92A97916C6DC5EA9",
    "tx_json": {
      "Account": "rEhw9vD98ZrkY4tZPvkZst5H18RysqFdaB",
      "Amount": "100000",
      "CancelAfter": 556927412,
      "Condition": "A0258020E24D9E1473D4DF774F6D8E089067282034E4FA7ECACA2AD2E547953B2C113CBD810120",
      "Destination": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
      "Fee": "10",
      "Flags": 2147483648,
      "Sequence": 5,
      "SigningPubKey": "03E498E35BC1E109C5995BD3AB0A6D4FFAB61B853C8F6010FABC5DABAF34478B61",
      "TransactionType": "EscrowCreate",
      "TxnSignature": "30450221008AC8BDC2151D5EF956197F0E6E89A4F49DEADC1AC38367870E444B1EA8D88D97022075E31427B455DFF87F0F22B849C71FC3987A91C19D63B6D0242E808347EC8A8F",
      "hash": "E22D1F6EB006CAD35E0DBD3B4F3748427055E4C143EBE95AA6603823AEEAD324"
    }
  }
}
```
{% endtab %}
{% endtabs %}

## 4. 유효성 검사 대기&#x20;

라이브 네트워크(mainnet, testnet 또는 devnet 포함)에서는 ledger가 자동으로 닫힐 때까지 4\~7초 정도 기다릴 수 있습니다.

Stand-alone 모드에서 rippled를 실행하는 경우, ledger\_accept 메소드를 사용하여 ledger을 수동으로 닫아야 합니다.&#x20;

## 5. 에스크로가 생성되었는지 확인

트랜잭션의 식별 해시와 함께 tx 메서드를 사용해 최종 상태를 확인합니다. 특히 트랜잭션 메타데이터에서 에스크로 ledger 객체를 생성했음을 나타내는 CreatedNode를 찾아보세요.

요청:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 3,
  "command": "tx",
  "transaction": "E22D1F6EB006CAD35E0DBD3B4F3748427055E4C143EBE95AA6603823AEEAD324"
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
    "Account": "rEhw9vD98ZrkY4tZPvkZst5H18RysqFdaB",
    "Amount": "100000",
    "CancelAfter": 556927412,
    "Condition": "A0258020E24D9E1473D4DF774F6D8E089067282034E4FA7ECACA2AD2E547953B2C113CBD810120",
    "Destination": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "Fee": "10",
    "Flags": 2147483648,
    "Sequence": 5,
    "SigningPubKey": "03E498E35BC1E109C5995BD3AB0A6D4FFAB61B853C8F6010FABC5DABAF34478B61",
    "TransactionType": "EscrowCreate",
    "TxnSignature": "30450221008AC8BDC2151D5EF956197F0E6E89A4F49DEADC1AC38367870E444B1EA8D88D97022075E31427B455DFF87F0F22B849C71FC3987A91C19D63B6D0242E808347EC8A8F",
    "date": 556841101,
    "hash": "E22D1F6EB006CAD35E0DBD3B4F3748427055E4C143EBE95AA6603823AEEAD324",
    "inLedger": 1772019,
    "ledger_index": 1772019,
    "meta": {
      "AffectedNodes": [
        {
          "ModifiedNode": {
            "LedgerEntryType": "AccountRoot",
            "LedgerIndex": "13F1A95D7AAB7108D5CE7EEAF504B2894B8C674E6D68499076441C4837282BF8",
            "PreviousTxnID": "52C4F626FE6F33699B6BE8ADF362836DDCE9B0B1294BFAA15D65D61501350BE6",
            "PreviousTxnLgrSeq": 1771204
          }
        },
        {
          "ModifiedNode": {
            "FinalFields": {
              "Flags": 0,
              "Owner": "rEhw9vD98ZrkY4tZPvkZst5H18RysqFdaB",
              "RootIndex": "4B4EBB6D8563075813D47491CC325865DFD3DC2E94889F0F39D59D9C059DD81F"
            },
            "LedgerEntryType": "DirectoryNode",
            "LedgerIndex": "4B4EBB6D8563075813D47491CC325865DFD3DC2E94889F0F39D59D9C059DD81F"
          }
        },
        {
          "ModifiedNode": {
            "FinalFields": {
              "Account": "rEhw9vD98ZrkY4tZPvkZst5H18RysqFdaB",
              "Balance": "9999798970",
              "Flags": 0,
              "OwnerCount": 1,
              "Sequence": 6
            },
            "LedgerEntryType": "AccountRoot",
            "LedgerIndex": "5F3B7107F4B524367A173A2B0EAB66E8CC4D2178C1B0C0528CB2F73A8B6BF254",
            "PreviousFields": {
              "Balance": "9999898980",
              "OwnerCount": 0,
              "Sequence": 5
            },
            "PreviousTxnID": "52C4F626FE6F33699B6BE8ADF362836DDCE9B0B1294BFAA15D65D61501350BE6",
            "PreviousTxnLgrSeq": 1771204
          }
        },
        {
          "CreatedNode": {
            "LedgerEntryType": "Escrow",
            "LedgerIndex": "E2CF730A31FD419382350C9DBD8DB7CD775BA5AA9B97A9BE9AB07304AA217A75",
            "NewFields": {
              "Account": "rEhw9vD98ZrkY4tZPvkZst5H18RysqFdaB",
              "Amount": "100000",
              "CancelAfter": 556927412,
              "Condition": "A0258020E24D9E1473D4DF774F6D8E089067282034E4FA7ECACA2AD2E547953B2C113CBD810120",
              "Destination": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn"
            }
          }
        }
      ],
      "TransactionIndex": 0,
      "TransactionResult": "tesSUCCESS"
    },
    "validated": true
  }
}
```
{% endtab %}
{% endtabs %}

## 6. EscrowFinish 트랜잭션 제출&#x20;

FinishAfter 시간이 경과한 후 자금 릴리스를 실행하려면 EscrowFinish 트랜잭션에 서명하고 제출합니다. 트랜잭션의 소유자 필드를 EscrowCreate 트랜잭션의 계정 주소로 설정하고 오퍼 시퀀스를 EscrowCreate 트랜잭션의 시퀀스 번호로 설정합니다. 조건 및 주문 처리 필드를 1단계에서 생성한 조건 및 주문 처리 값(16진수)으로 설정합니다. 조건부 EscrowFinish에는 최소 330드랍의 XRP에 16바이트당 10드랍을 더한 16바이트의 주문 처리 크기를 기준으로 수수료(트랜잭션 비용) 값을 설정합니다.

{% hint style="info" %}
Note:

EscrowCreate 트랜잭션에 FinishAfter 필드를 포함했다면, 에스크로 조건에 맞는 이행을 제공하더라도 해당 시간이 지나기 전에는 트랜잭션을 실행할 수 없습니다. 이전에 마감한 ledger의 마감 시간이 FinishAfter 시간 이전인 경우 EscrowFinish 트랜잭션은 결과 코드 tecNO\_PERMISSION과 함께 실패합니다.
{% endhint %}

에스크로가 만료된 경우 에스크로를 취소만 가능합니다.

{% hint style="info" %}
Cuation:

제어하지 않는 서버에는 절대로 비밀 키를 제출하지 마세요. 네트워크를 통해 암호화되지 않은 비밀 키를 전송하지 마세요.
{% endhint %}

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 4,
  "command": "submit",
  "secret": "s████████████████████████████",
  "tx_json": {
    "Account": "rEhw9vD98ZrkY4tZPvkZst5H18RysqFdaB",
    "TransactionType": "EscrowFinish",
    "Owner": "rEhw9vD98ZrkY4tZPvkZst5H18RysqFdaB",
    "OfferSequence": 5,
    "Condition": "A0258020E24D9E1473D4DF774F6D8E089067282034E4FA7ECACA2AD2E547953B2C113CBD810120",
    "Fulfillment": "A0228020D280D1A02BAD0D2EBC0528B92E9BF37AC3E2530832C2C52620307135156F1048",
    "Fee": "500"
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
  "id": 4,
  "status": "success",
  "type": "response",
  "result": {
    "engine_result": "tesSUCCESS",
    "engine_result_code": 0,
    "engine_result_message": "The transaction was applied. Only final in a validated ledger.",
    "tx_blob": "120002228000000024000000062019000000056840000000000001F4732103E498E35BC1E109C5995BD3AB0A6D4FFAB61B853C8F6010FABC5DABAF34478B617446304402207DE4EA9C8655E75BA01F96345B3F62074313EB42C15D9C4871E30F02202D2BA50220070E52AD308A31AC71E33BA342F31B68D1D1B2A7A3A3ED6E8552CA3DCF14FBB2701024A0228020D280D1A02BAD0D2EBC0528B92E9BF37AC3E2530832C2C52620307135156F1048701127A0258020E24D9E1473D4DF774F6D8E089067282034E4FA7ECACA2AD2E547953B2C113CBD81012081149A2AA667E1517EFA8A6B552AB2EDB859A99F26B282149A2AA667E1517EFA8A6B552AB2EDB859A99F26B2",
    "tx_json": {
      "Account": "rEhw9vD98ZrkY4tZPvkZst5H18RysqFdaB",
      "Condition": "A0258020E24D9E1473D4DF774F6D8E089067282034E4FA7ECACA2AD2E547953B2C113CBD810120",
      "Fee": "500",
      "Flags": 2147483648,
      "Fulfillment": "A0228020D280D1A02BAD0D2EBC0528B92E9BF37AC3E2530832C2C52620307135156F1048",
      "OfferSequence": 5,
      "Owner": "rEhw9vD98ZrkY4tZPvkZst5H18RysqFdaB",
      "Sequence": 6,
      "SigningPubKey": "03E498E35BC1E109C5995BD3AB0A6D4FFAB61B853C8F6010FABC5DABAF34478B61",
      "TransactionType": "EscrowFinish",
      "TxnSignature": "304402207DE4EA9C8655E75BA01F96345B3F62074313EB42C15D9C4871E30F02202D2BA50220070E52AD308A31AC71E33BA342F31B68D1D1B2A7A3A3ED6E8552CA3DCF14FBB2",
      "hash": "0E88368CAFC69A722ED829FAE6E2DD3575AE9C192691E60B5ACDF706E219B2BF"
    }
  }
}
```
{% endtab %}
{% endtabs %}

트랜잭션이 검증된 ledger 버전에 포함되었을 때 최종 상태를 확인할 수 있도록 트랜잭션의 식별 해시값을 메모해 두세요.

## 7. 검증 대기&#x20;

라이브 네트워크(mainnet, testnet 또는 devnet 포함)에서는 ledger가 자동으로 닫힐 때까지 4\~7초 정도 기다릴 수 있습니다.

Stand-alone 모드에서 Ripple을 실행하는 경우, ledger\_accept 메소드를 사용해 ledger를를 수동으로 닫아야 합니다.&#x20;

## 8. 최종 결과 확인&#x20;

트랜잭션의 식별 해시와 함께 tx 메서드를 사용해 트랜잭션의 최종 상태를 확인합니다. 특히 트랜잭션 메타데이터에서 에스크로된 결제의 목적지에 대해 AccountRoot 유형의 ModifiedNode를 찾아보세요. 객체의 FinalFields에 잔액 필드에 XRP가 증가했음을 표시해야 합니다.

요청:

```json
{
  "id": 20,
  "command": "tx",
  "transaction": "0E88368CAFC69A722ED829FAE6E2DD3575AE9C192691E60B5ACDF706E219B2BF"
}
```

응답:

```json
{
  "id": 20,
  "status": "success",
  "type": "response",
  "result": {
    "Account": "rEhw9vD98ZrkY4tZPvkZst5H18RysqFdaB",
    "Condition": "A0258020E24D9E1473D4DF774F6D8E089067282034E4FA7ECACA2AD2E547953B2C113CBD810120",
    "Fee": "500",
    "Flags": 2147483648,
    "Fulfillment": "A0228020D280D1A02BAD0D2EBC0528B92E9BF37AC3E2530832C2C52620307135156F1048",
    "OfferSequence": 2,
    "Owner": "rEhw9vD98ZrkY4tZPvkZst5H18RysqFdaB",
    "Sequence": 4,
    "SigningPubKey": "03E498E35BC1E109C5995BD3AB0A6D4FFAB61B853C8F6010FABC5DABAF34478B61",
    "TransactionType": "EscrowFinish",
    "TxnSignature": "3045022100925FEBE21C2E57F81C472A4E5869CAB1D0164C472A46532F39F6F9F7ED6846D002202CF9D9063ADC4CC0ADF4C4692B7EE165C5D124CAA855649389E245D993F41D4D",
    "date": 556838610,
    "hash": "0E88368CAFC69A722ED829FAE6E2DD3575AE9C192691E60B5ACDF706E219B2BF",
    "inLedger": 1771204,
    "ledger_index": 1771204,
    "meta": {
      "AffectedNodes": [
        {
          "ModifiedNode": {
            "FinalFields": {
              "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
              "Balance": "400100000",
              "Flags": 0,
              "OwnerCount": 0,
              "Sequence": 1
            },
            "LedgerEntryType": "AccountRoot",
            "LedgerIndex": "13F1A95D7AAB7108D5CE7EEAF504B2894B8C674E6D68499076441C4837282BF8",
            "PreviousFields": {
              "Balance": "400000000"
            },
            "PreviousTxnID": "795CBC8AFAAB9DC7BD9944C7FAEABF9BB0802A84520BC649213AD6A2C3256C95",
            "PreviousTxnLgrSeq": 1770775
          }
        },
        {
          "ModifiedNode": {
            "FinalFields": {
              "Flags": 0,
              "Owner": "rEhw9vD98ZrkY4tZPvkZst5H18RysqFdaB",
              "RootIndex": "4B4EBB6D8563075813D47491CC325865DFD3DC2E94889F0F39D59D9C059DD81F"
            },
            "LedgerEntryType": "DirectoryNode",
            "LedgerIndex": "4B4EBB6D8563075813D47491CC325865DFD3DC2E94889F0F39D59D9C059DD81F"
          }
        },
        {
          "ModifiedNode": {
            "FinalFields": {
              "Account": "rEhw9vD98ZrkY4tZPvkZst5H18RysqFdaB",
              "Balance": "9999898980",
              "Flags": 0,
              "OwnerCount": 0,
              "Sequence": 5
            },
            "LedgerEntryType": "AccountRoot",
            "LedgerIndex": "5F3B7107F4B524367A173A2B0EAB66E8CC4D2178C1B0C0528CB2F73A8B6BF254",
            "PreviousFields": {
              "Balance": "9999899480",
              "OwnerCount": 1,
              "Sequence": 4
            },
            "PreviousTxnID": "5C2A1E7B209A7404D3722A010D331A8C1C853109A47DDF620DE5E3D59F026581",
            "PreviousTxnLgrSeq": 1771042
          }
        },
        {
          "DeletedNode": {
            "FinalFields": {
              "Account": "rEhw9vD98ZrkY4tZPvkZst5H18RysqFdaB",
              "Amount": "100000",
              "Condition": "A0258020E24D9E1473D4DF774F6D8E089067282034E4FA7ECACA2AD2E547953B2C113CBD810120",
              "Destination": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
              "FinishAfter": 556838185,
              "Flags": 0,
              "OwnerNode": "0000000000000000",
              "PreviousTxnID": "795CBC8AFAAB9DC7BD9944C7FAEABF9BB0802A84520BC649213AD6A2C3256C95",
              "PreviousTxnLgrSeq": 1770775
            },
            "LedgerEntryType": "Escrow",
            "LedgerIndex": "DC524D17B3F650E7A215B332F418E54AE59B0DFC5392E74958B0037AFDFE8C8D"
          }
        }
      ],
      "TransactionIndex": 1,
      "TransactionResult": "tesSUCCESS"
    },
    "validated": true
  }
}
```
