# Javascript로 시작하기

이 튜토리얼은 Node.js 또는 웹 브라우저에서 xrpl.js 클라이언트 라이브러리를 사용하여 JavaScript 또는 TypeScript로 XRP Ledger 연결 애플리케이션을 구축하는 기초를 안내합니다.

이 튜토리얼에서 사용하는 스크립트와 설정 파일은 이 웹사이트의 GitHub 저장소에서 사용할 수 있습니다.

## 학습 목표&#x20;

이 튜토리얼에서 배우게 될 내용은 다음과 같습니다:

* XRP Ledger 기반 응용 프로그램의 기본 구성 요소.
* xrpl.js를 사용하여 XRP Ledger에 연결하는 방법.
* xrpl.js를 사용하여 Testnet에서 계정을 얻는 방법.
* xrpl.js 라이브러리를 사용하여 XRP Ledger의 계정에 대한 정보를 찾는 방법.
* 이러한 단계를 결합하여 JavaScript 앱 또는 웹 앱을 생성하는 방법.

## 요구 사항&#x20;

이 튜토리얼을 따르려면 JavaScript로 코드를 작성하고 작은 JavaScript 프로젝트를 관리하는 데 익숙해야 합니다. 브라우저에서는 JavaScript를 지원하는 모든 최신 웹 브라우저에서 잘 작동해야 합니다. Node.js에서는 버전 14가 권장됩니다. Node.js 버전 12와 16도 정기적으로 테스트됩니다.

## npm으로 설치하기&#x20;

빈 폴더를 생성하여 새 프로젝트를 시작한 다음, 해당 폴더로 이동하고 NPM을 사용하여 xrpl.js의 최신 버전을 설치합니다:

```
npm install xrpl
```

## 개발 시작&#x20;

XRP Ledger를 사용하면, 계정에 XRP를 추가하거나, 탈중앙화 거래소와 통합하거나, 토큰을 발행하는 것이라 할지라도 관리해야 할 몇 가지 사항이 있습니다. 이 튜토리얼은 이러한 모든 사용 사례를 시작하는 데 공통적인 기본 패턴을 안내하며, 이를 구현하는 샘플 코드를 제공합니다.

다음은 많은 XRP Ledger 프로젝트에서 사용하는 단계입니다:

1. 라이브러리를 가져옵니다.
2. XRP Ledger에 연결합니다.
3. 계정을 얻습니다.
4. XRP Ledger를 조회합니다.
5. 이벤트를 수신합니다.

## 1. 라이브러리 가져오기&#x20;

프로젝트에 xrpl.js를 로드하는 방법은 개발 환경에 따라 다릅니다:

### 웹 브라우저&#x20;

HTML에 다음과 같은 \<script> 태그를 추가합니다:

```
<script src="https://unpkg.com/xrpl/build/xrpl-latest-min.js"></script>
```

위의 예제처럼 CDN에서 라이브러리를 로드하거나, 릴리스를 다운로드하여 자체 웹사이트에서 호스팅할 수 있습니다.

이렇게 하면 모듈이 xrpl로 최상위 레벨에 로드됩니다.

## Node.js&#x20;

npm을 사용하여 라이브러리를 추가합니다. 이는 package.json 파일을 업데이트하거나, 존재하지 않을 경우 새로 만듭니다:

```
npm install xrpl
```

그런 다음 라이브러리를 가져옵니다:

```
const xrpl = require("xrpl")
```

## 2. XRP Ledger에 연결하기&#x20;

쿼리를 수행하고 트랜잭션을 제출하려면 XRP Ledger에 연결해야 합니다. xrpl.js로 이를 수행하려면 Client 클래스의 인스턴스를 생성하고 connect() 메소소드를 사용합니다.

{% hint style="info" %}
Tip:

xrpl.js의 많은 네트워크 함수는 Promises를 사용하여 값 반환을 비동기적으로 처리합니다. 여기에 제공된 코드 샘플은 실제 Promise의 결과를 대기하기 위해 async/await 패턴을 사용합니다.
{% endhint %}

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

## XRP Ledger Mainnet에 연결하기&#x20;

이전 섹션의 샘플 코드는 Testnet에 연결하는 방법을 보여줍니다. 이는 사용 가능한 병렬 네트워크 중 하나입니다. 프로덕션으로 이동할 준비가 되면 XRP Ledger Mainnet에 연결해야 합니다. 이는 두 가지 방법으로 수행할 수 있습니다:

* 핵심 서버(rippled)를 설치하고 자체 노드를 실행합니다. 핵심 서버는 기본적으로 Mainnet에 연결하지만, Testnet 또는 Devnet를 사용하도록 설정을 변경할 수 있습니다. 자체 코어 서버를 실행하는 것이 좋은 이유가 있습니다. 서버를 직접 실행하는 경우, 다음과 같이 연결할 수 있습니다:

```javascript
const MY_SERVER = "ws://localhost:6006/"
const client = new xrpl.Client(MY_SERVER)
await client.connect()
```

기본 값에 대한 자세한 정보는 코어 서버설정 파일 예제를 참조하십시오.

* 사용 가능한 공용 서버 중 하나를 사용합니다:

```
const PUBLIC_SERVER = "wss://xrplcluster.com/"
const client = new xrpl.Client(PUBLIC_SERVER)
await client.connect()
```

## 3. 계정 얻기&#x20;

xrpl.js 라이브러리는 XRP Ledger 계정의 키와 주소를 처리하기 위한 Wallet 클래스를 가지고 있습니다. Testnet에서는 다음과 같이 새 계정을 충전할 수 있습니다:

```javascript
// Create a wallet and fund it with the Testnet faucet:
  const fund_result = await client.fundWallet()
  const test_wallet = fund_result.wallet
  console.log(fund_result)
```

키만 생성하려면 이렇게 새 Wallet 인스턴스를 생성할 수 있습니다:

```javascript
const test_wallet = xrpl.Wallet.generate()
```

또는, 이미 base58로 인코딩된 시드가 있는 경우, 다음과 같이 Wallet 인스턴스를 생성할 수 있습니다:

```javascript
const test_wallet = xrpl.Wallet.fromSeed("sn3nxiW7v8KXzPzAqzyHXbSSKNuN9") // Test secret; don't use for real
```

## 4. XRP Ledger 조회하기&#x20;

Client의 request() 메소소드를 사용하여 XRP Ledger의 WebSocket API에 액세스할 수 있습니다. 예를 들면:

```javascript
// Get info from the ledger about the address we just funded
  const response = await client.request({
    "command": "account_info",
    "account": test_wallet.address,
    "ledger_index": "validated"
  })
  console.log(response)
```

## 5. 이벤트 수신하기&#x20;

xrpl.js에서는 XRP Ledger의 컨센서스 프로세스가 새 ledger 버전을 생성할 때마다 등 여러 유형의 이벤트에 대한 핸들러를 설정할 수 있습니다. 이를 수행하려면 먼저 subscribe 메소드드를 호출하여 원하는 유형의 이벤트를 가져오고, 그 다음에 클라이언트의 on(eventType, callback) 메소드를 사용하여 이벤트 핸들러를 연결합니다.

```javascript
javascriptCopy code// 원장 종료 이벤트를 수신합니다
  client.request({
    "command": "subscribe",
    "streams": ["ledger"]
  })
  client.on("ledgerClosed", async (ledger) => {
    console.log(`Ledger #${ledger.ledger_index} validated with ${ledger.txn_count} transactions!`)
  })
```

## 계속 만들기&#x20;

이제 xrpl-py를 사용하여 XRP Ledger에 연결하고, 계정을 가져오고, 그 정보를 조회하는 방법을 알았으니, xrpl-py를 이용하여 다음 작업도 수행할 수 있습니다:

* 계정에 대한 안전한 서명 설정하기
* XRP 보내기

\
