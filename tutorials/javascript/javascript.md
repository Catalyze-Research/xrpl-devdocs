# JavaScript로 시작하기

이 튜토리얼은 Node.js 또는 웹 브라우저에서 [xrpl.js](https://github.com/XRPLF/xrpl.js/) 클라이언트 라이브러리를 사용해 자바스크립트 또는 타입스크립트로 XRP 레저 연결 애플리케이션을 구축하는 기본 사항을 안내합니다.

이 가이드에 사용된 스크립트와 구성 파일은 이 웹사이트의 [깃허브 리포지토리](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/get-started/js/)에서 구할 수 있습니다.

## 학습 목표

튜토리얼에서 배우게 될 내용:

* XRP Ledger 기반 애플리케이션의 기본 구성 요소.&#x20;
* xrpl.js를 사용해 XRP Ledger에 연결하는 방법.&#x20;
* xrpl.js를 사용하여 테스트넷에서 계정을 얻는 방법.&#x20;
* xrpl.js 라이브러리를 사용해 XRP Ledger에서 계정에 대한 정보를 조회하는 방법.
* 이러한 단계를 조합하여 자바스크립트 앱 또는 웹앱을 만드는 방법.

## 요구 사항

이 튜토리얼을 따라오기 위해서는, JavaScript로 코드를 작성하고 소규모 JavaScript 프로젝트를 관리하는 데 어느 정도 익숙해야 합니다. 브라우저의 경우 JavaScript를 지원하는 최신 웹 브라우저는 모두 정상적으로 작동합니다. Node.js에서는 **버전 14**를 권장합니다. Node.js 버전 12와 16도 정기적으로 테스트됩니다.

## NPM으로 설치

빈 폴더를 생성하여 새 프로젝트를 시작한 다음 해당 폴더로 이동하여 [**NPM**](https://www.npmjs.com/)을 사용하여 최신 버전의 xrpl.js를 설치합니다:

```

npm install xrpl

```



## BUILD 시작하기

XRP 원장으로 작업할 때 계정에 XRP를 추가하거나, 탈중앙화 거래소와 통합하거나, 토큰을 발행하는 등 몇 가지 관리해야 할 사항이 있습니다. 이 튜토리얼에서는 이러한 모든 사용 사례를 시작하는 데 공통적인 기본 패턴을 안내하고, 이를 구현하기 위한 샘플 코드를 제공합니다.

다음은 많은 XRP 레저 프로젝트에서 사용하는 몇 가지 단계입니다:

1. [Import the library.](https://xrpl.org/get-started-using-javascript.html#1-import-the-library)
2. [Connect to the XRP Ledger.](https://xrpl.org/get-started-using-javascript.html#2-connect-to-the-xrp-ledger)
3. [Get an account.](https://xrpl.org/get-started-using-javascript.html#3-get-account)
4. [Query the XRP Ledger.](https://xrpl.org/get-started-using-javascript.html#4-query-the-xrp-ledger)
5. [Listen for Events.](https://xrpl.org/get-started-using-javascript.html#5-listen-for-events)

## 1. Import the Library(라이브러리 가져오기) <a href="#1-import-the-library" id="1-import-the-library"></a>

프로젝트에 xrpl.js를 로드하는 방법은 개발 환경에 따라 다릅니다:

### **Web Browsers(웹 브라우저)**

HTML에 다음과 같은 태그를 추가합니다:

```
<script src="https://unpkg.com/xrpl/build/xrpl-latest-min.js"></script>
```

위의 예에서와 같이 CDN에서 라이브러리를 로드하거나 릴리스를 다운로드하여 자체 웹사이트에서 호스팅할 수 있습니다.

이렇게 하면 모듈이 최상위 레벨에 xrpl로 로드됩니다.

### Node.js

npm을 사용하여 라이브러리를 추가합니다. 이렇게 하면 package.json 파일이 업데이트되거나 파일이 없는 경우 새 파일이 생성됩니다:

```
npm install xrpl
```

그런 다음 라이브러리를 가져옵니다:

```
const xrpl = require("xrpl")
```

## 2. Connect to the XRP Ledger <a href="#2-connect-to-the-xrp-ledger" id="2-connect-to-the-xrp-ledger"></a>

트랜잭션을 쿼리하고 제출하려면 XRP 원장에 연결해야 합니다. xrpl.js를 사용하여 이 작업을 수행하려면 Client 클래스의 인스턴스를 생성하고 connect() 메서드를 사용합니다.

{% hint style="success" %}
**TIP:** \
xrpl.js의 많은 네트워크 함수는 Promises를 사용하여 값을 비동기적으로 반환합니다. 여기 코드 샘플은 async/await 패턴을 사용하여 Promises의 실제 결과를 기다립니다.
{% endhint %}

```
// In browsers, use a <script> tag. In Node.js, uncomment the following line:
// const xrpl = require('xrpl')

// Wrap code in an async function so we can use await
async function main() {

  // Define the network client
  const client = new xrpl.Client("wss://s.altnet.rippletest.net:51233")
  await client.connect()

  // ... custom code goes here

  // Disconnect when done (If you omit this, Node.js won't end the process)
  await client.disconnect()
}

main()
```

### **Connect to the XRP Ledger Mainnet**

이전 섹션의 샘플 코드는 사용 가능한 병렬 네트워크 중 하나인 테스트넷에 연결하는 방법을 보여드리고 있습니다. 프로덕션으로 전환할 준비가 되면, XRP 원장 메인넷에 연결해야 합니다. 두 가지 방법으로 연결할 수 있습니다:

*   코어 서버(리플드)를 설치하고 노드를 직접 실행합니다. 코어 서버는 기본적으로 메인넷에 연결되지만, 테스트넷이나 개발넷을 사용하도록 구성을 변경할 수 있습니다. 자체 코어 서버를 실행하는 데는 여러 가지 이유가 있습니다. 자체 서버를 실행하는 경우 다음과 같이 연결할 수 있습니다:\


    ```
    const MY_SERVER = "ws://localhost:6006/"
    const client = new xrpl.Client(MY_SERVER)
    await client.connect()
    ```

    \
    기본값에 대한 자세한 내용은 예제 코어 서버 구성 파일을 참조하세요.
*   사용 가능한 Public Severs 중 하나를 사용합니다:\


    ```
    const PUBLIC_SERVER = "wss://xrplcluster.com/"
    const client = new xrpl.Client(PUBLIC_SERVER)
    await client.connect()
    ```

## 3. Get Account <a href="#3-get-account" id="3-get-account"></a>

xrpl.js 라이브러리에는 XRP 레저 계정의 키와 주소를 처리하는 월렛 클래스가 있습니다. 테스트넷에서는 이와 같이 새 계정에 자금을 입금할 수 있습니다:

```
// Create a wallet and fund it with the Testnet faucet:
  const fund_result = await client.fundWallet()
  const test_wallet = fund_result.wallet
  console.log(fund_result)
```

키만 생성하려면 다음과 같이 새 지갑 인스턴스를 만들면 됩니다:

```
const test_wallet = xrpl.Wallet.generate()
```

또는 이미 base58로 인코딩된 시드가 있는 경우, 다음과 같이 월렛 인스턴스를 만들 수 있습니다.

```
const test_wallet = xrpl.Wallet.fromSeed("sn3nxiW7v8KXzPzAqzyHXbSSKNuN9") // Test secret; don't use for real
```

## 4. Query the XRP Ledger <a href="#4-query-the-xrp-ledger" id="4-query-the-xrp-ledger"></a>

클라이언트의 요청() 메서드를 사용해 XRP 원장의 웹소켓 API에 액세스합니다. 예를 들어

```
// Get info from the ledger about the address we just funded
  const response = await client.request({
    "command": "account_info",
    "account": test_wallet.address,
    "ledger_index": "validated"
  })
  console.log(response)
```

## 5. Listen for Events

XRP 레저의 합의 프로세스가 새로운 레저 버전을 생성할 때마다 등 다양한 유형의 이벤트에 대한 핸들러를 xrpl.js에 설정할 수 있습니다. 이렇게 하려면 먼저 구독 메서드를 호출하여 원하는 이벤트 유형을 가져온 다음, 클라이언트의 on(eventType, callback) 메서드를 사용하여 이벤트 핸들러를 첨부합니다.

```
// Listen to ledger close events
  client.request({
    "command": "subscribe",
    "streams": ["ledger"]
  })
  client.on("ledgerClosed", async (ledger) => {
    console.log(`Ledger #${ledger.ledger_index} validated with ${ledger.txn_count} transactions!`)
  })
```

## NEXT STEP

이제 xrpl.js를 사용해 XRP 원장에 연결하고, 계정을 생성하고, 관련 정보를 조회하는 방법을 알게 되었으니, 이제 다른 방법도 사용하실 수 있습니다:

* [Send XRP](https://xrpl.org/send-xrp.html).
* [Issue a Fungible Token](https://xrpl.org/issue-a-fungible-token.html)
* [Set up secure signing](https://xrpl.org/secure-signing.html) for your account.

### See Also <a href="#see-also" id="see-also"></a>

* **Concepts:**
  * [XRP Ledger Overview](https://xrpl.org/xrp-ledger-overview.html)
  * [Client Libraries](https://xrpl.org/client-libraries.html)
* **Tutorials:**
  * [Send XRP](https://xrpl.org/send-xrp.html)
* **References:**
  * [`xrpl.js` Reference ](https://js.xrpl.org/)
  * [Public API Methods](https://xrpl.org/public-api-methods.html)
  * [API Conventions](https://xrpl.org/api-conventions.html)
    * [base58 Encodings](https://xrpl.org/base58-encodings.html)
  * [Transaction Formats](https://xrpl.org/transaction-formats.html)



