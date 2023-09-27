# ledger\_accept

ledger\_accept 메소드는 서버가 현재 작업 중인 ledger을 닫고 다음 ledger 번호로 이동하도록 강제합니다. 이 메소드는 테스트 목적으로만 사용되며, rippled 서버가 stand-alone 모드로 실행 중일 때만 사용할 수 있습니다.

ledger\_accept 메소드는 권한이 없는 사용자가 실행할 수 없는 관리자 메소드입니다!

## 요청 형식

요청 형식의 예시입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
   "id": "Accept my ledger!",
   "command": "ledger_accept"
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
#Syntax: ledger_accept
rippled ledger_accept
```
{% endtab %}
{% endtabs %}

이 요청은 매개변수를 허용하지 않습니다.

## 응답 형식

성공적인 응답의 예입니다:

```json
{
  "id": "Accept my ledger!",
  "status": "success",
  "type": "response",
  "result": {
    "ledger_current_index": 6643240
  }
}
```

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`                | 유형                | 설명                     |
| ---------------------- | ----------------- | ---------------------- |
| `ledger_current_index` | 부호 없는 정수 - 원장 인덱스 | 새로 생성된 '현재' 원장의 원장 인덱스 |

{% hint style="info" %}
Note:

ledger을 닫으면 Ripple은 해당 ledger에 있는 트랜잭션의 표준 순서를 결정하고 다시 재생합니다. 이로 인해 현재 ledger에 임시로 적용되었던 트랜잭션의 결과가 변경될 수 있습니다.
{% endhint %}

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* notStandAlone - rippled 서버가 현재 stand-alone 모드로 실행되고 있지 않은 경우.
