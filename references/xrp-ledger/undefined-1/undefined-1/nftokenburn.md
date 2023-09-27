# NFTokenBurn

NFTokenBurn 트랜잭션은 토큰이 보관되어 있는 NFTokenPage에서 NFToken 객체를 제거하여 ledger에서 토큰을 효과적으로 제거(소각)하는 데 사용됩니다.

이 트랜잭션의 발신자는 소각할 NFToken의 소유자이어야 하며, NFToken에 lsfBurnable 플래그가 활성화된 경우 발행자 또는 발행자의 승인된 NFTokenMinter 계정이 대신할 수 있습니다.

이 작업이 성공하면 해당 NFToken이 제거됩니다. 이 연산으로 NFToken을 보유한 NFTokenPage가 비워지거나 통합이 발생하여 NFTokenPage가 제거되면, 소유자의 reserve requirement은 1만큼 감소합니다.

_(NonFungibleTokenV1\_1 수정안에 의해 추가되었습니다.)_

## NFTokenBurn JSON 예시

```
{
  "TransactionType": "NFTokenBurn",
  "Account": "rNCFjv8Ek5oDrNiMJ3pw6eLLFtMjZLJnf2",
  "Owner": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
  "Fee": "10",
  "NFTokenID": "000B013A95F14B0044F78A264E41713C64B5F89242540EE208C3098E00000D65"
}
```

## NFTokenBurn 필드

일반적인 필드 외에도 NFTokenBurn 트랜잭션은 다음 필드를 사용합니다:

| 필드          | JSON 유형 | 내부 유형 | 설명                                                                                                                                     |
| ----------- | ------- | ----- | -------------------------------------------------------------------------------------------------------------------------------------- |
| `NFTokenID` | 문자열     | 해시256 | 이 트랜잭션으로 제거할 NFToken입니다.                                                                                                               |
| `Owner`     | 문자열     | 계정 ID | (선택 사항) 소각할 NFToken의 소유자입니다. 해당 소유자가 이 트랜잭션을 전송하는 계정과 다른 경우에만 사용됩니다. 발행자 또는 승인된 마이너는 이 필드를 사용하여 lsfBurnable 플래그가 활성화된 NFT를 소각할 수 있습니다. |

## 오류 사례

모든 트랜잭션에서 발생할 수 있는 에러 외에도 NFTokenBurn 트랜잭션은 다음과 같은 트랜잭션 결과 코드를 생성할 수 있습니다:

| 에러 코드              | 설명                                    |
| ------------------ | ------------------------------------- |
| `temDISABLED`      | NonFungibleTokensV1 수정안이 활성화되지 않았습니다. |
| `tecNO_ENTRY`      | 지정한 항목을 `TokenID`찾을 수 없습니다.           |
| `tecNO_PERMISSION` | 계정에 토큰을 소각할 권한이 없습니다.                 |
