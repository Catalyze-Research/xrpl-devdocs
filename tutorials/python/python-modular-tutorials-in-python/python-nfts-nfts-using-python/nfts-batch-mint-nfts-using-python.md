# NFTs 일괄 발행 (Batch Mint NFTs Using Python)

한 번에 여러 개의 NFT를 채굴하는 애플리케이션을 생성하고, for 루프를 사용해 트랜잭션을 차례로 전송할 수 있습니다. 가장 좋은 방법은 티켓을 사용해 트랜잭션 시퀀스 번호를 예약하는 것입니다. 티켓을 사용하지 않고 NFT를 생성하는 애플리케이션을 만들 경우, 어떤 이유로든 트랜잭션이 실패하면 애플리케이션이 오류와 함께 중지됩니다. 티켓을 사용하는 경우 애플리케이션은 트랜잭션을 계속 전송하며, 나중에 개별 실패의 원인을 조사할 수 있습니다.

<figure><img src="../../../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

### 사용법(Usage) <a href="#usage" id="usage"></a>

[Quickstart Samples ](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/quickstart/py/)을 다운로드하거나 복제하여 자신의 브라우저에서 샘플을 사용해 볼 수 있습니다.

### 계정 가져오기(Get an Account) <a href="#get-an-account" id="get-an-account"></a>

1. `lesson7-batch-minting.py을 열고 실행합니다.`
2. 테스트 계정을 받습니다.
   1. 기존 계정 시드를 사용하려는 경우:
      1. **Standby Seed** 필드에 계정 시드를 붙여넣습니다.
      2. **Get Standby Account 클릭**
   2. 기존 계정 시드를 사용하지 않으려면 **Get Standby Account Info**를 클릭합니다.
3. **Get Standby Account Info**를 클릭하여 현재 XRP 잔액을 확인합니다.

### Batch Mint NFTs <a href="#batch-mint-nfts-1" id="batch-mint-nfts-1"></a>

{% embed url="https://youtu.be/NjEqEWcqhwc" %}

이 예시를 통해 하나의 고유 아이템에 대해 여러 개의 NFT를 발행할 수 있습니다. NFT는 오리지널 아트워크의 "프린트", 이벤트 티켓 또는 기타 한정된 고유 아이템 세트를 나타낼 수 있습니다.

To batch mint non-fungible token objects:

1. NFT URI를 입력합니다. 이는 NFT 객체와 관련된 데이터 또는 메타데이터를 가리키는 URI입니다. 고유한 URI가 없는 경우 이 샘플 URI를 사용할 수 있습니다: ipfs://bafybeigdyrzt5sfp7udm7hu76uh7y26nf4dfuylqabf3oclgtqy55fbzdi.
2. flage 필드를 설정합니다. 테스트 목적으로 값을 8로 설정하는 것이 좋습니다. 이렇게 하면 tsTransferable 플래그가 설정되어 NFT 객체를 다른 계정으로 전송할 수 있습니다. 그렇지 않으면 NFT 개체는 발행 계정으로만 다시 전송할 수 있습니다. 사용 가능한 NFT 발행 플래그는 NFTokenMint를 참조하세요.
3. Enter the **Transfer Fee**, a percentage of the proceeds that the original creator receives from future sales of the NFT. This is a value of 0-50000 inclusive, allowing transfer fees between 0.000% and 50.000% in increments of 0.001%. If you do not set the **Flags** field to allow the NFT to be transferrable, set this field to 0.
4. 값은 0-50000을 포함하여 0.000%에서 50.000% 사이의 전송 수수료를 0.001% 단위로 허용합니다. NFT를 양도할 수 있도록 플래그 필드를 설정하지 않은 경우 이 필드를 0으로 설정합니다.
5. Enter the **Taxon** for the NFT. If you do not have a need for the Taxon field, set this value to 0.
6. Enter an **NFT Count** of up to 200 NFTs to create in one batch.
7. Click **Batch Mint NFTs**.

### Get Batch NFTs <a href="#get-batch-nfts" id="get-batch-nfts"></a>

Click **Get Batch NFTs** to get the current list of NFTs for your account.

The difference between this function and the `getTokens()` function used earlier is that it allows for larger lists of tokens, sending multiple requests if the tokens exceed the number of objects allowed in a single request.

## Code Walkthrough <a href="#code-walkthrough" id="code-walkthrough"></a>

You can download or clone the [Quickstart Samples ](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/quickstart/py/) to try each of the samples locally.

Import dependencies and define the testnet\_url variable.

```python
import xrpl
from xrpl.clients import JsonRpcClient
from xrpl.wallet import Wallet
from xrpl.models.requests import AccountNFTs

testnet_url = "https://s.altnet.rippletest.net:51234"
```

### Batch Mint <a href="#batch-mint" id="batch-mint"></a>

Pass the values `seed`, `uri`, `flags`, `transfer_fee`, `taxon`, and `count`.

```python
def batch_mint(seed, uri, flags, transfer_fee, taxon, count):
    """batch_mint"""
```

Get the account wallet and a client instance.

```python
    wallet=Wallet.from_seed(seed)
    client=JsonRpcClient(testnet_url)
```

Request the full account info.

```python
    acct_info = xrpl.models.requests.account_info.AccountInfo(
        account=wallet.classic_address,
        ledger_index='validated'
    )
    get_seq_request = client.request(acct_info)
```

Parse the current sequence value.

```python
    current_sequence=get_seq_request.result['account_data']['Sequence']
```

Create a transaction to create tickets, starting at the current sequence number.

```python
    ticket_tx=xrpl.models.transactions.TicketCreate(
        account=wallet.address,
        ticket_count=int(count),
        sequence=current_sequence  
    )
```

Submit the ticket transaction and wait for the result.

```python
    response=xrpl.transaction.submit_and_wait(ticket_tx,client,wallet)
```

Create a request to obtain the next ticket number.

```python
    ticket_numbers_req=xrpl.models.requests.AccountObjects(
        account=wallet.address,
        type='ticket'
    )
    ticket_response=client.request(ticket_numbers_req)
```

Create the `tickets` variable to store an array of tickets.

```python
    tickets=[int(0)] * int(count)
    acct_objs= ticket_response.result['account_objects']
```

Create an array of ticket numbers.

```python
    for x in range(int(count)):
        tickets[x] = acct_objs[x]['TicketSequence']
```

Initialize variables to be used in the mint loop.

```python
    reply=""
    create_count=0
```

Use a `for` loop to send repeated mint requests, using the `ticket_sequence` field to uniquely identify the mint transactions.

```python
    for x in range(int(count)):
        mint_tx=xrpl.models.transactions.NFTokenMint(
            account=wallet.classic_address,
            uri=xrpl.utils.str_to_hex(uri),
            flags=int(flags),
            transfer_fee=int(transfer_fee),
            ticket_sequence=tickets[x],
            sequence=0,
            nftoken_taxon=int(taxon)
        )
```

Submit each transaction and report the results.

```python
        try:
            response=xrpl.transaction.submit_and_wait(mint_tx,client,
                wallet)
            create_count+=1 
        except xrpl.transaction.XRPLReliableSubmissionException as e:
            reply+=f"Submit failed: {e}\n"
    reply+=str(create_count)+' NFTs generated.'
    return reply
```

#### Get Batch <a href="#get-batch" id="get-batch"></a>

This version of `getTokens()` allows for a larger set of NFTs by watching for a `marker` at the end of each batch of NFTs. Subsequent requests get the next batch of NFTs starting at the previous marker, until all NFTs are retrieved.

```python
def get_batch(seed, account):
    """get_batch"""
```

Get a client instance. Since this is a request for publicly available information, no wallet is required.

```python
    client=JsonRpcClient(testnet_url)
```

Define the request for account NFTs. Set a return limit of 400 objects.

```python
    acct_nfts=AccountNFTs(
        account=account,
        limit=400
    )
```

Send the request.

```python
    response=client.request(acct_nfts)
```

Capture the result in the _responses_ variable.

```python
    responses=response.result
```

While there is a _marker_ value in the response, continue to define and send requests for 400 account NFTs at a time. Capture the _result_ from each request in the _responses_ variable.

```python
    while(acct_nfts.marker):
        acct_nfts=AccountNFTs(
            account=account,
            limit=400,
            marker=acct_nfts.marker
        )
        response=client.request(acct_nfts)
        responses+=response.result
```

Return the _responses_ variable.

```python
    return responses
```

### lesson7-batch-minting.py <a href="#lesson7-batch-mintingpy" id="lesson7-batch-mintingpy"></a>

This form is based on earlier examples, with unused fields, handlers, and buttons removed. Additions are highlighted below.

```python
import tkinter as tk
import xrpl
import json

from mod1 import get_account, get_account_info
```

Import dependencies from `mod7.py`.

```python
from mod7 import batch_mint, get_batch
```

Add Module 7 handlers.

```python
def batch_mint_nfts():
    results = batch_mint(
        ent_standby_seed.get(),
        ent_standby_uri.get(),
        ent_standby_flags.get(),
        ent_standby_transfer_fee.get(),
        ent_standby_taxon.get(),
        ent_standby_nft_count.get()
    )
    text_standby_results.delete("1.0", tk.END)
    text_standby_results.insert("1.0", json.dumps(results, indent=4))

def standby_get_batch_nfts():
    results = get_batch(
        ent_standby_seed.get(),
        ent_standby_account.get()
    )
    text_standby_results.delete("1.0", tk.END)
    text_standby_results.insert("1.0", json.dumps(results, indent=4))

# Module 1 Handlers
def get_standby_account():
    new_wallet = get_account(ent_standby_seed.get())
    ent_standby_account.delete(0, tk.END)
    ent_standby_seed.delete(0, tk.END)
    ent_standby_account.insert(0, new_wallet.classic_address)
    ent_standby_seed.insert(0, new_wallet.seed)

def get_standby_account_info():
    accountInfo = get_account_info(ent_standby_account.get())
    ent_standby_balance.delete(0, tk.END)
    ent_standby_balance.insert(0,accountInfo['Balance'])
    text_standby_results.delete("1.0", tk.END)
    text_standby_results.insert("1.0",json.dumps(accountInfo, indent=4))
```

Rename the window for Module 7.

```python
# Create a new window with the title "Python Module - Batch Minting"
window = tk.Tk()
window.title("Python Module - Batch Minting")



# Form frame
frm_form = tk.Frame(relief=tk.SUNKEN, borderwidth=3)
frm_form.pack()

# Create the Label and Entry widgets for "Standby Account"
lbl_standy_seed = tk.Label(master=frm_form, text="Standby Seed")
ent_standby_seed = tk.Entry(master=frm_form, width=50)
lbl_standby_account = tk.Label(master=frm_form, text="Standby Account")
ent_standby_account = tk.Entry(master=frm_form, width=50)
lbl_standby_balance = tk.Label(master=frm_form, text="XRP Balance")
ent_standby_balance = tk.Entry(master=frm_form, width=50)
lbl_standby_uri = tk.Label(master=frm_form, text="NFT URI")
ent_standby_uri = tk.Entry(master=frm_form, width=50)
lbl_standby_flags = tk.Label(master=frm_form, text="Flags")
ent_standby_flags = tk.Entry(master=frm_form, width=50)
lbl_standby_transfer_fee = tk.Label(master=frm_form, text="Transfer Fee")
ent_standby_transfer_fee = tk.Entry(master=frm_form, width="50")
lbl_standby_taxon = tk.Label(master=frm_form, text="Taxon")
ent_standby_taxon = tk.Entry(master=frm_form, width="50")
lbl_standby_nft_id = tk.Label(master=frm_form, text="NFT ID")
ent_standby_nft_id = tk.Entry(master=frm_form, width="50")
lbl_standby_nft_offer_index = tk.Label(master=frm_form, text="NFT Offer Index")
ent_standby_nft_offer_index = tk.Entry(master=frm_form, width="50")
```

Add the **NFT Count** field for batch minting.

```python
lbl_standby_nft_count=tk.Label(master=frm_form, text="NFT Count")
ent_standby_nft_count=tk.Entry(master=frm_form, width="50")
lbl_standby_results = tk.Label(master=frm_form,text="Results")
text_standby_results = tk.Text(master=frm_form, height = 20, width = 65)

# Place field in a grid.
lbl_standy_seed.grid(row=0, column=0, sticky="w")
ent_standby_seed.grid(row=0, column=1)
lbl_standby_account.grid(row=2, column=0, sticky="e")
ent_standby_account.grid(row=2, column=1)
lbl_standby_balance.grid(row=5, column=0, sticky="e")
ent_standby_balance.grid(row=5, column=1)
lbl_standby_uri.grid(row=8, column=0, sticky="e")
ent_standby_uri.grid(row=8, column=1, sticky="w")
lbl_standby_flags.grid(row=9, column=0, sticky="e")
ent_standby_flags.grid(row=9, column=1, sticky="w")
lbl_standby_transfer_fee.grid(row=10, column=0, sticky="e")
ent_standby_transfer_fee.grid(row=10, column=1, sticky="w")
lbl_standby_taxon.grid(row=11, column=0, sticky="e")
ent_standby_taxon.grid(row=11, column=1, sticky="w")
```

Place the **NFT Count** field in the grid.

```python
lbl_standby_nft_count.grid(row=13, column=0, sticky="e")
ent_standby_nft_count.grid(row=13, column=1, sticky="w")
lbl_standby_results.grid(row=14, column=0, sticky="ne")
text_standby_results.grid(row=14, column=1, sticky="nw")

#############################################
## Buttons ##################################
#############################################

# Create the Standby Account Buttons
btn_get_standby_account = tk.Button(master=frm_form, text="Get Standby Account",
                                    command = get_standby_account)
btn_get_standby_account.grid(row=0, column=2, sticky = "nsew")
btn_get_standby_account_info = tk.Button(master=frm_form,
                                         text="Get Standby Account Info",
                                         command = get_standby_account_info)
btn_get_standby_account_info.grid(row=1, column=2, sticky = "nsew")
```

Add the **Batch Mint NFTs** and **Get Batch NFTs** buttons.

```python
btn_standby_batch_mint = tk.Button(master=frm_form,
                                         text="Batch Mint NFTs",
                                         command = standby_batch_mint)
btn_standby_batch_mint.grid(row=5, column=2, sticky = "nsew")
btn_standby_get_batch_nfts = tk.Button(master=frm_form,
                                         text="Get Batch NFTs",
                                         command = standby_get_batch_nfts)
btn_standby_get_batch_nfts.grid(row=8, column=2, sticky = "nsew")
# Start the application
window.mainloop()
```

Was this page helpful?\
