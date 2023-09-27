# 히스토리 샤딩 구성

히스토리 샤딩을 사용하면 각 서버가 전체 히스토리를 저장할 필요 없이 서버가 과거 XRP Ledger 데이터를 보존하는 데 기여할 수 있습니다. 기본적으로 rippled 서버는 히스토리 샤드를 저장하지 않습니다.

{% hint style="info" %}
Tip:

유효성 검증인 및 추적(또는 스톡) rippled 서버 모두 히스토리 샤드를 저장하도록 구성할 수 있지만, rippled은 해당 서버의 오버헤드를 줄이기 위해 유효성 검증인 rippled 서버에 샤드를 저장하도록 구성하지 않는 것을 권장합니다. 유효성 검증인을 실행하고 XRP Ledger 히스토리 저장에 기여하고 싶다면, rippled은 히스토리 샤딩이 활성화된 별도의 rippled 서버를 실행할 것을 권장합니다.
{% endhint %}

Ledger 히스토리의 샤드를 저장하도록 ripppled를 구성하려면 다음 단계를 완료하세요:

## 1. 유지할 샤드 수 결정

히스토리 샤드를 저장하도록 rippled 서버를 구성하기 전에 보관할 히스토리 샤드 수를 결정해야 하는데, 이는 대부분 샤드 저장소에 사용할 수 있는 디스크 공간의 양에 따라 결정됩니다. 이는 기본 ledger 저장소에 보관하는 히스토리의 양에도 영향을 줍니다. 샤드 저장소를 구성할 크기를 결정할 때는 다음 사항을 고려해야 합니다:

* ledger 저장소(\[node\_db] 구절에 의해 정의됨)는 히스토리 샤드 저장소와 분리되어 있습니다. ledger 저장소는 모든 서버에 필요하며, online\_delete 매개변수에서 사용할 수 있도록 유지할 ledger 수에 따라 정의되는 최근 히스토리 범위를 항상 포함합니다. (기본 구성은 가장 최근 2000개의 ledger을 저장합니다.)
  * ledger 저장소에 최소 215개의 ledger(32768개)을 보관하는 경우, ledger 저장소에서 샤드 저장소로 최근 히스토리 청크를 효율적으로 가져올 수 있습니다.
* 히스토리 샤드 저장소(\[shard\_db] 구절에 의해 정의됨)는 히스토리 샤드를 저장하는 데만 필요합니다. 히스토리 샤드를 저장하지 않는 서버에서는 구성 구절을 생략해야 합니다. 저장되는 샤드의 총 개수는 max\_historical\_shards 매개변수에 의해 정의되며, 서버는 이 개수 이하의 완전한 샤드만 저장하려고 시도합니다. 히스토리 샤드 저장소는 반드시 솔리드 스테이트 디스크 또는 이와 유사한 고속 미디어에 저장해야 합니다. 기존의 회전식 하드 디스크는 충분하지 않습니다.
* 샤드는 214개의 ledger(16384개)으로 구성되며, 샤드의 나이에 따라 약 200MB에서 4GB를 차지합니다. 오래된 샤드는 당시 XRP Ledger의 활동이 적었기 때문에 더 작습니다.
* 히스토리 샤드 저장소와 ledger 저장소는 반드시 다른 파일 경로에 저장해야 합니다. 원하는 경우 ledger 저장소와 내역 저장소를 다른 디스크나 파티션에 저장하도록 구성할 수 있습니다.
* ledger 저장소와 히스토리 샤드 저장소 모두에 전체 ledger 히스토리을 보유하는 것이 가능하지만 중복됩니다.
* 샤드 획득 시간, rippled 서버에 필요한 파일 핸들 수, 메모리 캐시 사용량은 샤드 크기에 직접적인 영향을 받습니다.
* \[historical\_shard\_paths] 구를 제공하여 이전 히스토리 샤드를 저장할 추가 경로를 지정할 수 있습니다. 이러한 경로는 사용 빈도가 낮은 데이터를 저장하기 때문에 다른 느린 디스크에 있을 수 있습니다. 가장 최근의 두 샤드(가장 큰 ledger 인덱스를 가진 샤드)는 항상 \[shard\_db] 구절에 지정된 경로에 저장됩니다. [![New in: rippled 1.7.0](https://img.shields.io/badge/New%20in-rippled%201.7.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/1.7.0)

## 2. rippled.cfg 편집

rippled.cfg 파일을 편집하여 \[shard\_db] 구절 및 선택적으로 \[historical\_shard\_paths] 구절을 추가합니다.

권장 설치는 기본적으로 구성 파일 /etc/opt/ripple/rippled.cfg를 사용합니다. 구성 파일을 저장할 수 있는 다른 위치로는 $HOME/.config/ripple/rippled.cfg(여기서 $HOME은 ripppled를 실행하는 사용자의 홈 디렉터리), $HOME/.local/ripple/rippled.cfg 또는 ripppled를 시작한 현재 작업 디렉터리 등이 있습니다.

다음 스니펫은 \[shard\_db] 구절의 예를 보여줍니다:

```
[shard_db]
path=/var/lib/rippled/db/shards/nudb
max_historical_shards=12

# Optional paths for shards other than the newest 2
[historical_shard_paths]
/mnt/disk1
/mnt/disk2
```

shard\_db]의 유형 필드는 생략할 수 있습니다. 있는 경우 반드시 NuDB여야 합니다.  [![New in: rippled 1.3.1](https://img.shields.io/badge/New%20in-rippled%201.3.1-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.3.1)

{% hint style="info" %}
Caution:

rippled가 샤드 저장 경로에서 잘못된 데이터 유형을 감지하면 시작에 실패할 수 있습니다. 샤드 저장소에 새 폴더를 사용해야 합니다. 이전에 RocksDB 샤드 저장소(rippled 1.2.x 이하)를 사용했다면 다른 경로를 사용하거나 RocksDB 샤드 데이터를 삭제하세요.
{% endhint %}

자세한 내용은 rippled.cfg 구성 예제에서 \[shard\_db] 예제를 참조하세요.

## 3. 서버 재시작

```
systemctl restart rippled
```

## 4. 샤드가 다운로드될 때까지 기다리기

서버가 네트워크에 동기화되면 자동으로 히스토리 샤드 다운로드를 시작하여 샤드 저장소의 사용 가능한 공간을 채웁니다. 샤드 저장소를 구성한 폴더에 어떤 폴더가 생성되었는지 확인하여 어떤 샤드가 다운로드되고 있는지 확인할 수 있습니다. (이 폴더는 rippled.cfg 파일에 있는 \[shard\_db] 구절의 경로 필드에 정의되어 있습니다.)\
\
이 폴더에는 서버에 있는 각 샤드에 대해 번호가 매겨진 폴더가 포함되어야 합니다. 언제든지 최대 하나의 폴더에 불완전함을 나타내는 control.txt 파일이 포함될 수 있습니다.\
\
[download\_shard](https://xrpl.org/download\_shard.html) 메소드를 사용하여 아카이브 파일에서 샤드를 다운로드하고 가져오도록 서버에 지시할 수 있습니다.\
\
서버와 해당 피어에서 사용 가능한 샤드를 나열하려면 crawl\_shards 메소드 또는 피어 크롤러를 사용할 수 있습니다.\
\
\
