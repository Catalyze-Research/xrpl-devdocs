# 만료된 에스크로 취소

## 1. 만료된 에스크로 확인&#x20;

XRP ledger의 에스크로는 취소 후 시간이 검증된 ledger 버전의 닫기 시간보다 낮으면 만료된 것입니다. (에스크로에 취소 후 시간이 없으면 만료되지 않습니다.) ledger 메소드를 사용하여 가장 최근에 유효성이 검증된 ledger의 마감 시간을 조회할 수 있습니다:

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
  "id": 1,
  "status": "success",
  "type": "response",
  "result": {
    "ledger": {
      ... (trimmed) ...

      "close_time": 560302643,
      "close_time_human": "2017-Oct-02 23:37:23",
      "close_time_resolution": 10,

      ... (trimmed) ...
    },
    "ledger_hash": "668F0647A6F3CC277496245DBBE9BD2E3B8E70E7AA824E97EF3237FE7E1EE3F2",
    "ledger_index": 2906341,
    "validated": true
  }
}
```
{% endtab %}
{% endtabs %}

[account\_objects](https://xrpl.org/account\_objects.html) 메소드를 사용하여 에스크로를 조회하고 CancelAfter 시간과 비교할 수 있습니다:

요청:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 2,
  "command": "account_objects",
  "account": "r3wN3v2vTUkr5qd6daqDc2xE4LSysdVjkT",
  "ledger_index": "validated",
  "type": "escrow"
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
    "account": "r3wN3v2vTUkr5qd6daqDc2xE4LSysdVjkT",
    "account_objects": [
      {
        "Account": "r3wN3v2vTUkr5qd6daqDc2xE4LSysdVjkT",
        "Amount": "10000",
        "CancelAfter": 559913895,
        "Destination": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
        "FinishAfter": 559892324,
        "Flags": 0,
        "LedgerEntryType": "Escrow",
        "OwnerNode": "0000000000000000",
        "PreviousTxnID": "4756C22BBB7FC23D9081FDB180806939D6FEBC967BE0EC2DB95B166AF9C086E9",
        "PreviousTxnLgrSeq": 2764813,
        "index": "7243A9750FA4BE3E63F75F6DACFD79AD6B6C76947F6BDC46CD0F52DBEEF64C89"
      }
    ],
    "ledger_hash": "82F24FFA72AED16F467BBE79D387E92FDA39F29038B26E79464CDEDFB506E366",
    "ledger_index": 2764826,
    "validated": true
  }
}
```
{% endtab %}
{% endtabs %}

## 2. EscrowCancel 트랜잭션 제출&#x20;

누구나 EscrowCancel 트랜잭션에 서명하고 제출하여 XRP Ledger에서 만료된 에스크로를 취소할 수 있습니다. 트랜잭션의 소유자 필드를 이 에스크로를 생성한 EscrowCreate 트랜잭션의 계정으로 설정합니다. 오퍼 시퀀스 필드를 EscrowCreate 트랜잭션의 시퀀스로 설정합니다.

{% hint style="info" %}
Caution:

제어하지 않는 서버에는 절대로 비밀 키를 제출하지 마세요. 네트워크를 통해 암호화되지 않은 비밀 키를 전송하지 마세요.
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
    "Account": "rhgdnc82FwHFUKXp9ZcpgwXWRAxKf5Buqp",
    "TransactionType": "EscrowCancel",
    "Owner": "r3wN3v2vTUkr5qd6daqDc2xE4LSysdVjkT",
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
    "tx_blob": "1200042280000000240000000320190000000168400000000000000A7321027FB1CF34395F18901CD294F77752EEE25277C6E87A224FC7388AA7EF872DB43D74473045022100AC45749FC4291F7811B2D8AC01CA04FEE38910CB7216FB0C5C0AEBC9C0A95F4302203F213C71C00136A0ADC670EFE350874BCB2E559AC02059CEEDFB846685948F2B81142866B7B47574C8A70D5E71FFB95FFDB18951427B82144E87970CD3EA984CF48B1AA6AB6C77DC4AB059FC",
    "tx_json": {
      "Account": "rhgdnc82FwHFUKXp9ZcpgwXWRAxKf5Buqp",
      "Fee": "10",
      "Flags": 2147483648,
      "OfferSequence": 1,
      "Owner": "r3wN3v2vTUkr5qd6daqDc2xE4LSysdVjkT",
      "Sequence": 3,
      "SigningPubKey": "027FB1CF34395F18901CD294F77752EEE25277C6E87A224FC7388AA7EF872DB43D",
      "TransactionType": "EscrowCancel",
      "TxnSignature": "3045022100AC45749FC4291F7811B2D8AC01CA04FEE38910CB7216FB0C5C0AEBC9C0A95F4302203F213C71C00136A0ADC670EFE350874BCB2E559AC02059CEEDFB846685948F2B",
      "hash": "65F36C5514153D94F0ADE5CE747061A5E70B73B56B4C66DA5040D99CAF252831"
    }
  }
}
```
{% endtab %}
{% endtabs %}

트랜잭션의 식별 해시값을 기록해 두면 검증된 ledger 버전에 포함되었을 때 최종 상태를 확인할 수 있습니다.

## 3. 검증 대기&#x20;

라이브 네트워크(mainnet, testnet 또는 devnet 포함)에서는 ledger가 자동으로 닫힐 때까지 4\~7초 정도 기다릴 수 있습니다.

stand-alone 모드에서 rippled를 실행하는 경우, ledger\_accept 메소드를 사용해 ledger를 수동으로 닫아야 합니다.&#x20;

## 4. 최종 결과 확인&#x20;

트랜잭션의 식별 해시와 함께 tx 메소드를 사용해 트랜잭션의 최종 상태를 확인합니다. 트랜잭션 메타데이터에서 LedgerEntryType이 에스크로인 DeletedNode를 찾습니다. 또한 에스크로된 결제의 발신자에 대해 AccountRoot 유형의 ModifiedNode를 찾습니다. 개체의 FinalFields에는 반환된 XRP에 대한 잔액 필드에 XRP 증가가 표시되어야 합니다.

요청:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 6,
  "command": "tx",
  "transaction": "65F36C5514153D94F0ADE5CE747061A5E70B73B56B4C66DA5040D99CAF252831"
}
```
{% endtab %}
{% endtabs %}

응답:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 6,
  "status": "success",
  "type": "response",
  "result": {
    "Account": "rhgdnc82FwHFUKXp9ZcpgwXWRAxKf5Buqp",
    "Fee": "10",
    "Flags": 2147483648,
    "OfferSequence": 1,
    "Owner": "r3wN3v2vTUkr5qd6daqDc2xE4LSysdVjkT",
    "Sequence": 3,
    "SigningPubKey": "027FB1CF34395F18901CD294F77752EEE25277C6E87A224FC7388AA7EF872DB43D",
    "TransactionType": "EscrowCancel",
    "TxnSignature": "3045022100AC45749FC4291F7811B2D8AC01CA04FEE38910CB7216FB0C5C0AEBC9C0A95F4302203F213C71C00136A0ADC670EFE350874BCB2E559AC02059CEEDFB846685948F2B",
    "date": 560302841,
    "hash": "65F36C5514153D94F0ADE5CE747061A5E70B73B56B4C66DA5040D99CAF252831",
    "inLedger": 2906406,
    "ledger_index": 2906406,
    "meta": {
      "AffectedNodes": [
        {
          "ModifiedNode": {
            "LedgerEntryType": "AccountRoot",
            "LedgerIndex": "13F1A95D7AAB7108D5CE7EEAF504B2894B8C674E6D68499076441C4837282BF8",
            "PreviousTxnID": "4756C22BBB7FC23D9081FDB180806939D6FEBC967BE0EC2DB95B166AF9C086E9",
            "PreviousTxnLgrSeq": 2764813
          }
        },
        {
          "ModifiedNode": {
            "FinalFields": {
              "Account": "rhgdnc82FwHFUKXp9ZcpgwXWRAxKf5Buqp",
              "Balance": "9999999970",
              "Flags": 0,
              "OwnerCount": 0,
              "Sequence": 4
            },
            "LedgerEntryType": "AccountRoot",
            "LedgerIndex": "3430FA3A160FA8F9842FA4A8B5549ECDCB3783E585D0F9796A1736DEAE35F6FE",
            "PreviousFields": {
              "Balance": "9999999980",
              "Sequence": 3
            },
            "PreviousTxnID": "DA6F5CA8CE13A03B8BC58515E085F2FEF90B3C08230B5AEC8DE4FAF39F79010B",
            "PreviousTxnLgrSeq": 2906391
          }
        },
        {
          "DeletedNode": {
            "FinalFields": {
              "Account": "r3wN3v2vTUkr5qd6daqDc2xE4LSysdVjkT",
              "Amount": "10000",
              "CancelAfter": 559913895,
              "Destination": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
              "FinishAfter": 559892324,
              "Flags": 0,
              "OwnerNode": "0000000000000000",
              "PreviousTxnID": "4756C22BBB7FC23D9081FDB180806939D6FEBC967BE0EC2DB95B166AF9C086E9",
              "PreviousTxnLgrSeq": 2764813
            },
            "LedgerEntryType": "Escrow",
            "LedgerIndex": "7243A9750FA4BE3E63F75F6DACFD79AD6B6C76947F6BDC46CD0F52DBEEF64C89"
          }
        },
        {
          "ModifiedNode": {
            "FinalFields": {
              "Flags": 0,
              "Owner": "r3wN3v2vTUkr5qd6daqDc2xE4LSysdVjkT",
              "RootIndex": "DACDBEBD31D14EAC4207A45DB88734AD14D26D908507F41D2FC623BDD91C582F"
            },
            "LedgerEntryType": "DirectoryNode",
            "LedgerIndex": "DACDBEBD31D14EAC4207A45DB88734AD14D26D908507F41D2FC623BDD91C582F"
          }
        },
        {
          "ModifiedNode": {
            "FinalFields": {
              "Account": "r3wN3v2vTUkr5qd6daqDc2xE4LSysdVjkT",
              "Balance": "9999999990",
              "Flags": 0,
              "OwnerCount": 0,
              "Sequence": 2
            },
            "LedgerEntryType": "AccountRoot",
            "LedgerIndex": "F5F1834B80A8B5DA878270AB4DE4EA444281181349375F1D21E46D5F3F0ABAC8",
            "PreviousFields": {
              "Balance": "9999989990",
              "OwnerCount": 1
            },
            "PreviousTxnID": "4756C22BBB7FC23D9081FDB180806939D6FEBC967BE0EC2DB95B166AF9C086E9",
            "PreviousTxnLgrSeq": 2764813
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

위 예시에서 r3wN3v2vTUkr5qd6daqDc2xE4LSysdVjkT는 에스크로 발신자이며, 잔액이 9999989990드롭에서 9999999990드롭으로 증가한 것은 에스크로된 10,000드롭의 XRP(0.01XRP)를 반환한 것을 나타냅니다.

{% hint style="info" %}
Tip:

에스크로를 실행하기 위해 EscrowFinish 트랜잭션에서 어떤 OfferSequence를 사용할지 모르는 경우, tx 메소드를 사용하여 에스크로의 PreviousTxnID 필드에 있는 트랜잭션의 식별 해시를 이용해 에스크로를 생성한 트랜잭션을 조회합니다. 에스크로를 완료할 때 해당 트랜잭션의 시퀀스 값을 오퍼 시퀀스 값으로 사용합니다.
{% endhint %}
