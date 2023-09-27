# Java로 시작하기

이 튜토리얼에서는 XRP Ledger에 연결된 애플리케이션을 구축하는 기본 사항에 대해 안내하며, 이를 위해 XRP Ledger와 상호 작용하기 위해 만들어진 순수 Java 라이브러리인 xrpl4j를 사용합니다.

이 튜토리얼에서는 초보자를 대상으로 하며 완료하는데 30분 이상 걸리지 않아야 합니다.

## 학습 목표&#x20;

이 튜토리얼에서 다음을 배울 수 있습니다:

* XRP Ledger 기반 응용 프로그램의 기본 구성 요소.&#x20;
* xrpl4j를 사용하여 XRP Ledger에 연결하는 방법.&#x20;
* xrpl4j를 사용하여 Testnet에서 계정을 얻는 방법.
* XRP Ledger에 있는 계정에 대한 정보를 조회하는 데 xrpl4j 라이브러리를 사용하는 방법.&#x20;
* 이러한 단계를 모두 결합하여 Java 앱을 생성하는 방법.&#x20;

## 요구 사항&#x20;

* xrpl4j 라이브러리는 Java 1.8 이상을 지원합니다.&#x20;
* 프로젝트 관리 도구로는 Maven 또는 Gradle이 필요합니다.&#x20;

## 설치&#x20;

xrpl4j 라이브러리는 Maven Central에서 사용할 수 있습니다. xrpl4j는 필요에 따라 가져올 수 있는 여러 요소소로 나뉩니다.

이 자습서에서는 xrpl4j-client, xrpl4j-address-codec, xrpl4j-keypairs 및 xrpl4j-model 모듈이 필요합니다.&#x20;

Maven으로 설치하려면 프로젝트의 pom.xml 파일에 다음을 추가하고 mvn install을 실행하세요:

```
<dependencies>
    <dependency>
      <groupId>org.xrpl</groupId>
      <artifactId>xrpl4j-core</artifactId>
      <version>3.0.1</version>
    </dependency>
    <dependency>
      <groupId>org.xrpl</groupId>
      <artifactId>xrpl4j-client</artifactId>
      <version>3.0.1</version>
    </dependency>
</dependencies>
```

```
<dependencyManagement>
  <dependencies>
    <dependency>
      <groupId>org.xrpl</groupId>
      <artifactId>xrpl4j-bom</artifactId>
      <version>3.0.1</version>
      <type>pom</type>
      <scope>import</scope>
    </dependency>
  </dependencies>
</dependencyManagement>
```

이 튜토리얼에서의 코드를 포함하는 전체 Maven 프로젝트인 xrpl4j 샘플 프로젝트를 확인하세요.

## 개발 시작&#x20;

XRP Ledger에서 작업할 때는 계정에 XRP를 추가하거나, 탈중앙화 거래소와 통합하거나, 토큰을 발행하는 등의 작업을 관리해야 합니다. 이 자습서에서는 이러한 모든 사용 사례를 시작하는 데 공통적인 기본 패턴에 대해 안내하고 이를 구현하는 샘플 코드를 제공합니다.

거의 모든 XRP Ledger 프로젝트에서 다루어야 할 기본 단계는 다음과 같습니다:

1. XRP Ledger에 연결하기.&#x20;
2. 계정 얻기.&#x20;
3. XRP Ledger 조회하기.

## 1. XRP Ledger에 연결하기&#x20;

조회를 수행하고 트랜잭션을 제출하려면 XRP Ledger에 연결해야 합니다. xrpl4j를 사용하여 이를 수행하려면 XrplClient를 사용할 수 있습니다:

```
// Construct a network client
HttpUrl rippledUrl = HttpUrl.get("https://s.altnet.rippletest.net:51234/");
System.out.println("Constructing an XrplClient connected to " + rippledUrl);
XrplClient xrplClient = new XrplClient(rippledUrl);
```

## 제작용 XRP Ledger에 연결&#x20;

이전 섹션의 샘플 코드는 돈이 실제 가치가 없는 테스트를 위한 병렬 네트워크인 Testnet에 연결하는 방법을 보여줍니다. 제작용 XRP Ledger과 통합할 준비가 되면 Mainnet에 연결해야 합니다. 두 가지 방법으로 할 수 있습니다:

* 핵심 서버(rippled)를 설치하고 노드를 직접 실행합니다. 핵심 서버는 기본적으로 Mainnet에 연결하지만, 구성을 변경하여 Testnet 또는 Devnet을 사용할 수 있습니다. 자체 핵심 서버를 실행하는 좋은 이유가 있습니다. 자체 서버를 실행하면 다음과 같이 연결할 수 있습니다:

```
final HttpUrl rippledUrl = HttpUrl.get("http://localhost:5005/");
XrplClient xrplClient = new XrplClient(rippledUrl);
```

기본 값에 대한 자세한 정보는 예제 핵심 서버 구성 파일을 참조하십시오.

* 사용 가능한 공개 서버 중 하나를 사용하여:

```
final HttpUrl rippledUrl = HttpUrl.get("https://s2.ripple.com:51234/");
XrplClient xrplClient = new XrplClient(rippledUrl);
```

## 2. 계정 가져오기

XRP Ledger에서 값을 저장하고 트랜잭션을 실행하려면, 키 세트와 주소가 있는 계정이 필요합니다. 이 계정은 충분한 XRP로 자금이 지원되어야 합니다, 이를 계정 reserve라 합니다. 주소는 계정의 식별자이며, XRP Ledger에 제출하는 트랜잭션에 서명하기 위해 개인 키를 사용합니다. 제작용으로 사용한다면 키를 저장하고 안전한 서명 방법을 설정하는데 주의를 기울여야 합니다.

새 계정을 생성하려면, xrpl4j에서 DefaultWalletFactory를 제공합니다.

```java
// Create a random KeyPair
KeyPair randomKeyPair = Seed.ed25519Seed().deriveKeyPair();
System.out.println("Generated KeyPair: " + randomKeyPair);
```

walletFactory.randomWallet(true).wallet()을 호출하면 Wallet 인스턴스가 반환됩니다:

```java
System.out.println(testWallet);

// print output
Wallet {
    privateKey= -HIDDEN-,
    publicKey=ED90635A6F2A5905D3D5CD2C14905FFB2D838185993576CA4CEE24A920D0D6BD6B,
    classicAddress=raj5eirfEpbN9YzG9FzB8ZPNyjpFvH6ycV,
    xAddress=T76mQFr9zLGi2LCjVDgJ7mEQCk4767SdEL32mZFygpdGcFf,
    isTest=true
}
```

테스트와 개발 목적으로, XRP Ledger Testnet에 연결된 FaucetClient를 사용할 수 있습니다:

```java
// Fund the account using the testnet Faucet
FaucetClient faucetClient = FaucetClient.construct(HttpUrl.get("https://faucet.altnet.rippletest.net"));
faucetClient.fundAccount(FundAccountRequest.of(classicAddress));
System.out.println("Funded the account using the Testnet faucet.");
```

## XRP Ledger 쿼리

특정 계정, 특정 거래, 현재 또는 과거의 ledger 상태, 그리고 XRP Ledger의 분산형 거래소에 대한 정보를 얻기 위해 XRP Ledger를 쿼리할 수 있습니다. 이러한 쿼리를 실행하는 이유 중 하나는, 신뢰할 수 있는 거래 제출에 따른 최상의 방법을 준수하기 위해 계정 정보를 찾아보는 것입니다.

여기서는 이전 단계에서 얻은 계정 정보를 조회하기 위해 우리가 구성한 XrplClient를 사용하겠습니다.

```java
// Look up your Account Info
AccountInfoRequestParams requestParams = AccountInfoRequestParams.of(classicAddress);
AccountInfoResult accountInfoResult = xrplClient.accountInfo(requestParams);
```

## 4. 모든 것을 함께 사용하기&#x20;

이러한 구성 요소들을 사용하여, 우리는 다음과 같은 기능을 하는 Java 앱을 생성할 수 있습니다:

1. Testnet에서 계정을 가져옵니다.
2. XRP Ledger에 연결합니다.
3. 생성한 계정에 대한 정보를 조회하고 출력합니다.

```java
// Construct a network client
HttpUrl rippledUrl = HttpUrl.get("https://s.altnet.rippletest.net:51234/");
System.out.println("Constructing an XrplClient connected to " + rippledUrl);
XrplClient xrplClient = new XrplClient(rippledUrl);

// Create a random KeyPair
KeyPair randomKeyPair = Seed.ed25519Seed().deriveKeyPair();
System.out.println("Generated KeyPair: " + randomKeyPair);

// Derive the Classic and X-Addresses from testWallet
Address classicAddress = randomKeyPair.publicKey().deriveAddress();
XAddress xAddress = AddressCodec.getInstance().classicAddressToXAddress(classicAddress, true);
System.out.println("Classic Address: " + classicAddress);
System.out.println("X-Address: " + xAddress);

// Fund the account using the testnet Faucet
FaucetClient faucetClient = FaucetClient.construct(HttpUrl.get("https://faucet.altnet.rippletest.net"));
faucetClient.fundAccount(FundAccountRequest.of(classicAddress));
System.out.println("Funded the account using the Testnet faucet.");

// Look up your Account Info
AccountInfoRequestParams requestParams = AccountInfoRequestParams.of(classicAddress);
AccountInfoResult accountInfoResult = xrplClient.accountInfo(requestParams);

// Print the result
System.out.println(accountInfoResult);
```

앱을 실행하려면 Github에서 코드를 다운로드하고 IDE나 커맨드 라인에서 GetAccountInfo를 실행하면 됩니다.

```bash
git clone https://github.com/XRPLF/xrpl4j-sample.git
cd xrpl4j-sample
mvn compile exec:java -Dexec.mainClass="org.xrpl.xrpl4j.samples.GetAccountInfo"
```

이렇게 실행하면 아래와 비슷한 출력을 볼 수 있습니다:

```bash
Running the GetAccountInfo sample...
Constructing an XrplClient connected to https://s.altnet.rippletest.net:51234/
Generated KeyPair: KeyPair{
  privateKey=PrivateKey{value=[redacted], destroyed=false}, 
  publicKey=PublicKey{value=UnsignedByteArray{unsignedBytes=List(size=33)}, 
  base58Value=aKGgrZL2WTc85HJSkQGuKfinem5oMH1uCJankSWFATGUhqvygxir, 
  base16Value=EDFB1073327CCBDA342AD685AF1C04530294866B9CB10C21126DC004BFDBA287D1, 
  keyType=ED25519
  }
}
Classic Address: rBXHGshqXu3Smy9FUsQTmo49bGpQUQEm3X
X-Address: T7yMiiJJCmgY2yg5WB2davUedDeBFAG5B8r9KHjKCxDdvv3
Funded the account using the Testnet faucet.
AccountInfoResult{
  status=success, 
  accountData=AccountRootObject{
    ledgerEntryType=ACCOUNT_ROOT, 
    account=rDNwS2t4afhBogKqSFFmsDi1k7gmeGhz4p, 
    balance=10000000000, 
    flags=0, 
    ownerCount=0,
    previousTransactionId=0000000000000000000000000000000000000000000000000000000000000000, 
    previousTransactionLedgerSequence=0, 
    sequence=37649083, 
    signerLists=[],
    index=F607809578C2A413774B9A240480B8B7B10C3E296CA609337D2F41813F566B92
  }, 
  ledgerCurrentIndex=37649083, 
  validated=false
}
```

## 응답 해석하기&#x20;

대부분의 경우, AccountInfoResult에 포함된 응답 필드 중 주로 검사하고자 하는 것은 다음과 같습니다:

* accountData.sequence — 이것은 계정에 대한 다음 유효한 트랜잭션의 시퀀스 번호입니다. 트랜잭션을 준비할 때 이 시퀀스 번호를 명시해야 합니다.
* accountData.balance — 이것은 계정의 XRP 잔액으로, 드랍 단위로 표시됩니다. 이를 사용하여 (결제를 하려는 경우) 충분한 XRP를 보유하고 있는지, 그리고 주어진 트랜잭션에 대한 현재 거래 비용을 충족시키는지 확인할 수 있습니다.
* validated — 반환된 데이터가 유효한 렛저로부터 가져온 것인지를 나타냅니다. 트랜잭션을 검사할 때, 결과가 확정적인 것인지를 확인한 후에 트랜잭션을 추가로 처리하는 것이 중요합니다. validated가 true라면 결과가 변하지 않을 것임을 확실히 알 수 있습니다. 트랜잭션 처리에 대한 모범 사례에 대한 자세한 정보는 Reliable Transaction Submission을 참조하십시오.

모든 응답 필드에 대한 자세한 설명은 account\_info를 참조하십시오.

## 계속 만들기&#x20;

이제 xrpl-py를 사용하여 XRP Ledger에 연결하고, 계정을 가져오고, 그 정보를 조회하는 방법을 알았으니, xrpl-py를 이용하여 다음 작업도 수행할 수 있습니다:

* XRP 보내기
* 계정에 대한 안전한 서명 설정하기
