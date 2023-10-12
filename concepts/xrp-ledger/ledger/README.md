# Ledger 역사

\
[컨센서스 프로세스](../../consensus-protocol/consensus-structure.md)는 이전 히스토리에 대한 일련의 [유효한 ledger 버전](../../undefined-1/ledgers.md)을 생성하며, 각 버전은 일련의 [트랜잭션](../../transactions/)을 적용하는 방식으로 이전 버전에서 유도됩니다. 모든 [<mark style="background-color:yellow;">rippled</mark> 서버](../)는 로컬로 ledger 버전과 트랜잭션 히스토리를 저장합니다. 서버가 저장하는 트랜잭션 기록은 해당 서버가 온라인 상태였던 기간과 가져와서 보관하도록 설정된 양에 따라 달라집니다.

XRP Ledger P2P 네트워크의 서버들은 컨센서스 프로세스의 일부로 서로 간에 트랜잭션 및 기타 데이터를 공유합니다. 각 서버는 독립적으로 새로운 ledger 버전을 구축하고 신뢰하는 유효성 검증인과 결과를 비교하여 일관성을 확인합니다. (신뢰하는 검증의 컨센서스가 서버의 결과와 일치하지 않으면, 서버는 일관성을 달성하기 위해 필요한 데이터를 피어로부터 가져옵니다.) 서버는 사용 가능한 히스토리를 채우기 위해 피어로부터 이전 데이터를 다운로드할 수 있습니다. ledger의 구조는 데이터의 암호 [해시](../../../references/xrp-ledger/undefined/)를 사용하여 어느 서버에든 데이터의 무결성과 일관성을 검증할 수 있습니다.

## 데이터베이스(Databases)

서버는 ledger 상태 데이터와 트랜잭션을 키-값 저장소인 _ledger 저장소_에 유지합니다. 추가로, <mark style="background-color:yellow;">rippled</mark>는 트랜잭션 히스토리와 특정 설정 변경을 추적하기 위해 몇 개의 SQLite 데이터베이스 파일을 유지합니다.

일반적으로, <mark style="background-color:yellow;">rippled</mark> 서버가 실행되지 않을 때는 해당 서버의 모든 데이터베이스 파일을 삭제하는 것이 안전합니다. (예를 들어 서버의 저장소 설정을 변경하거나 테스트 네트워크에서 프로덕션 네트워크로 전환하는 경우 이 작업을 수행해야 할 수 있습니다.)

## 사용 가능한 히스토리(Available History)

&#x20;XRP Ledger의 모든 데이터와 트랜잭션은 공개적으로 되어 있으며 누구나 검색하거나 조회할 수 있습니다. 그러나 서버는 로컬로 사용 가능한 데이터만 검색할 수 있습니다. 서버가 사용할 수 없는 ledger 버전이나 트랜잭션을 조회하려고 하면 서버는 해당 데이터를 찾을 수 없다는 응답을 반환합니다. 해당 히스토리를 가진 다른 서버는 동일한 조회에 대해 성공적으로 응답할 수 있습니다. XRP ledger 데이터를 사용하는 비즈니스를 운영한다면 서버가 사용 가능한 히스토리 양을 고려해야 합니다.

[server\_info 메소드](../../../references/http-websocket-apis/api-1/undefined-5/server\_info.md)는 <mark style="background-color:yellow;">complete\_ledgers</mark> 필드에 서버에서 사용 가능한 ledger 버전의 수를 보고합니다.

## 히스토리 가져오기(Fetching History)

XRP Ledger 서버가 시작되면 가장 먼저 최신 유효한 ledger의 완전한 사본을 가져오는 것이 우선입니다. 이후에는 ledger 진행 상황을 따라가게 됩니다. 서버는 동기화 이후에 발생하는 ledger 히스토리의 누락된 부분을 채우고, 동기화 이전의 히스토리를 역사적으로 채울 수 있습니다. (ledger 히스토리의 누락된 부분은 서버가 일시적으로 네트워크와 동기화를 유지하기 어려워지거나 네트워크 연결을 잃거나 다른 일시적인 문제가 있을 때 발생할 수 있습니다.) ledger 히스토리를 다운로드할 때, 서버는 누락된 데이터를 [피어 서버](../peer-protocol.md)로부터 요청하고, 암호 [해시](../../../references/xrp-ledger/undefined/)를 사용하여 데이터의 무결성을 확인합니다.

히스토리 역사 채우기는 서버의 가장 낮은 우선 순위 작업 중 하나이므로, 누락된 히스토리를 채우는 데에는 시간이 오래 걸릴 수 있습니다. 특히 서버가 바쁘거나 하드웨어 및 네트워크 사양이 충분하지 않은 경우에는 더 오랜 시간이 걸릴 수 있습니다. 하드웨어 사양에 대한 권장 사항은 [용량 계획](../../../tutorials/rippled/rippled/undefined-4.md)을 참조하십시오. 히스토리 역사를 채우려면 적어도 하나의 직접 연결된 피어 서버가 해당 히스토리를 가지고 있어야 합니다. 서버의 P2P 연결을 관리하는 방법에 대한 자세한 내용은 [피어링 구성](../../../tutorials/rippled/undefined/)을 참조하십시오.

XRP ledger는 데이터의 고유한 해시를 사용하여 데이터를 식별합니다. XRP ledger의 상태 데이터는 [LedgersHashes 객체 유형](../../../references/xrp-ledger/ledger/ledger-1/ledgerhashes.md)의 형태로 ledger의 히스토리에 대한 간략한 요약을 포함합니다. 서버는 ledger 해시 객체를 사용하여 가져올 ledger 버전을 알고, 수신한 ledger 데이터가 올바르고 완전한지 확인합니다.

## 히스토리 역사 채우기(Backfilling)

[![Updated in: rippled 1.6.0](https://img.shields.io/badge/Updated%20in-rippled%201.6.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/1.6.0)

서버가 다운로드하려는 히스토리 양은 서버의 구성에 따라 다릅니다. 서버는 이미 사용 **가능한 가장 오래된 ledger까지** 히스토리를 다운로드하여 누락된 부분을 채우려고 자동적으로 시도합니다. <mark style="background-color:yellow;">\[ledger\_history]</mark> 설정을 사용하여 서버가 해당 시점 이후의 히스토리를 역사적으로 채울 수 있습니다. 그러나 서버는 삭제 예정인 ledger를 다운로드하지 않습니다.

<mark style="background-color:yellow;">\[ledger\_history]</mark> 설정은 현재 유효한 ledger 이전에 축적할 최소한의 ledger 수를 정의합니다. <mark style="background-color:yellow;">full</mark>이라는 특수한 값을 사용하여 네트워크의 전체 히스토리를 다운로드할 수 있습니다. 특정한 ledger 수를 지정하는 경우, 해당 수는 <mark style="background-color:yellow;">online\_deletion</mark> 설정보다 크거나 같아야 합니다. <mark style="background-color:yellow;">\[ledger\_history]</mark>를 사용하여 서버가 더 적은 양의 히스토리를 다운로드하지 못하도록 설정할 수는 없습니다. 서버가 저장하는 히스토리 양을 줄이려면 [online\_deletion](../../../infrastructure/configure-rippled/data-retention/online-deletion.md) 설정을 변경하십시오.

## 전체 히스토리(Full History)

XRP Ledger 네트워크의 일부 서버는 "전체 히스토리" 서버로 구성됩니다. 이러한 서버는 다른 추적 서버보다 훨씬 더 많은 디스크 공간이 필요하며, 모든 사용 가능한 XRP ledger 히스토리를 수집하고 **온라인 삭제를 사용하지 않습니다.**

XRP Ledger 재단은 커뮤니티 구성원이 운영하는 전체 히스토리 서버 세트에 대한 접근 제공합니다 (자세한 내용은 [xrplcluster.com](https://xrplcluster.com/)을 참조하십시오). Ripple 또 공개 서비스로서 <mark style="background-color:yellow;">s2.ripple.com</mark>에서 전체 히스토리 서버 세트를 제공합니다.&#x20;

전체 히스토리 서버 제공 업체는 리소스 낭비로 판단되거나 시스템에 과도한 부하를 주는 접근 차단할 권리를 보유합니다.

{% hint style="info" %}
Tip:

일부 암호화폐 네트워크와 달리, XRP Ledger의 서버는 현재 상태를 파악하고 현재 거래를 따라잡기 위해 전체 내역이 필요하지 않습니다.
{% endhint %}

전체 히스토리 설정에 대한 지침은 [전체 히스토리 구성](../../../tutorials/rippled/rippled-1/undefined-5.md) 참조하십시오.

## 히스토리 샤딩(History Sharding)

비용이 많이 드는 단일 기계에서 XRP Ledger의 전체 히스토리를 저장하는 대신 여러 서버를 구성하여 각각 모든 ledger 히스토리의 일부를 저장할 수 있습니다. [히스토리 샤딩](../../../infrastructure/configure-rippled/data-retention/history-sharding.md) 기능을 사용하여 이를 가능하게 할 수 있으며, 이는 샤드 스토어라는 별도의 저장 영역에 ledger 히스토리의 범위를 저장합니다. 피어 서버가 특정 데이터를 요청할 때 (앞서 설명한 [히스토리 가져오기](./#undefined-2)에 따라), 서버는 ledger 저장소나 샤드 스토어에서 데이터를 사용하여 응답할 수 있습니다.

온라인 삭제는 샤드 스토어에서 **삭제되지 않습니다**. 그러나 온라인 삭제를 설정하여 서버의 ledger 저장소에서 적어도 32768개의 ledger 버전을 유지하도록 구성한 경우, 서버는 ledger 저장소에서 완전한 샤드를 샤드 스토어로 복사한 다음 자동으로 ledger 저장소에서 해당 샤드를 삭제할 수 있습니다.

자세한 내용은 [히스토리 샤딩 구성을](../../../tutorials/rippled/rippled-1/undefined-4.md) 참조하십시오

## 참고

* **Concepts:**
  * [Ledgers](https://xrpl.org/ledgers.html)
  * [Consensus](https://xrpl.org/consensus.html)
* **Tutorials:**
  * [Configure `rippled`](https://xrpl.org/configure-rippled.html)
    * [Configure Online Deletion](https://xrpl.org/configure-online-deletion.html)
    * [Configure Advisory Deletion](https://xrpl.org/configure-advisory-deletion.html)
    * [Configure History Sharding](https://xrpl.org/configure-history-sharding.html)
    * [Configure Full History](https://xrpl.org/configure-full-history.html)
* **References:**
  * [ledger method](https://xrpl.org/ledger.html)
  * [server\_info method](https://xrpl.org/server\_info.html)
  * [ledger\_request method](https://xrpl.org/ledger\_request.html)
  * [can\_delete method](https://xrpl.org/can\_delete.html)
  * [ledger\_cleaner method](https://xrpl.org/ledger\_cleaner.html)

### See Also <a href="#see-also" id="see-also"></a>

&#x20;
