# WebSocket을 사용하여 수신 결제 모니터링

이 튜토리얼은 WebSocket API를 사용하여 들어오는 지불을 모니터링하는 방법을 보여줍니다. 모든 XRP Ledger 거래는 공개적이므로, 누구든지 어떤 주소로든 들어오는 지불을 모니터링할 수 있습니다.

WebSocket은 클라이언트와 서버가 하나의 연결을 열고, 동일한 연결을 통해 양방향으로 메시지를 보낸 후, 명시적으로 닫히거나 연결이 실패할 때까지 열려 있는 모델을 따릅니다. 이는 클라이언트가 각 요청에 대해 새로운 연결을 열고 닫는 HTTP 기반 API 모델(JSON-RPC 및 RESTful API 포함)과 대조적입니다.

{% hint style="info" %}
Tip:

이 페이지의 예제들은 웹 브라우저에서 원시적으로 실행할 수 있도록 JavaScript를 사용합니다. 만약 JavaScript로 개발하고 있다면, 일부 작업을 단순화하기 위해 xrpl.js 라이브러리를 사용할 수도 있습니다. 이 튜토리얼은 xrpl.js를 사용하지 않고 거래를 모니터링하는 방법을 보여주므로, 기본적으로 XRP Ledger 클라이언트 라이브러리가 없는 다른 프로그래밍 언어로 단계를 번역할 수 있습니다.
{% endhint %}

## 요구 사항&#x20;

* 이 페이지의 예제들은 JavaScript와 WebSocket 프로토콜을 사용하며, 이들은 모든 주요 현대 브라우저에서 사용 가능합니다. 만약 JavaScript에 대한 지식과 WebSocket 클라이언트가 있는 다른 프로그래밍 언어에 대한 전문 지식이 있다면, 당신의 선택한 언어로 지시사항을 조정하면서 따라갈 수 있습니다.&#x20;
* 안정적인 인터넷 연결과 XRP Ledger 서버에 대한 접근이 필요합니다. 내장된 예제들은 Ripple의 공개 서버 풀에 연결합니다. 만약 rippled 또는 클리오 서버를 직접 운영하고 있다면, 그 서버에 로컬로 연결할 수도 있습니다.
* 올림 오류 없이 XRP 값을 적절하게 처리하기 위해, 64비트 부호 없는 정수에 대한 수학 연산을 수행할 수 있는 숫자 유형에 대한 접근이 필요합니다. 이 튜토리얼의 예제들은 big.js를 사용합니다. 토큰과 작업할 때에는 더욱 더 정밀성이 필요합니다. 자세한 정보는 Currency Precision을 참조하세요.

## 1. XRP Ledger에 연결하기&#x20;

들어오는 지불을 모니터링하는 첫 단계는 XRP Ledger, 특히 rippled 서버에 연결하는 것입니다.

다음 JavaScript 코드는 Ripple의 공개 서버 클러스터 중 하나에 연결합니다. 그 다음 콘솔에 메시지를 로그하고, ping 메소드드를 사용하여 요청을 보내고, 서버 측에서 메시지를 받을 때마다 콘솔에 다시 로그를 작성하는 핸들러를 설정합니다.

```javascript
const socket = new WebSocket('wss://s.altnet.rippletest.net:51233')
socket.addEventListener('open', (event) => {
  // This callback runs when the connection is open
  console.log("Connected!")
  const command = {
    "id": "on_open_ping_1",
    "command": "ping"
  }
  socket.send(JSON.stringify(command))
})
socket.addEventListener('message', (event) => {
  console.log('Got message from server:', event.data)
})
socket.addEventListener('close', (event) => {
  // Use this event to detect when you have become disconnected
  // and respond appropriately.
  console.log('Disconnected...')
})
```

위 예제는 Ripple의 Test Net의 공개 API 서버 중 하나에 안전한 연결(wss://)을 엽니다. 대신 로컬에서 실행 중인 rippled 서버에 기본 설정으로 연결하려면, 로컬 6006포트에 보안이 안된 연결(ws://)을 열어 다음과 같은 첫 줄을 사용하세요:

`const socket = new WebSocket('ws://localhost:6006')`

{% hint style="info" %}
Tip:

기본적으로 로컬 rippled 서버에 연결하면 서버 정보, 그리고 인터넷을 통해 공개 서버에 연결할 때 사용할 수 있는 공개 메소드 등, 관리자 메소드드와 일부 응답에서 관리자 전용 데이터의 전체 세트에 접근할 수 있습니다.
{% endhint %}

## 2. 들어오는 메시지를 핸들러에 전달하기&#x20;

WebSocket 연결은 양방향으로 여러 메시지를 가질 수 있고 요청과 응답 사이에 엄격한 1:1 대응이 없으므로, 들어오는 각 메시지를 어떻게 처리할지 식별해야 합니다. 이를 코딩하는 좋은 모델은 들어오는 메시지를 읽고 각 메시지를 처리하는 데 필요한 코드 경로로 전달하는 "디스패처" 함수를 설정하는 것입니다. 메시지를 적절하게 전달하는 데 도움이 되도록, rippled 서버는 모든 WebSocket 메시지에 type 필드를 제공합니다:

* 클라이언트 측의 요청에 대한 직접 응답인 메시지의 경우 유형은 문자열 응답입니다. 이 경우 서버는 다음 기능도 제공합니다:
* 응답 대상 요청에 제공된 ID와 일치하는 ID 필드입니다. (응답이 순서에 따라 도착할 수 있으므로 중요합니다.)
* API가 요청을 성공적으로 처리했는지 여부를 나타내는 상태 필드입니다. 문자열 값 성공은 성공적인 응답을 나타냅니다. 문자열 값 오류는 오류를 나타냅니다.

{% hint style="info" %}
Warning:

트랜잭션을 제출할 때 WebSocket 메시지의 최상위 수준에서 성공했다고 해서 트랜잭션 자체가 성공한 것은 아닙니다. 서버가 사용자의 요청을 이해했음을 나타낼 뿐입니다. 트랜잭션의 실제 결과를 조회하려면 트랜잭션 결과 조회를 참조하십시오.
{% endhint %}

* 구독에서 보내는 확인할 메일 메시지의 경우 유형은 새 트랜잭션, ledger 또는 유효성 검사 통지 또는 진행 중인 경로 찾기 요청에 대한 확인할 메일 메시지 유형을 나타냅니다. 클라이언트는 이러한 메시지에 가입한 경우에만 메시지를 수신합니다.

{% hint style="info" %}
Tip:\
JavaScript용 xrpl.js 라이브러리는 기본적으로 이 단계를 처리합니다. 모든 비동기 API 요청은 약속을 사용하여 응답을 제공하며 클라이언트의 .on(이벤트, 콜백) 메소드를 사용하여 스트림을 수신할 수 있습니다.
{% endhint %}

다음 JavaScript 코드는 API 요청을 편리한 비동기 Promise로 만들기 위한 도우미 기능을 정의하고, 다른 유형의 메시지를 글로벌 처리기에 매핑하기 위한 인터페이스를 설정합니다:

```javascript
const AWAITING = {}
const handleResponse = function(data) {
  if (!data.hasOwnProperty("id")) {
    console.error("Got response event without ID:", data)
    return
  }
  if (AWAITING.hasOwnProperty(data.id)) {
    AWAITING[data.id].resolve(data)
  } else {
    console.warn("Response to un-awaited request w/ ID " + data.id)
  }
}

let autoid_n = 0
function api_request(options) {
  if (!options.hasOwnProperty("id")) {
    options.id = "autoid_" + (autoid_n++)
  }

  let resolveHolder;
  AWAITING[options.id] = new Promise((resolve, reject) => {
    // Save the resolve func to be called by the handleResponse function later
    resolveHolder = resolve
    try {
      // Use the socket opened in the previous example...
      socket.send(JSON.stringify(options))
    } catch(error) {
      reject(error)
    }
  })
  AWAITING[options.id].resolve = resolveHolder;
  return AWAITING[options.id]
}

const WS_HANDLERS = {
  "response": handleResponse
  // Fill this out with your handlers in the following format:
  // "type": function(event) { /* handle event of this type */ }
}
socket.addEventListener('message', (event) => {
  const parsed_data = JSON.parse(event.data)
  if (WS_HANDLERS.hasOwnProperty(parsed_data.type)) {
    // Call the mapped handler
    WS_HANDLERS[parsed_data.type](parsed_data)
  } else {
    console.log("Unhandled message from server", event)
  }
})

// Show api_request functionality
async function pingpong() {
  console.log("Ping...")
  const response = await api_request({command: "ping"})
  console.log("Pong!", response)
}
// Add pingpong() to the 'open' listener for socket
```

## 3. 계정에 가입하기&#x20;

거래가 계정에 영향을 미칠 때마다 실시간 알림을 받으려면 가입 방법으로 계정을 가입할 수 있습니다. 사실, 그것은 당신 자신의 계정일 필요는 없습니다. 모든 거래가 공개적이기 때문에, 당신은 어떤 계정이든 가입할 수 있고 심지어는 계정의 조합도 가입할 수 있습니다.

하나 이상의 계정에 가입한 후, 서버는 지정된 계정에 영향을 미치는 각 검증된 트랜잭션에 대해 "type": "transaction" 메시지를 발송합니다. 이를 확인하려면 트랜잭션 메시지에서 true인 "validated"를 찾습니다.

다음 코드 샘플은 테스트 네트워크 수도꼭지의 송신 주소를 구독합니다. 이전 단계의 처리기를 디스패처에 추가하여 이러한 트랜잭션마다 메시지를 기록합니다.

```javascript
async function do_subscribe() {
  const sub_response = await api_request({
    command:"subscribe",
    accounts: ["rPT1Sjq2YGrBMTttX4GZHjKu9dyfzbpAYe"]
  })
  if (sub_response.status === "success") {
    console.log("Successfully subscribed!")
  } else {
    console.error("Error subscribing: ", sub_response)
  }
}
// Add do_subscribe() to the 'open' listener for socket

const log_tx = function(tx) {
  console.log(tx.transaction.TransactionType + " transaction sent by " +
              tx.transaction.Account +
              "\n  Result: " + tx.meta.TransactionResult +
              " in ledger " + tx.ledger_index +
              "\n  Validated? " + tx.validated)
}
WS_HANDLERS["transaction"] = log_tx
```

다음 예에서는 다른 창 또는 다른 장치에서 트랜잭션 보낸 사람을 열고 가입한 주소로 트랜잭션을 전송합니다.

## 4. 들어오는 결제 내용 읽기&#x20;

계정에 가입하면 해당 계정에 대한 모든 트랜잭션뿐만 아니라 토큰 거래와 같이 계정에 간접적으로 영향을 미치는 트랜잭션에 대한 메시지가 표시됩니다. 계정이 언제 수신된 지불을 받았는지 인식하는 것이 목표라면 트랜잭션 스트림을 필터링하고 실제 전달된 금액을 기준으로 지불을 처리해야 합니다. 다음 정보를 찾습니다:

* **`validated` field**는 트랜잭션의 결과가 최종임을 나타냅니다. 계정에 가입할 때는 항상 이와 같아야 하지만 accounts\_proposed 또는 transactions\_proposed 스트림에도 가입한 경우 서버는 확인되지 않은 트랜잭션에 대해 동일한 연결에 유사한 메시지를 보냅니다. 예방 조치로 항상 확인된 필드를 확인하는 것이 좋습니다.&#x20;
* **`meta.TransactionResult` field**는 트랜잭션 결과입니다. 결과가 tesSUCCESS가 아닌 경우 트랜잭션이 실패하고 값을 전달할 수 없습니다.&#x20;
* **`transaction.Account`** field는 트랜잭션의 보낸 사람입니다. 다른 사람이 보낸 거래만 찾는 경우 이 필드가 사용자의 계정 주소와 일치하는 모든 거래를 무시할 수 있습니다. (본인에게 통화 간 결제가 가능합니다.)&#x20;
* **`transaction.TransactionType` field**는 트랜잭션의 유형입니다. 계정에 통화를 전달할 수 있는 거래 유형은 다음과 같습니다:
  * 결제 트랜잭션은 XRP 또는 토큰을 전달할 수 있습니다. 트랜잭션을 기준으로 필터링합니다.수신자의 주소를 포함하는 대상 필드이며, 항상 meta.delivered\_금액을 사용하여 실제로 결제가 얼마나 전달되었는지 확인합니다. XRP 양은 문자열 형식으로 지정됩니다.

{% hint style="info" %}
Warning:

대신 transaction.Amount 필드를 사용하면 부분 결제 악용에 취약할 수 있습니다.악의적인 사용자는 이 취약성을 이용하여 사용자를 속여서 사용자가 지불한 금액보다 더 많은 금액을 거래하거나 인출할 수 있습니다.
{% endhint %}

* CheckCash 트랜잭션을 통해 다른 계정의 CheckCreate 트랜잭션에서 승인된 돈을 받을 수 있습니다. CheckCash 트랜잭션의 메타데이터를 확인하여 해당 계정이 받은 통화 수를 확인합니다.
  * EscrowFinish 트랜잭션은 이전 EscrowCreate 트랜잭션에서 생성된 에스크로를 완료하여 XRP를 제공할 수 있습니다. EscrowFinish 트랜잭션의 메타데이터를 확인하여 Escrow에서 XRP를 받은 계정과 금액을 확인합니다.
  * OfferCreate 트랜잭션은 귀하의 계정이 이전에 XRP Ledger의 분산 거래소에 배치된 제안을 소비하여 XRP 또는 토큰을 전달할 수 있습니다. 만약 당신이 제안을 하지 않는다면, 당신은 이런 식으로 돈을 받을 수 없습니다. 메타데이터를 보고 계정이 받은 통화, 받은 통화 및 금액을 확인합니다.
  * PaymentChannelClaim 트랜잭션은 결제 채널에서 XRP를 전송할 수 있습니다. 메타데이터를 확인하여 트랜잭션에서 XRP를 받은 계정(있는 경우)을 확인합니다.
  * Payment Channel Fund 트랜잭션은 폐쇄된(만료된) 지불 채널에서 발신자에게 XRP를 반환할 수 있습니다.
* Meta 필드에는 트랜잭션 메타데이터가 포함되어 있으며, 여기에는 정확히 어느 통화 또는 어느 통화가 전달되었는지가 포함됩니다. 트랜잭션 메타데이터를 이해하는 방법에 대한 자세한 내용은 트랜잭션 결과 조회를 참조하십시오.

다음 샘플 코드는 위의 모든 트랜잭션 유형의 트랜잭션 메타데이터를 조사하여 계정이 받은 XRP 양을 보고합니다:

```javascript
function CountXRPDifference(affected_nodes, address) {
  // Helper to find an account in an AffectedNodes array and see how much
  // its balance changed, if at all. Fortunately, each account appears at most
  // once in the AffectedNodes array, so we can return as soon as we find it.

  // Note: this reports the net balance change. If the address is the sender,
  // the transaction cost is deducted and combined with XRP sent/received

  for (let i=0; i<affected_nodes.length; i++) {
    if ((affected_nodes[i].hasOwnProperty("ModifiedNode"))) {
      // modifies an existing ledger entry
      let ledger_entry = affected_nodes[i].ModifiedNode
      if (ledger_entry.LedgerEntryType === "AccountRoot" &&
          ledger_entry.FinalFields.Account === address) {
        if (!ledger_entry.PreviousFields.hasOwnProperty("Balance")) {
          console.log("XRP balance did not change.")
        }
        // Balance is in PreviousFields, so it changed. Time for
        // high-precision math!
        const old_balance = new Big(ledger_entry.PreviousFields.Balance)
        const new_balance = new Big(ledger_entry.FinalFields.Balance)
        const diff_in_drops = new_balance.minus(old_balance)
        const xrp_amount = diff_in_drops.div(1e6)
        if (xrp_amount.gte(0)) {
          console.log("Received " + xrp_amount.toString() + " XRP.")
          return
        } else {
          console.log("Spent " + xrp_amount.abs().toString() + " XRP.")
          return
        }
      }
    } else if ((affected_nodes[i].hasOwnProperty("CreatedNode"))) {
      // created a ledger entry. maybe the account just got funded?
      let ledger_entry = affected_nodes[i].CreatedNode
      if (ledger_entry.LedgerEntryType === "AccountRoot" &&
          ledger_entry.NewFields.Account === address) {
        const balance_drops = new Big(ledger_entry.NewFields.Balance)
        const xrp_amount = balance_drops.div(1e6)
        console.log("Received " + xrp_amount.toString() + " XRP (account funded).")
        return
      }
    } // accounts cannot be deleted at this time, so we ignore DeletedNode
  }

  console.log("Did not find address in affected nodes.")
  return
}

function CountXRPReceived(tx, address) {
  if (tx.meta.TransactionResult !== "tesSUCCESS") {
    console.log("Transaction failed.")
    return
  }
  if (tx.transaction.TransactionType === "Payment") {
    if (tx.transaction.Destination !== address) {
      console.log("Not the destination of this payment.")
      return
    }
    if (typeof tx.meta.delivered_amount === "string") {
      const amount_in_drops = new Big(tx.meta.delivered_amount)
      const xrp_amount = amount_in_drops.div(1e6)
      console.log("Received " + xrp_amount.toString() + " XRP.")
      return
    } else {
      console.log("Received non-XRP currency.")
      return
    }
  } else if (["PaymentChannelClaim", "PaymentChannelFund", "OfferCreate",
          "CheckCash", "EscrowFinish"].includes(
          tx.transaction.TransactionType)) {
    CountXRPDifference(tx.meta.AffectedNodes, address)
  } else {
    console.log("Not a currency-delivering transaction type (" +
                tx.transaction.TransactionType + ").")
  }
}
```

## 다음 단계&#x20;

* 트랜잭션 결과를 조회하여 트랜잭션이 수행한 작업을 정확하게 확인하고 적절하게 대응할 수 있도록 소프트웨어를 구축합니다.&#x20;
* 자신의 주소에서 XRP를 전송해 보십시오.&#x20;
* 에스크로, 수표 또는 지불 채널과 같은 고급 유형의 트랜잭션을 모니터링하고 수신 알림에 응답합니다.&#x20;

## 기타 프로그래밍 언어&#x20;

많은 프로그래밍 언어에는 WebSocket 연결을 통해 데이터를 보내고 받을 수 있는 라이브러리가 있습니다. JavaScript가 아닌 다른 언어로 Rippled의 WebSocket API와 통신하는 것을 미리 시작하고 싶다면 다음 예를 참조하십시오:

{% tabs %}
{% tab title="Go" %}
```go
package main

// Connect to the XRPL Ledger using websocket and subscribe to an account
// translation from the JavaScript example to Go
// https://xrpl.org/monitor-incoming-payments-with-websocket.html
// This example uses the Gorilla websocket library to create a websocket client
// install: go get github.com/gorilla/websocket

import (
    "encoding/json"
    "flag"
    "log"
    "net/url"
    "os"
    "os/signal"
    "time"

    "github.com/gorilla/websocket"
)

// websocket address
var addr = flag.String("addr", "s.altnet.rippletest.net:51233", "http service address")

// Payload object
type message struct {
    Command  string   `json:"command"`
    Accounts []string `json:"accounts"`
}

func main() {
    flag.Parse()
    log.SetFlags(0)

    var m message

    // check for interrupts and cleanly close the connection
    interrupt := make(chan os.Signal, 1)
    signal.Notify(interrupt, os.Interrupt)

    u := url.URL{Scheme: "ws", Host: *addr, Path: "/"}
    log.Printf("connecting to %s", u.String())

    // make the connection
    c, _, err := websocket.DefaultDialer.Dial(u.String(), nil)
    if err != nil {
        log.Fatal("dial:", err)
    }
    // on exit close
    defer c.Close()

    done := make(chan struct{})

    // send a subscribe command and a target XRPL account
    m.Command = "subscribe"
    m.Accounts = append(m.Accounts, "rUCzEr6jrEyMpjhs4wSdQdz4g8Y382NxfM")

    // struct to JSON marshalling
    msg, _ := json.Marshal(m)
    // write to the websocket
    err = c.WriteMessage(websocket.TextMessage, []byte(string(msg)))
    if err != nil {
        log.Println("write:", err)
        return
    }

    // read from the websocket
    _, message, err := c.ReadMessage()
    if err != nil {
        log.Println("read:", err)
        return
    }
    // print the response from the XRP Ledger
    log.Printf("recv: %s", message)

    // handle interrupt
    for {
        select {
        case <-done:
            return
        case <-interrupt:
            log.Println("interrupt")

            // Cleanly close the connection by sending a close message and then
            // waiting (with timeout) for the server to close the connection.
            err := c.WriteMessage(websocket.CloseMessage, websocket.FormatCloseMessage(websocket.CloseNormalClosure, ""))
            if err != nil {
                log.Println("write close:", err)
                return
            }
            select {
            case <-done:
            case <-time.After(time.Second):
            }
            return
        }
    }
}
```
{% endtab %}

{% tab title="Python" %}
```python
import asyncio
import websockets
import json

# Using client libraries for ASYNC functions and websockets are needed in python.
# To install, use terminal command 'pip install asyncio && pip install websockets'

# Handles incoming messages
async def handler(websocket):
    message = await websocket.recv()
    return message

# Use this to send API requests
async def api_request(options, websocket):
    try:
        await websocket.send(json.dumps(options))
        message = await websocket.recv()
        return json.loads(message)
    except Exception as e:
        return e

# Tests functionality of API_Requst
async def pingpong(websocket):
    command = {
        "id": "on_open_ping_1",
        "command": "ping"
    }
    value = await api_request(command, websocket)
    print(value)

async def do_subscribe(websocket):
    command = await api_request({
        'command': 'subscribe',
        'accounts': ['rPT1Sjq2YGrBMTttX4GZHjKu9dyfzbpAYe']
        }, websocket)

    if command['status'] == 'success':
        print('Successfully Subscribed!')
    else:
        print("Error subscribing: ", command)
    print('Received message from server', await handler(websocket))


async def run():
    # Opens connection to ripple testnet
    async for websocket in websockets.connect('wss://s.altnet.rippletest.net:51233'):
        try:
           await pingpong(websocket)
           await do_subscribe(websocket)
        except websockets.ConnectionClosed:
            print('Disconnected...')

# Runs the webhook on a loop
def main():
    loop = asyncio.get_event_loop()
    loop.run_until_complete(run())
    loop.close()
    print('Restarting Loop')

if __name__ == '__main__':
    main()
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Tip:

선택한 프로그래밍 언어가 표시되지 않습니까? 이 페이지의 맨 위에 있는 "Edit on GitHub" 링크를 클릭하고 자신만의 샘플 코드를 제공합니다!
{% endhint %}

## 각주

1. 실제로 HTTP 기반 API를 여러 번 호출할 때 클라이언트와 서버는 여러 요청 및 응답에 대해 동일한 연결을 재사용할 수 있습니다. 이러한 방식을 HTTP 영구 연결 또는 킵얼라이브(keep-alive)라고 합니다. 개발 관점에서 HTTP 기반 API를 사용하는 코드는 기본 연결이 새 것인지 재사용되는지에 관계없이 동일합니다.

### See Also <a href="#see-also" id="see-also"></a>

* **Concepts:**
  * [Transaction Basics](https://xrpl.org/transaction-basics.html)
  * [Finality of Results](https://xrpl.org/finality-of-results.html) - 트랜잭션의 성공 또는 실패가 최종인지 확인하는 방법. (간단한 버전: 트랜잭션이 유효한 대장에 있는 경우, 결과와 메타데이터가 최종적으로 표시됩니다.)
* **Tutorials:**
  * [Reliable Transaction Submission](https://xrpl.org/reliable-transaction-submission.html)
  * [Look Up Transaction Results](https://xrpl.org/look-up-transaction-results.html)
* **References:**
  * [Transaction Types](https://xrpl.org/transaction-types.html)
  * [Transaction Metadata](https://xrpl.org/transaction-metadata.html) - 메타데이터에 나타나는 메타데이터 형식 및 필드의 요약
  * [Transaction Results](https://xrpl.org/transaction-results.html) - 트랜잭션에 대해 가능한 모든 결과 코드의 테이블입니다.
