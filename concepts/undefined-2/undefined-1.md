# 다중 서명

XRP Ledger의 다중 서명은 여러 개인 키의 조합을 사용하여 XRP Ledger에 대한 [트랜잭션을 승인](../transactions/#undefined-2)하는 방법입니다. 다중 서명, [마스터 키 쌍](undefined.md#undefined-7) 및 [일반 키 쌍](undefined.md#undefined-9)을 포함하여 주소에 대해 임의의 인증 방법 조합을 사용할 수 있습니다. (단, 하나의 요구 사항은 하나 이상의 메소드를 활성화해야 한다는 것입니다.)

다중 서명의 이점은 다음과 같습니다:

* 서로 다른 장치에서 키를 요구하여 악의적인 행위자가 사용자 대신 트랜잭션을 전송하기 위해 여러 컴퓨터를 손상시켜야 합니다.&#x20;
* 여러 사용자 간에 주소의 보관을 공유할 수 있으며, 각 사용자는 해당 주소에서 트랜잭션을 보내는 데 필요한 여러 키 중 하나만 가지고 있습니다.&#x20;
* 사용자가 정상적으로 서명할 수 없거나 서명할 수 없는 경우 주소를 제어할 수 있는 사용자 그룹에 주소에서 트랜잭션을 보낼 권한을 위임할 수 있습니다.
* ... 기타 등등.

## 서명자 목록

다중 서명하려면 먼저 서명할 수 있는 주소 목록을 만들어야 합니다.

[SignerListSet 트랜잭션](../../references/xrp-ledger/undefined-1/undefined-1/signerlistset.md)은 주소에서 트랜잭션을 승인할 수 있는 주소 집합인 서명자 목록을 정의합니다. 서명자 목록에 1-32개의 주소를 포함할 수 있습니다. 목록에 사용자의 주소를 포함할 수 없으며 중복된 항목이 있을 수 없습니다. 목록의 서명자 가중치 및 쿼럼 설정을 사용하여 필요한 서명 수, 조합을 제어할 수 있습니다.

_(_[_ExpandedSignerList수정안_](../xrp-ledger/amendments/undefined.md#expandedsignerlist)_에 의해 업데이트됨.)_

## 서명자 무게&#x20;

목록의 각 서명자에게 가중치를 할당합니다. 가중치는 목록의 다른 서명자에 대한 서명자의 권한을 나타냅니다. 값이 높을수록 더 많은 권한을 가집니다. 개별 가중치 값은 2^16-1을 초과할 수 없습니다.&#x20;

## 쿼럼

목록의 쿼럼 값은 트랜잭션을 승인하는 데 필요한 최소 가중치 합계입니다. 쿼럼은 0보다 크고 서명자 목록의 가중치 합계보다 작거나 같아야 합니다. 즉, 지정된 서명자 가중치로 쿼럼을 달성할 수 있어야 합니다.

## 지갑 위치

각 서명자의 목록에는 최대 256비트의 임의의 데이터를 추가할 수도 있습니다. 이 데이터는 네트워크에서 필요하거나 사용되지 않지만 스마트 계약이나 다른 애플리케이션에서 서명자에 대한 기타 데이터를 식별하거나 확인하는 데 사용될 수 있습니다.

_(_[_ExpandedSignerList수정안_](../xrp-ledger/amendments/undefined.md#expandedsignerlist)_에 의해 추가됨.)_

## 서명자 가중치 및 쿼럼 사용 예제&#x20;

가중치와 쿼럼을 사용하면 각 트랜잭션에 대해 관리하는 책임 있는 참여자에게 할당된 상대적인 신뢰와 권한에 기반하여 적절한 감독 수준을 설정할 수 있습니다.

공유 계정 사용 사례에서는 쿼럼이 1인 목록을 생성한 후, 모든 참여자에게 가중치 1을 부여할 수 있습니다. 그들 중 아무나 한 명의 승인만 필요합니다.

매우 중요한 계정의 경우, 쿼럼을 3으로 설정하고 가중치가 1인 3명의 참여자를 가질 수 있습니다. 모든 참여자는 각 트랜잭션에 동의하고 승인해야 합니다.

다른 계정은 쿼럼이 3인 경우도 있을 수 있습니다. CEO에게 3의 가중치, 3명의 부사장에게 2의 가중치, 3명의 이사에게 1의 가중치를 할당합니다. 이 계정의 트랜잭션를 승인하려면 3명의 이사 (총 가중치 3), 1명의 부사장과 1명의 이사 (총 가중치 3), 2명의 부사장 (총 가중치 4) 또는 CEO (총 가중치 3)의 승인이 필요합니다. 이전 세 가지 사용 사례에서는 일반 키를 구성하지 않고 마스터 키를 비활성화하여 다중 서명이 [트랜잭션 승인](../transactions/#undefined-2)의 유일한 방법이 되도록 설정합니다.

"백업 계획"으로 다중 서명 목록을 생성하는 시나리오도 있을 수 있습니다. 계정 소유자는 보통 트랜잭션에 일반 키(다중 서명 키가 아님)를 사용합니다. 안전을 위해 계정 소유자는 가중치가 각각 1인 3명의 친구를 포함하고 쿼럼이 3인 서명자 목록을 추가합니다. 계정 소유자가 개인 키를 분실한 경우, 친구들에게 정규 키를 대체하기 위해 다중 서명 트랜잭션를 멀티 서명할 수 있습니다.

## 다중 서명 트랜잭션 전송하기&#x20;

다중 서명 트랜잭션를 성공적으로 제출하려면 다음을 수행해야 합니다:

* 트랜잭션를 보내는 주소(<mark style="background-color:yellow;">계정</mark> 필드에 지정됨)는 [ledger에 <mark style="background-color:yellow;">SignerList</mark> 객체](../../references/xrp-ledger/ledger/ledger-1/signerlist.md)를 가져야 합니다. 이를 설정하는 방법에 대한 지침은 [다중 서명 설정](../../tutorials/tasks/manage-account-settings/undefined-3.md)을 참조하세요.
* 트랜잭션에는 <mark style="background-color:yellow;">SigningPubKey</mark> 필드를 빈 문자열로 포함해야 합니다.
* 트랜잭션에는 [<mark style="background-color:yellow;">Signers</mark> 필드](../../references/xrp-ledger/undefined-1/undefined.md#undefined-5)가 포함되어야 하며, 서명 배열을 포함해야 합니다.&#x20;
* <mark style="background-color:yellow;">Signers</mark> 배열에 있는 서명은 <mark style="background-color:yellow;">SignerList</mark>에서 정의된 서명자와 일치해야 합니다.&#x20;
* 제공된 서명에 대해 해당 서명자와 관련된 총 가중치는 SignerList의 쿼럼과 동일하거나 크거나 같아야 합니다.
* [트랜잭션 비용](../transactions/transaction-cost.md)(<mark style="background-color:yellow;">수수료</mark> 필드에 지정됨)은 제공된 서명의 수에 대해 최소한 (N+1) 배 이상이어야 합니다. 여기서 N은 제공된 서명의 수입니다.&#x20;
* 모든 트랜잭션 필드는 서명을 수집하기 전에 정의되어야 합니다. 어떤 필드도 [자동으로 채울 수](../../references/xrp-ledger/undefined-1/undefined.md#undefined) 없습니다.&#x20;
* 이진 형식으로 제시된 경우 <mark style="background-color:yellow;">Signers</mark> 배열은 서명자 주소의 숫자 값에 따라 정렬되어야 합니다. 가장 낮은 값부터 시작합니다. (JSON으로 제출하는 경우, [submit\_multisigned 메소드](../../references/http-websocket-apis/api-1/undefined-1/submit\_multisigned.md)가 이를 자동으로 처리합니다.)
