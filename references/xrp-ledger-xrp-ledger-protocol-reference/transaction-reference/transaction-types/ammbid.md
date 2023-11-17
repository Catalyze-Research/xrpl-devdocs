# AMMBid

_(Requires the_ [_AMM amendment_](https://xrpl.org/known-amendments.html#amm) _)_

자동화된 시장 조성자(AMM)의 경매 슬롯에 입찰합니다. 낙찰되면 유찰되거나 24시간이 경과할 때까지 할인된 수수료로 AMM을 통해 거래할 수 있습니다. 24시간이 지나기 전에 낙찰되면 남은 시간에 따라 입찰 비용의 일부를 환불받게 됩니다. AMM의 LP 토큰을 사용하여 입찰하면 낙찰된 금액이 AMM에 반환되어 LP 토큰의 미결제 잔액이 감소합니다.

## Example AMMBid JSON <a href="#example-ammbid-json" id="example-ammbid-json"></a>

```json
{
    "Account" : "rJVUeRqDFNs2xqA7ncVE6ZoAhPUoaJJSQm",
    "Asset" : {
        "currency" : "XRP"
    },
    "Asset2" : {
        "currency" : "TST",
        "issuer" : "rP9jPyP5kyvFRb6ZiRghAGw5u8SGAmU4bd"
    },
    "AuthAccounts" : [
        {
          "AuthAccount" : {
              "Account" : "rMKXGCbJ5d8LbrqthdG46q3f969MVK2Qeg"
          }
        },
        {
          "AuthAccount" : {
              "Account" : "rBepJuTLFJt3WmtLXYAxSjtBWAeQxVbncv"
          }
        }
    ],
    "BidMax" : {
        "currency" : "039C99CD9AB0B70B32ECDA51EAAE471625608EA2",
        "issuer" : "rE54zDvgnghAoPopCgvtiqWNq3dU5y836S",
        "value" : "100"
    },
    "Fee" : "10",
    "Flags" : 2147483648,
    "Sequence" : 9,
    "TransactionType" : "AMMBid"
}
```

## AMMBid Fields <a href="#ammbid-fields" id="ammbid-fields"></a>

공통 필드([common fields](https://xrpl.org/transaction-common-fields.html)) 외에도 AMMBid 트랜잭션은 다음 필드를 사용합니다:

| Field          | JSON Type                                                                             | [Internal Type](https://xrpl.org/serialization.html) | Required? | Description                                                                                                                                                                                                                                                  |
| -------------- | ------------------------------------------------------------------------------------- | ---------------------------------------------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `Asset`        | Object                                                                                | STIssue                                              | Yes       | The definition for one of the assets in the AMM's pool. In JSON, this is an object with `currency` and `issuer` fields (omit `issuer` for XRP).                                                                                                              |
| `Asset2`       | Object                                                                                | STIssue                                              | Yes       | The definition for the other asset in the AMM's pool. In JSON, this is an object with `currency` and `issuer` fields (omit `issuer` for XRP).                                                                                                                |
| `BidMin`       | [Currency Amount](https://xrpl.org/basic-data-types.html#specifying-currency-amounts) | Amount                                               | No        | Pay at least this amount for the slot. Setting this value higher makes it harder for others to outbid you. If omitted, pay the minimum necessary to win the bid.                                                                                             |
| `BidMax`       | [Currency Amount](https://xrpl.org/basic-data-types.html#specifying-currency-amounts) | Amount                                               | No        | Pay at most this amount for the slot. If the cost to win the bid is higher than this amount, the transaction fails. If omitted, pay as much as necessary to win the bid.                                                                                     |
| `AuthAccounts` | Array                                                                                 | STArray                                              | No        | A list of up to 4 additional accounts that you allow to trade at the discounted fee. This cannot include the address of the transaction sender. Each of these objects should be an [Auth Account object](https://xrpl.org/ammbid.html#auth-account-objects). |

`BidMin,` `BidMax`을 모두 지정할 수 없습니다.

## 인증 계정 개체(Auth Account Objects) <a href="#auth-account-objects" id="auth-account-objects"></a>

AuthAccounts 배열의 각 멤버는 다음 필드를 가진 객체여야 합니다:

| Field     | JSON Type | [Internal Type](https://xrpl.org/serialization.html) | Required? | Description                              |
| --------- | --------- | ---------------------------------------------------- | --------- | ---------------------------------------- |
| `Account` | String    | AccountID                                            | Yes       | The address of the account to authorize. |

배열에 나타날 수 있는 다른 "내부 객체"와 마찬가지로, 이러한 각 객체의 JSON 표현은 객체 유형인 AuthAccount가 유일한 키인 객체로 래핑됩니다.

## 경매 슬롯 가격(Auction Slot Price) <a href="#auction-slot-price" id="auction-slot-price"></a>

거래가 낙찰되면 이전 슬롯 소유자를 자동으로 제치고 발신자의 LP 토큰에서 입찰 가격이 인출됩니다. 경매 낙찰 가격은 시간이 지남에 따라 72분 간격으로 20회씩 나누어 감소합니다. 발신자에게 낙찰에 필요한 LP 토큰이 충분하지 않거나 입찰 가격이 트랜잭션의 BidMax 값보다 높으면 트랜잭션은 tecAMM\_FAILED\_BID 결과와 함께 실패합니다.

*   경매 슬롯이 현재 비어 있거나 만료되었거나 마지막 간격인 경우, 최소 입찰가는 **AMM의 총 LP 토큰 잔액**의 0.001%입니다.

    주의: 이 최소값은 자리 표시자이며 AMM 기능이 확정되기 전에 변경될 수 있습니다.
* 그렇지 않은 경우, 현재 보유자보다 높은 입찰가는 다음 공식을 사용하여 계산됩니다:
*

    ```
    P = B × 1.05 × (1 - t⁶⁰) + M
    ```

    * P는 LP 토큰 단위로, 초과 입찰할 가격입니다.
    * B는 현재 입찰 가격(LP 토큰 단위)입니다.
    * t는 현재 24시간 슬롯에서 경과한 시간의 분율로, 0.05의 배수로 반올림됩니다.
    * M은 위에 정의된 최소 입찰가입니다.

    다른 사람을 능가하는 비용에는 두 가지 특별한 경우가 있습니다. 누군가가 낙찰된 후 첫 번째 간격에서는 최저 입찰가에 기존 입찰가보다 5%를 더한 금액을 더한 금액으로 낙찰가를 결정합니다:

    ```
    P = B × 1.05 + M
    ```

    In the **last interval** of someone's slot, the cost to outbid someone is only the minimum bid:

    ```
    P = M
    ```

**Note:**To make sure all servers in the network reach the same results when building a ledger, time measurements are based on the [official close time](https://xrpl.org/ledger-close-times.html) of the previous ledger, which is approximate.

참고: 원장을 작성할 때 네트워크의 모든 서버가 동일한 결과에 도달하도록 하기 위해, 시간 측정은 대략적인 이전 원장의 공식 마감 시간([official close time](https://xrpl.org/ledger-close-times.html))을 기준으로 합니다.

### 입찰 환불(Bid Refunds) <a href="#bid-refunds" id="bid-refunds"></a>

활성화 된 경매 슬롯에 더 높은 입찰가를 제시하면 AMM은 다음 공식에 따라 이전 보유자에게 가격의 일부를 환불합니다:

```
R = B × (1 - t)
```

* R은 환불할 금액(LP 토큰 단위)입니다.&#x20;
* B는 환불받을 이전 입찰가의 LP 토큰 단위 가격입니다.&#x20;
* t는 현재 24시간 슬롯에서 경과한 시간의 분율로, 0.05의 배수로 반올림됩니다.

특별한 경우로, 경매 슬롯의 마지막(20일) 간격 동안에는 환불 금액이 0이 됩니다.&#x20;

참고: 모든 XRP 원장의 시간과 마찬가지로 거래 처리는 이전 원장의 공식 마감 시간을 사용하므로 실시간과 최대 약 10초의 차이가 발생할 수 있습니다.

### 오류 사례(Error Cases) <a href="#error-cases" id="error-cases"></a>

모든 트랜잭션에서 발생할 수 있는 오류 외에도 AMMBid 트랜잭션에서 다음과 같은 트랜잭션 결과 코드([transaction result codes](https://xrpl.org/transaction-results.html))가 발생할 수 있습니다:

| Error Code              | Description                                                                                               |
| ----------------------- | --------------------------------------------------------------------------------------------------------- |
| `tecAMM_EMPTY`          | AMM에 풀에 자산이 없습니다. 이 상태에서는 AMM을 삭제하거나 새 예금으로만 자금을 조달할 수 있습니다.                                              |
| `tecAMM_FAILED`         | 이 트랜잭션은 발신자가 필요한 입찰가를 지불하기에 충분한 LP 토큰을 보유하고 있지 않거나 경매 낙찰 가격이 트랜잭션의 지정된 입찰가 최대값보다 높았기 때문에 경매에서 낙찰되지 못했습니다. |
| `tecAMM_INVALID_TOKENS` | 이 트랜잭션의 발신자가 슬롯 가격을 충족하기에 충분한 LP 토큰을 보유하고 있지 않습니다.                                                        |
| `temBAD_AMM_TOKENS`     | 지정한 입찰 최소값 또는 입찰 최대값이 이 AMM에 대한 올바른 LP 토큰으로 지정되지 않았습니다.                                                   |
| `temDISABLED`           | 이 네트워크에서는 AMM 기능이 활성화되어 있지 않습니다.                                                                          |
| `temMALFORMED`          | 트랜잭션에서 너무 긴 인증 계정 목록과 같이 잘못된 옵션을 지정했습니다.                                                                  |
| `terNO_ACCOUNT`         | 이 요청에 지정된 계정 중 하나가 존재하지 않습니다.                                                                             |
| `terNO_AMM`             | 이 거래의 자산 쌍에 대한 자동화된 시장 조성자 인스턴스가 존재하지 않습니다.                                                               |
