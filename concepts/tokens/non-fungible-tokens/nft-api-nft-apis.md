# NFT 관련 API(NFT APIs)

이 페이지는 편리한 참조를 위해 NFT와 관련된 트랜잭션 및 요청을 나열합니다.

## NFT 객체들&#x20;

* [NFToken](../../../references/xrp-ledger-xrp-ledger-protocol-reference/basic-data-types/nftoken.md) 데이터 타입 - ledger에 저장된 NFT 객체입니다.&#x20;
* Ledger 객체들&#x20;
  * [NFTokenOffer 객체](../../../references/xrp-ledger-xrp-ledger-protocol-reference/ledger-ledger-data-formats/ledger/nftokenoffer.md) - NFT를 구매하거나 판매하기 위한 제안입니다.
  * [NFTokenPage 객체](../../../references/xrp-ledger-xrp-ledger-protocol-reference/ledger-ledger-data-formats/ledger/nftokenpage.md) - NFT 페이지는 최대 32개의 NFT 객체를 보유할 수 있습니다. 실제로 각 NFT 페이지는 일반적으로 16-24개의 NFT를 보유합니다.&#x20;

## NFT 트랜잭션들&#x20;

* [NFTokenMint](../../../references/xrp-ledger-xrp-ledger-protocol-reference/transaction-reference/transaction-types/nftokenmint.md) - NFT를 생성합니다.
* [NFTokenCreateOffer](../../../references/xrp-ledger-xrp-ledger-protocol-reference/transaction-reference/transaction-types/nftokencreateoffer.md) - NFT를 구매하거나 판매하기 위한 제안을 생성합니다.
* [NFTokenCancelOffer](../../../references/xrp-ledger-xrp-ledger-protocol-reference/transaction-reference/transaction-types/nftokencanceloffer.md) - NFT를 구매하거나 판매하기 위한 제안을 취소합니다.
* [NFTokenAcceptOffer](../../../references/xrp-ledger-xrp-ledger-protocol-reference/transaction-reference/transaction-types/nftokenacceptoffer.md) - NFT를 구매하거나 판매하기 위한 제안을 수락합니다.
* [NFTokenBurn](../../../references/xrp-ledger-xrp-ledger-protocol-reference/transaction-reference/transaction-types/nftokenburn.md) - NFT를 영구히 파괴합니다.

## NFT 요청들&#x20;

* [account\_nfts 메소드](../../../references/http-websocket-apis/api-1/undefined/account\_nfts.md) - 계정이 소유한 비교가 불가능한 토큰 목록을 가져옵니다.&#x20;
* [nft\_buy\_offers 메소드](../../../references/http-websocket-apis/api-1/undefined-2/nft\_buy\_offers.md) - 지정된 NFToken 객체에 대한 구매 제안 목록을 가져옵니다.&#x20;
* [nft\_sell\_offers 메소드](../../../references/http-websocket-apis/api-1/undefined-2/nft\_sell\_offers.md) - 지정된 NFToken 객체에 대한 판매 제안 목록을 가져옵니다.&#x20;
* [subscribe 메소드](../../../references/http-websocket-apis/api-1/undefined-4/undefined.md) - 특정 주제에 대한 업데이트를 듣습니다. 예를 들어, 마켓플레이스는 플랫폼에 나열된 NFT들의 상태에 대한 실시간 업데이트를 발행할 수 있습니다.&#x20;
* [unsubscribe 메소드](../../../references/http-websocket-apis/api-1/undefined-4/undefined-1.md) - NFT에 대한 업데이트 수신을 중지합니다.&#x20;

## 클리오

클리오 서버는 캐싱된 정보를 기반으로 정보 요청을 처리함으로써 전체 네트워크 성능을 향상시킵니다, 이로써 XRP Ledger의 유효자들이 트랜잭션 처리에 집중할 수 있습니다. 클리오 서버는 더 자세한 응답을 제공하는 추가 요청 유형을 처리하는 모든 일반적인 XRP Ledger 요청 유형을 처리합니다.

## 클리오 특정 NFT 요청들&#x20;

* [nft\_info](../../../references/http-websocket-apis/api-1/undefined-5/nft\_info.md) - 지정된 NFT에 대한 현재 상태 정보를 가져옵니다.
* [nft\_history](../../../references/http-websocket-apis/api-1/undefined-5/nft\_history.md) - 지정된 NFT에 대한 과거 트랜잭션 메타데이터를 가져옵니다.&#x20;

클리오 서버에 접속하려면 해당 URL과 클리오 포트(일반적으로 51233)로 요청을 보내면 됩니다. 공개 클리오 API 서버는 SLA(Servie Level Agreement)가 없으며 우선순위로 수정되는 책임도 없습니다. 비즈니스 케이스에서 지속적인 모니터링과 정보 요청이 필요한 경우, 자체 클리오 서버 인스턴스를 설정하는 것을 고려해야 합니다. [install-clio-on-ubuntu](../../../tutorials/undefined-3/undefined.md)를 참조하세요.
