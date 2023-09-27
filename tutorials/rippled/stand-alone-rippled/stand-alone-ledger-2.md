# stand-alone 모드에서 ledger 진행하기

stand-alone 모드에서 rippled는 P2P 네트워크의 다른 구성원과 통신하거나 컨센서스 프로세스에 참여하지 않습니다. 이 모드에서는 컨센서스 프로세스가 없으므로 ledger\_accept 메소드를 사용하여 수동으로 ledger 인덱스를 진행해야 합니다:

```
rippled ledger_accept --conf=/path/to/rippled.cfg
```

stand-alone 모드에서 rippled는 "폐쇄" ledger 버전과 "검증된" ledger 버전을 구분하지 않습니다. (차이점에 대한 자세한 내용은 XRP Ledger 컨센서스 프로세스를 참조하세요.)

rippled는 ledger를 닫을 때마다 결정론적이지만 hard-to-game 알고리즘에 따라 트랜잭션을 재정렬합니다. (트랜잭션이 네트워크의 다른 부분에 다른 순서로 도착할 수 있기 때문에 이는 컨센서스의 중요한 부분입니다.) stand-alone 모드에서 rippled를 사용하는 경우, 다른 주소의 트랜잭션 결과에 따라 달라지는 트랜잭션을 제출하기 전에 ledger를 수동으로 진행해야 합니다. 그렇지 않으면 ledger가 닫힐 때 두 트랜잭션이 역순으로 실행될 수 있습니다.

{% hint style="info" %}
Note:

rippled는 동일한 주소의 트랜잭션을 시퀀스 번호 오름차순으로 정렬하므로 단일 주소에서 단일 ledger로 여러 트랜잭션을 안전하게 제출할 수 있습니다.
{% endhint %}
