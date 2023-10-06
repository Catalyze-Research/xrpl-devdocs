# Ledger 구조(Ledger Structure)

XRP Ledger는 블록체인의 일종으로, 데이터 블록의 순차적인 기록으로 구성됩니다. XRP 레저 블록체인의 블록을 Ledger 버전 또는 줄여서 Ledger이라고 합니다.

합의 프로토콜은 이전 Ledger 버전을 시작점으로 삼아 다음에 적용할 일련의 트랜잭션에 대해 검증자 간에 합의를 형성한 다음, 모든 사람이 해당 트랜잭션을 적용하여 동일한 결과를 얻었는지 확인합니다. 이 과정이 성공적으로 완료되면 새로운 검증 Ledger 버전이 생성됩니다. 거기서부터 프로세스가 반복되어 다음 Ledger 버전을 구축합니다.

각 Ledger 버전에는 상태 데이터, 트랜잭션 세트, 메타데이터가 포함된 헤더가 포함됩니다.

<figure><img src="../../.gitbook/assets/ledger.svg" alt=""><figcaption></figcaption></figure>



## 상태 데이터(State Data)

![](../../.gitbook/assets/ledger-state-data.svg)

상태 데이터는 이 Ledger 버전을 기준으로 모든 계정, 잔액, 설정 및 기타 정보의 스냅샷을 나타냅니다. 서버가 네트워크에 연결하면 가장 먼저 하는 일 중 하나는 새 트랜잭션을 처리하고 현재 상태에 대한 쿼리에 응답할 수 있도록 현재 상태 데이터의 전체 세트를 다운로드하는 것입니다. 네트워크의 모든 서버는 상태 데이터의 전체 복사본을 가지고 있으므로 모든 데이터는 공개되며 모든 복사본은 동일하게 유효합니다.

상태 데이터는 트리 형식으로 저장되는 Ledger 항목이라는 개별 개체로 구성됩니다. 각 Ledger 항목에는 상태 트리에서 조회하는 데 사용할 수 있는 고유한 256비트 ID가 있습니다.



## 트랜잭션 세트(Transaction Set)

<div align="left">

<figure><img src="../../.gitbook/assets/ledger-transaction-set (1).svg" alt=""><figcaption></figcaption></figure>

</div>

Ledger에 대한 모든 변경은 트랜잭션의 결과입니다. 각 Ledger 버전에는 특정 순서로 새로 적용된 트랜잭션의 그룹인 트랜잭션 세트가 포함되어 있습니다. 이전 Ledger 버전의 상태 데이터를 가져와서 그 위에 이 Ledger의 트랜잭션 세트를 적용하면 결과적으로 이 Ledger의 상태 데이터를 얻게 됩니다.

Ledger의 트랜잭션 세트에 있는 모든 트랜잭션은 다음 두 부분을 모두 포함합니다:

* 트랜잭션 발신자가 Ledger에 지시한 내용을 보여주는 트랜잭션 설명.
* 트랜잭션이 어떻게 처리되었는지, 트랜잭션이 Ledger의 상태 데이터에 어떤 영향을 미쳤는지 정확히 보여주는 트랜잭션 메타데이터.



## Ledger Header



Ledger 헤더는 Ledger 버전을 요약하는 데이터 블록입니다. 보고서의 표지와 마찬가지로, Ledger 버전을 고유하게 식별하고, 내용을 나열하며, 다른 메모와 함께 생성된 시간을 표시합니다. Ledger 헤더에는 다음 정보가 포함됩니다:

&#x20;

<div align="left">

<figure><img src="../../.gitbook/assets/ledger-index-icon (1).svg" alt=""><figcaption><p>Ledger Index</p></figcaption></figure>

</div>

Ledger 인덱스는 체인에서 해당 원장 버전의 포지션을 식별합니다. 이는 제네시스 Ledger이라고 알려진 시작 지점으로 되돌아가 한 단계 낮은 인덱스를 가진 Ledger을 기반으로 구축됩니다. 이는 모든 트랜잭션과 결과의 공개 기록을 형성합니다.



<div align="left">

<figure><img src="../../.gitbook/assets/ledger-hash-icon.svg" alt=""><figcaption></figcaption></figure>

</div>



<div align="left">

<figure><img src="../../.gitbook/assets/ledger-parent-icon.svg" alt=""><figcaption></figcaption></figure>

</div>



<div align="left">

<figure><img src="../../.gitbook/assets/ledger-timestamp-icon (1).svg" alt=""><figcaption></figcaption></figure>

</div>



<div align="left">

<figure><img src="../../.gitbook/assets/ledger-state-data-hash-icon (1).svg" alt=""><figcaption></figcaption></figure>

</div>

<div align="left">

<figure><img src="../../.gitbook/assets/ledger-tx-set-hash-icon (1).svg" alt=""><figcaption></figcaption></figure>

</div>



<div align="left">

<figure><img src="../../.gitbook/assets/ledger-notes-icon (1).svg" alt=""><figcaption></figcaption></figure>

</div>



<div align="left">

<figure><img src="../../.gitbook/assets/ledger-validated-mark (1).svg" alt=""><figcaption></figcaption></figure>

</div>







