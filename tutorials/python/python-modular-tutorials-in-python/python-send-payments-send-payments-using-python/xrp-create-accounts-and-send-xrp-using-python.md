# 계정 생성 및 XRP 전송(Create Accounts and Send XRP Using Python)

이 예제에서는 다음을 보여줍니다:

1. 실제 가치가 없는 1000개의 테스트 XRP로 펀딩된 Testnet의 계정을 생성합니다.
2. Seed 값에서 계정을 검색합니다.
3. 계정 간에 XRP를 전송합니다.

계정을 생성하면 오프라인에서 공개/비공개 키 쌍을 받습니다. 계정은 XRP로 펀딩될 때까지 ledger에 표시되지 않습니다. 이 예제는 Testnet을 위한 계정을 생성하는 방법을 보여주지만, Mainnet에서 사용할 수 있는 계정을 생성하는 방법은 보여주지 않습니다.

<figure><img src="https://xrpl.org/img/quickstart-py2.png" alt=""><figcaption></figcaption></figure>

## 요구 사항&#x20;

시작하려면 로컬 디스크에 새 폴더를 만들고 pip 사용하여 Python 라이브러리를 설치하세요.

```
    pip3 install xrpl-py
```

[Quickstart Samples](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/quickstart/py)를 다운로드 받고 펼쳐주세요.

{% hint style="info" %}
Note:

Quickstart Samples가 없다면, 이후 예제들을 시도할 수 없습니다.
{% endhint %}

## 사용법&#x20;

{% embed url="https://youtu.be/Uu36ga0iMv0" %}

테스트 계정을 얻으려면:

1. 브라우저에서 `lesson1-send-xrp.py`을 엽니다
2. **Get Standby Account**를 클릭합니다.
3. **Get Operational Account**를 클릭합니다.
4. **Get Standby Account Info**를 클릭합니다.
5. **Get Operational Account Info**를 클릭합니다.
6. **Standby Seed** 및 **Operational Seed** 필드를 메모장과 같은 영구 위치에 복사하여 붙여넣으면, 양식을 다시 로드한 후 계정을 다시 사용할 수 있습니다.

<figure><img src="https://xrpl.org/img/quickstart-py3.png" alt=""><figcaption></figcaption></figure>

당신의 새로운 계정 간에 XRP를 전송할 수 있습니다. 각 계정은 자신만의 필드와 버튼을 가지고 있습니다.

{% embed url="https://youtu.be/qUd-CTFdiks" %}

Standby account에서 Operational account로 XRP를 전송하려면:

1. 양식의 Standby(왼쪽) 부분에서 전송할 XRP의 **Amount**를 입력합니다.
2. **Operational Account** 필드를 Standby **Destination** 필드에 복사하고 붙여넣습니다.
3. **Send XRP>**를 클릭하여 standby 계정에서 operational 계정으로 XRP를 전송합니다.

<figure><img src="https://xrpl.org/img/quickstart-py4.png" alt=""><figcaption></figcaption></figure>

Operational account에서 Standby account로 XRP를 전송하려면:

1. 양식의 Operational(오른쪽) 부분에서 전송할 XRP의 **Amount**를 입력합니다.
2. **Standby Account** 필드를 Operational **Destination** 필드에 복사하고 붙여넣습니다.
3. **\<Send XRP**를 클릭하여 Operational 계정에서 Standby 계정으로 XRP를 전송합니다.

## Code Walkthrough <a href="#code-walkthrough" id="code-walkthrough"></a>

이 웹사이트 소스 저장소에서 [Quickstart Samples](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/quickstart/py)를 다운로드할 수 있습니다.

### mod1.py <a href="#mod1py" id="mod1py"></a>

mod1.py 모듈에는 XRP Ledger와 상호 작용하기 위한 비즈니스 로직이 포함되어 있습니다.&#x20;

XRPL 및 JSON 라이브러리와 모듈 1 메소드를 가져옵니다.

```python
import xrpl
import json
import xrpl.clients
import xrpl.wallet
```

### get\_account <a href="#get_account" id="get_account"></a>

이 방법을 사용하면 시드 값을 제공하여 기존 계정을 가져올 수 있습니다. 시드 값을 제공하지 않으면 메소드는 새 계정을 만듭니다.&#x20;

필수 메소드를 가져옵니다.

```python
def get_account(seed):
    """get_account"""
```

이 예에서는 _Testnet_ ledger를 사용합니다. URI를 업데이트하여 다른 XRP Ledger 인스턴스를 선택할 수 있습니다.

```python
    JSON_RPC_URL = "https://s.altnet.rippletest.net:51234/"
```

XRP Ledger에서 새 클라이언트를 요청합니다.

```python
    client = xrpl.clients.JsonRpcClient(JSON_RPC_URL)
```

시드를 입력하지 않으면 새 지갑을 생성하여 반환합니다. 시드 값을 제공하는 경우 해당 시드의 지갑을 반환합니다.

```python
    if (seed == ''):
        new_wallet = xrpl.wallet.generate_faucet_wallet(client)
    else:
        new_wallet = xrpl.wallet.Wallet(seed, sequence = 79396029)
    return(new_wallet)
```

### get\_account\_info <a href="#get_account_info" id="get_account_info"></a>

`get_account_info` 메소드에 계정 ID를 전달합니다.

```python
def get_account_info(accountId):
    """get_account_info"""
```

Testnet에서 클라이언트 인스턴스를 가져옵니다.

```python
    JSON_RPC_URL = 'wss://s.altnet.rippletest.net:51234'
    client = xrpl.clients.JsonRpcClient(JSON_RPC_URL)
```

계정 ID와 ledger 인덱스(이 경우 최신 유효성 확인 ledger)를 전달하여 계정 정보 요청을 생성합니다.

```python
    acct_info = xrpl.models.requests.account_info.AccountInfo(
        account=accountId,
        ledger_index="validated"
    )
```

요청을 XRP Ledger 인스턴스로 보냅니다.

```python
    response=client.request(acct_info)
```

계정 데이터를 반환합니다.

```python
    return response.result['account_data']
```

### send\_xrp <a href="#send_xrp" id="send_xrp"></a>

클라이언트 시드, 전송할 금액 및 대상 계정을 전달하여 XRP를 다른 계정으로 전송합니다.

```python
def send_xrp(seed, amount, destination):
```

보내는 지갑을 가져옵니다.

```python
    sending_wallet = xrpl.wallet.Wallet(seed, sequence = 16237283)
    testnet_url = "https://s.altnet.rippletest.net:51234"
    client = xrpl.clients.JsonRpcClient(testnet_url)
```

전송 계정, 금액 및 대상 계정을 전달하는 트랜잭션 요청을 만듭니다.

```python
    payment=xrpl.models.transactions.Payment(
        account=sending_wallet.classic_address,
        amount=xrpl.utils.xrp_to_drops(int(amount)),
        destination=destination,
    )
```

거래에 서명합니다.

```python
    signed_tx = xrpl.transaction.safe_sign_and_autofill_transaction(
        payment, sending_wallet, client)
```

트랜잭션을 제출하고 응답을 반환합니다. 트랜잭션이 실패하면 오류 메시지를 반환합니다.

```python
    try:
        tx_response = xrpl.transaction.send_reliable_submission(signed_tx,client)
        response = tx_response
    except xrpl.transaction.XRPLReliableSubmissionException as e:
        response = f"Submit failed: {e}"
    return response
```

### lesson1-send-xrp.py <a href="#lesson1-send-xrppy" id="lesson1-send-xrppy"></a>

이 모듈은 응용 프로그램의 UI를 처리하여 트랜잭션 및 요청 결과를 입력하고 보고하기 위한 필드를 제공합니다.&#x20;

tkinter, xrpl 및 json 모듈을 가져옵니다.

```python
import tkinter as tk
import xrpl
import json
```

mod1.py 에서 메소드를 가져옵니다.

```python
from .mod1 import get_account, get_account_info, send_xrp
```

#### getStandbyAccount <a href="#getstandbyaccount" id="getstandbyaccount"></a>

```python
def get_standby_account():
```

대기 시드 필드의 값(또는 빈 값)을 사용하여 새 계정을 요청합니다.

```python
    new_wallet = get_account(ent_standby_seed.get())
```

**Standby Seed** 및 **Standby Account** 필드를 지웁니다.

```python
    ent_standby_account.delete(0, tk.END)
    ent_standby_seed.delete(0, tk.END)
```

standby 필드에 계정 ID 및 시드 값을 입력합니다.

```python
    ent_standby_account.insert(0, new_wallet.classic_address)
    ent_standby_seed.insert(0, new_wallet.seed)
```

### get\_standby\_account\_info <a href="#get_standby_account_info" id="get_standby_account_info"></a>

계정 ID만 있으면 누구나 계정에 대한 정보를 요청할 수 있습니다. standby account 값을 가져와 `get_account_info` 요청을 채우는 데 사용합니다.

```python
def get_standby_account_info():
    accountInfo = get_account_info(ent_standby_account.get())
```

Standby **Balance** 필드를 지우고 계정 정보 응답에서 값을 삽입합니다.

```python
    ent_standby_balance.delete(0, tk.END)
    ent_standby_balance.insert(0,accountInfo['Balance'])
```

Standby **Results** 텍스트 영역을 지우고 전체 JSON 응답으로 채웁니다.

```python
    text_standby_results.delete("1.0", tk.END)
    text_standby_results.insert("1.0",json.dumps(accountInfo, indent=4))
```

### standby\_send\_xrp <a href="#standby_send_xrp" id="standby_send_xrp"></a>

```python
def standby_send_xrp():
```

`send_xrp` 메소드를 호출하여 standby 시드, 양 및 대상 값을 전달합니다.

```python
    response = send_xrp(ent_standby_seed.get(),ent_standby_amount.get(),
        ent_standby_destination.get())
```

Standby **Results** 필드의 선택을 취소하고 JSON 응답을 삽입합니다.

```python
    text_standby_results.delete("1.0", tk.END)
    text_standby_results.insert("1.0",json.dumps(response.result, indent=4))
```

`get_standby_account_info()` 및 `get_operational_account_info()`를 사용하여 두 계정의 잔액 필드를 업데이트합니다.

```python
    get_standby_account_info()
    get_operational_account_info()
```

### Reciprocal Transactions and Requests <a href="#reciprocal-transactions-and-requests" id="reciprocal-transactions-and-requests"></a>

다음 네 가지 방법은 operational account에 대해 이전 대기 트랜잭션과 동일합니다.

```python
def get_operational_account():
    new_wallet = get_account(ent_operational_seed.get())
    ent_operational_account.delete(0, tk.END)
    ent_operational_account.insert(0, new_wallet.classic_address)
    ent_operational_seed.delete(0, tk.END)
    ent_operational_seed.insert(0, new_wallet.seed)


def get_operational_account_info():
    account_info = get_account_info(ent_operational_account.get())
    ent_operational_balance.delete(0, tk.END)
    ent_operational_balance.insert(0,accountInfo['Balance'])
    text_operational_results.delete("1.0", tk.END)
    text_operational_results.insert("1.0",json.dumps(accountInfo, indent=4))


def operational_send_xrp():
    response = send_xrp(ent_operational_seed.get(),ent_operational_amount.get(),
                        ent_operational_destination.get())
    text_operational_results.delete("1.0", tk.END)
    text_operational_results.insert("1.0",json.dumps(response.result,indent=4))
    get_standby_account_info()
    get_operational_account_info()
```

기본 창부터 UI 요소를 만듭니다.

```python
window = tk.Tk()
window.title("Quickstart Module 1")
```

양식의 프레임을 추가합니다.

```python
frm_form = tk.Frame(relief=tk.SUNKEN, borderwidth=3)
frm_form.pack()
```

Standby account에 대한 `Label` 및 `Entry` 위젯을 만듭니다.

```python
lbl_standy_seed = tk.Label(master=frm_form, text="Standby Seed")
ent_standby_seed = tk.Entry(master=frm_form, width=50)
lbl_standby_account = tk.Label(master=frm_form, text="Standby Account")
ent_standby_account = tk.Entry(master=frm_form, width=50)
lbl_standy_amount = tk.Label(master=frm_form, text="Amount")
ent_standby_amount = tk.Entry(master=frm_form, width=50)
lbl_standby_destination = tk.Label(master=frm_form, text="Destination")
ent_standby_destination = tk.Entry(master=frm_form, width=50)
lbl_standby_balance = tk.Label(master=frm_form, text="XRP Balance")
ent_standby_balance = tk.Entry(master=frm_form, width=50)
lbl_standby_results = tk.Label(master=frm_form,text='Results')
text_standby_results = tk.Text(master=frm_form, height = 20, width = 65)
```

필드를 그리드에 배치합니다.

```python
lbl_standy_seed.grid(row=0, column=0, sticky="w")
ent_standby_seed.grid(row=0, column=1)
lbl_standby_account.grid(row=2, column=0, sticky="e")
ent_standby_account.grid(row=2, column=1)
lbl_standy_amount.grid(row=3, column=0, sticky="e")
ent_standby_amount.grid(row=3, column=1)
lbl_standby_destination.grid(row=4, column=0, sticky="e")
ent_standby_destination.grid(row=4, column=1)
lbl_standby_balance.grid(row=5, column=0, sticky="e")
ent_standby_balance.grid(row=5, column=1)
lbl_standby_results.grid(row=6, column=0, sticky="ne")
text_standby_results.grid(row=6, column=1, sticky="nw")
```

Operational account에 대한 `Label` 및 `Entry` 위젯을 만듭니다.

```python
lbl_operational_seed = tk.Label(master=frm_form, text="Operational Seed")
ent_operational_seed = tk.Entry(master=frm_form, width=50)
lbl_operational_account = tk.Label(master=frm_form, text="Operational Account")
ent_operational_account = tk.Entry(master=frm_form, width=50)
lbl_operational_amount = tk.Label(master=frm_form, text="Amount")
ent_operational_amount = tk.Entry(master=frm_form, width=50)
lbl_operational_destination = tk.Label(master=frm_form, text="Destination")
ent_operational_destination = tk.Entry(master=frm_form, width=50)
lbl_operational_balance = tk.Label(master=frm_form, text="XRP Balance")
ent_operational_balance = tk.Entry(master=frm_form, width=50)
lbl_operational_results = tk.Label(master=frm_form,text='Results')
text_operational_results = tk.Text(master=frm_form, height = 20, width = 65)
```

운영 위젯을 그리드에 배치합니다.

```python
lbl_operational_seed.grid(row=0, column=4, sticky="e")
ent_operational_seed.grid(row=0, column=5, sticky="w")
lbl_operational_account.grid(row=2,column=4, sticky="e")
ent_operational_account.grid(row=2,column=5, sticky="w")
lbl_operational_amount.grid(row=3, column=4, sticky="e")
ent_operational_amount.grid(row=3, column=5, sticky="w")
lbl_operational_destination.grid(row=4, column=4, sticky="e")
ent_operational_destination.grid(row=4, column=5, sticky="w")
lbl_operational_balance.grid(row=5, column=4, sticky="e")
ent_operational_balance.grid(row=5, column=5, sticky="w")
lbl_operational_results.grid(row=6, column=4, sticky="ne")
text_operational_results.grid(row=6, column=5, sticky="nw")
```

Standby account 버튼을 생성하여 그리드에 추가합니다.

```python
btn_get_standby_account = tk.Button(master=frm_form, text="Get Standby Account", command = get_standby_account)
btn_get_standby_account.grid(row=0, column=2, sticky = "nsew")
btn_get_standby_account_info = tk.Button(master=frm_form, text="Get Standby Account Info", command = get_standby_account_info)
btn_get_standby_account_info.grid(row=1, column=2, sticky = "nsew")
btn_standby_send_xrp = tk.Button(master=frm_form, text="Send XRP >", command = standby_send_xrp)
btn_standby_send_xrp.grid(row=2, column = 2, sticky = "nsew")
```

Operational account 버튼을 생성하여 그리드에 추가합니다.

```python
btn_get_operational_account = tk.Button(master=frm_form, text="Get Operational Account",
                                        command = get_operational_account)
btn_get_operational_account.grid(row=0, column=3, sticky = "nsew")
btn_get_op_account_info = tk.Button(master=frm_form, text="Get Op Account Info", 
                                    command = get_operational_account_info)
btn_get_op_account_info.grid(row=1, column=3, sticky = "nsew")
btn_op_send_xrp = tk.Button(master=frm_form, text="< Send XRP",
                            command = operational_send_xrp)
btn_op_send_xrp.grid(row=2, column = 3, sticky = "nsew")
```

응용 프로그램을 시작합니다.

```python
window.mainloop()
```
