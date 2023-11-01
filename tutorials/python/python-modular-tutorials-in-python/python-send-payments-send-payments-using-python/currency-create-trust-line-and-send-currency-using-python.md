# 신뢰 생성 및 Currency 전송 (Create Trust Line and Send Currency Using Python)

이 예제는 다음을 보여줍니다:

1. 타사 계정으로 자금을 전송할 수 있도록 계정을 구성합니다.
2. 거래를 위한 currency type을 설정합니다.
3. 대기 계정과 운영 계정 사이에 신뢰선을 생성합니다.
4. 계정 간에 발행된 currency를 전송합니다.
5. 모든 currency에 대한 계정 잔액을 표시합니다.

<figure><img src="https://xrpl.org/img/quickstart-py5.png" alt=""><figcaption></figcaption></figure>

[Quickstart Samples](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/quickstart/py) 아카이브를 다운로드하여 각 샘플을 브라우저에서 시도해볼 수 있습니다.

{% hint style="info" %}
Note:

Quickstart Samples가 없다면, 이후 예제들을 시도할 수 없습니다.
{% endhint %}

## 사용법&#x20;

Quickstart 창을 열고 다음 계정을 가져옵니다:

1. `lesson2-send-currency.py`을 열고 실행합니다.
2. 테스트 계정을 가져옵니다.
   1. 기존 계정 시드가 있는 경우
      1. 계정 시드를 **Seeds** 필드에 붙여넣습니다.
      2. **Get Accounts from Seeds**를 클릭합니다.
   2. 계정 시드가 없는 경우:
      1. **Get New Standby Account**를 클릭합니다.&#x20;
      2. **Get New Operational Account**를 클릭합니다.

## 신뢰선 생성

{% embed url="https://youtu.be/6KWP0PV6J8Y" %}

계정 간 신뢰선을 생성하려면:

1. **Currency** 필드에 [currency 코드](https://www.iban.com/currency-codes)를 입력합니다.
2. **Amount** 필드에 최대 전송 한도를 입력합니다.
3. **Destination** 필드에 목적지 계정 값을 입력합니다.
4. **Create 신뢰선**을 클릭합니다.

<figure><img src="https://xrpl.org/img/quickstart-py6.png" alt=""><figcaption></figcaption></figure>

## 발행된 Currency Token 보내기&#x20;

신뢰선을 생성한 후에 발행된 Currency Token을 전송하려면:

1. **Amount**를 입력합니다.
2. **Destination**을 입력합니다.
3. Currency 유형을 입력합니다.
4. **Send Currency**를 클릭합니다.&#x20;

<figure><img src="https://xrpl.org/img/quickstart-py7.png" alt=""><figcaption></figcaption></figure>

## 계정 구성

법정 화폐를 송금할 때, 실제 자금 이체는 XRP와 같이 동시에 이루어지지 않습니다. 화폐가 다른 화폐로 제3자에게 이전되는 경우, 원래 계정에 영향을 미치는 화폐의 평가절하가 발생할 수 있습니다. 이러한 상황을 피하기 위해 _rippling으_로 알려진 화폐의 이러한 상승 및 하강 평가는 기본적으로 허용되지 않습니다. 한 계좌에서 이체된 화폐는 동일한 계좌로만 다시 이체할 수 있습니다. 타사로의 화폐 전송을 활성화하려면 riffDefault 값을 true로 설정해야 합니다. 토큰 테스트 Harness에는 rippling을 활성화하거나 비활성화하는 확인란이 제공됩니다.

rippling 활성화 방법:

1. **Allow Rippling** 확인란을 선택합니다.
2. **Configure Account를 클**릭합니다.

응답에서 _Set Flag_ 값을 찾아 설정을 확인합니다. 플래그 설정은 _8_이어야 합니다.

<figure><img src="https://xrpl.org/img/quickstart-py8.png" alt=""><figcaption></figcaption></figure>

rippling 비활성화 방법:

1. **Allow Rippling** 확인란의 선택을 취소합니다.
2. **Configure Account를 클**릭합니다.

응답에서 _Clear Flag_ 값을 찾아 설정을 확인합니다. 플래그 설정은 _8_이어야 합니다.

<figure><img src="https://xrpl.org/img/quickstart-py9.png" alt=""><figcaption></figcaption></figure>

## Code Walkthrough <a href="#code-walkthrough" id="code-walkthrough"></a>

이 웹사이트 소스 저장소에서 [Quickstart Samples](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/quickstart/py)를 다운로드할 수 있습니다.

### mod2.py <a href="#mod2py" id="mod2py"></a>

모듈 2는 신뢰선을 생성하고 계정 간에 발행된 화폐 토큰을 전송하는 논리를 제공합니다.&#x20;

Dependencies을 가져오고 `testnet_url`을 설정합니다.

```python
import xrpl
from xrpl.clients import JsonRpcClient
from xrpl.wallet import Wallet

testnet_url = "https://s.altnet.rippletest.net:51234"
```

### Create 신뢰선 <a href="#create-trust-line-1" id="create-trust-line-1"></a>

지갑 seed, 발행자 계좌, 화폐 코드, 보낼 최대 화폐량을 전달합니다.

```python
def create_trust_line(seed, issuer, currency, amount):
    """create_trust_line"""
```

지갑과 새 클라이언트 인스턴스를 가져옵니다.

```python
    receiving_wallet = Wallet.from_seed(seed)
    client = JsonRpcClient(testnet_url)
```

`TrustSet` 트랜잭션을 정의합니다.

```python
    trustline_tx=xrpl.models.transactions.TrustSet(
        account=receiving_wallet.classic_address,
        limit_amount=xrpl.models.amounts.IssuedCurrencyAmount(
            currency=currency,
            issuer=issuer,
            value=int(amount)
        )
    )
```

트랜잭션을 XRP Ledger에 제출합니다.

```python
    response =  xrpl.transaction.submit_and_wait(trustline_tx,
        client, receiving_wallet)
```

결과를 반환합니다.

```python
    return response.result
```

### send\_currency <a href="#send_currency" id="send_currency"></a>

보낸 사람 지갑, 대상 계정, 화폐 유형 및 화폐 금액을 기준으로 다른 계정으로 화폐를 보냅니다.

```python
def send_currency(seed, destination, currency, amount):
    """send_currency"""
```

Testnet에서 송신 지갑 및 클라이언트 인스턴스를 가져옵니다.

```python
    sending_wallet=Wallet.from_seed(seed)
    client=JsonRpcClient(testnet_url)
```

결제 트랜잭션을 정의합니다. 금액은 화폐 유형과 발행자를 식별하기 위해 추가 설명이 필요합니다.

```python
    send_currency_tx=xrpl.models.transactions.Payment(
        account=sending_wallet.address,
        amount=xrpl.models.amounts.IssuedCurrencyAmount(
            currency=currency,
            value=int(amount),
            issuer=sending_wallet.address
        ),
        destination=destination
    )
```

트랜잭션을 제출하고 응답을 받습니다.

```python
    response=xrpl.transaction.submit_and_wait(send_currency_tx, client, sending_wallet)

```

JSON 응답을 반환하거나 트랜잭션이 실패할 경우 오류 메시지를 반환합니다.

```python
    return response.result
```

### get\_balance <a href="#get_balance" id="get_balance"></a>

**XRP Balance** 필드를 업데이트하고 **Results** 텍스트 영역에 발행 화폐에 대한 잔액 정보를 나열합니다.

```python
def get_balance(sb_account_id, op_account_id):
    """get_balance"""
```

XRP Ledger에 연결하고 클라이언트를 인스턴스화합니다.

```python
    client=JsonRpcClient(testnet_url)
```

`GatewayBalances` 요청을 만듭니다.

```python
    balance=xrpl.models.requests.GatewayBalances(
        account=sb_account_id,
        ledger_index="validated",
        hotwallet=[op_account_id]
    )
```

결과를 반환합니다.

```python
    response = client.request(balance)
    return response.result
```

### configure\_account <a href="#configure_account" id="configure_account"></a>

이 예에서는 `AccountSet` 메소드를 사용하여 구성 플래그를 설정하고 지우는 방법을 보여 줍니다. `ASF_DEFAULT_RIPPLE` 플래그는 발행 화폐를 타사 계정으로 전송하는 실험과 관련이 있으므로 여기에서 설명합니다. 설정할 특정 플래그를 대체하여 동일한 구조를 사용하여 구성 플래그를 설정할 수 있습니다. [AccountSet Flags](https://xrpl.org/accountset.html)을 참조하십시오.&#x20;

rippling을 사용할지 또는 사용하지 않을지 여부에 대한 계정 시드와 Boolean 값을 보냅니다.

```python
def configure_account(seed, default_setting):
    """configure_account"
```

계정 지갑을 가져오고 클라이언트를 인스턴스화합니다.

```python
    wallet=Wallet.from_seed(seed)
    client=JsonRpcClient(testnet_url)
```

`default_setting`이 참이면 `set_flag` 트랜잭션을 생성하여 rippling을 사용하도록 설정합니다. false인 경우 `clear_flag` 트랜잭션을 생성하여 rippling을 사용하지 않도록 설정합니다.

```python
    if (default_setting):
        setting_tx=xrpl.models.transactions.AccountSet(
            account=wallet.classic_address,
            set_flag=xrpl.models.transactions.AccountSetFlag.ASF_DEFAULT_RIPPLE
        )
    else:
        setting_tx=xrpl.models.transactions.AccountSet(
            account=wallet.classic_address,
            clear_flag=xrpl.models.transactions.AccountSetFlag.ASF_DEFAULT_RIPPLE
        )
```

트랜잭션을 제출하고 결과를 가져옵니다

```python
    response=xrpl.transaction.submit_and_wait(setting_tx,client,wallet)
    return response.result   
```

### lesson2-send-currency.py <a href="#lesson2-send-currencypy" id="lesson2-send-currencypy"></a>

This module builds on `lesson1-send-xrp.py`. Changes are noted below.

이 모듈은 `lesson1-send-xrp.py` 을 기반으로 합니다. 변경 사항은 아래에 나와 있습니다.

```python
import tkinter as tk
import xrpl
import json
```

`mod2.py` 에서 메소드를 가져옵니다.

```python
from mod1 import get_account, get_account_info, send_xrp
from mod2 import (
    create_trust_line,
    send_currency,
    get_balance,
    configure_account,
)
```

모듈 2 핸들러.

```python
def standby_create_trust_line():
    results = create_trust_line(ent_standby_seed.get(),
        ent_standby_destination.get(),
        ent_standby_currency.get(),
        ent_standby_amount.get())
    text_standby_results.delete("1.0", tk.END)
    text_standby_results.insert("1.0", json.dumps(results, indent=4))


def standby_send_currency():
    results = send_currency(ent_standby_seed.get(),
        ent_standby_destination.get(),
        ent_standby_currency.get(),
        ent_standby_amount.get())
    text_standby_results.delete("1.0", tk.END)
    text_standby_results.insert("1.0", json.dumps(results, indent=4))


def standby_configure_account():
    results = configure_account(
        ent_standby_seed.get(),
        standbyRippling)
    text_standby_results.delete("1.0", tk.END)
    text_standby_results.insert("1.0", json.dumps(results, indent=4))


def operational_create_trust_line():
    results = create_trust_line(ent_operational_seed.get(),
        ent_operational_destination.get(),
        ent_operational_currency.get(),
        ent_operational_amount.get())
    text_operational_results.delete("1.0", tk.END)
    text_operational_results.insert("1.0", json.dumps(results, indent=4))


def operational_send_currency():
    results = send_currency(ent_operational_seed.get(),
        ent_operational_destination.get(),
        ent_operational_currency.get(),
        ent_operational_amount.get())
    text_operational_results.delete("1.0", tk.END)
    text_operational_results.insert("1.0", json.dumps(results, indent=4))


def operational_configure_account():
    results = configure_account(
        ent_operational_seed.get(),
        operationalRippling)
    text_operational_results.delete("1.0", tk.END)
    text_operational_results.insert("1.0", json.dumps(results, indent=4))


def get_balances():
    results = get_balance(ent_operational_account.get(), ent_standby_account.get())
    text_standby_results.delete("1.0", tk.END)
    text_standby_results.insert("1.0", json.dumps(results, indent=4))
    results = get_balance(ent_standby_account.get(), ent_operational_account.get())
    text_operational_results.delete("1.0", tk.END)
    text_operational_results.insert("1.0", json.dumps(results, indent=4))


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


def standby_send_xrp():
    response = send_xrp(ent_standby_seed.get(),ent_standby_amount.get(),
                       ent_standby_destination.get())
    text_standby_results.delete("1.0", tk.END)
    text_standby_results.insert("1.0",json.dumps(response.result, indent=4))
    get_standby_account_info()
    get_operational_account_info()


def get_operational_account():
    new_wallet = get_account(ent_operational_seed.get())
    ent_operational_account.delete(0, tk.END)
    ent_operational_account.insert(0, new_wallet.classic_address)
    ent_operational_seed.delete(0, tk.END)
    ent_operational_seed.insert(0, new_wallet.seed)


def get_operational_account_info():
    accountInfo = get_account_info(ent_operational_account.get())
    ent_operational_balance.delete(0, tk.END)
    ent_operational_balance.insert(0,accountInfo['Balance'])
    text_operational_results.delete("1.0", tk.END)
    text_operational_results.insert("1.0",json.dumps(accountInfo, indent=4))


def operational_send_xrp():
    response = send_xrp(ent_operational_seed.get(),ent_operational_amount.get(), ent_operational_destination.get())
    text_operational_results.delete("1.0", tk.END)
    text_operational_results.insert("1.0",json.dumps(response.result,indent=4))
    get_standby_account_info()
    get_operational_account_info()

# Create a new window with the title "Quickstart Module 2"

window = tk.Tk()
window.title("Quickstart Module 2")


standbyRippling = tk.BooleanVar()
operationalRippling = tk.BooleanVar()


# Form frame
frm_form = tk.Frame(relief=tk.SUNKEN, borderwidth=3)
frm_form.pack()

# Create the Label and Entry widgets for "Standby Account"
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
```

**Currency** 필드를 추가합니다.

```python
lbl_standby_currency = tk.Label(master=frm_form, text="Currency")
ent_standby_currency = tk.Entry(master=frm_form, width=50)
```

**Allow Rippling**에 확인란을 추가합니다.

```python
cb_standby_allow_rippling = tk.Checkbutton(master=frm_form, text="Allow Rippling", variable=standbyRippling, onvalue=True, offvalue=False)
lbl_standby_results = tk.Label(master=frm_form,text='Results')
text_standby_results = tk.Text(master=frm_form, height = 20, width = 65)

# Place field in a grid.
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
```

새 UI 요소를 배치합니다.

```python
lbl_standby_currency.grid(row=6, column=0, sticky="e")
ent_standby_currency.grid(row=6, column=1)
cb_standby_allow_rippling.grid(row=7,column=1, sticky="w")
lbl_standby_results.grid(row=8, column=0, sticky="ne")
text_standby_results.grid(row=8, column=1, sticky="nw")
cb_standby_allow_rippling.select()

###############################################
## Operational Account ########################
###############################################

# Create the Label and Entry widgets for "Operational Account"
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
```

**Currency** 필드 및 **Allow Rippling** 확인란을 추가합니다.

```python
lbl_operational_currency = tk.Label(master=frm_form, text="Currency")
ent_operational_currency = tk.Entry(master=frm_form, width=50)
cb_operational_allow_rippling = tk.Checkbutton(master=frm_form, text="Allow Rippling", variable=operationalRippling, onvalue=True, offvalue=False)
lbl_operational_results = tk.Label(master=frm_form,text='Results')
text_operational_results = tk.Text(master=frm_form, height = 20, width = 65)

#Place the widgets in a grid
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
```

UI에 요소를 추가합니다.

```python
lbl_operational_currency.grid(row=6, column=4, sticky="e")
ent_operational_currency.grid(row=6, column=5)
cb_operational_allow_rippling.grid(row=7,column=5, sticky="w")
lbl_operational_results.grid(row=8, column=4, sticky="ne")
text_operational_results.grid(row=8, column=5, sticky="nw")
cb_operational_allow_rippling.select()
```

Standby Account 버튼을 만듭니다.

```python
btn_get_standby_account = tk.Button(master=frm_form, text="Get Standby Account",
                                    command = get_standby_account)
btn_get_standby_account.grid(row=0, column=2, sticky = "nsew")
btn_get_standby_account_info = tk.Button(master=frm_form,
                                         text="Get Standby Account Info",
                                         command = get_standby_account_info)
btn_get_standby_account_info.grid(row=1, column=2, sticky = "nsew")
btn_standby_send_xrp = tk.Button(master=frm_form, text="Send XRP >",
                                 command = standby_send_xrp)
btn_standby_send_xrp.grid(row=2, column = 2, sticky = "nsew")
```

**Create Trust Line**, **Send Currency**, **Get Balances**와 **Configure Account 버튼을 추가합니다.**

```python
btn_standby_create_trust_line = tk.Button(master=frm_form,
                                         text="Create Trust Line",
                                         command = standby_create_trust_line)
btn_standby_create_trust_line.grid(row=3, column=2, sticky = "nsew")
btn_standby_send_currency = tk.Button(master=frm_form, text="Send Currency >",
                                      command = standby_send_currency)
btn_standby_send_currency.grid(row=4, column=2, sticky = "nsew")
btn_standby_send_currency = tk.Button(master=frm_form, text="Get Balances",
                                      command = get_balances)
btn_standby_send_currency.grid(row=5, column=2, sticky = "nsew")
btn_standby_configure_account = tk.Button(master=frm_form,
                                          text="Configure Account",
                                          command = standby_configure_account)
btn_standby_configure_account.grid(row=7,column=0, sticky = "nsew")
```

Operational Account버튼 생성.

```python
btn_get_operational_account = tk.Button(master=frm_form,
                                        text="Get Operational Account",
                                        command = get_operational_account)
btn_get_operational_account.grid(row=0, column=3, sticky = "nsew")
btn_get_op_account_info = tk.Button(master=frm_form, text="Get Op Account Info",
                                    command = get_operational_account_info)
btn_get_op_account_info.grid(row=1, column=3, sticky = "nsew")
btn_op_send_xrp = tk.Button(master=frm_form, text="< Send XRP",
                            command = operational_send_xrp)
btn_op_send_xrp.grid(row=2, column = 3, sticky = "nsew")
```

**Create Trust Line**, **Send Currency**, **Get Balances와** **Configure Account의** operational account 버튼을 추가합니다.

```python
btn_op_create_trust_line = tk.Button(master=frm_form, text="Create Trust Line",
                                    command = operational_create_trust_line)
btn_op_create_trust_line.grid(row=3, column=3, sticky = "nsew")
btn_op_send_currency = tk.Button(master=frm_form, text="< Send Currency",
                                 command = operational_send_currency)
btn_op_send_currency.grid(row=4, column=3, sticky = "nsew")
btn_op_get_balances = tk.Button(master=frm_form, text="Get Balances",
                                command = get_balances)
btn_op_get_balances.grid(row=5, column=3, sticky = "nsew")
btn_op_configure_account = tk.Button(master=frm_form, text="Configure Account",
                                     command = operational_configure_account)
btn_op_configure_account.grid(row=7,column=4, sticky = "nsew")
```

## Start the application <a href="#start-the-application" id="start-the-application"></a>

```
window.mainloop()
```
