# SignerList

_(MultiSign 수정안에 의해 추가되었습니다.)_

<mark style="background-color:yellow;">SignerList</mark> 객체 유형은 그룹으로서 개별 계정 대신 트랜잭션에 서명할 권한이 있는 당사자 목록을 나타냅니다. SignerList은 SignerList 세트 트랜잭션을 사용해 생성, 교체 또는 제거할 수 있습니다.

## SignerList JSON 예시 <a href="#example-signerlist-json" id="example-signerlist-json"></a>

```json
{
    "Flags": 0,
    "LedgerEntryType": "SignerList",
    "OwnerNode": "0000000000000000",
    "PreviousTxnID": "5904C0DC72C58A83AEFED2FFC5386356AA83FCA6A88C89D00646E51E687CDBE4",
    "PreviousTxnLgrSeq": 16061435,
    "SignerEntries": [
        {
            "SignerEntry": {
                "Account": "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW",
                "SignerWeight": 2
            }
        },
        {
            "SignerEntry": {
                "Account": "raKEEVSGnKSD9Zyvxu4z6Pqpm4ABH8FS6n",
                "SignerWeight": 1
            }
        },
        {
            "SignerEntry": {
                "Account": "rUpy3eEg8rqjqfUoLeBnZkscbKbFsKXC3v",
                "SignerWeight": 1
            }
        }
    ],
    "SignerListID": 0,
    "SignerQuorum": 3,
    "index": "A9C28A28B85CD533217F5C0A0C7767666B093FA58A0F2D80026FCC4CD932DDC7"
}
```

## SignerList 필드

SignerList 객체에는 다음과 같은 필드가 있습니다:

| 이름                | JSON 유형 | 내부 유형  | 필수 여부 | 설명                                                                                            |
| ----------------- | ------- | ------ | ----- | --------------------------------------------------------------------------------------------- |
| Flags             | 숫자      | UInt32 | 예     | 이 서명자 목록에 대해 활성화된 boolean 플래그의 비트맵입니다. 자세한 내용은 SignerList 플래그를 참조하세요.                         |
| LedgerEntryType   | 문자열     | UInt16 | 예     | 0x0053문자열에 매핑된 값은 SignerList이 개체가 SignerList 개체임을 나타냅니다.                                      |
| OwnerNode         | 문자열     | UInt64 | 예     | 디렉터리가 여러 페이지로 구성된 경우 소유자 디렉터리의 어느 페이지가 이 개체에 연결되는지 나타내는 힌트입니다.                                |
| PreviousTxnID     | 문자열     | 해시256  | 예     | 가장 최근에 이 개체를 수정한 트랜잭션의 식별 해시입니다.                                                              |
| PreviousTxnLgrSeq | 숫자      | UInt32 | 예     | 가장 최근에 이 개체를 수정한 트랜잭션이 포함된 ledger의 인덱스입니다.                                                    |
| SignerEntries     | 배열      | 정렬     | 예     | 이 서명자 목록의 일부인 당사자를 나타내는 서명자 항목 개체의 배열입니다.                                                     |
| SignerListID      | 숫자      | UInt32 | 예     | 이 서명자 목록의 ID입니다. 현재는 항상 0으로 설정되어 있습니다. 향후 개정에서 계정에 대한 여러 서명자 목록을 허용하는 경우 변경될 수 있습니다.          |
| SignerQuorum      | 숫자      | UInt32 | 예     | 서명자 가중치의 대상 숫자입니다. 이 SignerList의 소유자에 대한 유효한 서명을 생성하려면 서명자는 가중치 합계가 이 값 이상인 유효한 서명을 제공해야 합니다. |

SignerEntries은 secp256k1 또는 ed25519 키를 사용하는 펀딩된 주소와 펀딩되지 않은 주소의 임의 조합일 수 있습니다.

## Signer Entry 객체

Signer Entry 필드의 각 멤버는 목록에서 해당 서명자를 설명하는 객체입니다. Signer Entry에는 다음과 같은 필드가 있습니다:

| 이름            | JSON 유형 | 내부 유형  | 설명                                                                                             |
| ------------- | ------- | ------ | ---------------------------------------------------------------------------------------------- |
| Account       | 문자열     | 계정 ID  | 서명이 다중 서명에 기여하는 XRP Ledger 주소. ledger에 입금된 주소일 필요는 없습니다.                                       |
| SignerWeight  | 숫자      | UInt16 | 이 서명자의 서명 가중치입니다. 다중 서명은 제공된 서명의 총 가중치가 서명자 목록의 SignerQuorum값을 충족하거나 초과하는 경우에만 유효합니다.          |
| WalletLocator | 문자열     | 해시256  | (선택 사항) 임의의 16진수 데이터입니다. 이는 서명자를 식별하거나 기타 관련 목적으로 사용할 수 있습니다. ( ExpandedSignerList 수정안에서 추가됨.) |

&#x20;다중 서명 트랜잭션을 처리할 때 서버는 트랜잭션 실행 시점에 ledger와 관련하여 계정 값을 조회합니다. 주소가 자금이 지원된 AccountRoot 객체에 해당하지 않으면 해당 주소와 연결된 마스터 개인 키만 유효한 서명을 생성하는 데 사용할 수 있습니다. 계정이 원장에 존재하는 경우, 해당 계정의 상태에 따라 달라집니다. 계정에 일반 키가 구성되어 있으면 일반 키를 사용할 수 있습니다. 계정의 마스터 키는 비활성화되어 있지 않은 경우에만 사용할 수 있습니다. 다중 서명은 다른 다중 서명의 일부로 사용할 수 없습니다.

## SignerList 플래그

([_MultiSignReserve_](https://xrpl.org/known-amendments.html#multisignreserve) 수정안에 의해 추가되었습니다.)

SignerList 객체는 다음과 같은 플래그 값을 가질 수 있습니다:

| 플래그 이름           | 16진수 값     | 소수점 값 | 설명                                                                                                                                                                                  |
| ---------------- | ---------- | ----- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| lsfOneOwnerCount | 0x00010000 | 65536 | 이 플래그가 활성화되면 이 SignerList는 소유자 reserve을 위해 하나의 항목으로 계산됩니다 . 그렇지 않으면 이 목록은 N+2 항목으로 계산되며 여기서 N은 포함된 서명자의 수입니다. MultiSignReserve 수정안이 활성화된 후 서명자 목록을 추가하거나 업데이트하면 이 플래그가 자동으로 활성화됩니다. |

## SignerList 및 Reserve

SignerList은 소유자의 reserve 요건에 기여합니다.

[MultiSignReserve](https://xrpl.org/known-amendments.html#multisignreserve) 수정안(2019-04-17 활성화)으로 인해 각 SignerList은 구성원 수에 관계없이 하나의 개체로 계산됩니다. 그 결과, 새 SignerList과 연결된 소유자 준비금은 2XRP입니다.

[MultiSignReserve](https://xrpl.org/known-amendments.html#multisignreserve) 수정 이전에 생성된 SignerList은 두 개의 객체로 계산되며, 목록의 각 구성원은 한 개로 계산됩니다. 결과적으로 SignerList과 연결된 총 소유자 준비금은 원장에 있는 단일 신뢰([RippleState](https://xrpl.org/ripplestate.html)) 또는 오퍼 객체에 필요한 reserve의 3배에서 10배까지입니다. 새롭고 감소된 reserve를 사용하도록 SignerList를 업데이트하려면 SignerList 세트 트랜잭션을 전송하여 SignerList을 업데이트합니다.

## SignerList ID 형식

SignerList 개체의 ID는 다음 값의 절반을 순서대로 연결한 SHA-512입니다:

* RippleState 스페이스 키(0x0053).
* SignerList 소유자의 계정 ID입니다.
* SignerList ID(현재 항상 0).
