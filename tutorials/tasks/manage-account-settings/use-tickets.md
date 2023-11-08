# 티켓 사용(Use Tickets)

티켓은 정상적인 순서에서 거래를 보내는 방법을 제공합니다. 이 튜토리얼에서는 티켓을 만든 다음 이를 사용하여 다른 트랜잭션을 보내는 단계를 안내합니다.

## 요구 사항(Prerequisites) <a href="#prerequisites" id="prerequisites"></a>

이 페이지는 xrpl.js 라이브러리를 사용하는 JavaScript 예제를 제공합니다. 설정 지침은 JavaScript 사용 시작하기를 참조하세요.

JavaScript는 웹 브라우저에서 작동하므로 설정 없이 대화형 단계를 읽고 사용할 수 있습니다.

## 단계(Steps) <a href="#steps" id="steps"></a>

이 튜토리얼 몇 단계로 나뉩니다.

* (1-2단계) **설정:** XRP Ledger 주소와 비밀번호가 필요합니다. 프로덕션의 경우 동일한 주소와 비밀번호를 일관되게 사용할 수 있습니다. 이 튜토리얼에서는 필요에 따라 새 테스트 자격 증명을 생성할 수 있습니다. 또한 네트워크에 연결되어 있어야 합니다.
* (3-6단계) **티켓 만들기:** 일부 티켓을 따로 보관하기 위해 트랜잭션을 보냅니다.
* (선택 사항) **중단:** 티켓을 생성한 후 다음 단계 전, 도중 및 후에 언제든지 다양한 기타 트랜잭션을 보낼 수 있습니다.
* (7\~10단계) **티켓 사용:** 따로 보관해 둔 티켓 중 하나를 사용하여 트랜잭션을 전송합니다. 사용할 티켓이 하나 이상 남아 있는 한 이전 부분을 건너뛰면서 이 단계를 반복할 수 있습니다.

## 1. 자격 증명 받기(Get Credentials) <a href="#1-get-credentials" id="1-get-credentials"></a>

XRP Ledger에서 거래하려면 주소와 비밀 키, 그리고 약간의 XRP가 필요합니다. 개발 목적으로 다음 인터페이스를 사용하여 테스트넷에서 얻을 수 있습니다.

* [생성](https://xrpl.org/use-tickets.html#interactive-generate)
* [연결](https://xrpl.org/use-tickets.html#interactive-connect)
* [시퀀스 확인](https://xrpl.org/use-tickets.html#interactive-check\_sequence)
* [준비 및 서명](https://xrpl.org/use-tickets.html#interactive-prepare\_sign)
* [제출](https://xrpl.org/use-tickets.html#interactive-submit)
* [대기](https://xrpl.org/use-tickets.html#interactive-wait)
* [중지](https://xrpl.org/use-tickets.html#interactive-intermission)
* [티켓 확인](https://xrpl.org/use-tickets.html#interactive-check\_tickets)
* [발권 Tx 준비](https://xrpl.org/use-tickets.html#interactive-prepare\_ticketed\_tx)
* [발권 Tx 제출](https://xrpl.org/use-tickets.html#interactive-submit\_ticketed\_tx)

{% hint style="info" %}
Caution:

Ripple은 테스트 목적으로만 Testnet과 Devnet을 제공하며 때로는 모든 잔액과 함께 이러한 테스트 네트워크의 상태를 재설정합니다. 예방 조치로 Testnet/Devnet과 Mainnet에서 동일한 주소를 사용 하지 마세요.
{% endhint %}

프로덕션에 바로 사용할 수 있는 소프트웨어를 구축 할 때 기존 계정을 사용하고 보안 서명 구성을 사용하여 키를 관리해야 합니다.

## 2. 네트워크에 연결(Connect to Network) <a href="#2-connect-to-network" id="2-connect-to-network"></a>

트랜잭션을 제출하려면 네트워크에 연결되어 있어야 합니다. 지금까지 티켓은 Devnet에서만 사용할 수 있으므로 Devnet 서버에 연결해야 합니다. 예를 들어:

```javascript
// Connect to Devnet (since that's where tickets are available)
async function main() {
  const client = new xrpl.Client("wss://s.devnet.rippletest.net:51233")
  await client.connect()
```

{% hint style="info" %}
Note:

이 튜토리얼의 코드 샘플은 JavaScript의 [`async`/ `await`](https://javascript.info/async-await) 패턴을 사용하며, 비동기 함수 내에서 await를 사용해야 하므로 여기서 시작된 main() 함수 내에서 계속 진행되도록 나머지 코드 샘플을 작성합니다. 원하는 경우 [`async`/ `await`](https://javascript.info/async-await) 대신 Promise 메소드 .then() 및 .catch()를 사용할 수도 있습니다.
{% endhint %}

소이 튜토리에서는 다음 버튼을 클릭하여 연결합니다.

* [생성](https://xrpl.org/use-tickets.html#interactive-generate)
* [연결](https://xrpl.org/use-tickets.html#interactive-connect)
* [시퀀스 확인](https://xrpl.org/use-tickets.html#interactive-check\_sequence)
* [준비 및 서명](https://xrpl.org/use-tickets.html#interactive-prepare\_sign)
* [제출](https://xrpl.org/use-tickets.html#interactive-submit)
* [대기](https://xrpl.org/use-tickets.html#interactive-wait)
* [중지](https://xrpl.org/use-tickets.html#interactive-intermission)
* [티켓 확인](https://xrpl.org/use-tickets.html#interactive-check\_tickets)
* [발권 Tx 준비](https://xrpl.org/use-tickets.html#interactive-prepare\_ticketed\_tx)
* [발권 Tx 제출](https://xrpl.org/use-tickets.html#interactive-submit\_ticketed\_tx)

## 3. 시퀀스 번호 확인(Check Sequence Number) <a href="#3-check-sequence-number" id="3-check-sequence-number"></a>

티켓을 생성하기 전에 계정의 시퀀스 번호를 확인해야 합니다. 다음 단계에 대한 현재 시퀀스 번호가 필요하며 별도로 설정한 티켓 시퀀스 번호는 이 번호에서 시작합니다.

```javascript
// Check Sequence Number -----------------------------------------------------
  const account_info = await client.request({
    "command": "account_info",
    "account": wallet.address
  })
  let current_sequence = account_info.result.account_data.Sequence
```

* [생성](https://xrpl.org/use-tickets.html#interactive-generate)
* [연결](https://xrpl.org/use-tickets.html#interactive-connect)
* [시퀀스 확인](https://xrpl.org/use-tickets.html#interactive-check\_sequence)
* [준비 및 서명](https://xrpl.org/use-tickets.html#interactive-prepare\_sign)
* [제출](https://xrpl.org/use-tickets.html#interactive-submit)
* [대기](https://xrpl.org/use-tickets.html#interactive-wait)
* [중지](https://xrpl.org/use-tickets.html#interactive-intermission)
* [티켓 확인](https://xrpl.org/use-tickets.html#interactive-check\_tickets)
* [발권 Tx 준비](https://xrpl.org/use-tickets.html#interactive-prepare\_ticketed\_tx)
* [발권 Tx 제출](https://xrpl.org/use-tickets.html#interactive-submit\_ticketed\_tx)

## 4. TicketCreate 준비 및 서명(Prepare and Sign TicketCreate) <a href="#4-prepare-and-sign-ticketcreate" id="4-prepare-and-sign-ticketcreate"></a>

이전 단계에서 결정한 시퀀스 번호를 사용하여 TicketCreate 트랜잭션을 생성합니다. 필드를 사용하여 `TicketCount`만들 티켓 수를 지정합니다. 예를 들어, 10개의 티켓을 만드는 거래를 준비하려면:

```javascript
// Prepare and Sign TicketCreate ---------------------------------------------
  const prepared = await client.autofill({
    "TransactionType": "TicketCreate",
    "Account": wallet.address,
    "TicketCount": 10,
    "Sequence": current_sequence
  })
  const signed = wallet.sign(prepared)
  console.log(`Prepared TicketCreate transaction ${signed.hash}`)
```

나중에 검증되었는지 여부를 확인할 수 있도록 트랜잭션의 해시와 `LastLedgerSequence`값을 기록하세요.

* [생성](https://xrpl.org/use-tickets.html#interactive-generate)
* [연결](https://xrpl.org/use-tickets.html#interactive-connect)
* [시퀀스 확인](https://xrpl.org/use-tickets.html#interactive-check\_sequence)
* [준비 및 서명](https://xrpl.org/use-tickets.html#interactive-prepare\_sign)
* [제출](https://xrpl.org/use-tickets.html#interactive-submit)
* [대기](https://xrpl.org/use-tickets.html#interactive-wait)
* [중지](https://xrpl.org/use-tickets.html#interactive-intermission)
* [티켓 확인](https://xrpl.org/use-tickets.html#interactive-check\_tickets)
* [발권 Tx 준비](https://xrpl.org/use-tickets.html#interactive-prepare\_ticketed\_tx)
* [발권 Tx 제출](https://xrpl.org/use-tickets.html#interactive-submit\_ticketed\_tx)

## 5. 티켓 제출 생성(Submit TicketCreate) <a href="#5-submit-ticketcreate" id="5-submit-ticketcreate"></a>

이전 단계에서 만든 서명된 트랜잭션 Blob을 제출합니다. 예를 들어:

```javascript
// Submit TicketCreate -------------------------------------------------------
  const tx = await client.submitAndWait(signed.tx_blob)
  console.log(tx)
```

* [생성](https://xrpl.org/use-tickets.html#interactive-generate)
* [연결](https://xrpl.org/use-tickets.html#interactive-connect)
* [시퀀스 확인](https://xrpl.org/use-tickets.html#interactive-check\_sequence)
* [준비 및 서명](https://xrpl.org/use-tickets.html#interactive-prepare\_sign)
* [제출](https://xrpl.org/use-tickets.html#interactive-submit)
* [대기](https://xrpl.org/use-tickets.html#interactive-wait)
* [중지](https://xrpl.org/use-tickets.html#interactive-intermission)
* [티켓 확인](https://xrpl.org/use-tickets.html#interactive-check\_tickets)
* [발권 Tx 준비](https://xrpl.org/use-tickets.html#interactive-prepare\_ticketed\_tx)
* [발권 Tx 제출](https://xrpl.org/use-tickets.html#interactive-submit\_ticketed\_tx)

## 6. 유효성 검사 대기(Wait for Validation) <a href="#6-wait-for-validation" id="6-wait-for-validation"></a>

대부분의 트랜잭션은 제출된 후 다음 ledger 버전으로 승인됩니다. 즉, 트랜잭션 결과가 최종 결과가 되기까지 4-7초가 걸릴 수 있습니다. XRP Ledger가 사용 중이거나 네트워크 연결 상태가 좋지 않아 트랜잭션이 네트워크 전체에 전달되는 것이 지연되면 트랜잭션을 확인하는 데 시간이 더 오래 걸릴 수 있습니다. (거래 만료 설정 방법에 대한 자세한 내용은 신뢰할 수 있는 거래 제출을 참조하세요.)

```javascript
// Wait for Validation -------------------------------------------------------
  // submitAndWait() handles this automatically, but it can take 4-7s.
```

* [생성](https://xrpl.org/use-tickets.html#interactive-generate)
* [연결](https://xrpl.org/use-tickets.html#interactive-connect)
* [시퀀스 확인](https://xrpl.org/use-tickets.html#interactive-check\_sequence)
* [준비 및 서명](https://xrpl.org/use-tickets.html#interactive-prepare\_sign)
* [제출](https://xrpl.org/use-tickets.html#interactive-submit)
* [대기](https://xrpl.org/use-tickets.html#interactive-wait)
* [중지](https://xrpl.org/use-tickets.html#interactive-intermission)
* [티켓 확인](https://xrpl.org/use-tickets.html#interactive-check\_tickets)
* [발권 Tx 준비](https://xrpl.org/use-tickets.html#interactive-prepare\_ticketed\_tx)
* [발권 Tx 제출](https://xrpl.org/use-tickets.html#interactive-submit\_ticketed\_tx)

| 거래 ID:                   | (없음)      |
| ------------------------ | --------- |
| 최신 검증 원장 인덱스:            | (연결되지 않은) |
| 제출 시점의 원장 인덱스:           | (제출하지 않은) |
| 거래 `LastLedgerSequence`: | (미준비)     |
|                          |           |

### (선택) 중간 정지 <a href="#optional-intermission" id="optional-intermission"></a>

티켓의 장점은 티켓 거래를 준비하는 동안 평소와 같이 계정의 비즈니스를 계속할 수 있다는 것입니다. 티켓을 사용하여 트랜잭션을 전송하려는 경우 다른 티켓을 사용하는 트랜잭션을 포함하여 다른 전송 트랜잭션과 병렬로 수행하고 언제든지 티켓이 있는 트랜잭션을 제출할 수 있습니다. 유일한 제약은 각 티켓이 한 번만 사용할 수 있다는 것입니다.

{% hint style="info" %}
Tip:

발권 거래의 성공을 방해하지 않고 다음 단계 사이 또는 도중에 여기로 돌아와 순차 거래를 보낼 수 있습니다.
{% endhint %}

* [생성](https://xrpl.org/use-tickets.html#interactive-generate)
* [연결](https://xrpl.org/use-tickets.html#interactive-connect)
* [시퀀스 확인](https://xrpl.org/use-tickets.html#interactive-check\_sequence)
* [준비 및 서명](https://xrpl.org/use-tickets.html#interactive-prepare\_sign)
* [제출](https://xrpl.org/use-tickets.html#interactive-submit)
* [대기](https://xrpl.org/use-tickets.html#interactive-wait)
* [중지](https://xrpl.org/use-tickets.html#interactive-intermission)
* [티켓 확인](https://xrpl.org/use-tickets.html#interactive-check\_tickets)
* [발권 Tx 준비](https://xrpl.org/use-tickets.html#interactive-prepare\_ticketed\_tx)
* [발권 Tx 제출](https://xrpl.org/use-tickets.html#interactive-submit\_ticketed\_tx)

## 7. 사용 가능한 티켓 확인(Check Available Tickets) <a href="#7-check-available-tickets" id="7-check-available-tickets"></a>

발권된 트랜잭션을 보내려면 사용할 티켓 시퀀스 번호를 알아야 합니다. 계정을 주의 깊게 추적해 왔다면 어떤 티켓이 있는지 이미 알고 있지만 확실하지 않은 경우 account\_objects 메서드를 사용하여 사용 가능한 티켓을 조회할 수 있습니다. 예를 들어:

```javascript
// Check Available Tickets ---------------------------------------------------
  let response = await client.request({
    "command": "account_objects",
    "account": wallet.address,
    "type": "ticket"
  })
  console.log("Available Tickets:", response.result.account_objects)

  // Choose an arbitrary Ticket to use
  use_ticket = response.result.account_objects[0].TicketSequence
```

* [생성](https://xrpl.org/use-tickets.html#interactive-generate)
* [연결](https://xrpl.org/use-tickets.html#interactive-connect)
* [시퀀스 확인](https://xrpl.org/use-tickets.html#interactive-check\_sequence)
* [준비 및 서명](https://xrpl.org/use-tickets.html#interactive-prepare\_sign)
* [제출](https://xrpl.org/use-tickets.html#interactive-submit)
* [대기](https://xrpl.org/use-tickets.html#interactive-wait)
* [중지](https://xrpl.org/use-tickets.html#interactive-intermission)
* [티켓 확인](https://xrpl.org/use-tickets.html#interactive-check\_tickets)
* [발권 Tx 준비](https://xrpl.org/use-tickets.html#interactive-prepare\_ticketed\_tx)
* [발권 Tx 제출](https://xrpl.org/use-tickets.html#interactive-submit\_ticketed\_tx)

{% hint style="info" %}
Tip:

사용할 티켓이 남아 있는 한 여기에서 끝까지 단계를 반복할 수 있습니다!
{% endhint %}

## 8. Ticketed 트랜잭션준비(Prepare Ticketed Transaction) <a href="#8-prepare-ticketed-transaction" id="8-prepare-ticketed-transaction"></a>

이제 사용 가능한 티켓이 있으므로 이를 사용하는 트랜잭션을 준비할 수 있습니다.

원하는 모든 유형의 트랜잭션이 될 수 있습니다. 다음 예제에서는 ledger에 다른 설정이 필요하지 않으므로 no-op AccountSet 트랜잭션을 사용합니다. `Sequence` 필드를 0으로 설정하고 사용 가능한 티켓 중 하나의 `TicketSequence` 번호가 있는 티켓 시퀀스 필드를 포함합니다.

```javascript
// Prepare and Sign Ticketed Transaction -------------------------------------
  const prepared_t = await client.autofill({
    "TransactionType": "AccountSet",
    "Account": wallet.address,
    "TicketSequence": use_ticket,
    "LastLedgerSequence": null, // Never expire this transaction.
    "Sequence": 0
  })
  const signed_t = wallet.sign(prepared_t)
  console.log(`Prepared ticketed transaction ${signed_t.hash}`)
```

{% hint style="info" %}
Tip:&#x20;

TicketCreate 트랜잭션을 즉시 제출할 계획이 없다면 트랜잭션이 만료되지 않도록 `LastLedgerSequence`를 설정하지 않아야 합니다. 이 작업을 수행하는 방법은 라이브러리에 따라 다릅니다.

* **xrpl.js:**`LastLedgerSequence: null` 트랜잭션을 자동으로 채울 때 지정합니다.
* **`rippled`:**`LastLedgerSequence` 를준비된 설명서에서 생략합니다. 서버는 기본적으로 값을 제공하지 않습니다.
{% endhint %}

* [생성](https://xrpl.org/use-tickets.html#interactive-generate)
* [연결](https://xrpl.org/use-tickets.html#interactive-connect)
* [시퀀스 확인](https://xrpl.org/use-tickets.html#interactive-check\_sequence)
* [준비 및 서명](https://xrpl.org/use-tickets.html#interactive-prepare\_sign)
* [제출](https://xrpl.org/use-tickets.html#interactive-submit)
* [대기](https://xrpl.org/use-tickets.html#interactive-wait)
* [중지](https://xrpl.org/use-tickets.html#interactive-intermission)
* [티켓 확인](https://xrpl.org/use-tickets.html#interactive-check\_tickets)
* [발권 Tx 준비](https://xrpl.org/use-tickets.html#interactive-prepare\_ticketed\_tx)
* [발권 Tx 제출](https://xrpl.org/use-tickets.html#interactive-submit\_ticketed\_tx)

## 9. 발권 거래 제출(Submit Ticketed Transaction) <a href="#9-submit-ticketed-transaction" id="9-submit-ticketed-transaction"></a>

이전 단계에서 만든 서명된 트랜잭션 Blob을 제출합니다. 예를 들어:

```javascript
// Submit Ticketed Transaction -----------------------------------------------
  const tx_t = await client.submitAndWait(signed_t.tx_blob)
  console.log(tx_t)
```

* [생성](https://xrpl.org/use-tickets.html#interactive-generate)
* [연결](https://xrpl.org/use-tickets.html#interactive-connect)
* [시퀀스 확인](https://xrpl.org/use-tickets.html#interactive-check\_sequence)
* [준비 및 서명](https://xrpl.org/use-tickets.html#interactive-prepare\_sign)
* [제출](https://xrpl.org/use-tickets.html#interactive-submit)
* [대기](https://xrpl.org/use-tickets.html#interactive-wait)
* [중지](https://xrpl.org/use-tickets.html#interactive-intermission)
* [티켓 확인](https://xrpl.org/use-tickets.html#interactive-check\_tickets)
* [발권 Tx 준비](https://xrpl.org/use-tickets.html#interactive-prepare\_ticketed\_tx)
* [발권 Tx 제출](https://xrpl.org/use-tickets.html#interactive-submit\_ticketed\_tx)

## 10. 유효성 검사 대기(Wait for Validation) <a href="#10-wait-for-validation" id="10-wait-for-validation"></a>

티켓 거래는 순차 거래와 동일한 방식으로 합의 프로세스를 거칩니다.

* [생성](https://xrpl.org/use-tickets.html#interactive-generate)
* [연결](https://xrpl.org/use-tickets.html#interactive-connect)
* [시퀀스 확인](https://xrpl.org/use-tickets.html#interactive-check\_sequence)
* [준비 및 서명](https://xrpl.org/use-tickets.html#interactive-prepare\_sign)
* [제출](https://xrpl.org/use-tickets.html#interactive-submit)
* [대기](https://xrpl.org/use-tickets.html#interactive-wait)
* [중지](https://xrpl.org/use-tickets.html#interactive-intermission)
* [티켓 확인](https://xrpl.org/use-tickets.html#interactive-check\_tickets)
* [발권 Tx 준비](https://xrpl.org/use-tickets.html#interactive-prepare\_ticketed\_tx)
* [발권 Tx 제출](https://xrpl.org/use-tickets.html#interactive-submit\_ticketed\_tx)

| 거래 ID:                   | (없음)      |
| ------------------------ | --------- |
| 최신 검증 원장 인덱스:            | (연결되지 않은) |
| 제출 시점의 원장 인덱스:           | (제출하지 않은) |
| 거래 `LastLedgerSequence`: | (미준비)     |

## 다중 서명 사용(With Multi-Signing) <a href="#with-multi-signing" id="with-multi-signing"></a>

티켓의 주요 사용 사례 중 하나는 여러 다중 서명 트랜잭션 에 대한 서명을 병렬로 수집할 수 있는 것입니다. 티켓을 사용하면 어떤 것이 먼저 준비될지 걱정할 필요 없이 완전히 서명되고 준비가 되는 즉시 다중 서명 트랜잭션을 보낼 수 있습니다.

이 시나리오에서는 8단계 "발권 거래 준비" 가 약간 다릅니다. 한 번에 모두 준비하고 서명하는 대신 다중 서명된 트랜잭션을 보내는 단계를 따릅니다. 먼저 트랜잭션을 준비한 다음 신뢰할 수 있는 서명자 간에 순환하여 서명을 수집하고 마지막으로 서명을 최종적으로 다중 서명된 트랜잭션으로 결합합니다.

각각 다른 티켓을 사용하는 한 여러 잠재적 트랜잭션에 대해 이 작업을 병렬로 수행할 수 있습니다.

티켓의 주요 사용 사례 중 하나는 여러 개의 다중 서명 트랜잭션에 대한 서명을 병렬로 수집할 수 있는 것입니다. 티켓을 사용하면 여러 서명된 트랜잭션이 완전히 서명되고 준비되는 즉시 어떤 트랜잭션이 먼저 준비될지 걱정하지 않고 전송할 수 있습니다.&#x20;

이 시나리오에서는 8단계, "발권된 트랜잭션 준비"와 약간 다릅니다. 동시에 준비하고 서명하는 대신 다중 서명 트랜잭션을 전송하는 단계를 따릅니다. 먼저 트랜잭션을 준비한 다음 신뢰할 수 있는 서명자 간에 트랜잭션을 순환하여 서명을 수집하고 마지막으로 서명을 최종 다중 서명 트랜잭션에 결합합니다.

각각 다른 티켓을 사용하는 경우 여러 잠재적 트랜잭션에 대해 병렬로 이 작업을 수행할 수 있습니다.\
