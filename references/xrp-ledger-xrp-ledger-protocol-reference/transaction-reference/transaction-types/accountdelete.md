# AccountDelete

_AccountDelete 수정안에 의해 추가됨_

AccountDelete 트랜잭션은 계정과 해당 계정이 XRP Ledger에서 소유하고 있는 모든 개체를 삭제하고, 가능한 경우 계정의 남은 XRP를 지정된 대상 계정으로 전송합니다. AccountDelete에 필요한 요건은 AccountDelete를 참조하세요.

## AccountDelete JSON 예시

```
{
    "TransactionType": "AccountDelete",
    "Account": "rWYkbWkCeg8dP6rXALnjgZSjjLyih5NXm",
    "Destination": "rPT1Sjq2YGrBMTttX4GZHjKu9dyfzbpAYe",
    "DestinationTag": 13,
    "Fee": "2000000",
    "Sequence": 2470665,
    "Flags": 2147483648
}
```

### AccountDelete 필드

AccountDelete 트랜잭션은 공통 필드 외에도 다음 필드를 사용합니다:

| 필드             | JSON 유형                                                      | [내부 유형](https://xrpl.org/serialization.html) | 설명                                                                        |
| -------------- | ------------------------------------------------------------ | -------------------------------------------- | ------------------------------------------------------------------------- |
| Destination    | 문자열 - [주소](https://xrpl.org/basic-data-types.html#addresses) | 계정 ID                                        | 송금 계정을 삭제한 후 남은 XRP를 받을 계좌의 주소입니다. 원장에 자금이 입금된 계정이어야 하며, 송금 계정이어서는 안 됩니다. |
| DestinationTag | 숫자                                                           | UInt32                                       | (선택 사항) 삭제된 계정의 남은 XRP를 받는 수신자를 위해 호스팅된 수신자 또는 기타 정보를 식별하는 임의의 목적지 태그입니다. |

## 특별 트랜잭션 비용

ledger 스팸에 대한 추가적인 억제책으로, AccountDelete 트랜잭션에는 일반적인 트랜잭션 비용보다 훨씬 높은 비용이 필요합니다. 표준 최소값인 0.00001 XRP 대신, AccountDelete는 최소 소유자 준비금(현재 2 XRP) 이상을 소멸해야 합니다. 이는 계정을 삭제해도 준비금을 완전히 회수할 수 없기 때문에 과도한 새 계정 생성을 방지합니다.

트랜잭션 비용은 트랜잭션이 계정을 삭제하지 못하더라도 트랜잭션이 검증된 ledger에 포함될 때 항상 적용됩니다. (오류 사례 참조) 계정을 삭제할 수 없는 경우 높은 트랜잭션 비용을 지불할 가능성을 크게 줄이려면 fail\_hard를 활성화한 상태로 트랜잭션을 제출하세요.

## 오류 사례

모든 트랜잭션에서 발생할 수 있는 오류 외에도 AccountDelete 트랜잭션에서 다음과 같은 트랜잭션 결과 코드가 발생할 수 있습니다:

| 에러 코드               | 설명                                                                                                                                                                                                                 |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| temDISABLED         | [DeletableAccounts ](https://xrpl.org/known-amendments.html#deletableaccounts)수정안이 활성화되지 않은 경우 발생합니다.                                                                                                              |
| temDST\_IS\_SRC     | 대상이 트랜잭션 발신자(계정 필드)와 일치하는 경우 발생합니다.                                                                                                                                                                                |
| tecDST\_TAG\_NEEDED | 계정에 데스티네이션 태그가 필요. 하지만 필드가 제공되지 않은 경우 발생합니다.                                                                                                                                                                       |
| tecNO\_DST          | 목적지 계정에 입금 승인이 필요하고 발신자가 사전 승인을 받지 않은 경우 발생합니다.                                                                                                                                                                    |
| tecNO\_PERMISSION   | 목적지 계정에 입금 승인이 필요하고 발신자가 사전 승인을 받지 않은 경우 발생합니다.                                                                                                                                                                    |
| tecTOO\_SOON        | 발신자 Sequence번호가 너무 높을 경우 발생합니다. 트랜잭션 번호에 256을 더한 값은 현재 Ledger Index Sequence 보다 작아야 합니다 . 이렇게 하면 이 계정이 삭제된 후 부활하는 경우 이전 트랜잭션이 재생되지 않습니다.                                                                           |
| tecHAS\_OBLIGATIONS | 삭제할 계정이 ledger에서 삭제할 수 없는 객체에 연결된 경우 발생합니다. (여기에는 다른 계정이 소유한 경우에도 에스크로 및 예를 들어 NFT의 주조와 같은 다른 계정에서 생성된 개체가 포함 됩니다[.](https://github.com/XRPLF/rippled/blob/develop/src/ripple/app/tx/impl/DeleteAccount.cpp#L197)) |
| tefTOO\_BIG         | 보내는 계정이 ledger에서 1000개 이상의 개체에 연결된 경우 발생합니다. 해당 개체 중 일부가 먼저 개별적으로 삭제된 경우 재시도 시 트랜잭션이 성공할 수 있습니다.                                                                                                                   |
