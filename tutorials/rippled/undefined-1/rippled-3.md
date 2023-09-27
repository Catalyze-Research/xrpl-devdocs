# rippled 서버가 시작되지 않음

이 페이지에서는 rippled 서버가 시작되지 않는 가능한 이유와 해결 방법에 대해 설명합니다.

이 지침은 지원되는 플랫폼에 rippled를 설치했다고 가정합니다.

## 파일 설명자 제한

일부 Linux 버전에서는 rippled를 실행하려고 할 때 다음과 같은 오류 메시지가 표시될 수 있습니다:

```
WARNING: There are only 1024 file descriptors (soft limit) available, which
limit the number of simultaneous connections.
```

이는 시스템에 단일 프로세스가 열 수 있는 파일 수에 대한 보안 제한이 있지만 rippled에 대해 이 제한이 너무 낮게 설정되어 있기 때문에 발생합니다. 이 문제를 해결하려면 루트 액세스 권한이 필요합니다. 다음 단계에 따라 rippled하여 열 수 있는 파일 수를 늘릴 수 있습니다:

1. etc/security/limits.conf 파일의 끝에 다음 줄을 추가합니다:

```
*                soft    nofile          65536
*                hard    nofile          65536
```

2. 열 수 있는 파일 수에 대한 하드 제한이 이제 65536개인지 확인합니다:

```
ulimit -Hn
```

명령은 65536을 출력해야 합니다.

3. rippled를 다시 시작해보세요.

```
systemctl start rippled
```

4. 그래도 rippled가 시작되지 않으면 /etc/sysctl.conf를 열고 다음 커널 수준 설정을 추가하세요:

```
fs.file-max = 65536
```

## etc/opt/ripple/rippled.cfg를 열지 못했을 경우

시작 시 다음과 같은 오류와 함께 rippled가 충돌하는 경우, rippled는는 구성 파일을 읽을 수 없다는 뜻입니다:

```
Loading: "/etc/opt/ripple/rippled.cfg"
Failed to open '"/etc/opt/ripple/rippled.cfg"'.
Terminating thread rippled: main: unhandled St13runtime_error 'Can not create "/var/opt/ripple"'
Aborted (core dumped)
```

가능한 해결 방법:

* 구성 파일이 존재하고(기본 위치는 /etc/opt/ripple/rippled.cfg) rippled 프로세스를 실행하는 사용자(일반적으로 rippled)에게 해당 파일에 대한 읽기 권한이 있는지 확인합니다.
* HOME/.config/ripple/rippled.cfg에서 rippled된 사용자가 읽을 수 있는 구성 파일을 만듭니다(여기서 $HOME은 rippled된 사용자의 홈 디렉터리를 가리킴).

{% hint style="info" %}
Tip:

rippled 저장소에는 RPM 설치 시 기본 구성으로 제공되는 예제 rippled.cfg 파일이 포함되어 있습니다. 파일이 없는 경우 여기에서 복사할 수 있습니다.
{% endhint %}

* \--conf 커맨드라인 옵션을 사용하여 원하는 구성 파일의 경로를 지정합니다.

## 유효성 검증인 파일을 열지 못함

시작 시 다음과 같은 오류와 함께 rippled이 충돌하는 경우 기본 구성 파일을 읽을 수 있지만 해당 구성 파일에 별도의 유효성 검증인 구성 파일(일반적으로 validators.txt라는 이름)이 지정되어 있어 rippled가 읽을 수 없다는 의미입니다.

```
Loading: "/home/rippled/.config/ripple/rippled.cfg"
Terminating thread rippled: main: unhandled St13runtime_error 'The file specified in [validators_file] does not exist: /home/rippled/.config/ripple/validators.txt'
Aborted (core dumped)
```

가능한 해결 방법:

* \[validators.txt] 파일이 존재하고 rippled 사용자에게 해당 파일을 읽을 수 있는 권한이 있는지 확인합니다.

{% hint style="info" %}
Tip:

rippled 저장소에는 RPM 설치 시 기본 구성으로 제공되는 예제 validators.txt 파일이 포함되어 있습니다. 파일이 없는 경우 여기에서 복사할 수 있습니다.
{% endhint %}

* rippled.cfg 파일을 편집하고 유효성 검증인 파일(또는 이와 동등한 파일)의 올바른 경로를 갖도록 \[validators\_file] 설정을 수정합니다. 파일 이름 앞뒤에 여분의 공백이 있는지 확인하세요.
* rippled.cfg 파일을 편집하고 \[validators\_file] 설정을 제거합니다. 유효성 검증인 설정을 rippled.cfg 파일에 직접 추가합니다. 예를 들어:

```
[validator_list_sites]
https://vl.ripple.com

[validator_list_keys]
ED2677ABFFD1B33AC6FBC3062B71F1E8397C1505E1C42C64D11AD1B28FF73F4734
```

## 데이터베이스 경로를 만들 수 없음

rippled 시작 시 다음과 같은 오류와 함께 rippled가 충돌하는 경우 서버에 구성 파일에서 \[database\_path]에 대한 쓰기 권한이 없다는 의미입니다.

```
Loading: "/home/rippled/.config/ripple/rippled.cfg"
Terminating thread rippled: main: unhandled St13runtime_error 'Can not create "/var/lib/rippled/db"'
Aborted (core dumped)
```

구성 파일(/home/rippled/.config/ripple/rippled.cfg)과 데이터베이스 경로(/var/lib/rippled/db)의 경로는 시스템에 따라 다를 수 있습니다.

가능한 해결 방법:

* 오류 메시지에 표시된 데이터베이스 경로에 대한 쓰기 권한이 있는 다른 사용자로 rippled를 실행합니다.
* rippled.cfg 파일을 편집하고 \[database\_path] 설정을 변경하여 rippled된 사용자에게 쓰기 권한이 있는 경로를 사용하도록 합니다.
* rippled된 사용자에게 구성된 데이터베이스 경로에 대한 쓰기 권한을 부여합니다.

## 상태 DB 오류

rippled된 서버의 상태 데이터베이스가 손상된 경우 다음과 같은 오류가 발생할 수 있습니다. 예기치 않게 종료된 경우 또는 구성 파일에서 경로 및 \[database\_path] 설정을 변경하지 않고 데이터베이스 유형을 RocksDB에서 NuDB로 변경한 경우 발생할 수 있습니다.

```
2018-Aug-21 23:06:38.675117810 SHAMapStore:ERR state db error:
  writableDbExists false archiveDbExists false
  writableDb '/var/lib/rippled/db/rocksdb/rippledb.11a9' archiveDb '/var/lib/rippled/db/rocksdb/rippledb.2d73'

To resume operation, make backups of and remove the files matching /var/lib/rippled/db/state* and contents of the directory /var/lib/rippled/db/rocksdb

Terminating thread rippled: main: unhandled St13runtime_error 'state db error'
```

이 문제를 해결하는 가장 쉬운 방법은 데이터베이스를 완전히 삭제하는 것입니다. 대신 다른 곳에 백업하는 것이 좋습니다. 예를 들어:

```
mv /var/lib/rippled/db /var/lib/rippled/db-bak
```

또는 데이터베이스가 필요하지 않다고 확신하는 경우:

```
rm -r /var/lib/rippled/db
```

{% hint style="info" %}
Tip:

개별 서버는 XRP Ledger 네트워크의 다른 서버에서 ledger 기록을 다시 다운로드할 수 있기 때문에 일반적으로 rippled 데이터베이스는 삭제하는 것이 안전합니다.
{% endhint %}

또는 구성 파일에서 데이터베이스 경로를 변경할 수도 있습니다. 예를 들어:

```
[node_db]
type=NuDB
path=/var/lib/rippled/custom_nudb_path

[database_path]
/var/lib/rippled/custom_sqlite_db_path
```

## 온라인 삭제가 ledger 내역보다 적음

다음과 같은 오류 메시지는 rippled.cfg 파일에 \[ledger\_history] 및 online\_delete에 대해 모순되는 값이 있음을 나타냅니다.

```
Terminating thread rippled: main: unhandled St13runtime_error 'online_delete must not be less than ledger_history (currently 3000)
```

\[ledger\_history] 설정은 서버가 다시 채우려고 하는 기록 ledger의 수를 나타냅니다. online\_delete 필드(\[node\_db] 구절에 있음)는 이전 기록을 삭제할 때 보관할 기록 ledger의 수를 나타냅니다. online\_delete 값은 서버가 다운로드하려고 하는 기록 ledger를 삭제하지 않도록 \[ledger\_history]보다 크거나 같아야 합니다.

이 문제를 해결하려면 rippled.cfg 파일을 편집하여 \[ledger\_history] 또는 online\_delete 옵션을 변경하거나 제거하세요. (\[ledger\_history]를 생략하면 기본값인 256개의 ledger 버전이 사용됩니다. online\_delete 필드를 지정하는 경우 256보다 커야 합니다. online\_delete 필드를 생략하면 이전 ledger 버전의 자동 삭제가 비활성화됩니다).

## 잘못된 node\_size 값

다음과 같은 오류는 rippled.cfg 파일에 node\_size 설정 값이 잘못되었음을 나타냅니다:

```
Terminating thread rippled: main: unhandled N5beast14BadLexicalCastE 'std::bad_cast'
```

node\_size 필드에 유효한 매개변수는 작은, 작은, 중간, 큰 또는 큰입니다. 자세한 내용은 노드 크기를 참조하세요.

## 샤드 경로 누락

다음과 같은 오류는 rippled.cfg에 불완전한 히스토리 샤딩 구성이 있음을 나타냅니다:

```
Terminating thread rippled: main: unhandled St13runtime_error 'shard path missing'
```

구성에 \[shard\_db] 구절이 포함된 경우, rippled가 샤드 저장소에 대한 데이터를 쓸 수 있는 디렉터리를 가리키는 경로 필드가 포함되어야 합니다. 이 오류는 경로 필드가 누락되었거나 잘못된 위치에 있다는 것을 의미합니다. 구성 파일에 여분의 공백이나 오타가 있는지 확인하고 샤드 구성 예제와 비교하세요.

## 지원되지 않는 샤드 저장소 유형: RocksDB

RocksDB는 더 이상 기록 샤딩을 위한 백엔드로 지원되지 않습니다. RocksDB 샤드 저장소를 정의하는 기존 구성이 있는 경우 서버가 시작되지 않습니다. [![New in: rippled 1.3.1](https://img.shields.io/badge/New%20in-rippled%201.3.1-blue.svg)](https://github.com/ripple/rippled/releases/tag/1.3.1)

이 경우 로그 시작 명령 직후 프로세스가 종료되고 출력 로그의 앞부분에 다음과 같은 메시지가 표시됩니다:

```
ShardStore:ERR Unsupported shard store type: RocksDB
```

이 문제를 해결하려면 다음 중 하나를 수행한 다음 서버를 다시 시작합니다:

* 대신 NuDB를 사용하도록 샤드 저장소를 변경합니다.
* 기록 샤딩을 비활성화합니다.
