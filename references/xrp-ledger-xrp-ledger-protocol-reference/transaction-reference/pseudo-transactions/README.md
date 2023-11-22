# Pseudo-Transactions

Pseudo-transactions은 사용자가 제출하거나 네트워크를 통해 전파되지 않습니다. 대신, 서버는 특정 프로토콜 규칙에 따라 제안된 Ledger에 직접 pseudo-transactions을 삽입할 수 있습니다. 충분한 서버가 동일한 pseudo-transactions을 제안하면 합의 프로세스가 이를 승인하고 해당 원장의 트랜잭션 데이터에 pseudo-transactions이 포함됩니다.

## 공통 필드에 대한 특수 값(Special Values for Common Fields)

일반 트랜잭션에 필요한 공통 필드([common fields](https://xrpl.org/transaction-common-fields.html)) 중 일부는 pseudo-transactions에 적합하지 않습니다. pseudo-transactions은 이러한 공통 필드에 다음과 같은 특수 값을 사용합니다:

| 필드            | JSON 유형 | [내부 유형](https://xrpl.org/serialization.html) | 값                                                                 |
| ------------- | ------- | -------------------------------------------- | ----------------------------------------------------------------- |
| Account       | 문자열     | 계정 ID                                        | [ACCOUNT\_ZERO](https://xrpl.org/accounts.html#special-addresses) |
| Fee           | 문자열     | 양                                            | 0                                                                 |
| Sequence      | 숫자      | UInt32                                       | 0                                                                 |
| SigningPubKey | 문자열     | 얼룩                                           | ""(빈 문자열)                                                         |
| TxnSignature  | 문자열     | 얼룩                                           | ""(빈 문자열)                                                         |

Pseudo-transactions은 다음과 같은 공통 필드를 정상적으로 사용합니다:

* `TransactionType`
* `Flags`

| 필드              | JSON 유형 | 내부 유형  | 설명                                                             |
| --------------- | ------- | ------ | -------------------------------------------------------------- |
| TransactionType | String  | UInt16 | _(필수)_ 트랜잭션 유형입니다.                                             |
| Flags           | Number  | UInt32 | _(선택 사항)_ 이 트랜잭션에 대한 비트 플래그 집합입니다. 특정 플래그의 의미는 거래 유형에 따라 다릅니다. |

*   ### [EnableAmendment](https://xrpl.org/enableamendment.html)

    트랜잭션 처리 변경을 활성화합니다.\

*   ### SetFee

    global reserve 및 트랜잭션 비용 설정을 변경합니다.\

*   ### UNLModify

    현재 오프라인으로 간주되는 신뢰할 수 있는 유효성 검증인 목록을 변경합니다.
