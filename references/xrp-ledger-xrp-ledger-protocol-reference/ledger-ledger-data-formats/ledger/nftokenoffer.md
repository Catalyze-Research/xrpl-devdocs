# NFTokenOffer

lsfTransferable 플래그가 설정된 토큰은 제안을 사용해 참여자 간에 전송할 수 있습니다. NFTokenOffer 객체는 NFToken 객체를 구매, 판매 또는 전송하는 제안을 나타냅니다. NFToken의 소유자는 NFTokenCreateOffer를 사용해 트랜잭션을 시작할 수 있습니다.

_(NonFungibleTokenV1\_1 수정안에 의해 추가되었습니다.)_

## NFTokenOffer JSON 예시 <a href="#example-nftokenoffer-json" id="example-nftokenoffer-json"></a>

```
{
    "Amount": "1000000",
    "Flags": 1,
    "LedgerEntryType": "NFTokenOffer",
    "NFTokenID": "00081B5825A08C22787716FA031B432EBBC1B101BB54875F0002D2A400000000",
    "NFTokenOfferNode": "0",
    "Owner": "rhRxL3MNvuKEjWjL7TBbZSDacb8PmzAd7m",
    "OwnerNode": "17",
    "PreviousTxnID": "BFA9BE27383FA315651E26FDE1FA30815C5A5D0544EE10EC33D3E92532993769",
    "PreviousTxnLgrSeq": 75443565,
    "index": "AEBABA4FAC212BF28E0F9A9C3788A47B085557EC5D1429E7A8266FB859C863B3"
}
```

## NFTokenOffer 필드

| 이름                  | JSON 유형 | 내부 유형  | 필수 여부 | 설명                                                                                                                                                                              |
| ------------------- | ------- | ------ | ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Amount`            | 화폐 금액   | 금액     | 예     | NFToken에 대해 예상되거나 제공되는 금액입니다. 토큰에 lsfOnlyXRP 플래그가 설정되어 있는 경우 금액은 XRP로 지정해야 합니다. XRP 이외의 자산을 지정하는 판매 오퍼는 0이 아닌 금액을 지정해야 합니다. XRP를 지정하는 판매 오퍼는 '무료'일 수 있습니다(즉, 금액 필드가 "0"일 수 있음). |
| `Destination`       | 문자열     | 계정 ID  | 아니요   | 이 오퍼의 대상이 되는 계정 ID입니다. 있는 경우 해당 계정만 오퍼를 수락할 수 있습니다.                                                                                                                             |
| `Expiration`        | 숫자      | UInt32 | 아니요   | 오퍼가 더 이상 활성화되지 않는 시간입니다. 이 값은 Ripple 에포크 이후의 시간(초)입니다.                                                                                                                          |
| `Flags`             | 숫자      | UInt32 | 예     | 플래그 번호 UInt32 예 다양한 옵션이나 설정을 지정하는 데 사용되는 이 객체와 관련된 플래그 집합입니다. 플래그는 아래 표에 나열되어 있습니다.                                                                                             |
| `LedgerEntryType`   | 문자열     | UInt16 | 예     | 0x0074 값은 NFTokenOffer 문자열에 매핑되며, 이는 NFToken을 거래하기 위한 오퍼임을 나타냅니다.                                                                                                               |
| `NFTokenID`         | 문자열     | 해시256  | 예     | 이 오퍼에서 참조하는 NFToken 오브젝트의 NFTokenID입니다.                                                                                                                                         |
| `NFTokenOfferNode`  | 문자열     | UInt64 | 아니요   | 내부 ledger, 토큰 구매 또는 판매 오퍼 디렉터리 내에서 이 토큰이 추적되고 있는 페이지를 적절히 나타냅니다. 이 필드를 통해 오퍼를 효율적으로 삭제할 수 있습니다.                                                                                 |
| `Owner`             | 문자열     | 계정 ID  | 예     | 오퍼를 생성하고 소유하는 계정의 소유자입니다. 현재 NFT 토큰의 소유자만 NFT 토큰을 판매하는 오퍼를 만들 수 있지만, 모든 계정이 NFT 토큰을 구매하는 오퍼를 만들 수 있습니다.                                                                         |
| `OwnerNode`         | 문자열     | UInt64 | 아니요   | 이 토큰이 추적되는 소유자 디렉터리 내부 페이지를 나타냅니다. 이 필드를 통해 오퍼를 효율적으로 삭제할 수 있습니다.                                                                                                               |
| `PreviousTxnID`     | 문자열     | 해시256  | 예     | 개체를 가장 최근에 수정한 트랜잭션의 해시를 식별합니다.                                                                                                                                                 |
| `PreviousTxnLgrSeq` | 숫자      | UInt32 | 예     | 가장 최근에 이 개체를 수정한 트랜잭션이 포함된 ledger의 인덱스입니다.                                                                                                                                      |

## NFTokenOffer 플래그

| 플래그 이름           | 16진수 값       | 소수점 값 | 설명                                        |
| ---------------- | ------------ | ----- | ----------------------------------------- |
| `lsfSellNFToken` | `0x00000001` | 1     | 활성화하면 오퍼는 판매 오퍼입니다. 그렇지 않으면 오퍼는 구매 오퍼입니다. |

## NFTokenOffer 트랜잭션

대체 가능한 토큰에 대한 제안과 달리 NFTokenOffer는 오더북에 저장되지 않으며, 자동으로 매칭되거나 실행되지 않습니다. 구매자는 NFT 토큰 구매를 제안하는 NFT 토큰 제안을 명시적으로 수락하도록 선택해야 합니다. 마찬가지로 판매자는 자신이 소유한 NFToken 객체를 구매하겠다고 제안하는 특정 NFToken 제안을 수락하도록 명시적으로 선택해야 합니다.

NFToken 트랜잭션을 위한 트랜잭션은 다음과 같습니다:

* NFTokenCreateOffer
* NFTokenCancelOffer
* NFTokenAcceptOffer

## NFTokenOffer 객체 찾기

각 NFToken에는 두 개의 디렉터리가 있습니다. 하나는 토큰 구매 제안을 포함하고 다른 하나는 토큰 판매 제안을 포함합니다. 마켓플레이스나 다른 클라이언트 애플리케이션은 이 디렉터리를 사용하여 사용자에게 NFToken 객체를 트랜잭션하는 제안을 찾아 표시하거나 자동으로 매칭하여 수락할 수도 있습니다.

## NFTokenOffer Reserve

각 NFTokenOffer 객체는 제안을 올리는 계정에 하나의 증분 예치금을 요구합니다. 이 글을 쓰는 현재 증분 예치금은 2 XRP입니다. 예치금은 제안을 취소하면 복구할 수 있습니다.

## NFTokenOffer ID 형식

NFTokenOffer 객체의 고유 ID(NFTokenOfferID)는 다음 값을 순서대로 연결한 결과입니다:

* NFTokenOffer 공백 키, 0x0074;
* 제안을 올린 계정의 계정 ID, 그리고
* NFTokenOffer를 생성한 NFTokenCreateOffer 트랜잭션의 시퀀스(또는 티켓)입니다.
