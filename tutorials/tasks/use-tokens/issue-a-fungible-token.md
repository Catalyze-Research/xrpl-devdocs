# 대체가능한 토큰 발행(Issue a Fungible Token)

누구나 비공식적인 "IOUs"부터 법정화폐 기반 스테이블코인, 순수 디지털 대체 가능 토큰 및 반대체 가능 토큰 등 다양한 유형의 토큰을 XRP Ledger에서 발행할 수 있습니다. 이 튜토리얼에서는 원장에 토큰을 생성하는 기술적 단계를 보여드립니다. XRP Ledger 토큰의 작동 방식에 대한 자세한 내용은 [**Issued Currencies**](https://xrpl.org/issued-currencies.html)를, 스테이블코인 발행과 관련된 비즈니스 의사 결정에 대한 자세한 내용은 [**Stablecoin Issuer**](https://xrpl.org/stablecoin-issuer.html)를 참조하세요.

## 요구 조건(Prerequisites)

* 각각 주소, 비밀 키, 약간의 XRP가 예치 되어 있는 2개의 XRP Ledger 계정이 필요합니다. 이 튜토리얼에서는 필요에 따라 새 테스트 자격 증명을 생성할 수 있습니다.&#x20;
  * 각 주소에는 신뢰선에 대한 추가 준비금을 포함해 준비금 요건을 충족할 수 있는 충분한 XRP가 필요합니다.&#x20;
* XRP 원장 네트워크에 연결해야 합니다. 이 튜토리얼에 나와 있는 것처럼 공용 서버를 사용해 테스트할 수 있습니다.&#x20;
* 선호하는 클라이언트 라이브러리에 대한 시작하기 지침을 숙지하고 있어야 합니다. 이 페이지에서는 다음에 대한 예제를 제공합니다:&#x20;
  * [xrpl.js library](https://github.com/XRPLF/xrpl.js/)를 사용한 **JavaScript** . \
    설정 단계는 JavaScript를 사용하여 시작하기를 참조하세요.&#x20;
  * [`xrpl-py` library](https://xrpl-py.readthedocs.io/)를 사용하는 **Python** . \
    설정 단계는 Python을 사용하여 시작하기를 참조하세요.&#x20;
  * [xrpl4j library](https://github.com/XRPLF/xrpl4j)가 포함된 **Java** . \
    설정 단계는 Java를 사용하여 시작하기를 참조하세요.&#x20;
  * 별도의 설정 없이 브라우저에서 대화형 단계를 따라 읽고 사용할 수도 있습니다.

## 예제 코드(Example Code)

이 튜토리얼의 모든 단계에 대한 전체 샘플 코드는 [MIT license](https://github.com/XRPLF/xrpl-dev-portal/blob/master/LICENSE)에 따라 사용할 수 있습니다.

* [Code Samples: Issue a Fungible Token ](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/issue-a-token/)을 참고하세요:&#x20;

## 단계(Steps)

## 1. 자격 증명 받기(Get Credentials)

XRP Ledger에서 거래하려면 주소와 비밀 키, 그리고 약간의 XRP가 필요합니다. 또한 발행한 토큰을 기꺼이 보유할 한 명 이상의 수신자가 필요합니다. 다른 블록체인과 달리 XRP Ledger에서는 원하지 않는 토큰을 강제로 보유하도록 강요할 수 없습니다.

가장 좋은 방법은 "콜드" 주소와 "핫" 주소를 사용하는 것입니다. 콜드 주소는 토큰의 발행자입니다. 핫 주소는 사용자가 관리하는 일반 사용자의 주소와 같습니다. 콜드 주소에서 토큰을 받은 다음 다른 사용자에게 전송할 수 있습니다. 콜드 주소에서 사용자에게 직접 토큰을 보낼 수 있으므로 핫 주소가 반드시 필요한 것은 아니지만, 보안상 좋은 습관입니다. 프로덕션 환경에서는 콜드 주소를 핫 주소보다 교체하기가 훨씬 어렵기 때문에 콜드 주소의 암호화 키를 오프라인 상태로 유지하는 등 각별히 관리해야 합니다.

이 튜토리얼에서는 핫 주소가 콜드 주소에서 발급한 토큰을 받습니다. 다음 인터페이스를 사용하여 두 주소의 키를 얻을 수 있습니다.

* [Generate](https://xrpl.org/issue-a-fungible-token.html#interactive-generate)
* [Connect](https://xrpl.org/issue-a-fungible-token.html#interactive-connect)
* [Configure Issuer](https://xrpl.org/issue-a-fungible-token.html#interactive-configure\_issuer)
* [Wait (Issuer Setup)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_issuer\_setup)
* [Configure Hot Address](https://xrpl.org/issue-a-fungible-token.html#interactive-configure\_hot\_address)
* [Wait (Hot Address Setup)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_hot\_address\_setup)
* [Make Trust Line](https://xrpl.org/issue-a-fungible-token.html#interactive-make\_trust\_line)
* [Wait (TrustSet)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_trustset)
* [Send Token](https://xrpl.org/issue-a-fungible-token.html#interactive-send\_token)
* [Wait (Payment)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_payment)
* [Confirm Balances](https://xrpl.org/issue-a-fungible-token.html#interactive-confirm\_balances)

{% hint style="danger" %}
**Caution**:

리플은 테스트 목적으로만 [Testnet and Devnet](https://xrpl.org/parallel-networks.html)을 제공하며, 때때로 이러한 테스트 네트워크의 상태를 모든 잔액과 함께 초기화하기도 합니다. 예방책으로 테스트넷/데브넷과 메인넷에서 동일한 주소를 사용하지 마시기 바랍니다.
{% endhint %}

프로덕션용 소프트웨어를 구축하는 경우([building production-ready software](https://xrpl.org/production-readiness.html))에는 기존 계정을 사용하고 보안 서명 구성([secure signing configuration](https://xrpl.org/secure-signing.html))을 사용하여 키를 관리해야 합니다.

## 2. 네트워크에 연결(Connect to the Network)

네트워크에 트랜잭션을 제출하려면 네트워크에 연결되어 있어야 합니다. 다음 코드는 지원되는 클라이언트 라이브러리가 있는 public XRP Ledger 테스트넷 서버에 연결하는 방법을 보여줍니다:

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
# Connect ----------------------------------------------------------------------
import xrpl
testnet_url = "https://s.altnet.rippletest.net:51234"
client = xrpl.clients.JsonRpcClient(testnet_url)
```
{% endtab %}

{% tab title="Java" %}
```java
// Construct a network client ----------------------------------------------
    HttpUrl rippledUrl = HttpUrl
      .get("https://s.altnet.rippletest.net:51234/");
    XrplClient xrplClient = new XrplClient(rippledUrl);
    // Get the current network fee
    FeeResult feeResult = xrplClient.fee();Field	Value
TransactionType	"TrustSet"
Account	The hot address. (More generally, this is the account that wants to receive the token.)
LimitAmount	An object specifying how much, of which token, from which issuer, you are willing to hold.
LimitAmount.currency	The currency code of the token.
LimitAmount.issuer	The cold address.
LimitAmount.value	The maximum amount of the token you are willing to hold.
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Note**:

이 튜토리얼의 JavaScript 코드 샘플은 async/await 패턴을 사용합니다. await은 비동기 함수 내에서 사용해야 하므로, 나머지 코드 샘플은 여기서 시작한 main() 함수 내에서 계속 진행되도록 작성되었습니다. 원하는 경우 async/await 대신 Promise 메소드 .then() 및 .catch()를 사용할 수도 있습니다.
{% endhint %}

이 튜토리얼에서는 다음 버튼을 클릭하여 연결합니다:

* [Generate](https://xrpl.org/issue-a-fungible-token.html#interactive-generate)
* [Connect](https://xrpl.org/issue-a-fungible-token.html#interactive-connect)
* [Configure Issuer](https://xrpl.org/issue-a-fungible-token.html#interactive-configure\_issuer)
* [Wait (Issuer Setup)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_issuer\_setup)
* [Configure Hot Address](https://xrpl.org/issue-a-fungible-token.html#interactive-configure\_hot\_address)
* [Wait (Hot Address Setup)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_hot\_address\_setup)
* [Make Trust Line](https://xrpl.org/issue-a-fungible-token.html#interactive-make\_trust\_line)
* [Wait (TrustSet)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_trustset)
* [Send Token](https://xrpl.org/issue-a-fungible-token.html#interactive-send\_token)
* [Wait (Payment)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_payment)
* [Confirm Balances](https://xrpl.org/issue-a-fungible-token.html#interactive-confirm\_balances)

## 3. 발급자 설정 구성하기(Configure Issuer Settings)

먼저, 콜드 주소(토큰 발급자가 될)에 대한 설정을 구성합니다. 다음과 같은 예외를 제외하고 대부분의 설정은 나중에 재구성할 수 있습니다:

* 기본 Ripple([Default Ripple](https://xrpl.org/rippling.html)): \
  이 설정은 사용자가 서로 토큰을 보낼 수 있도록 하기 위해 필요합니다. 신뢰선을 설정하거나 토큰을 발급하기 전에 이 설정을 활성화하는 것이 가장 좋습니다.
* 승인된 신뢰선([Authorized Trust Lines](https://xrpl.org/authorized-trust-lines.html)): \
  (선택 사항) 이 설정('인증 필요'라고도 함)은 명시적으로 승인한 계정만 토큰을 보유하도록 제한합니다. 이미 토큰에 대한 인증번호나 오퍼가 있는 경우에는 이 설정을 사용할 수 없습니다. \
  \
  **참고**: 승인된 신뢰선을을 사용하려면 이 튜토리얼에 표시되지 않은 추가 단계를 수행해야 합니다.

콜드 주소(발급자)에 대해 선택적으로 구성할 수 있는 기타 설정:\


| 설정                                                                             | 권장값          | 요약약                                                                                                |
| ------------------------------------------------------------------------------ | ------------ | -------------------------------------------------------------------------------------------------- |
| [**Require Destination Tags**](https://xrpl.org/require-destination-tags.html) | 활성화 또는 비활성화  | 외부 시스템으로 토큰 출금을 처리하는 경우 활성화합니다. (예: 토큰이 스테이블코인인 경우).                                               |
| **Disallow XRP**                                                               | 활성화 또는 비활성화  | 이 주소가 XRP 결제를 처리하지 않는 경우 활성화합니다.                                                                   |
| [**Transfer Fee**](https://xrpl.org/transfer-fees.html)                        | 0–1%         | 사용자가 서로 토큰을 보낼 때 일정 비율의 수수료를 부과합니다.                                                                |
| [**Tick Size**](https://xrpl.org/ticksize.html)                                | 5            | 탈중앙화 거래소에서 토큰의 환율 소수점 이하 자릿수를 제한합니다. 틱 크기가 5\~6이면 기본값인 15에 비해 거의 동일한 오퍼의 이탈이 줄어들고 가격 검색 속도가 빨라집니다. |
| [**Domain**](https://xrpl.org/accountset.html#domain)                          | (자신의 도메인 이름) | 계정 소유권을 확인할 수 있도록 소유한 도메인으로 설정합니다. 이렇게 하면 혼동이나 사칭 시도를 줄일 수 있습니다.                                   |

이러한 설정은 나중에 변경할 수도 있습니다.

{% hint style="info" %}
Note:

많은 발행 설정은 통화 코드에 관계없이 주소에서 발행한 모든 토큰에 동일하게 적용됩니다. XRP Ledger에서 서로 다른 설정으로 여러 유형의 토큰을 발행하려면 각 토큰을 발행할 때마다 다른 주소를 사용해야 합니다.
{% endhint %}

다음 코드 샘플은 권장 콜드 주소 설정을 활성화하기 위해 계정 세트 트랜잭션([AccountSet transaction](https://xrpl.org/accountset.html))을 전송하는 방법을 보여줍니다:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// Configure issuer (cold address) settings ----------------------------------
  const cold_settings_tx = {
    "TransactionType": "AccountSet",
    "Account": cold_wallet.address,
    "TransferRate": 0,
    "TickSize": 5,
    "Domain": "6578616D706C652E636F6D", // "example.com"
    "SetFlag": xrpl.AccountSetAsfFlags.asfDefaultRipple,
    // Using tf flags, we can enable more flags in one transaction
    "Flags": (xrpl.AccountSetTfFlags.tfDisallowXRP |
             xrpl.AccountSetTfFlags.tfRequireDestTag)
  }

  const cst_prepared = await client.autofill(cold_settings_tx)
  const cst_signed = cold_wallet.sign(cst_prepared)
  console.log("Sending cold address AccountSet transaction...")
  const cst_result = await client.submitAndWait(cst_signed.tx_blob)
  if (cst_result.result.meta.TransactionResult == "tesSUCCESS") {
    console.log(`Transaction succeeded: https://testnet.xrpl.org/transactions/${cst_signed.hash}`)
  } else {
    throw `Error sending transaction: ${cst_result}`
  }
```
{% endtab %}

{% tab title="Python" %}
```python
# Configure issuer (cold address) settings -------------------------------------
cold_settings_tx = xrpl.models.transactions.AccountSet(
    account=cold_wallet.address,
    transfer_rate=0,
    tick_size=5,
    domain=bytes.hex("example.com".encode("ASCII")),
    set_flag=xrpl.models.transactions.AccountSetAsfFlag.ASF_DEFAULT_RIPPLE,
)

print("Sending cold address AccountSet transaction...")
response = xrpl.transaction.submit_and_wait(cold_settings_tx, client, cold_wallet)
print(response)
```
{% endtab %}

{% tab title="Java" %}
```java
// Configure issuer settings -----------------------------------------------
    UnsignedInteger coldWalletSequence = xrplClient.accountInfo(
      AccountInfoRequestParams.builder()
        .ledgerSpecifier(LedgerSpecifier.CURRENT)
        .account(coldWalletKeyPair.publicKey().deriveAddress())
        .build()
    ).accountData().sequence();

    AccountSet setDefaultRipple = AccountSet.builder()
      .account(coldWalletKeyPair.publicKey().deriveAddress())
      .fee(feeResult.drops().minimumFee())
      .sequence(coldWalletSequence)
      .signingPublicKey(coldWalletKeyPair.publicKey())
      .setFlag(AccountSet.AccountSetFlag.DEFAULT_RIPPLE)
      .lastLedgerSequence(computeLastLedgerSequence(xrplClient))
      .build();

    SignatureService<PrivateKey> signatureService = new BcSignatureService();
    SingleSignedTransaction<AccountSet> signedAccountSet = signatureService.sign(
      coldWalletKeyPair.privateKey(), setDefaultRipple
    );

    submitAndWaitForValidation(signedAccountSet, xrplClient);
```
{% endtab %}
{% endtabs %}

## 4. 유효성 검사 대기(Wait for Validation)

대부분의 트랜잭션은 제출된 후 다음 ledger 버전으로 승인되므로, 트랜잭션 결과가 최종 확정되기까지 4\~7초가 소요될 수 있습니다. 이전 트랜잭션이 완전히 검증될 때까지 기다렸다가 이후 단계로 진행해야 순서에 맞지 않게 실행되어 예기치 않은 실패를 방지할 수 있습니다. 자세한 내용은 안정적인 트랜잭션 제출을 참조하세요.

이 튜토리얼의 코드 샘플은 트랜잭션을 제출할 때 유효성 검사를 기다리는 도우미 함수를 사용합니다:

* **JavaScript**: 제출 및 확인 코드 샘플에 정의된 submit\_and\_verify() 함수.
* **Python**: xrpl-py 라이브러리의 submit\_and\_wait() 메소드.
* **Java**: Java: 샘플 Java 클래스의 submitAndWaitForValidation() 메소드.

{% hint style="info" %}
Tip:

기술적으로 핫 주소는 발급자 주소 구성과 병행하여 구성할 수 있습니다. 간단하게 하기 위해 이 튜토리얼에서는 각 트랜잭션을 한 번에 하나씩 대기합니다.&#x20;
{% endhint %}

* [Generate](https://xrpl.org/issue-a-fungible-token.html#interactive-generate)
* [Connect](https://xrpl.org/issue-a-fungible-token.html#interactive-connect)
* [Configure Issuer](https://xrpl.org/issue-a-fungible-token.html#interactive-configure\_issuer)
* [Wait (Issuer Setup)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_issuer\_setup)
* [Configure Hot Address](https://xrpl.org/issue-a-fungible-token.html#interactive-configure\_hot\_address)
* [Wait (Hot Address Setup)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_hot\_address\_setup)
* [Make Trust Line](https://xrpl.org/issue-a-fungible-token.html#interactive-make\_trust\_line)
* [Wait (TrustSet)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_trustset)
* [Send Token](https://xrpl.org/issue-a-fungible-token.html#interactive-send\_token)
* [Wait (Payment)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_payment)
* [Confirm Balances](https://xrpl.org/issue-a-fungible-token.html#interactive-confirm\_balances)

## 5. 핫 주소 설정 구성하기

핫 주소는 기본값에서 반드시 설정을 변경해야 하는 것은 아니지만, 다음 사항을 모범 사례로 권장합니다:

| 설설정                                                                        | 권장값         | 요약                                                                                        |
| -------------------------------------------------------------------------- | ----------- | ----------------------------------------------------------------------------------------- |
| [Default Ripple](https://xrpl.org/rippling.html)                           | 비활성화        | 이 설정을 비활성화합니다. (기본값입니다.)                                                                  |
| [Authorized Trust Lines](https://xrpl.org/authorized-trust-lines.html)     | 활성화         | 이 설정을 활성화하고 핫 주소에 대한 신뢰선을 승인하지 않으면 실수로 잘못된 주소에서 토큰을 발행하는 것을 방지할 수 있습니다. (선택 사항이지만 권장합니다.) |
| [Require Destination Tags](https://xrpl.org/require-destination-tags.html) | 활성화 또는 비활성화 | 외부 시스템으로 토큰 인출을 처리하는 경우 활성화합니다. (예: 토큰이 스테이블코인 경우).                                       |
| Disallow XRP                                                               | 활성화 또는 비활성화 | 이 주소가 XRP 결제를 처리하지 않는 경우 활성화합니다.                                                          |
| [Domain](https://xrpl.org/accountset.html#domain)                          | (도메인 이름)    | 계정 소유권을 확인할 수 있도록 소유하고 있는 도메인으로 설정합니다. 이렇게 하면 혼동이나 사칭 시도를 줄일 수 있습니다.                      |

다음 코드 샘플은 권장 핫 주소 설정을 활성화하기 위해 AccountSet 트랜잭션을 보내는 방법을 보여줍니다:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// Configure hot address settings --------------------------------------------

  const hot_settings_tx = {
    "TransactionType": "AccountSet",
    "Account": hot_wallet.address,
    "Domain": "6578616D706C652E636F6D", // "example.com"
    // enable Require Auth so we can't use trust lines that users
    // make to the hot address, even by accident:
    "SetFlag": xrpl.AccountSetAsfFlags.asfRequireAuth,
    "Flags": (xrpl.AccountSetTfFlags.tfDisallowXRP |
              xrpl.AccountSetTfFlags.tfRequireDestTag)
  }

  const hst_prepared = await client.autofill(hot_settings_tx)
  const hst_signed = hot_wallet.sign(hst_prepared)
  console.log("Sending hot address AccountSet transaction...")
  const hst_result = await client.submitAndWait(hst_signed.tx_blob)
  if (hst_result.result.meta.TransactionResult == "tesSUCCESS") {
    console.log(`Transaction succeeded: https://testnet.xrpl.org/transactions/${hst_signed.hash}`)
  } else {
    throw `Error sending transaction: ${hst_result.result.meta.TransactionResult}`
  }
```
{% endtab %}

{% tab title="Python" %}
```python
# Configure hot address settings -----------------------------------------------
hot_settings_tx = xrpl.models.transactions.AccountSet(
    account=hot_wallet.address,
    set_flag=xrpl.models.transactions.AccountSetAsfFlag.ASF_REQUIRE_AUTH,
)

print("Sending hot address AccountSet transaction...")
response = xrpl.transaction.submit_and_wait(hot_settings_tx, client, hot_wallet)
print(response)
```
{% endtab %}

{% tab title="Java" %}
```java
// Configure hot address settings ------------------------------------------
    UnsignedInteger hotWalletSequence = xrplClient.accountInfo(
      AccountInfoRequestParams.builder()
        .ledgerSpecifier(LedgerSpecifier.CURRENT)
        .account(hotWalletKeyPair.publicKey().deriveAddress())
        .build()
    ).accountData().sequence();

    AccountSet setRequireAuth = AccountSet.builder()
      .account(hotWalletKeyPair.publicKey().deriveAddress())
      .fee(feeResult.drops().minimumFee())
      .sequence(hotWalletSequence)
      .signingPublicKey(hotWalletKeyPair.publicKey())
      .setFlag(AccountSet.AccountSetFlag.REQUIRE_AUTH)
      .lastLedgerSequence(computeLastLedgerSequence(xrplClient))
      .build();

    SingleSignedTransaction<AccountSet> signedSetRequireAuth = signatureService.sign(
      hotWalletKeyPair.privateKey(), setRequireAuth
    );
    submitAndWaitForValidation(signedSetRequireAuth, xrplClient);
```
{% endtab %}
{% endtabs %}

* [Generate](https://xrpl.org/issue-a-fungible-token.html#interactive-generate)
* [Connect](https://xrpl.org/issue-a-fungible-token.html#interactive-connect)
* [Configure Issuer](https://xrpl.org/issue-a-fungible-token.html#interactive-configure\_issuer)
* [Wait (Issuer Setup)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_issuer\_setup)
* [Configure Hot Address](https://xrpl.org/issue-a-fungible-token.html#interactive-configure\_hot\_address)
* [Wait (Hot Address Setup)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_hot\_address\_setup)
* [Make Trust Line](https://xrpl.org/issue-a-fungible-token.html#interactive-make\_trust\_line)
* [Wait (TrustSet)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_trustset)
* [Send Token](https://xrpl.org/issue-a-fungible-token.html#interactive-send\_token)
* [Wait (Payment)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_payment)
* [Confirm Balances](https://xrpl.org/issue-a-fungible-token.html#interactive-confirm\_balances)

## 6. 유효성 검사 대기

이전과 마찬가지로 이전 트랜잭션이 컨센서스를 통해 검증될 때까지 기다렸다가 계속 진행합니다.

* [Generate](https://xrpl.org/issue-a-fungible-token.html#interactive-generate)
* [Connect](https://xrpl.org/issue-a-fungible-token.html#interactive-connect)
* [Configure Issuer](https://xrpl.org/issue-a-fungible-token.html#interactive-configure\_issuer)
* [Wait (Issuer Setup)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_issuer\_setup)
* [Configure Hot Address](https://xrpl.org/issue-a-fungible-token.html#interactive-configure\_hot\_address)
* [Wait (Hot Address Setup)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_hot\_address\_setup)
* [Make Trust Line](https://xrpl.org/issue-a-fungible-token.html#interactive-make\_trust\_line)
* [Wait (TrustSet)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_trustset)
* [Send Token](https://xrpl.org/issue-a-fungible-token.html#interactive-send\_token)
* [Wait (Payment)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_payment)
* [Confirm Balances](https://xrpl.org/issue-a-fungible-token.html#interactive-confirm\_balances)

## 7. 핫 주소에서 콜드 주소로 신뢰선 생성

토큰을 받으려면 먼저 토큰 발행자에 대한 신뢰선을 생성해야 합니다. 이 신뢰선은 발행하려는 토큰의 통화 코드(예: USD 또는 FOO)에 따라 다릅니다. 원하는 통화 코드는 무엇이든 선택할 수 있으며, 각 발행자의 토큰은 XRP Ledggrer 프로토콜에서 별도의 토큰으로 취급됩니다. 그러나 사용자가 리플 설정을 활성화하면 동일한 통화 코드를 가진 토큰의 잔액이 다른 발행자 간에 ripple될 수 있습니다.

핫 주소는 발행자로부터 토큰을 받기 전에 이와 같은 신뢰선이 필요합니다. 마찬가지로 토큰을 보유하려는 각 사용자도 trust line¹을 생성해야 합니다. 각 신뢰선은 핫 주소의 [reserve requirement](https://xrpl.org/reserves.html)을 증가시키므로, 증가된 요구량을 지불할 수 있는 충분한 양의 XRP를 보유해야 합니다. 신뢰선을 제거하면 reserve requirement은 다시 낮아집니다.

{% hint style="info" %}
Tip:

신뢰선은 사람이 보유할 수 있는 '한도'가 있으며, 다른 사람은 지정된 한도보다 더 많은 토큰을 보낼 수 없습니다. 커뮤니티 크레딧 시스템의 경우, 해당 사람을 얼마나 신뢰하는지에 따라 개인별 한도를 구성할 수 있습니다. 다른 유형과 용도의 토큰의 경우 일반적으로 한도를 매우 큰 수로 설정해도 괜찮습니다.
{% endhint %}

신뢰선을 만들려면 핫 주소에서 다음 필드가 포함된 [TrustSet](https://xrpl.org/trustset.html)트랜잭션을 전송합니다:

| 필드                     | 값값                                   |
| ---------------------- | ------------------------------------ |
| `TransactionType`      | `"TrustSet"`                         |
| `Account`              | 계정 핫 주소입니다. (일반적으로 토큰을 받으려는 계정입니다.)  |
| `LimitAmount`          | 어느 발행자의 토큰을 얼마만큼 보유할 것인지 지정하는 객체입니다. |
| `LimitAmount.currency` | 토큰의 통화 코드입니다.                        |
| `LimitAmount.issuer`   | 콜드 주소입니다.                            |
| `LimitAmount.value`    | 보유하고자 하는 토큰의 최대 금액입니다.               |

&#x20;다음 코드 샘플은 핫 주소에서 10억 FOO의 한도로 발행 주소를 신뢰하여 [TrustSet transaction](https://xrpl.org/trustset.html)을 보내는 방법을 보여줍니다:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// Create trust line from hot to cold address --------------------------------
  const currency_code = "FOO"
  const trust_set_tx = {
    "TransactionType": "TrustSet",
    "Account": hot_wallet.address,
    "LimitAmount": {
      "currency": currency_code,
      "issuer": cold_wallet.address,
      "value": "10000000000" // Large limit, arbitrarily chosen
    }
  }

  const ts_prepared = await client.autofill(trust_set_tx)
  const ts_signed = hot_wallet.sign(ts_prepared)
  console.log("Creating trust line from hot address to issuer...")
  const ts_result = await client.submitAndWait(ts_signed.tx_blob)
  if (ts_result.result.meta.TransactionResult == "tesSUCCESS") {
    console.log(`Transaction succeeded: https://testnet.xrpl.org/transactions/${ts_signed.hash}`)
  } else {
    throw `Error sending transaction: ${ts_result.result.meta.TransactionResult}`
  }
```
{% endtab %}

{% tab title="Python" %}
```python
# Create trust line from hot to cold address -----------------------------------
currency_code = "FOO"
trust_set_tx = xrpl.models.transactions.TrustSet(
    account=hot_wallet.address,
    limit_amount=xrpl.models.amounts.issued_currency_amount.IssuedCurrencyAmount(
        currency=currency_code,
        issuer=cold_wallet.address,
        value="10000000000", # Large limit, arbitrarily chosen
    )
)

print("Creating trust line from hot address to issuer...")
response = xrpl.transaction.submit_and_wait(trust_set_tx, client, hot_wallet)
print(response)
```
{% endtab %}

{% tab title="Java" %}
```java
// Create trust line -------------------------------------------------------
    String currencyCode = "FOO";
    ImmutableTrustSet trustSet = TrustSet.builder()
      .account(hotWalletKeyPair.publicKey().deriveAddress())
      .fee(feeResult.drops().openLedgerFee())
      .sequence(hotWalletSequence.plus(UnsignedInteger.ONE))
      .limitAmount(IssuedCurrencyAmount.builder()
        .currency(currencyCode)
        .issuer(coldWalletKeyPair.publicKey().deriveAddress())
        .value("10000000000")
        .build())
      .signingPublicKey(hotWalletKeyPair.publicKey())
      .build();

    SingleSignedTransaction<TrustSet> signedTrustSet = signatureService.sign(
      hotWalletKeyPair.privateKey(), trustSet
    );

    submitAndWaitForValidation(signedTrustSet, xrplClient);
```
{% endtab %}
{% endtabs %}

* [Generate](https://xrpl.org/issue-a-fungible-token.html#interactive-generate)
* [Connect](https://xrpl.org/issue-a-fungible-token.html#interactive-connect)
* [Configure Issuer](https://xrpl.org/issue-a-fungible-token.html#interactive-configure\_issuer)
* [Wait (Issuer Setup)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_issuer\_setup)
* [Configure Hot Address](https://xrpl.org/issue-a-fungible-token.html#interactive-configure\_hot\_address)
* [Wait (Hot Address Setup)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_hot\_address\_setup)
* [Make Trust Line](https://xrpl.org/issue-a-fungible-token.html#interactive-make\_trust\_line)
* [Wait (TrustSet)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_trustset)
* [Send Token](https://xrpl.org/issue-a-fungible-token.html#interactive-send\_token)
* [Wait (Payment)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_payment)
* [Confirm Balances](https://xrpl.org/issue-a-fungible-token.html#interactive-confirm\_balances)

{% hint style="info" %}
Note:

인증된 신뢰선을 사용하는 경우 이 단계 다음에 추가 단계가 있는데, 바로 콜드 주소가 핫 주소의 신뢰선을 승인해야 한다는 것입니다. 이 방법에 대한 자세한 내용은 인증 신뢰선 승인하기를 참조하세요.
{% endhint %}

## 8. 유효성 검사 대기

이전과 마찬가지로 이전 트랜잭션이 컨센서스를 통해 검증될 때까지 기다렸다가 계속 진행합니다.

* [Generate](https://xrpl.org/issue-a-fungible-token.html#interactive-generate)
* [Connect](https://xrpl.org/issue-a-fungible-token.html#interactive-connect)
* [Configure Issuer](https://xrpl.org/issue-a-fungible-token.html#interactive-configure\_issuer)
* [Wait (Issuer Setup)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_issuer\_setup)
* [Configure Hot Address](https://xrpl.org/issue-a-fungible-token.html#interactive-configure\_hot\_address)
* [Wait (Hot Address Setup)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_hot\_address\_setup)
* [Make Trust Line](https://xrpl.org/issue-a-fungible-token.html#interactive-make\_trust\_line)
* [Wait (TrustSet)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_trustset)
* [Send Token](https://xrpl.org/issue-a-fungible-token.html#interactive-send\_token)
* [Wait (Payment)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_payment)
* [Confirm Balances](https://xrpl.org/issue-a-fungible-token.html#interactive-confirm\_balances)

## 9. 토큰 보내기

이제 콜드 주소에서 핫 주소로 결제 트랜잭션을 전송하여 토큰을 생성할 수 있습니다. 이 트랜잭션에는 다음과 같은 속성이 있어야 합니다(점 표기는 중첩된 필드를 나타냄):

| 필드                | 값                                                                                                                                                |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| `TransactionType` | `"Payment"`                                                                                                                                      |
| `Account`         | 계정 토큰을 발급하는 콜드 주소입니다.                                                                                                                            |
| `Amount`          | 금액 생성할 토큰의 양을 지정하는 토큰 금액입니다.                                                                                                                     |
| `Amount.currency` | 토큰의 화폐 코드입니다.                                                                                                                                    |
| `Amount.value`    | 발행할 토큰의 십진수 금액(문자열)입니다.                                                                                                                          |
| `Amount.issuer`   | 발급자 토큰을 발급하는 콜드 주소입니다.                                                                                                                           |
| `Destination`     | 대상 토큰을 받는 핫 주소(또는 토큰을 받는 다른 계정)입니다.                                                                                                              |
| `Paths`           | `Paths`토큰을 발급할 때는 이 필드를 생략합니다.                                                                                                                   |
| `SendMax`         | `SendMax` 토큰을 발행할 때는 이 필드를 생략합니다.                                                                                                                |
| `DestinationTag`  | 데스티네이 태그 0에서 232-1 사이의 정수입니다. 핫 주소에서 [Require Destination Tags](https://xrpl.org/require-destination-tags.html)를 사용하도록 설정한 경우 여기에 무언가를 지정해야 합니다. |

다른 모든 필수 필드에는 자동 입력된 값을 사용할 수 있습니다.

다음 코드 샘플은 콜드 주소에서 핫 주소로 88 FOO를 발행하는 결제 트랜잭션을 전송하는 방법을 보여줍니다:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// Send token ----------------------------------------------------------------
  const issue_quantity = "3840"
  const send_token_tx = {
    "TransactionType": "Payment",
    "Account": cold_wallet.address,
    "Amount": {
      "currency": currency_code,
      "value": issue_quantity,
      "issuer": cold_wallet.address
    },
    "Destination": hot_wallet.address,
    "DestinationTag": 1 // Needed since we enabled Require Destination Tags
                        // on the hot account earlier.
  }

  const pay_prepared = await client.autofill(send_token_tx)
  const pay_signed = cold_wallet.sign(pay_prepared)
  console.log(`Sending ${issue_quantity} ${currency_code} to ${hot_wallet.address}...`)
  const pay_result = await client.submitAndWait(pay_signed.tx_blob)
  if (pay_result.result.meta.TransactionResult == "tesSUCCESS") {
    console.log(`Transaction succeeded: https://testnet.xrpl.org/transactions/${pay_signed.hash}`)
  } else {
    throw `Error sending transaction: ${pay_result.result.meta.TransactionResult}`
  }
```
{% endtab %}

{% tab title="Python" %}
```python
# Send token -------------------------------------------------------------------
issue_quantity = "3840"
send_token_tx = xrpl.models.transactions.Payment(
    account=cold_wallet.address,
    destination=hot_wallet.address,
    amount=xrpl.models.amounts.issued_currency_amount.IssuedCurrencyAmount(
        currency=currency_code,
        issuer=cold_wallet.address,
        value=issue_quantity
    )
)

print(f"Sending {issue_quantity} {currency_code} to {hot_wallet.address}...")
response = xrpl.transaction.submit_and_wait(send_token_tx, client, cold_wallet)
print(response)
```
{% endtab %}

{% tab title="Java" %}
```java
// Send token --------------------------------------------------------------
    Payment payment = Payment.builder()
      .account(coldWalletKeyPair.publicKey().deriveAddress())
      .fee(feeResult.drops().openLedgerFee())
      .sequence(coldWalletSequence.plus(UnsignedInteger.ONE))
      .destination(hotWalletKeyPair.publicKey().deriveAddress())
      .amount(IssuedCurrencyAmount.builder()
        .issuer(coldWalletKeyPair.publicKey().deriveAddress())
        .currency(currencyCode)
        .value("3840")
        .build())
      .signingPublicKey(coldWalletKeyPair.publicKey())
      .build();

    SingleSignedTransaction<Payment> signedPayment = signatureService.sign(
      coldWalletKeyPair.privateKey(), payment
    );

    submitAndWaitForValidation(signedPayment, xrplClient);
```
{% endtab %}
{% endtabs %}

## 10. 유효성 검사 대기

이전과 마찬가지로 이전 트랜잭션이 컨센서스를 통해 유효성을 검사할 때까지 기다렸다가 계속 진행합니다.

* [Generate](https://xrpl.org/issue-a-fungible-token.html#interactive-generate)
* [Connect](https://xrpl.org/issue-a-fungible-token.html#interactive-connect)
* [Configure Issuer](https://xrpl.org/issue-a-fungible-token.html#interactive-configure\_issuer)
* [Wait (Issuer Setup)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_issuer\_setup)
* [Configure Hot Address](https://xrpl.org/issue-a-fungible-token.html#interactive-configure\_hot\_address)
* [Wait (Hot Address Setup)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_hot\_address\_setup)
* [Make Trust Line](https://xrpl.org/issue-a-fungible-token.html#interactive-make\_trust\_line)
* [Wait (TrustSet)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_trustset)
* [Send Token](https://xrpl.org/issue-a-fungible-token.html#interactive-send\_token)
* [Wait (Payment)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_payment)
* [Confirm Balances](https://xrpl.org/issue-a-fungible-token.html#interactive-confirm\_balances)

## 11. 토큰 잔액 확인

토큰 발행자 또는 핫 주소의 관점에서 토큰의 잔액을 확인할 수 있습니다. XRP Ledger에서 발행된 토큰의 잔액은 항상 0이 되며, 발행자 관점에서는 음수이고 보유자 관점에서는 양수입니다.

[account\_lines method](https://xrpl.org/account\_lines.html)를 사용하여 소유자 관점에서 잔액을 조회할 수 있습니다. 여기에는 각 신뢰이 한도, 잔액 및 설정과 함께 나열됩니다.&#x20;

토큰 발행자 관점에서 잔액을 조회하려면 [gateway\_balances](https://xrpl.org/gateway\_balances.html) 메소드를 사용합니다. 이 메소드는 특정 주소에서 발행한 모든 토큰의 합계를 제공합니다.

{% hint style="info" %}
Tip:

XRP Ledger은 완전히 공개되어 있으므로 암호화 키 없이도 언제든지 모든 계정의 잔액을 확인할 수 있습니다.&#x20;
{% endhint %}

다음 코드 샘플은 두 가지 방법을 모두 사용하는 방법을 보여줍니다:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// Check balances ------------------------------------------------------------
  console.log("Getting hot address balances...")
  const hot_balances = await client.request({
    command: "account_lines",
    account: hot_wallet.address,
    ledger_index: "validated"
  })
  console.log(hot_balances.result)

  console.log("Getting cold address balances...")
  const cold_balances = await client.request({
    command: "gateway_balances",
    account: cold_wallet.address,
    ledger_index: "validated",
    hotwallet: [hot_wallet.address]
  })
  console.log(JSON.stringify(cold_balances.result, null, 2))

  client.disconnect()
}
```
{% endtab %}

{% tab title="Python" %}
```python
# Check balances ---------------------------------------------------------------
print("Getting hot address balances...")
response = client.request(xrpl.models.requests.AccountLines(
    account=hot_wallet.address,
    ledger_index="validated",
))
print(response)

print("Getting cold address balances...")
response = client.request(xrpl.models.requests.GatewayBalances(
    account=cold_wallet.address,
    ledger_index="validated",
    hotwallet=[hot_wallet.address]
))
print(response)
```
{% endtab %}

{% tab title="Java" %}
```java
// Check balances ----------------------------------------------------------
    List<TrustLine> lines = xrplClient.accountLines(
      AccountLinesRequestParams.builder()
        .account(hotWalletKeyPair.publicKey().deriveAddress())
        .ledgerSpecifier(LedgerSpecifier.VALIDATED)
        .build()
    ).lines();
    System.out.println("Hot wallet TrustLines: " + lines);
  }
```
{% endtab %}
{% endtabs %}

* [Generate](https://xrpl.org/issue-a-fungible-token.html#interactive-generate)
* [Connect](https://xrpl.org/issue-a-fungible-token.html#interactive-connect)
* [Configure Issuer](https://xrpl.org/issue-a-fungible-token.html#interactive-configure\_issuer)
* [Wait (Issuer Setup)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_issuer\_setup)
* [Configure Hot Address](https://xrpl.org/issue-a-fungible-token.html#interactive-configure\_hot\_address)
* [Wait (Hot Address Setup)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_hot\_address\_setup)
* [Make Trust Line](https://xrpl.org/issue-a-fungible-token.html#interactive-make\_trust\_line)
* [Wait (TrustSet)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_trustset)
* [Send Token](https://xrpl.org/issue-a-fungible-token.html#interactive-send\_token)
* [Wait (Payment)](https://xrpl.org/issue-a-fungible-token.html#interactive-wait\_payment)
* [Confirm Balances](https://xrpl.org/issue-a-fungible-token.html#interactive-confirm\_balances)

## 다음 단계

이제 토큰을 만들었으므로, 토큰이 XRP Ledger의 기능에 어떻게 적용되는지 살펴볼 수 있습니다:

* 핫 주소에서 다른 사용자에게 토큰 보내기.
* 탈중앙화 거래소에서 토큰을 거래합니다.
* 토큰의 수신 결제를 모니터링합니다.
* xrp-ledger.toml 파일을 생성하고 토큰 발행자에 대한 도메인 확인을 설정합니다.
* XRP Ledger 토큰의 다른 기능에 대해 알아보세요.

## 각주

¹ 사용자가 탈중앙화 거래소에서 토큰을 구매하면 명시적으로 신뢰선을 만들지 않고도 토큰을 보유할 수 있습니다. 거래소에서 토큰을 구매하면 필요한 신뢰선이 자동으로 생성됩니다. 이는 누군가 탈중앙화 거래소에서 토큰을 판매하는 경우에만 가능합니다.
