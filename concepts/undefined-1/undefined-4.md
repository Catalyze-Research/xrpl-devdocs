# 결제 채널

결제 채널은 매우 작은 단위로 분할되고 나중에 정산할 수 있는 "비동기적" XRP 결제를 보낼 수 있는 고급 기능입니다.

결제 채널의 XRP는 일시적으로 따로 보관됩니다. 송신자는 채널에 대한 청구를 생성하며, 수신자는 XRP Ledger 트랜잭션을 보내거나 새로운 Ledger 버전이 [컨센서스](../consensus-protocol/consensus-structure.md)에 의해 승인될 때까지 기다릴 필요 없이 해당 청구를 확인합니다. (이것은 일반적으로 합의에 의해 트랜잭션이 승인되는 평소 패턴과는 별도로 발생하기 때문에 비동기 프로세스입니다.) 수신자는 언제든지 청구를 교환하여 해당 청구에 의해 승인된 양의 XRP를 받을 수 있습니다. 이러한 방식으로 청구를 정산하는 것은 일반적인 컨센서스 프로세스의 일부로 표준 XRP Ledger 트랜잭션을 사용합니다. 이 단일 트랜잭션은 더 작은 클레임으로 보장된 여러 트랜잭션을 포함할 수 있습니다.

청구는 개별적으로 확인될 수 있지만 나중에 일괄적으로 정산되므로, 결제 채널을 사용하면 참여자가 해당 청구의 디지털 서명을 생성하고 검증하는 능력에만 제한되어 트랜잭션을 처리할 수 있습니다. 이 제한은 주로 참여자의 하드웨어 속도와 서명 알고리즘의 복잡성에 기반합니다. 최대 속도를 위해서는 XRP Ledger의 기본 secp256k1 ECDSA 서명보다 더 빠른 Ed25519 서명을 사용하세요. 연구에서는 [2011년 기준으로 일반 하드웨어에서 초당 100,000개 이상의 Ed25519 서명 생성 및 초당 70,000개 이상의 서명 확인이 가능함](https://ed25519.cr.yp.to/ed25519-20110926.pdf)을 입증했습니다.

## 결제 채널 사용 이유&#x20;

결제 채널 사용 프로세스는 항상 지불자(payer)와 수취인(payee) 두 가지 당사자를 포함합니다. 지불자는 XRP Ledger를 사용하는 개인 또는 기관으로서 수취인의 고객입니다. 수취인은 상품이나 서비스의 대가로서 XRP를 수령하는 사람이나 비즈니스입니다.

결제 채널은 그 자체로 어떤 상품과 서비스를 구매하고 판매하는지에 대해 명시적으로 규정하지 않습니다. 그러나 결제 채널에 적합한 상품과 서비스의 유형은 다음과 같습니다:

* 디지털 항목과 같이 거의 즉시 전송할 수 있는 것들.
* 가격의 일부로 트랜잭션 처리 비용이 비교적 중요한 저렴한 것들.
* 일반적으로 대량으로 구입한 물건으로, 원하는 정확한 수량을 미리 알 수 없는 경우.

## 결제 채널의 수명주기&#x20;

아래의 다이어그램은 결제 채널의 수명주기를 요약합니다:

<figure><img src="https://xrpl.org/img/paychan-flow.svg" alt=""><figcaption></figcaption></figure>



## 참고자료

* **Related Concepts:**
  * [Escrow](https://xrpl.org/escrow.html), a similar feature for higher-value, lower-speed conditional XRP payments.
* **Tutorials and Use Cases:**
  * [Use Payment Channels](https://xrpl.org/use-payment-channels.html), a tutorial stepping through the process of using a payment channel.
  * [Open a Payment Channel to Enable an Inter-Exchange Network](https://xrpl.org/open-a-payment-channel-to-enable-an-inter-exchange-network.html)
* **References:**
  * [channel\_authorize method](https://xrpl.org/channel\_authorize.html)
  * [channel\_verify method](https://xrpl.org/channel\_verify.html)
  * [PayChannel object](https://xrpl.org/paychannel.html)
  * [PaymentChannelClaim transaction](https://xrpl.org/paymentchannelclaim.html)
  * [PaymentChannelCreate transaction](https://xrpl.org/paymentchannelcreate.html)
  * [PaymentChannelFund transaction](https://xrpl.org/paymentchannelfund.html)



