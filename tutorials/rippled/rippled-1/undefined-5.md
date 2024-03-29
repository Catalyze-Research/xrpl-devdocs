# 전체 히스토리 구성

기본 구성에서 rippled 서버는 새로운 ledger 버전을 사용할 수 있게 되면 오래된 XRP Ledger 상태 및 트랜잭션 히스토리를 자동으로 삭제합니다. 이는 현재 상태를 파악하고 트랜잭션을 처리하는 데 이전 히스토리가 필요하지 않은 대부분의 서버에 충분합니다. 그러나 일부 서버가 가능한 한 많은 XRP Ledger의 히스토리를 제공한다면 네트워크에 유용할 수 있습니다.

## 경고

전체 트랜잭션 히스토리를 저장하는 데는 비용이 많이 듭니다. 2020년 11월 10일 기준, XRP Ledger의 전체 내역은 약 14테라바이트의 디스크 공간을 차지하며, 적절한 서버 성능을 위해 고속 솔리드 스테이트 디스크 드라이브에 전적으로 저장해야 합니다. 이렇게 많은 양의 솔리드 스테이트 스토리지는 저렴하지 않으며, 저장해야 하는 총 히스토리의 양은 하루에 약 12GB씩 증가합니다.

P2P 네트워크에서 전체 트랜잭션 히스토리를 수집하는 데는 오랜 시간(수개월)이 걸리며, 서버에 새로운 ledger 진행 상황을 따라잡으면서 이전 히스토리를 수집할 수 있는 충분한 시스템 및 네트워크 리소스가 있어야 합니다. ledger 히스토리를 더 빨리 확보하려면 이미 많은 양의 히스토리를 다운로드한 다른 서버 운영자를 찾아 데이터베이스 덤프를 제공하거나 적어도 서버가 명시적으로 장시간 피어링하여 히스토리를 확보할 수 있도록 허용하는 것이 좋습니다. 서버는 파일에서 장부 히스토리를 로드하고 가져온 히스토리 장부의 무결성을 확인할 수 있습니다.

네트워크에 참여하거나 트랜잭션의 유효성을 검사하거나 네트워크의 현재 상태를 알기 위해 전체 히스토리 서버가 필요하지 않습니다. 전체 내역은 과거에 발생한 트랜잭션의 결과 또는 과거 특정 시점의 ledger 상태를 파악하는 데에만 유용합니다. 이러한 정보를 얻으려면 필요한 히스토리를 보유한 다른 서버에 의존해야 합니다.

전체 히스토리를 저장하지 않고 XRP Ledger 네트워크의 히스토리를 저장하는 데 기여하고 싶다면, 무작위로 선택된 Ledger 히스토리 청크를 저장하도록 히스토리 샤딩을 구성할 수 있습니다.

## 구성 단계

전체 트랜잭션 히스토리를 수집하고 저장하도록 서버를 구성하려면 다음 단계를 완료하세요:

1. rippled 서버가 실행 중이면 중지합니다.

```
$ sudo systemctl stop rippled
```

2. 서버 구성 파일의 \[node\_db] 구절에서 online\_delete 및 advisory\_delete 설정을 제거(또는 주석 처리)하고, 아직 변경하지 않은 경우 유형을 NuDB로 변경합니다:

```
[node_db]
type=NuDB
path=/var/lib/rippled/db/nudb
#online_delete=2000
#advisory_delete=0
```

전체 히스토리 서버에서는 데이터베이스가 그렇게 큰 경우 RocksDB가 너무 많은 RAM을 필요로 하므로 ledger 저장소에 NuDB를 사용해야 합니다. 자세한 내용은 용량 계획을 참조하세요. 다음과 같은 성능 관련 구성 옵션은 RocksDB에만 적용되므로 기본 \[node\_db] 구절에서 제거할 수 있습니다: open\_files, filter\_bits, cache\_mb, file\_size\_mb 및 file\_size\_mult.

{% hint style="info" %}
Caution:

RocksDB로 이미 다운로드한 히스토리가 있는 경우, NuDB로 전환할 때 해당 데이터를 삭제하거나 구성 파일에서 데이터베이스의 경로를 변경해야 합니다. \[node\_db] 구절의 경로 필드와 \[database\_path]\(SQLite 데이터베이스) 설정을 모두 변경해야 합니다. 그렇지 않으면 서버가 시작되지 않을 수 있습니다.
{% endhint %}

권장 설치는 기본적으로 구성 파일 /etc/opt/ripple/rippled.cfg를 사용합니다. 구성 파일을 저장할 수 있는 다른 위치로는 $HOME/.config/ripple/rippled.cfg(여기서 $HOME은 ripppled를 실행하는 사용자의 홈 디렉터리), $HOME/.local/ripple/rippled.cfg 또는 ripppled를 시작한 현재 작업 디렉터리 등이 있습니다.

3. 서버 구성 파일의 \[ledger\_history] 구절을 전체로 설정합니다:

```
[ledger_history]
full
```

4. 전체 히스토리를 사용할 수 있는 하나 이상의 서버와 명시적으로 피어링하도록 서버 구성 파일의 \[ips\_fixed] 구절을 설정합니다.

```
[ips_fixed]
169.55.164.20 51235
50.22.123.215 51235
```

서버는 직접 피어 중 하나에 사용 가능한 데이터가 있는 경우에만 P2P 네트워크에서 히스토리 데이터를 다운로드할 수 있습니다. 전체 히스토리를 다운로드할 수 있도록 하는 가장 쉬운 방법은 이미 전체 히스토리가 있는 서버와 피어링하는 것입니다.

{% hint style="info" %}
Tip:

rippled는 전체 내역 서버 풀을 공개적으로 제공합니다. s2.ripple.com 도메인을 몇 번 확인하여 이러한 서버의 IP 주소를 얻을 수 있습니다. rippled는는 이러한 서버를 공개 서비스로 제공하므로 다른 서버와 피어링할 수 있는 서버가 제한되어 있으며 이를 남용할 경우 차단될 수 있다는 점에 유의하시기 바랍니다.
{% endhint %}

5. 다른 전체 히스토리 서버의 데이터베이스 덤프가 있는 경우, 가져올 데이터를 가리키도록 서버 구성 파일의 \[import\_db] 구절을 설정하세요. (그렇지 않으면 이 단계를 건너뛰세요.)

```
[import_db]
type=NuDB
path=/tmp/full_history_dump/
```

6. 이전에 ripppled를 실행한 서버의 기존 데이터베이스 파일이 있는 경우 제거합니다.\
   온라인 삭제를 비활성화하면 서버에서 온라인 삭제가 활성화된 상태에서 다운로드한 모든 데이터를 무시하므로 디스크 공간을 정리하는 것이 좋습니다. 예를 들어:

```
rm -r /var/lib/rippled/db/*
```

{% hint style="info" %}
Warning:

폴더를 삭제하기 전에 해당 폴더에 보관하려는 파일이 없는지 확인하세요. 일반적으로 rippled 서버의 데이터베이스 파일을 모두 삭제해도 안전하지만, 구성된 데이터베이스 폴더가 rippled 데이터베이스 이외의 다른 용도로 사용되지 않는 경우에만 삭제해야 합니다.
{% endhint %}

7. rippled 서버를 시작하여 데이터베이스 덤프가 있는 경우 가져옵니다:\
   \[import\_db]에 로드할 데이터베이스 덤프가 구성되어 있는 경우, 서버를 명시적으로 시작하고 --import 커맨드라인 옵션을 포함하세요:

```
$ /opt/ripple/bin/rippled --conf /etc/opt/ripple/rippled.cfg --import
```

대용량 데이터베이스 덤프를 가져오는 데는 몇 분 또는 몇 시간이 걸릴 수 있습니다. 이 시간 동안 서버가 완전히 시작되고 네트워크와 동기화되지 않습니다. 가져오기 상태를 보려면 서버 로그를 확인하세요.\
데이터베이스 덤프를 가져오지 않는 경우에는 서버를 정상적으로 시작하세요:

```
$ sudo systemctl start rippled
```

8. 서버의 구성 파일에 \[import\_db] 구절을 추가한 경우 가져오기가 완료된 후 제거하세요.\
   그렇지 않으면 서버가 다음에 다시 시작될 때 동일한 데이터를 다시 가져오려고 시도할 수 있습니다.
9. server\_info 메소드를 사용하여 서버의 사용 가능한 히스토리를 모니터링합니다.\
   complete\_ledgers 필드에 보고되는 사용 가능한 장부의 범위는 시간이 지남에 따라 증가합니다.\
   프로덕션 XRP Ledger의 히스토리에서 가장 먼저 사용 가능한 ledger 버전은 ledger 인덱스 32570입니다. 당시 서버의 버그로 인해 처음 2주 정도의 ledger 히스토리가 손실되었습니다. 테스트넷과 다른 체인은 일반적으로 ledger 인덱스 1로 거슬러 올라가는 히스토리를 가지고 있습니다.
