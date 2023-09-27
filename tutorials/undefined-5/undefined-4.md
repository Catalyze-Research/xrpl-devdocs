# 신뢰선 동결하기

이 튜토리얼에서는 개별 트랜잭션 라인을 동결하는 단계를 보여드리겠습니다. XRP Ledger의 토큰 발행자는 해당 계정이 의심스러운 활동을 하는 경우 특정 트랜잭션 상대방에 대한 신뢰선을 동결할 수 있습니다.

{% hint style="info" %}
Tip:

다시 한 번 말씀드리자면, 동결은 XRP가 아닌 발행된 토큰에만 적용됩니다.&#x20;
{% endhint %}

## 요구  조건

* XRP Ledger 네트워크에 연결해야 합니다. 이 튜토리얼에 표시된 대로 공용 서버를 사용해 테스트할 수 있습니다.
* 선호하는 클라이언트 라이브러리에 대한 시작하기 지침을 숙지하고 있어야 합니다. 이 페이지에서는 다음에 대한 예제를 제공합니다:
  * xrpl.js 라이브러리를 사용한 JavaScript. 설정 단계는 JavaScript를 사용하여 시작하기를 참조하세요.
* 이 튜토리얼에서는 XRP Ledger에서 이미 토큰을 발행했다고 가정합니다.
* 개별 신뢰선을 동결하는 기능을 포기하는 동결 금지 설정을 활성화할 수 없습니다.

## 예제 코드

이 튜토리얼의 모든 단계에 대한 전체 샘플 코드는 MIT 라이선스 에 따라 사용할 수 있습니다.

* 코드 샘플을 참조하세요: 이 웹사이트의 소스 저장소에서 고정하기를 참조하세요.

## 단계

## 1. 자격증명 가져오기

XRP Ledger에서 트랜잭션하려면 주소와 비밀 키, 그리고 약간의 XRP가 필요합니다. "콜드" 주소와 "핫" 주소를 별도로 보유하는 모범 사례를 사용하는 경우, 토큰 발행자인 콜드 주소의 키가 필요합니다.

* [Generate](https://xrpl.org/freeze-a-trust-line.html#interactive-generate)
* [Connect](https://xrpl.org/freeze-a-trust-line.html#interactive-connect)
* [Choose Trust Line](https://xrpl.org/freeze-a-trust-line.html#interactive-choose\_trust\_line)
* [Send TrustSet to Freeze](https://xrpl.org/freeze-a-trust-line.html#interactive-send\_trustset\_to\_freeze)
* [Wait](https://xrpl.org/freeze-a-trust-line.html#interactive-wait)
* [Check Freeze Status](https://xrpl.org/freeze-a-trust-line.html#interactive-check\_freeze\_status)
* [Send TrustSet to End Freeze](https://xrpl.org/freeze-a-trust-line.html#interactive-send\_trustset\_to\_end\_freeze)
* [Wait (again)](https://xrpl.org/freeze-a-trust-line.html#interactive-wait\_again)

{% hint style="info" %}
Caution:

Ripple은 testnet devnet을 테스트 목적으로만 제공하며, 때때로 이러한 테스트 네트워크의 상태를 모든 잔액과 함께 초기화하기도 합니다. 예방책으로 testnet/devnet 과 mainnet에서 동일한 주소를 사용하지 마시기 바랍니다.
{% endhint %}

## 2. 네트워크에 연결하기

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

{% tab title="WebSocket" %}
```json
(Connect to wss:// URL of an XRP Ledger server using your preferred client.)
```
{% endtab %}
{% endtabs %}

* [Generate](https://xrpl.org/freeze-a-trust-line.html#interactive-generate)
* [Connect](https://xrpl.org/freeze-a-trust-line.html#interactive-connect)
* [Choose Trust Line](https://xrpl.org/freeze-a-trust-line.html#interactive-choose\_trust\_line)
* [Send TrustSet to Freeze](https://xrpl.org/freeze-a-trust-line.html#interactive-send\_trustset\_to\_freeze)
* [Wait](https://xrpl.org/freeze-a-trust-line.html#interactive-wait)
* [Check Freeze Status](https://xrpl.org/freeze-a-trust-line.html#interactive-check\_freeze\_status)
* [Send TrustSet to End Freeze](https://xrpl.org/freeze-a-trust-line.html#interactive-send\_trustset\_to\_end\_freeze)
* [Wait (again)](https://xrpl.org/freeze-a-trust-line.html#interactive-wait\_again)

## 3. 신뢰선 선택

트랜잭션당 하나의 신뢰선만 동결할 수 있으므로 어떤 신뢰선을 원하는지 알아야 합니다. 각 신뢰선은 다음 세 가지로 고유하게 식별됩니다:

* 본인의 주소.
* 신뢰선을 통해 본인에게 연결된 계정의 주소.
* 신뢰선의 화폐 코드.

두 계정 사이에 각각 다른 화폐를 사용하는 여러 개의 신뢰선이 있을 수 있습니다. 특정 계정이 악의적으로 행동하는 것으로 의심되는 경우 계정 간의 모든 신뢰선을 한 번에 하나씩 동결할 수 있습니다. 한 쌍의 계정에 account\_lines 메소드를 사용하여 해당 계정 간의 모든 신뢰선을 찾은 다음, 그 결과 중에서 동결할 신뢰선을 선택합니다. 예를 들면 다음과 같습니다:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// Look up current trust lines -----------------------------------------------
  const issuing_address = wallet.address
  const address_to_freeze = 'rhPuJPcd9kcSRCGHWudV3tjUuTvvysi6sv'
  const currency_to_freeze = 'FOO'

  console.log('Looking up', currency_to_freeze, 'trust line from',
              issuing_address, 'to', address_to_freeze)
  const account_lines = await client.request({
    "command": "account_lines",
    "account": issuing_address,
    "peer": address_to_freeze,
    "ledger_index": "validated"
  })
  const trustlines = account_lines.result.lines
  console.log("Found lines:", trustlines)

  // Find the trust line for our currency_to_freeze ----------------------------
  let trustline = null
  for (let i = 0; i < trustlines.length; i++) {
    if(trustlines[i].currency === currency_to_freeze) {
      trustline = trustlines[i]
      break
    }
  }

  if (trustline === null) {
    console.error(`Couldn't find a ${currency_to_freeze} trustline between
                  ${issuing_address} and ${address_to_freeze}`)
    return
  }a
```
{% endtab %}

{% tab title="WebSocket" %}
```json
Example Request:

{
  "id": "example_look_up_trust_lines",
  "command": "account_lines",
  "account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
  "peer": "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW",
  "ledger_index": "validated"
}

// Example Response:

{
  "id": "example_look_up_trust_lines",
  "result": {
    "account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "ledger_hash": "AF5C6E6FBC44183D8662C7F5BF88D52F99738A0E66FF07FC7B5A516AC8EA1B37",
    "ledger_index": 67268474,
    "lines": [
      {
        "account": "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW",
        "balance": "0",
        "currency": "USD",
        "limit": "0",
        "limit_peer": "110",
        "quality_in": 0,
        "quality_out": 0
      }
    ],
    "validated": true
  },
  "status": "success",
  "type": "response"
}
```
{% endtab %}
{% endtabs %}

이 튜토리얼에서는 두 번째 테스트 주소가 다음 예시에서 볼 수 있는 것처럼 화폐 'FOO'에 대한 테스트 주소에 대한 신뢰선을 만들었습니다:

* [Generate](https://xrpl.org/freeze-a-trust-line.html#interactive-generate)
* [Connect](https://xrpl.org/freeze-a-trust-line.html#interactive-connect)
* [Choose Trust Line](https://xrpl.org/freeze-a-trust-line.html#interactive-choose\_trust\_line)
* [Send TrustSet to Freeze](https://xrpl.org/freeze-a-trust-line.html#interactive-send\_trustset\_to\_freeze)
* [Wait](https://xrpl.org/freeze-a-trust-line.html#interactive-wait)
* [Check Freeze Status](https://xrpl.org/freeze-a-trust-line.html#interactive-check\_freeze\_status)
* [Send TrustSet to End Freeze](https://xrpl.org/freeze-a-trust-line.html#interactive-send\_trustset\_to\_end\_freeze)
* [Wait (again)](https://xrpl.org/freeze-a-trust-line.html#interactive-wait\_again)

## 4. 신뢰선 동결을 위해 TrustSet 트랜잭션 보내기

특정 트랜잭션 라인에서 개별 동결을 활성화 또는 비활성화하려면 tfSetFreeze 플래그가 활성화된 TrustSet 트랜잭션을 전송합니다. 트랜잭션의 필드는 다음과 같아야 합니다:

| 필드                       | 값   | 설명                                                                  |
| ------------------------ | --- | ------------------------------------------------------------------- |
| `Account`                | 문자열 | 발급 계정의 주소입니다.                                                       |
| `TransactionType`        | 문자열 | `TrustSet`                                                          |
| `LimitAmount`            | 객체  | 동결할 신뢰선을 정의하는 객체입니다.                                                |
| `LimitAmount`.`currency` | 문자열 | 신뢰선의 화폐(XRP일 수 없음)                                                  |
| `LimitAmount`.`issuer`   | 문자열 | 동결할 트랜잭션 상대방의 XRP Ledger 주소.                                        |
| `LimitAmount`.`value`    | 문자열 | 트랜잭션 상대방이 당신에게 발행하도록 신뢰하는 화폐 금액(따옴표로 표시됨)입니다. 발행자의 경우 일반적으로 "0"입니다. |
| `Flags`                  | 숫자  | 동결을 활성화하려면 tfSetFreeze 비트(0x00100000)를 켭니다.                         |

항상 그렇듯이 트랜잭션을 전송하려면 필요한 모든 필드를 채우고 암호화 키로 서명하여 트랜잭션을 준비한 다음 네트워크에 제출합니다. 예를 들어:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// Send a TrustSet transaction to set an individual freeze -------------------
  // Construct a TrustSet, preserving our existing limit value
  const trust_set = {
    "TransactionType": 'TrustSet',
    "Account": issuing_address,
    "LimitAmount": {
      "value": trustline.limit,
      "currency": trustline.currency,
      "issuer": trustline.account
    },
    "Flags": xrpl.TrustSetFlags.tfSetFreeze
  }

  // Best practice for JavaScript users: use validate(tx_json) to confirm
  // that a transaction is well-formed or throw ValidationError if it isn't.
  xrpl.validate(trust_set)

  console.log('Submitting TrustSet tx:', trust_set)
  const result = await client.submitAndWait(trust_set, { wallet: wallet })
  console.log("Transaction result:", result)

  // Confirm trust line status -------------------------------------------------
  const account_lines_2 = await client.request({
    "command": "account_lines",
    "account": issuing_address,
    "peer": address_to_freeze,
    "ledger_index": "validated"
  })
  const trustlines_2 = account_lines_2.result.lines

  let line = null
  for (let i = 0; i < trustlines_2.length; i++) {
    if(trustlines_2[i].currency === currency_to_freeze) {
      line = trustlines_2[i]
      console.log(`Status of ${currency_to_freeze} line between
          ${issuing_address} and ${address_to_freeze}:
          ${JSON.stringify(line, null, 2)}`)
      if (line.freeze === true) {
        console.log(`✅ Line is frozen.`)
      } else {
        console.error(`❌ Line is NOT FROZEN.`)
      }
    }
  }
  if (line === null) {
    console.error(`Couldn't find a ${CURRENCY_TO_FREEZE} line between
        ${issuing_address} and ${address_to_freeze}.`)
  }
```
{% endtab %}

{% tab title="WebSocket" %}
```json
{
  "id": "example_freeze_individual_line",
  "command": "submit",
  "tx_json": {
    "TransactionType": "TrustSet",
    "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "Fee": "12",
    "Flags": 1048576,
    "LastLedgerSequence": 18103014,
    "LimitAmount": {
      "currency": "USD",
      "issuer": "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW",
      "value": "0"
    },
    "Sequence": 340
  },
  "secret": "s████████████████████████████"
}
```
{% endtab %}
{% endtabs %}

* [Generate](https://xrpl.org/freeze-a-trust-line.html#interactive-generate)
* [Connect](https://xrpl.org/freeze-a-trust-line.html#interactive-connect)
* [Choose Trust Line](https://xrpl.org/freeze-a-trust-line.html#interactive-choose\_trust\_line)
* [Send TrustSet to Freeze](https://xrpl.org/freeze-a-trust-line.html#interactive-send\_trustset\_to\_freeze)
* [Wait](https://xrpl.org/freeze-a-trust-line.html#interactive-wait)
* [Check Freeze Status](https://xrpl.org/freeze-a-trust-line.html#interactive-check\_freeze\_status)
* [Send TrustSet to End Freeze](https://xrpl.org/freeze-a-trust-line.html#interactive-send\_trustset\_to\_end\_freeze)
* [Wait (again)](https://xrpl.org/freeze-a-trust-line.html#interactive-wait\_again)

{% hint style="info" %}
Note:

동일한 트랜잭션 상대방과 서로 다른 화폐로 여러 개의 신뢰선을 동결하려면 각 신뢰선에 대해 이 단계를 반복하세요. 각 트랜잭션에 다른 시퀀스 번호를 사용하면 단일 Ledger에 여러 트랜잭션을 전송할 수 있습니다
{% endhint %}

## 5. 유효성 검사 대기

대부분의 트랜잭션은 제출된 후 다음 Ledger 버전으로 승인되므로, 트랜잭션 결과가 최종적으로 확정되기까지 4\~7초가 소요될 수 있습니다. XRP Ledger이 사용 중이거나 네트워크 연결 상태가 좋지 않아 트랜잭션이 네트워크를 통해 릴레이되는 것이 지연되는 경우, 트랜잭션이 확인되는 데 더 오랜 시간이 걸릴 수 있습니다. (트랜잭션 만료를 설정하는 방법에 대한 자세한 내용은 안정적인 트랜잭션 제출을 참조하세요.)

* [Generate](https://xrpl.org/freeze-a-trust-line.html#interactive-generate)
* [Connect](https://xrpl.org/freeze-a-trust-line.html#interactive-connect)
* [Choose Trust Line](https://xrpl.org/freeze-a-trust-line.html#interactive-choose\_trust\_line)
* [Send TrustSet to Freeze](https://xrpl.org/freeze-a-trust-line.html#interactive-send\_trustset\_to\_freeze)
* [Wait](https://xrpl.org/freeze-a-trust-line.html#interactive-wait)
* [Check Freeze Status](https://xrpl.org/freeze-a-trust-line.html#interactive-check\_freeze\_status)
* [Send TrustSet to End Freeze](https://xrpl.org/freeze-a-trust-line.html#interactive-send\_trustset\_to\_end\_freeze)
* [Wait (again)](https://xrpl.org/freeze-a-trust-line.html#interactive-wait\_again)

## 6. 신뢰선 동결 상태 확인

이 시점에서 트랜잭션 상대방의 신뢰선이 동결되어야 합니다. 다음 필드와 함께 account\_lines 메소드를 사용하여 모든 신뢰선의 동결 상태를 확인할 수 있습니다:

| 필드        | 값      | 설명                      |
| --------- | ------ | ----------------------- |
| `account` | String | 주소입니다. (이 경우 발급 주소입니다.) |
| `peer`    | String | 상대방의 주소입니다.             |

{% hint style="info" %}
Caution:

응답에는 두 계정 간의 모든 신뢰선이 포함됩니다. (각 화폐 코드는 서로 다른 신뢰선을 사용합니다.) 올바른 토큰에 대한 신뢰선을 확인해야 합니다.&#x20;
{% endhint %}

응답에서 "freeze": true 필드는 요청을 보낸 계정이 해당 신뢰선에서 개별 동결을 활성화했음을 나타냅니다. "freeze\_peer": true 필드는 요청의 상대방(피어)이 신뢰선을을 동결했음을 나타냅니다. 예를 들면 다음과 같습니다:

* [Generate](https://xrpl.org/freeze-a-trust-line.html#interactive-generate)
* [Connect](https://xrpl.org/freeze-a-trust-line.html#interactive-connect)
* [Choose Trust Line](https://xrpl.org/freeze-a-trust-line.html#interactive-choose\_trust\_line)
* [Send TrustSet to Freeze](https://xrpl.org/freeze-a-trust-line.html#interactive-send\_trustset\_to\_freeze)
* [Wait](https://xrpl.org/freeze-a-trust-line.html#interactive-wait)
* [Check Freeze Status](https://xrpl.org/freeze-a-trust-line.html#interactive-check\_freeze\_status)
* [Send TrustSet to End Freeze](https://xrpl.org/freeze-a-trust-line.html#interactive-send\_trustset\_to\_end\_freeze)
* [Wait (again)](https://xrpl.org/freeze-a-trust-line.html#interactive-wait\_again)

## 7. (선택 사항) TrustSet 트랜잭션을 전송하여 동결 종료하기

의심스러운 활동을 조사한 결과 양성이라고 판단되는 등 더 이상 신뢰선을 동결할 필요가 없다고 판단되면 처음에 신뢰선을 동결할 때와 거의 동일한 방법으로 개별 동결을 종료할 수 있습니다. 개별 동결을 종료하려면 tfClearFreeze 플래그를 활성화한 상태로 TrustSet 트랜잭션을 전송합니다. 트랜잭션의 다른 필드는 신뢰선을 동결할 때와 동일해야 합니다:

| Field                    | Value  | Description                                                         |
| ------------------------ | ------ | ------------------------------------------------------------------- |
| `Account`                | String | 발급 계정의 주소입니다.                                                       |
| `TransactionType`        | String | `TrustSet`                                                          |
| `LimitAmount`            | Object | 동결을 해제할 지정한도를 정의하는 객체입니다.                                           |
| `LimitAmount`.`currency` | String | 신뢰선의 화폐(XRP일 수 없음)                                                  |
| `LimitAmount`.`issuer`   | String | 동결을 해제할 트랜잭션. 상대방의 XRP Ledger 주소.                                   |
| `LimitAmount`.`value`    | String | 트랜잭션 상대방이 당신에게 발행하도록 신뢰하는 화폐 금액(따옴표로 표시됨)입니다. 발행자의 경우 일반적으로 "0"입니다. |
| `Flags`                  | Number | 개별 동결을 종료하려면 tfClearFreeze 비트(0x00200000)를 켭니다.                     |

항상 그렇듯이 트랜잭션을 전송하려면 필요한 모든 필드를 채우고 암호화 키로 서명하여 트랜잭션을 준비한 다음 네트워크에 제출합니다. 예를 들어:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// Clear the individual freeze -----------------------------------------------
  // We're reusing our TrustSet transaction from earlier with a different flag.
  trust_set.Flags = xrpl.TrustSetFlags.tfClearFreeze

  // Submit a TrustSet transaction to clear an individual freeze ---------------
  console.log('Submitting TrustSet tx:', trust_set)
  const result2 = await client.submitAndWait(trust_set, { wallet: wallet })
  console.log("Transaction result:", result2)

  console.log("Finished submitting. Now disconnecting.")
  await client.disconnect()
```
{% endtab %}

{% tab title="WebSocket" %}
```json
{
  "id": "example_end_individual_freeze",
  "command": "submit",
  "tx_json": {
    "TransactionType": "TrustSet",
    "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "Fee": "12",
    "Flags": 2097152,
    "LastLedgerSequence": 18105115,
    "LimitAmount": {
      "currency": "USD",
      "issuer": "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW",
      "value": "0"
    },
    "Sequence": 341
  },
  "secret": "s████████████████████████████"
}
```
{% endtab %}
{% endtabs %}

* [Generate](https://xrpl.org/freeze-a-trust-line.html#interactive-generate)
* [Connect](https://xrpl.org/freeze-a-trust-line.html#interactive-connect)
* [Choose Trust Line](https://xrpl.org/freeze-a-trust-line.html#interactive-choose\_trust\_line)
* [Send TrustSet to Freeze](https://xrpl.org/freeze-a-trust-line.html#interactive-send\_trustset\_to\_freeze)
* [Wait](https://xrpl.org/freeze-a-trust-line.html#interactive-wait)
* [Check Freeze Status](https://xrpl.org/freeze-a-trust-line.html#interactive-check\_freeze\_status)
* [Send TrustSet to End Freeze](https://xrpl.org/freeze-a-trust-line.html#interactive-send\_trustset\_to\_end\_freeze)
* [Wait (again)](https://xrpl.org/freeze-a-trust-line.html#interactive-wait\_again)

## 8. 유효성 검사 대기

이전과 마찬가지로 컨센서스에 의해 트랜잭션이 검증될 때까지 기다립니다.

* [Generate](https://xrpl.org/freeze-a-trust-line.html#interactive-generate)
* [Connect](https://xrpl.org/freeze-a-trust-line.html#interactive-connect)
* [Choose Trust Line](https://xrpl.org/freeze-a-trust-line.html#interactive-choose\_trust\_line)
* [Send TrustSet to Freeze](https://xrpl.org/freeze-a-trust-line.html#interactive-send\_trustset\_to\_freeze)
* [Wait](https://xrpl.org/freeze-a-trust-line.html#interactive-wait)
* [Check Freeze Status](https://xrpl.org/freeze-a-trust-line.html#interactive-check\_freeze\_status)
* [Send TrustSet to End Freeze](https://xrpl.org/freeze-a-trust-line.html#interactive-send\_trustset\_to\_end\_freeze)
* [Wait (again)](https://xrpl.org/freeze-a-trust-line.html#interactive-wait\_again)
