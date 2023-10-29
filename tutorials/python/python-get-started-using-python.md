# Python으로 시작하기(Get Started Using Python)

이 튜토리얼에서는 순수 Python 라이브러리인 xrpl-py를 사용하여 XRP Ledger와 연결된 애플리케이션을 구축하는 기본 사항을 안내합니다.

이 튜토리얼은 초보자를 위한 것이며 완료하는 데 약 30분이 소요됩니다.

## 학습 목표(Learning Goals)

이 튜토리얼에서는 다음을 배우게 됩니다:

* XRP Ledger 기반 애플리케이션의 기본 구성 요소.
* xrpl-py를 사용하여 XRP Ledger에 연결하는 방법.
* xrpl-py를 사용하여 Testnet에 계정을 가져오는 방법.
* xrpl-py 라이브러리를 사용하여 XRP Ledger의 계정 정보를 조회하는 방법.
* 이러한 단계를 결합하여 파이썬 앱을 만드는 방법.&#x20;

## 요구 사항(Requirements)

xrpl-py 라이브러리는 Python 3.7 이상을 지원합니다.

## 설치(Installation)

xrpl-py 라이브러리는 PyPI에서 사용할 수 있습니다.&#x20;

pip로 설치하세요:

```python
pip3 install xrpl-py
```

## 개발 시작(Start Building)

XRP Ledger을 다룰 때 몇 가지를 관리해야 하며, 이는 계정에 XRP를 추가하든, 탈중앙화 거래소와 통합하든, 토큰을 발행하든 간에 마찬가지입니다. 이 튜토리얼은 이러한 모든 사용 사례로 시작하는 기본 패턴을 안내하며, 그것들을 구현하는 샘플 코드를 제공합니다.

여기에는 거의 모든 XRP Ledger 프로젝트에서 다루어야 할 기본 단계가 있습니다:

* XRP Ledger에 연결합니다.
* 계정을 가져옵니다.
* XRP Ledger을 쿼리합니다.

## 1. XRP Ledger에 연결 (Connect to the XRP Ledger)

쿼리를 하고 트랜잭션을 제출하려면 XRP Ledger에 연결해야 합니다. 이를 xrpl-py로 수행하려면 xrp.clients 모듈을 사용합니다:

```python
# Define the network client
from xrpl.clients import JsonRpcClient
JSON_RPC_URL = "https://s.altnet.rippletest.net:51234/"
client = JsonRpcClient(JSON_RPC_URL)
```

## 제작용 XRP Ledger에 연결(**Connect to the production XRP Ledger)**

이전 섹션의 샘플 코드는 돈이 실제 가치가 없는 테스트를 위한 병렬 네트워크인 Testnet에 연결하는 방법을 보여줍니다. 제작용 XRP Ledger과 통합할 준비가 되면 Mainnet에 연결해야 합니다. 두 가지 방법으로 할 수 있습니다:

* 핵심 서버(rippled)를 설치하고 노드를 직접 실행합니다. 핵심 서버는 기본적으로 Mainnet에 연결하지만, 구성을 변경하여 Testnet 또는 Devnet을 사용할 수 있습니다. 자체 핵심 서버를 실행하는 좋은 이유가 있습니다. 자체 서버를 실행하면 다음과 같이 연결할 수 있습니다:

```python
from xrpl.clients import JsonRpcClient
JSON_RPC_URL = "http://localhost:5005/"
client = JsonRpcClient(JSON_RPC_URL)
```

기본 값에 대한 자세한 정보는 예제 핵심 서버 구성 파일을 참조하십시오.

* 사용 가능한 퍼블릭 서버 중 하나를 사용하여:

```python
from xrpl.clients import JsonRpcClient
JSON_RPC_URL = "https://s2.ripple.com:51234/"
client = JsonRpcClient(JSON_RPC_URL)
```

## 2. 계정 가져오기&#x20;

XRP Ledger에서 가치를 저장하고 트랜잭션을 실행하려면 계정이 필요합니다. 계정은 키 세트와 계정 reserve를 충족시킬만큼 XRP로 충전된 주소입니다. 주소는 계정의 식별자이며, 개인 키를 사용하여 XRP Ledger에 제출하는 트랜잭션에 서명합니다.

테스트 및 개발 목적으로 XRP Faucets를 사용하여 키를 생성하고 Testnet 또는 Devnet에서 계정을 충전할 수 있습니다. 메인넷에서 키를 저장하고 안전한 서명 방법을 설정하는 것이 중요합니다. 메인넷에서 또 다른 차이점은 XRP가 실제 가치를 가지므로 faucet에서 무료로 얻을 수 없습니다.

Testnet에서 계정을 만들고 충전하려면, xrpl-py는 generate\_faucet\_wallet 메소드를 제공합니다:

```python
# Create a wallet using the testnet faucet:
# https://xrpl.org/xrp-testnet-faucet.html
from xrpl.wallet import generate_faucet_wallet
test_wallet = generate_faucet_wallet(client, debug=True)
```

이 메소드는 지갑 인스턴스를 반환합니다:

```python
print(test_wallet)

# print output
public_key:: 022FA613294CD13FFEA759D0185007DBE763331910509EF8F1635B4F84FA08AEE3
private_key:: -HIDDEN-
classic_address: raaFKKmgf6CRZttTVABeTcsqzRQ51bNR6Q
```

## 계정 사용&#x20;

이 튜토리얼에서는 XRP Ledger에서 생성된 계정에 대한 정보만을 쿼리합니다. 하지만 xrpl-py를 사용하면 Wallet 인스턴스의 값을 이용하여 트랜잭션을 준비하고, 서명하고, 제출할 수 있습니다.

### 준비하기&#x20;

트랜잭션을 준비하려면:

```python
# Prepare payment
from xrpl.models.transactions import Payment
from xrpl.utils import xrp_to_drops
my_tx_payment = Payment(
    account=test_account,
    amount=xrp_to_drops(22),
    destination="rPT1Sjq2YGrBMTttX4GZHjKu9dyfzbpAYe",
)
```

## 서명하고 제출하기&#x20;

트랜잭션을 서명하고 제출하려면:

```python
# Sign and submit the transaction
from xrpl.transaction import submit_and_wait

tx_response = submit_and_wait(my_tx_payment, client, test_wallet)
```

## X-주소 유도하기&#x20;

xrpl-py의 xrpl.core.addresscodec 모듈을 사용하여 Wallet.address 필드에서 X-주소를 유도할 수 있습니다.

```python
# Derive an x-address from the classic address:
# https://xrpaddress.info/
from xrpl.core import addresscodec
test_xaddress = addresscodec.classic_address_to_xaddress(test_account, tag=12345, is_test_network=True)
print("\nClassic address:\n\n", test_account)
print("X-address:\n\n", test_xaddress)
```

X-주소 형식은 주소와 데스티네이션 태그를 사용자 친화적인 값으로 패키징합니다.

## 3. XRP Ledger 쿼리하기&#x20;

특정 계정, 트랜잭션, 현재 또는 과거의 레저 상태, 그리고 XRP Ledger의 분산 거래에 대한 정보를 얻기 위해 XRP Ledger를 쿼리할 수 있습니다. 이러한 쿼리를 통해 신뢰할 수 있는 트랜잭션 제출을 위한 모범 사례를 따르기 위해 계정 정보를 찾아볼 필요가 있습니다.

여기에서는 이전 단계에서 얻은 계정에 대한 정보를 찾아보기 위해 xrpl-py의 xrpl.account 모듈을 사용합니다.

```python
pythonCopy code# 계정 정보 조회하기
from xrpl.models.requests.account_info import AccountInfo
acct_info = AccountInfo(
    account=test_account,
    ledger_index="validated",
    strict=True,
)
response = client.request(acct_info)
result = response.result
print("response.status: ", response.status)
import json
print(json.dumps(response.result, indent=4, sort_keys=True))
```

## 4. 모든 것을 함께 사용하기&#x20;

이러한 구성 요소들을 이용하면 testnet에서 계정을 얻고, XRP Ledger에 연결하고, 생성한 계정에 대한 정보를 조회하고 출력하는 파이썬 앱을 만들 수 있습니다.

이러한 구성 요소를 사용하여 다음과 같은 Python 앱을 만들 수 있습니다:

1. testnet에서 계정을 가져옵니다.&#x20;
2. XRP Ledger에 연결합니다.&#x20;
3. 생성한 계정에 대한 정보를 검색하고 인쇄합니다.

```python
# Define the network client
from xrpl.clients import JsonRpcClient
JSON_RPC_URL = "https://s.altnet.rippletest.net:51234/"
client = JsonRpcClient(JSON_RPC_URL)


# Create a wallet using the testnet faucet:
# https://xrpl.org/xrp-testnet-faucet.html
from xrpl.wallet import generate_faucet_wallet
test_wallet = generate_faucet_wallet(client, debug=True)

# Create an account str from the wallet
test_account = test_wallet.address

# Derive an x-address from the classic address:
# https://xrpaddress.info/
from xrpl.core import addresscodec
test_xaddress = addresscodec.classic_address_to_xaddress(test_account, tag=12345, is_test_network=True)
print("\nClassic address:\n\n", test_account)
print("X-address:\n\n", test_xaddress)


# Look up info about your account
from xrpl.models.requests.account_info import AccountInfo
acct_info = AccountInfo(
    account=test_account,
    ledger_index="validated",
    strict=True,
)
response = client.request(acct_info)
result = response.result
print("response.status: ", response.status)
import json
print(json.dumps(response.result, indent=4, sort_keys=True))
```

앱을 실행하려면 코드를 편집기 또는 IDE에 복사 붙여넣기한 후 거기서 실행할 수 있습니다. 또는 XRP Ledger Dev 포털 저장소에서 파일을 다운로드하여 로컬에서 실행할 수도 있습니다:

```bash
git clone git@github.com:XRPLF/xrpl-dev-portal.git
cd xrpl-dev-portal/content/_code-samples/get-started/py/get-acct-info.py
python3 get-acct-info.py
```

다음 예시와 유사한 출력을 볼 수 있어야 합니다:

```bash
Classic address:

 rnQLnSEA1YFMABnCMrkMWFKxnqW6sQ8EWk
X-address:

 T7dRN2ktZGYSTyEPWa9SyDevrwS5yDca4m7xfXTGM3bqff8
response.status:  ResponseStatus.SUCCESS
{
    "account_data": {
        "Account": "rnQLnSEA1YFMABnCMrkMWFKxnqW6sQ8EWk",
        "Balance": "1000000000",
        "Flags": 0,
        "LedgerEntryType": "AccountRoot",
        "OwnerCount": 0,
        "PreviousTxnID": "5A5203AFF41503539D11ADC41BE4185761C5B78B7ED382E6D001ADE83A59B8DC",
        "PreviousTxnLgrSeq": 16126889,
        "Sequence": 16126889,
        "index": "CAD0F7EF3AB91DA7A952E09D4AF62C943FC1EEE41BE926D632DDB34CAA2E0E8F"
    },
    "ledger_current_index": 16126890,
    "queue_data": {
        "txn_count": 0
    },
    "validated": false
}
```

## 응답 해석하기&#x20;

대부분의 경우 검토하려는 응답 필드는 다음과 같습니다:

* account\_data.Sequence - 이는 계정의 다음 유효한 트랜잭션의 시퀀스 번호입니다. 트랜잭션을 준비할 때 시퀀스 번호를 명시해야 합니다. xrpl-py를 사용하면, XRP Leger에서 이를 자동으로 얻을 수 있습니다. 이 사용법의 예시는 프로젝트 README에서 확인할 수 있습니다.
* account\_data.Balance - 이는 XRP의 계정 잔액으로, 드랍 단위로 표시됩니다. 이를 통해 충분한 XRP를 보유하고 있는지 (결제를 진행하는 경우), 특정 트랜잭션에 대한 현재 트랜잭션 비용을 충족시키는지 확인할 수 있습니다.
* validated - 반환된 데이터가 유효한 ledger에서 왔는지 여부를 나타냅니다. 트랜잭션을 검사할 때, 결과가 최종적인 것인지 확인하는 것이 중요합니다. validated가 true라면 결과가 변경되지 않는다는 것을 확실히 알 수 있습니다. 트랜잭션 제출의 신뢰성에 대한 자세한 정보는 Reliable Transaction Submission를 참조하세요.

모든 응답 필드에 대한 자세한 설명은 account\_info를 참조하세요.

## 계속 만들기&#x20;

이제 xrpl-py를 사용하여 XRP Ledger에 연결하고, 계정을 가져오고, 그 정보를 조회하는 방법을 알았으니, xrpl-py를 이용하여 다음 작업도 수행할 수 있습니다:

* XRP 보내기
* 계정에 대한 안전한 서명 설정하기
