# Python에서 데스크톱 지갑 구축

이 튜토리얼에서는 Python 프로그래밍 언어와 다양한 라이브러리를 사용하여 XRP Ledger용 데스크톱 지갑을 구축하는 방법을 설명합니다. 이 애플리케이션은 보다 완벽하고 강력한 애플리케이션을 구축하기 위한 출발점으로, 유사한 애플리케이션을 구축하기 위한 참조점으로, 또는 XRP Ledger 기능을 더 큰 프로젝트에 통합하는 방법을 더 잘 이해하기 위한 학습 경험으로 사용될 수 있습니다.

## 요구사항 <a href="#prerequisites" id="prerequisites"></a>

이 튜토리얼을 완료하려면 다음 지침을 충족해야 합니다:&#x20;

* Python 3.7 이상이 설치되어 있어야 합니다.
* Python의 객체 지향 프로그래밍에 어느 정도 익숙하고 [Get Started Using Python tutorial](https://xrpl.org/get-started-using-python.html)을 완료했습니다.
* 당신은 XRP Ledger가 무엇을 할 수 있는지와 일반적인 암호화폐에 대해 어느정도 이해하고 있습니다. 전문가가 될 필요는 없습니다.

## Source Code <a href="#source-code" id="source-code"></a>

이 튜토리얼의 모든 예제에 대한 전체 소스 코드는 [이 웹 사이트 저장소의 코드 샘플 섹션](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/build-a-wallet/py/)에서 찾을 수 있습니다.

## Goals <a href="#goals" id="goals"></a>

이 튜토리얼의 마지막 부분에는 다음과 같은 Python 애플리케이션이 있어야 합니다:

![Desktop wallet screenshot](https://xrpl.org/img/python-wallet-preview.png)

사용자 인터페이스의 정확한 모양과 느낌은 컴퓨터의 운영 체제에 따라 다릅니다. 이 응용 프로그램은 다음 기능을 수행할 수 있습니다:

* XRP Ledger에 대한 업데이트를 실시간으로 표시합니다.
* 각 트랜잭션에서 XRP가 얼마나 제공되었는지 보여주는 것을 포함하여 모든 XRP Ledger 계정의 "읽기 전용" 활동을 볼 수 있습니다.
* 계정의 [reserve requirement](https://xrpl.org/reserves.html)에 대해 얼마나 많은 XRP가 책정되어 있는지 보여줍니다.
* [direct XRP payments](https://xrpl.org/direct-xrp-payments.html)을 전송할 수 있으며, 다음을 포함하여 목적지 주소에 대한 피드백을 제공합니다:
  * 목적지가 이미 XRP Ledger에 존재하는지, 또는 지불이 XRP Ledger의 생성에 자금을 지원해야 하는지 여부.&#x20;
  * 주소가 XRP([`DisallowXRP` flag](https://xrpl.org/become-an-xrp-ledger-gateway.html#disallow-xrp)  사용)를 수신하지 않으려는 경우. 주소에 확인된 도메인 이름이 연결되어 있는 경우.
  * 주소에 [verified domain name](https://xrpl.org/xrp-ledger-toml.html#account-verification)이 연결되어 있는 경우.

이 튜토리얼의 응용 프로그램은 토큰들의 전송, 교환, 에스크로 또는 결제 채널과 같은 다른 [payment types](https://xrpl.org/payment-types.html)을 사용할 수 없습니다. 그러나 이러한 기능과 기타 기능을 구현할 수 있는 기반을 제공합니다.&#x20;

위의 기능 외에도 Python의 그래픽 사용자 인터페이스(GUI) 프로그래밍, 스레드화 및 비동기 코드에 대해서도 조금 배울 수 있습니다.

## Steps <a href="#steps" id="steps"></a>

### Dependencies 설치 <a href="#install-dependencies" id="install-dependencies"></a>

이 튜토리얼은다양한 프로그래밍 라이브러리에 따라 달라집니다. 코딩을 시작하기 전에 다음과 같이 모든 코드를 설치해야 합니다:

```
pip3 install --upgrade xrpl-py wxPython requests toml
```

(일부 시스템에서는 명령이 pip이거나 sudo pip3를 대신 사용해야 할 수도 있습니다.) 그러면 다음 Python 라이브러리가 설치 및 업그레이드됩니다:

* [xrpl-py](https://xrpl-py.readthedocs.io/)는 XRP Ledger의 클라이언트 라이브러리입니다. 이 자습서에는 버전 1.3.0 이상이 필요합니다.
* [wxPython](https://wxpython.org/), 교차 플랫폼 그래픽 툴킷입니다.
* [Requests](https://requests.readthedocs.io/), HTTP 요청을 수행하기 위한 라이브러리입니다.
* [toml](https://github.com/uiri/toml), TOML 형식 파일을 구문 분석하기 위한 라이브러리입니다.

요청 및 toml 라이브러리는 [domain verification in step 6](https://xrpl.org/build-a-desktop-wallet-in-python.html#6-domain-verification-and-polish)에만 필요하지만 다른 종속성을 설치하는 동안 설치할 수 있습니다.

### Windows 사용자를 위한 참고 사항

Windows에서는 Windows를 기본적으로 사용하거나 WSL(Windows Subsystem for Linux)을 사용하여 앱을 빌드할 수 있습니다.&#x20;

기본 Windows에서는 GUI가 기본 Windows 컨트롤을 사용하므로 위에서 언급한 것 이상의 dependencies 없이 실행되어야 합니다.&#x20;

**Caution**: 2022-02-01 현재 최신 wxPython은 Windows의 Python 3.10과 호환되지 않습니다. 최신 버전의 Python 3.9로 다운그레이드하면 이 튜토리얼을 따를 수 있습니다.

&#x20;WSL에서 libnotify-dev를 다음과 같이 설치해야 할 수 있습니다:

```
apt-get install libnotify-dev
```

WSL에 wxPython을 설치하는 데 문제가 있는 경우 이 방법으로 설치해 볼 수도 있습니다:

```
python -m pip install -U -f https://extras.wxpython.org/wxPython4/extras/linux/gtk3/ubuntu-18.04 wxPython
```

### 1. Hello World <a href="#1-hello-world" id="1-hello-world"></a>

첫 번째 단계는 XRP Ledger와 wxPython 프로그래밍을 위한 "hello world" 등가물을 결합한 앱을 구축하는 것입니다. 코드는 다음과 같습니다:

```python
# "Build a Wallet" tutorial, step 1: slightly more than "Hello World"
# This step demonstrates a simple GUI and XRPL connectivity.
# License: MIT. https://github.com/XRPLF/xrpl-dev-portal/blob/master/LICENSE

import xrpl
import wx

class TWaXLFrame(wx.Frame):
    """
    Tutorial Wallet for the XRP Ledger (TWaXL)
    user interface, main frame.
    """
    def __init__(self, url):
        wx.Frame.__init__(self, None, title="TWaXL", size=wx.Size(800,400))

        self.client = xrpl.clients.JsonRpcClient(url)

        main_panel = wx.Panel(self)
        self.ledger_info = wx.StaticText(main_panel,
                label=self.get_validated_ledger())

    def get_validated_ledger(self):
        try:
            response = self.client.request(xrpl.models.requests.Ledger(
                ledger_index="validated"
            ))
        except Exception as e:
            return f"Failed to get validated ledger from server. ({e})"

        if response.is_successful():
            return f"Latest validated ledger: {response.result['ledger_index']}"
        else:
            # Connected to the server, but the request failed. This can
            # happen if, for example, the server isn't synced to the network
            # so it doesn't have the latest validated ledger.
            return f"Server returned an error: {response.result['error_message']}"

if __name__ == "__main__":
    JSON_RPC_URL = "https://s.altnet.rippletest.net:51234/"
    app = wx.App()
    frame = TWaXLFrame(JSON_RPC_URL)
    frame.Show()
    app.MainLoop()
```

스크립트를 실행하면 XRP Ledger Testnet에서 최신 유효성이 검증된ledger 인덱스를 보여주는 단일 창이 표시됩니다. 다음과 같이 표시됩니다:

![Screenshot: Step 1, hello world equivalent](https://xrpl.org/img/python-wallet-1.png)

후드 아래에서 코드는 JSON-RPC 클라이언트를 만들고, 공용 Testnet 서버에 연결하고, 이 정보를 얻기 위해 [ledger method](https://xrpl.org/ledger.html)을 사용합니다. 한편, 이것은 [`wx.Frame`](https://docs.wxpython.org/wx.Frame.html)`을` 생성합니다.사용자 인터페이스의 기본 프레임 하위 클래스입니다. 이 클래스는 사용자가 볼 수 있는 창을 생성하며, 사용자에게 텍스트를 표시하는 [`wx.StaticText`](https://docs.wxpython.org/wx.StaticText.html)t 위젯과 해당 위젯을 담을 수 있는 [`wx.Panel`](https://docs.wxpython.org/wx.Panel.html)을 사용합니다.

### 2. Show Ledger Updates <a href="#2-show-ledger-updates" id="2-show-ledger-updates"></a>

**Full code for this step:** [`2_threaded.py` ](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/build-a-wallet/py/2\_threaded.py).

1단계의 앱은 앱을 열었을 때 최신 유효성이 확인된 ledger만 표시됩니다. 앱을 닫았다가 다시 열지 않으면 표시되는 텍스트는 변경되지 않습니다. 실제 XRP Ledger는 지속적으로 발전하고 있으므로 더 유용한 앱이 다음과 같은 것을 보여줄 것입니다:

![Animation: Step 2, showing ledger updates](https://xrpl.org/img/python-wallet-2.gif)

새로운 트랜잭션이 확인된 시점을 기다리는 등의 방법으로 ledger의 업데이트를 지속적으로 확인하려면 앱의 아키텍처를 약간 변경해야 합니다. Python과 관련된 이유로 사용자 입력 및 표시를 처리하는 "GUI" 스레드와 XRP Ledger 네트워크 연결을 위한 "worker" 스레드 두 개를 사용하는 것이 가장 좋습니다. 운영 체제는 언제든지 두 스레드 간에 빠르게 전환할 수 있으므로 백그라운드 스레드가 네트워크에서 전송되는 정보를 기다리는 동안 사용자 인터페이스가 응답을 유지할 수 있습니다.&#x20;

스레드의 주요 문제는 다른 스레드가 변경 중인 한 스레드의 데이터에 액세스하지 않도록 주의해야 한다는 것입니다. 이렇게 하는 간단한 방법은 각 스레드가 "소유"하고 다른 스레드의 변수에 쓰지 않도록 프로그램을 설계하는 것입니다. 이 프로그램에서 각 스레드는 고유한 클래스이므로 각 스레드는 고유한 클래스 속성(self로 시작하는 모든 것)에만 써야 합니다. 스레드가 통신해야 할 때는 구체적인 "스레드 세이프" 통신 방법, 즉 다음을 사용합니다:

* GUI에서 작업자로의 통신에는 [`asyncio.run_coroutine_threadsafe()`](https://docs.python.org/3/library/asyncio-task.html#asyncio.run\_coroutine\_threadsafe)를 사용합니다.&#x20;
* 작업자에서 GUI로의 통신에는 [`wx.CallAfter()`](https://docs.wxpython.org/wx.functions.html#wx.CallAfter)를 사용합니다.

클라이언트에 메시지를 전달하는 XRP Ledger의 기능을 최대한 활용하려면 `JsonRpcClient` 대신 [xrpl-py's `AsyncWebsocketClient`](https://xrpl-py.readthedocs.io/en/stable/source/xrpl.asyncio.clients.html#xrpl.asyncio.clients.AsyncWebsocketClient)를 사용합니다. 이렇게 하면 비동기 코드를 사용하여 업데이트를 "구독"하는 동시에 사용자 입력과 같은 다양한 이벤트에 대한 다른 요청/응답 작업을 수행할 수 있습니다.&#x20;

**Note**:기술적으로 동기식(비동기식) WebSocket 클라이언트를 사용할 수는 있지만 GUI에서 입력을 처리하는 동시에 이러한 작업을 관리하는 것이 훨씬 더 복잡해집니다. 비동기 코드를 작성하는 것이 익숙하지 않은 경우에도 나중에 작성해야 하는 코드의 전체적인 복잡성을 줄이는 것이 가치가 있을 수 있습니다.&#x20;

다음 가져오기를 파일의 맨 위에 추가합니다:

```
import asyncio
from threading import Thread
```

그러면 모니터 스레드의 코드는 다음과 같습니다(이를 앱의 나머지 부분과 동일한 파일에 넣습니다):

```python
class XRPLMonitorThread(Thread):
    """
    A worker thread to watch for new ledger events and pass the info back to
    the main frame to be shown in the UI. Using a thread lets us maintain the
    responsiveness of the UI while doing work in the background.
    """
    def __init__(self, url, gui):
        Thread.__init__(self, daemon=True)
        # Note: For thread safety, this thread should treat self.gui as
        # read-only; to modify the GUI, use wx.CallAfter(...)
        self.gui = gui
        self.url = url
        self.loop = asyncio.new_event_loop()
        asyncio.set_event_loop(self.loop)
        self.loop.set_debug(True)

    def run(self):
        """
        This thread runs a never-ending event-loop that monitors messages coming
        from the XRPL, sending them to the GUI thread when necessary, and also
        handles making requests to the XRPL when the GUI prompts them.
        """
        self.loop.run_forever()

    async def watch_xrpl(self):
        """
        This is the task that opens the connection to the XRPL, then handles
        incoming subscription messages by dispatching them to the appropriate
        part of the GUI.
        """

        async with xrpl.asyncio.clients.AsyncWebsocketClient(self.url) as self.client:
            await self.on_connected()
            async for message in self.client:
                mtype = message.get("type")
                if mtype == "ledgerClosed":
                    wx.CallAfter(self.gui.update_ledger, message)

    async def on_connected(self):
        """
        Set up initial subscriptions and populate the GUI with data from the
        ledger on startup. Requires that self.client be connected first.
        """
        # Set up a subscriptions for new ledgers
        response = await self.client.request(xrpl.models.requests.Subscribe(
            streams=["ledger"]
        ))
        # The immediate response contains details for the last validated ledger.
        # We can use this to fill in that area of the GUI without waiting for a
        # new ledger to close.
        wx.CallAfter(self.gui.update_ledger, response.result)
```

이 코드는 작업자에 대한 스레드 하위 클래스를 정의합니다. 스레드가 시작되면 이벤트 loop를 설정하여 비동기 작업이 생성되고 실행될 때까지 기다립니다. 코드는 비동기 작업에서 발생하는 오류를 콘솔에 표시하도록 [`asyncio`'s Debug Mode](https://docs.python.org/3/library/asyncio-dev.html#asyncio-debug-mode)를 사용합니다.

`watch_xrpl()`함수는 이러한 작업(GUI 스레드가 준비될 때 시작됨)의 예입니다. 이 작업은 XRP Ledger에 연결한 다음 새 ledger가 확인될 때마다 [subscribe method](https://xrpl.org/subscribe.html)를 호출하여 알림을 받습니다. 즉시 응답 및 이후의 모든 구독 스트림 메시지를 사용하여 GUI 업데이트를 추적합니다.

{% hint style="info" %}
Tip:

wait 키워드를 사용할 수 있도록 def 대신 asyncdef를 사용하여 이와 같은 작업자 작업을 정의합니다. [`AsyncWebsocketClient.request()` method](https://xrpl-py.readthedocs.io/en/stable/source/xrpl.asyncio.clients.html#xrpl.asyncio.clients.AsyncWebsocketClient.request)에 대한 응답을 얻으려면 `await`를 사용해야 합니다. 일반적으로 asyncdef로 정의한 함수에서 응답을 얻으려면 wait 또는 유사한 작업을 사용해야 합니다. 하지만 이 앱에서는, run\_bg\_job() 도우미는 다른 방식으로 이를 처리합니다.
{% endhint %}

메인 스레드와 GUI 프레임의 코드를 다음과 같이 업데이트합니다:

```python
class TWaXLFrame(wx.Frame):
    """
    Tutorial Wallet for the XRP Ledger (TWaXL)
    user interface, main frame.
    """
    def __init__(self, url):
        wx.Frame.__init__(self, None, title="TWaXL", size=wx.Size(800,400))

        self.build_ui()

        # Start background thread for updates from the ledger ------------------
        self.worker = XRPLMonitorThread(url, self)
        self.worker.start()
        self.run_bg_job(self.worker.watch_xrpl())

    def build_ui(self):
        """
        Called during __init__ to set up all the GUI components.
        """
        main_panel = wx.Panel(self)
        self.ledger_info = wx.StaticText(main_panel, label="Not connected")

        main_sizer = wx.BoxSizer(wx.VERTICAL)
        main_sizer.Add(self.ledger_info, 1, flag=wx.EXPAND|wx.ALL, border=5)
        main_panel.SetSizer(main_sizer)

    def run_bg_job(self, job):
        """
        Schedules a job to run asynchronously in the XRPL worker thread.
        The job should be a Future (for example, from calling an async function)
        """
        task = asyncio.run_coroutine_threadsafe(job, self.worker.loop)

    def update_ledger(self, message):
        """
        Process a ledger subscription message to update the UI with
        information about the latest validated ledger.
        """
        close_time_iso = xrpl.utils.ripple_time_to_datetime(message["ledger_time"]).isoformat()
        self.ledger_info.SetLabel(f"Latest validated ledger:\n"
                         f"Ledger Index: {message['ledger_index']}\n"
                         f"Ledger Hash: {message['ledger_hash']}\n"
                         f"Close time: {close_time_iso}")
```

GUI를 빌드하는 부분이 별도의 메서드인 `build_ui(self)`로 이동되었습니다. `__init__()` 생성자는 다른 작업도 수행해야 하기 때문에 코드를 이해하기 쉬운 청크로 나누는 데 도움이 됩니다. **init**() 생성자는 작업자 스레드를 시작하고 첫 번째 작업을 지정합니다. 또한 GUI 설정에서는 [sizer](https://docs.wxpython.org/sizers\_overview.html)를 사용하여 프레임 내 텍스트 배치를 제어합니다.

{% hint style="info" %}
Tip:

이 튜토리얼에서는 모든 GUI 코드가 손으로 작성되지만 wxGlade와 같은 "빌더" 도구를 사용하여 강력한 GUI를 만드는 것이 더 쉬울 수 있습니다. 생성자와 GUI 코드를 분리하면 나중에 이러한 유형의 접근 방식으로 쉽게 전환할 수 있습니다.
{% endhint %}

작업자 스레드에서 비동기 함수(asyncdef로 정의됨)를 실행하는 새로운 도우미 메소드 `run_bg_job()`이 있습니다. 작업자 스레드가 XRP Ledger 네트워크와 상호 작용하도록 하려면 언제든지 이 방법을 사용하십시오.

`get_validated_ledger()` 메소드 대신에, GUI 클래스는 이제  [ledger stream message](https://xrpl.org/subscribe.html#ledger-stream) 형식의 객체를 받아서 그 정보 중 일부를 사용자에게 표시하는`update_ledger()` 메소드를 갖게 되었습니다. 작업자 스레드는 ledger에서 `ledgerClosed`이벤트를 받을 때마다 `wx.CallAfter()`를 사용하여 이 메소드를 호출합니다.

마지막으로 코드를 변경하여 (파일 끝에 있는) 앱을 시작합니다:

```python
if __name__ == "__main__":
    WS_URL = "wss://s.altnet.rippletest.net:51233" # Testnet
    app = wx.App()
    frame = TWaXLFrame(WS_URL)
    frame.Show()
    app.MainLoop()
```

이제 앱이 JSON-RPC 클라이언트 대신 웹소켓 클라이언트를 사용하므로, 연결을 위해 웹소켓 URL을 사용해야 합니다.

{% hint style="info" %}
Tip:

자신만의 rippled 서버를 운영하면 ws://localhost:6006 라는 URL을 사용하여 연결할 수 있습니다. 또한 Mainnet 또는 기타 테스트 네트워크에 연결하기 위해 공개 서버의 웹소켓 URL을 사용할 수도 있습니다.
{% endhint %}

## SSL 인증서 오류 문제 해결&#x20;

이와 같은 오류가 발생하면, 운영 체제의 인증 기관 스토어가 업데이트 되어 있는지 확인해야 할 수 있습니다:

```
[SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate
```

macOS에서는 Python 버전에 대한 [`Install Certificates.command`](https://stackoverflow.com/questions/52805115/certificate-verify-failed-unable-to-get-local-issuer-certificate)를 실행하세요.&#x20;

Windows에서는 Edge 또는 Chrome을 열고 [https://s1.ripple.com](https://s1.ripple.com/) 로 이동한 후 페이지를 닫습니다. 이렇게 하면 시스템의 인증서가 업데이트 됩니다. (Firefox나 Safari를 사용하면 안 되는데, 이 브라우저들이 Windows의 인증서 유효성 검사 API를 사용하지 않기 때문입니다.)

## 3. 계정 표시하기 <a href="#3-display-an-account" id="3-display-an-account"></a>

**이 단계의 전체 코드:** [`3_account.py` ](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/build-a-wallet/py/3\_account.py)

이제 XRP 원장에 지속적으로 연결할 수 있게 되었으니, 개별 계정을 관리할 수 있는 "지갑" 기능을 추가하기 시작할 시간입니다. 이 단계에서는 사용자가 주소나 마스터 시드를 입력하도록 프롬프트를 제시한 후, 그를 이용하여 [reserve requirement](https://xrpl.org/reserves.html)을 위해 설정된 XRP 양을 포함한 계정에 대한 정보를 표시해야 합니다.

프롬프트는 이와 같은 팝업 대화 상자에 나타납니다:

![Screenshot: step 3, account input prompt](https://xrpl.org/img/python-wallet-3-enter.png)

사용자가 프롬프트를 입력한 후에, 업데이트된 GUI는 이렇게 보입니다:

![Screenshot, step 3, showing account details](https://xrpl.org/img/python-wallet-3-main.png)

XRP 금액에 대한 수학 계산을 할 때는 Decimal 클래스를 사용해야 합니다. 그렇지 않으면 반올림 오류가 발생할 수 있습니다. 파일의 맨 위, 다른 import 다음에 다음을 추가하세요:

```
from decimal import Decimal
```

XRPLMonitorThread 클래스에서 watch\_xrpl() 메소드의 이름을 변경하고 다음과 같이 업데이트하세요:

```python
    async def watch_xrpl_account(self, address, wallet=None):
        """
        This is the task that opens the connection to the XRPL, then handles
        incoming subscription messages by dispatching them to the appropriate
        part of the GUI.
        """
        self.account = address
        self.wallet = wallet

        async with xrpl.asyncio.clients.AsyncWebsocketClient(self.url) as self.client:
            await self.on_connected()
            async for message in self.client:
                mtype = message.get("type")
                if mtype == "ledgerClosed":
                    wx.CallAfter(self.gui.update_ledger, message)
                elif mtype == "transaction":
                    response = await self.client.request(xrpl.models.requests.AccountInfo(
                        account=self.account,
                        ledger_index=message["ledger_index"]
                    ))
                    wx.CallAfter(self.gui.update_account, response.result["account_data"])
```

이름이 새롭게 변경된 watch\_xrpl\_account() 메소드는 이제 주소와 선택적인 지갑을 받아 후에 사용하기 위해 저장합니다. (GUI 스레드가 사용자 입력에 기반하여 이들을 제공합니다.) 이 메소드는 또한 [transaction stream messages](https://xrpl.org/subscribe.html#transaction-streams)에 대한 새로운 케이스를 추가합니다. 새로운 트랜잭션을 보게 되면, 워커는 아직 트랜잭션 자체를 사용하지 않지만, 이를 트리거로 사용하여 [account\_info method](https://xrpl.org/account\_info.html)를 사용하여 계정의 최신 XRP 잔액과 기타 정보를 얻습니다. 그 응답이 도착하면, 워커는 계정 데이터를 GUI에 표시하기 위해 GUI로 전달합니다.

여전히 XRPLMonitorThread 클래스 내에서, `on_connected()` 메소드를 다음과 같이 업데이트합니다:

```python
    async def on_connected(self):
        """
        Set up initial subscriptions and populate the GUI with data from the
        ledger on startup. Requires that self.client be connected first.
        """
        # Set up 2 subscriptions: all new ledgers, and any new transactions that
        # affect the chosen account.
        response = await self.client.request(xrpl.models.requests.Subscribe(
            streams=["ledger"],
            accounts=[self.account]
        ))
        # The immediate response contains details for the last validated ledger.
        # We can use this to fill in that area of the GUI without waiting for a
        # new ledger to close.
        wx.CallAfter(self.gui.update_ledger, response.result)

        # Get starting values for account info.
        response = await self.client.request(xrpl.models.requests.AccountInfo(
            account=self.account,
            ledger_index="validated"
        ))
        if not response.is_successful():
            print("Got error from server:", response)
            # This most often happens if the account in question doesn't exist
            # on the network we're connected to. Better handling would be to use
            # wx.CallAfter to display an error dialog in the GUI and possibly
            # let the user try inputting a different account.
            exit(1)
        wx.CallAfter(self.gui.update_account, response.result["account_data"])
```

on\_connected() 메소드는 이제 제공급된 계정의 트랜잭션에 구독하고(그리고 ledger 스트림도) [account\_info](https://xrpl.org/account\_info.html)를 시작할 때 호출하며, 그 응답을 GUI로 전달하여 표시합니다.

새로운 GUI에는 두 차원에서 배치해야 하는 많은 필드가 있습니다. 다음의 [`wx.GridBagSizer`](https://docs.wxpython.org/wx.GridBagSizer.html)의 하위 클래스는 두 차원의 위젯 목록에 적절한 패딩과 크기 값을 설정하여 빠르게 배치하는 방법을 제공합니다. 이 코드를 동일한 파일에 추가하세요:

```python
class AutoGridBagSizer(wx.GridBagSizer):
    """
    Helper class for adding a bunch of items uniformly to a GridBagSizer.
    """
    def __init__(self, parent):
        wx.GridBagSizer.__init__(self, vgap=5, hgap=5)
        self.parent = parent

    def BulkAdd(self, ctrls):
        """
        Given a two-dimensional iterable `ctrls`, add all the items in a grid
        top-to-bottom, left-to-right, with each inner iterable being a row. Set
        the total number of columns based on the longest iterable.
        """
        flags = wx.EXPAND|wx.ALL|wx.RESERVE_SPACE_EVEN_IF_HIDDEN|wx.ALIGN_CENTER_VERTICAL
        for x, row in enumerate(ctrls):
            for y, ctrl in enumerate(row):
                self.Add(ctrl, (x,y), flag=flags, border=5)
        self.parent.SetSizer(self)
```

다음과 같이`TWaXLFrame`의 생성자를 업데이트합니다:

```python
    def __init__(self, url, test_network=True):
        wx.Frame.__init__(self, None, title="TWaXL", size=wx.Size(800,400))

        self.test_network = test_network
        # The ledger's current reserve settings. To be filled in later.
        self.reserve_base = None
        self.reserve_inc = None

        self.build_ui()

        # Pop up to ask user for their account ---------------------------------
        address, wallet = self.prompt_for_account()
        self.classic_address = address

        # Start background thread for updates from the ledger ------------------
        self.worker = XRPLMonitorThread(url, self)
        self.worker.start()
        self.run_bg_job(self.worker.watch_xrpl_account(address, wallet))
```

이제 생성자는 테스트 네트워크에 연결하는지를 나타내는 boolean 값을 받습니다. (Mainnet URL을 제공한다면 False도 전달해야 합니다.) 이것을 사용해 X-주소를 인코딩하고 디코딩하고, 그것들이 다른 네트워크를 위한 것이라면 경고합니다. 또한 새로운 메소드인 `prompt_for_account()`를 호출하여 주소와 지갑을 가져오고, 이것들을 이름이 바뀐 `watch_xrpl_account()` 백그라운드 작업에 전달합니다.&#x20;

`build_ui()`메소드 정의를 다음과 같이 업데이트하십시오:

```python
    def build_ui(self):
        """
        Called during __init__ to set up all the GUI components.
        """
        main_panel = wx.Panel(self)

        self.acct_info_area = wx.StaticBox(main_panel, label="Account Info")

        lbl_address = wx.StaticText(self.acct_info_area, label="Classic Address:")
        self.st_classic_address = wx.StaticText(self.acct_info_area, label="TBD")
        lbl_xaddress = wx.StaticText(self.acct_info_area, label="X-Address:")
        self.st_x_address = wx.StaticText(self.acct_info_area, label="TBD")
        lbl_xrp_bal = wx.StaticText(self.acct_info_area, label="XRP Balance:")
        self.st_xrp_balance = wx.StaticText(self.acct_info_area, label="TBD")
        lbl_reserve = wx.StaticText(self.acct_info_area, label="XRP Reserved:")
        self.st_reserve = wx.StaticText(self.acct_info_area, label="TBD")

        aia_sizer = AutoGridBagSizer(self.acct_info_area)
        aia_sizer.BulkAdd( ((lbl_address, self.st_classic_address),
                           (lbl_xaddress, self.st_x_address),
                           (lbl_xrp_bal, self.st_xrp_balance),
                           (lbl_reserve, self.st_reserve)) )

        self.ledger_info = wx.StaticText(main_panel, label="Not connected")

        main_sizer = wx.BoxSizer(wx.VERTICAL)
        main_sizer.Add(self.acct_info_area, 1, flag=wx.EXPAND|wx.ALL, border=5)
        main_sizer.Add(self.ledger_info, 1, flag=wx.EXPAND|wx.ALL, border=5)
        main_panel.SetSizer(main_sizer)
```

이제 생성자는 여러 개의 새로운 위젯이 있는 wx.StaticBox를 추가하고, AutoGridBagSizer(위에서 정의)를 사용하여 그들을 상자 내부의 2x4 그리드에 레이아웃합니다. 이 새로운 위젯들은 모두 사용자에게 계정의 세부 사항을 표시하는 정적 텍스트이지만, 그 중 일부는 플레이스홀더 텍스트로 시작합니다. (이들은 ledger로부터 데이터를 필요로 하므로, 워커 스레드가 그 데이터를 되돌려줄 때까지 기다려야 합니다.)

{% hint style="info" %}
**Caution**:&#x20;

이 클래스의 생성자가 wallet 변수를 볼 수 있지만, 이를 객체의 속성으로 저장하지 않는 것을 알 수 있습니다. 이는 지갑이 주로 워커 스레드에 의해 관리되어야 하며, GUI 스레드가 아니라는 점과, 두 곳에서 모두 업데이트하는 것이 스레드에 안전하지 않을 수 있기 때문입니다.
{% endhint %}

TWaXLFrame 클래스에 새로운 prompt\_for\_account() 메소드를 추가하세요:

```python
    def prompt_for_account(self):
        """
        Prompt the user for an account to use, in a base58-encoded format:
        - master key seed: Grants read-write access.
          (assumes the master key pair is not disabled)
        - classic address. Grants read-only access.
        - X-address. Grants read-only access.

        Exits with error code 1 if the user cancels the dialog, if the input
        doesn't match any of the formats, or if the user inputs an X-address
        intended for use on a different network type (test/non-test).

        Populates the classic address and X-address labels in the UI.

        Returns (classic_address, wallet) where wallet is None in read-only mode
        """
        account_dialog = wx.TextEntryDialog(self,
                "Please enter an account address (for read-only)"
                " or your secret (for read-write access)",
                caption="Enter account",
                value="rPT1Sjq2YGrBMTttX4GZHjKu9dyfzbpAYe")
        account_dialog.Bind(wx.EVT_TEXT, self.toggle_dialog_style)

        if account_dialog.ShowModal() != wx.ID_OK:
            # If the user presses Cancel on the account entry, exit the app.
            exit(1)

        value = account_dialog.GetValue().strip()
        account_dialog.Destroy()

        classic_address = ""
        wallet = None
        x_address = ""

        if xrpl.core.addresscodec.is_valid_xaddress(value):
            x_address = value
            classic_address, dest_tag, test_network = xrpl.core.addresscodec.xaddress_to_classic_address(value)
            if test_network != self.test_network:
                on_net = "a test network" if self.test_network else "Mainnet"
                print(f"X-address {value} is meant for a different network type"
                      f"than this client is connected to."
                      f"(Client is on: {on_net})")
                exit(1)

        elif xrpl.core.addresscodec.is_valid_classic_address(value):
            classic_address = value
            x_address = xrpl.core.addresscodec.classic_address_to_xaddress(
                    value, tag=None, is_test_network=self.test_network)

        else:
            try:
                # Check if it's a valid seed
                seed_bytes, alg = xrpl.core.addresscodec.decode_seed(value)
                wallet = xrpl.wallet.Wallet.from_seed(seed=value)
                x_address = wallet.get_xaddress(is_test=self.test_network)
                classic_address = wallet.address
            except Exception as e:
                print(e)
                exit(1)

        # Update the UI with the address values
        self.st_classic_address.SetLabel(classic_address)
        self.st_x_address.SetLabel(x_address)

        return classic_address, wallet
```

생성자는 이 메소드를 호출하여 사용자의 주 [address](https://xrpl.org/accounts.html#addresses) [master seed](https://xrpl.org/cryptographic-keys.html#seed)를 요청하고, 사용자 입력을 처리하여 사용자가 입력한 값을 디코드하여 그에 따라 사용합니다. wxPython을 사용하면 대화 상자에서 다음과 같은 패턴을 일반적으로 따릅니다:

1. wx.TextEntryDialog와 같은 대화 상자 클래스의 새 인스턴스를 생성합니다.
2. showModal()을 사용하여 사용자에게 표시하고 사용자가 클릭한 버튼에 기반한 반환 코드를 얻습니다.
3. 사용자가 OK를 클릭하면 사용자가 입력한 값을 얻습니다. 이 예에서는 사용자가 박스에 입력한 텍스트를 얻습니다.
4. 대화 상자 인스턴스를 파괴합니다. 이 작업을 잊으면 사용자가 새 대화 상자를 열 때마다 애플리케이션이 메모리를 누출할 수 있습니다.

여기서 prompt\_for\_account() 코드는 입력이 클래식 주소, X-주소, 시드 또는 유효한 값이 전혀 없는지 여부에 따라 분기됩니다. 값이 성공적으로 디코딩되면 wx가 업데이트됩니다.주소의 클래식 및 X-주소 등가물을 모두 포함하는 정적 텍스트 위젯은 이 값을 반환합니다. (위에서 설명한 대로, 생성자는 이러한 값을 작업자 스레드에 전달합니다.)

{% hint style="info" %}
Tip:

이 코드는 사용자가 잘못된 값을 입력하면 종료되지만 다시 입력하여 메시지를 표시하거나 사용자에게 다른 메시지를 표시할 수 있습니다.
{% endhint %}

이 코드는 또한 이벤트 핸들러를 바인딩합니다. 이벤트 핸들러는 GUI의 특정 부분에서 특정 유형의 작업이 발생할 때마다 호출되는 메소드로, 일반적으로 사용자의 작업을 기반으로 합니다. 이 경우 트리거는 대화 상자의 wx.EVT\_TEXT이며, 사용자가 대화 상자의 텍스트 상자에 내용을 입력하거나 붙여넣을 때 즉시 트리거됩니다.&#x20;

다음 메소드를 TWaXLFrame 클래스에 추가하여 핸들러를 정의합니다:

```python
    def toggle_dialog_style(self, event):
        """
        Automatically switches to a password-style dialog if it looks like the
        user is entering a secret key, and display ***** instead of s12345...
        """
        dlg = event.GetEventObject()
        v = dlg.GetValue().strip()
        if v[:1] == "s":
            dlg.SetWindowStyle(wx.TE_PASSWORD)
        else:
            dlg.SetWindowStyle(wx.TE_LEFT)
```

이벤트 핸들러는 일반적으로 한 개의 위치 인수, 즉 발생한 정확한 이벤트를 설명하는 wx.Event 객체를 받습니다. 이 경우, 핸들러는 이 객체를 사용하여 사용자가 입력한 값을 알아냅니다. 입력이 마스터 시드처럼 보인다면(문자 "s"로 시작한다면), 핸들러는 대화 상자를 "비밀번호" 스타일로 전환하여 사용자 입력을 마스킹하므로, 사람들이 사용자의 화면을 볼 때 비밀을 볼 수 없습니다. 그리고, 사용자가 그것을 지우고 주소 입력으로 다시 전환하면, 핸들러는 스타일을 다시 전환합니다.

update\_ledger() 메소드의 끝에 다음 라인을 추가하세요:

```
        # Save reserve settings so we can calculate account reserve
        self.reserve_base = xrpl.utils.drops_to_xrp(str(message["reserve_base"]))
        self.reserve_inc = xrpl.utils.drops_to_xrp(str(message["reserve_inc"]))
```

이 코드는 ledger의 현재 예약 설정을 저장하므로, 이를 사용하여 계정의 reserve XRP 총량을 계산할 수 있습니다. TWaXLFrame 클래스에 아래와 같은 메소드를 추가하여 정확히 이 작업을 수행하도록 합니다.

```python
    def calculate_reserve_xrp(self, owner_count):
        """
        Calculates how much XRP the user needs to reserve based on the account's
        OwnerCount and the reserve values in the latest ledger.
        """
        if self.reserve_base == None or self.reserve_inc == None:
            return None
        oc_decimal = Decimal(owner_count)
        reserve_xrp = self.reserve_base + (self.reserve_inc * oc_decimal)
        return reserve_xrp
```

TWaXLFrame 클래스에 update\_account() 메소드를 추가하십시오:

```python
    def update_account(self, acct):
        """
        Update the account info UI based on an account_info response.
        """
        xrp_balance = str(xrpl.utils.drops_to_xrp(acct["Balance"]))
        self.st_xrp_balance.SetLabel(xrp_balance)

        # Display account reserve.
        reserve_xrp = self.calculate_reserve_xrp(acct.get("OwnerCount", 0))
        if reserve_xrp != None:
            self.st_reserve.SetLabel(str(reserve_xrp))
```

워커 스레드는 이 메소드드를 호출하여 계정의 세부 정보를 GUI로 전달하고 표시하게 합니다.&#x20;

마지막으로, 파일의 끝 부분에서, if **name** == "**main**": 블록 내에서, TWaXLFrame 클래스의 인스턴스를 생성하는 라인을 업데이트하여 새로운 test\_net 매개변수를 전달하십시오.

```
    frame = TWaXLFrame(WS_URL, test_network=True)
```

(만약 코드를 Mainnet 서버 URL에 연결하도록 변경한다면, 이 값을 False로도 변경해야 합니다.)&#x20;

지갑 앱을 테스트 계정으로 테스트하려면, 먼저 Testnet Faucet로 이동하고 Testnet 자격증명을 받으십시오. 주소와 비밀 키를 어딘가에 저장하고, 그 중 하나를 사용하여 지갑 앱을 시도해 보십시오. 그런 다음, 잔액 변경 사항을 보려면 Transaction Sender로 이동하고 주소를 Destination Address 필드에 붙여 넣습니다. Initialize를 클릭하고 거기에 있는 일부 거래 유형을 시도해 보십시오. 그리고 지갑 앱에 표시되는 잔액이 예상대로 업데이트되는지 확인하십시오.

## 4. 계정의 트랜잭션 표시 <a href="#4-show-accounts-transactions" id="4-show-accounts-transactions"></a>

**이 단계에 대한 전체 코드:** [`4_tx_history.py` ](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/build-a-wallet/py/4\_tx\_history.py)

이 시점에서, 지갑은 계정의 잔액이 업데이트되는 것을 보여주지만, 실제로 업데이트를 일으킨 거래에 대해선 아무것도 보여주지 않습니다. 그래서 다음 단계는 계정의 거래 내역을 표시하고(그리고 계속 업데이트하는 것)입니다.

새로운 거래 내역은 새 탭에 이렇게 표시됩니다:

![Screenshot: transaction history tab](https://xrpl.org/img/python-wallet-4-main.png)

또한, 앱은 데스크톱 알림(때때로 "토스트"라고 불립니다)을 생성할 수 있으며, 이는 운영 체제에 따라 다음과 같이 보일 수 있습니다:

![Screenshot: notification message](https://xrpl.org/img/python-wallet-4-notif.png)

먼저, 테이블 뷰와 알림을 위한 GUI 클래스를 얻기 위해 다음의 import를 추가합니다:

```
import wx.dataview
import wx.adv
```

다음으로, 거래 구독 메시지를 받을 때 GUI에 거래 상세 정보를 전달하기 위해 작업자 클래스의 `watch_xrpl_account()` 메소드를 업데이트합니다. 이는 오직 한 줄만 필요합니다:

```
wx.CallAfter(self.gui.add_tx_from_sub, message)
```

전체 메소드는 다음과 같아야 합니다:

```python
    async def watch_xrpl_account(self, address, wallet=None):
        """
        This is the task that opens the connection to the XRPL, then handles
        incoming subscription messages by dispatching them to the appropriate
        part of the GUI.
        """
        self.account = address
        self.wallet = wallet

        async with xrpl.asyncio.clients.AsyncWebsocketClient(self.url) as self.client:
            await self.on_connected()
            async for message in self.client:
                mtype = message.get("type")
                if mtype == "ledgerClosed":
                    wx.CallAfter(self.gui.update_ledger, message)
                elif mtype == "transaction":
                    wx.CallAfter(self.gui.add_tx_from_sub, message)
                    response = await self.client.request(xrpl.models.requests.AccountInfo(
                        account=self.account,
                        ledger_index=message["ledger_index"]
                    ))
                    wx.CallAfter(self.gui.update_account, response.result["account_data"])
```

작업자가 account\_tx 메소드를 사용하여 계정의 거래 내역을 찾아 GUI로 전달하게 하십시오. 이 메소드는 기본적으로 가장 최근의 거래부터 시작하여 해당 계정으로, 또는 해당 계정을 통해 이루어진 거래를 포함한 계정에 영향을 미친 거래 목록을 가져옵니다. XRPLMonitorThread의 on\_connected() 메소드의 끝에 새로운 코드를 추가하십시오:

```
        # Get the first page of the account's transaction history. Depending on
        # the server we're connected to, the account's full history may not be
        # available.
        response = await self.client.request(xrpl.models.requests.AccountTx(
            account=self.account
        ))
        wx.CallAfter(self.gui.update_account_tx, response.result)
```

{% hint style="info" %}
Note:&#x20;

계정 생성 이후 계정에 영향을 미친 모든 거래의 완전한 목록을 원한다면, 여러 account\_tx 요청과 응답을 페이징 처리해야 할 수도 있습니다. 이 예시는 페이징 처리를 보여주지 않으므로, 앱은 계정에 영향을 미친 가장 최근의 거래만을 표시합니다.
{% endhint %}

이제, TWaXLFrame 클래스의 build\_ui() 메소드를 편집하십시오. 메소드의 시작 부분을 업데이트하여 새로운 wx.Notebook를 추가하고, "탭" 인터페이스를 만들고, main\_panel을 첫 번째 탭으로 만드십시오. 다음과 같이 수정하십시오:

```python
    def build_ui(self):
        """
        Called during __init__ to set up all the GUI components.
        """
        self.tabs = wx.Notebook(self, style=wx.BK_DEFAULT)
        # Tab 1: "Summary" pane ------------------------------------------------
        main_panel = wx.Panel(self.tabs)
        self.tabs.AddPage(main_panel, "Summary")
```

또한, 거래 내역을 위한 새 탭을 build\_ui() 메소드의 끝에 추가하십시오. 다음과 같이 진행하십시오:

```python
        # Tab 2: "Transaction History" pane ------------------------------------
        objs_panel = wx.Panel(self.tabs)
        self.tabs.AddPage(objs_panel, "Transaction History")
        objs_sizer = wx.BoxSizer(wx.VERTICAL)

        self.tx_list = wx.dataview.DataViewListCtrl(objs_panel)
        self.tx_list.AppendTextColumn("Confirmed")
        self.tx_list.AppendTextColumn("Type")
        self.tx_list.AppendTextColumn("From")
        self.tx_list.AppendTextColumn("To")
        self.tx_list.AppendTextColumn("Value Delivered")
        self.tx_list.AppendTextColumn("Identifying Hash")
        self.tx_list.AppendTextColumn("Raw JSON")
        objs_sizer.Add(self.tx_list, 1, wx.EXPAND|wx.ALL)

        objs_panel.SetSizer(objs_sizer)
```

이는 계정의 거래의 일부 관련 세부 사항을 표시할 수 있는 표 형식으로 많은 정보를 표시할 수 있는 wx.dataview.DataViewListCtrl을 포함하는 두 번째 탭을 추가합니다.

TWaXLFrame 클래스에 다음과 같은 도우미 메소드를 추가하십시오:

```python
    def displayable_amount(self, a):
        """
        Convert an arbitrary amount value from the XRPL to a string to be
        displayed to the user:
        - Convert drops of XRP to 6-decimal XRP (e.g. '12.345000 XRP')
        - For issued tokens, show amount, currency code, and issuer. For
          example, 100 USD issued by address r12345... is returned as
          '100 USD.r12345...'

        Leaves non-standard (hex) currency codes as-is.
        """
        if a == "unavailable":
            # Special case for pre-2014 partial payments.
            return a
        elif type(a) == str:
            # It's an XRP amount in drops. Convert to decimal.
            return f"{xrpl.utils.drops_to_xrp(a)} XRP"
        else:
            # It's a token amount.
            return f"{a['value']} {a['currency']}.{a['issuer']}"
```

이 메소드는 통화 금액을 사용자에게 표시하기 위해 문자열로 변환합니다. 특히 delivered\_amount 필드와 함께 사용되므로, 2014년 이전의 부분 결제(partial payment)에서 사용할 수 없는 delivered amount의 특별한 경우도 처리합니다.

그 후, TWaXLFrame 클래스에 다른 도우미 메소드를 추가하십시오:

```python
    def add_tx_row(self, t, prepend=False):
        """
        Add one row to the account transaction history control. Helper function
        called by other methods.
        """
        conf_dt = xrpl.utils.ripple_time_to_datetime(t["tx"]["date"])
        # Convert datetime to locale-default representation & time zone
        confirmation_time = conf_dt.astimezone().strftime("%c")

        tx_hash = t["tx"]["hash"]
        tx_type = t["tx"]["TransactionType"]
        from_acct = t["tx"].get("Account") or ""
        if from_acct == self.classic_address:
            from_acct = "(Me)"
        to_acct = t["tx"].get("Destination") or ""
        if to_acct == self.classic_address:
            to_acct = "(Me)"

        delivered_amt = t["meta"].get("delivered_amount")
        if delivered_amt:
            delivered_amt = self.displayable_amount(delivered_amt)
        else:
            delivered_amt = ""

        cols = (confirmation_time, tx_type, from_acct, to_acct, delivered_amt,
                tx_hash, str(t))
        if prepend:
            self.tx_list.PrependItem(cols)
        else:
            self.tx_list.AppendItem(cols)
```

이 메소드는 거래 객체를 가져와서 일부 필드를 사용자에게 적합한 형식으로 구문 분석한 다음, 거래 내역 탭의 DataViewListCtrl에 추가합니다.

TWaXLFrame 클래스에 다음과 같은 메소드를 추가하여 worker 스레드의 account\_tx 응답을 기반으로 거래 내역을 업데이트하십시오:

```python
    def update_account_tx(self, data):
        """
        Update the transaction history tab with information from an account_tx
        response.
        """
        txs = data["transactions"]
        # Note: if you extend the code to do paginated responses, you might want
        # to keep previous history instead of deleting the contents first.
        self.tx_list.DeleteAllItems()
        for t in txs:
            self.add_tx_row(t)
```

마지막으로, TWaXLFrame에 유사한 메소드를 추가하여 worker 스레드가 거래 구독 메시지를 전달할 때마다 거래 내역 테이블에 단일 거래를 추가하십시오:

```python
    def add_tx_from_sub(self, t):
        """
        Add 1 transaction to the history based on a subscription stream message.
        Assumes only validated transaction streams (e.g. transactions, accounts)
        not proposed transaction streams.

        Also, send a notification to the user about it.
        """
        # Convert to same format as account_tx results
        t["tx"] = t["transaction"]

        self.add_tx_row(t, prepend=True)
        # Scroll to top of list.
        self.tx_list.EnsureVisible(self.tx_list.RowToItem(0))

        # Send a notification message (aka a "toast") about the transaction.
        # Note the transaction stream and account_tx include all transactions
        # that "affect" the account, no just ones directly from/to the account.
        # For example, if the account has issued tokens, it gets notified when
        # other users transfer those tokens among themselves.
        notif = wx.adv.NotificationMessage(title="New Transaction", message =
                f"New {t['tx']['TransactionType']} transaction confirmed!")
        notif.SetFlags(wx.ICON_INFORMATION)
        notif.Show()
```

앞에서와 마찬가지로, Testnet Faucet과 Transaction Sender를 사용하여 자체 테스트 계정에서 월렛 앱을 테스트할 수 있습니다. Faucet 페이지에서 Get Testnet credentials를 선택하고 (또는 이전에 사용한 동일한 자격 증명을 사용하십시오), 월렛 앱을 열 때 주소 또는 비밀키를 입력하십시오. Transaction Sender 페이지에서 주소를 Destination Address 필드에 붙여넣고 Initialize를 클릭한 다음 다양한 거래 버튼을 클릭하여 월렛이 결과를 표시하는 방식을 확인할 수 있습니다.

## 5. Send XRP <a href="#5-send-xrp" id="5-send-xrp"></a>

**이 단계의 전체 코드:** [`5_send_xrp.py` ](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/build-a-wallet/py/5\_send\_xrp.py)

지금까지 앱은 ledger의 데이터를 볼 수 있도록 만들었으며, 계정이 받은 거래를 표시할 수 있도록 만들었습니다. 이제 마침내 앱이 거래를 보낼 수 있도록 만들 차례입니다. 당분간은 직접 XRP 지불만 보내는 데 집중하겠습니다. 발행된 토큰을 보내는 데는 더 복잡성이 관련되기 때문입니다.

메인 창에는 "Send XRP"라는 새로운 버튼이 추가됩니다.

![Screenshot: main frame with "Send XRP" button enabled](https://xrpl.org/img/python-wallet-5-main.png)

이 버튼을 클릭하면 사용자가 지불의 세부 정보를 입력할 수 있는 대화 상자가 열립니다.

![Screenshot: "Send XRP" dialog](https://xrpl.org/img/python-wallet-5-dialog.png)

먼저, 파일 상단의 import 목록에 regular expressions 라이브러리를 추가하십시오:

```
import re
```

XRPLMonitorThread 클래스에 다음 코드를 on\_connected() 메소드에 추가하십시오. account\_info 응답을 성공적으로 받은 후 어디에서든 사용하면 됩니다.

```python
        if self.wallet:
            wx.CallAfter(self.gui.enable_readwrite)
```

XRPLMonitorThread 클래스에 새로운 메소드를 추가하여 사용자가 제공한 데이터를 기반으로 XRP 지불을 보내고, 보낸 후 GUI에 알림을 표시하십시오.

```python
    async def send_xrp(self, paydata):
        """
        Prepare, sign, and send an XRP payment with the provided parameters.
        Expects a dictionary with:
        {
            "dtag": Destination Tag, as a string, optional
            "to": Destination address (classic or X-address)
            "amt": Amount of decimal XRP to send, as a string
        }
        """
        dtag = paydata.get("dtag", "")
        if dtag.strip() == "":
            dtag = None
        if dtag is not None:
            try:
                dtag = int(dtag)
                if dtag < 0 or dtag > 2**32-1:
                    raise ValueError("Destination tag must be a 32-bit unsigned integer")
            except ValueError as e:
                print("Invalid destination tag:", e)
                print("Canceled sending payment.")
                return

        tx = xrpl.models.transactions.Payment(
            account=self.account,
            destination=paydata["to"],
            amount=xrpl.utils.xrp_to_drops(paydata["amt"]),
            destination_tag=dtag
        )
        # Autofill provides a sequence number, but this may fail if you try to
        # send too many transactions too fast. You can send transactions more
        # rapidly if you track the sequence number more carefully.
        tx_signed = await xrpl.asyncio.transaction.autofill_and_sign(
                tx, self.client, self.wallet)
        await xrpl.asyncio.transaction.submit(tx_signed, self.client)
        wx.CallAfter(self.gui.add_pending_tx, tx_signed)
```

이 흐름에서 앱은 일관성 프로세스에서 확인될 때까지 거래를 기다리지 않고 거래를 보냅니다. 실제 거래 결과는 확인될 때까지 최종적이지 않으므로, 초기 제출 결과를 "pending"이나 "tentative"로 표시해야합니다. 앱이 계정의 거래를 구독하고 있으므로 거래가 확인되면 자동으로 알림을 받게 됩니다.&#x20;

이제 필요한 지불 세부 정보를 입력할 수 있도록 사용자 정의 대화 상자를 만듭니다.

```python
class SendXRPDialog(wx.Dialog):
    """
    Pop-up dialog that prompts the user for the information necessary to send a
    direct XRP-to-XRP payment on the XRPL.
    """
    def __init__(self, parent):
        wx.Dialog.__init__(self, parent, title="Send XRP")
        sizer = AutoGridBagSizer(self)
        self.parent = parent

        lbl_to = wx.StaticText(self, label="To (Address):")
        lbl_dtag = wx.StaticText(self, label="Destination Tag:")
        lbl_amt = wx.StaticText(self, label="Amount of XRP:")
        self.txt_to = wx.TextCtrl(self)
        self.txt_dtag = wx.TextCtrl(self)
        self.txt_amt = wx.SpinCtrlDouble(self, value="20.0", min=0.000001)
        self.txt_amt.SetDigits(6)
        self.txt_amt.SetIncrement(1.0)

        # The "Send" button is functionally an "OK" button except for the text.
        self.btn_send = wx.Button(self, wx.ID_OK, label="Send")
        btn_cancel = wx.Button(self, wx.ID_CANCEL)

        sizer.BulkAdd(((lbl_to, self.txt_to),
                       (lbl_dtag, self.txt_dtag),
                       (lbl_amt, self.txt_amt),
                       (btn_cancel, self.btn_send)) )
        sizer.Fit(self)

        self.txt_dtag.Bind(wx.EVT_TEXT, self.on_dest_tag_edit)
        self.txt_to.Bind(wx.EVT_TEXT, self.on_to_edit)

    def get_payment_data(self):
        """
        Construct a dictionary with the relevant payment details to pass to the
        worker thread for making a payment. Called after the user clicks "Send".
        """
        return {
            "to": self.txt_to.GetValue().strip(),
            "dtag": self.txt_dtag.GetValue().strip(),
            "amt": self.txt_amt.GetValue(),
        }
```

이 wx.Dialog의 서브클래스는 여러 사용자 정의 위젯을 갖고 있으며, 앞에서 정의한 GridBagSizer를 사용하여 배치됩니다. 주목할만한 점으로는 "To" 주소, XRP 금액, 및 대상 태그(있는 경우)에 대한 텍스트 상자가 있습니다. (데스티네이션태그는 XRP Ledger 주소에 대한 전화 내선처럼 작동합니다. 개인이 소유한 주소인 경우에는 필요하지 않지만, 대상 주소에 많은 사용자가 있는 경우 대상이 의도한 수신자를 알기 위해 대상 태그를 지정해야합니다. 암호화폐 거래소에 예금하려면 대상 태그가 필요한 경우가 일반적입니다.) 이 대화 상자에는 OK와 취소 버튼도 있으며, 이 버튼은 대화 상자를 취소하거나 완료하는 기능을 자동으로 수행하지만, "OK" 버튼은 "Send"로 라벨이 지정되어 있어 사용자가 클릭했을 때 앱이 어떤 작업을 수행하는지 명확하게 표시됩니다.&#x20;

SendXRPDialog 생성자는 또한 사용자가 "to" 및 "destination tag" 필드에 텍스트를 입력할 때의 이벤트 핸들러를 바인딩하므로 해당 핸들러의 정의도 SendXRPDialog 클래스에 추가해야합니다. 먼저 on\_to\_edit()를 추가하십시오:

```python
    def on_to_edit(self, event):
        """
        When the user edits the "To" field, check that the address is valid.
        """
        v = self.txt_to.GetValue().strip()

        if not (xrpl.core.addresscodec.is_valid_classic_address(v) or
                xrpl.core.addresscodec.is_valid_xaddress(v) ):
            self.btn_send.Disable()
        elif v == self.parent.classic_address:
            self.btn_send.Disable()
        else:
            self.btn_send.Enable()
```

이 핸들러는 다음 두 가지 조건을 확인하여 "To" 주소가 일치하는지 확인합니다:

1. 올바르게 포맷된 클래식 주소 또는 X-주소인지 확인합니다.
2. 사용자 자신의 주소가 아닌지 확인합니다. 자기 자신에게 XRP를 보낼 수는 없습니다.

조건 중 하나라도 충족되지 않으면 핸들러는 이 대화 상자의 "Send" 버튼을 비활성화합니다. 두 가지 조건이 모두 충족되면 "Send" 버튼을 활성화합니다.

다음으로, on\_dest\_tag\_edit() 핸들러를 추가하십시오. SendXRPDialog 클래스의 메소드로 추가하십시오:

```python
    def on_dest_tag_edit(self, event):
        """
        When the user edits the Destination Tag field, strip non-numeric
        characters from it.
        """
        v = self.txt_dtag.GetValue().strip()
        v = re.sub(r"[^0-9]", "", v)
        self.txt_dtag.ChangeValue(v) # SetValue would generate another EVT_TEXT
        self.txt_dtag.SetInsertionPointEnd()
```

기타 GUI 도구킷에서는 데스티네이션 태그 필드에 전용 숫자 입력 컨트롤을 사용할 수 있지만, wxPython에서는 일반적인 텍스트 입력 필드만 제공되므로 on\_dest\_tag\_edit() 핸들러는 사용자가 필드에 입력한 비숫자 문자를 즉시 삭제하여 숫자 전용 컨트롤처럼 작동하도록 합니다.&#x20;

여기까지 진행한 후, TWaXLFrame 클래스를 편집해야합니다. 먼저, build\_ui() 메소드에서 새로운 "Send XRP" 버튼을 추가하고, 새로운 이벤트 핸들러에 바인딩해야합니다. 다음 코드를 sizer에 추가하는 코드보다 앞에 추가하십시오:

```python
        # Send XRP button. Disabled until we have a secret key & network connection
        self.sxb = wx.Button(main_panel, label="Send XRP")
        self.sxb.SetToolTip("Disabled in read-only mode.")
        self.sxb.Disable()
        self.Bind(wx.EVT_BUTTON, self.click_send_xrp, source=self.sxb)
```

또한, build\_ui() 메소드에서 main\_sizer에 새로운 버튼을 추가하여 계정 정보 영역과 원장 정보 영역 사이에 깔끔하게 배치하십시오. "Tab 1" 섹션 끝에 있는 sizer 코드는 다음과 같아야 합니다. 한 줄을 추가하고 이전 줄을 포함하십시오:

```python
        main_sizer = wx.BoxSizer(wx.VERTICAL)
        main_sizer.Add(self.acct_info_area, 1, flag=wx.EXPAND|wx.ALL, border=5)
        main_sizer.Add(self.sxb, 0, flag=wx.ALL, border=5)
        main_sizer.Add(self.ledger_info, 1, flag=wx.EXPAND|wx.ALL, border=5)
        main_panel.SetSizer(main_sizer)
```

또한, build\_ui() 메소드에서 펜딩 거래의 세부 정보를 보유하기 위해 딕셔너리를 초기화하십시오. 이렇게 하면 나중에 거래가 확인되면 펜딩 버전의 테이블 행을 제거하고 거래의 최종 버전으로 대체할 수 있습니다. self.tx\_list를 설정하는 "Tab 2" 섹션 근처 어디에서든 이 줄을 추가하십시오:

```python
        self.pending_tx_rows = {} # Map pending tx hashes to rows in the history UI
```

"Send XRP" 버튼은 처음에 비활성화되므로, 올바른 조건이 충족되었을 때 활성화되도록 TWaXLFrame 클래스에 새로운 메소드를 추가해야합니다:

```python
    def enable_readwrite(self):
        """
        Enable buttons for sending transactions.
        """
        self.sxb.Enable()
        self.sxb.SetToolTip("")
```

이 단계의 이전에 on\_connected()에 대한 변경 사항은 작업자 클래스가 Wallet 인스턴스를 가지고 있을 때 (즉, 사용자가 실제로 존재하는 계정의 비밀키를 입력한 경우) 이 메소드가호출됩니다. 사용자가 주소를 입력한 경우에는 이 메소드가 호출되지 않습니다.&#x20;

TWaXLFrame 클래스에 사용자가 "Send XRP" 버튼을 클릭할 때의 핸들러를 추가하십시오:

```python
    def click_send_xrp(self, event):
        """
        Pop up a dialog for the user to input how much XRP to send where, and
        send the transaction (if the user doesn't cancel).
        """
        dlg = SendXRPDialog(self)
        dlg.CenterOnScreen()
        resp = dlg.ShowModal()
        if resp != wx.ID_OK:
            print("Send XRP canceled")
            dlg.Destroy()
            return

        paydata = dlg.get_payment_data()
        dlg.Destroy()
        self.run_bg_job(self.worker.send_xrp(paydata))
        notif = wx.adv.NotificationMessage(title="Sending!", message =
                f"Sending a payment for {paydata['amt']} XRP!")
        notif.SetFlags(wx.ICON_INFORMATION)
        notif.Show()
```

이 대화 상자는 이전 단계에서 정의한 사용자 정의 SendXRPDialog 클래스를 사용하여 "Send XRP" 대화 상자를 엽니다. 사용자가 "Send" 버튼을 클릭하면 세부 정보를 작업자 스레드에 전달하여 지불을 보내고, 거래가 전송되었다는 것을 나타내는 알림을 표시합니다. (참고로, 이 시점 이후에도 거래가 실패할 수 있으므로 알림은 거래가 수행한 작업을 나타내지 않습니다.)&#x20;

또한, TWaXLFrame 클래스에 펜딩 거래를 거래 내역 패널에 표시하는 새로운 메소드를 추가하십시오:

```python
    def add_pending_tx(self, txm):
        """
        Add a "pending" transaction to the history based on a transaction model
        that was (presumably) just submitted.
        """
        confirmation_time = "(pending)"
        tx_type = txm.transaction_type
        from_acct = txm.account
        if from_acct == self.classic_address:
            from_acct = "(Me)"
        # Some transactions don't have a destination, so we need to handle that.
        to_acct = getattr(txm, "destination", "")
        if to_acct == self.classic_address:
            to_acct = "(Me)"
        # Delivered amount is only known after a transaction is processed, so
        # leave this column empty in the display for pending transactions.
        delivered_amt = ""
        tx_hash = txm.get_hash()
        cols = (confirmation_time, tx_type, from_acct, to_acct, delivered_amt,
                tx_hash, str(txm.to_xrpl()))
        self.tx_list.PrependItem(cols)
        self.pending_tx_rows[tx_hash] = self.tx_list.RowToItem(0)
```

이 메소드는 거래를 처리하고 표시하기 위해 거래를 거래 내역 테이블에 추가하는 add\_tx\_row() 메소드와 유사합니다. 차이점은 xrpl-py의 Transaction 모델 대신 JSON과 유사한 API 응답을 사용한다는 것이며, 거래가 아직 확인되지 않았으므로 일부 열을 다르게 처리합니다. 중요한 점은 이 트랜잭션을 포함하는 테이블 행에 대한 참조를 pending\_tx\_rows 딕셔너리에 저장하므로, 나중에 트랜잭션이 확인될 때 펜딩 버전의 테이블 행을 제거하고 최종 버전의 거래로 대체할 수 있습니다.&#x20;

마지막으로, add\_tx\_from\_sub() 메소드를 수정하여 펜딩 거래의 최종 결과로 펜딩 거래를 찾아 업데이트하십시오. self.add\_tx\_row() 호출 직전에 다음 줄을 추가하십시오:

```python
        if t["tx"]["hash"] in self.pending_tx_rows.keys():
            dvi = self.pending_tx_rows[t["tx"]["hash"]]
            pending_row = self.tx_list.ItemToRow(dvi)
            self.tx_list.DeleteItem(pending_row)
```

이제 XRP를 보내는 월렛을 사용할 수 있습니다! 새로운 계정에 자금을 추가할 수도 있습니다. 다음 단계를 수행하십시오.

1.  Python 인터프리터를 엽니다.

    ```
    python3
    ```

    **Caution:**OS에 따라 명령이 python 또는 python3 일 수 있습니다. Python 3를 열려면 Python 2.x 버전이 아닌 Python 3를 선택해야합니다.
2.  Python 인터프리터에서 다음 명령을 실행하십시오:

    ```
    import xrpl
    w = xrpl.wallet.Wallet.create()
    print(w.address)
    print(w.seed)
    exit()
    ```

    클래식 주소와 시드를 어딘가에 저장합니다.
3. 월렛 앱을 열고, Testnet Faucet 등에서 얻은 이미 자금이 들어있는 주소의 시크릿(시드) 값을 제공합니다.
4. 적어도 기본 reserve(현재 10 XRP)를 새로 생성한 클래식 주소에 보냅니다.
5. 거래가 확인될 때까지 기다린 다음 월렛 앱을 닫습니다.
6. 월렛 앱을 열고, Python 인터프리터에서 생성한 시드 값을 제공합니다.
7. Python 인터프리터에서 생성한 주소와 일치하는 새로운 계정의 잔고 및 거래 내역을 확인해야 합니다.

## 6. Domain Verification and Polish <a href="#6-domain-verification-and-polish" id="6-domain-verification-and-polish"></a>

**이 단계의 전체 코드:** [`6_verification_and_polish.py` ](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/\_code-samples/build-a-wallet/py/6\_verification\_and\_polish.py)

이전 단계에서 만든 지갑 앱의 가장 큰 단점 중 하나는 사용자를 인간적인 실수나 사기로부터 보호하는 데 많은 보호 기능이나 피드백을 제공하지 않는다는 것입니다. 암호화폐 공간과 같은 분산 시스템에서는 XRP Ledger와 같은 시스템이 취소나 환불을 요청할 수 있는 관리자나 지원팀을 찾을 수 없으므로 실수로 잘못된 주소로 전송하는 경우에 대비하여 이러한 보호 기능이 특히 중요합니다. 이 단계에서는 전송 전에 사용자에게 경고하기 위해 목적지 주소를 확인하는 몇 가지 검사를 추가하는 방법을 보여줍니다.

하나의 검사 유형은 XRP Ledger 주소와 관련된 도메인 이름을 확인하는 것입니다. 이를 계정 도메인 확인이라고 합니다. 계정 도메인이 확인된 경우, 다음과 같이 표시할 수 있습니다:

![Screenshot: domain verified destination](https://xrpl.org/img/python-wallet-6.png)

기타 오류가 있는 경우, 아이콘과 툴팁을 사용하여 사용자에게 오류를 표시할 수 있습니다. 다음과 같이 보입니다:

![Screenshot: invalid address error icon with tooltip](https://xrpl.org/img/python-wallet-6-err.png)

다음 코드는 계정 도메인 확인을 구현한 것이며, verify\_domain.py라는 새 파일로 저장하고 앱의 메인 파일과 동일한 폴더에 저장하십시오:

```python
# Domain verification of XRP Ledger accounts using xrp-ledger.toml file.
# For information on this process, see:
#   https://xrpl.org/xrp-ledger-toml.html#account-verification
# License: MIT. https://github.com/XRPLF/xrpl-dev-portal/blob/master/LICENSE

import requests
import toml
import xrpl

def verify_account_domain(account):
    """
    Verify an account using an xrp-ledger.toml file.

    Params:
        account:dict - the AccountRoot object to verify
    Returns (domain:str, verified:bool)
    """
    domain_hex = account.get("Domain")
    if not domain_hex:
        return "", False
    verified = False
    domain = xrpl.utils.hex_to_str(domain_hex)
    toml_url = f"https://{domain}/.well-known/xrp-ledger.toml"
    toml_response = requests.get(toml_url)
    if toml_response.ok:
        parsed_toml = toml.loads(toml_response.text)
        toml_accounts = parsed_toml.get("ACCOUNTS", [])
        for t_a in toml_accounts:
            if t_a.get("address") == account.get("Account"):
                verified = True
                break
    return domain, verified


if __name__ == "__main__":
    from argparse import ArgumentParser
    parser = ArgumentParser()
    parser.add_argument("address", type=str,
            help="Classic address to check domain verification of")
    args = parser.parse_args()
    client = xrpl.clients.JsonRpcClient("https://xrplcluster.com")
    r = xrpl.account.get_account_info(args.address, client,
            ledger_index="validated")
    print(verify_account_domain(r.result["account_data"]))
```

앱의 메인 파일에서 verify\_account\_domain 함수를 가져옵니다:

```python
from verify_domain import verify_account_domain
```

XRPLMonitorThread 클래스에 새로운 check\_destination() 메소드를 추가하여 데스티네이션 주소를 확인합니다:

```python
    async def check_destination(self, destination, dlg):
        """
        Check a potential destination address's details, and pass them back to
        a "Send XRP" dialog:
        - Is the account funded?
            If not, payments below the reserve base will fail
        - Do they have DisallowXRP enabled?
            If so, the user should be warned they don't want XRP, but can click
            through.
        - Do they have a verified Domain?
            If so, we want to show the user the associated domain info.

        Requires that self.client be connected first.
        """

        # The data to send back to the GUI thread: None for checks that weren't
        # performed, True/False for actual results except where noted.
        account_status = {
            "funded": None,
            "disallow_xrp": None,
            "domain_verified": None,
            "domain_str": "" # the decoded domain, regardless of verification
        }

        # Look up the account. If this fails, the account isn't funded.
        try:
            response = await xrpl.asyncio.account.get_account_info(destination,
                    self.client, ledger_index="validated")
            account_status["funded"] = True
            dest_acct = response.result["account_data"]
        except xrpl.asyncio.clients.exceptions.XRPLRequestFailureException:
            # Not funded, so the other checks don't apply.
            account_status["funded"] = False
            wx.CallAfter(dlg.update_dest_info, account_status)
            return

        # Check DisallowXRP flag
        lsfDisallowXRP = 0x00080000
        if dest_acct["Flags"] & lsfDisallowXRP:
            account_status["disallow_xrp"] = True
        else:
            account_status["disallow_xrp"] = False

        # Check domain verification
        domain, verified = verify_account_domain(dest_acct)
        account_status["domain_verified"] = verified
        account_status["domain_str"] = domain

        # Send data back to the main thread.
        wx.CallAfter(dlg.update_dest_info, account_status)
```

이 코드는 xrpl.asyncio.account.get\_account\_info()를 사용하여 ledger에서 계정을 조회합니다. 클라이언트의 request() 메소드를 사용하는 것과 달리 get\_account\_info()는 계정이 없는 경우 예외를 발생시킵니다

계정이 존재하는 경우, 코드는 lsfDisallowXRP 플래그를 확인합니다. lsfDisallowXRP 플래그는 계정 설정을 구성하는 AccountSet 거래에서 사용되는 플래그와 다르므로 주의하십시오.

마지막으로, 코드는 계정의 도메인 필드를 디코딩하고, 새로 가져온 메소드를 사용하여 도메인 확인을 수행합니다.

{% hint style="info" %}
Caution:

백그라운드 검사는 Send XRP 대화 상자 (dlg)를 매개변수로 사용하며, 각 대화 상자는 별도의 인스턴스이므로 직접 대화 상자를 수정하지 않습니다. 왜냐하면 그렇게 하면 스레드 안전성이 보장되지 않을 수 있기 때문입니다. (검사 결과를 대화 상자로 다시 전달하기 위해 wx.CallAfter만 사용합니다.)
{% endhint %}

이제 SendXRPDialog 클래스를 업데이트하여 이러한 오류를 표시할 수 있도록 해야합니다. 또한 계정이 실제로 보낼 수 있는 최대 XRP 금액에 대한 더 구체적인 상한 값을 설정할 수도 있습니다. 생성자를 변경하여 새 매개변수를 사용하도록 변경하십시오.

```
    def __init__(self, parent, max_send=100000000.0):
```

다음으로, SendXRPDialog 생성자에서 UI에 일부 아이콘 위젯을 추가하겠습니다:

```python
        # Icons to indicate a validation error
        bmp_err = wx.ArtProvider.GetBitmap(wx.ART_ERROR, wx.ART_CMN_DIALOG, size=(16,16))
        self.err_to = wx.StaticBitmap(self, bitmap=bmp_err)
        self.err_dtag = wx.StaticBitmap(self, bitmap=bmp_err)
        self.err_amt = wx.StaticBitmap(self, bitmap=bmp_err)
        self.err_to.Hide()
        self.err_dtag.Hide()
        self.err_amt.Hide()

        # Icons for domain verification
        bmp_check = wx.ArtProvider.GetBitmap(wx.ART_TICK_MARK, wx.ART_CMN_DIALOG, size=(16,16))
        self.domain_text = wx.StaticText(self, label="")
        self.domain_verified = wx.StaticBitmap(self, bitmap=bmp_check)
        self.domain_verified.Hide()

        if max_send <= 0:
            max_send = 100000000.0
            self.err_amt.Show()
            self.err_amt.SetToolTip("Not enough XRP to pay the reserve and transaction cost!")
```

SendXRPDialog 생성자 내에서 아이콘 위젯을 UI에 추가하겠습니다. 또한 self.txt\_amt 위젯을 만드는 줄에 최대값을 추가하십시오:

```
        self.txt_amt = wx.SpinCtrlDouble(self, value="20.0", min=0.000001, max=max_send)
```

생성자에서 추가한 모든 위젯을 SendXRPDialog의 sizer에 추가하여 올바른 위치에 맞도록 하십시오. 생성자에서 BulkAdd 호출을 다음과 같이 업데이트하십시오:

```python
        sizer.BulkAdd(((lbl_to, self.txt_to, self.err_to),
                       (self.domain_verified, self.domain_text),
                       (lbl_dtag, self.txt_dtag, self.err_dtag),
                       (lbl_amt, self.txt_amt, self.err_amt),
                       (btn_cancel, self.btn_send)) )
```

다음으로, SendXRPDialog 클래스의 on\_to\_edit() 핸들러를 리팩토링하여 대상 주소에 대한 백그라운드 검사를 포함한 추가적인 검사를 수행하도록 수정하겠습니다. 업데이트된 핸들러는 다음과 같아야 합니다:

```python
    def on_to_edit(self, event):
        """
        When the user edits the "To" field, check that the address is well-
        formatted. If it's an X-address, fill in the destination tag and disable
        it. Also, start a background check to confirm more details about the
        address.
        """
        v = self.txt_to.GetValue().strip()
        # Reset warnings / domain verification
        err_msg = ""
        self.err_to.SetToolTip("")
        self.err_to.Hide()
        self.domain_text.SetLabel("")
        self.domain_verified.Hide()

        if xrpl.core.addresscodec.is_valid_xaddress(v):
            cl_addr, tag, is_test = xrpl.core.addresscodec.xaddress_to_classic_address(v)
            if tag is None: # Not the same as tag = 0
                tag = ""
            self.txt_dtag.ChangeValue(str(tag))
            self.txt_dtag.Disable()

            if cl_addr == self.parent.classic_address:
                err_msg = "Can't send XRP to self."
            elif is_test != self.parent.test_network:
                err_msg = "This address is intended for a different network."

        elif not self.txt_dtag.IsEditable():
            self.txt_dtag.Clear()
            self.txt_dtag.Enable()

        if not (xrpl.core.addresscodec.is_valid_classic_address(v) or
                xrpl.core.addresscodec.is_valid_xaddress(v) ):
            self.btn_send.Disable()
            err_msg = "Not a valid address."
        elif v == self.parent.classic_address:
            self.btn_send.Disable()
            err_msg = "Can't send XRP to self."
        else:
            self.parent.run_bg_job(self.parent.worker.check_destination(v, self))

        if err_msg:
            self.err_to.SetToolTip(err_msg)
            self.err_to.Show()
        else:
            self.err_to.Hide()
```

백그라운드 검사를 시작하는 것 외에도, 이 핸들러는 즉시 몇 가지 검사를 수행합니다. 네트워크에서 데이터를 가져올 필요가 없는 검사는 핸들러에서 직접 실행할 수 있는 경우가 많습니다. 네트워크 액세스가 필요한 경우에는 워커 스레드에서 실행해야 합니다.&#x20;

새로운 검사 중 하나는 X-주소를 디코딩하여 추가 데이터를 추출하는 것입니다:

* X-주소에 대상 태그가 포함되어 있는 경우, 데스티네이션 태그 필드에 표시합니다.
* X-주소가 테스트 네트워크를 위한 것이 아니고 앱이 테스트 네트워크에 연결되어 있는 경우 (또는 그 반대의 경우), 오류를 표시합니다.

GUI 코드에서 이와 같은 핸들러를 작성하는 가장 어려운 점 중 하나는 사용자가 데이터를 입력하고 지우는 동안 핸들러가 여러 번 호출될 수 있다는 것을 고려해야 한다는 것입니다. 예를 들어, 일부 입력이 잘못된 경우 필드를 비활성화하는 경우, 사용자가 입력을 올바르게 변경하면 필드를 다시 활성화해야 합니다.

이 코드는 오류를 찾을 때 오류 아이콘을 표시하고 (오류가 없는 경우 숨깁니다) 오류 메시지와 함께 툴팁을 추가합니다. 물론, 오류를 다른 방식으로 사용자에게 표시할 수도 있습니다. 예를 들어, 추가 팝업 대화 상자나 상태 표시줄을 사용할 수 있습니다.

이제 SendXRPDialog 클래스에 새로운 메소드를 추가하여 백그라운드 검사의 결과를 처리해야 합니다. 다음 코드를 추가하십시오:

```python
    def update_dest_info(self, dest_status):
        """
        Update the UI with details provided by a background job to check the
        destination address.
        """
        # Keep existing error message if there is one
        try:
            err_msg = self.err_to.GetToolTip().GetTip().strip()
        except RuntimeError:
            # This method can be called after the dialog it belongs to has been
            # closed. In that case, there's nothing to do here.
            return

        if not dest_status["funded"]:
            err_msg = ("Warning: this account does not exist. The payment will "
                      "fail unless you send enough to fund it.")
        elif dest_status["disallow_xrp"]:
            err_msg = "This account does not want to receive XRP."

        # Domain verification
        bmp_err = wx.ArtProvider.GetBitmap(wx.ART_ERROR, wx.ART_CMN_DIALOG, size=(16,16))
        bmp_check = wx.ArtProvider.GetBitmap(wx.ART_TICK_MARK, wx.ART_CMN_DIALOG, size=(16,16))
        domain = dest_status["domain_str"]
        verified = dest_status["domain_verified"]
        if not domain:
            self.domain_text.Hide()
            self.domain_verified.Hide()
        elif verified:
            self.domain_text.SetLabel(domain)
            self.domain_text.Show()
            self.domain_verified.SetToolTip("Domain verified")
            self.domain_verified.SetBitmap(bmp_check)
            self.domain_verified.Show()
        else:
            self.domain_text.SetLabel(domain)
            self.domain_text.Show()
            self.domain_verified.SetToolTip("Failed to verify domain")
            self.domain_verified.SetBitmap(bmp_err)
            self.domain_verified.Show()

        if err_msg:
            # Disabling the button is optional. These types of errors can be
            # benign, so you could let the user "click through" them.
            # self.btn_send.Disable()
            self.err_to.SetToolTip(err_msg)
            self.err_to.Show()
        else:
            self.btn_send.Enable()
            self.err_to.SetToolTip("")
            self.err_to.Hide()
```

이 코드는 check\_destination()에서 전달된 딕셔너리를 사용하여 Send XRP 대화 상자의 GUI의 다양한 위젯을 업데이트합니다.&#x20;

Send XRP 대화 상자에서 최대 송금액을 구성하기 위해 몇 가지 작은 업데이트를 수행해야 합니다. 먼저, TWaXLFrame 클래스의 생성자에 다음 줄을 추가하십시오:

```python
        # This account's total XRP reserve including base + owner amounts
        self.reserve_xrp = None
```

다음으로, TWaXLFrame의 update\_account() 메소드를 수정하여 최신 계산된 예약을 저장하도록 수정하십시오. 마지막 몇 줄을 다음과 같이 변경하십시오:

```
        # Display account reserve and save for calculating max send.
        reserve_xrp = self.calculate_reserve_xrp(acct.get("OwnerCount", 0))
        if reserve_xrp != None:
            self.st_reserve.SetLabel(str(reserve_xrp))
            self.reserve_xrp = reserve_xrp
```

마지막으로, 사용자가 보낼 수 있는 최대 금액을 계산하고 Send XRP 대화 상자에 제공해야 합니다. click\_send\_xrp() 핸들러의 시작 부분을 다음과 같이 수정하십시오:

```
        xrp_bal = Decimal(self.st_xrp_balance.GetLabelText())
        tx_cost = Decimal("0.000010")
        reserve = self.reserve_xrp or Decimal(0.000000)
        dlg = SendXRPDialog(self, max_send=float(xrp_bal - reserve - tx_cost))
```

이 코드에서 사용하는 최대 금액을 계산하기 위한 공식은 계정의 XRP 잔액에서 reserve와 트랜잭션 비용을 뺀 값입니다. 계산은 반올림 오류를유발하지 않도록 Decimal 클래스를 사용하지만, 최종적으로는 wxPython의 wx.SpinCtrlDouble이 최소값 및 최대값으로 허용하는 float으로 변환되어야 합니다. 다른 계산 후에 변환되기 때문에 부동 소수점 반올림 오류가 발생할 가능성이 적습니다.

이전 단계와 동일한 방식으로 월렛 앱을 테스트해보세요. 도메인 검증을 테스트하기 위해 다음 주소를 "To" 상자에 입력해보세요:

| 주소                                   | 도메인          | 인증 여부 |
| ------------------------------------ | ------------ | ----- |
| `rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW` | `mduo13.com` | ✅ Yes |
| `rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn` | `xrpl.org`   | ❌ No  |
| `rPT1Sjq2YGrBMTttX4GZHjKu9dyfzbpAYe` | (Not set)    | ❌ No  |

X-주소를 테스트하려면 다음 주소를 사용해 보세요:

| 주소                                                | 데스티네이션 태그 | testnet 여부 |
| ------------------------------------------------- | --------- | ---------- |
| `T7YChPFWifjCAXLEtg5N74c7fSAYsvPKxzQAET8tbZ8q3SC` | 0         | Yes        |
| `T7YChPFWifjCAXLEtg5N74c7fSAYsvJVm6xKZ14AmjegwRM` | None      | Yes        |
| `X7d3eHCXzwBeWrZec1yT24iZerQjYLjJrFT7A8ZMzzYWCCj` | 0         | No         |
| `X7d3eHCXzwBeWrZec1yT24iZerQjYLeTFXz1GU9RBnWr7gZ` | None      | No         |

### 다음 단계 <a href="#next-steps" id="next-steps"></a>

이제 기능적인 월렛을 가지고 있으므로 다양한 새로운 방향으로 나아갈 수 있습니다. 다음은 몇 가지 아이디어입니다:

* 토큰 및 교차 통화 결제를 포함한 XRP Ledger의 더 많은 트랜잭션 유형 지원
  * 토큰 잔액 및 기타 객체를 표시하는 예제 코드: 7\_owned\_objects.py
* 탈중앙화 거래소에서 거래할 수 있도록 사용자에게 제공
* QR 코드나 URIs를 통해 결제를 요청할 수 있는 방법을 월렛에 추가
* 정기 키 쌍이나 다중 서명을 사용하여 보다 강력한 계정 보안을 지원하는 기능 추가
