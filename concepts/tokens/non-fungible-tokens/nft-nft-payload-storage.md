# NFT 정보 저장소(NFT Payload Storage)

NFT는 블록체인에서 만들어집니다. 하지만 NFT의 중심정보(payload), 미디어, 메타데이터, 특성은 탈중앙화된 분산원장에 저장하든, 중앙화된 오프체인에 저장하든, 다양한 방식으로 저장될 수 있습니다.&#x20;

## 분산원장에 저장

만일 데이터가 256바이트를 넘지 않는다면, 사용자는 <mark style="background-color:yellow;">data://</mark> URI를 사용하여 데이터를 URI 필드에 곧바로 임베딩하는 방법을 고려할 수 있습니다. 이 방법은 데이터를 신뢰할 수 있고, 영구적이고, 빠른 데이터베이스에 저장한다는 이점이 있습니다.

## 탈중앙화된 오프 XRP

사용자는 다른 탈중앙화된 스토리지 솔루션을 통해 NFT 메타데이터를 저장할 수 있습니다.

IFPS와 Arweave는 탈중앙화 솔루션을 제공합니다. 하지만, 메타 데이터를 직접 가져오는 것에 대한 효율성 문제가 발생할 수 있습니다. IPFS나 Arweave에서 메타데이터를 가져오기 위해 쿼리를 보내는 것은 모던 웹사이트와 비교할 때, 여러장의 NFT 페이지를 게시하는 고퀄리티 미디어에서 요구 기준으로 삼는  속도를 만족하기 힘듭니다.

클라우드 스토리지 솔루션에 대한 몇가지 예시를 작성해놓은 블로그 포스트 [NFT Payload Storage Options](https://dev.to/ripplexdev/nft-payload-storage-options-569i) 를 보십시오.

## 중앙화된 오프 XRP

사용자는 중심 데이터를 다루고 있는 웹서버의 URI 필드를 사용할 수  있습니다.

대안으로, 그리고 분산 원장의 공간을 절약하기 위해, 사용자는 발행자의 <mark style="background-color:yellow;">Domain</mark> 필드를 <mark style="background-color:yellow;">AccounSet</mark>를 사용하여 설정할 수 있고, NFT ID를 해당 도메인의 path로써 사용할 수 있습니다. 예를 들어, NFT ID가 <mark style="background-color:yellow;">123ABC</mark>이고, 발행자의 도메인이 <mark style="background-color:yellow;">example.com</mark>이라면, 사용자는 데이터를 <mark style="background-color:yellow;">example.com/tokens/123ABC</mark>로부터 가져올 수 있습니다.
