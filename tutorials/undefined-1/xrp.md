# XRP 보내기

이 튜토리얼은 JavaScript를 위한 xrpl.js, Python을 위한 xrpl-py, 혹은 Java를 위한 xrpl4j를 사용하여 직접 XRP 결제를 보내는 방법에 대해 설명합니다. 먼저, XRP Ledger testnet에서 프로세스를 거칩니다. 그런 다음, 프로덕션에서 동일한 작업을 수행하기 위한 추가 요구사항에 대해 비교합니다.

{% hint style="info" %}
Tip:

이 튜토리얼에서 사용된 코드의 완전한 버전을 보려면 Code Samples을 확인해 보세요.
{% endhint %}

## 요구사항&#x20;

XRP Ledger와 상호작용하려면 필요한 도구가 설치된 개발 환경을 설정해야 합니다. 이 튜토리얼에서는 다음 옵션을 사용한 예제를 제공합니다:

1. JavaScript와 xrpl.js 라이브러리를 사용. 설정 단계는 Get Started Using JavaScript를 참조하세요.&#x20;
2. Python과 xrpl-py 라이브러리를 사용. 설정 단계는 Get Started using Python을 참조하세요.&#x20;
3. Java와 xrpl4j 라이브러리를 사용. 설정 단계는 Get Started Using Java를 참조하세요.&#x20;

## Testnet에서 결제 보내기

### 1. 자격 증명 받기&#x20;

XRP Ledger에서 거래를 수행하려면 주소와 비밀 키, 그리고 일부 XRP가 필요합니다. 주소와 비밀 키는 다음과 같습니다:

```javascript
// Example credentials 
const wallet = xrpl.Wallet.fromSeed("sn3nxiW7v8KXzPzAqzyHXbSSKNuN9") 
console.log(wallet.address) 
// rMCcNuTcajgw7YTgBy1sys3b89QqjUrMpH
```

여기에 보여진 비밀 키는 예시입니다. 개발 목적으로는, 다음 인터페이스를 사용하여 Testnet에서 XRP로 미리 충전된 자신만의 자격증명을 받을 수 있습니다:

{% hint style="info" %}
Caution:

Ripple은 테스트 목적으로만 Testnet과 Devnet을 제공하며, 때때로 이들 테스트 네트워크의 상태와 모든 잔액을 초기화합니다. 예방책으로, Testnet/Devnet과 Mainnet에서 동일한 주소를 사용하지 마세요.
{% endhint %}

당신이 프로덕션 준비 소프트웨어를 만들고 있다면, 기존 계정을 사용해야 하며, 안전한 서명 구성을 사용하여 키를 관리해야 합니다.

## 2. Testnet 서버에 연결하기&#x20;

먼저, XRP Ledger 서버에 연결해야 합니다. 이렇게 하면 계정의 현재 상태와 공유 ledger를 얻을 수 있습니다. 이 정보를 사용하여 트랜잭션의 일부 필요한 필드를 자동으로 채울 수 있습니다. 또한, 네트워크에 연결되어 있어야 트랜잭션을 제출할 수 있습니다.

다음 코드는 공용 Testnet 서버에 연결합니다:

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

## 3. 트랜잭션 준비하기&#x20;

일반적으로, 우리는 JSON 트랜잭션 형식의 객체로 XRP Ledger 트랜잭션을 생성합니다. 다음 예제는 최소한의 결제 사양을 보여줍니다:

```javascript
{
  "TransactionType": "Payment",
  "Account": "rPT1Sjq2YGrBMTttX4GZHjKu9dyfzbpAYe",
  "Amount": "2000000",
  "Destination": "rUCzEr6jrEyMpjhs4wSdQdz4g8Y382NxfM"
}
```

&#x20;XRP 결제에 반드시 제공해야 하는 최소한의 지시사항은 다음과 같습니다:

* 결제를 나타내는 지시자입니다. ("TransactionType": "Payment")&#x20;
* 송금 주소입니다. ("Account")&#x20;
* XRP를 받아야 하는 주소입니다 ("Destination"). 이는 송금 주소와 동일할 수 없습니다.&#x20;
* 송금할 XRP의 금액입니다 ("Amount"). 일반적으로, 이는 "drops" 단위의 XRP에서 정수로 지정됩니다, 여기서 1,000,000 drops는 1 XRP와 같습니다.&#x20;

기술적으로, 트랜잭션은 몇 가지 추가 필드를 포함해야 하며, LastLedgerSequence와 같은 일부 선택적 필드는 강력히 권장됩니다. 일부 언어별 참고사항은 다음과 같습니다:

* JavaScript의 xrpl.js를 사용하는 경우, Client.autofill() 메소드를 사용하여 트랜잭션의 나머지 필드에좋은 기본값을 자동으로 채울 수 있습니다. TypeScript에서는, xrpl.Payment와 같은 트랜잭션 모델을 사용하여 올바른 필드를 강제할 수 있습니다.&#x20;
* Python의 xrpl-py를 사용하는 경우, xrpl.models.transactions의 모델을 사용하여 트랜잭션을 원시 Python 객체로 구성할 수 있습니다.
* &#x20;Java의 xrpl4j를 사용하는 경우, xrpl4j-model 모듈의 모델 객체를 사용하여 트랜잭션을 Java 객체로 구성할 수 있습니다.&#x20;
  * 다른 라이브러리와 달리, 트랜잭션의 구성 시점에 소스 계정의 계정 시퀀스와 signingPublicKey를 제공해야 하며, 또한 수수료도 제공해야 합니다.&#x20;

다음은 위의 결제를 준비하는 예제입니다:

```javascript
// Prepare transaction -------------------------------------------------------
  const prepared = await client.autofill({
    "TransactionType": "Payment",
    "Account": wallet.address,
    "Amount": xrpl.xrpToDrops("22"),
    "Destination": "rPT1Sjq2YGrBMTttX4GZHjKu9dyfzbpAYe"
  })
  const max_ledger = prepared.LastLedgerSequence
  console.log("Prepared transaction instructions:", prepared)
  console.log("Transaction cost:", xrpl.dropsToXrp(prepared.Fee), "XRP")
  console.log("Transaction expires after ledger:", max_ledger)
```

## 4. 트랜잭션 지시사항에 서명하기&#x20;

트랜잭션에 서명하는 것은 당신의 자격을 이용해 트랜잭션을 승인하는 작업입니다. 이 단계의 입력값은 완성된 트랜잭션 지시사항 세트(일반적으로 JSON 형태)이며, 출력값은 지시사항과 보내는 사람의 서명이 포함된 이진 데이터들입니다.

* JavaScript: Wallet 인스턴스의 sign() 메소드를 사용하여 xrpl.js를 이용해 트랜잭션에 서명합니다.
* &#x20;Python: 모델과 Wallet 객체를 사용하여 xrpl.transaction.safe\_sign\_transaction() 메소드를 사용해 트랜잭션에 서명합니다.&#x20;
* Java: SignatureService 인스턴스를 사용해 트랜잭션에 서명합니다. 이 튜토리얼에서는 SingleKeySignatureService를 사용하세요.&#x20;

```javascript
// Sign prepared instructions ------------------------------------------------
  const signed = wallet.sign(prepared)
  console.log("Identifying hash:", signed.hash)
  console.log("Signed blob:", signed.tx_blob)
```

서명 작업의 결과는 서명이 포함된 트랜잭션 객체입니다. 일반적으로, XRP Ledger API들은 서명된 트랜잭션을 트랜잭션의 정규 이진 형식의 16진수 표현, 즉 "blob"으로 기대합니다.

* xrpl.js에서는, 서명 API가 트랜잭션의 ID, 즉 식별 해시도 반환하는데, 이를 이용해 나중에 트랜잭션을 검색할 수 있습니다. 이것은 이 트랜잭션에 고유한 64자리 16진수 문자열입니다.&#x20;
* xrpl-py에서는, 다음 단계에서 제출에 대한 응답에서 트랜잭션의 해시를 얻을 수 있습니다.&#x20;
* xrpl4j에서, SignatureService.sign은 SignedTransaction을 반환하는데, 이는 나중에 트랜잭션을 검색하는 데 사용할 수 있는 트랜잭션의 해시를 포함합니다.&#x20;

## 서명된 Blob 제출하기&#x20;

이제 서명된 트랜잭션이 생겼으니, 이를 XRP Ledger 서버에 제출할 수 있습니다. 이 서버는 트랜잭션을 네트워크를 통해 전달합니다. 또한 제출하기 전에 최근 검증된 ledger 인덱스를 확인하는 것이 좋습니다. 제출 결과로 당신의 트랜잭션이 포함될 수 있는 가장 이른바 ledger 버전은 제출 시점의 최근 검증된 ledger보다 하나 높습니다. 물론, 같은 트랜잭션이 이전에 제출되었다면, 그 트랜잭션은 이미 이전 ledger에 있을 수 있습니다. (두 번째에는 성공할 수 없지만, 올바른 ledger 버전을 확인하지 않는다면, 트랜잭션이 성공했음을 알지 못할 수 있습니다.)

* JavaScript: Client의 submitAndWait() 메소드를 사용하여 서명된 트랜잭션을 네트워크에 제출하고 응답을 기다리거나, submitSigned()를 사용하여 트랜잭션을 제출하고 초기 응답만 받을 수 있습니다.&#x20;
* Python: xrpl.transaction.submit\_and\_wait() 메소드를 사용하여 트랜잭션을 네트워크에 제출하고 응답을 기다립니다.&#x20;
* Java: XrplClient.submit(SignedTransaction) 메소드를 사용하여 트랜잭션을 네트워크에 제출합니다. XrplClient.ledger() 메소드를 사용하여 최근 검증된 ledger 인덱스를 얻습니다.&#x20;

```javascript
// Submit signed blob --------------------------------------------------------
  const tx = await client.submitAndWait(signed.tx_blob)
```

이 메소드는 트랜잭션을 오픈된 ledger에 적용해보려는 시도의 잠정적 결과를 반환합니다. 이 결과는 트랜잭션이 검증된 ledger에 포함될 때 변경될 수 있습니다: 처음에 성공한 트랜잭션은 최종적으로 실패할 수 있으며, 처음에 실패한 트랜잭션은 최종적으로 성공할 수 있습니다. 그러나 잠정적인 결과는 종종 최종 결과와 일치하므로, 여기에서 tesSUCCESS를 보게 되면 기쁘게 생각해도 좋습니다. 😁

다른 결과를 본다면, 다음 사항을 확인해 보세요:

* 발신자와 목적지의 주소가 올바른가요?&#x20;
* 트랜잭션의 다른 필드를 잊어버리거나, 어떤 단계를 건너뛰거나, 다른 오타가 있는가요?&#x20;
* 테스트 XRP를 충분히 보내기 위한 자금이 있는가요? XRP를 보낼 수 있는 양은 reserve requirement에 의해 제한되며, 현재 10 XRP이며, 당신이 ledger에 소유한 "객체"마다 추가 2 XRP가 필요합니다. (Testnet Faucet으로 새 주소를 생성했다면, 당신은 어떤 객체도 소유하지 않습니다.)&#x20;
* 테스트 네트워크의 서버에 연결되어 있나요?

더 많은 가능성을 보려면 트랜잭션 결과의 전체 목록을 확인하세요.

## 검증을 기다리기

제출된 대부분의 트랜잭션은 그 다음 버전의 ledger에 포함되므로, 트랜잭션 결과가 확정되기까지는 4-7초가 걸릴 수 있습니다. XRP Ledger가 바쁘거나 트랜잭션이 네트워크 전체로 중계되는 데 네트워크 연결이 느려지면 트랜잭션이 확인되는 데 더 오래 걸릴 수 있습니다. (미확인 트랜잭션의 만료에 대한 자세한 정보는 Reliable Transaction Submission를 참조하세요.)

* JavaScript: 만약 .submitAndWait() 메소드를 사용했다면, 반환된 Promise가 resolve될 때까지 기다릴 수 있습니다. 다른, 보다 비동기적인 접근법도 가능합니다.
* Python: xrpl.transaction.submit\_and\_wait() 메소드를 사용했다면, 함수가 반환될 때까지 기다릴 수 있습니다. WebSocket 클라이언트를 사용한 비동기 접근법 등 다른 접근법도 가능합니다.
* Java: XrplClient.transaction() 메소드를 폴링하여 트랜잭션에 최종 결과가 있는지 확인합니다. 트랜잭션의 LastLedgerIndex가 최근 검증된 ledger 인덱스를 넘지 않았는지 XrplClient.ledger() 메소드를 사용하여 주기적으로 확인합니다.

| Transaction ID:                     | (None)          |
| ----------------------------------- | --------------- |
| Latest Validated Ledger Index:      | (Not connected) |
| Ledger Index at Time of Submission: | (Not submitted) |
| Transaction `LastLedgerSequence`:   | (Not prepared)  |

## 7.  트랜잭션 상태 확인하기&#x20;

트랜잭션이 무엇을 했는지 정확히 알기 위해서는, 검증된 ledger 버전에 트랜잭션 결과를 조회해야 합니다.&#x20;

* JavaScript: submitAndWait()의 응답을 사용하거나 Client.request()를 사용하여 tx 메소드를 호출합니다.

{% hint style="info" %}
Tip:

TypeScript에서는 Client.request() 메소드에 TxRequest를 전달할 수 있습니다.
{% endhint %}

* Python: submit\_and\_wait()의 응답을 사용하거나 xrpl.transaction.get\_transaction\_from\_hash() 메소드를 호출합니다. (이 메소드가 포함할 수 있는 필드에 대한 자세한 참조는 tx 메소드 응답 형식을 참조하세요.)
* Java: 트랜잭션의 상태를 확인하기 위해 XrplClient.transaction() 메소드를 사용합니다.

```javascript
// Check transaction results -------------------------------------------------
  console.log("Transaction result:", tx.result.meta.TransactionResult)
  console.log("Balance changes:", JSON.stringify(xrpl.getBalanceChanges(tx.result.meta), null, 2))
```

{% hint style="info" %}
Caution:

XRP Ledger API는 아직 검증되지 않은 렛저 버전에서 잠정적인 결과를 반환할 수 있습니다. 예를 들어, tx 메소드 응답에서 "validated": true를 확인하여 데이터가 검증된 ledger 버전에서 온 것인지 확인하세요. 검증되지 않은 ledger 버전에서 나온 트랜잭션 결과는 변경될 수 있습니다. 결과의 확정에 대한 자세한 정보는 Finality of Results를 참조하세요.
{% endhint %}

## 생산을 위한 차이점&#x20;

실제 XRP Ledger에서 XRP 결제를 보내려면, 기본적으로 수행하는 단계는 대부분 동일합니다. 그러나 필요한 설정에는 몇 가지 주요 차이점이 있습니다:

* 실제 XRP를 얻는 것은 무료가 아닙니다.&#x20;
* 당신은 실제 XRP Ledger 네트워크와 동기화된 서버에 연결해야 합니다.

## 실제 XRP 계정 얻기&#x20;

이 튜토리얼에서는 Test Net XRP로 이미 충전된 주소를 얻기 위해 버튼을 사용하며, 이는 Test Net XRP가 가치가 없기 때문에 작동합니다. 실제 XRP를 얻으려면, 이미 일부를 가진 사람으로부터 XRP를 얻어야 합니다. (예를 들어, 교환을 통해 구입할 수 있습니다.) 다음과 같이 생산 또는 testnet에서 작동할 주소와 비밀을 생성할 수 있습니다:

```javascript
const wallet = new xrpl.Wallet()
console.log(wallet.address) // Example: rGCkuB7PBr5tNy68tPEABEtcdno4hE6Y7f
console.log(wallet.seed) // Example: sp6JS7f14BuwFY8Mw6bTtLKWauoUs
```

{% hint style="info" %}
Warning:

당신은 다른 컴퓨터가 주소와 비밀을 생성하고 네트워크를 통해 전송하지 않고, 당신이 로컬 머신에서 안전하게 생성한 주소와 비밀을 사용해야 합니다. 네트워크에 다른 사람이 그 정보를 볼 수 있을 수도 있다면, 그들은 당신만큼 XRP를 제어할 수 있습니다. 또한 testnet과 mainnet에서 같은 주소를 사용하지 않는 것이 좋습니다. 왜냐하면 당신이 제공한 파라미터에 따라 한 네트워크에서 사용하려고 생성한 트랜잭션이 다른 네트워크에서도 실행될 수 있기 때문입니다.
{% endhint %}

주소와 비밀을 생성하는 것은 XRP를 직접 얻는 것이 아닌, 단지 랜덤 숫자를 선택하는 것입니다. 또한 해당 주소에서 XRP를 받아야 계정을 충전할 수 있습니다. XRP를 얻는 일반적인 방법은 교환에서 구매한 후, 당신의 주소로 인출하는 것입니다.

## 실제 XRP Ledger에 연결하기&#x20;

당신이 클라이언트의 XRP Ledger에 연결을 초기화할 때, 해당 네트워크와 동기화된 서버를 지정해야 합니다. 많은 경우에, 다음 예시처럼 공개 서버를 사용할 수 있습니다:

```javascript
const xrpl = require('xrpl')
const api = new xrpl.Client('wss://xrplcluster.com')
api.connect()
```

만약 당신이 직접 rippled를 설치했다면, 기본적으로 생산 네트워크에 연결됩니다. (또한 테스트 넷에 연결하도록 설정할 수도 있습니다.) 서버가 동기화된 후 (일반적으로 시작한 후 약 15분 이내), 당신은 로컬로 그것에 연결할 수 있으며, 이는 여러 가지 이점이 있습니다. 다음 예시는 기본 설정을 실행하는 서버에 연결하는 방법을 보여줍니다:

```javascript
const xrpl = require('xrpl')
const api = new xrpl.Client('ws://localhost:6006')
api.connect()// Some code
```

{% hint style="info" %}
Tip:

로컬 연결은 암호화되지 않은 프로토콜(ws 또는 http)을 사용하며, 이는 TLS 암호화 버전(wss 또는 https) 대신 사용됩니다. 이는 통신이 동일한 기계를 벗어나지 않기 때문에 안전하며, TLS 인증서가 필요하지 않아 설정이 더 쉽습니다. 외부 네트워크에서의 연결은 항상 wss 또는 https를 사용하세요.
{% endhint %}

## 다음 단계&#x20;

이 튜토리얼을 완료한 후에는 다음과 같은 작업을 시도해 보실 수 있습니다:

* XRP Ledger testnet에서 토큰 발행하기.&#x20;
* 탈중앙화거래소에서 거래하기.&#x20;
* 생산 시스템용 신뢰성 있는 트랜잭션 제출을 구축하기.&#x20;
* 클라이언트 라이브러리의 API 참조를 확인하여 XRP Ledger 기능의 전체 범위를 확인하기.&#x20;
* 계정 설정을 사용자 지정하기.&#x20;
* 트랜잭션 메타데이터가 트랜잭션 결과를 자세히 설명하는 방법을 배우기.&#x20;
* Escrows와 Payment Channels와 같은 더 많은 Payment Types 탐색하기.&#x20;
* XRP Ledger 사업에 대한 모범 사례 읽기.
