# 시간 보류 에스크로 생성(Create Time-based Escrows Using Python)

이 예시를 통해 아래와 같은 방법을 알 수 있습니다.

1. 지정된 시간에 사용할 수 있고 지정된 시간에 만료되는 에스크로 결제를 생성합니다.
2. 에스크로 결제를 완료합니다.
3. 계정에 연결된 에스크로에 대한 정보를 검색합니다.
4. 에스크로 결제를 취소하고 송금 계좌로 XRP를 반환합니다.

<figure><img src="../../../../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>



## 요구사항

&#x20;[Python Modular Code Samples ](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/quickstart/py/)으세요.

To get test accounts:





1.  테스트 계정을 받으려면:

    lesson8-time-escrow.py를 엽니다. 테스트 계정을 받습니다. 기존 계정 시드가 있는 경우 스탠바이 시드 필드에 스탠바이 계정 시드를 붙여넣습니다. 대기 계정 가져오기를 클릭합니다. 대기 계정 정보 가져오기를 클릭합니다. 운영 계정 시드를 운영 시드 필드에 붙여넣습니다. 운영 계정 가져오기를 클릭합니다. 운영 계정 정보 가져오기를 클릭합니다. 계정 시드가 없는 경우: 대기 계정 가져오기를 클릭합니다. 대기 계정 정보 가져오기를 클릭합니다. 운영 계정 가져오기를 클릭합니다. 운영 계정 정보 가져오기를 클릭합니다.

## 사용법

To get test accounts:

1. Open `lesson8-time-escrow.py.`
2. 테스트 계정을 받기
   1. 기존 계정 시드가 있는 경우:
      1. Paste Standby account seed in the **Standby Seed** field.
      2. Click **Get Standby Account**.
      3. Click **Get Standby Account Info**.
      4. Paste Operational account seed in the **Operational Seed** field.
      5. Click **Get Operational Account**.
      6. Click **Get Op Account Info**.
   2. 계정 시드가 없는 경우:
      1. Click **Get Standby Account**.
      2. Click **Get Standby Account Info**.
      3. Click **Get Operational Account**.
      4. Click **Get Op Account Info**.

<figure><img src="../../../../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

### ![](<../../../../.gitbook/assets/image (30).png>)에스크로 생성(Create Escrow)

{% embed url="https://youtu.be/L_2sKokW5E4" %}



에스크로를 완료하는 데 걸리는 최소 시간과 수취인이 에스크로에 있는 자금을 더 이상 사용할 수 없는 취소 시간을 설정하여 시간 기반 에스크로를 만들 수 있습니다. 실제 시나리오에서는 시간을 며칠 또는 몇 주 단위로 표현하지만, 이 양식을 사용하면 완료 및 취소 시간을 초 단위로 설정할 수 있으므로 다양한 시나리오를 빠르게 실행할 수 있습니다. (장기 에스크로를 사용하려는 경우 하루에 86,400초를 사용할 수 있습니다.)

시간 기반 에스크로를 만들려면 다음과 같이 하세요:

1. 송금할 금액을 입력합니다. 예: 100000000.
2. 운영 계정 값을 복사합니다.
3. 대상 계정 필드에 붙여넣습니다.
4. 에스크로 완료(초) 값을 설정합니다. 예를 들어 10을 입력합니다.
5. 에스크로 취소(초) 값을 설정합니다. 예를 들어 120을 입력합니다.
6. 시간 기반 에스크로 생성을 클릭합니다.
7. 대기 결과 필드에 호출된 에스크로의 시퀀스 번호를 복사합니다.

에스크로는 XRP Ledger 인스턴스에서 생성되며, 거래 비용과 예약금 요건을 더한 100XRP를 예약합니다. 에스크로를 생성할 때 에스크로 트랜잭션을 완료하는 데 사용할 수 있도록 시퀀스 번호를 캡처하여 저장하세요.

<figure><img src="../../../../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

### 에스크로 끝내기(Finish Escrow)

에스크로에 보관된 XRP를 받는 사람은 에스크로 완료 날짜 및 시간 이후부터 에스크로 취소 날짜 및 시간 전까지 기간 내에 언제든지 거래를 완료할 수 있습니다. 위의 예시에 따라 시퀀스 번호를 사용하여 10초가 지나면 트랜잭션을 완료할 수 있습니다.

시간 기반 에스크로를 완료합니다:

1. 운영 계정 시퀀스 번호 필드에 **Sequence Number**를 붙여넣습니다.
2. **Finish Escrow 클릭.**
3. **Get Op Account Info,** **Get Standby Account Info** 클릭해 XRP 잔액을 업데이트합니다.

거래가 완료되고 대기 계정과 운영 계정 모두에 대한 잔액이 업데이트됩니다.

<figure><img src="https://xrpl.org/img/quickstart-py-escrow4.png" alt=""><figcaption></figcaption></figure>

### 에스크로 가져오기(Get Escrows) <a href="#get-escrows" id="get-escrows"></a>

**Get Escrows**를 클릭하면 운영 계정에 대한 현재 에스크로 목록을 볼 수 있습니다. 지금 버튼을 클릭하면 현재 에스크로가 없습니다.

이 튜토리얼에서는 위의 에스크로 만들기의 단계에 따라 새 에스크로 트랜잭션을 생성한 다음 조회할 수 있습니다. 트랜잭션 결과에서 시퀀스 번호를 캡처하는 것을 잊지 마세요.

<figure><img src="https://xrpl.org/img/quickstart-py-escrow5.png" alt=""><figcaption></figcaption></figure>

### 에스크로 취소(Cancel Escrow) <a href="#cancel-escrow" id="cancel-escrow"></a>

에스크로 취소 시간이 지나면 수령인은 더 이상 에스크로를 사용할 수 없습니다. 에스크로 개시자는 XRP를 회수할 수 있습니다. 에스크로 취소 시간 전에 거래를 취소하려고 하면 거래 비용이 청구되지만, 실제 에스크로는 시간 제한에 도달할 때까지 취소할 수 없습니다. 이전 단계에서 생성한 에스크로에 할당된 시간을 기다린 다음 **Escrow Cancel** 버튼을 사용해 볼 수 있습니다.

만료된 에스크로를 취소하려면 다음과 같이 하세요:

1. 대기 시퀀스 번호 필드에 **Sequence Number** 를 입력합니다.
2. **Standby Account** 값을 복사하여 **Escrow Owner**  필드에 붙여넣습니다.
3. **Cancel Time-based Escrow**. 취소를 클릭합니다.

자금은 초기 거래 수수료를 제외한 금액이 스탠바이 계좌로 반환됩니다.

<figure><img src="https://xrpl.org/img/quickstart-py-escrow6.png" alt=""><figcaption></figcaption></figure>

### 시퀀스 넘버를 잊어버렸다면(Oh No! I Forgot to Save the Sequence Number!) <a href="#oh-no-i-forgot-to-save-the-sequence-number" id="oh-no-i-forgot-to-save-the-sequence-number"></a>

시퀀스 번호를 저장하는 것을 잊어버린 경우 에스크로 거래 기록에서 찾을 수 있습니다.

1. 위의 에스크로 만들기에 설명된 대로 새 에스크로를 생성합니다.
2. 에스크로 가져오기를 클릭하여 에스크로 정보를 가져옵니다.
3. 결과에서 PreviousTxnLgrSeq 값을 복사합니다.&#x20;

<figure><img src="https://xrpl.org/img/quickstart-py-escrow7.png" alt=""><figcaption></figcaption></figure>

4. 조회할 트랜잭션 필드에 PreviousTxnLgrSeq를 붙여넣습니다.
5. **Get Transaction 클릭합니다.**
6.  결과에서 Sequence 값을 찾습니다.

    <figure><img src="https://xrpl.org/img/quickstart-py-escrow8.png" alt=""><figcaption></figcaption></figure>

## 코드 실습(Code Walkthrough <a href="#code-walkthrough" id="code-walkthrough"></a>

이 웹사이트의 소스 리포지토리에서  [Python Modular Code Samples](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/quickstart/py/)을 다운로드할 수 있습니다.

### mod8.py <a href="#mod8py" id="mod8py"></a>

이 예제는 XRP Ledger 네트워크인 테스트넷에서 사용할 수 있습니다. 코드를 업데이트하여 다른 또는 추가적인 XRP Ledger 네트워크를 선택할 수 있습니다.

#### add\_seconds <a href="#add_seconds" id="add_seconds"></a>

이 함수는 두 가지 작업을 수행합니다. 새 날짜 객체를 생성하고 양식 필드에서 가져온 시간(초)을 추가합니다. 그런 다음 날짜를 파이썬 형식에서 XRP Ledger 형식으로 조정합니다.&#x20;

numOfSeconds 인수를 제공합니다.

```
def add_seconds(numOfSeconds):
```

새 Python 날짜 객체를 만듭니다.

```
    new_date = datetime.now()
```

리플 epoch의 날짜 변수를 변환합니다.

```
    if new_date != '':
        new_date = xrpl.utils.datetime_to_ripple_time(new_date)
```

날짜에 초를 추가합니다.

```
        new_date = new_date + int(numOfSeconds)
```

결과 날짜 값을 반환합니다.

```
    return new_date
```

#### create\_time\_escrow <a href="#create_time_escrow" id="create_time_escrow"></a>

create\_time\_escrow 함수를 호출하여 시드, 금액, 목적지, 완료 간격 및 취소 간격을 전달합니다.

```
def create_time_escrow(seed, amount, destination, finish, cancel):
```

클라이언트 지갑을 받습니다.

```
    wallet=Wallet.from_seed(seed)
```

테스트넷에 연결합니다.

```
    client=JsonRpcClient(testnet_url)
```

add\_seconds 함수를 사용하여 완료 및 취소 날짜에 대한 변수를 만듭니다.

```
    finish_date = add_seconds(finish)
    cancel_date = add_seconds(cancel)
```

EscrowCreate 트랜잭션을 정의합니다.

```
    escrow_tx=xrpl.models.transactions.EscrowCreate(
        account=wallet.address,
        amount=amount,
        destination=destination,
        finish_after=finish_date,
        cancel_after=cancel_date
    ) 
```

거래를 제출하고 결과를 보고합니다.

```
    reply=""
    try:
        response=xrpl.transaction.submit_and_wait(escrow_tx,client,wallet)
        reply=response.result
    except xrpl.transaction.XRPLReliableSubmissionException as e:
        reply=f"Submit failed: {e}"
    return reply
```

#### finish\_time\_escrow <a href="#finish_time_escrow" id="finish_time_escrow"></a>

운영 계정 시드, 에스크로 소유자(이 예에서는 대기 주소), 에스크로 시퀀스 번호를 전달합니다.

```
def finish_time_escrow(seed, owner, sequence):
```

지갑과 클라이언트를 인스턴스화합니다.

```
    wallet=Wallet.from_seed(seed)
    client=JsonRpcClient(testnet_url)
```

EscrowFinish 트랜잭션을 정의합니다.

```
    finish_tx=xrpl.models.transactions.EscrowFinish(
        account=wallet.address,
        owner=owner,
        offer_sequence=int(sequence)
    )
```

거래를 제출하고 결과를 보고합니다.

```
    reply=""
    try:
        response=xrpl.transaction.submit_and_wait(finish_tx,client,wallet)
        reply=response.result
    except xrpl.transaction.XRPLReliableSubmissionException as e:
        reply=f"Submit failed: {e}"
    return reply
```

#### get\_escrows <a href="#get_escrows" id="get_escrows"></a>

이 요청에는 계정 인자만 필요합니다.

```
def get_escrows(account):
```

요청이므로 쿼리를 수행하기 위해 계정으로 로그인할 필요가 없습니다. 테스트넷에서 클라이언트를 인스턴스화하기만 하면 됩니다.

```
    client=JsonRpcClient(testnet_url)
```

에스크로 유형의 개체를 지정하여 AccountObjects 요청을 정의합니다.

```
    acct_escrows=AccountObjects(
        account=account,
        ledger_index="validated",
        type="escrow"
    )
```

요청을 제출하고 결과를 반환합니다.

```
    response=client.request(acct_escrows)
    return response.result
```

#### cancel\_time\_escrows <a href="#cancel_time_escrows" id="cancel_time_escrows"></a>

발급자 계정 시드, 소유자 계정, 에스크로 시퀀스 번호를 전달합니다.

```
def cancel_time_escrow(seed, owner, sequence):
```

지갑을 가져와 테스트넷에서 클라이언트를 인스턴스화합니다.

```
    wallet=Wallet.from_seed(seed)
    client=JsonRpcClient(testnet_url)
```

거래 취소 정의

```
    cancel_tx=xrpl.models.transactions.EscrowCancel(
        account=wallet.address,
        owner=owner,
        offer_sequence=int(sequence)
    )
```

거래 제출 및 결과 보고

```
    reply=""
    try:
        response=xrpl.transaction.submit_and_wait(cancel_tx,client,wallet)
        reply=response.result
    except xrpl.transaction.XRPLReliableSubmissionException as e:
        reply=f"Submit failed: {e}"
    return reply
```

#### get\_transaction <a href="#get_transaction" id="get_transaction"></a>

요청 계좌 번호와 이전 거래 원장 시퀀스 번호를 전달합니다.

```
def get_transaction(account, ledger_index):
```

클라이언트 인스턴스를 만듭니다.

```
    client=JsonRpcClient(testnet_url)
```

AccountTx 요청을 생성합니다.

```
    tx_info=AccountTx(
        account=account,
        ledger_index=int(ledger_index)
    )
```

요청을 보내고 결과를 보고합니다.

```
    response=client.request(tx_info)
    return response.result
```

### lesson8-time-escrow.py <a href="#lesson8-time-escrowpy" id="lesson8-time-escrowpy"></a>

이 모듈은 lesson1-send-xrp.py를 기반으로 합니다. 변경 사항은 아래와 같습니다.

```
import tkinter as tk
import xrpl
import json

from mod1 import get_account, get_account_info, send_xrp
```

mod8.py에서 새 함수를 가져옵니다.

```
from mod8 import create_time_escrow, finish_time_escrow, get_escrows, cancel_time_escrow, get_transaction
```

모듈 8 핸들러

```
def standby_create_time_escrow():
    results = create_time_escrow(
        ent_standby_seed.get(),
        ent_standby_amount.get(),
        ent_standby_destination.get(),
        ent_standby_escrow_finish.get(),
        ent_standby_escrow_cancel.get()
    )
    text_standby_results.delete("1.0", tk.END)
    text_standby_results.insert("1.0", json.dumps(results, indent=4))

def operational_finish_time_escrow():
    results = finish_time_escrow(
        ent_operational_seed.get(),
        ent_operational_escrow_owner.get(),
        ent_operational_sequence_number.get()
    )
    text_operational_results.delete("1.0", tk.END)
    text_operational_results.insert("1.0", json.dumps(results, indent=4))

def operational_get_escrows():
    results = get_escrows(ent_operational_account.get())
    text_operational_results.delete("1.0", tk.END)
    text_operational_results.insert("1.0", json.dumps(results, indent=4))

def standby_cancel_time_escrow():
    results = cancel_time_escrow(
        ent_standby_seed.get(),
        ent_standby_escrow_owner.get(),
        ent_standby_escrow_sequence_number.get()
    )
    text_standby_results.delete("1.0", tk.END)
    text_standby_results.insert("1.0", json.dumps(results, indent=4))

def operational_get_transaction():
    results = get_transaction(ent_operational_account.get(),
                              ent_operational_look_up.get())
    text_operational_results.delete("1.0", tk.END)
    text_operational_results.insert("1.0", json.dumps(results, indent=4))    

## Mod 1 Handlers

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
    response = send_xrp(ent_operational_seed.get(),ent_operational_amount.get(),
                        ent_operational_destination.get())
    text_operational_results.delete("1.0", tk.END)
    text_operational_results.insert("1.0",json.dumps(response.result,indent=4))
    get_standby_account_info()
    get_operational_account_info()


# Create a new window with the title "Quickstart Module 1"
window = tk.Tk()
window.title("Time-based Escrow Example")

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

에스크로 명령에 대한 지원 필드를 추가합니다.

```
lbl_standby_escrow_finish = tk.Label(master=frm_form, text="Escrow Finish (seconds)")
ent_standby_escrow_finish = tk.Entry(master=frm_form, width=50)
lbl_standby_escrow_cancel = tk.Label(master=frm_form, text="Escrow Cancel (seconds)")
ent_standby_escrow_cancel = tk.Entry(master=frm_form, width=50)
lbl_standby_escrow_sequence_number = tk.Label(master=frm_form, text="Sequence Number")
ent_standby_escrow_sequence_number = tk.Entry(master=frm_form, width=50)
lbl_standby_escrow_owner = tk.Label(master=frm_form, text="Escrow Owner")
ent_standby_escrow_owner = tk.Entry(master=frm_form, width=50)                    
lbl_standby_results = tk.Label(master=frm_form, text="Results")
text_standby_results = tk.Text(master=frm_form, height = 20, width = 65)

# Place fields in a grid.
lbl_standy_seed.grid(row=0, column=0, sticky="e")
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

양식의 대기 필드에 에스크로를 위한 지원 필드를 추가합니다.

```
lbl_standby_escrow_finish.grid(row=6, column=0, sticky="e")
ent_standby_escrow_finish.grid(row=6, column=1)
lbl_standby_escrow_cancel.grid(row=7, column=0, sticky="e")
ent_standby_escrow_cancel.grid(row=7, column=1)
lbl_standby_escrow_sequence_number.grid(row=8, column=0, sticky="e")
ent_standby_escrow_sequence_number.grid(row=8, column=1)
lbl_standby_escrow_owner.grid(row=9, column=0, sticky="e")
ent_standby_escrow_owner.grid(row=9, column=1)
lbl_standby_results.grid(row=10, column=0, sticky="ne")
text_standby_results.grid(row=10, column=1, sticky="nw")

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

양식의 운영 측면에 대한 에스크로 지원 필드를 정의합니다.

```
lbl_operational_sequence_number = tk.Label(master=frm_form, text="Sequence Number")
ent_operational_sequence_number = tk.Entry(master=frm_form, width=50)
lbl_operational_escrow_owner=tk.Label(master=frm_form, text="Escrow Owner")
ent_operational_escrow_owner=tk.Entry(master=frm_form, width=50)
lbl_operational_look_up = tk.Label(master=frm_form, text="Transaction to Look Up")
ent_operational_look_up = tk.Entry(master=frm_form, width=50)
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

Add supporting fields for escrow to the operational side of the form.

```
lbl_operational_sequence_number.grid(row=6, column=4, sticky="e")
ent_operational_sequence_number.grid(row=6, column=5, sticky="w")
lbl_operational_escrow_owner.grid(row=7, column=4, sticky="e")
ent_operational_escrow_owner.grid(row=7, column=5, sticky="w")
lbl_operational_look_up.grid(row=8, column=4, sticky="e")
ent_operational_look_up.grid(row=8, column=5, sticky="w")
lbl_operational_results.grid(row=10, column=4, sticky="ne")
text_operational_results.grid(row=10, column=5, sticky="nw")

#############################################
## Buttons ##################################
#############################################

# Create the Get Standby Account Buttons
btn_get_standby_account = tk.Button(master=frm_form, text="Get Standby Account",
                                    command = get_standby_account)
btn_get_standby_account.grid(row = 0, column = 2, sticky = "nsew")
btn_get_standby_account_info = tk.Button(master=frm_form,
                                         text="Get Standby Account Info",
                                         command = get_standby_account_info)
btn_get_standby_account_info.grid(row = 1, column = 2, sticky = "nsew")
btn_standby_send_xrp = tk.Button(master=frm_form, text="Send XRP >",
                                 command = standby_send_xrp)
btn_standby_send_xrp.grid(row = 2, column = 2, sticky = "nsew")
```

양식의 운영 측면에 에스크로를 위한 지원 필드를 추가합니다.

```
btn_standby_create_escrow = tk.Button(master=frm_form, text="Create Time-based Escrow",
                                 command = standby_create_time_escrow)
btn_standby_create_escrow.grid(row = 4, column = 2, sticky="nsew")
btn_standby_cancel_escrow = tk.Button(master=frm_form, text="Cancel Time-based Escrow",
                                 command = standby_cancel_time_escrow)
btn_standby_cancel_escrow.grid(row=5,column = 2, sticky="nsew")

# Create the Operational Account Buttons
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

양식의 운영 측면에 에스크로 활동을 지원하는 버튼을 추가합니다.

```
btn_op_finish_escrow = tk.Button(master=frm_form, text="Finish Escrow",
                            command = operational_finish_time_escrow)
btn_op_finish_escrow.grid(row = 4, column = 3, sticky="nsew")
btn_op_finish_escrow = tk.Button(master=frm_form, text="Get Escrows",
                            command = operational_get_escrows)
btn_op_finish_escrow.grid(row = 5, column = 3, sticky="nsew")
btn_op_get_transaction = tk.Button(master=frm_form, text="Get Transaction",
                            command = operational_get_transaction)
btn_op_get_transaction.grid(row = 6, column = 3, sticky = "nsew")


# Start the application
window.mainloop()
```
