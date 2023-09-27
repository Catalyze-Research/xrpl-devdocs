# 일반 키 쌍 할당

XRP Ledger를 통해 계정은 일반 키 쌍이라고 하는 보조 키 쌍을 승인하여 향후 트랜잭션에 서명할 수 있습니다.일반 키 쌍의 개인 키가 손상된 경우 계정의 나머지 부분을 변경하거나 다른 계정과의 관계를 다시 설정하지 않고 키 쌍을 제거하거나 교체할 수 있습니다.또한 일반 키 쌍을 사전에 순환시킬 수도 있습니다. (계정의 주소와 본질적으로 연결된 계정의 마스터 키 쌍에서는 이러한 작업을 수행할 수 없습니다.)

마스터 및 일반 키 쌍에 대한 자세한 내용은 암호화 키를 참조하세요.

이 튜토리얼에서는 계정에 일반 키 쌍을 할당하는 데 필요한 단계를 설명합니다.

1. [키 쌍 생성](undefined.md#1-generate-a-key-pair)
2. [계정에 키 쌍을 일반 키 쌍으로 할당](undefined.md#2-assign-the-key-pair-to-your-account-as-a-regular-key-pair)
3. [일반 키 쌍 확인](undefined.md#3.)
4. 다음 단계 탐색

## 1. 키 쌍 생성 <a href="#1-generate-a-key-pair" id="1-generate-a-key-pair"></a>

계정에 일반 키 쌍으로 할당할 키 쌍을 생성합니다.

이 키 쌍은 마스터 키 쌍과 동일한 데이터 유형이므로 원하는 클라이언트 라이브러리를 사용하거나 실행 중인 서버의 wallet\_propose 메소드를 사용할 수 있습니다. 다음과 같이 표시될 수 있습니다.

{% tabs %}
{% tab title="WebSocket" %}
```json
// Request:

{
  "command": "wallet_propose"
}

// Response:

{
  "result": {
    "account_id": "rsprUqu6BHAffAeG4HpSdjBNvnA6gdnZV7",
    "key_type": "secp256k1",
    "master_key": "KNEW BENT LYNN LED GAD BEN KENT SHAM HOBO RINK WALT ALLY",
    "master_seed": "sh8i92YRnEjJy3fpFkL8txQSCVo79",
    "master_seed_hex": "966C0F68643EFBA50D58D191D4CA8AA7",
    "public_key": "aBRNH5wUurfhZcoyR6nRwDSa95gMBkovBJ8V4cp1C1pM28H7EPL1",
    "public_key_hex": "03AEEFE1E8ED4BBC009DE996AC03A8C6B5713B1554794056C66E5B8D1753C7DD0E"
  },
  "status": "success",
  "type": "response"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
// Request:

{
  "method": "wallet_propose"
}

// Response:

{
    "result": {
        "account_id": "rsprUqu6BHAffAeG4HpSdjBNvnA6gdnZV7",
        "key_type": "secp256k1",
        "master_key": "KNEW BENT LYNN LED GAD BEN KENT SHAM HOBO RINK WALT ALLY",
        "master_seed": "sh8i92YRnEjJy3fpFkL8txQSCVo79",
        "master_seed_hex": "966C0F68643EFBA50D58D191D4CA8AA7",
        "public_key": "aBRNH5wUurfhZcoyR6nRwDSa95gMBkovBJ8V4cp1C1pM28H7EPL1",
        "public_key_hex": "03AEEFE1E8ED4BBC009DE996AC03A8C6B5713B1554794056C66E5B8D1753C7DD0E",
        "status": "success"
    }
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
$ rippled wallet_propose

{
   "result" : {
      "account_id" : "rsprUqu6BHAffAeG4HpSdjBNvnA6gdnZV7",
      "key_type" : "secp256k1",
      "master_key" : "KNEW BENT LYNN LED GAD BEN KENT SHAM HOBO RINK WALT ALLY",
      "master_seed" : "sh8i92YRnEjJy3fpFkL8txQSCVo79",
      "master_seed_hex" : "966C0F68643EFBA50D58D191D4CA8AA7",
      "public_key" : "aBRNH5wUurfhZcoyR6nRwDSa95gMBkovBJ8V4cp1C1pM28H7EPL1",
      "public_key_hex" : "03AEEFE1E8ED4BBC009DE996AC03A8C6B5713B1554794056C66E5B8D1753C7DD0E",
      "status" : "success"
   }
}
```
{% endtab %}

{% tab title="Python" %}
```python
keypair = xrpl.wallet.Wallet.create()
print("seed:", keypair.seed)
print("classic address:", keypair.address)
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const keypair = new xrpl.Wallet()
console.log("seed:", keypair.seed)
console.log("classic address:", keypair.classicAddress)
```
{% endtab %}

{% tab title="Java" %}
```java
WalletFactory walletFactory = DefaultWalletFactory.getInstance();
Wallet keypair = walletFactory.randomWallet(true).wallet();
System.out.println(keypair);
System.out.println(keypair.privateKey().get());
```
{% endtab %}
{% endtabs %}

다음 단계에서는 이 응답의 주소를 사용합니다.`account_id`키 쌍을 일반 키 쌍으로 계정에 할당합니다.또한 이 키 쌍에서 시드 값을 저장합니다(`master_seed`API 응답에서) 이 키를 사용하여 나중에 트랜잭션에 서명할 수 있습니다. (다른 모든 것은 잊어버릴 수 있습니다.)

## 2. 계정에 키 쌍을 일반 키 쌍으로 할당 <a href="#2-assign-the-key-pair-to-your-account-as-a-regular-key-pair" id="2-assign-the-key-pair-to-your-account-as-a-regular-key-pair"></a>

SetRegularKey 트랜잭션을 사용하여 1단계에서 생성한 키 쌍을 계정에 일반 키 쌍으로 할당합니다.

일반 키 쌍을 계정에 처음으로 할당할 때 SetRegularKey 트랜잭션은 계정의 마스터 개인 키(비밀)로 서명해야 합니다. 거래를 안전하게 서명하는 방법에는 여러 가지가 있지만 이 자습서에서는 로컬 `rippled`서버를 사용합니다.

나중에 SetRegularKey 트랜잭션을 보낼 때 기존 일반 개인 키를 사용하여 서명하여 자체를 교체하거나 제거 할 수 있습니다. 여전히 네트워크를 통해 일반 개인 키를 제출해서는 안 됩니다.

## 트랜잭션 서명 <a href="#sign-your-transaction" id="sign-your-transaction"></a>

트랜잭션에 서명하는 가장 안전한 방법은 클라이언트 라이브러리를 사용하여 로컬로 서명하는 것입니다. 또는 자체 노드를 실행하는 경우 서명 방법을`rippled`를 사용하여 트랜잭션에 서명할 수 있지만 이는 신뢰할 수 있고 암호화된 연결 또는 로컬(동일 시스템) 연결을 통해 수행되어야 합니다.

모든 경우에 나중을 위해 서명된 트랜잭션의 식별 해시를 기록해 두세요.

요청 필드를 다음 값으로 채웁니다.

<table data-header-hidden><thead><tr><th width="269"></th><th></th></tr></thead><tbody><tr><td>요청 필드</td><td>값</td></tr><tr><td><code>Account</code></td><td>귀하의 계정 주소.</td></tr><tr><td><code>RegularKey</code></td><td><code>account_id</code>1단계에서 생성.</td></tr><tr><td><code>secret</code></td><td>귀하  계정에 대한 <code>master_key</code>, <code>master_seed</code>, 또는 <code>master_seed_hex</code>(마스터 개인 키)</td></tr></tbody></table>

### 요청 형식&#x20;

요청 형식의 예는 다음과 같습니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "command": "sign",
  "tx_json": {
      "TransactionType": "SetRegularKey",
      "Account": "rUAi7pipxGpYfPNg3LtPcf2ApiS8aw9A93",
      "RegularKey": "rsprUqu6BHAffAeG4HpSdjBNvnA6gdnZV7"
      },
   "secret": "ssCATR7CBvn4GLd1UuU2bqqQffHki"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
   "method": "sign",
   "params": [
      {
         "tx_json": {
            "TransactionType": "SetRegularKey",
            "Account": "rUAi7pipxGpYfPNg3LtPcf2ApiS8aw9A93",
            "RegularKey": "rsprUqu6BHAffAeG4HpSdjBNvnA6gdnZV7"
         },
         "secret": "ssCATR7CBvn4GLd1UuU2bqqQffHki"
      }
   ]
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
#Syntax: sign secret tx_json
rippled sign ssCATR7CBvn4GLd1UuU2bqqQffHki '{"TransactionType": "SetRegularKey", "Account": "rUAi7pipxGpYfPNg3LtPcf2ApiS8aw9A93", "RegularKey": "rsprUqu6BHAffAeG4HpSdjBNvnA6gdnZV7"}'
```
{% endtab %}
{% endtabs %}

### 응답 형식&#x20;

성공적인 응답의 예는 다음과 같습니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "result": {
    "tx_blob": "1200052280000000240000000468400000000000000A73210384CA3C528F10C75F26E0917F001338BD3C9AA1A39B9FBD583DFFFD96CF2E2D7A7446304402204BCD5663F3A2BA02D2CE374439096EC6D27273522CD6E6E0BDBFB518730EAAE402200ECD02D8D2525D6FA4642613E71E395ECCEA01C42C35A668BF092A00EB649C268114830923439D307E642CED308FD91EF701A7BAA74788141620D685FB08D81A70D0B668749CF2E130EA7540",
    "tx_json": {
      "Account": "rUAi7pipxGpYfPNg3LtPcf2ApiS8aw9A93",
      "Fee": "10",
      "Flags": 2147483648,
      "RegularKey": "rsprUqu6BHAffAeG4HpSdjBNvnA6gdnZV7",
      "Sequence": 4,
      "SigningPubKey": "0384CA3C528F10C75F26E0917F001338BD3C9AA1A39B9FBD583DFFFD96CF2E2D7A",
      "TransactionType": "SetRegularKey",
      "TxnSignature": "304402204BCD5663F3A2BA02D2CE374439096EC6D27273522CD6E6E0BDBFB518730EAAE402200ECD02D8D2525D6FA4642613E71E395ECCEA01C42C35A668BF092A00EB649C26",
      "hash": "AB73BBF7C99061678B59FB48D72CA0F5FC6DD2815B6736C6E9EB94439EC236CE"
    }
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
        "status": "success",
        "tx_blob": "1200052280000000240000000768400000000000000A73210384CA3C528F10C75F26E0917F001338BD3C9AA1A39B9FBD583DFFFD96CF2E2D7A7446304402201453CA3D4D17F0EE3828B9E3D6ACF65327F5D4FC2BA30953CACF6CBCB4145E3502202F2154BED1D7462CAC1E3DBB31864E48C3BA0B3133ACA5E37EC54F0D0C339E2D8114830923439D307E642CED308FD91EF701A7BAA74788141620D685FB08D81A70D0B668749CF2E130EA7540",
        "tx_json": {
            "Account": "rUAi7pipxGpYfPNg3LtPcf2ApiS8aw9A93",
            "Fee": "10",
            "Flags": 2147483648,
            "RegularKey": "rsprUqu6BHAffAeG4HpSdjBNvnA6gdnZV7",
            "Sequence": 4,
            "SigningPubKey": "0384CA3C528F10C75F26E0917F001338BD3C9AA1A39B9FBD583DFFFD96CF2E2D7A",
            "TransactionType": "SetRegularKey",
            "TxnSignature": "304402201453CA3D4D17F0EE3828B9E3D6ACF65327F5D4FC2BA30953CACF6CBCB4145E3502202F2154BED1D7462CAC1E3DBB31864E48C3BA0B3133ACA5E37EC54F0D0C339E2D",
            "hash": "AB73BBF7C99061678B59FB48D72CA0F5FC6DD2815B6736C6E9EB94439EC236CE"
        }
    }
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
{
   "result" : {
      "status" : "success",
      "tx_blob" : "1200052280000000240000000768400000000000000A73210384CA3C528F10C75F26E0917F001338BD3C9AA1A39B9FBD583DFFFD96CF2E2D7A7446304402201453CA3D4D17F0EE3828B9E3D6ACF65327F5D4FC2BA30953CACF6CBCB4145E3502202F2154BED1D7462CAC1E3DBB31864E48C3BA0B3133ACA5E37EC54F0D0C339E2D8114830923439D307E642CED308FD91EF701A7BAA74788141620D685FB08D81A70D0B668749CF2E130EA7540",
      "tx_json" : {
         "Account" : "rUAi7pipxGpYfPNg3LtPcf2ApiS8aw9A93",
         "Fee" : "10",
         "Flags" : 2147483648,
         "RegularKey" : "rsprUqu6BHAffAeG4HpSdjBNvnA6gdnZV7",
         "Sequence" : 4,
         "SigningPubKey" : "0384CA3C528F10C75F26E0917F001338BD3C9AA1A39B9FBD583DFFFD96CF2E2D7A",
         "TransactionType" : "SetRegularKey",
         "TxnSignature" : "304402201453CA3D4D17F0EE3828B9E3D6ACF65327F5D4FC2BA30953CACF6CBCB4145E3502202F2154BED1D7462CAC1E3DBB31864E48C3BA0B3133ACA5E37EC54F0D0C339E2D",
         "hash" : "AB73BBF7C99061678B59FB48D72CA0F5FC6DD2815B6736C6E9EB94439EC236CE"
      }
   }
}
```
{% endtab %}
{% endtabs %}

거래 명령 응답에는 tx\_blob 값이 포함되어 있습니다. 이는 위에서 보여진 것과 같이 거래의 서명된 이진 표현(blobs)입니다. 오프라인 서명 응답에는 signedTransaction값이 포함됩니다. 이것도 거래의 서명된 이진 표현입니다.

다음으로, submit 명령을 사용하여 거래 blob(tx\_blob 또는 signedTransaction)을 네트워크에 전송합니다.

## 트랜잭션 제출

오프라인 서명 응답에서의 signedTransaction 값 또는 sign 명령 응답에서의 tx\_blob 값을 가져와서 submit 메소드를 사용할 때 tx\_blob 값으로 제출하세요.

### 요청 형식&#x20;

요청 형식의 예는 다음과 같습니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "command": "submit",
    "tx_blob": "1200052280000000240000000468400000000000000A73210384CA3C528F10C75F26E0917F001338BD3C9AA1A39B9FBD583DFFFD96CF2E2D7A7446304402204BCD5663F3A2BA02D2CE374439096EC6D27273522CD6E6E0BDBFB518730EAAE402200ECD02D8D2525D6FA4642613E71E395ECCEA01C42C35A668BF092A00EB649C268114830923439D307E642CED308FD91EF701A7BAA74788141620D685FB08D81A70D0B668749CF2E130EA7540"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
   "method":"submit",
   "params": [
      {
         "tx_blob": "1200052280000000240000000468400000000000000A73210384CA3C528F10C75F26E0917F001338BD3C9AA1A39B9FBD583DFFFD96CF2E2D7A7446304402204BCD5663F3A2BA02D2CE374439096EC6D27273522CD6E6E0BDBFB518730EAAE402200ECD02D8D2525D6FA4642613E71E395ECCEA01C42C35A668BF092A00EB649C268114830923439D307E642CED308FD91EF701A7BAA74788141620D685FB08D81A70D0B668749CF2E130EA7540"
      }
   ]
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
#Syntax: submit tx_blob
rippled submit 1200052280000000240000000468400000000000000A73210384CA3C528F10C75F26E0917F001338BD3C9AA1A39B9FBD583DFFFD96CF2E2D7A7446304402204BCD5663F3A2BA02D2CE374439096EC6D27273522CD6E6E0BDBFB518730EAAE402200ECD02D8D2525D6FA4642613E71E395ECCEA01C42C35A668BF092A00EB649C268114830923439D307E642CED308FD91EF701A7BAA74788141620D685FB08D81A70D0B668749CF2E130EA7540
```
{% endtab %}
{% endtabs %}

### 응답 형식&#x20;

성공적인 응답의 예는 다음과 같습니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "result": {
    "engine_result": "tesSUCCESS",
    "engine_result_code": 0,
    "engine_result_message": "The transaction was applied. Only final in a validated ledger.",
    "tx_blob": "1200052280000000240000000468400000000000000A73210384CA3C528F10C75F26E0917F001338BD3C9AA1A39B9FBD583DFFFD96CF2E2D7A7446304402204BCD5663F3A2BA02D2CE374439096EC6D27273522CD6E6E0BDBFB518730EAAE402200ECD02D8D2525D6FA4642613E71E395ECCEA01C42C35A668BF092A00EB649C268114830923439D307E642CED308FD91EF701A7BAA74788141620D685FB08D81A70D0B668749CF2E130EA7540",
    "tx_json": {
      "Account": "rUAi7pipxGpYfPNg3LtPcf2ApiS8aw9A93",
      "Fee": "10",
      "Flags": 2147483648,
      "RegularKey": "rsprUqu6BHAffAeG4HpSdjBNvnA6gdnZV7",
      "Sequence": 4,
      "SigningPubKey": "0384CA3C528F10C75F26E0917F001338BD3C9AA1A39B9FBD583DFFFD96CF2E2D7A",
      "TransactionType": "SetRegularKey",
      "TxnSignature": "304402204BCD5663F3A2BA02D2CE374439096EC6D27273522CD6E6E0BDBFB518730EAAE402200ECD02D8D2525D6FA4642613E71E395ECCEA01C42C35A668BF092A00EB649C26",
      "hash": "AB73BBF7C99061678B59FB48D72CA0F5FC6DD2815B6736C6E9EB94439EC236CE"
    }
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
       "engine_result": "tesSUCCESS",
       "engine_result_code": 0,
       "engine_result_message": "The transaction was applied. Only final in a validated ledger.",
        "status": "success",
        "tx_blob": "1200052280000000240000000468400000000000000A73210384CA3C528F10C75F26E0917F001338BD3C9AA1A39B9FBD583DFFFD96CF2E2D7A7446304402204BCD5663F3A2BA02D2CE374439096EC6D27273522CD6E6E0BDBFB518730EAAE402200ECD02D8D2525D6FA4642613E71E395ECCEA01C42C35A668BF092A00EB649C268114830923439D307E642CED308FD91EF701A7BAA74788141620D685FB08D81A70D0B668749CF2E130EA7540",
        "tx_json": {
            "Account": "rUAi7pipxGpYfPNg3LtPcf2ApiS8aw9A93",
            "Fee": "10",
            "Flags": 2147483648,
            "RegularKey": "rsprUqu6BHAffAeG4HpSdjBNvnA6gdnZV7",
            "Sequence": 4,
            "SigningPubKey": "0384CA3C528F10C75F26E0917F001338BD3C9AA1A39B9FBD583DFFFD96CF2E2D7A",
            "TransactionType": "SetRegularKey",
            "TxnSignature": "304402204BCD5663F3A2BA02D2CE374439096EC6D27273522CD6E6E0BDBFB518730EAAE402200ECD02D8D2525D6FA4642613E71E395ECCEA01C42C35A668BF092A00EB649C26",
            "hash": "AB73BBF7C99061678B59FB48D72CA0F5FC6DD2815B6736C6E9EB94439EC236CE"
        }
    }
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
{
   "result" : {
      "engine_result" : "tesSUCCESS",
      "engine_result_code" : 0,
      "engine_result_message" : "The transaction was applied. Only final in a validated ledger.",
      "status" : "success",
      "tx_blob" : "1200052280000000240000000468400000000000000A73210384CA3C528F10C75F26E0917F001338BD3C9AA1A39B9FBD583DFFFD96CF2E2D7A7446304402204BCD5663F3A2BA02D2CE374439096EC6D27273522CD6E6E0BDBFB518730EAAE402200ECD02D8D2525D6FA4642613E71E395ECCEA01C42C35A668BF092A00EB649C268114830923439D307E642CED308FD91EF701A7BAA74788141620D685FB08D81A70D0B668749CF2E130EA7540",
      "tx_json" : {
         "Account" : "rUAi7pipxGpYfPNg3LtPcf2ApiS8aw9A93",
         "Fee" : "10",
         "Flags" : 2147483648,
         "RegularKey" : "rsprUqu6BHAffAeG4HpSdjBNvnA6gdnZV7",
         "Sequence" : 4,
         "SigningPubKey" : "0384CA3C528F10C75F26E0917F001338BD3C9AA1A39B9FBD583DFFFD96CF2E2D7A",
         "TransactionType" : "SetRegularKey",
         "TxnSignature" : "304402204BCD5663F3A2BA02D2CE374439096EC6D27273522CD6E6E0BDBFB518730EAAE402200ECD02D8D2525D6FA4642613E71E395ECCEA01C42C35A668BF092A00EB649C26",
         "hash" : "AB73BBF7C99061678B59FB48D72CA0F5FC6DD2815B6736C6E9EB94439EC236CE"
      }
   }
}
```
{% endtab %}
{% endtabs %}

응답에는 거래의 해시가 포함되어 있으며, 이를 사용하여 거래의 최종 결과를 확인할 수 있습니다.

## 3. 일반 키 쌍 확인&#x20;

이 시점에서, 일반 키 쌍이 계정에 할당되고 일반 키 쌍을 사용하여 거래를 보낼 수 있어야 합니다. 마스터 키 쌍을 비활성화하는 등 추가적인 단계를 수행하기 전에 계정을 잃어버리는 것을 방지하기 위해 일반키를 테스트하는 것이 중요합니다. 실수로 계정에 접근을 잃어버리면 아무도 그것을 복구할 수 없습니다.

계정에 일반 키 쌍이 제대로 설정되어 있는지 확인하려면, 단계 2에서 계정에 할당한 일반 비공개 키로부터 계정의 AccountSet 거래를 제출합니다. 단계 1과 같이, 이 튜토리얼은 안전하게 거래를 서명하기 위한 방법으로 로컬 rippled 서버를 사용합니다.

## 트랜잭션 서명 <a href="#sign-your-transaction" id="sign-your-transaction"></a>

트랜잭션에 서명하는 가장 안전한 방법은 클라이언트 라이브러리를 사용하여 로컬로 서명하는 것입니다. 또는 자체 rippled 노드를 실행하는 경우 서명 방법을 사용하여 트랜잭션에 서명할 수 있지만 이는 신뢰할 수 있고 암호화된 연결 또는 로컬(동일 시스템) 연결을 통해 수행되어야 합니다.&#x20;

모든 경우에 나중을 위해 서명된 트랜잭션의 식별 해시를 기록해 두세요.

요청 필드를 다음 값으로 채웁니다:

<table><thead><tr><th width="267">Request Field</th><th>Value</th></tr></thead><tbody><tr><td><code>Account</code></td><td>계정의 주소입니다.</td></tr><tr><td><code>secret</code></td><td>1단계에서 생성하고 2단계에서 계정에 할당된 <code>master_key</code>, <code>master_seed</code>, 또는 <code>master_seed_hex</code>(일반 개인 키)를 입력합니다.</td></tr></tbody></table>

### 요청  형식

다음은 요청 형식의 예입니다. 요청에는 AccountSet 옵션이 포함되어 있지 않습니다. 이것은 성공적인 거래가 귀하의 계정에 대해 일반 키 쌍이 올바르게 설정되었는지 확인하고 거래 비용을 없애는 것 외에는 아무런 영향을 미치지 않는다는 것을 의미합니다.

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "command": "sign",
  "tx_json": {
      "TransactionType": "AccountSet",
      "Account": "rUAi7pipxGpYfPNg3LtPcf2ApiS8aw9A93"
      },
   "secret": "sh8i92YRnEjJy3fpFkL8txQSCVo79"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
   "method": "sign",
   "params": [
      {
         "tx_json": {
            "TransactionType": "AccountSet",
            "Account": "rUAi7pipxGpYfPNg3LtPcf2ApiS8aw9A93"
         },
         "secret": "sh8i92YRnEjJy3fpFkL8txQSCVo79"
      }
   ]
}
```
{% endtab %}

{% tab title="Commandline" %}
```
#Syntax: sign secret tx_json
rippled sign sh8i92YRnEjJy3fpFkL8txQSCVo79 '{"TransactionType": "AccountSet", "Account": "rUAi7pipxGpYfPNg3LtPcf2ApiS8aw9A93"}'
```
{% endtab %}
{% endtabs %}

### 응답 형식&#x20;

성공적인 응답의 예는 다음과 같습니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "result": {
    "tx_blob": "1200032280000000240000000468400000000000000A73210330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD02074473045022100A50E867D3B1B5A39F23F1ABCA5C7C3EC755442FDAA357EFD897B865ACA7686DB02206077BF459BCE39BCCBFE1A128DA986D1E00CBEC5F0D6B0E11710F60BE2976FB88114623B8DA4A0BFB3B61AB423391A182DC693DC159E",
    "tx_json": {
      "Account": "rUAi7pipxGpYfPNg3LtPcf2ApiS8aw9A93",
      "Fee": "10",
      "Flags": 2147483648,
      "Sequence": 4,
      "SigningPubKey": "0330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD020",
      "TransactionType": "AccountSet",
      "TxnSignature": "3045022100A50E867D3B1B5A39F23F1ABCA5C7C3EC755442FDAA357EFD897B865ACA7686DB02206077BF459BCE39BCCBFE1A128DA986D1E00CBEC5F0D6B0E11710F60BE2976FB8",
      "hash": "D9B305CB6E861D0994A5CDD4726129D91AC4277111DC444DE4CEE44AD4674A9F"
    }
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
        "status": "success",
        "tx_blob": "1200032280000000240000000468400000000000000A73210330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD02074473045022100A50E867D3B1B5A39F23F1ABCA5C7C3EC755442FDAA357EFD897B865ACA7686DB02206077BF459BCE39BCCBFE1A128DA986D1E00CBEC5F0D6B0E11710F60BE2976FB88114623B8DA4A0BFB3B61AB423391A182DC693DC159E",
        "tx_json": {
            "Account": "rUAi7pipxGpYfPNg3LtPcf2ApiS8aw9A93",
            "Fee": "10",
            "Flags": 2147483648,
            "Sequence": 4,
            "SigningPubKey": "0330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD020",
            "TransactionType": "AccountSet",
            "TxnSignature": "3045022100A50E867D3B1B5A39F23F1ABCA5C7C3EC755442FDAA357EFD897B865ACA7686DB02206077BF459BCE39BCCBFE1A128DA986D1E00CBEC5F0D6B0E11710F60BE2976FB8",
            "hash": "D9B305CB6E861D0994A5CDD4726129D91AC4277111DC444DE4CEE44AD4674A9F"
        }
    }
}
```
{% endtab %}

{% tab title="Commandline" %}
```
{
   "result" : {
      "status" : "success",
      "tx_blob" : "1200032280000000240000000468400000000000000A73210330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD02074473045022100A50E867D3B1B5A39F23F1ABCA5C7C3EC755442FDAA357EFD897B865ACA7686DB02206077BF459BCE39BCCBFE1A128DA986D1E00CBEC5F0D6B0E11710F60BE2976FB88114623B8DA4A0BFB3B61AB423391A182DC693DC159E",
      "tx_json" : {
         "Account" : "rUAi7pipxGpYfPNg3LtPcf2ApiS8aw9A93",
         "Fee" : "10",
         "Flags" : 2147483648,
         "Sequence" : 4,
         "SigningPubKey" : "0330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD020",
         "TransactionType" : "AccountSet",
         "TxnSignature" : "3045022100A50E867D3B1B5A39F23F1ABCA5C7C3EC755442FDAA357EFD897B865ACA7686DB02206077BF459BCE39BCCBFE1A128DA986D1E00CBEC5F0D6B0E11710F60BE2976FB8",
         "hash" : "D9B305CB6E861D0994A5CDD4726129D91AC4277111DC444DE4CEE44AD4674A9F"
      }
   }
}
```
{% endtab %}
{% endtabs %}

서명 명령 응답에는 위와 같이 tx\_blob 값이 포함됩니다. 오프라인 서명 응답에는 signedTransaction 값이 포함되어 있습니다. 둘 다 트랜잭션의 서명된 이진 표현(blobs)입니다.

그런 다음 제출 명령을 사용하여 트랜잭션 blob(tx\_blob 또는 signedTransaction)을 네트워크로 보냅니다.

### 트랜잭션 제출&#x20;

오프라인 서명 응답에서 signedTransaction 값 또는 서명 명령 응답에서 tx\_blob 값을 가져오고 submit 메소드를 사용하여 tx\_blob 값으로 제출합니다.

### 요청 형식&#x20;

요청 형식의 예는 다음과 같습니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "command": "submit",
    "tx_blob": "1200032280000000240000000468400000000000000A73210330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD02074473045022100A50E867D3B1B5A39F23F1ABCA5C7C3EC755442FDAA357EFD897B865ACA7686DB02206077BF459BCE39BCCBFE1A128DA986D1E00CBEC5F0D6B0E11710F60BE2976FB88114623B8DA4A0BFB3B61AB423391A182DC693DC159E"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
   "method":"submit",
   "params": [
      {
         "tx_blob": "1200032280000000240000000468400000000000000A73210330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD02074473045022100A50E867D3B1B5A39F23F1ABCA5C7C3EC755442FDAA357EFD897B865ACA7686DB02206077BF459BCE39BCCBFE1A128DA986D1E00CBEC5F0D6B0E11710F60BE2976FB88114623B8DA4A0BFB3B61AB423391A182DC693DC159E"
      }
   ]
}
```
{% endtab %}

{% tab title="Commandline" %}
```
#Syntax: submit tx_blob
rippled submit 1200032280000000240000000468400000000000000A73210330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD02074473045022100A50E867D3B1B5A39F23F1ABCA5C7C3EC755442FDAA357EFD897B865ACA7686DB02206077BF459BCE39BCCBFE1A128DA986D1E00CBEC5F0D6B0E11710F60BE2976FB88114623B8DA4A0BFB3B61AB423391A182DC693DC159E
```
{% endtab %}
{% endtabs %}

### 응답 형식&#x20;

성공적인 응답의 예는 다음과 같습니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "result": {
    "engine_result": "tesSUCCESS",
    "engine_result_code": 0,
    "engine_result_message": "The transaction was applied. Only final in a validated ledger.",
    "tx_blob": "1200032280000000240000000468400000000000000A73210330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD02074473045022100A50E867D3B1B5A39F23F1ABCA5C7C3EC755442FDAA357EFD897B865ACA7686DB02206077BF459BCE39BCCBFE1A128DA986D1E00CBEC5F0D6B0E11710F60BE2976FB88114623B8DA4A0BFB3B61AB423391A182DC693DC159E",
    "tx_json": {
      "Account": "rUAi7pipxGpYfPNg3LtPcf2ApiS8aw9A93",
      "Fee": "10",
      "Flags": 2147483648,
      "Sequence": 4,
      "SigningPubKey": "0330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD020",
      "TransactionType": "AccountSet",
      "TxnSignature": "3045022100A50E867D3B1B5A39F23F1ABCA5C7C3EC755442FDAA357EFD897B865ACA7686DB02206077BF459BCE39BCCBFE1A128DA986D1E00CBEC5F0D6B0E11710F60BE2976FB8",
      "hash": "D9B305CB6E861D0994A5CDD4726129D91AC4277111DC444DE4CEE44AD4674A9F"
    }
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
        "engine_result": "tesSUCCESS",
        "engine_result_code": 0,
        "engine_result_message": "The transaction was applied. Only final in a validated ledger.",
        "status": "success",
        "tx_blob": "1200032280000000240000000468400000000000000A73210330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD02074473045022100A50E867D3B1B5A39F23F1ABCA5C7C3EC755442FDAA357EFD897B865ACA7686DB02206077BF459BCE39BCCBFE1A128DA986D1E00CBEC5F0D6B0E11710F60BE2976FB88114623B8DA4A0BFB3B61AB423391A182DC693DC159E",
        "tx_json": {
            "Account": "rUAi7pipxGpYfPNg3LtPcf2ApiS8aw9A93",
            "Fee": "10",
            "Flags": 2147483648,
            "Sequence": 4,
            "SigningPubKey": "0330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD020",
            "TransactionType": "AccountSet",
            "TxnSignature": "3045022100A50E867D3B1B5A39F23F1ABCA5C7C3EC755442FDAA357EFD897B865ACA7686DB02206077BF459BCE39BCCBFE1A128DA986D1E00CBEC5F0D6B0E11710F60BE2976FB8",
            "hash": "D9B305CB6E861D0994A5CDD4726129D91AC4277111DC444DE4CEE44AD4674A9F"
        }
    }
}
```
{% endtab %}

{% tab title="Commandline" %}
```
{
   "result" : {
      "engine_result" : "tesSUCCESS",
      "engine_result_code" : 0,
      "engine_result_message" : "The transaction was applied. Only final in a validated ledger.",
      "status" : "success",
      "tx_blob" : "1200032280000000240000000468400000000000000A73210330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD02074473045022100A50E867D3B1B5A39F23F1ABCA5C7C3EC755442FDAA357EFD897B865ACA7686DB02206077BF459BCE39BCCBFE1A128DA986D1E00CBEC5F0D6B0E11710F60BE2976FB88114623B8DA4A0BFB3B61AB423391A182DC693DC159E",
      "tx_json" : {
         "Account" : "rUAi7pipxGpYfPNg3LtPcf2ApiS8aw9A93",
         "Fee" : "10",
         "Flags" : 2147483648,
         "Sequence" : 4,
         "SigningPubKey" : "0330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD020",
         "TransactionType" : "AccountSet",
         "TxnSignature" : "3045022100A50E867D3B1B5A39F23F1ABCA5C7C3EC755442FDAA357EFD897B865ACA7686DB02206077BF459BCE39BCCBFE1A128DA986D1E00CBEC5F0D6B0E11710F60BE2976FB8",
         "hash" : "D9B305CB6E861D0994A5CDD4726129D91AC4277111DC444DE4CEE44AD4674A9F"
      }
   }
}
```
{% endtab %}
{% endtabs %}

다음 결과 코드와 함께 트랜잭션이 실패하면 다음 사항을 확인해야 합니다:

* tefBAD\_AUTH: 테스트 트랜잭션에 서명한 일반 키가 이전 단계에서 설정한 일반 키와 일치하지 않습니다. 일반 키 쌍의 비밀과 주소가 일치하는지 확인하고 각 단계에서 사용한 값을 다시 확인하세요.
* tefBAD\_AUTH\_MASTER 또는 temBAD\_AUTH\_MASTER: 계정에 할당된 일반 키가 없습니다. SetRegularKey 트랜잭션이 성공적으로 실행되었는지 확인하세요. 또한 account\_info 메소드를 사용하여 일반 키가 예상대로 RegularKey 필드에 설정되어 있는지 확인할 수 있습니다.&#x20;

다른 결과 코드의 가능한 원인은 트랜잭션 결과를 참조하세요.
