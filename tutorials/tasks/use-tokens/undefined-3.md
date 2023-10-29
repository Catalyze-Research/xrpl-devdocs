# 글로벌 동결 시행

XRP Ledger에서 토큰을 발행하는 경우, 사용자가 서로 토큰을 보내거나 탈중앙화 트랜잭션소에서 토큰을 트랜잭션하지 못하도록 글로벌 동결을 시행할 수 있습니다. 이 튜토리얼에서는 글로벌 동결을 시작하고 종료하는 방법을 보여드리겠습니다. 예를 들어, Ledger에서 토큰을 발행한 주소나 토큰을 관리하는 데 사용하는 오프-ledger 시스템과 관련된 의심스러운 활동의 징후를 발견한 경우 이 작업을 수행할 수 있습니다. (예를 들어, 토큰이 스테이블코인이고 Ledger에서 출금과 입금을 처리하는 경우 시스템이 해킹당한 것으로 의심되는 경우 조사하는 동안 토큰을 동결할 수 있습니다.) 나중에 동결 안 함 설정을 활성화하지 않은 경우 전역 동결 설정을 비활성화할 수 있습니다.

{% hint style="info" %}
Tip:

다시 한 번 말씀드리자면, 동결은 발급된 토큰에만 적용되며 XRP에는 적용되지 않으며 사용자가 토큰을 발급자에게 직접 다시 보내는 것을 막지는 않습니다.
{% endhint %}

## 요구 조건

* XRP Ledger 네트워크에 연결해야 합니다. 이 튜토리얼에 표시된 대로 공용 서버를 사용하여 테스트할 수 있습니다.
* 선호하는 클라이언트 라이브러리에 대한 시작하기 지침을 숙지하고 있어야 합니다. 이 페이지에서는 다음에 대한 예제를 제공합니다:
  * xrpl.js 라이브러리를 사용한 JavaScript. 설정 단계는 JavaScript를 사용하여 시작하기를 참조하세요.
* 글로벌 동결을 실행하기 위해 XRP Ledger에 토큰을 발행할 필요는 없지만, 글로벌 동결을 실행하는 주된 이유는 이미 토큰을 발행한 경우에 해당합니다.

## 예제 코드

이 튜토리얼의 모든 단계에 대한 전체 샘플 코드는 MIT 라이선스에 따라 사용할 수 있습니다.

* 코드 샘플을 참조하세요: 이 웹사이트의 소스 저장소에서 동결을 참조하세요.

## 단계

## 1. 자격증명 가져오기

XRP Ledger에서 트랜잭션하려면 주소와 비밀 키, 그리고 약간의 XRP가 필요합니다. "콜드" 주소와 "핫" 주소를 분리하는 모범 사례를 사용하는 경우, 토큰 발행자인 콜드 주소의 키가 필요합니다. 발행자의 글로벌 동결 설정만 토큰에 영향을 미칩니다.

{% hint style="info" %}
Tip:

동결 없음 설정과 달리 일반 키 쌍 또는 다중 서명을 사용하여 글로벌 동결을 활성화 및 비활성화할 수 있습니다.
{% endhint %}

이 튜토리얼에서는 다음 인터페이스에서 자격 증명을 얻을 수 있습니다:

* [Generate](https://xrpl.org/enact-global-freeze.html#interactive-generate)
* [Connect](https://xrpl.org/enact-global-freeze.html#interactive-connect)
* [Send AccountSet (Start Freeze)](https://xrpl.org/enact-global-freeze.html#interactive-send\_accountset\_start\_freeze)
* [Wait](https://xrpl.org/enact-global-freeze.html#interactive-wait)
* [Confirm Settings](https://xrpl.org/enact-global-freeze.html#interactive-confirm\_settings)
* [Send AccountSet (End Freeze)](https://xrpl.org/enact-global-freeze.html#interactive-send\_accountset\_end\_freeze)
* [Wait (again)](https://xrpl.org/enact-global-freeze.html#interactive-wait\_again)
* [Confirm Settings (After Freeze)](https://xrpl.org/enact-global-freeze.html#interactive-confirm\_settings\_after\_freeze)

{% hint style="info" %}
Caution:

Ripple은 testnet과 devnet을 테스트 목적으로만 제공하며, 때때로 이러한 테스트 네트워크의 상태를 모든 잔액과 함께 초기화합니다. 예방책으로 testnet/devnet 과 mainnet에서 동일한 주소를 사용하지 마시기 바랍니다.
{% endhint %}

프로덕션에 사용할 수 있는 소프트웨어를 빌드할 때는 기존 계정을 사용하고 보안 서명 구성을 사용하여 키를 관리해야 합니다.

## 2. 네트워크에 연결

네트워크에 트랜잭션을 제출하려면 네트워크에 연결해야 합니다. 다음 코드는 지원되는 클라이언트 라이브러리로 퍼블릭 XRP Ledger testnet 서버에 연결하는 방법을 보여줍니다:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// In browsers, use a <script> tag. In Node.js, uncomment the following line:
// const xrpl = require('xrpl')

// Wrap code in an async function so we can use await
async function main() {

  // Define the network client
  const client = new xrpl.Client("wss://s.altnet.rippletest.net:51233")
  await client.connect()

  // ... custom code goes here

  // Disconnect when done (If you omit this, Node.js won't end the process)
  client.disconnect()
}

main()
```
{% endtab %}
{% endtabs %}

이 튜토리얼에서는 다음 버튼을 클릭하여 연결합니다:

* [Generate](https://xrpl.org/enact-global-freeze.html#interactive-generate)
* [Connect](https://xrpl.org/enact-global-freeze.html#interactive-connect)
* [Send AccountSet (Start Freeze)](https://xrpl.org/enact-global-freeze.html#interactive-send\_accountset\_start\_freeze)
* [Wait](https://xrpl.org/enact-global-freeze.html#interactive-wait)
* [Confirm Settings](https://xrpl.org/enact-global-freeze.html#interactive-confirm\_settings)
* [Send AccountSet (End Freeze)](https://xrpl.org/enact-global-freeze.html#interactive-send\_accountset\_end\_freeze)
* [Wait (again)](https://xrpl.org/enact-global-freeze.html#interactive-wait\_again)
* [Confirm Settings (After Freeze)](https://xrpl.org/enact-global-freeze.html#interactive-confirm\_settings\_after\_freeze)

## 3. 동결을 시작하기 위해 AccountSet 트랜잭션 보내기

글로벌 동결 설정을 활성화하려면, asfGlobalFreeze 값(7)이 포함된 SetFlag 필드가 있는 AccountSet 트랜잭션을 전송합니다. 트랜잭션을 보내려면 먼저 필요한 모든 필드를 채우도록 트랜잭션을 준비한 다음 계정의 비밀 키로 서명하고 마지막으로 네트워크에 제출합니다.

{% hint style="info" %}
Caution:

글로벌 동결을 시행하면 해당 주소에서 발행한 모든 토큰에 영향을 미칩니다. 또한 동결 안 함 설정을 사용하는 경우 이 작업을 취소할 수 없습니다.
{% endhint %}

예를 들어:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// Prepare an AccountSet transaction to enable global freeze -----------------
  const accountSetTx = {
    TransactionType: "AccountSet",
    Account: wallet.address,
    // Set a flag to turn on a global freeze on this account
    SetFlag: xrpl.AccountSetAsfFlags.asfGlobalFreeze
  }

  // Best practice for JS users - validate checks if a transaction is well-formed
  xrpl.validate(accountSetTx)

  // Sign and submit the AccountSet transaction to enable a global freeze ------
  console.log('Signing and submitting the transaction:', accountSetTx)
  await client.submitAndWait(accountSetTx, { wallet })
  console.log(`Finished submitting! ${wallet.address} should be frozen now.`)
```
{% endtab %}

{% tab title="WebSocket" %}
```json
{
  "id": "example_enable_global_freeze",
  "command": "submit",
  "tx_json": {
    "TransactionType": "AccountSet",
    "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "Fee": "12",
    "Flags": 0,
    "SetFlag": 7,
    "LastLedgerSequence": 18122753,
    "Sequence": 349
  },
  "secret": "s████████████████████████████"
}
```
{% endtab %}
{% endtabs %}

* [Generate](https://xrpl.org/enact-global-freeze.html#interactive-generate)
* [Connect](https://xrpl.org/enact-global-freeze.html#interactive-connect)
* [Send AccountSet (Start Freeze)](https://xrpl.org/enact-global-freeze.html#interactive-send\_accountset\_start\_freeze)
* [Wait](https://xrpl.org/enact-global-freeze.html#interactive-wait)
* [Confirm Settings](https://xrpl.org/enact-global-freeze.html#interactive-confirm\_settings)
* [Send AccountSet (End Freeze)](https://xrpl.org/enact-global-freeze.html#interactive-send\_accountset\_end\_freeze)
* [Wait (again)](https://xrpl.org/enact-global-freeze.html#interactive-wait\_again)
* [Confirm Settings (After Freeze)](https://xrpl.org/enact-global-freeze.html#interactive-confirm\_settings\_after\_freeze)

## 4. 유효성 검사 대기

대부분의 트랜잭션은 제출된 후 다음 Ledger 버전으로 승인되므로, 트랜잭션 결과가 최종적으로 확정되기까지 4\~7초가 소요될 수 있습니다. XRP Ledger이 사용 중이거나 네트워크 연결 상태가 좋지 않아 트랜잭션이 네트워크를 통해 전달되는 것이 지연되는 경우, 트랜잭션이 확인되는 데 더 오랜 시간이 걸릴 수 있습니다. (트랜잭션 만료를 설정하는 방법에 대한 자세한 내용은 안정적인 트랜잭션 제출을 참조하세요.)

* [Generate](https://xrpl.org/enact-global-freeze.html#interactive-generate)
* [Connect](https://xrpl.org/enact-global-freeze.html#interactive-connect)
* [Send AccountSet (Start Freeze)](https://xrpl.org/enact-global-freeze.html#interactive-send\_accountset\_start\_freeze)
* [Wait](https://xrpl.org/enact-global-freeze.html#interactive-wait)
* [Confirm Settings](https://xrpl.org/enact-global-freeze.html#interactive-confirm\_settings)
* [Send AccountSet (End Freeze)](https://xrpl.org/enact-global-freeze.html#interactive-send\_accountset\_end\_freeze)
* [Wait (again)](https://xrpl.org/enact-global-freeze.html#interactive-wait\_again)
* [Confirm Settings (After Freeze)](https://xrpl.org/enact-global-freeze.html#interactive-confirm\_settings\_after\_freeze)

## 5. 계정 설정 확인

트랜잭션의 유효성이 확인되면 발급 계정 설정을 확인하여 글로벌 동결 플래그가 활성화되어 있는지 확인할 수 있습니다. 계정 정보 메소드를 호출하고 계정의 Flags 필드 값을 확인하여 lsfGlobalFreeze 비트(0x00400000)가 켜져 있는지 확인하면 됩니다.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// Request account info for my_address to check account settings ------------
  const response = await client.request(
    {command: 'account_info', account: my_address })
  const settings = response.result
  const lsfGlobalFreeze = xrpl.LedgerEntry.AccountRootFlags.lsfGlobalFreeze

  console.log('Got settings for address', my_address);
  console.log('Global Freeze enabled?',
              ((settings.account_data.Flags & lsfGlobalFreeze) 
              === lsfGlobalFreeze))
```
{% endtab %}

{% tab title="WebSocket" %}
```json
Request:

{
  "id": 1,
  "command": "account_info",
  "account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
  "ledger_index": "validated"
}

Response:

{
  "id": 4,
  "status": "success",
  "type": "response",
  "result": {
    "account_data": {
      "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
      "AccountTxnID": "41320138CA9837B34E82B3B3D6FB1E581D5DE2F0A67B3D62B5B8A8C9C8D970D0",
      "Balance": "100258663",
      "Domain": "6D64756F31332E636F6D",
      "EmailHash": "98B4375E1D753E5B91627516F6D70977",
      "Flags": 12582912,
      "LedgerEntryType": "AccountRoot",
      "MessageKey": "0000000000000000000000070000000300",
      "OwnerCount": 4,
      "PreviousTxnID": "41320138CA9837B34E82B3B3D6FB1E581D5DE2F0A67B3D62B5B8A8C9C8D970D0",
      "PreviousTxnLgrSeq": 18123095,
      "Sequence": 352,
      "TransferRate": 1004999999,
      "index": "13F1A95D7AAB7108D5CE7EEAF504B2894B8C674E6D68499076441C4837282BF8",
      "urlgravatar": "http://www.gravatar.com/avatar/98b4375e1d753e5b91627516f6d70977"
    },
    "ledger_hash": "A777B05A293A73E511669B8A4A45A298FF89AD9C9394430023008DB4A6E7FDD5",
    "ledger_index": 18123249,
    "validated": true
  }
}
```
{% endtab %}
{% endtabs %}

## 인터미션: 동결된 동안

이 시점에서는 주소에서 발급된 모든 토큰이 동결됩니다. 이 기간 동안 글로벌 동결을 시행한 이유에 따라 잠재적인 보안 침해를 조사하거나 토큰 잔액의 스냅샷을 찍을 수 있습니다.

토큰이 동결된 상태에서도 동결된 토큰이 발행자에게 직접 또는 발행자로부터 직접 전송될 수 있으므로 이러한 트랜잭션을 전송하도록 구성된 시스템을 비활성화하고, 수신되는 트랜잭션을 처리하지 않고 추적하여 정상적인 트랜잭션을 처리할 수 있도록 해야 할 수도 있습니다.

핫월렛이나 운영 주소를 사용하는 경우 다른 사용자에 비해 특별한 지위를 가지지 않으므로 발행자와 직접 트랜잭션하는 경우를 제외하고 동결된 토큰을 주고받을 수 없습니다.

동결 안 함 설정을 사용하면 글로벌 동결이 영원히 지속됩니다. 토큰 발급을 재개하려면 새 계정을 생성하고 거기서부터 다시 시작해야 합니다.

그렇지 않으면 준비가 되면 언제든지 다음 단계로 넘어갈 수 있습니다.

## 6. 동결을 종료하기 위해 AccountSet 트랜잭션 보내기

글로벌 동결을 종료하려면 asfGlobalFreeze 값(7)이 포함된 ClearFlag 필드가 있는 AccountSet 트랜잭션을 보냅니다. 항상 그렇듯이 먼저 트랜잭션을 준비하고 서명한 다음 마지막으로 네트워크에 제출합니다.\
\
예를 들어:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// Now we disable the global freeze ------------------------------------------
  const accountSetTx2 = {
    TransactionType: "AccountSet",
    Account: wallet.address,
    // ClearFlag let's us turn off a global freeze on this account
    ClearFlag: xrpl.AccountSetAsfFlags.asfGlobalFreeze
  }

  // Best practice for JS users - validate checks if a transaction is well-formed
  xrpl.validate(accountSetTx2)

  // Sign and submit the AccountSet transaction to end a global freeze ---------
  console.log('Signing and submitting the transaction:', accountSetTx2)
  const result = await client.submitAndWait(accountSetTx2, { wallet: wallet })
  console.log("Finished submitting!")
```
{% endtab %}

{% tab title="WebSocket" %}
```json
{
  "id": "example_disable_global_freeze",
  "command": "submit",
  "tx_json": {
    "TransactionType": "AccountSet",
    "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "Fee": "12",
    "Flags": 0,
    "ClearFlag": 7,
    "LastLedgerSequence": 18122788,
    "Sequence": 350
  },
  "secret": "s████████████████████████████"
}
```
{% endtab %}
{% endtabs %}

* [Generate](https://xrpl.org/enact-global-freeze.html#interactive-generate)
* [Connect](https://xrpl.org/enact-global-freeze.html#interactive-connect)
* [Send AccountSet (Start Freeze)](https://xrpl.org/enact-global-freeze.html#interactive-send\_accountset\_start\_freeze)
* [Wait](https://xrpl.org/enact-global-freeze.html#interactive-wait)
* [Confirm Settings](https://xrpl.org/enact-global-freeze.html#interactive-confirm\_settings)
* [Send AccountSet (End Freeze)](https://xrpl.org/enact-global-freeze.html#interactive-send\_accountset\_end\_freeze)
* [Wait (again)](https://xrpl.org/enact-global-freeze.html#interactive-wait\_again)
* [Confirm Settings (After Freeze)](https://xrpl.org/enact-global-freeze.html#interactive-confirm\_settings\_after\_freeze)

## 7. 유효성 검사 대기

이전과 마찬가지로 이전 트랜잭션이 컨센서스에 의해 검증될 때까지 기다렸다가 계속 진행합니다.

* [Generate](https://xrpl.org/enact-global-freeze.html#interactive-generate)
* [Connect](https://xrpl.org/enact-global-freeze.html#interactive-connect)
* [Send AccountSet (Start Freeze)](https://xrpl.org/enact-global-freeze.html#interactive-send\_accountset\_start\_freeze)
* [Wait](https://xrpl.org/enact-global-freeze.html#interactive-wait)
* [Confirm Settings](https://xrpl.org/enact-global-freeze.html#interactive-confirm\_settings)
* [Send AccountSet (End Freeze)](https://xrpl.org/enact-global-freeze.html#interactive-send\_accountset\_end\_freeze)
* [Wait (again)](https://xrpl.org/enact-global-freeze.html#interactive-wait\_again)
* [Confirm Settings (After Freeze)](https://xrpl.org/enact-global-freeze.html#interactive-confirm\_settings\_after\_freeze)

## 8. 계정 설정 확인

트랜잭션이 유효성을 검사한 후 이전과 동일한 방법으로 계정 정보 메소드를 호출하고 계정의 플래그 필드 값을 확인하여 lsfGlobalFreeze 비트(0x00400000)가 꺼져 있는지 확인하여 글로벌 동결 플래그의 상태를 확인할 수 있습니다.

* [Generate](https://xrpl.org/enact-global-freeze.html#interactive-generate)
* [Connect](https://xrpl.org/enact-global-freeze.html#interactive-connect)
* [Send AccountSet (Start Freeze)](https://xrpl.org/enact-global-freeze.html#interactive-send\_accountset\_start\_freeze)
* [Wait](https://xrpl.org/enact-global-freeze.html#interactive-wait)
* [Confirm Settings](https://xrpl.org/enact-global-freeze.html#interactive-confirm\_settings)
* [Send AccountSet (End Freeze)](https://xrpl.org/enact-global-freeze.html#interactive-send\_accountset\_end\_freeze)
* [Wait (again)](https://xrpl.org/enact-global-freeze.html#interactive-wait\_again)
* [Confirm Settings (After Freeze)](https://xrpl.org/enact-global-freeze.html#interactive-confirm\_settings\_after\_freeze)

\
