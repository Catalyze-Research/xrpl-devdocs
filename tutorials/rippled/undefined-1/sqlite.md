# SQLite 트랜잭션 데이터베이스 페이지 크기 문제 해결

전체 ledger 기록(또는 매우 많은 양의 트랜잭션 기록)이 있는 rippled 서버와 처음에 0.40.0(2017년 1월 출시) 이전의 rippled 버전으로 생성된 데이터베이스에서 서버가 제대로 작동하지 않게 하는 SQLite 데이터베이스 페이지 크기 문제가 발생할 수 있습니다. 최근 트랜잭션 기록만 저장하는 서버(기본 구성)와 데이터베이스 파일이 rippled 버전 0.40.0 이상으로 생성된 서버는 이 문제가 발생하지 않을 가능성이 높습니다.

이 문서에서는 이 문제가 발생하는 경우 이를 감지하고 수정하는 단계를 설명합니다.

## 배경

rippled 서버는 트랜잭션 기록의 사본을 SQLite 데이터베이스에 저장합니다. 버전 0.40.0 이전에는 이 데이터베이스의 용량이 약 2TB로 구성되었습니다. 대부분의 용도로는 이 정도면 충분합니다. 그러나 ledger 32570(프로덕션 XRP Ledger 기록에서 사용 가능한 가장 오래된 ledger 버전)까지의 전체 트랜잭션 내역은 SQLite 데이터베이스 용량을 초과할 가능성이 높습니다. rippled 서버 버전 0.40.0 이상에서는 더 큰 용량으로 SQLite 데이터베이스 파일을 생성하므로 이 문제가 발생하지 않을 가능성이 높습니다.

SQLite 데이터베이스의 용량은 데이터베이스의 페이지 크기 매개변수의 결과이며, 데이터베이스가 생성된 후에는 쉽게 변경할 수 없습니다. (SQLite 내부에 대한 자세한 내용은 공식 SQLite 설명서를 참조하세요.) 데이터베이스가 저장된 디스크 및 파일 시스템에 여유 공간이 남아 있어도 데이터베이스가 용량에 도달할 수 있습니다. 아래 수정 사항에 설명된 대로 이 문제를 방지하기 위해 페이지 크기를 재구성하려면 마이그레이션 프로세스에 다소 시간이 소요됩니다.

{% hint style="info" %}
Tip:

대부분의 사용 사례에서는 전체 기록이 필요하지 않습니다. 전체 트랜잭션 기록이 있는 서버는 장기 분석 및 아카이브 목적이나 재해에 대한 예방책으로 유용할 수 있습니다. 트랜잭션 기록을 저장하는 데 리소스를 덜 많이 사용하는 방법은 히스토리 샤딩을 참조하세요.
{% endhint %}

## 탐지

서버가 이 문제에 취약한 경우 두 가지 방법으로 문제를 감지할 수 있습니다:

* rippled 서버가 1.1.0 버전 이상인 경우 문제가 발생하기 전에 문제를 사전에 감지할 수 있습니다.
* 모든 rippled 버전에서 문제를 사후적으로(서버가 충돌할 때) 감지할 수 있습니다.

두 경우 모두 문제를 감지하려면 rippled 서버 로그에 액세스해야 합니다.

{% hint style="info" %}
Tip:

디버그 로그의 위치는 rippled 서버의 구성 파일에 따라 다릅니다. 기본 구성은 서버의 디버그 로그를 /var/log/rippled/debug.log 파일에 기록합니다.
{% endhint %}

## 사전 감지

SQLite 페이지 크기 문제를 사전에 감지하려면 rippled 1.1.0 이상을 실행 중이어야 합니다. rippled 서버는 디버그 로그에 다음과 같은 메시지를 2분에 한 번 이상 주기적으로 기록합니다. (로그 항목의 정확한 숫자 값과 트랜잭션 데이터베이스 경로는 사용자 환경에 따라 다릅니다.)

```
Transaction DB pathname: /opt/rippled/transaction.db; SQLite page size: 1024
  bytes; Free pages: 247483646; Free space: 253423253504 bytes; Note that this
  does not take into account available disk space.
```

SQLite 페이지 크기: 1024바이트 값은 트랜잭션 데이터베이스가 더 작은 페이지 크기로 구성되어 있으며 전체 트랜잭션 기록을 위한 용량이 없음을 나타냅니다. 값이 이미 4096바이트 이상인 경우에는 SQLite 데이터베이스에 전체 트랜잭션 기록을 저장할 수 있는 충분한 용량이 이미 있으므로 이 문서에 설명된 마이그레이션을 수행할 필요가 없습니다.

이 로그 메시지에 설명된 여유 공간이 524288000바이트(500MB) 미만이 되면 rippled 서버가 중지됩니다. 여유 공간이 이 임계값에 가까워지면 문제를 해결하여 예기치 않은 중단을 방지하세요.

## 사후 감지

서버의 SQLite 데이터베이스 용량이 이미 초과된 경우, rippled 서비스는 문제를 나타내는 로그 메시지를 작성하고 서비스를 중단합니다.

## rippled 1.1.0 이상 버전

rippled 버전 1.1.0 이상에서는 서버의 디버그 로그에 다음과 같은 메시지와 함께 서버가 정상적으로 종료됩니다:

```
Free SQLite space for transaction db is less than 512MB. To fix this, rippled
  must be executed with the vacuum <sqlitetmpdir> parameter before restarting.
  Note that this activity can take multiple days, depending on database size.
```

## rippled 1.1.0 이전 버전

1.1.0 이전 rippled 버전에서는 서버의 디버그 로그에 다음과 같은 메시지와 함께 서버가 반복적으로 충돌합니다:

```
Terminating thread doJob: AcquisitionDone: unhandled
  N4soci18sqlite3_soci_errorE 'sqlite3_statement_backend::loadOne: database or
  disk is full while executing "INSERT INTO [...]
```

## 수정

이 문서에 설명된 단계에 따라 지원되는 Linux 시스템에서 rippled를 사용하여 이 문제를 해결할 수 있습니다. 권장 하드웨어 구성과 거의 일치하는 시스템 사양을 갖춘 전체 기록 서버의 경우 이 프로세스에 이틀 이상이 소요될 수 있습니다.

## 요구 사항

* rippled 버전 1.1.0 이상을 실행 중이어야 합니다.
  * 이 프로세스를 시작하기 전에 rippled를 최신 안정 버전으로 업그레이드하세요.
  * 다음 명령을 실행하여 로컬에 설치한 rippled 버전을 확인할 수 있습니다:

```
rippled --version
```

rippled 사용자가 쓰기 가능한 디렉터리에 트랜잭션 데이터베이스의 두 번째 복사본을 임시로 저장할 수 있는 충분한 여유 공간이 있어야 합니다. 이 여유 공간은 기존 트랜잭션 데이터베이스와 동일한 파일 시스템에 있을 필요는 없습니다.\
트랜잭션 데이터베이스는 구성의 \[database\_path] 설정으로 지정한 폴더에 있는 transaction.db 파일에 저장됩니다. 이 파일의 크기를 확인하여 필요한 여유 공간을 확인할 수 있습니다. 예를 들어:

```
ls -l /var/lib/rippled/db/transaction.db
```

## 마이그레이션 프로세스

트랜잭션 데이터베이스를 더 큰 페이지 크기로 마이그레이션하려면 다음 단계를 수행하세요:

1. 모든 필수 요건을 충족하는지 확인합니다.
2. 마이그레이션 프로세스 중에 임시 파일을 저장할 폴더를 만듭니다.

```
mkdir /tmp/rippled_txdb_migration
```

3. rippled 사용자에게 임시 폴더에 대한 소유권을 부여하여 해당 폴더에 파일을 쓸 수 있도록 합니다. (임시 폴더가 rippled 사용자가 이미 쓰기 권한이 있는 위치에 있는 경우에는 이 작업이 필요하지 않습니다.)

```
chown rippled /tmp/rippled_txdb_migration
```

4. 임시 폴더에 트랜잭션 데이터베이스 사본을 저장할 수 있는 충분한 여유 공간이 있는지 확인합니다.\
   예를 들어, df 명령의 사용 가능 출력을 [`transaction.db`](https://xrpl.org/fix-sqlite-tx-db-page-size-issue.html#prerequisites) 파일의 크기와 비교합니다.

```
df -h /tmp/rippled_txdb_migration

Filesystem      Size  Used Avail Use% Mounted on
/dev/sda2       5.4T  2.6T  2.6T  50% /tmp
```

5. rippled가 여전히 실행 중이면 중지합니다:

```
sudo systemctl stop rippled
```

6. 로그아웃할 때 프로세스가 중지되지 않도록 화면 세션(또는 기타 유사한 도구)을 엽니다:

```
screen
```

7. rippled 사용자가 됩니다:

```
sudo su - rippled
```

8. 임시 디렉터리 경로와 함께 --vacuum 명령을 사용하여 rippled 실행 파일을 직접 실행합니다:

```
/opt/ripple/bin/rippled -q --vacuum /tmp/rippled_txdb_migration
```

rippled 실행 파일은 즉시 다음 메시지를 표시합니다:

```
VACUUM beginning. page_size: 1024
```

9. 프로세스가 완료될 때까지 기다리세요. 이 과정은 이틀 이상 걸릴 수 있습니다. 프로세스가 완료되면 rippled 실행 파일에 다음 메시지가 표시된 후 종료됩니다:

```
VACUUM finished. page_size: 4096
```

기다리는 동안 **CTRL-A**를 누른 다음 **D**를 눌러 화면 세션을 분리할 수 있으며, 나중에 다음과 같은 명령을 사용하여 화면 세션을 다시 연결할 수 있습니다:

```
screen -x -r
```

프로세스가 끝나면 화면 세션을 종료합니다:

```
exit
```

화면 명령에 대한 자세한 내용은 공식 화면 사용 설명서 또는 온라인에서 제공되는 기타 여러 리소스를 참조하세요.

10. &#x20;rippled 서비스를 다시 시작합니다.

```
sudo systemctl start rippled
```

11. rippled 서비스가 성공적으로 시작되었는지 확인합니다.\
    커맨드라인 인터페이스를 사용하여 서버 상태를 확인할 수 있습니다(JSON-RPC 요청을 수락하지 않도록 서버를 구성하지 않은 경우). 예를 들어:

```
/opt/ripple/bin/rippled server_info
```

이 명령의 예상 응답에 대한 설명은 server\_info 메소드 문서를 참조하세요.

12. 서버의 디버그 로그를 확인하여 SQLite 페이지 크기가 이제 4096인지 확인합니다:

```
tail -F /var/log/rippled/debug.log
```

주기적인 로그 메시지에도 마이그레이션 전보다 훨씬 더 많은 여유 페이지와 여유 페이지가 표시되어야 합니다.

13. 원하는 경우 마이그레이션 프로세스를 위해 만든 임시 폴더를 제거할 수 있습니다.

```
rm -r /tmp/rippled_txdb_migration
```

트랜잭션 데이터베이스의 임시 복사본을 보관하기 위해 추가 스토리지를 마운트한 경우 지금 마운트를 해제하고 제거할 수 있습니다.
