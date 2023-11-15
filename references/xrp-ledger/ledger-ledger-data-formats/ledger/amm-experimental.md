# AMM(experimental - 수정중)

AMM 원장 항목은 단일 자동화된 시장 조성자(AMM) 인스턴스를 설명합니다. 이는 항상 특별한 AccountRoot 항목과 짝을 이룹니다.

### Example AMM JSON <a href="#example-amm-json" id="example-amm-json"></a>

```python
{
    "Account" : "rE54zDvgnghAoPopCgvtiqWNq3dU5y836S",
    "Asset" : {
      "currency" : "XRP"
    },
    "Asset2" : {
      "currency" : "TST",
      "issuer" : "rP9jPyP5kyvFRb6ZiRghAGw5u8SGAmU4bd"
    },
    "AuctionSlot" : {
      "Account" : "rJVUeRqDFNs2xqA7ncVE6ZoAhPUoaJJSQm",
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
      "DiscountedFee" : 0,
      "Expiration" : 721870180,
      "Price" : {
          "currency" : "039C99CD9AB0B70B32ECDA51EAAE471625608EA2",
          "issuer" : "rE54zDvgnghAoPopCgvtiqWNq3dU5y836S",
          "value" : "0.8696263565463045"
      }
    },
    "Flags" : 0,
    "LPTokenBalance" : {
      "currency" : "039C99CD9AB0B70B32ECDA51EAAE471625608EA2",
      "issuer" : "rE54zDvgnghAoPopCgvtiqWNq3dU5y836S",
      "value" : "71150.53584131501"
    },
    "TradingFee" : 600,
    "VoteSlots" : [
      {
          "VoteEntry" : {
            "Account" : "rJVUeRqDFNs2xqA7ncVE6ZoAhPUoaJJSQm",
            "TradingFee" : 600,
            "VoteWeight" : 100000
          }
      }
    ]
}
```

### AMM Fields <a href="#amm-fields" id="amm-fields"></a>

AMM 항목에는 일반 필드 외에 다음과 같은 필드가 있습니다
