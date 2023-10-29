# 데스티네이션 태그 필요

데스티네이션 태그 설정은 사람들이 돈을 보내고 데스티네이션 태그를 사용하여 신용할 사람을 식별하는 것을 잊는 것을 방지하기 위해 여러 사람 또는 목적을 위해 잔액을 호스팅하는 주소를 위해 설계되었습니다. 주소에서 이 설정이 활성화되면 XRP Ledger는 데스티네이션 태그를 지정하지 않으면 주소에 대한 지불을 거부합니다.

이 튜토리얼에서는 계정에서 데스티네이션 태그 필요 플래그를 활성화하는 방법을 보여줍니다.

{% hint style="info" %}
Note:

특정 데스티네이션 태그의 의미는 전적으로 XRP Ledger 위에 구축된 논리에 달려 있습니다. Ledger는 시스템에서 특정 태그가 유효한지 여부를 알 방법이 없으므로 잘못된 데스티네이션 태그가 있는 트랜잭션을 받을 준비가 되어 있어야 합니다. 일반적으로 여기에는 잘못 이루어진 결제를 추적하고 그에 따라 고객에게 신용을 제공하고 원하지 않는 결제를 거부할 수 있는 고객 지원 경험을 제공하는 것이 포함됩니다.
{% endhint %}

## 요구 사항 <a href="#prerequisites" id="prerequisites"></a>

* 당신은 자금 지원을 받은 XRP Ledger 계정이 필요하며, 주소, 비밀 키 및 일부 XRP가 필요합니다. 생산을 위해 동일한 주소와 비밀번호를 지속적으로 사용할 수 있습니다. 이 튜토리얼에서는 필요에 따라 새 테스트 자격 증명을 생성할 수 있습니다.
* XRP Ledger 네트워크에 연결해야 합니다. 이 튜토리얼에서 볼 수 있듯이 테스트를 위해 공용 서버를 사용할 수 있습니다.
* 선호하는 클라이언트 라이브러리에 대한 시작하기 지침을 숙지해야 합니다. 이 페이지에서는 다음에 대한 예를 제공합니다.
  * xrpl.js 라이브러리가 있는 JavaScript입니다. 설정 단계는 JavaScript 사용 시작을 참조하세요.
  * xrpl-py 라이브러리가 있는 Python입니다. 설정 단계는 Python 사용 시작을 참조하세요.
  * 또한 설정 없이 브라우저에서 대화형 단계를 읽고 사용할 수 있습니다.

## 예제 코드 <a href="#example-code" id="example-code"></a>

이 튜토리얼의 모든 단계에 대한 전체 샘플 코드는 MIT 라이선스 에 따라 사용할 수 있습니다.

* 코드 샘플: 이 웹 사이트의 소스 저장소에 데스티네이션 태그 필요를 참조하세요.

## 단계 <a href="#steps" id="steps"></a>

#### 1. 자격 증명 받기 <a href="#1-get-credentials" id="1-get-credentials"></a>

XRP Ledger에서 거래하려면 주소와 비밀 키, 그리고 약간의 XRP가 필요합니다. 개발 목적으로 다음 인터페이스를 사용하여 얻을 수 있습니다.

* [생성하다](https://xrpl.org/require-destination-tags.html#interactive-generate)
* [연결하다](https://xrpl.org/require-destination-tags.html#interactive-connect)
* [AccountSet 보내기](https://xrpl.org/require-destination-tags.html#interactive-send\_accountset)
* [기다리다](https://xrpl.org/require-destination-tags.html#interactive-wait)
* [설정 확인](https://xrpl.org/require-destination-tags.html#interactive-confirm\_settings)
* [테스트 지불](https://xrpl.org/require-destination-tags.html#interactive-test\_payments)

{% hint style="info" %}
Caution:

Ripple은 테스트 목적으로만 Testnet과 Devnet을 제공하며 때로는 모든 잔액과 함께 이러한 테스트 네트워크의 상태를 재설정합니다. 예방 조치로 Testnet/Devnet과 Mainnet에서 동일한 주소를 사용하지 마세요.
{% endhint %}

프로덕션에 바로 사용할 수 있는 소프트웨어를 구축 할 때 기존 계정을 사용하고 보안 서명 구성을 사용하여 키를 관리해야 합니다.

## 2. 네트워크에 연결 <a href="#2-connect-to-the-network" id="2-connect-to-the-network"></a>

트랜잭션을 제출하려면 네트워크에 연결되어 있어야 합니다. 다음 코드는 공개 XRP Ledger Testnet 서버에 지원되는 [클라이언트 라이브러리](https://xrpl.org/client-libraries.html) 에 연결하는 방법을 보여줍니다.

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

{% tab title="Python" %}
```python
import asyncio
from xrpl.asyncio.clients import AsyncWebsocketClient


async def main():
    # Define the network client
    async with AsyncWebsocketClient("wss://s.altnet.rippletest.net:51233") as client:
        # inside the context the client is open

        # ... custom code goes here

    # after exiting the context, the client is closed


asyncio.run(main())
```
{% endtab %}
{% endtabs %}

이 튜토리얼에서는 다음 버튼을 클릭하여 연결합니다.

* [생성하다](https://xrpl.org/require-destination-tags.html#interactive-generate)
* [연결하다](https://xrpl.org/require-destination-tags.html#interactive-connect)
* [AccountSet 보내기](https://xrpl.org/require-destination-tags.html#interactive-send\_accountset)
* [기다리다](https://xrpl.org/require-destination-tags.html#interactive-wait)
* [설정 확인](https://xrpl.org/require-destination-tags.html#interactive-confirm\_settings)
* [테스트 지불](https://xrpl.org/require-destination-tags.html#interactive-test\_payments)

## 3. AccountSet 트랜잭션 보내기

RequireDest 플래그를 사용하려면 AccountSet 트랜잭션의 SetFlag 필드에서 aspRequireDest 값(1)을 설정합니다. 트랜잭션을 전송하려면 먼저 필요한 필드를 모두 입력하도록 준비한 다음 계정의 비밀 키로 서명하고 마지막으로 네트워크에 제출합니다.

예를 들어:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// Send AccountSet transaction -----------------------------------------------
  const prepared = await client.autofill({
    "TransactionType": "AccountSet",
    "Account": wallet.address,
    "SetFlag": xrpl.AccountSetAsfFlags.asfRequireDest
  })
  console.log("Prepared transaction:", prepared)

  const signed = wallet.sign(prepared)
  console.log("Transaction hash:", signed.hash)

  const submit_result = await client.submitAndWait(signed.tx_blob)
  console.log("Submit result:", submit_result)
```
{% endtab %}

{% tab title="Python" %}
```python
        # Send AccountSet transaction -----------------------------------------------
        tx = AccountSet(
            account=wallet.address,
            set_flag=AccountSetAsfFlag.ASF_REQUIRE_DEST,
        )

        # Sign and autofill the transaction (ready to submit)
        signed_tx = await autofill_and_sign(tx, client, wallet)
        print("Transaction hash:", signed_tx.get_hash())

        # Submit the transaction and wait for response (validated or rejected)
        print("Submitting transaction...")
        submit_result = await submit_and_wait(signed_tx, client)
        print("Submit result:", submit_result)
```
{% endtab %}
{% endtabs %}

* [생성하다](https://xrpl.org/require-destination-tags.html#interactive-generate)
* [연결하다](https://xrpl.org/require-destination-tags.html#interactive-connect)
* [AccountSet 보내기](https://xrpl.org/require-destination-tags.html#interactive-send\_accountset)
* [기다리다](https://xrpl.org/require-destination-tags.html#interactive-wait)
* [설정 확인](https://xrpl.org/require-destination-tags.html#interactive-confirm\_settings)
* [테스트 지불](https://xrpl.org/require-destination-tags.html#interactive-test\_payments)

## 4. 유효성 검사 대기 <a href="#4-wait-for-validation" id="4-wait-for-validation"></a>

대부분의 트랜잭션은 제출된 후 다음 ledger 버전으로 승인됩니다. 즉, 트랜잭션 결과가 최종 결과가 되기까지 4-7초가 걸릴 수 있습니다. XRP Ledger가 사용 중이거나 네트워크 연결 상태가 좋지 않아 트랜잭션이 네트워크 전체에 전달되는 것이 지연되면 트랜잭션을 확인하는 데 시간이 더 오래 걸릴 수 있습니다. (거래 만료 설정 방법에 대한 자세한 내용은 신뢰할 수 있는 거래 제출을 참조하세요.)

* [생성하다](https://xrpl.org/require-destination-tags.html#interactive-generate)
* [연결하다](https://xrpl.org/require-destination-tags.html#interactive-connect)
* [AccountSet 보내기](https://xrpl.org/require-destination-tags.html#interactive-send\_accountset)
* [기다리다](https://xrpl.org/require-destination-tags.html#interactive-wait)
* [설정 확인](https://xrpl.org/require-destination-tags.html#interactive-confirm\_settings)
* [테스트 지불](https://xrpl.org/require-destination-tags.html#interactive-test\_payments)

| 거래 ID:                   | (없음)      |
| ------------------------ | --------- |
| 최신 검증 원장 인덱스:            | (연결되지 않은) |
| 제출 시점의 원장 인덱스:           | (제출하지 않은) |
| 거래 `LastLedgerSequence`: | (미준비)     |

## 5. 계정 설정 확인 <a href="#5-confirm-account-settings" id="5-confirm-account-settings"></a>

거래가 확인된 후 계정 설정을 확인하여 데스티네이션 태그 필요 플래그가 활성화되었는지 확인할 수 있습니다.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// Confirm Account Settings --------------------------------------------------
  let account_info = await client.request({
    "command": "account_info",
    "account": address,
    "ledger_index": "validated"
  })
  const flags = xrpl.parseAccountFlags(account_info.result.account_data.Flags)
  console.log(JSON.stringify(flags, null, 2))
  if (flags.lsfRequireDestTag) {
    console.log("Require Destination Tag is enabled.")
  } else {
    console.log("Require Destination Tag is DISABLED.")
  }
```
{% endtab %}

{% tab title="Python" %}
```python
        # Confirm Account Settings --------------------------------------------------
        print("Requesting account information...")
        account_info = await client.request(
            AccountInfo(
                account=wallet.address,
                ledger_index="validated",
            )
        )

        # Verify that the AccountRoot lsfRequireDestTag flag is set
        flags = account_info.result["account_data"]["Flags"]
        if flags & 0x00020000 != 0:
            print(f"Require Destination Tag for account {wallet.address} is enabled.")
        else:
            print(f"Require Destination Tag for account {wallet.address} is DISABLED.")
```
{% endtab %}
{% endtabs %}

* [생성하다](https://xrpl.org/require-destination-tags.html#interactive-generate)
* [연결하다](https://xrpl.org/require-destination-tags.html#interactive-connect)
* [AccountSet 보내기](https://xrpl.org/require-destination-tags.html#interactive-send\_accountset)
* [기다리다](https://xrpl.org/require-destination-tags.html#interactive-wait)
* [설정 확인](https://xrpl.org/require-destination-tags.html#interactive-confirm\_settings)
* [테스트 지불](https://xrpl.org/require-destination-tags.html#interactive-test\_payments)

다른 주소에서 테스트 트랜잭션을 보내 설정이 예상대로 작동하는지 확인할 수 있습니다. 데스티네이션 태그로 결제하면 성공하고 데스티네이션 태그 없이 보내면 tecdDST\_TAG\_NEDED라는 오류 코드로 실패합니다.
