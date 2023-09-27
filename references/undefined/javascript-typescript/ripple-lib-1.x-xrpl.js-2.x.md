# ripple-lib 1.x에서 xrpl.js 2.x로의 마이그레이션 가이드

다음 지침에 따라 **ripple-lib**(1.x)를 사용하는 JavaScript / TypeScript 코드를 마이그레이션하여 XRP Ledger에 xrpl.js(2.x) 라이브러리를 대신 사용하세요.

{% hint style="info" %}
Tip:

필요한 경우 [legacy 1.x "RippleAPI" ](https://github.com/XRPLF/xrpl.js/blob/1.x/docs/index.md)에 대한 문서에 계속 액세스할 수 있습니다.
{% endhint %}

## 높은 수준의 차이점

많은 필드와 함수가 2.0 버전에서 "새로운" 이름을 갖게 되었으며, 더 정확하게 말하자면, 이제 xrpl.js는 HTTP/웹소켓 API와 동일한 이름을 사용합니다. 주문 취소 객체와 같이 리플 라이브러리 고유의 구조체는 사라졌으며, 대신 라이브러리는 "OfferCancel"과 같은 XRP Ledger의 기본 트랜잭션 유형을 사용합니다. 리플 라이브러리 1.x에서 이러한 구조를 반환하는 많은 API 메소드가 사라졌으며, 2.0에서는 웹소켓 API와 동일한 형식으로 요청을 하고 응답을 받습니다.

ripple-lib 1.x의 포괄적인 RippleAPI 클래스도 사라졌습니다. xrpl.js 2.x에서는 네트워크 작업을 처리하는 Client 클래스가 있으며, 그 외의 모든 작업은 엄격하게 오프라인으로 이루어집니다. 주소 및 키를 위한 새로운 Wallet 클래스와 최상위 xrpl 객체 아래에 다른 클래스 및 프로퍼티가 있습니다.

## Boilerplate 비교

**ripple-lib 1.10.0:**

```javascript
const ripple = require('ripple-lib');

(async function() {
  const api = new ripple.RippleAPI({
    server: 'wss://xrplcluster.com'
  });

  await api.connect();

  // Your code here

  api.disconnect();
})();
```

**xrpl.js 2.0.0:**

```javascript
const xrpl = require("xrpl");

(async function() {
  const client = new xrpl.Client('wss://xrplcluster.com');

  await client.connect();

  // Your code here

  client.disconnect();
})();
```

## 검증된 결과

기본적으로 ripple-lib 1.x의 대부분의 메소드는 컨센서 프로세스에 의해 유효성이 검사된 최종 결과만 반환합니다. 많은 메소드에 해당하는 xrpl.js 메소드는 Client.request() 메소드를 사용하여 웹소켓 API를 호출하는데, XRP LEDGER 서버의 기본 설정은 종종 현재(보류 중) ledger을 사용하여 최종이 아닌 데이터를 제공합니다.

탈중앙화 거래소의 상태를 조회할 때와 같이 성공 가능성이 높은 많은 트랜잭션의 보류 중인 결과가 있기 때문에 현재 공개 ledger을 사용하고자 하는 경우가 있습니다. 다른 경우에는 완료된 트랜잭션의 결과만 포함된 검증된 ledger을 사용하고 싶을 수도 있습니다.

Client.request()를 사용하여 xrpl.js 2.0으로 API 요청을 할 때는 어떤 ledger을 사용할지 명시적으로 지정해야 합니다. 예를 들어, 가장 최근에 검증된 ledger을 사용하여 신뢰선을 조회하려면:

**ripple-lib 1.x:**

```javascript
const trustlines = await api.getTrustlines("rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn")
console.log(trustlines)
```

**xrpl.js 2.0:**

```javascript
const trustlines = await client.request({
  "command": "account_lines",
  "account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
  "ledger_index": "validated"
})
console.log(trustlines.result)
```

## 트랜잭션 제출

xrpl.js에는 트랜잭션에 서명하고 제출하고 XRP Ledger 블록체인이 해당 트랜잭션의 최종 결과를 확인할 때까지 기다리는 특정 도우미 함수가 있습니다:

* submitAndWait()를 사용하여 트랜잭션을 제출하고 최종 결과를 기다립니다. 트랜잭션이 유효성이 확인되면 tx 메소드 응답으로 해결되고, 그렇지 않으면 예외가 발생합니다. 예외가 발생한다고 해서 트랜잭션이 유효하지 않다는 보장은 없습니다. 예를 들어 서버에 ledger 공백이 있는 경우 트랜잭션이 그 공백에서 유효성이 검사되었을 수 있습니다.
* 제출하고 즉시 반환하려면 submit()를 사용합니다. 이렇게 하면 예비(최종) 결과를 보여주는 제출 메소드 응답으로 확인됩니다. 이 메소드는 트랜잭션을 XRP Ledger 서버로 전송하는 데 문제가 있는 경우에만 예외를 발생시킵니다.
* 두 방법 모두 서명된 트랜잭션을 메소드에 직접 전달하거나, 준비된 트랜잭션 지침과 지갑 인스턴스를 전달하여 제출하기 바로 전에 트랜잭션에 서명할 수 있습니다.

```javascript
const tx_json = await client.autofill({
  "TransactionType": "AccountSet",
  "Account": wallet.address, // "wallet" is an instance of the Wallet class
  "SetFlag": xrpl.AccountSetAsfFlags.asfRequireDest
})
try {
  const submit_result = await client.submitAndWait(tx_json, wallet)
  // submitAndWait() doesn't return until the transaction has a final result.
  // Raises XrplError if the transaction doesn't get confirmed by the network.
  // Does not handle disaster recovery.
  console.log("Transaction result:", submit_result)
} catch(err) {
  console.log("Error submitting transaction:", err)
}
```

또는 지갑의 서명 메소드를 사용해 트랜잭션에 서명한 다음 submitAndWait(tx\_blob)을 사용해 제출할 수 있습니다. 이는 정전 및 기타 재해로부터 복구할 수 있는 안정적인 트랜잭션 제출을 구축하는 데 유용할 수 있습니다. (라이브러리는 자체적으로 재해 복구를 처리하지 않습니다.)

## LastLedgerSequence 제어하기

ripple-lib 1.x에서는 트랜잭션을 준비할 때 instructions.maxLedgerVersionOffset을 지정하여 준비된 트랜잭션의 LastLedgerSequence 매개변수를 당시 가장 최근에 검증된 ledger 이후 몇 개의 ledger으로 정의할 수 있었습니다. 2.0에서는 트랜잭션을 자동 채우기 전에 가장 최근에 유효성이 검증된 ledger 인덱스를 조회한 다음 LastLedgerSequence를 명시적으로 지정하여 이 작업을 수행할 수 있습니다.

**xrpl.js 2.0:**

```javascript
const vli = await client.getLedgerIndex()

const prepared = await client.autofill({
  "TransactionType": "Payment",
  "Account": sender,
  "Amount": xrpl.xrpToDrops("50.2"),
  "Destination": "rPT1Sjq2YGrBMTttX4GZHjKu9dyfzbpAYe",
  "LastLedgerSequence": vli+75 // gives ~5min, rather than the default ~1min
})
```

이전 준비 메소드와 마찬가지로 Client.autofill()은 기본적으로 합리적인 LastLedgerSequence 값을 제공합니다. LastLedgerSequence 필드가 없는 트랜잭션을 준비하려면 null 값으로 LastLedgerSequence를 제공하세요:

```javascript
const prepared = await client.autofill({
  "TransactionType": "Payment",
  "Account": sender,
  "Amount": xrpl.xrpToDrops("50.2"),
  "Destination": "rPT1Sjq2YGrBMTttX4GZHjKu9dyfzbpAYe",
  "LastLedgerSequence": null // Transaction never expires
})
```

## 키와 지갑

xrpl.js 2.0은 암호화 키를 관리하고 트랜잭션에 서명하기 위한 새로운 Wallet 클래스를 도입했습니다. 이는 리플 라이브러리 1.x에서 시드 또는 비밀 값을 받던 함수를 대체하며, 다양한 주소 인코딩 및 생성 작업도 처리합니다.

## 키 생성

**ripple-lib 1.x:**

```javascript
const api = new RippleAPI()
const {address, secret} = api.generateAddress({algorithm: "ed25519"})
console.log(address, secret)
// rJvMQ3cwtyrNpVJDTW4pZzLnGeovHcdE6E s████████████████████████████
```

**xrpl.js 2.0:**

```javascript
const wallet = xrpl.Wallet.generate("ed25519")
console.log(wallet)
// Wallet {
//   publicKey: 'ED872A4099B61B0C187C6A27258F49B421AC384FBAD23F31330E666A5F50E0ED7E',
//   privateKey: 'ED224D2BDCF6382030C7612654D2118C5CEE16344C81CB36EC7A01EC7D95C5F737',
//   classicAddress: 'rMV3CPSXAdRpW96bvvnSu4zHTZ6ETBkQkd',
//   seed: 's████████████████████████████'
// }
```

## 시드에서 파생 및 서명

**ripple-lib 1.x:**

```javascript
const api = new RippleAPI()
const seed = 's████████████████████████████';
const keypair = api.deriveKeypair(seed)
const address = api.deriveAddress(keypair.publicKey)
const tx_json = {
  "Account": address,
  "TransactionType":"Payment",
  "Destination":"rPT1Sjq2YGrBMTttX4GZHjKu9dyfzbpAYe",
  "Amount":"13000000",
  "Flags":2147483648,
  "LastLedgerSequence":7835923,
  "Fee":"13",
  "Sequence":2
}
const signed = api.sign(JSON.stringify(tx_json), seed)
```

**xrpl.js 2.0:**

```javascript
const wallet = xrpl.Wallet.fromSeed('s████████████████████████████')
const tx_json = {
  "Account": wallet.address,
  "TransactionType":"Payment",
  "Destination":"rPT1Sjq2YGrBMTttX4GZHjKu9dyfzbpAYe",
  "Amount":"13000000",
  "Flags":2147483648,
  "LastLedgerSequence":7835923,
  "Fee":"13",
  "Sequence":2
}
const signed = wallet.sign(tx_json)
```

## 이벤트 및 구독

1.x에서는 RippleAPI 클래스의 .on() 메서드를 사용하여 ledger 이벤트와 API 오류를 구독하거나 .connection.on()을 사용하여 특정 웹소켓 메시지 유형을 구독할 수 있었습니다. 이 메서드들은 Client.on() 메서드로 병합되었습니다. 또한 클라이언트 라이브러리는 더 이상 XRP Ledger 서버에 연결할 때 ledger 마감 이벤트를 자동으로 구독하지 않습니다. Ledger 닫기 이벤트를 받으려면 여전히 핸들러를 추가해야 하지만, 원장 스트림에 명시적으로 구독해야 합니다.

Ledger 닫기 이벤트를 구독하려면 Client.request(method)를 사용해 "스트림"으로 구독 메소드를 호출하세요: \["ledger"]로 구독 메소드를 호출합니다. 이벤트 핸들러를 연결하려면 Client.on(event\_type, callback)을 사용합니다. 이러한 호출은 어느 순서로든 할 수 있습니다.

1.x에서 RippleAPI 전용 ledger 이벤트 유형이 제거되었으므로 대신 ledgerClosed 이벤트를 사용하세요. 이러한 이벤트 메시지는 동일한 데이터를 포함하지만 형식은 웹소켓 API의 레저 스트림 메시지와 일치합니다.

예시:

**ripple-lib 1.x:**

```javascript
api.on("ledger", (ledger) => {
  console.log(`Ledger #${ledger.ledgerVersion} closed!
    It contains ${ledger.transactionCount} transaction(s) and has
    the ledger_hash ${ledger.ledgerHash}.`
  )
})
// "ledger" events happen automatically while API is connected.
```

**xrpl.js 2.0:**

```javascript
client.on("ledgerClosed", (ledger) => {
  console.log(`Ledger #${ledger.ledger_index} closed!
    It contains ${ledger.txn_count} transaction(s) and has
    the ledger_hash ${ledger.ledger_hash}.`
  )
})
// Must explicitly subscribe to the "ledger" stream to get "ledgerClosed" events
client.request({
  "command": "subscribe",
  "streams": ["ledger"]
})
```

## 등가물 참조

ripple-lib 1.x에서는 모든 메소드와 프로퍼티가 RippleAPI 클래스의 인스턴스에 있었습니다. xrpl.js 2.x에서 일부 메소드는 라이브러리의 정적 메소드고 일부 메소드는 특정 클래스에 속합니다. 다음 표에서 Client.method() 표기법은 method()가 Client 클래스의 인스턴스에 속한다는 것을 의미합니다.

**참고: 다음 표에는 3개의 열이 있습니다. 모든 정보를 보려면 가로로 스크롤해야 할 수도 있습니다.**&#x20;

<table data-header-hidden><thead><tr><th></th><th width="204"></th><th></th></tr></thead><tbody><tr><td>RippleAPI 인스턴스 메소드/속성</td><td>xrpl.js 메소드/속성</td><td>노트</td></tr><tr><td><code>new ripple.RippleAPI({server: url})</code></td><td><a href="https://js.xrpl.org/classes/Client.html#constructor"><code>new xrpl.Client(url)</code> </a></td><td><code>xrpl.BroadcastClient([url1, url2, ..])</code>여러 서버에 연결하는 데 사용됩니다 .</td></tr><tr><td><code>request(command, options)</code></td><td><a href="https://js.xrpl.org/classes/Client.html#request"><code>Client.request(options)</code> </a></td><td><ul><li>명령 필드가 WebSocket API와의 일관성을 위해 옵션 객체로 이동했습니다. </li><li>1.x에서 이 메소드의 반환 값(프로미스가 해결될 때)은 결과 객체만 반환했습니다. 이제 이 메서드는 전체 WebSocket 응답 형식을 반환하며, 동등한 값을 얻으려면 반환 값의 결과 필드를 읽으면 됩니다.</li></ul></td></tr><tr><td><code>hasNextPage()</code></td><td><a href="https://js.xrpl.org/modules.html#hasNextPage"><code>xrpl.hasNextPage(response)</code> </a></td><td>참조: <a href="https://js.xrpl.org/classes/Client.html#requestNextPage"><code>Client.requestNextPage()</code> </a>및<a href="https://js.xrpl.org/classes/Client.html#requestAll"><code>Client.requestAll()</code> </a></td></tr><tr><td><code>requestNextPage()</code></td><td><a href="https://js.xrpl.org/classes/Client.html#requestNextPage"><code>Client.requestNextPage()</code> </a></td><td></td></tr><tr><td><code>computeBinaryTransactionHash()</code></td><td><a href="https://js.xrpl.org/modules.html#hashes"><code>xrpl.hashes.hashTx()</code> </a></td><td></td></tr><tr><td><code>classicAddressToXAddress()</code></td><td><a href="https://js.xrpl.org/modules.html#classicAddressToXAddress"><code>xrpl.classicAddressToXAddress()</code> </a></td><td>이제 모듈의 정적 메소드입니다.</td></tr><tr><td><code>xAddressToClassicAddress()</code></td><td><a href="https://js.xrpl.org/modules.html#xAddressToClassicAddress"><code>xrpl.xAddressToClassicAddress()</code> </a></td><td>이제 모듈의 정적 메소드입니다.</td></tr><tr><td><code>renameCounterpartyToIssuer(object)</code></td><td>(삭제됨 - 메모 열 참조)</td><td>xrpl.js는 항상 이미 발행자를 사용하기 때문에 더 이상 필요하지 않습니다.</td></tr><tr><td><code>formatBidsAndAsks()</code></td><td>(삭제됨 - 메모 열 참조)</td><td>getOrderbook()으로 변경된 후 더 이상 필요하지 않습니다.</td></tr><tr><td><code>connect()</code></td><td><a href="https://js.xrpl.org/classes/Client.html#connect"><code>Client.connect()</code> </a></td><td></td></tr><tr><td><code>disconnect()</code></td><td><a href="https://js.xrpl.org/classes/Client.html#disconnect"><code>Client.disconnect()</code> </a></td><td></td></tr><tr><td><code>isConnected()</code></td><td><a href="https://js.xrpl.org/classes/Client.html#isConnected"><code>Client.isConnected()</code> </a></td><td></td></tr><tr><td><code>getServerInfo()</code></td><td>(삭제됨 - 메모 열 참조)</td><td>대신 Client.request()를 사용하여 server_info 메서드를 호출하세요.</td></tr><tr><td><code>getFee()</code></td><td>(삭제됨 - 메모 열 참조)</td><td>Client.autofill()을 사용하여 합리적인 거래 비용을 자동으로 제공하거나 Client.request({"command":"fee"})를 사용하여 현재 거래 비용에 대한 정보를 조회할 수 있습니다(XRP 단위로 표시).</td></tr><tr><td><code>getLedgerVersion()</code></td><td><a href="https://js.xrpl.org/classes/Client.html#getLedgerIndex"><code>Client.getLedgerIndex()</code> </a></td><td></td></tr><tr><td><code>getTransaction()</code></td><td><a href="https://js.xrpl.org/classes/Client.html#request"><code>Client.request()</code> </a></td><td>Client.request()를 사용해 tx 메서드를 대신 호출하세요. 경고: tx 메서드는 getTransaction()과 달리 유효성이 검사되지 않은 최종 결과를 반환할 수 있습니다. 트랜잭션에 대한 응답으로 작업을 수행하기 전에 응답 객체에서 "validated": true를 확인해야 합니다.</td></tr><tr><td><code>getTransactions()</code></td><td>(삭제됨 - 메모 열 참조)</td><td>대신 Client.request()를 사용하여 account_tx 메서드를 호출하세요.</td></tr><tr><td><code>getTrustlines()</code></td><td>(삭제됨 - 메모 열 참조)</td><td>Client.request()를 사용하여 account_lines 메서드를 대신 호출하세요. 주의: getTrustlines()와 달리 account_lines는 유효성이 검사되지 않은 최종 결과를 반환할 수 있습니다.</td></tr><tr><td><code>getBalances()</code></td><td><a href="https://js.xrpl.org/classes/Client.html#getBalances"><code>Client.getBalances()</code> </a></td><td></td></tr><tr><td><code>getBalanceSheet()</code></td><td>(삭제됨 - 메모 열 참조)</td><td>대신 Client.getBalances()를 사용하거나 Client.request()를 사용하여 gateway_balances 메서드를 호출합니다.</td></tr><tr><td><code>getPaths()</code></td><td>(삭제됨 - 메모 열 참조)</td><td>Client.request()를 사용하여 ripple_path_find 메서드를 대신 호출합니다.</td></tr><tr><td><code>getOrders()</code></td><td>(삭제됨 - 메모 열 참조)</td><td>Client.request()를 사용하여 계정_오퍼 메서드를 대신 호출합니다.</td></tr><tr><td><code>getOrderbook()</code></td><td><a href="https://js.xrpl.org/classes/Client.html#getOrderbook"><code>Client.getOrderbook()</code> </a></td><td></td></tr><tr><td><code>getSettings()</code></td><td>(삭제됨 - 메모 열 참조)</td><td>account_info 메서드를 대신 호출합니다. 개별 플래그 설정의 부울 값을 가져오려면 플래그 필드에서 xrpl.parseAccountRootFlags()를 사용합니다. 경고: getSettings()와 달리 account_info는 유효성이 검사되지 않은 최종 결과를 반환할 수 있습니다.</td></tr><tr><td><code>getAccountInfo(address, options)</code></td><td>(삭제됨 - 메모 열 참조)</td><td>대신 Client.request()를 사용하여 account_info 메서드를 호출하세요. 경고: getAccountInfo()와 달리 account_info는 유효성이 검사되지 않은 최종 결과를 반환할 수 있습니다.</td></tr><tr><td><code>getAccountObjects(address, options)</code></td><td>(삭제됨 - 메모 열 참조)</td><td>대신 Client.request()를 사용하여 account_objects 메서드를 호출하세요. 경고: getAccountObjects()와 달리 account_objects는 유효성이 검사되지 않은 최종 결과를 반환할 수 있습니다.</td></tr><tr><td><code>getPaymentChannel()</code></td><td>(삭제됨 - 메모 열 참조)</td><td>대신 Client.request()를 사용하여 ledger_entry 메서드를 호출하세요. 주의: getPaymentChannel()과 달리 ledger_entry는 유효성이 검사되지 않은 최종 결과를 반환할 수 있습니다.</td></tr><tr><td><code>getLedger()</code></td><td>(삭제됨 - 메모 열 참조)</td><td>ledger 메서드를 정확하게 호출하세요. 경고: getLedger()와 달리 ledger는 유효성이 검사되지 않은 최종 원장을 반환할 수 있습니다.</td></tr><tr><td><code>parseAccountFlags()</code></td><td><a href="https://js.xrpl.org/modules.html#parseAccountRootFlags"><code>xrpl.parseAccountRootFlags()</code> </a></td><td>이제 모듈의 정적 메서드입니다.</td></tr><tr><td><code>prepareTransaction()</code></td><td><a href="https://js.xrpl.org/classes/Client.html#autofill"><code>Client.autofill()</code> </a></td><td>자세한 내용은 트랜잭션 제출을 참조하세요.</td></tr><tr><td><code>preparePayment()</code></td><td>(삭제됨 - 메모 열 참조)</td><td>결제 트랜잭션을 구성하고 대신 Client.autofill()을 사용합니다.</td></tr><tr><td><code>prepareTrustline()</code></td><td>(삭제됨 - 메모 열 참조)</td><td>트러스트셋 트랜잭션을 구성하고 대신 Client.autofill()을 사용합니다.</td></tr><tr><td><code>prepareOrder()</code></td><td>(삭제됨 - 메모 열 참조)</td><td><a href="https://xrpl.org/offercreate.html">OfferCreate </a>트랜잭션을 구성하고 대신 Client.autofill()을 사용합니다.</td></tr><tr><td><code>prepareOrderCancellation()</code></td><td>(삭제됨 - 메모 열 참조)</td><td><a href="https://xrpl.org/offercancel.html">OfferCancel </a>트랜잭션을 구성하고 대신 Client.autofill()을 사용합니다.</td></tr><tr><td><code>prepareSettings()</code></td><td>(삭제됨 - 메모 열 참조)</td><td>대부분의 설정의 경우 대신 AccountSet 트랜잭션을 구성합니다. 일반 키를 회전 변경하려면 SetRegularKey 트랜잭션을 구성하세요. 다중 서명 설정을 추가하거나 업데이트하려면 서명자 목록 설정 트랜잭션을 대신 구성하세요. 세 가지 경우 모두 Client.autofill()을 사용하여 트랜잭션을 준비합니다.</td></tr><tr><td><code>prepareEscrowCreation()</code></td><td>(삭제됨 - 메모 열 참조)</td><td>EscrowCreate 트랜잭션을 구성하고 대신 Client.autofill()을 사용합니다.</td></tr><tr><td><code>prepareEscrowCancellation()</code></td><td>(삭제됨 - 메모 열 참조)</td><td>EscrowCancel 트랜잭션을 구성하고 대신 Client.autofill()을 사용합니다.</td></tr><tr><td><code>prepareEscrowExecution()</code></td><td>(삭제됨 - 메모 열 참조)</td><td>EscrowFinish 트랜잭션을 구성하고 대신 Client.autofill()을 사용합니다.</td></tr><tr><td><code>preparePaymentChannelCreate()</code></td><td>(삭제됨 - 메모 열 참조)</td><td>PaymentChannelCreate 트랜잭션을 생성하고 대신 Client.autofill()을 사용합니다.</td></tr><tr><td><code>preparePaymentChannelClaim()</code></td><td>(삭제됨 - 메모 열 참조)</td><td>결제 채널 청구 트랜잭션을 생성하고 대신 Client.autofill()을 사용합니다.</td></tr><tr><td><code>preparePaymentChannelFund()</code></td><td>(삭제됨 - 메모 열 참조)</td><td>PaymentChannelFund 트랜잭션을 생성하고 대신 Client.autofill()을 사용합니다.</td></tr><tr><td><code>prepareCheckCreate()</code></td><td>(삭제됨 - 메모 열 참조)</td><td>CheckCreate 트랜잭션을 생성하고 대신 Client.autofill()을 사용합니다.</td></tr><tr><td><code>prepareCheckCancel()</code></td><td>(삭제됨 - 메모 열 참조)</td><td>CheckCancel 트랜잭션을 구성하고 대신 Client.autofill()을 사용합니다.</td></tr><tr><td><code>prepareCheckCash()</code></td><td>(삭제됨 - 메모 열 참조)</td><td>CheckCash 트랜잭션을 생성하고 대신 Client.autofill()을 사용합니다.</td></tr><tr><td><code>prepareTicketCreate()</code></td><td>(삭제됨 - 메모 열 참조)</td><td>TicketCreate 트랜잭션을 구성하고 대신 Client.autofill()을 사용합니다.</td></tr><tr><td><code>sign()</code></td><td><a href="https://js.xrpl.org/classes/Wallet.html#sign"><code>Wallet.sign()</code> </a></td><td>자세한 내용은 키와 지갑을 참조하세요.</td></tr><tr><td><code>combine()</code></td><td><a href="https://js.xrpl.org/modules.html#multisign"><code>xrpl.multisign()</code> </a></td><td></td></tr><tr><td><code>submit()</code></td><td><a href="https://js.xrpl.org/classes/Client.html#submit"><code>Client.submit()</code> </a></td><td>이제 안정적인 거래 제출도 가능합니다. 자세한 내용은 <a href="https://xrpl.org/xrpljs2-migration-guide.html#transaction-submission">거래 제출을</a> 참조하세요.</td></tr><tr><td><code>generateXAddress()</code></td><td><a href="https://js.xrpl.org/classes/Wallet.html#generate"><code>xrpl.Wallet.generate()</code> </a></td><td>xrpl.Wallet.generate()로 지갑 인스턴스를 생성한 다음 지갑 인스턴스에서 .getXAddress()를 호출하여 X-주소를 얻습니다. 자세한 내용은 키와 지갑을 참조하세요.</td></tr><tr><td><code>generateAddress()</code></td><td><a href="https://js.xrpl.org/classes/Wallet.html#generate"><code>xrpl.Wallet.generate()</code> </a></td><td>지갑 인스턴스를 생성합니다. 자세한 내용은 키와 지갑을 참조하십시오.</td></tr><tr><td><code>isValidAddress()</code></td><td><a href="https://js.xrpl.org/modules.html#isValidAddress"><code>xrpl.isValidAddress()</code> </a></td><td>이제 모듈의 정적 메서드입니다.</td></tr><tr><td><code>isValidSecret()</code></td><td><a href="https://js.xrpl.org/modules.html#isValidSecret"><code>xrpl.isValidSecret()</code> </a></td><td>이제 모듈의 정적 메서드입니다.</td></tr><tr><td><code>deriveKeypair()</code></td><td><a href="https://js.xrpl.org/modules.html#deriveKeypair"><code>xrpl.deriveKeypair()</code> </a></td><td>이제 모듈의 정적 메서드입니다.</td></tr><tr><td><code>deriveAddress()</code></td><td>(삭제됨 - 메모 열 참조)</td><td>공개 키에서 X-주소를 얻으려면 xrpl.decodeXAddress()를 사용한 다음, 필요한 경우 xAddressToClassicAddress()를 사용하여 클래식 주소를 가져옵니다.</td></tr><tr><td><code>generateFaucetWallet()</code></td><td><a href="https://js.xrpl.org/classes/Client.html#fundWallet"><code>Client.fundWallet()</code> </a></td><td>boolean이 제거되었으며, 라이브러리는 연결된 네트워크에 적합한 데브넷 또는 테스트넷 수도꼭지를 자동으로 선택합니다. 선택적으로 월렛 인스턴스를 제공하여 수도꼭지가 연결된 주소에 자금을 충전하거나 리필하도록 할 수 있으며, 그렇지 않으면 메서드가 새 월렛 인스턴스를 생성합니다. 이제 반환값은 {wallet} 형식의 객체로 해석됩니다:<code>{wallet: &#x3C;object: Wallet instance>, balance: &#x3C;str: drops of XRP>}</code></td></tr><tr><td><code>signPaymentChannelClaim()</code></td><td><a href="https://js.xrpl.org/modules.html#signPaymentChannelClaim"><code>xrpl.signPaymentChannelClaim()</code> </a></td><td>이제 모듈의 정적 메서드입니다.</td></tr><tr><td><code>verifyPaymentChannelClaim()</code></td><td><a href="https://js.xrpl.org/modules.html#verifyPaymentChannelClaim"><code>xrpl.verifyPaymentChannelClaim()</code> </a></td><td>이제 모듈의 정적 메서드입니다.</td></tr><tr><td><code>computeLedgerHash()</code></td><td><a href="https://js.xrpl.org/modules.html#hashes"><code>xrpl.hashes.hashLedger()</code> </a></td><td></td></tr><tr><td><code>xrpToDrops()</code></td><td><a href="https://js.xrpl.org/modules.html#xrpToDrops"><code>xrpl.xrpToDrops()</code> </a></td><td>이제 모듈의 정적 메서드입니다.</td></tr><tr><td><code>dropsToXrp()</code></td><td><a href="https://js.xrpl.org/modules.html#dropsToXrp"><code>xrpl.dropsToXrp()</code> </a></td><td>이제 모듈의 정적 메서드입니다.</td></tr><tr><td><code>iso8601ToRippleTime()</code></td><td><a href="https://js.xrpl.org/modules.html#isoTimeToRippleTime"><code>xrpl.isoTimeToRippleTime()</code> </a></td><td>이제 모듈의 정적 메서드입니다.</td></tr><tr><td><code>rippleTimeToISO8601()</code></td><td><a href="https://js.xrpl.org/modules.html#rippleTimeToISOTime"><code>xrpl.rippleTimeToISOTime()</code> </a></td><td>이제 모듈의 정적 메서드입니다. 또한 새로운 메서드 rippleTimeToUnixTime()을 사용하여 UNIX 시대인 1970-01-01 00:00:00 UTC 이후 밀리초 단위의 UNIX 스타일 타임스탬프를 가져올 수 있습니다.</td></tr><tr><td><code>txFlags.Universal.FullyCanonicalSig</code></td><td>(삭제됨 - 메모 열 참조)</td><td>RequireFullyCanonicalSig 개정에 따라 더 이상 필요하지 않습니다.</td></tr><tr><td><code>txFlags.Payment.NoRippleDirect</code></td><td><code>xrpl.PaymentFlags.tfNoDirectRipple</code></td><td></td></tr><tr><td><code>txFlags.Payment.PartialPayment</code></td><td><code>xrpl.PaymentFlags.tfPartialPayment</code></td><td></td></tr><tr><td><code>txFlags.Payment.LimitQuality</code></td><td><code>xrpl.PaymentFlags.tfLimitQuality</code></td><td></td></tr><tr><td><code>txFlags.OfferCreate.Passive</code></td><td><code>xrpl.OfferCreateFlags.tfPassive</code></td><td></td></tr><tr><td><code>txFlags.OfferCreate.ImmediateOrCancel</code></td><td><code>xrpl.OfferCreateFlags.tfImmediateOrCancel</code></td><td></td></tr><tr><td><code>txFlags.OfferCreate.FillOrKill</code></td><td><code>xrpl.OfferCreateFlags.tfFillOrKill</code></td><td></td></tr><tr><td><code>txFlags.OfferCreate.Sell</code></td><td><code>xrpl.OfferCreateFlags.tfSell</code></td><td></td></tr><tr><td><code>accountSetFlags</code></td><td><code>xrpl.AccountSetAsfFlags</code></td><td>이제 모듈 수준의 Enum입니다.</td></tr><tr><td><code>schemaValidator</code></td><td>(삭제됨 - 메모 열 참조)</td><td>대부분의 유형을 검증하려면 TypeScript를 사용하세요.</td></tr><tr><td><code>schemaValidate()</code></td><td>(삭제됨 - 메모 열 참조)</td><td>TypeScript를 사용하여 대부분의 유형의 유효성을 검사합니다. xrpl.validate(transaction)를 호출하여 트랜잭션 객체의 유효성을 검사할 수도 있습니다.</td></tr></tbody></table>
