# HTTP/WebSocket API 사용 시작하기

선호하는 프로그래밍 언어에서 클라이언트 라이브러리를 가지고 있지 않거나 사용하고 싶지 않은 경우, XRP Ledger의 핵심 서버 소프트웨어인 rippled의 API를 통해 직접 액세스할 수 있습니다. 서버는 JSON-RPC와 WebSocket 프로토콜을 통해 API를 제공합니다. 자체 rippled 인스턴스를 실행하지 않더라도 공개 서버를 사용할 수 있습니다.

{% hint style="info" %}
Tip:

WebSocket API 도구를 이용해 바로 API로 점프하거나, XRP Ledger 탐색기를 사용하여 라이브 ledger 진행 상황을 확인할 수 있습니다.
{% endhint %}

## JSON-RPC와 WebSocket의 차이점&#x20;

JSON-RPC와 WebSocket은 모두 HTTP 기반 프로토콜이며, 대부분의 데이터는 두 프로토콜 모두 동일하게 제공됩니다. 주요 차이점은 다음과 같습니다:

* JSON-RPC는 각 호출에 대해 개별 HTTP 요청 및 응답을 사용하며, RESTful API와 유사합니다. curl, Postman, Requests와 같은 일반적인 HTTP 클라이언트를 사용하여 이 API에 액세스할 수 있습니다.&#x20;
* WebSocket은 서버가 클라이언트에게 데이터를 푸시할 수 있는 영구적인 연결을 사용합니다. 이벤트 구독과 같이 푸시 메시지를 필요로 하는 기능은 WebSocket을 사용하여만 가능합니다.&#x20;

두 API 모두 암호화되지 않은 상태(http:// 및 ws://) 또는 TLS를 사용하여 암호화된 상태(https:// 및 wss://)로 제공될 수 있습니다. 암호화되지 않은 연결은 개방형 네트워크를 통해 제공되어서는 안 되지만, 클라이언트가 서버와 동일한 머신에 있을 때 사용할 수 있습니다.

## 관리자 접근&#x20;

API 메소드는 Public Methods와 Admin Methods로 구분되어 있어, 조직이 커뮤니티를 위해 공개 서버를 제공할 수 있습니다. 관리자 메소드에 접근하거나, 공개 메소드의 관리자 기능에 접근하려면 서버의 설정 파일에서 관리자로 표시된 포트와 IP 주소를 통해 API에 연결해야 합니다.

예시 설정 파일은 로컬 루프백 네트워크(127.0.0.1)에서 연결을 수신하며, JSON-RPC(HTTP)는 5005 포트에서, WebSocket(WS)는 6006 포트에서 수신하며, 연결된 모든 클라이언트를 관리자로 취급합니다.

## WebSocket API

XRP Ledger의 몇 가지 메소드를 시도하고자 할 경우, 직접 WebSocket 코드를 작성하는 것을 건너뛰고 바로 WebSocket API 도구에서 API를 사용할 수 있습니다. 후에 자신의 rippled 서버에 연결하고 싶을 때는 직접 클라이언트를 구축하거나 WebSocket 지원 클라이언트 라이브러리를 사용할 수 있습니다.

예시 WebSocket API 요청:

```json
{
  "id": "my_first_request",
  "command": "server_info",
  "api_version": 1
}
```

응답은 서버의 현재 상태를 보여줍니다.

더 읽어보기: [Request Formatting](https://xrpl.org/request-formatting.html) \
[Response Formatting](https://xrpl.org/response-formatting.html) \
[About the server\_info method](https://xrpl.org/server\_info.html)

## JSON-RPC&#x20;

HTTP 클라이언트를 사용하여(예: Firefox용 RESTED, Chrome용 Postman 또는 온라인 HTTP 클라이언트 ExtendsClass) rippled 서버에 JSON-RPC 호출을 만들 수 있습니다. 대부분의 프로그래밍 언어에는 HTTP 요청을 만드는 라이브러리가 내장되어 있습니다.

예시 JSON-RPC 요청:

```
POST http://s1.ripple.com:51234/
Content-Type: application/json

{
    "method": "server_info",
    "params": [
        {
            "api_version": 1
        }
    ]
}
```

응답은 서버의 현재 상태를 보여줍니다.

더 읽어보기: [Request Formatting](https://xrpl.org/request-formatting.html#json-rpc-format) \
[Response Formatting](https://xrpl.org/response-formatting.html) \
[About the server\_info method](https://xrpl.org/server\_info.html)

## 커맨드라인&#x20;

커맨드라인 인터페이스는 JSON-RPC와 같은 서비스에 연결하므로, 공개 서버와 서버 설정은 동일합니다. 기본적으로 커맨드라인은 동일한 머신에서 실행 중인 rippled 서버에 연결합니다.

예시 커맨드라인 요청:

```
rippled --conf=/etc/opt/ripple/rippled.cfg server_info
```

더 읽어보기: [Commandline Usage Reference](https://xrpl.org/commandline-usage.html)

{% hint style="info" %}
Caution:

커맨드라인 인터페이스는 관리 목적으로만 사용되며 지원되는 API가 아닙니다. 새 버전의 rippled는 경고 없이 커맨드라인 API에 파괴적인 변경을 도입할 수 있습니다!
{% endhint %}

## 사용 가능한 메소드&#x20;

API 메소드의 전체 목록은 다음을 참조하십시오:

* Public rippled Methods: 공개 서버에서 사용할 수 있는 메소드로, ledger에서 데이터를 검색하고 트랜잭션을 제출하는 등의 작업을 포함합니다.
* Admin rippled Methods: rippled 서버를 관리하기 위한 메소드입니다.
