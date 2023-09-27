# Ledger 데이터 형식

XRP Ledger의 각 ledger 버전은 세 부분으로 구성됩니다:

* Ledger 헤더: 이 ledger 버전 자체에 대한 메타데이터입니다.
* 트랜잭션 세트: 이 ledger 버전을 생성하기 위해 실행된 모든 트랜잭션입니다.
* 상태 데이터: 이 ledger 버전을 기준으로 계정, 설정, 잔액을 나타내는 개체의 전체 기록입니다. ('계정 상태'라고도 합니다.)

## 상태 데이터

각 ledger 버전의 상태 데이터는 특정 시점의 모든 설정, 잔액, 관계를 총체적으로 나타내는 ledger 객체 집합으로, ledger 항목이라고도 합니다. 프로토콜은 상태 데이터에 개체를 저장하거나 검색하기 위해 해당 개체의 고유한 ledger 개체 ID를 사용합니다.

피어 프로토콜에서 ledger 개체는 표준 바이너리 형식을 갖습니다. 리플드 API에서 ledger 개체는 JSON 개체로 표현됩니다.

Ledger 개체의 데이터 필드는 개체 유형에 따라 다르며, XRP Ledger은 다음 유형을 지원합니다:

* [AccountRoot](https://xrpl.org/accountroot.html)

하나의 계정에 대한 설정, XRP 잔액 및 기타 메타데이터.

* [Amendments](https://xrpl.org/amendments-object.html)

활성화 및 보류 중인 수정 상태의 싱글톤 객체입니다.

* [Check](https://xrpl.org/check.html)

수취처에서 현금으로 교환할 수 있는 수표입니다.

* [DepositPreauth](https://xrpl.org/depositpreauth-object.html)

승인이 필요한 계정으로 송금하기 위한 사전 승인 기록입니다.

* [DirectoryNode](https://xrpl.org/directorynode.html)

다른 개체에 대한 링크를 포함합니다.

* [Escrow](https://xrpl.org/escrow-object.html)

조건부 결제를 위해 보류된 XRP를 포함합니다.

* [FeeSettings](https://xrpl.org/feesettings.html)

컨센서스가 승인한 기본 트랜잭션 비용과 준비금 요건을 가진 싱글톤 객체입니다.

* [LedgerHashes](https://xrpl.org/ledgerhashes.html)

내역 조회를 위한 이전 ledger 버전의 해시 목록입니다.

* [NegativeUNL](https://xrpl.org/negativeunl.html)

현재 오프라인 상태인 것으로 추정되는 검증인 목록입니다.

* [NFTokenOffer](https://xrpl.org/nftokenoffer.html)

NFT를 구매하거나 판매하기 위한 오퍼를 생성합니다.

* [NFTokenPage](https://xrpl.org/nftokenpage.html)

NFT토큰을 기록하는 ledger 구조입니다.

* [Offer](https://xrpl.org/offer.html)

화폐 트랜잭션을 위한 주문입니다.

* [PayChannel](https://xrpl.org/paychannel.html)

비동기 XRP 결제를 위한 채널입니다.

* [RippleState](https://xrpl.org/ripplestate.html)

두 계정을 연결하여 두 계정 사이의 한 화폐 잔액을 추적합니다. 트러스트 라인의 개념은 이 객체 유형을 추상화한 것입니다.

* [SignerList](https://xrpl.org/signerlist.html)

다중 서명 트랜잭션에 대한 주소 목록입니다.

* [Ticket](https://xrpl.org/ticket.html)

티켓은 나중에 사용하기 위해 따로 설정한 계정 시퀀스 번호를 추적합니다.
