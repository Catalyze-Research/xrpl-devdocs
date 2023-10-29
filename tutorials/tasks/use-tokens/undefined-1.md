# 탈중앙화 거래소에서 거래

이 튜토리얼에서는 탈중앙화 거래소(DEX)에서 토큰을 사고 파는 방법을 설명합니다.

## 요구 조건

* XRP Ledger 네트워크에 연결해야 합니다. 이 튜토리얼에 표시된 대로 공용 서버를 사용하여 테스트할 수 있습니다.
* 선호하는 클라이언트 라이브러리에 대한 시작하기 지침을 숙지하고 있어야 합니다. 이 페이지에서는 다음에 대한 예제를 제공합니다:
  * xrpl.js 라이브러리를 사용한 JavaScript. 설정 단계는 JavaScript를 사용하여 시작하기를 참조하세요.
  * xrpl-py 라이브러리를 사용하는 Python. 설정 단계는 Python을 사용하여 시작하기를 참조하세요.
  * 설정 없이 브라우저에서 대화형 단계를 따라 읽고 사용할 수도 있습니다.

## 예제 코드

이 튜토리얼의 모든 단계에 대한 전체 샘플 코드는 MIT 라이선스 에 따라 사용할 수 있습니다.&#x20;

* 코드 샘플을 참조하세요: 이 웹사이트의 소스 저장소에서 탈중앙화 거래소에서 트랜잭션하기를 참조하세요.

## 단계

이 튜토리얼에서는 XRP를 판매하여 탈중앙화 거래소에서 대체 가능한 토큰을 구매하는 방법을 보여드립니다. (다른 유형의 트랜잭션도 가능하지만, 토큰을 판매하려면 먼저 토큰을 보유해야 합니다.) 이 튜토리얼에서 사용하는 예시 토큰은 다음과 같습니다:

## 1. 네트워크에 연결

트랜잭션을 제출하려면 네트워크에 연결해야 합니다. 또한 일부 언어(JavaScript 포함)는 Ledger에서 찾을 수 있는 화폐 금액에 대한 계산을 수행하기 위해 고정밀 숫자 라이브러리가 필요합니다. 다음 코드는 적절한 종속성을 갖춘 지원되는 클라이언트 라이브러리를 퍼블릭 XRP Ledger 테스트넷 서버에 연결하는 방법을 보여줍니다.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// In browsers, add the following <script> tags to the HTML to load dependencies
// instead of using require():
// <script src="https://unpkg.com/xrpl@2.2.0/build/xrpl-latest-min.js"></script>
// <script src='https://cdn.jsdelivr.net/npm/bignumber.js@9.0.2/bignumber.min.js'></script>
const xrpl = require('xrpl')
const BigNumber = require('bignumber.js')

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

{% hint style="info" %}
Tip:

이 튜토리얼의 자바스크립트 코드 샘플은 async/await 패턴을 사용합니다. await은 비동기 함수 내에서 사용해야 하므로 나머지 코드 샘플은 여기서 시작한 main() 함수 내에서 계속 사용하도록 작성되었습니다. 원하는 경우 async/await 대신 Promise 메소드 .then() 및 .catch()를 사용할 수도 있습니다.
{% endhint %}

이 튜토리얼에서는 다음 버튼을 클릭하여 연결합니다:

* [Connect](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-connect)
* [Generate](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-generate)
* [Look Up Offers](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-look\_up\_offers)
* [Send OfferCreate](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-send\_offercreate)
* [Wait](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-wait)
* [Check Metadata](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-check\_metadata)
* [Check Balances and Offers](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-check\_balances\_and\_offers)

## 2. 자격증명 가져오기

XRP Ledger에서 거래하려면 주소, 비밀 키, 약간의 XRP가 필요합니다. 개발 목적으로 다음 인터페이스를 사용해 이 정보를 얻을 수 있습니다:

* [Connect](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-connect)
* [Generate](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-generate)
* [Look Up Offers](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-look\_up\_offers)
* [Send OfferCreate](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-send\_offercreate)
* [Wait](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-wait)
* [Check Metadata](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-check\_metadata)
* [Check Balances and Offers](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-check\_balances\_and\_offers)

{% hint style="info" %}
Caution:

Ripple은 testnet과 devnet을 테스트 목적으로만 제공하며, 때때로 이러한 테스트 네트워크의 상태를 모든 잔액과 함께 초기화하기도 합니다. 예방책으로 testnet, devnet과 mainnet에서 동일한 주소를 사용하지 마시기 바랍니다.
{% endhint %}

프로덕션용 소프트웨어를 빌드할 때는 기존 계정을 사용하고 보안 서명 구성을 사용하여 키를 관리해야 합니다. 다음 코드는 키를 사용하기 위해 월렛 인스턴스를 만드는 방법을 보여줍니다:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// Get credentials from the Testnet Faucet -----------------------------------
  console.log("Requesting address from the Testnet faucet...")
  const wallet = (await client.fundWallet()).wallet
  console.log(`Got address ${wallet.address}.`)
  // To use existing credentials, you can load them from a seed value, for
  // example using an environment variable as follows:
  // const wallet = xrpl.Wallet.fromSeed(process.env['MY_SEED'])
```
{% endtab %}

{% tab title="Python" %}
```python
        # Get credentials from the Testnet Faucet -----------------------------------
        print("Requesting addresses from the Testnet faucet...")
        wallet = await generate_faucet_wallet(client, debug=True)
```
{% endtab %}
{% endtabs %}

## 3. 제안 조회

토큰을 구매하거나 판매하기 전에 다른 사람들이 토큰을 어떤 가격에 구매하고 판매하는지 조회하여 다른 사람들이 토큰을 어떻게 평가하는지 파악하고 싶을 것입니다. XRP Ledger에서 book\_offers 메소드를 사용해 모든 화폐 쌍에 대한 기존 제안을 조회할 수 있습니다.

{% hint style="info" %}
Tip:

엄밀히 말하면 이 단계는 제안을 제출하기 위한 필수 조건은 아니지만, 실제 가치가 있는 상품을 트랜잭션하기 전에 현재 상황을 확인하는 것은 좋은 습관입니다.
{% endhint %}

다음 코드는 기존 제안을 조회하고 제안한 제안과 비교하여 어떻게 체결될지 예측하는 방법을 보여줍니다:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// Define the proposed trade. ------------------------------------------------
  // Technically you don't need to specify the amounts (in the "value" field)
  // to look up order books using book_offers, but for this tutorial we reuse
  // these variables to construct the actual Offer later.
  const we_want = {
    currency: "TST",
    issuer: "rP9jPyP5kyvFRb6ZiRghAGw5u8SGAmU4bd",
    value: "25"
  }
  const we_spend = {
    currency: "XRP",
           // 25 TST * 10 XRP per TST * 15% financial exchange (FX) cost
    value: xrpl.xrpToDrops(25*10*1.15)
  }
  // "Quality" is defined as TakerPays / TakerGets. The lower the "quality"
  // number, the better the proposed exchange rate is for the taker.
  // The quality is rounded to a number of significant digits based on the
  // issuer's TickSize value (or the lesser of the two for token-token trades.)
  const proposed_quality = BigNumber(we_spend.value) / BigNumber(we_want.value)

  // Look up Offers. -----------------------------------------------------------
  // To buy TST, look up Offers where "TakerGets" is TST:
  const orderbook_resp = await client.request({
    "command": "book_offers",
    "taker": wallet.address,
    "ledger_index": "current",
    "taker_gets": we_want,
    "taker_pays": we_spend
  })
  console.log(JSON.stringify(orderbook_resp.result, null, 2))

  // Estimate whether a proposed Offer would execute immediately, and...
  // If so, how much of it? (Partial execution is possible)
  // If not, how much liquidity is above it? (How deep in the order book would
  //    other Offers have to go before ours would get taken?)
  // Note: These estimates can be thrown off by rounding if the token issuer
  // uses a TickSize setting other than the default (15). In that case, you
  // can increase the TakerGets amount of your final Offer to compensate.

  const offers = orderbook_resp.result.offers
  const want_amt = BigNumber(we_want.value)
  let running_total = BigNumber(0)
  if (!offers) {
    console.log(`No Offers in the matching book.
                 Offer probably won't execute immediately.`)
  } else {
    for (const o of offers) {
      if (o.quality <= proposed_quality) {
        console.log(`Matching Offer found, funded with ${o.owner_funds}
            ${we_want.currency}`)
        running_total = running_total.plus(BigNumber(o.owner_funds))
        if (running_total >= want_amt) {
          console.log("Full Offer will probably fill")
          break
        }
      } else {
        // Offers are in ascending quality order, so no others after this
        // will match, either
        console.log(`Remaining orders too expensive.`)
        break
      }
    }
    console.log(`Total matched:
          ${Math.min(running_total, want_amt)} ${we_want.currency}`)
    if (running_total > 0 && running_total < want_amt) {
      console.log(`Remaining ${want_amt - running_total} ${we_want.currency}
            would probably be placed on top of the order book.`)
    }
  }

  if (running_total == 0) {
    // If part of the Offer was expected to cross, then the rest would be placed
    // at the top of the order book. If none did, then there might be other
    // Offers going the same direction as ours already on the books with an
    // equal or better rate. This code counts how much liquidity is likely to be
    // above ours.

    // Unlike above, this time we check for Offers going the same direction as
    // ours, so TakerGets and TakerPays are reversed from the previous
    // book_offers request.
    const orderbook2_resp = await client.request({
      "command": "book_offers",
      "taker": wallet.address,
      "ledger_index": "current",
      "taker_gets": we_spend,
      "taker_pays": we_want
    })
    console.log(JSON.stringify(orderbook2_resp.result, null, 2))

    // Since TakerGets/TakerPays are reversed, the quality is the inverse.
    // You could also calculate this as 1/proposed_quality.
    const offered_quality = BigNumber(we_want.value) / BigNumber(we_spend.value)

    const offers2 = orderbook2_resp.result.offers
    let tally_currency = we_spend.currency
    if (tally_currency == "XRP") { tally_currency = "drops of XRP" }
    let running_total2 = 0
    if (!offers2) {
      console.log(`No similar Offers in the book. Ours would be the first.`)
    } else {
      for (const o of offers2) {
        if (o.quality <= offered_quality) {
          console.log(`Existing offer found, funded with
                ${o.owner_funds} ${tally_currency}`)
          running_total2 = running_total2.plus(BigNumber(o.owner_funds))
        } else {
          console.log(`Remaining orders are below where ours would be placed.`)
          break
        }
      }
      console.log(`Our Offer would be placed below at least
            ${running_total2} ${tally_currency}`)
      if (running_total > 0 && running_total < want_amt) {
        console.log(`Remaining ${want_amt - running_total} ${tally_currency}
              will probably be placed on top of the order book.`)
      }
    }
  }
```
{% endtab %}

{% tab title="Python" %}
```python
        # Define the proposed trade. ------------------------------------------------
        # Technically you don't need to specify the amounts (in the "value" field)
        # to look up order books using book_offers, but for this tutorial we reuse
        # these variables to construct the actual Offer later.
        #
        # Note that XRP is represented as drops, whereas any other currency is
        # represented as a decimal value.
        we_want = {
            "currency": IssuedCurrency(
                currency="TST",
                issuer="rP9jPyP5kyvFRb6ZiRghAGw5u8SGAmU4bd"
            ),
            "value": "25",
        }

        we_spend = {
            "currency": XRP(),
            # 25 TST * 10 XRP per TST * 15% financial exchange (FX) cost
            "value": xrp_to_drops(25 * 10 * 1.15),
        }

        # "Quality" is defined as TakerPays / TakerGets. The lower the "quality"
        # number, the better the proposed exchange rate is for the taker.
        # The quality is rounded to a number of significant digits based on the
        # issuer's TickSize value (or the lesser of the two for token-token trades).
        proposed_quality = Decimal(we_spend["value"]) / Decimal(we_want["value"])

        # Look up Offers. -----------------------------------------------------------
        # To buy TST, look up Offers where "TakerGets" is TST:
        print("Requesting orderbook information...")
        orderbook_info = await client.request(
            BookOffers(
                taker=wallet.address,
                ledger_index="current",
                taker_gets=we_want["currency"],
                taker_pays=we_spend["currency"],
                limit=10,
            )
        )
        print(f"Orderbook:\n{pprint.pformat(orderbook_info.result)}")

        # Estimate whether a proposed Offer would execute immediately, and...
        # If so, how much of it? (Partial execution is possible)
        # If not, how much liquidity is above it? (How deep in the order book would
        # other Offers have to go before ours would get taken?)
        # Note: These estimates can be thrown off by rounding if the token issuer
        # uses a TickSize setting other than the default (15). In that case, you
        # can increase the TakerGets amount of your final Offer to compensate.

        offers = orderbook_info.result.get("offers", [])
        want_amt = Decimal(we_want["value"])
        running_total = Decimal(0)
        if len(offers) == 0:
            print("No Offers in the matching book. Offer probably won't execute immediately.")
        else:
            for o in offers:
                if Decimal(o["quality"]) <= proposed_quality:
                    print(f"Matching Offer found, funded with {o.get('owner_funds')} "
                          f"{we_want['currency']}")
                    running_total += Decimal(o.get("owner_funds", Decimal(0)))
                    if running_total >= want_amt:
                        print("Full Offer will probably fill")
                        break
                else:
                    # Offers are in ascending quality order, so no others after this
                    # will match either
                    print("Remaining orders too expensive.")
                    break

            print(f"Total matched: {min(running_total, want_amt)} {we_want['currency']}")
            if 0 < running_total < want_amt:
                print(f"Remaining {want_amt - running_total} {we_want['currency']} "
                      "would probably be placed on top of the order book.")

        if running_total == 0:
            # If part of the Offer was expected to cross, then the rest would be placed
            # at the top of the order book. If none did, then there might be other
            # Offers going the same direction as ours already on the books with an
            # equal or better rate. This code counts how much liquidity is likely to be
            # above ours.
            #
            # Unlike above, this time we check for Offers going the same direction as
            # ours, so TakerGets and TakerPays are reversed from the previous
            # book_offers request.

            print("Requesting second orderbook information...")
            orderbook2_info = await client.request(
                BookOffers(
                    taker=wallet.address,
                    ledger_index="current",
                    taker_gets=we_spend["currency"],
                    taker_pays=we_want["currency"],
                    limit=10,
                )
            )
            print(f"Orderbook2:\n{pprint.pformat(orderbook2_info.result)}")

            # Since TakerGets/TakerPays are reversed, the quality is the inverse.
            # You could also calculate this as 1 / proposed_quality.
            offered_quality = Decimal(we_want["value"]) / Decimal(we_spend["value"])

            tally_currency = we_spend["currency"]
            if isinstance(tally_currency, XRP):
                tally_currency = f"drops of {tally_currency}"

            offers2 = orderbook2_info.result.get("offers", [])
            running_total2 = Decimal(0)
            if len(offers2) == 0:
                print("No similar Offers in the book. Ours would be the first.")
            else:
                for o in offers2:
                    if Decimal(o["quality"]) <= offered_quality:
                        print(f"Existing offer found, funded with {o.get('owner_funds')} "
                              f"{tally_currency}")
                        running_total2 += Decimal(o.get("owner_funds", Decimal(0)))
                    else:
                        print("Remaining orders are below where ours would be placed.")
                        break

                print(f"Our Offer would be placed below at least {running_total2} "
                      f"{tally_currency}")
                if 0 < running_total2 < want_amt:
                    print(f"Remaining {want_amt - running_total2} {tally_currency} "
                          "will probably be placed on top of the order book.")
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Note:

XRP Ledger의 다른 사용자도 언제든지 트랜잭션을 할 수 있으므로, 이는 다른 변화가 없을 경우의 추정치일 뿐입니다. 트랜잭션이 최종적으로 체결될 때까지는 트랜잭션의 결과가 보장되지 않습니다.
{% endhint %}

다음 블록은 이러한 계산이 실제로 작동하는 모습을 보여줍니다:

* [Connect](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-connect)
* [Generate](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-generate)
* [Look Up Offers](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-look\_up\_offers)
* [Send OfferCreate](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-send\_offercreate)
* [Wait](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-wait)
* [Check Metadata](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-check\_metadata)
* [Check Balances and Offers](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-check\_balances\_and\_offers)

## 4. 제안 보내기 트랜잭션 생성

실제로 트랜잭션을 하려면 제안 생성 트랜잭션을 전송합니다. 이 경우 XRP를 사용해 TST를 구매하고자 하므로 다음과 같이 매개변수를 설정해야 합니다:

| 필드          | 유형                                                                                        | 설명                                                                                                           |
| ----------- | ----------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| `TakerPays` | [Token Amount object](https://xrpl.org/basic-data-types.html#specifying-currency-amounts) | TakerPays 토큰 금액 객체 총 구매하고자 하는 화폐 금액입니다. 이 튜토리얼에서는 rP9jPyP5kyvFRb6ZiRghAGw5u8SGAmU4bd가 발행한 일정 금액의 TST를 구매합니다. |
| `TakerGets` | [XRP, in drops](https://xrpl.org/basic-data-types.html#specifying-currency-amounts)       | 총 지불할 화폐의 양을 지정합니다. 이 튜토리얼에서는 TST당 약 11.5 XRP 또는 그 이상을 지정해야 합니다.                                             |

다음 코드는 트랜잭션을 준비하고, 서명하고, 제출하는 방법을 보여줍니다:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// Send OfferCreate transaction ----------------------------------------------

  // For this tutorial, we already know that TST is pegged to
  // XRP at a rate of approximately 10:1 plus spread, so we use
  // hard-coded TakerGets and TakerPays amounts.

  const offer_1 = {
    "TransactionType": "OfferCreate",
    "Account": wallet.address,
    "TakerPays": we_want,
    "TakerGets": we_spend.value // since it's XRP
  }

  const prepared = await client.autofill(offer_1)
  console.log("Prepared transaction:", JSON.stringify(prepared, null, 2))
  const signed = wallet.sign(prepared)
  console.log("Sending OfferCreate transaction...")
  const result = await client.submitAndWait(signed.tx_blob)
  if (result.result.meta.TransactionResult == "tesSUCCESS") {
    console.log(`Transaction succeeded:
          https://testnet.xrpl.org/transactions/${signed.hash}`)
  } else {
    throw `Error sending transaction: ${result}`
  }
```
{% endtab %}

{% tab title="Python" %}
```python
        # Send OfferCreate transaction ----------------------------------------------

        # For this tutorial, we already know that TST is pegged to
        # XRP at a rate of approximately 10:1 plus spread, so we use
        # hard-coded TakerGets and TakerPays amounts.

        tx = OfferCreate(
            account=wallet.address,
            taker_gets=we_spend["value"],
            taker_pays=we_want["currency"].to_amount(we_want["value"]),
        )

        # Sign and autofill the transaction (ready to submit)
        signed_tx = await autofill_and_sign(tx, client, wallet)
        print("Transaction:", signed_tx)

        # Submit the transaction and wait for response (validated or rejected)
        print("Sending OfferCreate transaction...")
        result = await submit_and_wait(signed_tx, client)
        if result.is_successful():
            print(f"Transaction succeeded: "
                  f"https://testnet.xrpl.org/transactions/{signed_tx.get_hash()}")
        else:
            raise Exception(f"Error sending transaction: {result}")
```
{% endtab %}
{% endtabs %}

이 인터페이스를 사용하여 이전 단계에서 지정한 금액으로 트랜잭션을 전송할 수 있습니다:

* [Connect](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-connect)
* [Generate](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-generate)
* [Look Up Offers](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-look\_up\_offers)
* [Send OfferCreate](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-send\_offercreate)
* [Wait](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-wait)
* [Check Metadata](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-check\_metadata)
* [Check Balances and Offers](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-check\_balances\_and\_offers)

## 5. 유효성 검사 대기

대부분의 트랜잭션은 제출된 후 다음 Ledger 버전으로 승인되므로, 트랜잭션 결과가 최종적으로 확정되기까지 4\~7초가 소요될 수 있습니다. XRP Ledger가 사용 중이거나 네트워크 연결 상태가 좋지 않아 트랜잭션이 네트워크를 통해 전달되는 것이 지연되는 경우, 트랜잭션이 확인되는 데 더 오랜 시간이 걸릴 수 있습니다. (트랜잭션 만료를 설정하는 방법에 대한 자세한 내용은 안정적인 트랜잭션 제출을 참조하세요.)

* [Connect](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-connect)
* [Generate](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-generate)
* [Look Up Offers](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-look\_up\_offers)
* [Send OfferCreate](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-send\_offercreate)
* [Wait](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-wait)
* [Check Metadata](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-check\_metadata)
* [Check Balances and Offers](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-check\_balances\_and\_offers)

## 6. 메타데이터 확인

검증된 트랜잭션의 메타데이터를 사용해 트랜잭션이 정확히 어떤 일을 했는지 확인할 수 있습니다. (특히 탈중앙화 거래소를 사용할 때는 최종 결과와 다를 수 있으므로 임시 트랜잭션 결과의 메타데이터를 사용하지 마세요). 제안 생성 트랜잭션의 경우 가능한 결과는 다음과 같습니다:

* 제안의 일부 또는 전부가 Ledger에 있는 기존 제안과 일치하여 채워졌을 수 있습니다.
* 매칭되지 않은 나머지 제안이 있다면 Ledger에 배치되어 새로운 매칭 제안을 기다리고 있을 것입니다.
* 매칭될 수 있는 만료되었거나 펀딩되지 않은 제안을 제거하는 등 다른 부기 작업이 발생했을 수도 있습니다.

다음 코드는 트랜잭션의 메타데이터를 확인하는 방법을 보여줍니다:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// Check metadata ------------------------------------------------------------
  // In JavaScript, you can use getBalanceChanges() to help summarize all the
  // balance changes caused by a transaction.
  const balance_changes = xrpl.getBalanceChanges(result.result.meta)
  console.log("Total balance changes:", JSON.stringify(balance_changes, null,2))

  // Helper to convert an XRPL amount to a string for display
  function amt_str(amt) {
    if (typeof amt == "string") {
      return `${xrpl.dropsToXrp(amt)} XRP`
    } else {
      return `${amt.value} ${amt.currency}.${amt.issuer}`
    }
  }

  let offers_affected = 0
  for (const affnode of result.result.meta.AffectedNodes) {
    if (affnode.hasOwnProperty("ModifiedNode")) {
      if (affnode.ModifiedNode.LedgerEntryType == "Offer") {
        // Usually a ModifiedNode of type Offer indicates a previous Offer that
        // was partially consumed by this one.
        offers_affected += 1
      }
    } else if (affnode.hasOwnProperty("DeletedNode")) {
      if (affnode.DeletedNode.LedgerEntryType == "Offer") {
        // The removed Offer may have been fully consumed, or it may have been
        // found to be expired or unfunded.
        offers_affected += 1
      }
    } else if (affnode.hasOwnProperty("CreatedNode")) {
      if (affnode.CreatedNode.LedgerEntryType == "RippleState") {
        console.log("Created a trust line.")
      } else if (affnode.CreatedNode.LedgerEntryType == "Offer") {
        const offer = affnode.CreatedNode.NewFields
        console.log(`Created an Offer owned by ${offer.Account} with
          TakerGets=${amt_str(offer.TakerGets)} and
          TakerPays=${amt_str(offer.TakerPays)}.`)
      }
    }
  }
  console.log(`Modified or removed ${offers_affected} matching Offer(s)`)
```
{% endtab %}

{% tab title="Python" %}
```python
        # Check metadata ------------------------------------------------------------
        balance_changes = get_balance_changes(result.result["meta"])
        print(f"Balance Changes:\n{pprint.pformat(balance_changes)}")

        # For educational purposes the transaction metadata is analyzed manually in the
        # following section. However, there is also a get_order_book_changes(metadata)
        # utility function available in the xrpl library, which is generally the easier
        # and preferred choice for parsing the metadata and computing orderbook changes.

        # Helper to convert an XRPL amount to a string for display
        def amt_str(amt) -> str:
            if isinstance(amt, str):
                return f"{drops_to_xrp(amt)} XRP"
            else:
                return f"{amt['value']} {amt['currency']}.{amt['issuer']}"

        offers_affected = 0
        for affnode in result.result["meta"]["AffectedNodes"]:
            if "ModifiedNode" in affnode:
                if affnode["ModifiedNode"]["LedgerEntryType"] == "Offer":
                    # Usually a ModifiedNode of type Offer indicates a previous Offer that
                    # was partially consumed by this one.
                    offers_affected += 1
            elif "DeletedNode" in affnode:
                if affnode["DeletedNode"]["LedgerEntryType"] == "Offer":
                    # The removed Offer may have been fully consumed, or it may have been
                    # found to be expired or unfunded.
                    offers_affected += 1
            elif "CreatedNode" in affnode:
                if affnode["CreatedNode"]["LedgerEntryType"] == "RippleState":
                    print("Created a trust line.")
                elif affnode["CreatedNode"]["LedgerEntryType"] == "Offer":
                    offer = affnode["CreatedNode"]["NewFields"]
                    print(f"Created an Offer owned by {offer['Account']} with "
                          f"TakerGets={amt_str(offer['TakerGets'])} and "
                          f"TakerPays={amt_str(offer['TakerPays'])}.")

        print(f"Modified or removed {offers_affected} matching Offer(s)")
```
{% endtab %}
{% endtabs %}

&#x20;이 인터페이스를 사용하여 테스트해 볼 수 있습니다:

* [Connect](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-connect)
* [Generate](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-generate)
* [Look Up Offers](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-look\_up\_offers)
* [Send OfferCreate](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-send\_offercreate)
* [Wait](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-wait)
* [Check Metadata](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-check\_metadata)
* [Check Balances and Offers](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-check\_balances\_and\_offers)

## 7. 잔액 및 제안 확인

가장 최근에 확인된 ledger를 기준으로 계정의 잔액과 미발행 제안을 조회할 수 있는 좋은 시기입니다. 여기에는 트랜잭션으로 인한 변경 사항과 동일한 ledger 버전에서 실행된 다른 트랜잭션이 모두 표시됩니다.

다음 코드는 account\_lines 메소드를 사용하여 잔액을 조회하고 account\_offers 메소드를 사용하여 제안 조회하는 방법을 보여줍니다.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// Check balances ------------------------------------------------------------
  console.log("Getting address balances as of validated ledger...")
  const balances = await client.request({
    command: "account_lines",
    account: wallet.address,
    ledger_index: "validated"
    // You could also use ledger_index: "current" to get pending data
  })
  console.log(JSON.stringify(balances.result, null, 2))

  // Check Offers --------------------------------------------------------------
  console.log(`Getting outstanding Offers from ${wallet.address} as of validated ledger...`)
  const acct_offers = await client.request({
    command: "account_offers",
    account: wallet.address,
    ledger_index: "validated"
  })
  console.log(JSON.stringify(acct_offers.result, null, 2))
```
{% endtab %}

{% tab title="Python" %}
```python
        # Check balances ------------------------------------------------------------
        print("Getting address balances as of validated ledger...")
        balances = await client.request(
            AccountLines(
                account=wallet.address,
                ledger_index="validated",
            )
        )
        pprint.pp(balances.result)

        # Check Offers --------------------------------------------------------------
        print(f"Getting outstanding Offers from {wallet.address} "
              f"as of validated ledger...")
        acct_offers = await client.request(
            AccountOffers(
                account=wallet.address,
                ledger_index="validated",
            )
        )
        pprint.pp(acct_offers.result)
```
{% endtab %}
{% endtabs %}

&#x20;이 인터페이스를 사용하여 테스트해 볼 수 있습니다:

* [Connect](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-connect)
* [Generate](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-generate)
* [Look Up Offers](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-look\_up\_offers)
* [Send OfferCreate](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-send\_offercreate)
* [Wait](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-wait)
* [Check Metadata](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-check\_metadata)
* [Check Balances and Offers](https://xrpl.org/trade-in-the-decentralized-exchange.html#interactive-check\_balances\_and\_offers)
