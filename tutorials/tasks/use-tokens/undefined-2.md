# 동결 금지 활성화

XRP Ledger에서 토큰을 발행하는 경우, 동결 금지 설정을 활성화하여 XRP Ledger의 토큰 동결 기능을 영구적으로 사용할 수 없도록 제한할 수 있습니다. (참고로, 이는 XRP가 아닌 발행된 토큰에만 적용됩니다.) 이 튜토리얼에서는 발행 계정에서 동결 금지 설정을 활성화하는 방법을 보여드리겠습니다.

## 요구 조건

* XRP Ledger 네트워크에 연결해야 합니다. 이 튜토리얼에 표시된 대로 공용 서버를 사용하여 테스트할 수 있습니다.
* 선호하는 클라이언트 라이브러리에 대한 시작하기 지침을 숙지하고 있어야 합니다. 이 페이지에서는 다음에 대한 예제를 제공합니다:
  * xrpl.js 라이브러리를 사용한 JavaScript. 설정 단계는 JavaScript를 사용하여 시작하기를 참조하세요.
* 동결 방지를 활성화하기 위해 XRP Ledger에 토큰을 발행할 필요는 없지만, 토큰을 발행할 계획이 있거나 이미 발행한 경우 동결 방지를 활성화하는 것이 좋습니다.

## 예제 코드

이 튜토리얼의 모든 단계에 대한 전체 샘플 코드는 MIT 라이선스에 따라 사용할 수 있습니다.

* 코드 샘플을 참조하세요: 이 웹사이트의 소스 저장소에서 동결을 참조하세요.

## 단계

## 1. 자격증명 가져오기

XRP Ledger에서 트랜잭션하려면 주소와 비밀 키, 그리고 약간의 XRP가 필요합니다. "콜드" 주소와 "핫" 주소를 별도로 사용하는 모범 사례를 사용하는 경우, 토큰 발행자인 콜드 주소의 마스터 키가 필요합니다. 발행자의 동결 금지 설정만 토큰에 영향을 미칩니다.

{% hint style="info" %}
Caution:

일반 키 쌍이나 다중 서명을 사용하여 동결 금지 설정을 사용할 수 없습니다.&#x20;
{% endhint %}

이 튜토리얼에서는 다음 인터페이스에서 자격 증명을 얻을 수 있습니다:

* [Generate](https://xrpl.org/enable-no-freeze.html#interactive-generate)
* [Connect](https://xrpl.org/enable-no-freeze.html#interactive-connect)
* [Send AccountSet](https://xrpl.org/enable-no-freeze.html#interactive-send\_accountset)
* [Wait](https://xrpl.org/enable-no-freeze.html#interactive-wait)
* [Confirm Settings](https://xrpl.org/enable-no-freeze.html#interactive-confirm\_settings)&#x20;

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

* [Generate](https://xrpl.org/enable-no-freeze.html#interactive-generate)
* [Connect](https://xrpl.org/enable-no-freeze.html#interactive-connect)
* [Send AccountSet](https://xrpl.org/enable-no-freeze.html#interactive-send\_accountset)
* [Wait](https://xrpl.org/enable-no-freeze.html#interactive-wait)
* [Confirm Settings](https://xrpl.org/enable-no-freeze.html#interactive-confirm\_settings)

## 3. AccountSet 트랜잭션 전송

동결 금지 설정을 활성화하려면 asfNoFreeze 값(6)이 포함된 SetFlag 필드가 있는 AccountSet 트랜잭션을 전송합니다. 트랜잭션을 보내려면 먼저 필요한 모든 필드를 채우도록 트랜잭션을 준비한 다음 계정의 비밀 키로 서명한 다음 마지막으로 네트워크에 제출합니다.

예를 들어:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// Submit an AccountSet transaction to enable No Freeze ----------------------
  const accountSetTx = {
    TransactionType: "AccountSet",
    Account: wallet.address,
    // Set the NoFreeze flag for this account
    SetFlag: xrpl.AccountSetAsfFlags.asfNoFreeze
  }

  // Best practice for JS users - validate checks if a transaction is well-formed
  xrpl.validate(accountSetTx)

  console.log('Sign and submit the transaction:', accountSetTx)
  await client.submitAndWait(accountSetTx, { wallet: wallet })
```
{% endtab %}

{% tab title="WebSocket" %}
```json
{
  "id": 12,
  "command": "submit",
  "tx_json": {
    "TransactionType": "AccountSet",
    "Account": "raKEEVSGnKSD9Zyvxu4z6Pqpm4ABH8FS6n",
    "Fee": "12",
    "Flags": 0,
    "SetFlag": 6,
    "LastLedgerSequence": 18124917,
    "Sequence": 4
  },
  "secret": "s████████████████████████████"
}
```
{% endtab %}
{% endtabs %}

* [Generate](https://xrpl.org/enable-no-freeze.html#interactive-generate)
* [Connect](https://xrpl.org/enable-no-freeze.html#interactive-connect)
* [Send AccountSet](https://xrpl.org/enable-no-freeze.html#interactive-send\_accountset)
* [Wait](https://xrpl.org/enable-no-freeze.html#interactive-wait)
* [Confirm Settings](https://xrpl.org/enable-no-freeze.html#interactive-confirm\_settings)

## 4. 유효성 검사 대기

대부분의 트랜잭션은 제출된 후 다음 Ledger 버전으로 승인되므로, 트랜잭션 결과가 최종적으로 확정되기까지 4\~7초가 소요될 수 있습니다. XRP Ledger 사용 중이거나 네트워크 연결 상태가 좋지 않아 트랜잭션이 네트워크를 통해 릴레이되는 것이 지연되는 경우, 트랜잭션이 확인되는 데 더 오랜 시간이 걸릴 수 있습니다. (트랜잭션 만료를 설정하는 방법에 대한 자세한 내용은 안정적인 트랜잭션 제출을 참조하세요.)

* [Generate](https://xrpl.org/enable-no-freeze.html#interactive-generate)
* [Connect](https://xrpl.org/enable-no-freeze.html#interactive-connect)
* [Send AccountSet](https://xrpl.org/enable-no-freeze.html#interactive-send\_accountset)
* [Wait](https://xrpl.org/enable-no-freeze.html#interactive-wait)
* [Confirm Settings](https://xrpl.org/enable-no-freeze.html#interactive-confirm\_settings)

## 5. 계정 설정 확인

트랜잭션이 확인된 후 계정 설정을 확인하여 동결 금지 플래그가 활성화되어 있는지 확인할 수 있습니다. 계정 정보 메소드를 호출하고 계정의 플래그 필드 값을 확인하여 lsfNoFreeze 비트(0x00200000)가 활성화되어 있는지 확인하면 됩니다.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// Request account info for my_address to check account settings ------------
  const response = await client.request(
    {command: 'account_info', account: my_address })
  const settings = response.result
  const lsfNoFreeze = xrpl.LedgerEntry.AccountRootFlags.lsfNoFreeze

  console.log('Got settings for address', my_address);
  console.log('No Freeze enabled?',
    (settings.account_data.Flags & lsfNoFreeze) 
    === lsfNoFreeze)
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

* [Generate](https://xrpl.org/enable-no-freeze.html#interactive-generate)
* [Connect](https://xrpl.org/enable-no-freeze.html#interactive-connect)
* [Send AccountSet](https://xrpl.org/enable-no-freeze.html#interactive-send\_accountset)
* [Wait](https://xrpl.org/enable-no-freeze.html#interactive-wait)
* [Confirm Settings](https://xrpl.org/enable-no-freeze.html#interactive-confirm\_settings)
