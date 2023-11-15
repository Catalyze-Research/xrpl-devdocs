# DepositPreauth

DepositPreauth 객체는 한 계정에서 다른 계정으로의 사전 승인을 추적합니다. DepositPreauth 트랜잭션은 이러한 객체를 생성합니다.

사전 승인을 제공한 계정에 입금 승인이 필요하지 않는 한 트랜잭션 처리에는 영향을 미치지 않습니다. 이 경우 사전 승인을 받은 계정은 사전 승인을 제공한 계정으로 직접 결제 및 기타 트랜잭션을 보낼 수 있습니다. 사전 승인은 단방향이며 반대 방향으로 진행되는 결제에는 영향을 미치지 않습니다.

## DepositPreauth JSON 예시

```
{
  "LedgerEntryType": "DepositPreauth",
  "Account": "rsUiUMpnrgxQp24dJYZDhmV4bE3aBtQyt8",
  "Authorize": "rEhxGqkqPPSxQ3P25J66ft5TwpzV14k2de",
  "Flags": 0,
  "OwnerNode": "0000000000000000",
  "PreviousTxnID": "3E8964D5A86B3CD6B9ECB33310D4E073D64C865A5B866200AD2B7E29F8326702",
  "PreviousTxnLgrSeq": 7,
  "index": "4A255038CC3ADCC1A9C91509279B59908251728D0DAADB248FFE297D0F7E068C"
}
```

## DepositPreauth 필드

DepositPreauth 객체에는 다음과 같은 필드가 있습니다:

| 필드                  | JSON 유형 | [내부 유형](https://xrpl.org/serialization.html) | 필수 여부 | 설명                                                                                                                                                   |
| ------------------- | ------- | -------------------------------------------- | ----- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Account`           | 문자열     | 계정                                           | 예     | 사전 승인을 부여한 계정입니다. (사전 승인된 결제 대상.)                                                                                                                    |
| `Authorize`         | 문자열     | 계정                                           | 예     | 사전 승인을 받은 계정입니다. (사전 승인된 지불의 발신자.)                                                                                                                   |
| `Flags`             | 숫자      | Uint32                                       | 예     | 이 개체에 대해 활성화된 boolean 플래그의 비트맵입니다. 현재 프로토콜은 `DepositPreauth`개체에 대한 플래그를 정의하지 않습니다. 값은 항상 `0`입니다.                                                     |
| `LedgerEntryType`   | 문자열     | Uint16                                       | 예     | `0x0070`문자열에 매핑된 값은 `DepositPreauth`이것이 DepositPreauth 개체임을 나타냅니다.                                                                                   |
| `OwnerNode`         | 문자열     | Uint64                                       | 예     | 디렉터리가 여러 페이지로 구성된 경우 발신자 소유자 디렉터리의 어느 페이지가 이 개체에 연결되어 있는지 나타내는 힌트입니다. **참고:** 해당 값은 계정에서 파생될 수 있으므로 객체에는 객체가 포함된 소유자 디렉터리로 직접 연결되는 링크가 포함되어 있지 않습니다. |
| `PreviousTxnID`     | 문자열     | 해시256                                        | 예     | 가장 최근에 이 개체를 수정한 트랜잭션의 식별 해시입니다.                                                                                                                     |
| `PreviousTxnLgrSeq` | 숫자      | Uint32                                       | 예     | 가장 최근에 이 개체를 수정한 트랜잭션이 포함된 ledger 인덱스입니다.                                                                                                            |

## DepositPreauth ID 형식

DepositPreauth 객체의 ID는 다음 값의 절반을 순서대로 연결한 SHA-512입니다:

* DepositPreauth 스페이스 키(0x0070).
* 이 객체 소유자(이 객체를 생성한 DepositPreauth 트랜잭션의 발신자, 즉 사전 승인을 부여한 트랜잭션의 발신자)의 AccountID입니다.
* 사전 승인된 계정의 AccountID(이 객체를 생성한 DepositPreauth 트랜잭션의 Authorized 필드, 즉 사전 승인을 받은 계정)
