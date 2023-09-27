# stand-alone 모드에서 저장된 ledger 불러오기

이전에 디스크에 저장된 과거 ledger 버전을 사용하여 stand-alone 모드에서 rippled 서버를 시작할 수 있습니다. 예를 들어, rippled 서버가 이전에 프로덕션 mainnet, testnet 또는 devnet을 포함한 XRP Ledger P2P 네트워크와 동기화되었다면, 서버에서 사용 가능한 모든 ledger 버전을 로드할 수 있습니다.

과거 ledger 버전을 로드하면 ledger를 "재생"하여 트랜잭션이 네트워크 규칙에 따라 처리되었는지 확인하거나 다른 수정이 활성화된 트랜잭션 세트의 처리 결과를 비교하는 데 유용합니다. 드물지만 XRP Ledger의 컨센서스 메커니즘에 대한 공격으로 인해 공유 ledger 상태에 원치 않는 영향이 발생하는 경우, 검증인 컨센서스는 이 프로세스를 시작으로 알려진 양호한 네트워크 상태로 "롤백"할 수 있습니다.

{% hint style="info" %}
Caution:

rippled가 최신 버전으로 업데이트되면 수정 사항은 폐기되고 ledger의 핵심 기능이 되어 트랜잭션 처리 방식에 영향을 줄 수 있습니다. 역사적으로 정확한 결과를 얻으려면 트랜잭션이 처리된 rippled 버전을 사용하여 ledger를 다시 재생해야 합니다.
{% endhint %}

## 1. rippled를 정상적으로 시작합니다.

기존 ledger을 로드하려면 먼저 네트워크에서 해당 ledger을 검색해야 합니다. 정상적으로 온라인 모드에서 rippled를 시작합니다:

```
rippled --conf=/path/to/rippled.cfg
```

## 2. rippled가 동기화될 때까지 기다립니다.

server\_info 메소드를 사용하여 네트워크와 관련된 서버의 상태를 확인합니다. server\_state 값에 다음 값 중 하나가 표시되면 서버가 동기화된 것입니다:

* `full`
* `proposing`
* `validating`

자세한 내용은 가능한 서버 상태를 참조하세요.

## 3. (선택 사항) 특정 ledger 버전을 검색합니다.

가장 최근의 ledger만 원하는 경우 이 단계를 건너뛸 수 있습니다.

특정 과거 ledger 버전을 불러오려면 ledger\_request 메소드를 사용해 rippled에서 해당 버전을 가져오도록 합니다. rippled에 아직 해당 ledger 버전이 없는 경우, ledger 검색이 완료될 때까지 ledger\_request 명령을 여러 번 실행해야 할 수 있습니다.

특정 과거 ledger 버전을 재생하려면 재생할 ledger 버전과 그 이전 ledger 버전을 모두 가져와야 합니다. (이전 ledger 버전은 재생하는 ledger 버전에서 설명하는 변경 사항을 적용하는 초기 상태를 설정합니다.)

## 4. rippled 종료.

중지 메소드를 사용합니다:

```
rippled stop --conf=/path/to/rippled.cfg
```

## 5. stand-alone 모드에서 rippled를 시작합니다.

가장 최신 ledger 버전을 로드하려면 -a 및 --load 옵션을 사용하여 서버를 시작합니다:

```
rippled -a --load --conf=/path/to/rippled.cfg
```

특정 기록 ledger를 로드하려면 --load 매개변수와 함께 --ledger 매개변수를 사용하여 서버를 시작하고, 로드할 ledger 인덱스 또는 ledger 버전의 식별 해시를 제공합니다:

```
rippled -a --load --ledger 19860944 --conf=/path/to/rippled.cfg
```

이렇게 하면 서버가 시작될 때 저장된 ledger 버전이 서버의 "현재"(열려 있는) ledger이 됩니다.

stand-alone 모드에서 rippled를 시작할 때 사용할 수 있는 옵션에 대한 자세한 내용은 커맨드라인 사용법을 참조하세요: stand-alone 모드 옵션을 참조하세요.

## 6. ledger를 수동으로 진행합니다.

저장된 ledger을 처리하려면 ledger\_accept 메소드를 사용하여 수동으로 진행합니다:

```
rippled ledger_accept --conf=/path/to/rippled.cfg
```

이렇게 하면 트랜잭션이 표준 순서대로 처리되어 폐쇄 ledger가 만들어집니다.
