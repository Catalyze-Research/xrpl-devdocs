# 보안 서명(Secure Signing)

XRP Ledger에 트랜잭션을 제출하기 위해서는 당신의 비밀 키의 보안을 저해하지 않으면서 그것들을 디지털로 서명할 수 있는 방법이 필요합니다. (타인이 당신의 비밀 키에 접근하면, 당신만큼 당신의 계정을 제어할 수 있으며, 당신의 모든 돈을 도난당하거나 파괴할 수 있습니다.) 이 페이지는 이러한 환경을 어떻게 설정하는지에 대해 요약하고 있습니다.

{% hint style="success" %}
Tip:

당신이 네트워크에 거래를 제출하지 않는다면, Ripple이 운영하는 것과 같은 신뢰할 수 있는 공개 서버를 사용하여 들어오는 거래나 다른 네트워크 활동을 모니터링할 수 있습니다. XRP Ledger의 모든 거래, 잔액, 그리고 데이터는 공개적입니다.
{% endhint %}

당신의 상황에 적합할 수 있는 다양한 보안 수준의 구성이 여러 가지 있습니다. 당신의 필요에 가장 잘 맞는 것을 선택하십시오:

* 로컬 또는 같은 LAN에서 rippled를 실행하십시오.&#x20;
* 로컬 서명을 할 수 있는 클라이언트 라이브러리를 사용하십시오.
* XRP Ledger 서명을 지원하는 전용 서명 장치를 사용하십시오.&#x20;
* 당신이 신뢰하는 원격 rippled 기계에 안전한 VPN을 사용하여 연결하십시오.

## 안전하지 않은 구성&#x20;

<figure><img src="https://xrpl.org/img/insecure-signing-options.svg" alt="" width="563"><figcaption></figcaption></figure>

외부 소스가 당신의 비밀 키에 접근할 수 있는 구성은 위험하며, 악의적인 사용자가 당신의 모든 XRP(그리고 당신의 XRP Ledger 주소가 가진 모든 것)를 도난당하는 결과를 초래할 가능성이 있습니다. 인터넷을 통해 다른 사람의 rippled 서버의 sign 메소드를 사용하거나, 당신의 비밀 키를 평문으로 인터넷을 통해 당신의 서버로 보내는 것과 같은 구성이 이러한 구성의 예입니다.

당신은 당신의 비밀 키의 보안을 항상 유지해야 하며, 이것은 당신이 그것들을 자신 이메일로 보내지 않는 것, 공개적으로 그것들을 타이핑하지 않는 것, 그리고 그것들을 사용하지 않을 때 암호화하여 저장하는 것을 포함합니다 — 절대 평문으로 저장하지 마십시오. 보안과 편의성 사이의 균형은 부분적으로 당신의 주소의 보유액의 가치에 따라 달라지므로 당신은 다양한 보안 구성을 가진 여러 주소를 사용하는 것이 좋습니다.

## 로컬에서 rippled를 실행&#x20;

<figure><img src="https://xrpl.org/img/secure-signing-local-rippled.svg" alt="" width="563"><figcaption></figcaption></figure>

이 구성에서, 당신은 거래를 생성하는 기계에서 rippled를 실행합니다. 비밀 키가 당신의 기계를 떠나지 않기 때문에, 당신의 기계에 접근할 수 없는 사람은 비밀 키에 접근할 수 없습니다. 당연히, 당신은 당신의 기계를 보호하기 위한 업계 표준 사례를 따르는 것이 좋습니다. 이 구성을 사용하려면:

1. rippled를 설치하십시오. \
   당신의 로컬 기계가 rippled의 최소 시스템 요구 사항을 충족하는지 확인하십시오.&#x20;
2. 트랜잭션을 서명해야 할 때, localhost 또는 127.0.0.1에 있는 서버에 연결하십시오. sign 메소드(단일 서명용) 또는 sign\_for 메소드(다중 서명용)를 사용하십시오. 예제 설정 파일은 로컬 루프백 네트워크(127.0.0.1)에서 연결을 수신하며, 포트 5005에서 JSON-RPC (HTTP)를, 포트 6006에서 WebSocket (WS)를 사용하고, 모든 연결된 클라이언트를 관리자로 취급합니다.&#x20;

{% hint style="warning" %}
Caution:

커맨드라인 API를 서명용으로 사용하는 것은 웹소켓이나 JSON-RPC API를 비커맨드라인 클라이언트를 통해 사용하는 것보다 안전하지 않습니다. 커맨드라인 구문을 사용하면, 당신의 비밀 키는 시스템의 프로세스 목록에서 다른 사용자에게 보일 수 있으며, 당신의 셸 이력이 평문으로 키를 저장할 수 있습니다.
{% endhint %}

3. 서버를 유지하여 작동, 업데이트, 그리고 당신이 사용하는 동안 네트워크와 동기화 상태를 유지하십시오.&#x20;

{% hint style="info" %}
Note:

트랜잭션을 보내지 않을 때는 rippled 서버를 끌 수 있지만, 다시 시작할 때 네트워크와 동기화하는 데 최대 15분이 걸릴 수 있습니다.
{% endhint %}

## 같은 LAN에서 rippled를 실행 (Run rippled on the same LAN)

<figure><img src="https://xrpl.org/img/secure-signing-lan-rippled.svg" alt="" width="563"><figcaption></figcaption></figure>

이 구성에서, 당신은 서명할 거래를 생성하는 기계와 같은 사설 로컬 영역 네트워크(LAN)의 전용 기계에서 rippled 서버를 실행합니다. 이 구성을 사용하면 매우 저렴한 시스템 사양의 하나 이상의 기계에서 거래 지시문을 조립하면서, 단일 전용 기계에서 rippled를 실행하는 것이 가능합니다. 이는 당신이 자체 데이터 센터나 서버 방을 운영하는 경우에 매력적일 수 있습니다.

이 구성을 사용하려면, 당신의 LAN 내에서 rippled 서버가 wss와 https 연결을 수락하도록 설정하십시오. 인증서 고정을 사용하는 경우 자체 서명 인증서를 사용하거나, 내부 또는 잘 알려진 인증서 권한에 의해 서명된 인증서를 사용할 수 있습니다. Let's Encrypt와 같은 일부 인증서 권한은 무료로 인증서를 자동으로 발행합니다.

항상, 방화벽, 바이러스 백신, 적절한 사용자 권한 등과 같은 기계를 보호하기 위한 업계 표준 사례를 따르십시오.

## 로컬 서명을 지원하는 클라이언트 라이브러리 사용(Use a Client Library with Local Signing)&#x20;

이 구성은 당신이 사용하는 프로그래밍 언어에서 내장 서명 기능을 가진 클라이언트 라이브러리를 사용합니다. 로컬 서명을 수행할 수 있는 라이브러리 목록은 클라이언트 라이브러리를 참조하십시오.

## 서명 라이브러리를 위한 보안 모범 사례(Security Best Practices for Signing Libraries)

당신의 서명 라이브러리의 보안성을 최적화하기 위해:

* 당신이 사용하는 서명 라이브러리가 그 서명 알고리즘을 적절하고 안전하게 구현하였는지 확인하십시오. 예를 들어, 라이브러리가 기본 ECDSA 알고리즘을 사용하는 경우, RFC-6979에 설명된 대로 결정적인 논스를 사용해야 합니다. 위에 나열된 모든 출판 라이브러리는 업계 모범 사례를 따릅니다.&#x20;
* 클라이언트 라이브러리를 최신 안정 버전으로 업데이트하십시오.&#x20;
* 향상된 보안을 위해, 당신은 Vault와 같은 관리 도구에서 비밀 키를 로드할 수 있습니다.

## 로컬 서명 예제(Local Signing Example)

다음은 다음 언어와 라이브러리를 사용하여 로컬에서 거래 지시문을 서명하는 방법에 대한 예입니다:

* JavaScript / TypeScript - xrpl.js&#x20;
* Python - xrpl-py&#x20;
* Java - xrpl4j

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// Sample code demonstrating secure offline signing using xrpl.js library.
const xrpl = require('xrpl')

// Load seed value from an environment variable:
const my_wallet = xrpl.Wallet.fromSeed(process.env['MY_SEED'])

// For offline signing, you need to know your address's next Sequence number.
// Alternatively, you could use a Ticket in place of the Sequence number.
// This is useful when you need multiple signatures and may want to process transactions out-of-order.
// For details, see: https://xrpl.org/tickets.html
let my_seq = 21404872

// Provide *all* required fields before signing a transaction
const txJSON = {
  "Account": my_wallet.address,
  "TransactionType":"Payment",
  "Destination":"rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
  "Amount":"13000000",
  "Flags":2147483648,
  "LastLedgerSequence":7835923, // Optional, but recommended.
  "Fee":"13",
  "Sequence": my_seq
}

const signed = my_wallet.sign(txJSON)

console.log("tx_blob is:", signed.tx_blob)
console.log("tx hash is:", signed.hash)
```
{% endtab %}

{% tab title="Python" %}
```python
# Define signer address
import os  
my_secret = os.getenv("MYSECRET")
from xrpl.wallet import Wallet
wallet = Wallet.from_seed(seed=my_secret)
print(wallet.address)  # "raaFKKmgf6CRZttTVABeTcsqzRQ51bNR6Q"

# For offline signing, you need to know your address's next Sequence number.
# Alternatively, you could use a Ticket in place of the Sequence number.
# This is useful when you need multiple signatures and may want to process transactions out-of-order.
# For details, see: https://xrpl.org/tickets.html
sequence = 0

from xrpl.models.transactions import Payment
from xrpl.utils import xrp_to_drops
my_payment = Payment(
    account=wallet.address,
    amount=xrp_to_drops(22),
    fee="10",
    destination="rPT1Sjq2YGrBMTttX4GZHjKu9dyfzbpAYe",
    sequence=sequence,
)
print("Payment object:", my_payment)

# Sign transaction -------------------------------------------------------------
import xrpl.transaction 
signed = xrpl.transaction.sign(my_payment, wallet)
print("Signed transaction blob:", signed)
```
{% endtab %}

{% tab title="Java" %}
```java
////////////////////////////////////////////////////////////////////////////
// Sign using a SingleKeySignatureService:
// This implementation of SignatureService signs Transactions using a
// supplied PrivateKey. This may be suitable for some applications, but is
// likely not secure enough for server side applications, as keys should not
// be stored in memory whenever possible.
////////////////////////////////////////////////////////////////////////////

// Create a random wallet
KeyPair randomKeyPair = Seed.ed25519Seed().deriveKeyPair();
PublicKey publicKey = randomKeyPair.publicKey();
PrivateKey privateKey = randomKeyPair.privateKey()

// Construct a SignatureService
SignatureService<PrivateKey> signatureService = new BcSignatureService();

// Construct and sign the Payment
Payment payment = Payment.builder()
  .account(publicKey.deriveAddress())
  .destination(Address.of("rPT1Sjq2YGrBMTttX4GZHjKu9dyfzbpAYe"))
  .amount(XrpCurrencyAmount.ofDrops(1000))
  .fee(XrpCurrencyAmount.ofDrops(10))
  .sequence(UnsignedInteger.valueOf(16126889))
  .signingPublicKey(publicKey)
  .build();
SingleSignedTransaction<Payment> signedPayment = signatureService.sign(privateKey, payment);
System.out.println("Signed Payment: " + signedPayment.signedTransaction());


////////////////////////////////////////////////////////////////////////////
// Sign using a DerivedKeysSignatureService:
// This implementation of SignatureService deterministically derives
// Private Keys from a secret value (likely a server secret) and a wallet
// identifier. That PrivateKey can then be used to sign transactions.
// The wallet identifier can be anything, but would likely be an existing ID
// tracked by a server side system.
//
// Though this implementation is more secure than SingleKeySignatureService
// and better suited for server-side applications, keys are still held
// in memory. For the best security, we suggest using a HSM-based
// implementation.
////////////////////////////////////////////////////////////////////////////

// Construct a DerivedKeysSignatureService with a server secret (in this case "shh")
SignatureService<PrivateKeyReference> derivedKeySignatureService = new BcDerivedKeySignatureService(
  () -> ServerSecret.of("shh".getBytes())
);

PrivateKeyReference privateKeyReference = new PrivateKeyReference() {
  @Override
  public KeyType keyType() {
    return KeyType.ED25519;
  }

  @Override
  public String keyIdentifier() {
    return "sample-keypair";
  }
};

// Get the public key and classic address for the given walletId
PublicKey publicKey = derivedKeySignatureService.derivePublicKey(privateKeyReference);
Address classicAddress = publicKey.deriveAddress();

// Construct and sign the Payment
Payment payment = Payment.builder()
  .account(classicAddress)
  .destination(Address.of("rPT1Sjq2YGrBMTttX4GZHjKu9dyfzbpAYe"))
  .amount(XrpCurrencyAmount.ofDrops(1000))
  .fee(XrpCurrencyAmount.ofDrops(10))
  .sequence(UnsignedInteger.valueOf(16126889))
  .signingPublicKey(publicKey)
  .build();

SingleSignedTransaction<Payment> signedPayment = derivedKeySignatureService.sign(privateKeyReference, payment);
System.out.println("Signed Payment: " + signedPayment.signedTransaction());
```
{% endtab %}
{% endtabs %}

## 전용 서명 장치 사용(Use a Dedicated Signing Device)

<figure><img src="https://xrpl.org/img/secure-signing-dedicated-hardware.svg" alt="" width="563"><figcaption></figcaption></figure>

일부 회사들은 Ledger Nano S와 같은 전용 서명 장치를 판매하며, 이 장치는 장치를 벗어나지 않는 비밀 키를 사용하여 XRP Ledger 거래를 서명할 수 있습니다. 일부 장치는 모든 유형의 거래를 지원하지 않을 수 있습니다.

이 구성을 설정하는 방법은 특정 장치에 따라 다릅니다. 서명 장치와 상호작용하기 위해 기계에서 "관리자" 애플리케이션을 실행해야 할 수 있습니다. 이런 장치를 설정하고 사용하는 방법에 대해서는 제조사의 지침을 참조하십시오.

## 원격 rippled 서버와 안전한 VPN 사용(Use a Secure VPN with a Remote rippled Server)

이 구성은 원격 호스팅된 rippled 서버를 사용하며, colocation 시설이나 먼 데이터센터 등에 위치해 있는 곳들도 암호화된 VPN을 사용하여 안전하게 연결합니다.

이 구성을 사용하려면, 사설 LAN에서 rippled를 실행하는 단계를 따르되, VPN을 사용하여 원격 rippled 서버의 LAN에 연결하십시오. VPN을 설정하는 지침은 환경에 따라 다르며, 이 가이드에서는 설명하지 않습니다.



## 참고 <a href="#see-also" id="see-also"></a>

* **Concepts:**
  * [Cryptographic Keys](https://xrpl.org/cryptographic-keys.html)
  * [Multi-Signing](https://xrpl.org/multi-signing.html)
* **Tutorials:**
  * [Install rippled](https://xrpl.org/install-rippled.html)
  * [Assign a Regular Key Pair](https://xrpl.org/assign-a-regular-key-pair.html)
  * [Reliable Transaction Submission](https://xrpl.org/reliable-transaction-submission.html)
  * [Enable Public Signing](https://xrpl.org/enable-public-signing.html)
* **References:**
  * [sign method](https://xrpl.org/sign.html)
  * [submit method](https://xrpl.org/submit.html)
  * [xrpl.js Reference ](https://js.xrpl.org/)
  * [`xrpl-py` Reference ](https://xrpl-py.readthedocs.io/en/latest/index.html)
  * [`xrpl4j` Reference ](https://javadoc.io/doc/org.xrpl/)
