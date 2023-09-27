# AccountSet

AccountSet 트랜잭션은 XRP Ledger에 있는 계정의 속성을 수정 합니다.

## AccountSet JSON 예시

```
{
    "TransactionType": "AccountSet",
    "Account" : "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "Fee": "12",
    "Sequence": 5,
    "Domain": "6578616D706C652E636F6D",
    "SetFlag": 5,
    "MessageKey": "03AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB"
}
```

## AccountSet 필드

AccountSet 트랜잭션은 공통 필드 외에도 다음 필드를 사용합니다:

| 필드                                                             | JSON 유형 | [내부 유형](https://xrpl.org/serialization.html) | 설명                                                                                                                                                                 |
| -------------------------------------------------------------- | ------- | -------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [ClearFlag](https://xrpl.org/accountset.html#accountset-flags) | 숫자      | UInt32                                       | (선택 사항) 이 계정에 대해 비활성화할 플래그의 고유 식별자입니다.                                                                                                                             |
| [Domain](https://xrpl.org/accountset.html#domain)              | 문자열     | blob                                         | (선택 사항) 이 계정을 소유한 도메인으로, 도메인의 ASCII를 소문자로 나타내는 16진수 문자열입니다. 길이는 256바이트를 초과할 수 없습니다.                                                                                |
| EmailHash                                                      | 문자열     | 해시128                                        | (선택 사항) 임의의 128비트 값입니다. 일반적으로 클라이언트는 이것을 Gravatar 이미지 를 표시하는 데 사용할 이메일 주소의 md5 해시로 취급합니다.                                                                          |
| MessageKey                                                     | 문자열     | blob                                         | (선택 사항) 이 계정으로 암호화된 메시지를 보내기 위한 공개 키입니다. 키를 설정하려면 정확히 33바이트여야 하며, 첫 번째 바이트는 키 유형을 나타냅니다: secp256k1 키의 경우 0x02 또는 0x03, Ed25519 키의 경우 0xED입니다. 키를 제거하려면 빈 값을 사용합니다. |
| NFTokenMinter                                                  | 문자열     | blob                                         | (선택 사항) NFT토큰을 발행할 수 있는 다른 계정. (NonFungibleTokensV1\_1 수정안에 의해 추가되었습니다.)                                                                                           |
| [SetFlag](https://xrpl.org/accountset.html#accountset-flags)   | 숫자      | UInt32                                       | (선택 사항) 이 계정에 대해 활성화할 정수 플래그입니다.                                                                                                                                   |
| [TransferRate](https://xrpl.org/accountset.html#transferrate)  | 숫자      | UInt32                                       | (선택 사항) 사용자가 이 계정의 토큰을 전송할 때 부과하는 수수료로, 10억 분의 1 단위로 표시됩니다. 수수료가 없는 특수 케이스 0을 제외하고 2000000000보다 크거나 1000000000보다 작을 수 없습니다.                                        |
| [TickSize](https://xrpl.org/ticksize.html)                     | 숫자      | UInt8                                        | (선택 사항) 이 주소에서 발행한 통화와 관련된 오퍼에 사용할 틱 크기입니다. 해당 오퍼의 환율은 이만큼 반올림됩니다. 유효한 값은 3에서 15까지이며, 0은 비활성화합니다. (TickSize 수정안으로 추가됨)                                             |
| WalletLocator                                                  | 문자열     | 해시256                                        | (선택 사항) 임의의 256비트 값입니다. 지정된 경우 값은 계정의 일부로 저장되지만 내재된 의미나 요구 사항은 없습니다.                                                                                               |
| WalletSize                                                     | 숫자      | UInt32                                       | (선택 사항) 사용되지 않습니다. 이 필드는 AccountSet 트랜잭션에서 유효하지만 아무 작업도 수행하지 않습니다.                                                                                                 |

이러한 옵션 중 어느 것도 제공되지 않으면 AccountSet 트랜잭션은 트랜잭션 비용을 소멸시키는 것 외에 아무런 효과가 없습니다. 자세한 내용은 트랜잭션 취소 또는 건너뛰기를 참조하세요.

## 도메인

도메인 필드는 도메인의 소문자 ASCII의 16진수 문자열로 표시됩니다. 예를 들어, 도메인 example.com은 "6578616D706C652E636F6D"로 표시됩니다.

계정에서 도메인 필드를 제거하려면 도메인이 빈 문자열로 설정된 AccountSet을 보내면 됩니다.

계정의 도메인 필드에 어떤 도메인이든 입력할 수 있습니다. 계정과 도메인이 동일한 사람 또는 비즈니스의 소유임을 증명하려면 '양방향 링크'가 필요합니다:

* 소유하고 있는 계정의 도메인 필드에 소유하고 있는 도메인이 있어야 합니다.
* 해당 도메인에 소유한 계정과 선택 사항으로 XRP Ledger를 사용하는 방법에 대한 기타 정보를 나열하는 xrp-ledger.toml 파일을 호스팅합니다.

## AccountSet 플래그

계정에 대해 활성화 또는 비활성화할 수 있는 몇 가지 옵션이 있습니다. 계정 옵션은 상황에 따라 다양한 유형의 플래그로 표시됩니다:

* AccountSet 트랜잭션 유형에는 여러 개의 "AccountSet 플래그"(접두사 asf)가 있으며, SetFlag 매개변수로 전달될 때 옵션을 활성화하거나 ClearFlag 매개변수로 전달될 때 옵션을 비활성화할 수 있습니다. 최신 옵션에는 이 스타일의 플래그만 있습니다. 트랜잭션당 최대 하나의 asf 플래그를 활성화하고 트랜잭션당 최대 하나의 asf 플래그를 비활성화할 수 있습니다.
* AccountSet 트랜잭션 유형에는 Flags 매개변수에 전달할 때 특정 계정 옵션을 사용하거나 사용하지 않도록 설정하는 데 사용할 수 있는 여러 트랜잭션 플래그(접두사 tf)가 있습니다. 여러 개의 tf 플래그를 사용하여 하나의 트랜잭션에서 설정 조합을 활성화 및 비활성화할 수 있지만, 모든 설정에 tf 플래그가 있는 것은 아닙니다.
* AccountRoot ledger 객체 유형에는 특정 ledger 내 특정 계정 옵션의 상태를 나타내는 여러 개의 ledger 상태 플래그(접두사 lsf)가 있습니다. 이러한 설정은 트랜잭션이 변경할 때까지 적용됩니다.

계정 플래그를 활성화 또는 비활성화하려면 계정세트 트랜잭션의 SetFlag 및 ClearFlag 매개변수를 사용합니다. AccountSet플래그의 이름은 asf로 시작합니다.

모든 플래그는 기본적으로 비활성화되어 있습니다.

사용 가능한 AccountSet 플래그는 다음과 같습니다:

| 소수점 값                           | 해당 ledger 플래그 | 설명                              |                                                                                                                                                                                                          |
| ------------------------------- | ------------- | ------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| asfAccountTxnID                 | 5             | (없음)                            | 이 계정의 가장 최근 거래의 ID를 추적합니다.                                                                                                                                                                               |
| asfAuthorizedNFTokenMinter      | 10            | (없음)                            | 다른 계정이 이 계정을 대신하여 대체 불가능한 토큰(NFToken)을 발행할 수 있도록 허용합니다. AccountRoot 오브젝트의 NFTokenMinter 필드에 승인된 계정을 지정합니다. 승인된 마이너를 제거하려면 이 플래그를 활성화하고 NFTokenMinter 필드를 생략하세요. (NonFungibleTokenV1\_1 수정안에 의해 추가되었습니다.) |
| asfDefaultRipple                | 8             | lsfDefaultRipple                | 기본적으로 이 계정의신뢰선에서 rippled을 활성화합니다.                                                                                                                                                                        |
| asfDepositAuth                  | 9             | lsfDepositAuth                  | 계정에서 입금 승인을 활성화합니다. (DepositAuth 개정에 의해 추가되었습니다.)                                                                                                                                                        |
| asfDisableMaster                | 4             | lsfDisableMaster                | 마스터 키 쌍의 사용을 허용하지 않습니다. 계정이 일반 키 또는 서명자 목록과 같은 다른 트랜잭션 서명 방법을 구성한 경우에만 활성화할 수 있습니다.                                                                                                                      |
| asfDisallowIncomingCheck        | 13            | lsfDisallowIncomingCheck        | 수신 확인을 차단합니다. _(DisallowIncoming 수정안이 필요합니다.)_                                                                                                                                                           |
| asfDisallowIncomingNFTokenOffer | 12            | lsfDisallowIncomingNFTokenOffer | 들어오는 NFTokenOffers를 차단합니다._(DisallowIncoming 수정안이 필요합니다.)_                                                                                                                                               |
| asfDisallowIncomingPayChan      | 14            | lsfDisallowIncomingPayChan      | 들어오는 지불 채널을 차단하십시오. _(DisallowIncoming 수정안이 필요합니다.)_                                                                                                                                                     |
| asfDisallowIncomingTrustline    | 15            | lsfDisallowIncomingTrustline    | 들어오는 신뢰선을 차단합니다. _(DisallowIncoming 수정안이 필요합니다.)_                                                                                                                                                        |
| asfDisallowXRP                  | 삼             | lsfDisallowXRP                  | 이 계정으로 XRP를 보내서는 안 됩니다. (권고; XRP 원장 프로토콜에 의해 시행되지 않음.)                                                                                                                                                   |
| asfGlobalFreeze                 | 7             | lsfGlobalFreeze                 | 이 계정에서 발행한 모든 자산을 동결합니다.                                                                                                                                                                                 |
| asfNoFreeze                     | 6             | lsfNoFreeze                     | 개별 신뢰선을 동결하거나 글로벌 동결을 비활성화하는 기능을 영구적으로 포기합니다. 이 플래그는 활성화된 후에는 비활성화할 수 없습니다.                                                                                                                              |
| asfRequireAuth                  | 2             | lsfRequireAuth                  | 사용자가 이 주소에서 발행한 잔액을 보유하려면 승인이 필요합니다. 주소에 연결된 신뢰선이 없는 경우에만 활성화할 수 있습니다.                                                                                                                                   |
| asfRequireDest                  | 1             | lsfRequireDestTag               | 이 계정으로 거래를 보내려면 데스티네이션 태그가 필요합니다.                                                                                                                                                                        |

asfDisableMaster 또는 asfNoFreeze 플래그를 사용하려면 마스터 키 쌍으로 서명하여 트랜잭션을 승인해야 합니다. 일반 키 쌍이나 다중 서명은 사용할 수 없습니다. 일반 키 쌍 또는 다중 서명을 사용하여 마스터 키 쌍을 다시 사용하도록 설정하면 asfDisableMaster를 비활성화할 수 있습니다.

AccountSet 트랜잭션 유형에 특정한 다음 트랜잭션 플래그(tf 플래그)도 같은 용도로 사용됩니다. 제한된 공간으로 인해 일부 설정에는 연결된 tf 플래그가 없으며, 새 tf 플래그는 AccountSet 트랜잭션 유형에 추가되지 않습니다. 단일 트랜잭션으로 여러 설정을 사용하려면 tf 플래그와 asf 플래그를 조합하여 사용할 수 있습니다.

| 플래그 이름            | 16진수 값     | 소수점 값   | AccountSet 플래그로 대체됨        |
| ----------------- | ---------- | ------- | -------------------------- |
| tfRequireDestTag  | 0x00010000 | 65536   | asfRequireDest( SetFlag)   |
| tfOptionalDestTag | 0x00020000 | 131072  | asfRequireDest( ClearFlag) |
| tfRequireAuth     | 0x00040000 | 262144  | asfRequireAuth( SetFlag)   |
| tfOptionalAuth    | 0x00080000 | 524288  | asfRequireAuth( ClearFlag) |
| tfDisallowXRP     | 0x00100000 | 1048576 | asfDisallowXRP( SetFlag)   |
| tfAllowXRP        | 0x00200000 | 2097152 | asfDisallowXRP( ClearFlag) |

{% hint style="info" %}
Caution:

트랜잭션의 tf 및 asf 플래그의 숫자 값은 ledger의 '휴면' 계정에 설정한 값과 일치하지 않습니다. ledger에서 계정의 플래그를 읽으려면 계정 루트 플래그를 참조하세요.
{% endhint %}

## 들어오는 트랜잭션 차단하기

목적이 불분명한 수신 트랜잭션은 고객이 실수했을 때 이를 인지하고 실수에 따라 계좌를 환불하거나 잔액을 조정해야 하는 금융 기관에 불편을 초래할 수 있습니다. asfRequireDest 및 asfDisallowXRP 플래그는 사용자가 실수로 송금 사유가 불분명한 방식으로 자금을 송금하지 않도록 보호하기 위한 것입니다.

예를 들어, 대상 태그는 일반적으로 금융기관이 결제를 받을 때 어떤 호스트 잔액을 입금해야 하는지 식별하는 데 사용됩니다. 대상 태그가 생략되면 어느 계정에 입금해야 하는지 불분명하여 환불이 필요한 등의 문제가 발생할 수 있습니다. asfRequireDest 태그를 사용하면 수신되는 모든 결제에 목적지 태그를 지정하여 다른 사람이 실수로 모호한 결제를 보내지 못하도록 할 수 있습니다.

해당 화폐로 신뢰선을 생성하지 않음으로써 XRP가 아닌 화폐에 대한 원치 않는 수신 결제를 방지할 수 있습니다. XRP는 신뢰가 필요하지 않으므로, 사용자가 계정으로 XRP를 보내지 못하도록 하기 위해 asfDisallowXRP 플래그가 사용됩니다. 그러나 이 플래그는 XRP가 부족할 경우 계정을 사용할 수 없게 만들 수 있기 때문에 XRP 레저 프로토콜에서는 적용되지 않습니다. 대신, 클라이언트 애플리케이션은 asfDisallowXRP 플래그가 활성화된 계정에 대한 XRP 결제를 허용하지 않도록 해야 합니다.

들어오는 모든 결제를 차단하려면 입금 승인을 활성화하면 됩니다. 이렇게 하면 계정이 준비금 요건 미만이 아닌 한, XRP를 포함한 어떤 트랜잭션도 내게 송금할 수 없게 됩니다.

수신 거부 수정안이 활성화된 경우, 수신되는 모든 수표, NFT 토큰 오퍼, 결제 채널, 신탁 라인을 차단하는 옵션도 있습니다. 일반적으로 이러한 개체를 수신하는 것은 문제가 되지 않지만, 계정을 삭제하지 못하도록 차단할 수 있으며 내가 만든 개체 목록에 예상하지 못한 개체가 섞여 있으면 혼란스러울 수 있습니다. 들어오는 개체를 차단하려면 다음 계정 플래그 중 하나 이상을 사용하세요:

* asfDisallowIncomingCheck - 확인 개체의 경우
* asfDisallowIncomingNFTOffer - NFTokenOffer 오브젝트용
* asfDisallowIncomingPayChan - 페이채널 객체용
* asfDisallowIncomingTrustline - 리플 스테이트(신뢰선) 오브젝트용

트랜잭션이 이러한 ledger 항목 중 하나를 생성할 때 대상 계정에 해당 플래그가 활성화되어 있으면 트랜잭션이 실패하고 결과 코드가 텍노\_퍼미션(tecNO\_PERMISSION)으로 표시됩니다. 입금 승인과 달리 이러한 설정은 일반적으로 결제를 받는 데 방해가 되지 않습니다. 또한 이 설정을 사용하도록 설정해도 이러한 유형의 객체를 직접 생성하는 것이 중단되지는 않습니다(물론 트랜잭션 대상도 이 설정을 사용하지 않는 한).

## TransferRate

TransferRate 필드는 트랜잭션 상대방이 발행한 화폐를 이체할 때마다 부과할 수수료를 지정합니다.

HTTP 및 웹소켓 API에서 송금 수수료는 정수로 표시되며, 10억 단위가 도착하기 위해 전송해야 하는 금액입니다. 예를 들어 20%의 송금 수수료는 1200000000이라는 값으로 표시됩니다. 이 값은 1000000000보다 작을 수 없습니다. (이보다 작으면 트랜잭션을 전송할 때 돈을 공짜로 제공한다는 의미이므로 악용될 수 있습니다.) 1000000000의 바로 가기 값으로 0을 지정할 수 있으며, 이는 수수료가 없음을 의미합니다.

자세한 내용은 전송 수수료를 참조하세요.

## NFTokenMinter

승인된 채굴자를 제거하려면 ClearFlag를 10(asfAuthorizedNFTokenMinter)으로 설정하고 NFTokenMinter 필드를 생략합니다.
