# 리포팅 모드에서 rippled 빌드 및 실행

리포팅 모드는 HTTP 및 웹소켓 API를 제공하는 데 특화된 XRP Ledger 코어 서버의 모드입니다.\
\
리포팅 모드에서 서버는 P2P 네트워크에 연결하지 않습니다. 대신, gRPC를 사용해 P2P 네트워크에 연결된 하나 이상의 신뢰할 수 있는 서버에서 검증된 데이터를 가져옵니다.\
\
그러면 API 호출을 효율적으로 처리하여 P2P 모드로 실행되는 rippled 서버의 부하를 줄일 수 있습니다.

<figure><img src="https://xrpl.org/img/reporting-mode-basic-architecture.svg" alt="" width="563"><figcaption></figcaption></figure>

rippled 리포팅 모드에서는 두 개의 데이터스토어를 사용합니다:

* 트랜잭션 메타데이터, 계정 상태, ledger 헤더를 포함하는 rippled용 기본 영구 데이터 저장소입니다. 기본 영구 데이터 저장소로 NuDB(소스에 포함됨) 또는 Cassandra를 사용할 수 있습니다. Cassandra를 사용하는 경우, 여러 리포팅 모드 서버가 단일 Cassandra 인스턴스 또는 클러스터의 데이터에 대한 액세스를 공유할 수 있습니다.
* 관계형 데이터를 저장하는 PostgreSQL 데이터베이스로, 주로 tx 메소드 및 account\_tx 메소드에 사용됩니다.

리포팅 모드 서버가 API 요청을 받으면 가능한 경우 이러한 데이터 저장소에서 데이터를 로드합니다. P2P 네트워크의 데이터가 필요한 요청의 경우 리포팅 모드는 요청을 P2P 서버로 전달한 다음 클라이언트에 응답을 다시 전달합니다.\
\
여러 리포팅 모드 서버가 동일한 네트워크 액세스 가능 데이터베이스(PostgreSQL 및 Cassandra)에 대한 액세스를 공유할 수 있으며, 언제든지 하나의 리포팅 모드 서버만 데이터베이스에 쓰고 다른 서버는 모두 데이터베이스에서 읽습니다.

## 리포팅 모드를 실행하는 방법

### 요구 조건

1. 시스템이 시스템 요구 사항을 충족하는지 확인합니다.

{% hint style="info" %}
Note:

데이터베이스로 Cassandra를 사용하도록 선택하면 데이터가 로컬 디스크에 저장되지 않으므로 rippled에 대한 디스크 요구 사항이 낮아집니다.
{% endhint %}

2. 리포팅 모드를 실행하려면 P2P 모드에서 하나 이상의 rippled 서버도 실행해야 합니다.
3. 호환되는 버전의 CMake가 설치되어 있어야 합니다.
4. 리포팅 모드에서 rippled을 실행하는 데 필요한 데이터스토어를 설치하고 구성합니다.
   1. PostgreSQL을 설치합니다.
   2. 기본 영구 데이터스토어로 사용할 데이터베이스를 설치하고 구성합니다. Cassandra 또는 NuDB를 사용하도록 선택할 수 있습니다.
   3. macOS에서는 Cassandra cpp 드라이버를 수동으로 설치해야 합니다. 다른 모든 플랫폼에서는 Cassandra 드라이버가 rippled 빌드의 일부로 빌드됩니다.

```
brew install cassandra-cpp-driver
```

## PostgreSQL 설치

### **리눅스에 PostgreSQL 설치**

1. 리눅스에 PostgreSQL을 다운로드하여 설치합니다.
2. psql을 사용하여 PostgreSQL 데이터베이스 서버에 연결하고, 신규 사용자 및 데이터베이스 보고를 생성합니다.

```
psql postgres
    CREATE ROLE newuser WITH LOGIN PASSWORD ‘password’;
    ALTER ROLE newuser CREATEDB;
\q
psql postgres -U newuser
postgres=# create database reporting;
```

### macOS에 PostgreSQL 설치

1. macOS에 PostgreSQL을 다운로드하여 설치합니다.

```
brew install postgres
brew services start postgres
```

2. psql을 사용하여 PostgreSQL 데이터베이스 서버에 연결하고 신규 사용자 및 데이터베이스 보고를 생성합니다.

```
psql postgres
    CREATE ROLE newuser WITH LOGIN PASSWORD ‘password’;
    ALTER ROLE newuser CREATEDB;
\q
psql postgres -U newuser
postgres=# create database reporting;
```

## 기본 영구 데이터스토어 설치 및 구성

**Cassandra**

Cassandra를 설치한 다음 복제를 사용하여 rippled를 위한 키 공간을 만듭니다.\
복제 계수 3을 권장하지만, 로컬로 실행할 때는 복제가 필요하지 않으며 replication\_factor를 1로 설정할 수 있습니다.

```
$ cqlsh [host] [port]
> CREATE KEYSPACE `rippled` WITH REPLICATION =
{'class' : 'SimpleStrategy', 'replication_factor' : 1    };
```

**NuDB**

로컬 네트워크에 대한 리포팅 모드에서 rippled을 실행하는 경우, 백엔드 데이터베이스로 Cassandra 대신 NuDB를 사용하도록 선택할 수 있습니다.\
\
NuDB는 rippled 빌드 설정의 일부로 설치되며 추가 설치 단계가 필요하지 않습니다.

## 절차

1. 우분투 또는 macOS에서 리포팅 모드용 rippled 빌드.

{% tabs %}
{% tab title="Linux" %}
```
wget https://github.com/Kitware/CMake/releases/download/v3.16.3/cmake-3.16.3-Linux-x86_64.sh
sudo sh cmake-3.16.3-Linux-x86_64.sh --prefix=/usr/local --exclude-subdir 
cmake -B build -Dreporting=ON -DCMAKE_BUILD_TYPE=Debug 
cmake --build build --parallel $(nproc)
```
{% endtab %}

{% tab title="macOS" %}
```
cmake -B build -G "Unix Makefiles" -Dreporting=ON -DCMAKE_BUILD_TYPE=Debug
cmake --build build --parallel $(nproc)
```
{% endtab %}
{% endtabs %}

2. 리포팅 모드에서 rippled을 실행할 구성 파일을 만듭니다.\
   예제 구성 파일인 rippled-example.cfg를 복사하여 루트 사용자가 아닌 사용자로 rippled리드를 실행할 수 있는 위치에 rippled-reporting-mode.cfg로 저장합니다. 예를 들어:

```
mkdir -p $HOME/.config/ripple
cp <RIPPLED_SOURCE>/cfg/rippled-example.cfg $HOME/.config/ripple/rippled-reporting-mode.cfg
```

3.  rippled-reporting-mode.cfg를 편집하여 필요한 파일 경로를 설정합니다. rippled을 실행하려는 사용자는 여기에 지정한 모든 경로에 대한 쓰기 권한이 있어야 합니다.

    1. ledger 데이터베이스를 저장할 위치로 \[node\_db] 경로를 설정합니다.
    2. \[database\_path]를 다른 데이터베이스 데이터를 저장할 위치로 설정합니다. (여기에는 구성 데이터가 있는 SQLite 데이터베이스가 포함되며, 일반적으로 \[node\_db] 경로 필드보다 한 단계 위입니다.)
    3. \[debug\_logfile]을 rippled가 로깅 정보를 기록할 수 있는 경로로 설정합니다.

    이 설정은 rippled을 성공적으로 시작하기 위해 필요한 유일한 구성입니다. 다른 모든 구성은 선택 사항이며 서버가 작동한 후에 조정할 수 있습니다.
4. 리포팅 모드를 사용하도록 설정하려면 rippled-reporting-mode.cfg 파일을 편집합니다:
   1. \[reporting] 구절의 주석을 해제하거나 새 구절을 추가합니다:

```
[reporting]
etl_source
read_only=0
```

2. 데이터를 추출할 rippled된 소스(ETL 소스)를 나열합니다. 이러한 rippled 서버에는 gRPC가 활성화되어 있어야 합니다.\
   참고: 리포팅 모드는 P2P 네트워크에 연결하지 않으므로 데이터가 실제로 네트워크 합의 ledger과 일치하는지 확인할 수 없으므로 신뢰할 수 있는 서버만 포함하세요.

```
[etl_source]
source_grpc_port=50051
source_ws_port=6006
source_ip=127.0.0.1
```

5. 데이터베이스 구성
   1. \[ledger\_tx\_tables]에 대한 Postgres DB를 지정합니다:

```
[ledger_tx_tables]
conninfo = postgres://newuser:password@127.0.0.1/reporting
use_tx_tables=1
```

2. \[node\_db]에 대한 데이터베이스를 지정합니다.

{% tabs %}
{% tab title="NuDB" %}
```
[node_db]
type=NuDB
path=/home/ubuntu/ripple/

[ledger_history]
1000000
```
{% endtab %}

{% tab title="Cassandra" %}
```
[node_db]
type=Cassandra

[ledger_history]
1000000
```
{% endtab %}
{% endtabs %}

6. rippled에 대한 구성을 수정하여 포트를 엽니다.

* 공용 웹소켓 포트를 엽니다:

```
[port_ws_admin_local]
port = 6006
ip = 127.0.0.1
admin = 127.0.0.1
protocol = ws
```

* gRPC 포트를 엽니다:

```
[port_grpc]
port = 60051
ip = 0.0.0.0
```

* 리포팅 시스템의 IP에 보안 게이트웨이를 추가합니다:

```
secure_gateway = 127.0.0.1
```

7. 리포팅 모드에서 rippled를 실행합니다:

## 기대할 수 있는 기능

다음은 터미널에 표시되는 내용을 발췌한 것입니다.

```
Loading: "/home/ubuntu/.config/ripple/rippled-reporting-example.cfg"
2021-Dec-09 21:31:52.245577 UTC JobQueue:NFO Using 10  threads
2021-Dec-09 21:31:52.255422 UTC LedgerConsensus:NFO Consensus engine started (cookie: 17859050541656985684)
2021-Dec-09 21:31:52.256542 UTC ReportingETL::ETLSource:NFO Using IP to connect to ETL source: 127.0.0.1:50051
2021-Dec-09 21:31:52.257784 UTC ReportingETL::ETLSource:NFO Made stub for remote = { validated_ledger :  , ip : 127.0.0.1 , web socket port : 6006, grpc port : 50051 }
2021-Dec-09 21:31:52.258032 UTC ReportingETL::LoadBalancer:NFO add : added etl source - { validated_ledger :  , ip : 127.0.0.1 , web socket port : 6006, grpc port : 50051 }
2021-Dec-09 21:31:52.258327 UTC Application:NFO process starting: rippled-1.8.1+DEBUG
2021-Dec-09 21:31:52.719186 UTC PgPool:DBG max_connections: 18446744073709551615, timeout: 600, connection params: port: 5432, hostaddr: 127.0.0.1, user: newuser, password: *, channel_binding: prefer, dbname: reporting_test_core, host: 127.0.0.1, options: , sslmode: prefer, sslcompression: 0, sslsni: 1, ssl_min_protocol_version: TLSv1.2, gssencmode: prefer, krbsrvname: postgres, target_session_attrs: any
2021-Dec-09 21:31:52.788851 UTC PgPool:NFO server message: NOTICE:  relation "version" already exists, skipping

2021-Dec-09 21:31:53.282807 UTC TaggedCache:DBG LedgerCache target size set to 384
2021-Dec-09 21:31:53.282892 UTC TaggedCache:DBG LedgerCache target age set to 240000000000
2021-Dec-09 21:31:53.283741 UTC Amendments:DBG Amendment 98DECF327BF79997AEC178323AD51A830E457BFC6D454DAF3E46E5EC42DC619F (CheckCashMakesTrustLine) is supported and will be down voted if not enabled on the ledger.
2021-Dec-09 21:31:53.283836 UTC Amendments:DBG Amendment 157D2D480E006395B76F948E3E07A45A05FE10230D88A7993C71F97AE4B1F2D1 (Checks) is supported and will be up voted if not enabled on the ledger.
2021-Dec-09 21:31:53.283917 UTC Amendments:DBG Amendment 1562511F573A19AE9BD103B5D6B9E01B3B46805AEC5D3C4805C902B514399146 (CryptoConditions) is supported and will be down voted if not enabled on the ledger.
2021-Dec-09 21:31:53.283975 UTC Amendments:DBG Amendment 86E83A7D2ECE3AD5FA87AB2195AE015C950469ABF0B72EAACED318F74886AE90 (CryptoConditionsSuite) is supported and will be down voted if not enabled on the ledger.
2021-Dec-09 21:31:53.284016 UTC Amendments:DBG Amendment 30CD365592B8EE40489BA01AE2F7555CAC9C983145871DC82A42A31CF5BAE7D9 (DeletableAccounts) is supported and will be up voted if not enabled on the ledger.
2021-Dec-09 21:31:53.284062 UTC Amendments:DBG Amendment F64E1EABBE79D55B3BB82020516CEC2C582A98A6BFE20FBE9BB6A0D233418064 (DepositAuth) is supported and will be up voted if not enabled on the ledger.
2021-Dec-09 21:31:53.284099 UTC Amendments:DBG Amendment 3CBC5C4E630A1B82380295CDA84B32B49DD066602E74E39B85EF64137FA65194 (DepositPreauth) is supported and will be up voted if not enabled on the ledger.
2021-Dec-09 21:31:53.284126 UTC Amendments:DBG Amendment DC9CA96AEA1DCF83E527D1AFC916EFAF5D27388ECA4060A88817C1238CAEE0BF (EnforceInvariants) is supported and will be down voted if not enabled on the ledger.
2021-Dec-09 21:31:53.284153 UTC Amendments:DBG Amendment 07D43DCE529B15A10827E5E04943B496762F9A88E3268269D69C44BE49E21104 (Escrow) is supported and will be down voted if not enabled on the ledger.
2021-Dec-09 21:31:53.284189 UTC Amendments:DBG Amendment 42426C4D4F1009EE67080A9B7965B44656D7714D104A72F9B4369F97ABF044EE (FeeEscalation) is supported and will be down voted if not enabled on the ledger.
2021-Dec-09 21:31:53.284216 UTC Amendments:DBG Amendment 740352F2412A9909880C23A559FCECEDA3BE2126FED62FC7660D628A06927F11 (Flow) is supported and will be up voted if not enabled on the ledger.
2021-Dec-09 21:31:53.284241 UTC Amendments:DBG Amendment 3012E8230864E95A58C60FD61430D7E1B4D3353195F2981DC12B0C7C0950FFAC (FlowCross) is supported and will be up voted if not enabled on the ledger.
2021-Dec-09 21:31:53.284284 UTC Amendments:DBG Amendment AF8DF7465C338AE64B1E937D6C8DA138C0D63AD5134A68792BBBE1F63356C422 (FlowSortStrands) is supported and will be up voted if not enabled on the ledger.
2021-Dec-09 21:31:53.284337 UTC Amendments:DBG Amendment 1F4AFA8FA1BC8827AD4C0F682C03A8B671DCDF6B5C4DE36D44243A684103EF88 (HardenedValidations) is supported and will be up voted if not enabled on the ledger.
2021-Dec-09 21:31:53.284412 UTC Amendments:DBG Amendment 4C97EBA926031A7CF7D7B36FDE3ED66DDA5421192D63DE53FFB46E43B9DC8373 (MultiSign) is supported and will be down voted if not enabled on the ledger.
2021-Dec-09 21:31:53.284455 UTC Amendments:DBG Amendment 586480873651E106F1D6339B0C4A8945BA705A777F3F4524626FF1FC07EFE41D (MultiSignReserve) is supported and will be up voted if not enabled on the ledger.
2021-Dec-09 21:31:53.284491 UTC Amendments:DBG Amendment B4E4F5D2D6FB84DF7399960A732309C9FD530EAE5941838160042833625A6076 (NegativeUNL) is supported and will be down voted if not enabled on the ledger.
2021-Dec-09 21:31:53.284528 UTC Amendments:DBG Amendment 08DE7D96082187F6E6578530258C77FAABABE4C20474BDB82F04B021F1A68647 (PayChan) is supported and will be down voted if not enabled on the ledger.
2021-Dec-09 21:31:53.284592 UTC Amendments:DBG Amendment 00C1FC4A53E60AB02C864641002B3172F38677E29C26C5406685179B37E1EDAC (RequireFullyCanonicalSig) is supported and will be up voted if not enabled on the ledger.
2021-Dec-09 21:31:53.284649 UTC Amendments:DBG Amendment CC5ABAE4F3EC92E94A59B1908C2BE82D2228B6485C00AFF8F22DF930D89C194E (SortedDirectories) is supported and will be down voted if not enabled on the ledger.
2021-Dec-09 21:31:53.284703 UTC Amendments:DBG Amendment 532651B4FD58DF8922A49BA101AB3E996E5BFBF95A913B3E392504863E63B164 (TickSize) is supported and will be down voted if not enabled on the ledger.
2021-Dec-09 21:31:53.284787 UTC Amendments:DBG Amendment 955DF3FA5891195A9DAEFA1DDC6BB244B545DDE1BAA84CBB25D5F12A8DA68A0C (TicketBatch) is supported and will be up voted if not enabled on the ledger.
2021-Dec-09 21:31:53.284950 UTC Amendments:DBG Amendment 6781F8368C4771B83E8B821D88F580202BCB4228075297B19E4FDC5233F1EFDC (TrustSetAuth) is supported and will be down voted if not enabled on the ledger.
2021-Dec-09 21:31:53.284997 UTC Amendments:DBG Amendment B4D44CC3111ADD964E846FC57760C8B50FFCD5A82C86A72756F6B058DDDF96AD (fix1201) is supported and will be down voted if not enabled on the ledger.
2021-Dec-09 21:31:53.285025 UTC Amendments:DBG Amendment E2E6F2866106419B88C50045ACE96368558C345566AC8F2BDF5A5B5587F0E6FA (fix1368) is supported and will be down voted if not enabled on the ledger.
2021-Dec-09 21:31:53.285067 UTC Amendments:DBG Amendment 42EEA5E28A97824821D4EF97081FE36A54E9593C6E4F20CBAE098C69D2E072DC (fix1373) is supported and will be down voted if not enabled on the ledger.
2021-Dec-09 21:31:53.285103 UTC Amendments:DBG Amendment 6C92211186613F9647A89DFFBAB8F94C99D4C7E956D495270789128569177DA1 (fix1512) is supported and will be down voted if not enabled on the ledger.
2021-Dec-09 21:31:53.285129 UTC Amendments:DBG Amendment 67A34F2CF55BFC0F93AACD5B281413176FEE195269FA6D95219A2DF738671172 (fix1513) is supported and will be up voted if not enabled on the ledger.
2021-Dec-09 21:31:53.285153 UTC Amendments:DBG Amendment 5D08145F0A4983F23AFFFF514E83FAD355C5ABFBB6CAB76FB5BC8519FF5F33BE (fix1515) is supported and will be up voted if not enabled on the ledger.
2021-Dec-09 21:31:53.285176 UTC Amendments:DBG Amendment B9E739B8296B4A1BB29BE990B17D66E21B62A300A909F25AC55C22D6C72E1F9D (fix1523) is supported and will be down voted if not enabled on the ledger.
2021-Dec-09 21:31:53.285202 UTC Amendments:DBG Amendment 1D3463A5891F9E589C5AE839FFAC4A917CE96197098A1EF22304E1BC5B98A454 (fix1528) is supported and will be down voted if not enabled on the ledger.
2021-Dec-09 21:31:53.285256 UTC Amendments:DBG Amendment CA7C02118BA27599528543DFE77BA6838D1B0F43B447D4D7F53523CE6A0E9AC2 (fix1543) is supported and will be up voted if not enabled on the ledger.
2021-Dec-09 21:31:53.285290 UTC Amendments:DBG Amendment 7117E2EC2DBF119CA55181D69819F1999ECEE1A0225A7FD2B9ED47940968479C (fix1571) is supported and will be up voted if not enabled on the ledger.
2021-Dec-09 21:31:53.285343 UTC Amendments:DBG Amendment FBD513F1B893AC765B78F250E6FFA6A11B573209D1842ADC787C850696741288 (fix1578) is supported and will be up voted if not enabled on the ledger.
2021-Dec-09 21:31:53.285381 UTC Amendments:DBG Amendment 58BE9B5968C4DA7C59BA900961828B113E5490699B21877DEF9A31E9D0FE5D5F (fix1623) is supported and will be up voted if not enabled on the ledger.
2021-Dec-09 21:31:53.285424 UTC Amendments:DBG Amendment 25BA44241B3BD880770BFA4DA21C7180576831855368CBEC6A3154FDE4A7676E (fix1781) is supported and will be up voted if not enabled on the ledger.
2021-Dec-09 21:31:53.285464 UTC Amendments:DBG Amendment 4F46DF03559967AC60F2EB272FEFE3928A7594A45FF774B87A7E540DB0F8F068 (fixAmendmentMajorityCalc) is supported and will be up voted if not enabled on the ledger.
2021-Dec-09 21:31:53.285500 UTC Amendments:DBG Amendment 8F81B066ED20DAECA20DF57187767685EEF3980B228E0667A650BAF24426D3B4 (fixCheckThreading) is supported and will be up voted if not enabled on the ledger.
2021-Dec-09 21:31:53.285527 UTC Amendments:DBG Amendment C4483A1896170C66C098DEA5B0E024309C60DC960DE5F01CD7AF986AA3D9AD37 (fixMasterKeyAsRegularKey) is supported and will be up voted if not enabled on the ledger.
2021-Dec-09 21:31:53.285550 UTC Amendments:DBG Amendment 621A0B264970359869E3C0363A899909AAB7A887C8B73519E4ECF952D33258A8 (fixPayChanRecipientOwnerDir) is supported and will be up voted if not enabled on the ledger.
2021-Dec-09 21:31:53.285575 UTC Amendments:DBG Amendment 89308AF3B8B10B7192C4E613E1D2E4D9BA64B2EE2D5232402AE82A6A7220D953 (fixQualityUpperBound) is supported and will be up voted if not enabled on the ledger.
2021-Dec-09 21:31:53.285614 UTC Amendments:DBG Amendment B6B3EEDC0267AB50491FDC450A398AF30DBCD977CECED8BEF2499CAB5DAC19E2 (fixRmSmallIncreasedQOffers) is supported and will be up voted if not enabled on the ledger.
2021-Dec-09 21:31:53.285651 UTC Amendments:DBG Amendment 452F5906C46D46F407883344BFDD90E672B672C5E9943DB4891E3A34FEEEB9DB (fixSTAmountCanonicalize) is supported and will be up voted if not enabled on the ledger.
2021-Dec-09 21:31:53.285725 UTC Amendments:DBG Amendment 2CD5286D8D687E98B41102BDD797198E81EA41DF7BD104E6561FEB104EFF2561 (fixTakerDryOfferRemoval) is supported and will be up voted if not enabled on the ledger.
2021-Dec-09 21:31:53.290446 UTC Server:NFO Opened 'port_rpc_admin_local' (ip=127.0.0.1:7005, admin IPs:127.0.0.1, http)
2021-Dec-09 21:31:53.290834 UTC Server:NFO Opened 'port_ws_admin_local' (ip=127.0.0.1:7006, admin IPs:127.0.0.1, ws)
2021-Dec-09 21:31:53.290984 UTC Application:WRN Running in standalone mode
2021-Dec-09 21:31:53.291048 UTC NetworkOPs:NFO STATE->full
2021-Dec-09 21:31:53.291192 UTC Application:FTL Startup RPC: 
{
    "command" : "log_level",
    "severity" : "debug"
}


2021-Dec-09 21:31:53.291347 UTC RPCHandler:DBG RPC call log_level completed in 2.2e-08seconds
2021-Dec-09 21:31:53.291440 UTC Application:FTL Result: 
{
    "warnings" : 
    [

        {
            "id" : 1004,
            "message" : "This is a reporting server.  The default behavior of a reporting server is to only return validated data. If you are looking for not yet validated data, include \"ledger_index : current\" in your request, which will cause this server to forward the request to a p2p node. If the forward is successful the response will include \"forwarded\" : \"true\""
        }
    ]
}


2021-Dec-09 21:31:53.291502 UTC ReportingETL:NFO Starting reporting etl
2021-Dec-09 21:31:53.291605 UTC Application:NFO Application starting. Version is 1.8.1+DEBUG
2021-Dec-09 21:31:53.291747 UTC LoadManager:DBG Starting
2021-Dec-09 21:31:53.291846 UTC gRPC Server:NFO Starting gRPC server at 0.0.0.0:60051
2021-Dec-09 21:31:53.293246 UTC LedgerCleaner:DBG Started
2021-Dec-09 21:31:53.295543 UTC ReportingETL::ETLSource:DBG handleMessage : Received a message on ledger  subscription stream. Message : {
   "result" : {},
   "status" : "success",
   "type" : "response"
}
 - { validated_ledger :  , ip : 127.0.0.1 , web socket port : 6006, grpc port : 50051 }
2021-Dec-09 21:31:53.368075 UTC ReportingETL:NFO monitor : Database is empty. Will download a ledger from the network.
2021-Dec-09 21:31:53.368183 UTC ReportingETL:NFO monitor : Waiting for next ledger to be validated by network...
```

## 자주 묻는 질문

**리포팅 모드를 사용하려면 rippled 인스턴스를 두 개 이상 실행해야 하나요?**\
\
예. 리포팅 모드로 실행되는 rippled 서버는 P2P 네트워크에 연결하지 않고 네트워크에 연결된 하나 이상의 rippled 서버에서 검증된 데이터를 추출하므로 P2P 모드 서버를 하나 이상 실행해야 합니다.\
\
**이미 rippled을 설치했습니다. 리포팅 모드를 사용하도록 구성 파일을 업데이트하고 rippled을 다시 시작할 수 있나요?**\
\
아니요. 현재 리포팅 모드를 사용하려면 소스를 다운로드하고 rippled를 빌드해야 합니다. 리포팅 모드용 패키지를 제공하기 위한 이니셔티브가 진행 중입니다.\
\
**리포팅 모드에서 rippled을 실행하려면 P2P 모드에서 실행 중인 rippled 서버도 하나 이상 필요합니다. 디스크 공간이 두 배로 필요하다는 뜻인가요?**\
\
기본 데이터 저장소의 위치에 따라 답은 달라집니다. Cassandra를 기본 데이터 저장소로 사용하는 경우, 리포팅 모드 서버는 로컬 디스크에 훨씬 적은 양의 데이터를 저장합니다. PostgreSQL 서버는 원격일 수도 있습니다. 이러한 방식으로 여러 리포팅 모드 서버가 동일한 데이터를 공유하도록 할 수 있습니다.

마지막으로, P2P 모드 서버는 최근 기록만 보관하면 되지만 리포팅 모드 서버는 장기간의 기록을 보관합니다.

rippled을 실행하기 위한 시스템 요구사항에 대한 자세한 내용은 rippled 시스템 요구사항을 참조하세요.

**PostgreSQL 또는 Cassandra 데이터베이스에서 가져온 데이터의 유효성을 어떻게 확인할 수 있나요?**

rippled가 리포팅 모드에서 실행되면 구성 파일에 지정된 ETL 소스의 유효성이 검증된 데이터만 제공합니다. P2P 모드에서 다른 사람의 rippled 서버를 ETL 소스로 사용하는 경우, 해당 서버를 암묵적으로 신뢰하는 것입니다. 그렇지 않은 경우, P2P 모드에서 자체 rippled 노드를 실행해야 합니다.\
\
**API를 사용하지 않고 관계형 데이터베이스에 기존 SQL 쿼리를 수행할 수 있나요?**

기술적으로는 원한다면 데이터베이스에 직접 액세스할 수 있습니다. 하지만 데이터는 바이너리 blob으로 저장되며, blob에 있는 데이터에 액세스하려면 blob을 디코딩해야 합니다. 따라서 기존 SQL 쿼리는 데이터의 개별 필드를 찾아 필터링할 수 없기 때문에 훨씬 덜 유용합니다.
