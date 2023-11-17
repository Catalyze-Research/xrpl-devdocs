# AMMCreate

_(Requires the_ [_AMM amendment_](https://xrpl.org/known-amendments.html#amm) _)_



한 쌍의 자산([fungible tokens](https://xrpl.org/tokens.html) or [XRP](https://xrpl.org/xrp.html))을 거래하기 위한 자동화된 시장 메이커(AMM) 인스턴스를 새로 생성합니다. AMM 항목([AMM entry](https://xrpl.org/amm.html))과 AMM을 나타내는 특별 계정 루트 항목([special AccountRoot entry](https://xrpl.org/accountroot.html#special-amm-accountroot-entries))을 모두 생성합니다. 또한 두 자산의 시작 잔액의 소유권을 발신자에서 생성된 AccountRoot로 이전하고 유동성 공급자 토큰(LP 토큰)의 초기 잔액을 AMM 계정에서 발신자에게 발행합니다.&#x20;



**주의**: AMM을 생성할 때는 각 자산의 가치가 거의 동일한 금액으로 펀딩해야 합니다. 그렇지 않으면 다른 사용자가 이 AMM([performing arbitrage](https://www.machow.ski/posts/an\_introduction\_to\_automated\_market\_makers/#price-arbitrage))으로 거래하여 사용자의 비용으로 이익을 얻을 수 있습니다. 유동성 공급자가 감수하는 통화 위험은 자산 쌍의 변동성(불균형 가능성)에 따라 증가합니다. 거래 수수료가 높을수록 이러한 위험을 상쇄할 수 있으므로 자산 쌍의 변동성을 기준으로 거래 수수료를 설정하는 것이 가장 좋습니다.

### Example AMMCreate JSON <a href="#example-ammcreate-json" id="example-ammcreate-json"></a>

```json
{
    "Account" : "rJVUeRqDFNs2xqA7ncVE6ZoAhPUoaJJSQm",
    "Amount" : {
        "currency" : "TST",
        "issuer" : "rP9jPyP5kyvFRb6ZiRghAGw5u8SGAmU4bd",
        "value" : "25"
    },
    "Amount2" : "250000000",
    "Fee" : "2000000",
    "Flags" : 2147483648,
    "Sequence" : 6,
    "TradingFee" : 500,
    "TransactionType" : "AMMCreate"
}
```

### AMMCreate Fields <a href="#ammcreate-fields" id="ammcreate-fields"></a>

공통 필드 외에도 AMMCreate 트랜잭션은 다음 필드를 사용합니다:

| Field        | JSON Type                                                                             | [Internal Type](https://xrpl.org/serialization.html) | Required? | Description                                                                                                                                                                                      |
| ------------ | ------------------------------------------------------------------------------------- | ---------------------------------------------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `Amount`     | [Currency Amount](https://xrpl.org/basic-data-types.html#specifying-currency-amounts) | Amount                                               | Yes       | The first of the two assets to fund this AMM with. This must be a positive amount.                                                                                                               |
| `Amount2`    | [Currency Amount](https://xrpl.org/basic-data-types.html#specifying-currency-amounts) | Amount                                               | Yes       | The second of the two assets to fund this AMM with. This must be a positive amount.                                                                                                              |
| `TradingFee` | Number                                                                                | UInt16                                               | Yes       | The fee to charge for trades against this AMM instance, in units of 1/100,000; a value of 1 is equivalent to 0.001%. The maximum value is `1000`, indicating a 1% fee. The minimum value is `0`. |

`Amount`과 `Amount`2 중 하나 또는 둘 다 토큰일 수 있으며, 최대 하나만 XRP일 수 있습니다. 두 토큰의 통화 코드와 발행자가 같을 수는 없습니다. 토큰의 발행자는 기본 리플을 활성화해야 합니다.  [Clawback amendment](https://xrpl.org/known-amendments.html#clawback)가 활성화된 경우, 해당 발행자는 Clawback 허용 flag를 활성화하지 않아야 합니다. 자산은 다른 AMM의 LP 토큰일 수 없습니다.

### 특별 거래 비용(Special Transaction Cost) <a href="#special-transaction-cost" id="special-transaction-cost"></a>

각 AMM 인스턴스에는 계정 루트 원장 항목, AMM 원장 항목, 풀의 각 토큰에 대한 트러스트 라인이 포함되므로, AMMCreate 트랜잭션은 원장 스팸을 막기 위해 일반적인 트랜잭션 비용보다 훨씬 높은 비용이 필요합니다. 표준 최소값인 0.00001 XRP 대신, AMMCreate는 최소 증분 소유자 준비금(현재 2 XRP) 이상을 소멸해야 합니다. 이는  [AccountDelete transactions](https://xrpl.org/accountdelete.html)과 동일한 특별 트랜잭션 비용입니다.

### 오류 사례(Error Cases) <a href="#error-cases" id="error-cases"></a>

모든 트랜잭션에서 발생할 수 있는 오류 외에도 AMMCreate 트랜잭션에서 다음과 같은 트랜잭션 결과 코드가 발생할 수 있습니다:



`Amount`과 `Amount`2 중 하나 또는 둘 다 토큰일 수 있으며, 최대 하나만 XRP일 수 있습니다. 두 토큰의 통화 코드와 발행자가 같을 수는 없습니다. 토큰의 발행자는 기본 리플을 활성화해야 합니다. 클로백 수정이 활성화된 경우, 해당 발행자는 클로백 허용 플래그를 활성화하지 않아야 합니다. 자산은 다른 AMM의 LP 토큰일 수 없습니다.



각 AMM 인스턴스에는 계정 루트 원장 항목, AMM 원장 항목, 풀의 각 토큰에 대한 트러스트 라인이 포함되므로, AMMCreate 트랜잭션은 원장 스팸을 막기 위해 일반적인 트랜잭션 비용보다 훨씬 높은 비용이 필요합니다. 표준 최소값인 0.00001 XRP 대신, AMMCreate는 최소 증분 소유자 준비금(현재 2 XRP) 이상을 소멸해야 합니다. 이는 계정 삭제 트랜잭션과 동일한 특별 트랜잭션 비용입니다.

오류 사례

모든 트랜잭션에서 발생할 수 있는 오류 외에도 AMMCreate 트랜잭션에서 다음과 같은 트랜잭션 결과 코드가 발생할 수 있습니다:

| Error Code              | Description                                                                                                                             |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `tecAMM_INVALID_TOKENS` | `Amount` 또는 `Amount2`에 이 AMM의 LP 토큰이 사용하는 것과 동일한 통화 코드가 있는 경우(발생할 가능성은 거의 없습니다.)                                                        |
| `tecDUPLICATE`          | 이 통화쌍에 대한 다른 AMM이 이미 있는 경우                                                                                                              |
| `tecFROZEN`             | 입금 자산(`Amount` 또는 `Amount2`) 중 하나 이상이 현재 동결되어 있습니다.                                                                                     |
| `tecINSUF_RESERVE_LINE` | 이 트랜잭션의 발신자는 LP 토큰을 보유하기 위해 새로운 트러스트 라인이 필요하고, 새로운 트러스트 라인에 대한 추가 소유자 준비금을 충족할 만큼 충분한 XRP가 없기 때문에 이 트랜잭션을 처리하는 데 필요한 증가된 준비금 요건을 충족합니다. |
| `tecNO_AUTH`            | 입금 자산 중 하나 이상이 [authorized trust lines](https://xrpl.org/authorized-trust-lines.html)을 사용하며 발신자에게 해당 자산을 보유할 수 있는 권한이 없습니다.             |
| `tecNO_LINE`            | 발신자에게 입금 자산 중 하나 이상에 대한 신탁 계좌가 없습니다.                                                                                                    |
| `tecNO_PERMISSION`      | 예치 자산 중 하나 이상을 AMM에서 사용할 수 없는 경우. 예를 들어 발행자가 Clawback 지원을 활성화한 경우입니다.                                                                   |
| `tecUNFUNDED_AMM`       | 발신자가 `Amount` 또는 `Amount2`에 지정된 자산을 충분히 보유하지 않아 AMM에 자금을 조달할 수 없습니다.                                                                    |
| `terNO_RIPPLE`          | 자산 중 하나 이상의 발행자가 기본 리플 플래그( [Default Ripple flag](https://xrpl.org/rippling.html#the-default-ripple-flag))를 활성화하지 않았습니다.                |
| `temAMM_BAD_TOKENS`     | 예를 들어, 둘 다 동일한 토큰을 참조하는 등 Amount 및 Amount2의 값이 유효하지 않습니다.                                                                               |
| `temBAD_FEE`            | TradingFee 값이 유효하지 않습니다. 0이거나 양의 정수여야 하며 1000을 초과할 수 없습니다.                                                                              |
| `temDISABLED`           | 이 네트워크에서는 AMM 기능이 활성화되어 있지 않습니다.                                                                                                        |
