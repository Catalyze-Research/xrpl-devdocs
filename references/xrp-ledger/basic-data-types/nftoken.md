# NFToken

NFToken 객체는 대체 불가능한 단일 토큰(NFT)을 나타냅니다. 이 토큰은 단독으로 저장되지 않고 다른 NFToken 객체와 함께 NFTokenPage 객체에 포함됩니다.

(NonFungibleTokensV1\_1 개정에 의해 추가되었습니다.)

NFToken JSON 예시

```
{
    "NFTokenID": "000B013A95F14B0044F78A264E41713C64B5F89242540EE208C3098E00000D65",
    "URI": "ipfs://bafybeigdyrzt5sfp7udm7hu76uh7y26nf4dfuylqabf3oclgtqy55fbzdi"
}
```

본격적인 ledger 항목과 달리 NFToken에는 오브젝트 유형이나 오브젝트의 현재 소유자를 식별하는 필드가 없습니다. NFToken 객체는 객체 유형을 암시적으로 정의하고 소유자를 식별하는 페이지로 그룹화됩니다.

## NFTokenID

NFTokenID, 선택 사항, 문자열, 해시256

이 복합 필드는 토큰을 고유하게 식별하며 다음 섹션으로 구성됩니다.

A) 16비트: NFToken에 특정한 플래그 또는 설정을 식별합니다.

B) 16비트: 해당 NFToken과 관련된 전송 수수료(있는 경우)를 인코딩합니다.

C) 발행자의 160비트 계정 식별자

D) 발행자가 지정한 32비트 NFTokenTaxon

E) (자동 생성된) 단조롭게 증가하는 32비트 시퀀스 번호.

<figure><img src="https://xrpl.org/img/nftoken1.png" alt=""><figcaption></figcaption></figure>

16비트 플래그, 이체 수수료 필드, 32비트 NFTokenTaxon, 시퀀스 번호 필드는 big-endian 형식으로 저장됩니다.

## NFToken 플래그(NFToken Flags)

플래그는 NFToken 객체와 관련된 속성 또는 기타 옵션입니다.

| 플래그 이름            | 플래그 값    | 설명                                                                                                                                                                                                      |
| ----------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `lsfBurnable`     | `0x0001` | 활성화하면 발행자(또는 발행자가 승인한 주체)가 이 NFToken을 파기할 수 있습니다. 객체의 소유자는 언제든지 이를 삭제할 수 있습니다.                                                                                                                          |
| `lsfOnlyXRP`      | `0x0002` | 활성화하면 이 NFT 토큰은 XRP로만 제공하거나 판매할 수 있습니다.                                                                                                                                                                 |
| `lsfTrustLine`    | `0x0004` | **DEPRECATED** 활성화하면 송금 수수료를 보관하기 위한 트러스트 라인을 자동으로 생성합니다. 그렇지 않으면 발행자가 해당 토큰에 대한 트러스트 라인이 없는 경우 대체 가능한 토큰 금액으로 이 NFToken을 구매하거나 판매하지 못합니다. 수정된 fixRemoveNFTokenAutoTrustLine은 이 플래그를 활성화하는 것을 무효로 만듭니다. |
| `lsfTransferable` | `0x0008` | 활성화된 경우, 이 NFToken은 한 홀더에서 다른 홀더로 전송할 수 있습니다. 그렇지 않으면 발행자와만 전송하거나 발행자로부터만 전송할 수 있습니다.                                                                                                                   |
| `lsfReservedFlag` | `0x8000` | 이 플래그는 나중에 사용하기 위해 예약되어 있습니다. 이 플래그를 설정하려는 시도는 실패합니다.                                                                                                                                                   |

NFToken 플래그는 불변이며, NFTokenMint 트랜잭션 중에만 설정할 수 있고 나중에 변경할 수 없습니다.

## Example

이 예시에서는 세 가지 플래그를 설정합니다: lsfBurnable(0x0001), lsfOnlyXRP(0x0002), lsfTransferable(0x0008). 1+2+8 = 11, 또는 빅 엔디안에서 0x000B

## TransferFee

TransferFee 값은 토큰의 2차 판매에 대해 발행자가 부과하는 백분율 수수료(1/100,000 단위)를 지정합니다. 이 필드의 유효한 값은 0에서 50,000 사이입니다. 값이 1이면 0.001% 또는 1/10 베이시스 포인트(bps)에 해당하며, 0%에서 50% 사이의 전송 수수료율을 허용합니다.

## Example

이 값은 이체 수수료를 314, 즉 0.314%로 설정합니다.

<figure><img src="https://xrpl.org/img/nftokenb.png" alt=""><figcaption></figcaption></figure>

## 발행자 식별(Issuer Identification)

NFTokenID의 세 번째 섹션은 발행자의 공개 주소를 큰 엔디안 단위로 표현한 것입니다.

<figure><img src="https://xrpl.org/img/nftokenc.png" alt=""><figcaption></figcaption></figure>

## NFTokenTaxon

네 번째 섹션은 발행자가 생성한 NFTokenTaxon입니다.

<figure><img src="https://xrpl.org/img/nftokend.png" alt=""><figcaption></figcaption></figure>

발행자는 동일한 NFTokenTaxon으로 여러 개의 NFToken 객체를 발행할 수 있으며, NFToken 객체가 여러 페이지에 분산되도록 하기 위해 다섯 번째 섹션인 일련번호를 난수 생성기의 시드로 사용하여 NFTokenTaxon을 스크램블링합니다. 스크램블링된 값은 NFToken과 함께 저장되지만, 스크램블링되지 않은 값은 실제 NFTokenTaxon입니다.

스크램블된 NFTokenTaxon의 버전은 발행자가 지정한 스크램블된 버전인 0xBC8B858E임을 알 수 있습니다. 그러나 NFTokenTaxon의 실제 값은 스크램블되지 않은 값입니다.

## 토큰 시퀀스(Token Sequence)

다섯 번째 섹션은 발행자가 NFToken을 생성할 때마다 증가하는 시퀀스 번호입니다.

<figure><img src="https://xrpl.org/img/nftokene.png" alt=""><figcaption></figcaption></figure>

NFTokenMint 트랜잭션은 발행자 계정의 MintedTokens 필드에 따라 NFTokenID의 이 부분을 자동으로 설정합니다. 발행자의 AccountRoot 객체에 MintedToken 필드가 없는 경우, 해당 필드는 값이 0인 것으로 간주되며, 해당 필드 값은 정확히 1씩 증가합니다.

## URI

URI 필드는 NFToken과 연관된 데이터 또는 메타데이터를 가리킵니다. 이 필드는 HTTP 또는 HTTPS URL일 필요는 없으며, IPFS URI, 마그넷 링크, RFC 2379 "데이터" URL 또는 완전히 사용자 정의 인코딩일 수도 있습니다. URI의 유효성은 확인되지 않지만 필드의 길이는 최대 256바이트로 제한됩니다.

{% hint style="info" %}
Caution:

URI는 변경할 수 없으므로 예를 들어 더 이상 존재하지 않는 웹사이트로 연결되는 경우 아무도 업데이트할 수 없습니다.
{% endhint %}

## NFToken 데이터 및 메타데이터 검색하기(Retrieving NFToken Data and Metadata)

기능을 희생하거나 불필요한 제한을 두지 않고 NFT토큰의 풋프린트를 최소화하기 위해 XRPL NFT에는 임의의 데이터 필드가 없습니다. 대신, 데이터는 별도로 유지되고 NFT토큰에 의해 참조됩니다. URI는 해시에 대한 불변 콘텐츠와 NFToken 객체에 대한 모든 변경 가능한 데이터에 대한 참조를 제공합니다.

URI 필드는 특히 비 전통적인 P2P URL을 참조할 때 유용합니다. 예를 들어, 행성 간 파일 시스템(IPFS)을 사용하여 NFToken 데이터 또는 메타데이터를 저장하는 채굴자는 URI 필드를 사용하여 각기 다른 사용 사례에 적합한 다양한 방식으로 IPFS의 데이터를 참조할 수 있습니다. NFT 데이터를 저장하는 데 사용할 수 있는 IPFS 링크 유형에 대한 자세한 내용은 IPFS를 사용한 NFT 데이터 저장 모범 사례를 참조하세요,

## TXT 레코드 형식(TXT Record Format)

텍스트 레코드의 형식은 다음과 같습니다.

```
xrpl-nft-data-token-info-v1 IN TXT "https://host.example.com/api/token-info/{nftokenid}"
```

정보를 쿼리하려고 할 때 {nftokenid} 문자열을 64바이트 16진수 문자열로 요청된 NFTokenID로 대체합니다.

구현은 TXT 레코드가 있는지 확인하고 있는 경우 해당 쿼리 문자열을 사용해야 합니다. 문자열이 없으면 구현은 기본 URL을 사용하려고 시도해야 합니다. 도메인이 example.com이라고 가정하면 기본 URL은 다음과 같습니다:

```
https://example.com/.well-known/xrpl-nft/{nftokenid}
```

NFTokenMint 트랜잭션을 사용하여 NFToken 객체를 생성합니다. 선택적으로 NFTokenBurn 트랜잭션을 사용하여 NFToken 오브젝트를 소멸할 수 있습니다.
