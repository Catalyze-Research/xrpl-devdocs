# AMMDelete

_(Requires the_ [_AMM amendment_](https://xrpl.org/known-amendments.html#amm) _)_

자동으로 완전히 삭제할 수 없는 빈 자동화된 시장조성자(AMM) 인스턴스를 삭제합니다.

일반적으로  [AMMWithdraw transaction](https://xrpl.org/ammwithdraw.html) 거래는 AMM 풀에서 모든 자산을 인출할 때 AMM과 관련된 모든 원장 항목을 자동으로 삭제합니다. 그러나 한 번의 트랜잭션으로 제거하기에는 AMM 계정에 대한 트러스트 라인이 너무 많으면 AMM을 완전히 제거하기 전에 트랜잭션이 중지될 수 있습니다. 마찬가지로 AMMDelete 트랜잭션은 최대 512개의 트러스트 라인을 제거하며, 모든 트러스트 라인과 관련 AMM을 삭제하려면 여러 번의 AMMDelete 트랜잭션이 필요할 수 있습니다. 모든 경우에 마지막 트랜잭션만 AMM 및 AccountRoot 원장 항목을 삭제합니다.

### Example AMMDelete JSON <a href="#example-ammdelete-json" id="example-ammdelete-json"></a>

```
{
    "Account" : "rJVUeRqDFNs2xqA7ncVE6ZoAhPUoaJJSQm",
    "Asset" : {
        "currency" : "XRP"
    },
    "Asset2" : {
        "currency" : "TST",
        "issuer" : "rP9jPyP5kyvFRb6ZiRghAGw5u8SGAmU4bd"
    },
    "Fee" : "10",
    "Flags" : 0,
    "Sequence" : 9,
    "TransactionType" : "AMMDelete"
}
```

### AMMDelete Fields <a href="#ammdelete-fields" id="ammdelete-fields"></a>

e [common fields](https://xrpl.org/transaction-common-fields.html) 외에도 AMMDelete 트랜잭션은 다음 필드를 사용합니다:

| Field    | JSON Type | [Internal Type](https://xrpl.org/serialization.html) | Required? | Description                                                                                                                                     |
| -------- | --------- | ---------------------------------------------------- | --------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| `Asset`  | Object    | STIssue                                              | Yes       | The definition for one of the assets in the AMM's pool. In JSON, this is an object with `currency` and `issuer` fields (omit `issuer` for XRP). |
| `Asset2` | Object    | STIssue                                              | Yes       | The definition for the other asset in the AMM's pool. In JSON, this is an object with `currency` and `issuer` fields (omit `issuer` for XRP).   |

### Error Cases <a href="#error-cases" id="error-cases"></a>

모든 트랜잭션에서 발생할 수 있는 오류 외에도 AMMCreate 트랜잭션에서 다음과 같은 트랜잭션 결과 코드가 발생할 수 있습니다:

| Error Code         | Description                                                                                                           |
| ------------------ | --------------------------------------------------------------------------------------------------------------------- |
| `tecAMM_NOT_EMPTY` | AMM은 풀에 자산을 보유하므로 삭제할 수 없습니다. AMM의 유동성 공급자 중 한 명인 경우 먼저 AMMWithdraw 사용하세요.                                            |
| `tecINCOMPLETE`    | 관련 원장 항목이 너무 많아 완전히 삭제할 수 없으므로 트랜잭션이 가능한 한 많이 제거했지만 AMM이 완전히 삭제되지 않았습니다. 다른 AMMDelete 트랜잭션을 전송하여 작업을 계속하고 완료할 수 있습니다. |
| `terNO_AMM`        | 지정한 AMM이 존재하지 않습니다. (이미 삭제되었거나 의도한 AMM에 대해 잘못된 자산을 지정했을 수 있습니다.)                                                      |
