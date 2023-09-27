# 트랜잭션 검열 감지

[![New in: rippled 1.2.0](https://img.shields.io/badge/New%20in-rippled%201.2.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.2.0)

XRP Ledger는 검열 저항성을 갖도록 설계되었습니다. 이 디자인을 지원하기 위해 XRP Ledger는 모든 <mark style="background-color:yellow;">rippled</mark> 서버에서 사용할 수 있는 자동 트랜잭션 검열 감지기를 제공하여 모든 참여자가 검열이 네트워크에 영향을 미치는지 확인할 수 있게 합니다.

<mark style="background-color:yellow;">rippled</mark> 서버가 네트워크와 동기화되는 동안, 검열 감지기는 최근 [컨센서스](../undefined/undefined.md) 라운드에서 수락되어 유효한 ledger에 포함되어야 했던 모든 트랜잭션을 추적합니다. 검열 감지기는 여러 라운드의 컨센서스 이후에도 유효성이 확인된 ledger에 포함되지 않은 트랜잭션을 발견하면 심각도가 높아지는 로그 메시지를 출력합니다.

## 작동 방식&#x20;

트랜잭션 검열 감지기의 작동 원리는 다음과 같습니다:

1. 검열 감지기는 서버의 초기 컨센서스 제안에 포함된 모든 트랜잭션을 추적 대상에 추가합니다.
2. 컨센서스 라운드가 종료되면, 검열 감지기는 검열된 ledger에 포함된 모든 트랜잭션을 추적에서 제거합니다.
3. 검열 감지기는 해당 트랜잭션이 15개의 ledger에 대한 추적기에 남아 있는 모든 트랜잭션에 대해 잠재적으로 검열된 트랜잭션으로 간주하여 로그에 [경고 메시지](undefined.md#undefined-1)를 출력합니다. 이 시점에서 트랜잭션이 추적기에 남아 있다는 것은 해당 트랜잭션이 15개의 컨센서스 라운드 이후에도 유효한 ledger에 포함되지 않았음을 의미합니다. 만약 해당 트랜잭션이 또 다른 15개의 ledger에 대해 추적기에 남아 있으면, 검열 감지기는 로그에 추가적인 경고 메시지를 출력합니다.\
   트랜잭션이 추적에 남아 있는 한, 검열 감지기는 최대 5개의 경고 메시지까지 매 15개의  ledger마다 로그에 경고 메시지를 출력합니다. 다섯 번째 경고 메시지 이후에는 검열 감지기가 마지막으로 에러 메시지를 로그에 출력한 후, 경고 및 에러 메시지를 더 이상 출력하지 않습니다.\
   rippeled 서버 로그에서 이러한 메시지를 확인하면, 다른 서버가 해당 트랜잭션을 포함하지 못하는 이유를 조사해야 합니다. 이때 악의적인 검열보다는 거짓 양성(무고한 버그) 가능성이 높다는 가정부터 시작하는 것이 좋습니다.

## 경고 메시지 예시&#x20;

메시지 트랜잭션 검열 감지기가 18851530 ledger에서 18851545  ledger까지 15개의 ledger에 걸쳐 트랜잭션 E08D6E9754025BA2534A78707605E0601F03ACE063687A0CA1BDDACFCD1698C7가 추적기에 남아 있는 경우, 이는 아래의 예시와 같이 검열 감지기에 의해 경고 메시지가 출력됩니다.

```
LedgerConsensus:WRN Potential Censorship: Eligible tx E08D6E9754025BA2534A78707605E0601F03ACE063687A0CA1BDDACFCD1698C7, which we are tracking since ledger 18851530 has not been included as of ledger 18851545.
```

## 오류 메시지 예시&#x20;

트랜잭션 E08D6E9754025 이후 트랜잭션 검열 감지에서 발생한 오류 메시지의 예입니다BA2534A78707605E0601F03ACE063687A0CA1BDDACFCD1698C7은 18851530 ledger에서 188551605 ledger까지 75개  ledger(15개  ledger 5세트) 동안 추적기에 남아 있었습니다.

```
LedgerConsensus:ERR Potential Censorship: Eligible tx E08D6E9754025BA2534A78707605E0601F03ACE063687A0CA1BDDACFCD1698C7, which we are tracking since ledger 18851530 has not been included as of ledger 18851605. Additional warnings suppressed.
```

## 잠재적인 거짓 양성

트랜잭션 검열 감지기는 특정 시나리오에서 거짓 양성을 출력할 수 있습니다. 이 경우, 거짓 양성이란 감지기가 15개 이상의  ledger에 걸쳐 추적에 남아 있는 트랜잭션을  무고한 이유로 플래그를 매긴 것을 의미합니다.

다음은 검열 감지기가 거짓 양성 메시지를 출력할 수 있는 시나리오입니다:

* 서버가 네트워크의 나머지와 다른 코드로 실행 중인 경우. 이는 서버가 트랜잭션을 다르게 적용하여 잘못된 거짓 양성을 초래할 수 있습니다. 이러한 종류의 거짓 양성은 드물지만, 일반적으로 호환되는 버전의 핵심 XRP Ledger 서버를 실행하는 것이 중요합니다.
* 서버가 네트워크와 동기화 되지 않았고 이를 아직 인식하지 못한 경우.
*   내 서버를 포함하여 네트워크에 있는 서버가 네트워크의 다른 서버에 트랜잭션을 일관성 없이 전달 되는 버그가 있을 수 있습니다.

    현재로서는 이러한 예기치 않은 동작을 일으키는 알려진 버그는 없습니다. 하지만 버그로 의심되는 영향을 확인하신 경우, [Ripple Bug Bounty](https://ripple.com/bug-bounty/) 프로그램에 신고해주시기 바랍니다.

### 참고 <a href="#see-also" id="see-also"></a>

* **Concepts:**
  * [Consensus Principles and Rules](https://xrpl.org/consensus-principles-and-rules.html)
  * [Transaction Queue](https://xrpl.org/transaction-queue.html)
* **Tutorials:**
  * [Reliable Transaction Submission](https://xrpl.org/reliable-transaction-submission.html)
  * [Understanding Log Messages](https://xrpl.org/understanding-log-messages.html)
* **References:**
  * [Transaction Results](https://xrpl.org/transaction-results.html)
