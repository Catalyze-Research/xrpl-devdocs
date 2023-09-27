# 마커 및 페이지네이션

일부 메소드는 하나의 응답에 효율적으로 들어갈 수 있는 것보다 더 많은 데이터를 반환합니다. 포함된 결과보다 많은 결과가 있는 경우 응답에 마커 필드가 포함됩니다. 이를 사용하여 여러 호출에서 더 많은 데이터 페이지를 검색할 수 있습니다. 각 요청에서 이전 응답의 마커 값을 전달하여 중단한 지점부터 다시 시작하세요. 응답에서 마커가 생략된 경우에는 데이터 집합의 끝에 도달한 것입니다.

마커 필드의 형식은 의도적으로 정의되지 않았습니다. 각 서버는 원하는 대로 마커 필드를 정의할 수 있으므로 문자열, 중첩된 객체 또는 다른 유형의 형식을 취할 수 있습니다. 서버마다, 그리고 같은 서버에서 제공하는 메소드마다 마커 정의가 다를 수 있습니다. 각 마커는 임시적이며 10분이 지나면 예상대로 작동하지 않을 수 있습니다.

{% tabs %}
{% tab title="Python" %}
```python
from xrpl.clients import JsonRpcClient
from xrpl.models.requests import LedgerData

# Create a client to connect to the network.
client = JsonRpcClient("https://xrplcluster.com/")

# Query the most recently validated ledger for info.
ledger = LedgerData(ledger_index="validated")
ledger_data = client.request(ledger).result
ledger_data_index = ledger_data["ledger_index"]

# Create a function to run on each API call.
def printLedgerResult():
    print(ledger_data)

# Execute function at least once before checking for markers.
while True:
    printLedgerResult()
    if "marker" not in ledger_data:
        break

    # Specify the same ledger and add the marker to continue querying.
    ledger_marker = LedgerData(ledger_index=ledger_data_index, marker=ledger_data["marker"])
    ledger_data = client.request(ledger_marker).result// Some code
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const xrpl = require("xrpl")

async function main() {

  // Create a client and connect to the network.
  const client = new xrpl.Client("wss://xrplcluster.com/")
  await client.connect()

  // Query the most recently validated ledger for info.
  let ledger = await client.request({
    "command": "ledger_data",
    "ledger_index": "validated",
  })
  const ledger_data_index = ledger["result"]["ledger_index"]

  // Create a function to run on each API call.
  function printLedgerResult(){
    console.log(ledger["result"])
  }

  // Execute function at least once before checking for markers.
  do {
    printLedgerResult()

    if (ledger["result"]["marker"] === undefined) {
        break
    }

    // Specify the same ledger and add the marker to continue querying.
    const ledger_marker = await client.request({
        "command": "ledger_data",
        "ledger_index": ledger_data_index,
        "marker": ledger["result"]["marker"]
    })
    ledger = ledger_marker

  } while (true)

  // Disconnect when done. If you omit this, Node.js won't end the process.
  client.disconnect()
}

main()
```
{% endtab %}
{% endtabs %}

