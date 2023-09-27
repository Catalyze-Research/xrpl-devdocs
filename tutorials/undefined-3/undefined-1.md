# 일반 키 쌍 변경 또는 제거

XRP Ledger를 통해 계정은 일반 키 쌍이라고 하는 보조 키 쌍을 승인하여 향후 트랜잭션에 서명할 수 있습니다.계정의 일반 키 쌍이 손상되었거나 보안 조치로 정기적으로 일반 키 쌍을 변경하려면 SetRegularKey 트랜잭션을 사용하여 계정의 일반 키 쌍을 제거하거나 변경합니다.

마스터 및 일반 키 쌍에 대한 자세한 내용은 암호화 키를 참조하세요.

## 일반 키 쌍 변경 <a href="#changing-a-regular-key-pair" id="changing-a-regular-key-pair"></a>

기존의 일반 키 쌍을 변경하는 단계는 일반 키를 처음 할당하는 단계와 거의 같습니다. 키 쌍을 생성하고 계정에 일반 키 쌍으로 할당하여 기존의 일반 키 쌍을 덮어씁니다. 그러나 가장 큰 차이점은 기존 일반 키 쌍을 변경할 때 기존의 일반 개인 키를 사용하여 자신을 대체할 수 있지만, 계정에 처음으로 일반 키 쌍을 할당할 때는 계정의 마스터 개인 키를 사용해야 한다는 것입니다.

마스터 및 일반 키 쌍에 대한 자세한 내용은 암호화 키를 참조하세요.

## 일반 키 쌍 제거 <a href="#removing-a-regular-key-pair" id="removing-a-regular-key-pair"></a>

손상된 일반 키 쌍을 계정에서 제거하려면 먼저 키 쌍을 생성할 필요가 없습니다. SetRegularKey 트랜잭션을 사용하고 RegularKey 필드는 생략합니다. 현재 사용 가능한 계정에 대해 다른 서명 방법이 없는 경우 트랜잭션이 실패합니다

계정에 대한 일반 키 쌍을 제거할 때`SetRegularKey`트랜잭션을 수행하려면 계정의 마스터 개인 키(비밀) 또는 기존 일반 키 쌍으로 서명해야 합니다. 마스터 또는 일반 개인 키를 아무 곳에나 보내는 것은 위험하므로 트랜잭션 서명은 네트워크에 대한 트랜잭션 제출과 별도로 유지합니다.

## 트랜잭션 서명 <a href="#sign-your-transaction" id="sign-your-transaction"></a>

트랜잭션에 서명하는 가장 안전한 방법은 클라이언트 라이브러리로 로컬로 서명하는 것입니다. 또는 사용자가 직접 실행하는 경우`rippled`노드 서명 방법을 사용하여 트랜잭션에 서명할 수 있지만 이 작업은 신뢰할 수 있고 암호화된 연결 또는 로컬(동일한 컴퓨터) 연결을 통해 수행해야 합니다.

모든 경우에 서명된 트랜잭션은 나중에 해시를 식별합니다.

요청 필드에 다음 값을 입력합니다.

| 요청 필드     | 가치                                                                    |
| --------- | --------------------------------------------------------------------- |
| `Account` | 계정의 주소입니다.                                                            |
| `secret`  | `master_key`,`master_seed`또는`master_seed_hex`계정에 대한 (마스터 또는 일반 개인 키). |

### 요청 형식&#x20;

요청 형식의 예는 다음과 같습니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "command": "sign",
  "tx_json": {
      "TransactionType": "SetRegularKey",
      "Account": "r9xQZdFGwbwTB3g9ncKByWZ3du6Skm7gQ8"
      },
   "secret": "snoPBrXtMeMyMHUVTgbuqAfg1SUTb"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "sign",
    "params": [
        {
        "secret" : "snoPBrXtMeMyMHUVTgbuqAfg1SUTb",
        "tx_json" : {
            "TransactionType" : "SetRegularKey",
            "Account" : "r9xQZdFGwbwTB3g9ncKByWZ3du6Skm7gQ8"
            }
        }
    ]
}
```
{% endtab %}

{% tab title="Commandline" %}
```
#Syntax: sign secret tx_json
rippled sign snoPBrXtMeMyMHUVTgbuqAfg1SUTb '{"TransactionType": "SetRegularKey", "Account": "rUAi7pipxGpYfPNg3LtPcf2ApiS8aw9A93"}'
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
    "tx_blob": "1200052280000000240000000268400000000000000A73210330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD02074473045022100CAB9A6F84026D57B05760D5E2395FB7BE86BF39F10DC6E2E69DC91238EE0970B022058EC36A8EF9EE65F5D0D8CAC4E88C8C19FEF39E40F53D4CCECBB59701D6D1E838114623B8DA4A0BFB3B61AB423391A182DC693DC159E",
    "tx_json": {
      "Account": "r9xQZdFGwbwTB3g9ncKByWZ3du6Skm7gQ8",
      "Fee": "10",
      "Flags": 2147483648,
      "Sequence": 2,
      "SigningPubKey": "0330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD020",
      "TransactionType": "SetRegularKey",
      "TxnSignature": "3045022100CAB9A6F84026D57B05760D5E2395FB7BE86BF39F10DC6E2E69DC91238EE0970B022058EC36A8EF9EE65F5D0D8CAC4E88C8C19FEF39E40F53D4CCECBB59701D6D1E83",
      "hash": "59BCAB8E5B9D4597D6A7BFF22F6C555D0F41420599A2E126035B6AF19261AD97"
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
        "tx_blob": "1200052280000000240000000268400000000000000A73210330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD02074473045022100CAB9A6F84026D57B05760D5E2395FB7BE86BF39F10DC6E2E69DC91238EE0970B022058EC36A8EF9EE65F5D0D8CAC4E88C8C19FEF39E40F53D4CCECBB59701D6D1E838114623B8DA4A0BFB3B61AB423391A182DC693DC159E",
        "tx_json": {
            "Account": "r9xQZdFGwbwTB3g9ncKByWZ3du6Skm7gQ8",
            "Fee": "10",
            "Flags": 2147483648,
            "Sequence": 2,
            "SigningPubKey": "0330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD020",
            "TransactionType": "SetRegularKey",
            "TxnSignature": "3045022100CAB9A6F84026D57B05760D5E2395FB7BE86BF39F10DC6E2E69DC91238EE0970B022058EC36A8EF9EE65F5D0D8CAC4E88C8C19FEF39E40F53D4CCECBB59701D6D1E83",
            "hash": "59BCAB8E5B9D4597D6A7BFF22F6C555D0F41420599A2E126035B6AF19261AD97"
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
      "tx_blob" : "1200052280000000240000000268400000000000000A73210330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD02074473045022100CAB9A6F84026D57B05760D5E2395FB7BE86BF39F10DC6E2E69DC91238EE0970B022058EC36A8EF9EE65F5D0D8CAC4E88C8C19FEF39E40F53D4CCECBB59701D6D1E838114623B8DA4A0BFB3B61AB423391A182DC693DC159E",
      "tx_json" : {
         "Account" : "r9xQZdFGwbwTB3g9ncKByWZ3du6Skm7gQ8",
         "Fee" : "10",
         "Flags" : 2147483648,
         "Sequence" : 2,
         "SigningPubKey" : "0330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD020",
         "TransactionType" : "SetRegularKey",
         "TxnSignature" : "3045022100CAB9A6F84026D57B05760D5E2395FB7BE86BF39F10DC6E2E69DC91238EE0970B022058EC36A8EF9EE65F5D0D8CAC4E88C8C19FEF39E40F53D4CCECBB59701D6D1E83",
         "hash" : "59BCAB8E5B9D4597D6A7BFF22F6C555D0F41420599A2E126035B6AF19261AD97"
      }
   }
}
```
{% endtab %}
{% endtabs %}

거래 명령 응답에는 tx\_blob 값이 포함되어 있습니다. 이는 위에서 보여진 것과 같이 거래의 서명된 이진 표현(blobs)입니다. 오프라인 서명 응답에는 signedTransaction 값이 포함됩니다. 이것도 거래의 서명된 이진 표현입니다.

다음으로, submit 명령을 사용하여 거래 blob(tx\_blob 또는 signedTransaction)을 네트워크에 전송합니다.

### 트랜잭션 제출 <a href="#submit-your-transaction" id="submit-your-transaction"></a>

오프라인 서명 응답에서 signedTransaction 값 또는 서명 명령 응답에서 tx\_blob 값을 가져오고 submit 메소드를 사용하여 tx\_blob 값으로 제출합니다.

### 요청 형식&#x20;

요청 형식의 예는 다음과 같습니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "command": "submit",
    "tx_blob": "1200052280000000240000000268400000000000000A73210330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD02074473045022100CAB9A6F84026D57B05760D5E2395FB7BE86BF39F10DC6E2E69DC91238EE0970B022058EC36A8EF9EE65F5D0D8CAC4E88C8C19FEF39E40F53D4CCECBB59701D6D1E838114623B8DA4A0BFB3B61AB423391A182DC693DC159E"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
   "method":"submit",
   "params":[
      {
         "tx_blob":"1200052280000000240000000268400000000000000A73210330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD02074473045022100CAB9A6F84026D57B05760D5E2395FB7BE86BF39F10DC6E2E69DC91238EE0970B022058EC36A8EF9EE65F5D0D8CAC4E88C8C19FEF39E40F53D4CCECBB59701D6D1E838114623B8DA4A0BFB3B61AB423391A182DC693DC159E"
      }
   ]
}
```
{% endtab %}

{% tab title="Commandline" %}
```
#Syntax: submit tx_blob
rippled submit 1200052280000000240000000268400000000000000A73210330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD02074473045022100CAB9A6F84026D57B05760D5E2395FB7BE86BF39F10DC6E2E69DC91238EE0970B022058EC36A8EF9EE65F5D0D8CAC4E88C8C19FEF39E40F53D4CCECBB59701D6D1E838114623B8DA4A0BFB3B61AB423391A182DC693DC159E
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
    "tx_blob": "1200052280000000240000000268400000000000000A73210330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD02074473045022100CAB9A6F84026D57B05760D5E2395FB7BE86BF39F10DC6E2E69DC91238EE0970B022058EC36A8EF9EE65F5D0D8CAC4E88C8C19FEF39E40F53D4CCECBB59701D6D1E838114623B8DA4A0BFB3B61AB423391A182DC693DC159E",
    "tx_json": {
      "Account": "r9xQZdFGwbwTB3g9ncKByWZ3du6Skm7gQ8",
      "Fee": "10",
      "Flags": 2147483648,
      "Sequence": 2,
      "SigningPubKey": "0330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD020",
      "TransactionType": "SetRegularKey",
      "TxnSignature": "3045022100CAB9A6F84026D57B05760D5E2395FB7BE86BF39F10DC6E2E69DC91238EE0970B022058EC36A8EF9EE65F5D0D8CAC4E88C8C19FEF39E40F53D4CCECBB59701D6D1E83",
      "hash": "59BCAB8E5B9D4597D6A7BFF22F6C555D0F41420599A2E126035B6AF19261AD97"
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
        "tx_blob": "1200052280000000240000000268400000000000000A73210330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD02074473045022100CAB9A6F84026D57B05760D5E2395FB7BE86BF39F10DC6E2E69DC91238EE0970B022058EC36A8EF9EE65F5D0D8CAC4E88C8C19FEF39E40F53D4CCECBB59701D6D1E838114623B8DA4A0BFB3B61AB423391A182DC693DC159E",
        "tx_json": {
            "Account": "r9xQZdFGwbwTB3g9ncKByWZ3du6Skm7gQ8",
            "Fee": "10",
            "Flags": 2147483648,
            "Sequence": 2,
            "SigningPubKey": "0330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD020",
            "TransactionType": "SetRegularKey",
            "TxnSignature": "3045022100CAB9A6F84026D57B05760D5E2395FB7BE86BF39F10DC6E2E69DC91238EE0970B022058EC36A8EF9EE65F5D0D8CAC4E88C8C19FEF39E40F53D4CCECBB59701D6D1E83",
            "hash": "59BCAB8E5B9D4597D6A7BFF22F6C555D0F41420599A2E126035B6AF19261AD97"
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
      "tx_blob" : "1200052280000000240000000268400000000000000A73210330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD02074473045022100CAB9A6F84026D57B05760D5E2395FB7BE86BF39F10DC6E2E69DC91238EE0970B022058EC36A8EF9EE65F5D0D8CAC4E88C8C19FEF39E40F53D4CCECBB59701D6D1E838114623B8DA4A0BFB3B61AB423391A182DC693DC159E",
      "tx_json" : {
         "Account" : "r9xQZdFGwbwTB3g9ncKByWZ3du6Skm7gQ8",
         "Fee" : "10",
         "Flags" : 2147483648,
         "Sequence" : 2,
         "SigningPubKey" : "0330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD020",
         "TransactionType" : "SetRegularKey",
         "TxnSignature" : "3045022100CAB9A6F84026D57B05760D5E2395FB7BE86BF39F10DC6E2E69DC91238EE0970B022058EC36A8EF9EE65F5D0D8CAC4E88C8C19FEF39E40F53D4CCECBB59701D6D1E83",
         "hash" : "59BCAB8E5B9D4597D6A7BFF22F6C555D0F41420599A2E126035B6AF19261AD97"
      }
   }
}
```
{% endtab %}
{% endtabs %}

일반 키 쌍 제거에 성공했는지 확인하는 방법은 제거된 일반 개인 키를 사용하여 트랜잭션을 보낼 수 없음을 확인하는 것입니다.

다음은 위의 SetRegularKey 트랜잭션에서 제거된 일반 개인 키를 사용하여 서명된 AccountSet 트랜잭션에 대한 오류 응답의 예입니다.

## 응답 형식&#x20;

성공적인 응답의 예는 다음과 같습니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "error": "badSecret",
  "error_code": 41,
  "error_message": "Secret does not match account.",
  "request": {
    "command": "submit",
    "secret": "snoPBrXtMeMyMHUVTgbuqAfg1SUTb",
    "tx_json": {
      "Account": "r9xQZdFGwbwTB3g9ncKByWZ3du6Skm7gQ8",
      "TransactionType": "AccountSet"
    }
  },
  "status": "error",
  "type": "response"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "result": {
        "error": "badSecret",
        "error_code": 41,
        "error_message": "Secret does not match account.",
        "request": {
            "command": "submit",
            "secret": "snoPBrXtMeMyMHUVTgbuqAfg1SUTb",
            "tx_json": {
                "Account": "r9xQZdFGwbwTB3g9ncKByWZ3du6Skm7gQ8",
                "TransactionType": "AccountSet"
            }
        },
        "status": "error"
    }
}
```
{% endtab %}

{% tab title="Commandline" %}
```
{
   "result" : {
      "error" : "badSecret",
      "error_code" : 41,
      "error_message" : "Secret does not match account.",
      "request" : {
         "command" : "submit",
         "secret" : "snoPBrXtMeMyMHUVTgbuqAfg1SUTb",
         "tx_json" : {
            "Account" : "r9xQZdFGwbwTB3g9ncKByWZ3du6Skm7gQ8",
            "TransactionType" : "AccountSet"
         }
      },
      "status" : "error"
   }
}
```
{% endtab %}
{% endtabs %}

경우에 따라 SetRegularKey 트랜잭션을 사용하여 트랜잭션 비용을 지불하지 않고 키 재설정 트랜잭션을 보낼 수도 있습니다. XRP Ledger의 트랜잭션 대기열은 키 재설정 트랜잭션의 명목 트랜잭션 비용이 0인 경우에도 키 재설정 트랜잭션을 다른 트랜잭션보다 우선시합니다.
