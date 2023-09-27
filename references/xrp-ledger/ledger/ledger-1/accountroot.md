# AccountRoot

금AccountRoot ledger 항목 유형은 단일 계정과 해당 설정, XRP 잔액을 설명합니다.

## AccountRoot JSON 예시

```
{
    "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "AccountTxnID": "0D5FB50FA65C9FE1538FD7E398FFFE9D1908DFA4576D8D7A020040686F93C77D",
    "Balance": "148446663",
    "Domain": "6D64756F31332E636F6D",
    "EmailHash": "98B4375E1D753E5B91627516F6D70977",
    "Flags": 8388608,
    "LedgerEntryType": "AccountRoot",
    "MessageKey": "0000000000000000000000070000000300",
    "OwnerCount": 3,
    "PreviousTxnID": "0D5FB50FA65C9FE1538FD7E398FFFE9D1908DFA4576D8D7A020040686F93C77D",
    "PreviousTxnLgrSeq": 14091160,
    "Sequence": 336,
    "TransferRate": 1004999999,
    "index": "13F1A95D7AAB7108D5CE7EEAF504B2894B8C674E6D68499076441C4837282BF8"
}
```

## AccountRoot 필드

AccountRoot 객체에는 다음과 같은 필드가 있습니다:

| 필드                                                             | JSON 유형 | 내부 유형  | 필수 여부 | 설명                                                                                                                                                        |
| -------------------------------------------------------------- | ------- | ------ | ----- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Account`                                                      | 문자열     | 계정 ID  | 예     | 이 계정 의 식별(기본) 주소입니다.                                                                                                                                      |
| `AccountTxnID`                                                 | 문자열     | 해시256  | 아니요   | 이 계정에서 가장 최근에 보낸 트랜잭션의 식별 해시입니다. `AccountTxnID`트랜잭션 필드를 사용하려면 이 필드를 활성화해야 합니다. 활성화하려면 플래그가 활성화된 AccountSet 트랜잭션을`asfAccountTxnID` 로 보내십시오.                |
| `Balance`                                                      | 문자열     | 금액     | 아니요   | 계정의 현재 XRP 잔액은 문자열로 표시됩니다.                                                                                                                                |
| `BurnedNFTokens`                                               | 숫자      | Uint32 | 아니요   | 소각 된 이 계정의 발행된 대체 불가능한 토큰 의 총 수 입니다. 이 숫자는 `MintedNFTokens` 보다 항상 같거나 작습니다.                                                                               |
| `Domain`                                                       | 문자열     | blob   | 아니요   | 이 계정과 연결된 도메인입니다. JSON에서 이는 도메인의 ASCII 표현에 대한 16진수입니다. 길이는 256바이트를 초과할 수 없습니다.                                                                            |
| `EmailHash`                                                    | 문자열     | 해시128  | 아니요   | 이메일 주소의 md5 해시입니다. 클라이언트는 이를 사용하여 Gravatar 와 같은 서비스를 통해 아바타를 조회할 수 있습니다 .                                                                                 |
| `FirstNFTokenSequence`                                         | 숫자      | Uint32 | 아니요   | 첫 번째 대체 불가능한 토큰을 발행한 시점의 계정 시퀀스 번호입니다 . _(fixNFTokenRemint 수정안 에 의해 추가됨)_                                                                                 |
| [`Flags`](https://xrpl.org/accountroot.html#accountroot-flags) | 숫자      | Uint32 | 예     | 이 계정에 대해 활성화된 boolean 플래그의 비트맵입니다.                                                                                                                        |
| `LedgerEntryType`                                              | 문자열     | Uint16 | 예     | `0x0061`문자열에 매핑된 값은 `AccountRoot`이것이 AccountRoot 개체임을 나타냅니다.                                                                                              |
| `MessageKey`                                                   | 문자열     | blob   | 아니요   | 이 계정으로 암호화된 메시지를 보내는 데 사용할 수 있는 공개 키입니다. JSON에서는 16진수를 사용합니다. 정확히 33바이트여야 하며, 첫 번째 바이트는 키 유형을 나타냅니다: secp256k1 키의 경우 0x02 또는 0x03, Ed25519 키의 경우 0xED입니다. |
| `MintedNFTokens`                                               | 숫자      | Uint32 | 아니요   | 이 계정에 의해 그리고 이 계정을 대신하여 발행된 총 대체 불가능한 토큰의 수입니다 . _(NonFungibleTokensV1\_1 수정안에서 추가됨. )_                                                                   |
| `NFTokenMinter`                                                | 문자열     | 계정 ID  | 아니요   | 이 계정을 대신하여 대체 불가능한 토큰을 주조할 수 있는 다른 계정입니다 . _(NonFungibleTokensV1\_1 수정안에서 추가됨 )_                                                                          |
| `OwnerCount`                                                   | 숫자      | Uint32 | 예     | 이 계정이 ledger에서 소유하는 개체 수로 소유자 예약에 기여합니다.                                                                                                                  |
| `PreviousTxnID`                                                | 문자열     | 해시256  | 예     | 가장 최근에 이 개체를 수정한 트랜잭션의 식별 해시입니다.                                                                                                                          |
| `PreviousTxnLgrSeq`                                            | 숫자      | Uint32 | 예     | 가장 최근에 이 개체를 수정한 트랜잭션이 포함된 ledger의 인덱스입니다.                                                                                                                |
| `RegularKey`                                                   | 문자열     | 계정 ID  | 아니요   | 마스터 키 대신 이 계정의 트랜잭션에 서명하는 데 사용할 수 있는 키 쌍 의 주소입니다 . 이 값을 변경하려면 SetRegularKey 트랜잭션을 사용하십시오.                                                                 |
| `Sequence`                                                     | 숫자      | Uint32 | 예     | 이 계정에 대한 다음 유효한 트랜잭션의 시퀀스 번호입니다.                                                                                                                          |
| `TicketCount`                                                  | 숫자      | Uint32 | 아니요   | ledger에서 이 계정이 소유한 티켓 수 입니다. 이것은 자동으로 업데이트되어 계정이 한 번에 250개의 티켓이라는 엄격한 한도 내에서 유지되도록 합니다. 계정에 티켓이 없는 경우 이 필드는 생략됩니다. _(TicketBatch 수정안에서 추가됨.)_             |
| `TickSize`                                                     | 숫자      | Uint8  | 아니요   | 이 주소에서 발행된 통화와 관련된 오퍼의 환율에 사용할 유효 자릿수입니다. 유효한 값은 3에서 15까지 입니다. _(TickSize 수정안에 의해 추가되었습니다.)_                                                              |
| `TransferRate`                                                 | 숫자      | Uint32 | 아니요   | 이 계정에서 발행된 통화를 서로에게 보낼 때 다른 사용자에게 청구하는 이체 수수료입니다.                                                                                                         |
| `WalletLocator`                                                | 문자열     | 해시256  | 아니요   | 사용자가 설정할 수 있는 임의의 256비트 값입니다.                                                                                                                             |
| `WalletSize`                                                   | 숫자      | Uint32 | 아니요   | 미사용. (코드는 이 필드를 지원하지만 설정할 방법이 없습니다.)                                                                                                                      |

## AccountRoot 플래그

계정에 대해 활성화 또는 비활성화할 수 있는 몇 가지 옵션이 있습니다. 이러한 옵션은 [AccountSet](https://xrpl.org/accountset.html) 트랜잭션으로 변경할 수 있습니다. ledger에서 플래그는 비트 또는 연산으로 결합할 수 있는 이진 값으로 표시됩니다. ledger에 있는 플래그의 비트값은 트랜잭션에서 해당 플래그를 활성화 또는 비활성화하는 데 사용되는 값과 다릅니다. ledger 플래그의 이름은 lsf로 시작합니다.

AccountRoot 개체는 다음과 같은 플래그 값을 가질 수 있습니다:

| 플래그 이름                            | 16진수 값       | 소수점 값     | 해당 AccountSet 플래그                 | 설명                                                                                                                              |
| --------------------------------- | ------------ | --------- | --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| `lsfDefaultRipple`                | `0x00800000` | 8388608   | `asfDefaultRipple`                | 기본적으로 이 주소의 신뢰선에서 rippling을 사용하도록 설정합니다. 주소를 발급하는 데 필요하지만 다른 용도로는 사용하지 않는 것이 좋습니다.                                              |
| `lsfDepositAuth`                  | `0x01000000` | 16777216  | `asfDepositAuth`                  | 이 계정은 [DepositAuth](https://xrpl.org/depositauth.html) 활성화되어 있어 송금하는 거래와 사전 승인된 계정으로부터만 자금을 받을 수 있습니다. (DepositAuth 수정안으로 추가됨.) |
| `lsfDisableMaster`                | `0x00100000` | 1048576   | `asfDisableMaster`                | 마스터 키를 사용하여 이 계정의 거래에 서명하는 것을 허용하지 않습니다.                                                                                        |
| `lsfDisallowIncomingCheck`        | `0x08000000` | 134217728 | `asfDisallowIncomingCheck`        | 이 계정은 수신 수표를 차단합니다. (수신 거부 수정안이 필요합니다.)                                                                                         |
| `lsfDisallowIncomingNFTokenOffer` | `0x04000000` | 67108864  | `asfDisallowIncomingNFTokenOffer` | 이 계정은 들어오는 NFTokenOffers를 차단합니다. (DisallowIncoming 수정안이 필요합니다.)                                                                 |
| `lsfDisallowIncomingPayChan`      | `0x10000000` | 268435456 | `asfDisallowIncomingPayChan`      | 이 계정은 들어오는 NFTokenOffers를 차단합니다. (DisallowIncoming 수정안이 필요합니다.)                                                                 |
| `lsfDisallowIncomingTrustline`    | `0x20000000` | 536870912 | `asfDisallowIncomingTrustline`    | 이 계정은 들어오는 신뢰선을 차단합니다. (수신 거부 수정안이 필요합니다.)                                                                                      |
| `lsfDisallowXRP`                  | `0x00080000` | 524288    | `asfDisallowXRP`                  | 클라이언트 애플리케이션은 이 계정으로 XRP를 전송해서는 안 됩니다. (권고 사항이며 프로토콜에 의해 강제되지 않습니다.)                                                            |
| `lsfGlobalFreeze`                 | `0x00400000` | 4194304   | `asfGlobalFreeze`                 | 이 계정에서 발행된 모든 자산은 동결됩니다.                                                                                                        |
| `lsfNoFreeze`                     | `0x00200000` | 2097152   | `asfNoFreeze`                     | 이 계정은 연결된 신뢰 선을 동결할 수 없습니다. 일단 활성화하면 비활성화할 수 없습니다.                                                                              |
| `lsfPasswordSpent`                | `0x00010000` | 65536     | (없음)                              | 이 계정은 무료 SetRegularKey 트랜잭션을 사용했습니다.                                                                                            |
| `lsfRequireAuth`                  | `0x00040000` | 262144    | `asfRequireAuth`                  | 이 계정은 다른 사용자를 개별적으로 승인해야 해당 사용자가 이 계정의 토큰을 보유할 수 있습니다.                                                                          |
| `lsfRequireDestTag`               | `0x00020000` | 131072    | `asfRequireDest`                  | 수신 결제가 데스티네이션 태그를 지정하도록 요구합니다.                                                                                                  |

## AccountRoot ID 형식

AccountRoot 개체의 ID는 다음 값의 절반을 순서대로 연결한 SHA-512입니다:

* 계정 스페이스 키(0x0061).
* 계정의 AccountID.
