# 공인 발행인 지정 (Assign an Authorized Minter Using Python)

다른 계정에게 당신을 대신하여 NFT를 생성할 수 있는 권한을 부여할 수 있습니다.

다음 예제에서는 다음 작업을 수행하는 방법을 보여줍니다:

1. 다른 계정에게 당신의 계정을 위해 NFT를 생성할 수 있는 권한 부여하기.&#x20;
2. 권한이 부여된 경우, 다른 계정을 대신하여 NFT를 발행하기.&#x20;

<figure><img src="https://xrpl.org/img/quickstart-py30.png" alt=""><figcaption></figcaption></figure>

## 사용법 <a href="#usage" id="usage"></a>

[Quickstart Samples](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/quickstart/py) 아카이브를 다운로드하여 각 샘플을 브라우저에서 시도해볼 수 있습니다.

### Get Accounts <a href="#get-accounts" id="get-accounts"></a>

1. `py-authorize-minter.md`을 열고 실행합니다.
2. 계정을 가져옵니다.
   1. 기존 테스트 계정 시드가 있는 경우:
      1. **Standby Seed** field에시드를 붙여넣습니다.
      2. **Get Standby Account**를 클릭합니다.
      3. Click **Get Standby Account Info**를 클릭합니다.
      4. **Operational Seed** field에 시드를 붙여넣습니다.
      5. **Get Operational Account**를 클릭합니다.
      6. **Get Operational Account** info를 클릭합니다.
   2. 기존 테스트 계정이 없는 경우:
      1. **Get Standby Account**를 클릭합니다.
      2. **Get Standby Account Info**를 클릭합니다.
      3. **Get Operational Account**를 클릭합니다.
      4. **Get Operational Account Info**를 클릭합니다.

## 계정에 NFT 생성 권한 부여(Authorize an Account to Create NFTs)

다른 계정이 당신의 계정을 위해 NFT를 생성할 수 있도록 하려면:

1. **Operational Account** 값을 복사합니다.&#x20;
2. **Operational Account** 값을 **Authorized Minter** 필드에 붙여넣습니다.&#x20;
3. **Set Minter**을 클릭합니다.&#x20;

<figure><img src="https://xrpl.org/img/quickstart-py31.png" alt=""><figcaption></figcaption></figure>

## 다른 계정을 위해 NFT 생성하기(Mint an NFT for Another Account)

이 예제에서는 이전 단계에서 권한이 부여된 운영 계정을 사용하여 Standby account를 대신하여 토큰을 발행합니다.

다른 계정을 위해 non-fungible token을 발행하려면:

1. **플래그** 필드를 설정합니다. 테스트 목적으로는 값이 _8_로 설정하는 것을 권장합니다.&#x20;
2. **NFT URL**을 입력합니다. 이는 NFT 개체와 관련된 데이터 또는 메타데이터를 가리키는 URI입니다. 직접 URL을 소유하고 있지 않은 경우 샘플 URI를 사용할 수 있습니다.&#x20;
3. 이전 판매에서 원작자가 향후 판매에서 수익의 일부를 받는 이체 수수료를 입력합니다. 0에서 50000 사이의 값을 입력할 수 있으며, 0부터 0.001% 단위로 50.000%까지의 이체 비율을 허용합니다. NFT가 이전 가능하도록 하기 위해 Flags 필드를 설정하지 않았다면, 이 필드를 0으로 설정합니다.&#x20;
4. NFT의 **Taxon**을 입력합니다. 필드를 사용하지 않는 경우 필드를 0으로 설정합니다.
5. **Standby Account** value을 복사합니다
6. Operational account **Issuer** 필드에 **Standby Account** 값을 붙여넣습니다.
7. Operational account **Mint Other** button을 클릭합니다.

<figure><img src="https://xrpl.org/img/quickstart-py32.png" alt=""><figcaption></figcaption></figure>

아이템이 발행되면 권한이 부여된 Minter는 일반적인 방식으로 NFT를 판매할 수 있습니다. 판매 수익은 이체 수수료를 제외한 권한이 부여된 Minter에게 지급됩니다. 발행자와 Minter는 가격 분배에 대해 별도로 협의할 수 있습니다.

## 판매 오더 생성하기(Create a Sell Offer)

NFT 판매 오더를 생성하려면:

1. Operational account 측에서 제안 **Amount**를 드롭 (XRP의 백만 분의 1)으로 입력합니다. 예를 들어, 100000000 (100 XRP)를 입력합니다.&#x20;
2. **Flags** 필드를 1로 설정합니다.&#x20;
3. 판매할 NFT의 **NFT ID**를 입력합니다.&#x20;
4. 선택적으로 **Expiration**까지 남은 일 수를 입력합니다.&#x20;
5. **Create Sell Offer**을 클릭합니다.&#x20;

응답에서 중요한 정보는 NFT Offer Index입니다. 이 인덱스는 sell offer를 수락하는 데 사용되며, `nft_offer_index`라고 표시됩니다.

<figure><img src="https://xrpl.org/img/quickstart-py33.png" alt=""><figcaption></figcaption></figure>

## Sell Offer 수락하기(Accept Sell Offer)

Sell Offer가 있는 경우, 새로운 계정을 생성하여 해당 offer를 수락하고 NFT를 구매할 수 있습니다.

사용 가능한 Sell Offer를 수락하려면:

1. **Standby Seed** field를 지웁니다.
2. **Get Standby Account**를 클릭합니다.&#x20;
3. **Get Standby Account Info**를 클릭합니다.
4. **NFT Offer Index**(NFT 오퍼 결과에서 `nft_offer_index`로 레이블이 지정됨을 입력합니다. 이는 nft\_id와 다릅니다).
5. **Accept Sell Offer**을 클릭합니다.

<figure><img src="https://xrpl.org/img/quickstart-py34.png" alt=""><figcaption></figcaption></figure>

구매자 계정은 거래 비용으로 100 XRP 가격에 10개의 하락을 더한 금액으로 인출되었습니다. 판매자(공인 발행인) 계정은 90 XRP로 인정됩니다. 발행자와 판매자는 별도의 거래에서 계약에 따라 수익을 나눌 수 있습니다. 원래 대기 계정은 10 XRP의 전송 수수료를 받습니다.

<figure><img src="https://xrpl.org/img/quickstart-py35.png" alt=""><figcaption></figcaption></figure>

## Code Walkthrough <a href="#code-walkthrough" id="code-walkthrough"></a>

이 웹사이트 소스 저장소에서 [Quickstart Samples](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/quickstart/py)를 다운로드할 수 있습니다.

### mod6.py <a href="#mod6py" id="mod6py"></a>

`mod6.py`에는 이 예제에 대한 새로운 비즈니스 로직인 `set_minter` 및 `mint_other` 메소소드가 포함되어 있습니다.

dependencies을 가져오고 `testnet_url` 글로벌 변수를 정의합니다.

```python
import xrpl 
import json
from xrpl.clients import JsonRpcClient
from xrpl.wallet import Wallet
testnet_url = "https://s.altnet.rippletest.net:51234"
```

### Set Minter <a href="#set-minter" id="set-minter"></a>

이 함수는 계정에 대한 권한 있는 관리자를 설정합니다. 각 계정에는 NFT를 대신 작성할 수 있는 권한이 있는 관리자가 0개 또는 1개 있을 수 있습니다.

권한을 부여하는 계정의 지갑을 가져오고 클라이언트를 인스턴스화합니다.

```python
def set_minter(seed, minter):
    """set_minter"""
    granter_wallet=Wallet(seed, sequence = 16237283)
    client=JsonRpcClient(testnet_url)
```

다른 계정에 부여자 계정을 대신하여 토큰을 민트할 수 있는 권한을 부여하는 AccountSet 트랜잭션을 정의합니다.

```python
    set_minter_tx=xrpl.models.transactions.AccountSet(
        account=granter_wallet.classic_address,
        nftoken_minter=minter,
        set_flag=xrpl.models.transactions.AccountSetFlag.ASF_AUTHORIZED_NFTOKEN_MINTER
    )    
```

트랜잭션에 서명합니다.

```python
    signed_tx=xrpl.transaction.safe_sign_and_autofill_transaction(
        set_minter_tx, granter_wallet, client)
```

트랜잭션을 제출하고 결과를 반환합니다.

```python
    reply=""
    try:
        response=xrpl.transaction.send_reliable_submission(signed_tx,client)
        reply=response.result
    except xrpl.transaction.XRPLReliableSubmissionException as e:
        reply=f"Submit failed: {e}"
    return reply
```

### mint\_other <a href="#mint_other" id="mint_other"></a>

Minter wallet을 가져와 XRP Ledger에 대한 클라이언트 연결을 인스턴스화합니다.

```python
def mint_other(seed, uri, flags, transfer_fee, taxon, issuer):
    """mint_other"""
    minter_wallet=Wallet(seed, sequence=16237283)
    client=JsonRpcClient(testnet_url)
```

`NFTokenMint`트랜잭션을 정의합니다. 이 메소드의 새 매개 변수는 토큰이 만들어지는 계정을 식별하는 _issuer_ 필드입니다.

```python
    mint_other_tx=xrpl.models.transactions.NFTokenMint(
        account=minter_wallet.classic_address,
        uri=xrpl.utils.str_to_hex(uri),
        flags=int(flags),
        transfer_fee=int(transfer_fee),
        nftoken_taxon=int(taxon),
        issuer=issuer
    )
```

트랜잭션에 서명하고 작성합니다.

```python
    signed_tx=xrpl.transaction.safe_sign_and_autofill_transaction(
        mint_other_tx, minter_wallet, client)
```

트랜잭션을 제출하고 결과를 보고합니다.

```python
    reply=""
    try:
        response=xrpl.transaction.send_reliable_submission(signed_tx,client)
        reply=response.result
    except xrpl.transaction.XRPLReliableSubmissionException as e:
        reply=f"Submit failed: {e}"
    return reply
```
