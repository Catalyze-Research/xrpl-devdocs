# NFT의 고정 공급 보장하기(Guaranteeing a Fixed Supply of NFTs)

일부 프로젝트에서는 발행 계정(issuing account)에서 발행된 NFT의 수가 고정된 수량을 초과하지 않도록 보장하고 싶을 수 있습니다.

고정된 수의 NFT를 보장하려면:

1. 발행자(issuer)로 새 계정을 생성하고 자금을 조달합니다. 이 계정은 컬렉션 내의 토큰의 발행자(issuer)입니다. [계정 생성](../../undefined-2/)을 참조하세요.
2. <mark style="background-color:yellow;">AccountSet</mark>을 사용하여 운영 지갑을 발행자(issuer)의 인증된 발행자로 지정합니다. [다른 계정에게 NFT 발행 권한 부여](nft-authorizing-another-account-to-mint-your-nfts.md)를 참조하세요.
3. 운영 계정을 사용하여 <mark style="background-color:yellow;">NFTokenMint</mark>를 사용하여 토큰을 발행합니다. 운영 지갑은 발행자를 위해 발행된 모든 토큰을 보유합니다. 일괄 발행을 참조하세요.
4. <mark style="background-color:yellow;">AccountSet</mark>을 사용하여 운영 지갑을 발행자의 인증된 발행자에서 제거합니다.
5. 발행자 계정을 "블랙홀" 처리합니다. [마스터 키 페어 비활성화](../../../tutorials/tasks/manage-account-settings/undefined-2.md)를 참조하세요.

이 시점에서 발행 계정으로서 발행자의 주소를 가진 새 토큰을 발행하는 것은 불가능합니다.

{% hint style="info" %}
Caution

계정을 "블랙홀" 처리하면 당신을 포함해서 누구도 NFTs의 미래 판매에 대한 전송 수수료를 받지 못하게 됩니다.
{% endhint %}

