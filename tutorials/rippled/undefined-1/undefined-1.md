# 로그 메시지 이해

다음 섹션에서는 rippled 서버의 디버그 로그에 나타날 수 있는 가장 일반적인 로그 메시지 유형 몇 가지와 이를 해석하는 방법에 대해 설명합니다.

이는 rippled 문제를 진단하는 데 중요한 단계입니다.

## 로그 메시지 구조

다음은 로그 파일의 형식을 보여줍니다:

```
2020-Jul-08 20:10:17.372178946 UTC Peer:WRN [236] onReadMessage from n9J2CP7hZypxDJ27ZSxoy4VjbaSgsCNaRRJtJkNJM5KMdGaLdRy7 at 197.27.127.136:53046: stream truncated
2020-Jul-08 20:11:13.582438263 UTC PeerFinder:ERR Logic testing     52.196.126.86:13308 with error, Connection timed out
2020-Jul-08 20:11:57.728448343 UTC Peer:WRN [242] onReadMessage from n9J2CP7hZypxDJ27ZSxoy4VjbaSgsCNaRRJtJkNJM5KMdGaLdRy7 at 197.27.127.136:53366: stream truncated
2020-Jul-08 20:12:12.075081020 UTC LoadMonitor:WRN Job: sweep run: 1172ms wait: 0ms
```

각 줄은 하나의 로그 항목을 나타내며, 다음 부분이 공백으로 구분된 순서대로 표시됩니다:

1. 로그 항목이 작성된 날짜(예: 2020-Jul-08).
2. 로그 항목이 작성된 시간(예: 20:12:12.075081020).
3. 표준 UTC 시간대 표(로그 날짜는 항상 UTC 기준입니다). [![New in: rippled 1.5.0](https://img.shields.io/badge/New%20in-rippled%201.5.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/1.5.0)
4. 로그 파티션 및 심각도(예: LoadMonitor:WRN).
5. 다음과 같은 로그 메시지(예: 작업: 스윕 실행: 1172ms wait: 0ms).

간단하게 하기 위해 이 페이지의 예제에서는 날짜, 시간 및 표준 시간대 표시를 생략했습니다.

## 충돌

로그에서 런타임 오류를 언급하는 메시지는 서버가 충돌했음을 나타낼 수 있습니다. 이러한 메시지는 일반적으로 다음 예제 중 하나와 같은 메시지로 시작됩니다:

```
Throw<std::runtime_error>
```

```
Terminating thread rippled: main: unhandled St13runtime_error
```

서버가 시작될 때 항상 충돌하는 경우, 가능하다면서버가 시작되지 않았을 때 확인하세요.

작동 중 또는 특정 명령의 결과로 서버가 무작위로 충돌하는 경우 최신 rippled 버전으로 업데이트했는지 확인하세요. 최신 버전을 사용 중인데도 서버가 계속 충돌하는 경우 다음을 확인하세요:

* 서버의 메모리가 부족한가요? 일부 시스템에서는 메모리 부족(OOM) 킬러 또는 다른 모니터 프로세스에 의해 rippled이 종료될 수 있습니다.
* 서버가 공유 환경에서 실행 중인 경우 다른 사용자나 관리자가 컴퓨터나 서비스를 다시 시작하게 하지는 않나요? 예를 들어, 일부 호스팅 제공업체는 공유 컴퓨터의 리소스를 장시간 대량으로 사용하는 모든 서비스를 자동으로 종료합니다.
* 서버가 rippled를 실행하기 위한 최소 요구 사항을 충족하나요? 프로덕션 서버에 대한 권장 사항은 어떻게 되나요?

위의 사항 중 어느 것도 해당되지 않는 경우, 해당 문제를 보안에 민감한 버그로 rippled에 신고해 주세요. rippled에서 충돌을 재현할 수 있다면 포상금을 받을 수 있습니다. 자세한 내용은 https://ripple.com/bug-bounty/를 참조하세요.

## 이미 유효성이 검증된 시퀀스 또는 과거

다음과 같은 로그 메시지는 서버가 다른 ledger 인덱스에 대한 유효성 검사를 순서대로 받지 못했음을 나타냅니다.

```
Validations:WRN Val for 2137ACEFC0D137EFA1D84C2524A39032802E4B74F93C130A289CD87C9C565011 trusted/full from nHUeUNSn3zce2xQZWNghQvd9WRH6FWEnCBKYVJu2vAizMxnXegfJ signing key n9KcRZYHLU9rhGVwB9e4wEMYsxXvUfgFxtmX25pc1QPNgweqzQf5 already validated sequence at or past 12133663 src=1
```

이 유형의 메시지가 가끔씩 발생하는 것은 일반적으로 문제를 나타내지 않습니다. 이러한 유형의 메시지가 동일한 전송 유효성 검사기에서 자주 발생하는 경우 다음 중 하나(가능성이 가장 높은 순서에서 가장 낮은 순서로)를 포함하여 문제를 나타낼 수 있습니다:

* 메시지를 작성하는 서버에 네트워크 문제가 있습니다.
* 메시지에 설명된 유효성 검사기에 네트워크 문제가 있습니다.
* 메시지에 설명된 유효성 검사기가 악의적으로 작동하고 있습니다.

## async\_send 실패

다음 로그 메시지는 StatsD 내보내기가 실패했음을 나타냅니다:

```
Collector:ERR async_send failed: Connection refused
```

이는 다음을 의미할 수 있습니다:

* StatsD 구성에 잘못된 IP 주소 또는 포트가 있습니다.
* 내보내려는 StatsD 서버가 다운되었거나 rippled된 서버에서 액세스할 수 없습니다.

rippled 구성 파일에서 \[insight] 구절을 확인하고 rippled리드 서버에서 StatsD 서버로 네트워크가 연결되어 있는지 확인하세요.

이 오류는 rippled 서버에 다른 영향을 미치지 않으며, StatsD 메트릭 전송을 제외하고는 정상적으로 계속 작동해야 합니다.

## 업그레이드 확인

다음 메시지는 서버가 신뢰할 수 있는 유효성 검사기의 60% 이상보다 오래된 소프트웨어 버전을 실행하고 있음을 감지했음을 나타냅니다:

```
LedgerMaster:ERR Check for upgrade: A majority of trusted validators are running a newer version.
```

이는 엄밀히 말하면 문제가 되지 않지만 오래된 서버 버전은 수정이 차단될 가능성이 높습니다. rippled를 최신의 안정적인 버전으로 업데이트해야 합니다. (devnet에 연결되어 있는 경우, 최신 나이틀리 버전으로 업데이트하세요.)

## 피어에 의한 연결 재설정

다음 로그 메시지는 피어 rippled 서버가 연결을 종료했음을 나타냅니다:

```
Peer:WRN [012] onReadMessage: Connection reset by peer
```

모든 P2P 네트워크에서 때때로 연결이 끊어지는 것은 정상입니다. **가끔 이런 종류의 메시지가 표시된다고 해서 문제가 있는 것은 아닙니다.**

이러한 메시지가 같은 시간에 대량으로 수신되면 다음과 같은 문제가 있을 수 있습니다:

* 하나 이상의 특정 피어에 대한 인터넷 연결이 끊어졌습니다.
* 내 서버의 요청으로 인해 피어에 과부하가 걸려 피어에서 내 서버 연결을 끊었을 수 있습니다.

## 잔액이 드롭 임계값 이상인 소비자 항목이 드롭됨

다음 로그 메시지는 비율 제한으로 인해 서버의 퍼블릭 API에 대한 클라이언트가 드롭되었음을 나타냅니다:

```
Resource:WRN Consumer entry 169.55.164.21 dropped with balance 15970 at or above drop threshold 15000
```

이 항목에는 요금 제한을 초과한 클라이언트의 IP 주소와 클라이언트의 '잔액'이 포함되어 있으며, 이는 클라이언트가 API를 사용 중인 속도를 추정하는 점수입니다. 클라이언트를 삭제하는 임계값은 15000 점으로 하드코딩되어 있습니다.

동일한 IP 주소로부터 메시지가 자주 수신되는 경우 해당 IP 주소를 네트워크에서 차단하여 서버의 공용 API에 대한 부하를 줄일 수 있습니다. (예를 들어 방화벽을 구성하여 해당 IP 주소를 차단할 수 있습니다.)

자체 서버에서 속도 제한으로 인해 전송이 중단되는 것을 방지하려면 관리자로 연결하세요.

## &#x20;ledger용 InboundLedger 11 시간 초과

```
InboundLedger:WRN 11 timeouts for ledger 8265938
```

이는 서버가 피어에게 특정 ledger 데이터를 요청하는 데 문제가 있음을 나타냅니다. ledger의 인덱스가 server\_info 메소드에서 보고한 가장 최근에 검증된 ledger의 인덱스보다 훨씬 낮다면, 서버가 히스토리 샤드를 다운로드하고 있다는 의미일 수 있습니다.

이는 엄밀히 말하면 문제가 되지 않지만, ledger 기록을 더 빨리 얻으려면 \[ips\_fixed] 구성 구문을 추가하거나 편집하고 서버를 다시 시작하여 전체 기록이 있는 피어에 연결하도록 rippled를 구성할 수 있습니다. 예를 들어, 항상 rippled의 풀 히스토리 서버 중 하나에 연결하도록 설정할 수 있습니다:

```
[ips_fixed]
s2.ripple.com 51235
```

## InboundLedger Want 해시

다음과 같은 로그 메시지는 서버가 다른 서버에서 ledger 데이터를 요청하고 있음을 나타냅니다:

```
InboundLedger:WRN Want: 5AE53B5E39E6388DBACD0959E5F5A0FCAF0E0DCBA45D9AB15120E8CDD21E019B
```

서버가 히스토리 샤드를 동기화, 백필 또는 다운로드하는 경우 이는 정상입니다.

## LoadMonitor 작업

함수를 실행하는 데 시간이 오래 걸리는 경우(이 예제에서는 11초 이상) 다음과 같은 메시지가 표시됩니다:

```
2018-Aug-28 22:56:36.180827973 LoadMonitor:WRN Job: gotFetchPack run: 11566ms wait: 0ms
```

작업이 실행 대기 시간이 길어질 때(이 예제에서는 11초 이상) 다음과 같은 유사한 메시지가 표시됩니다:

```
LoadMonitor:WRN Job: processLedgerData run: 0ms wait: 11566ms
LoadMonitor:WRN Job: AcquisitionDone run: 0ms wait: 11566ms
LoadMonitor:WRN Job: processLedgerData run: 0ms wait: 11566ms
LoadMonitor:WRN Job: AcquisitionDone run: 0ms wait: 11566ms
```

오래 실행되는 작업으로 인해 다른 작업이 완료될 때까지 오래 대기하는 경우 이 두 가지 유형의 메시지가 함께 발생하는 경우가 많습니다.

서버를 시작한 후 처음 몇 분 동안 이러한 유형의 메시지가 여러 개 표시되는 것은 **정상**입니다.

서버를 시작한 후 5분 이상 메시지가 계속 표시되는 경우, 특히 실행 시간이 1,000밀리초를 훨씬 넘는 경우에는 서버에 디스크 I/O, RAM 또는 CPU가 충분하지 않다는 의미일 수 있습니다. 이는 하드웨어가 충분히 강력하지 않거나 동일한 하드웨어에서 실행 중인 다른 프로세스가 rippled와 리소스를 놓고 경쟁하기 때문에 발생할 수 있습니다. (rippled와 리소스를 놓고 경쟁할 수 있는 다른 프로세스의 예로는 예약된 백업, 바이러스 스캐너, 주기적인 데이터베이스 클리너 등이 있습니다.)

또 다른 가능한 원인은 회전식 하드 디스크에서 NuDB를 사용하려고 할 때입니다. NuDB는 SSD(솔리드 스테이트 드라이브)에서만 사용해야 합니다. rippled는 rippled의 데이터베이스에 항상 SSD 스토리지를 사용할 것을 권장하지만, 회전식 디스크에서 RocksDB를 사용하여 rippled을 성공적으로 실행할 수 있습니다. 회전식 디스크를 사용하는 경우, \[node\_db]와 \[shard\_db]\(있는 경우)가 모두 RocksDB를 사용하도록 구성되어 있는지 확인하세요. 예를 들어:

```
[node_db]
type=RocksDB
# ... more config omitted

[shard_db]
type=RocksDB
```

## fetch pack에 대한 해시 없음

다음과 같은 메시지는 히스토리 샤딩을 위해 히스토리 ledger을 다운로드할 때 rippled v1.1.0 이하 버전에서 발생하는 버그로 인해 발생합니다:

```
2018-Aug-28 22:56:21.397076850 LedgerMaster:ERR No hash for fetch pack. Missing Index 7159808
```

이러한 메시지는 안전하게 무시해도 됩니다.

## 삭제되지 않음

온라인 삭제가 중단되면 다음과 같은 메시지가 표시됩니다:

```
SHAMapStore:WRN Not deleting. state: syncing. age 25s
```

상태는 서버 상태를 나타냅니다. 나이는 마지막으로 유효성이 검사된 ledger가 닫힌 후 몇 초가 지났는지를 나타냅니다. (마지막으로 유효성이 검사된 ledger의 정상 사용 기간은 7초 이하입니다.)

시작하는 동안에는 이러한 메시지는 정상이며 안전하게 무시해도 됩니다. 다른 경우에는 일반적으로 이와 같은 메시지는 서버가 다른 모든 작업과 동시에 온라인 삭제를 실행하기 위한 시스템 요구 사항, 특히 디스크 I/O를 충족하지 못한다는 것을 나타냅니다.

## 검열 가능성

XRP Ledger가 잠재적인 트랜잭션 검열을 감지하면 다음과 같은 로그 메시지가 발행됩니다. 이러한 로그 메시지와 트랜잭션 검열 감지기에 대한 자세한 내용은 트랜잭션 검열 감지를 참조하세요.

**경고 메시지**

```
LedgerConsensus:WRN Potential Censorship: Eligible tx E08D6E9754025BA2534A78707605E0601F03ACE063687A0CA1BDDACFCD1698C7, which we are tracking since ledger 18851530 has not been included as of ledger 18851545.
```

&#x20;**에러 메시지**

```
LedgerConsensus:ERR Potential Censorship: Eligible tx E08D6E9754025BA2534A78707605E0601F03ACE063687A0CA1BDDACFCD1698C7, which we are tracking since ledger 18851530 has not been included as of ledger 18851605. Additional warnings suppressed.
```

## validatedSeq 회전

이 메시지는 온라인 삭제가 실행되기 시작했음을 나타냅니다:

```
SHAMapStore:WRN rotating  validatedSeq 54635511 lastRotated 54635255 deleteInterval 256 canDelete_ 4294967295
```

이 로그 메시지는 정상이며 온라인 삭제가 예상대로 작동하고 있음을 나타냅니다.

로그 메시지에는 현재 온라인 삭제 실행을 설명하는 값이 포함되어 있습니다. 각 키워드는 바로 뒤에 나오는 값에 해당합니다:

| Keyword          | Value                                                               | Description                                                                                                                                                                                                   |
| ---------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `validatedSeq`   | [Ledger Index](https://xrpl.org/basic-data-types.html#ledger-index) | The current validated ledger version.                                                                                                                                                                         |
| `lastRotated`    | [Ledger Index](https://xrpl.org/basic-data-types.html#ledger-index) | The end of the ledger range in the ["old" (read-only) database](https://xrpl.org/online-deletion.html#how-it-works). Online deletion deletes this ledger version and earlier.                                 |
| `deleteInterval` | Number                                                              | How many ledger versions to keep after online deletion. The [`online_delete` setting](https://xrpl.org/online-deletion.html#configuration) controls this value.                                               |
| `canDelete_`     | [Ledger Index](https://xrpl.org/basic-data-types.html#ledger-index) | The newest ledger version that the server is allowed to delete, if using [advisory deletion](https://xrpl.org/online-deletion.html#advisory-deletion). If not using advisory deletion, this value is ignored. |

온라인 삭제가 완료되면 다음과 같은 로그 메시지가 기록됩니다:

```
SHAMapStore:WRN finished rotation 54635511
```

메시지 끝에 있는 숫자는 온라인 삭제가 시작된 시점의 유효성 검사된 ledger의 ledger 인덱스로, '회전' 메시지의 validatedSeq 값과 일치합니다. 이 값은 다음에 온라인 삭제가 실행될 때 lastRotated 값이 됩니다.

온라인 삭제를 실행하는 동안 서버가 동기화되지 않으면 온라인 삭제가 중단되고 "회전 완료" 메시지 대신 "삭제 중이 아님" 로그 메시지가 기록됩니다.

## 샤드: 해당 파일 또는 디렉터리 없음

히스토리 샤딩을 사용하도록 설정한 경우 다음과 같은 로그 메시지가 발생할 수 있습니다:

```
ShardStore:ERR shard 1804: No such file or directory
```

이는 서버가 새 기록 샤드 수집을 시작하려고 했지만 기본 파일 시스템에 쓸 수 없음을 나타냅니다. 가능한 원인은 다음과 같습니다:

* 저장 미디어의 하드웨어 오류.
* 파일 시스템이 마운트 해제.
* 샤드 폴더가 삭제.

{% hint style="info" %}
Tip:

일반적으로 서비스가 중지되면 rippled의 데이터베이스 파일을 삭제하는 것이 안전하지만, 서버가 실행 중일 때는 절대로 삭제해서는 안 됩니다.
{% endhint %}

## 조상 해시를 확인할 수 없음

서버가 피어로부터 유효성 검사 메시지를 받았는데 서버가 구축 중인 부모 ledger 버전을 알지 못할 때 다음과 같은 로그 메시지가 발생합니다. 이는 서버가 나머지 네트워크와 동기화되지 않은 경우에 발생할 수 있습니다:

```
Validations:WRN Unable to determine hash of ancestor seq=3 from ledger hash=00B1E512EF558F2FD9A0A6C263B3D922297F26A55AEB56A009341A22895B516E seq=12133675
```

서버가 시작된 후 처음 5\~15분 동안은 나머지 네트워크와 동기화되지 않아 이와 같은 메시지를 인쇄하는 것이 정상입니다. 서버가 시작되고 한참 후에 이러한 메시지를 출력한다면 문제가 있는 것일 수 있습니다. 일반적인 원인으로는 불안정한 네트워크 연결과 불충분한 하드웨어 사양이 있습니다. 동일한 하드웨어에서 실행 중인 다른 프로세스가 rippled와 리소스를 놓고 경쟁하는 경우에도 이 문제가 발생할 수 있습니다. (rippled와 리소스를 놓고 경쟁할 수 있는 다른 프로세스의 예로는 예약된 백업, 바이러스 스캐너, 주기적인 데이터베이스 클리너 등이 있습니다.)

## 구성 파일의 \[veto\_amendments] 섹션 무시됨

rippled.cfg 파일에 레거시 \[veto\_amendments] 구절이 포함된 경우 다음과 같은 로그 메시지가 발생합니다. 버전 1.7.0 이상에서 서버를 처음 시작할 때는 해당 구절을 읽어 수정안 투표를 설정하지만, 이후 다시 시작할 때는 \[amendments] 및 \[veto\_amendments] 구절을 무시하고 대신 이 메시지를 인쇄합니다.

```
Amendments:WRN [veto_amendments] section in config file ignored in favor of data in db/wallet.db.
```

이 오류를 해결하려면 구성 파일에서 \[amendments] 및 \[veto\_amendments] 구절을 제거하세요. 자세한 내용은 수정안 투표를 참조하세요.

## 공개 중에 변경된 컨센서스 보기

서버가 나머지 네트워크와 동기화되지 않은 경우 다음과 같은 로그 메시지가 발생합니다:

```
LedgerConsensus:WRN View of consensus changed during open status=open,  mode=proposing
LedgerConsensus:WRN 96A8DF9ECF5E9D087BAE9DDDE38C197D3C1C6FB842C7BB770F8929E56CC71661 to 00B1E512EF558F2FD9A0A6C263B3D922297F26A55AEB56A009341A22895B516E
LedgerConsensus:WRN {"accepted":true,"account_hash":"89A821400087101F1BF2D2B912C6A9F2788CC715590E8FA5710F2D10BF5E3C03","close_flags":0,"close_time":588812130,"close_time_human":"2018-Aug-28 22:55:30.000000000","close_time_resolution":30,"closed":true,"hash":"96A8DF9ECF5E9D087BAE9DDDE38C197D3C1C6FB842C7BB770F8929E56CC71661","ledger_hash":"96A8DF9ECF5E9D087BAE9DDDE38C197D3C1C6FB842C7BB770F8929E56CC71661","ledger_index":"3","parent_close_time":588812070,"parent_hash":"5F5CB224644F080BC8E1CC10E126D62E9D7F9BE1C64AD0565881E99E3F64688A","seqNum":"3","totalCoins":"100000000000000000","total_coins":"100000000000000000","transaction_hash":"0000000000000000000000000000000000000000000000000000000000000000"}
```

서버가 시작된 후 처음 5\~15분 동안은 나머지 네트워크와 동기화되지 않아 이와 같은 메시지가 출력되는 것이 정상입니다. 서버가 시작되고 한참 후에 이러한 메시지를 출력한다면 문제가 있는 것일 수 있습니다. 일반적인 원인으로는 불안정한 네트워크 연결과 불충분한 하드웨어 사양이 있습니다. 동일한 하드웨어에서 실행 중인 다른 프로세스가 rippled와 리소스를 놓고 경쟁하는 경우에도 이 문제가 발생할 수 있습니다. (rippled와 리소스를 놓고 경쟁할 수 있는 다른 프로세스의 예로는 예약된 백업, 바이러스 스캐너, 주기적인 데이터베이스 클리너 등이 있습니다.)

## 컨센서스 ledger에서 실행되지 않습니다.

```
NetworkOPs:WRN We are not running on the consensus ledger
```

서버가 시작된 후 처음 5\~15분 동안은 네트워크의 나머지 부분과 동기화되지 않아 이와 같은 메시지를 출력하는 것이 정상입니다. 서버를 시작한 지 한참 후에 이러한 메시지가 출력된다면 문제가 있는 것일 수 있습니다. 일반적인 원인으로는 불안정한 네트워크 연결과 불충분한 하드웨어 사양이 있습니다. 동일한 하드웨어에서 실행 중인 다른 프로세스가 rippled와 리소스를 놓고 경쟁하는 경우에도 이 문제가 발생할 수 있습니다. (rippled와 리소스를 놓고 경쟁할 수 있는 다른 프로세스의 예로는 예약된 백업, 바이러스 스캐너, 주기적인 데이터베이스 클리너 등이 있습니다.)
