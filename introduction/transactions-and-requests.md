# Transactions and Requests

XRP Ledger와의 대부분의 상호작용은 원장을 변경하는 트랜잭션을 전송하거나 원장에 대한 정보 요청을 전송하는 것입니다. 또한 관심 알림을 지속적으로 모니터링하기 위해 구독할 수도 있습니다.



## 트랜잭션은 어떻게 작동하나요?

트랜잭션을 사용해 계정 간 XRP 및 기타 토큰 전송, 대체 불가능한 토큰 발행 및 소각, 오퍼 생성, 수락, 취소 등 원장을 변경할 수 있습니다. XRP Ledger에 명령을 전송하고 트랜잭션이 완료되었는지 확인하여 트랜잭션을 실행합니다. 명령 구문 형식은 모든 트랜잭션에 대해 동일합니다.

* 항상 트랜잭션 유형과 트랜잭션을 만드는 계정의 공개 주소를 제공해야 합니다.
* 두 개의 필수 필드는 거래의 수수료와 계정의 거래에 대한 다음 시퀀스 번호입니다. 이 필드는 자동으로 입력할 수 있습니다.

거래 유형에 따라 거래에 특정 필수 필드가 있을 수도 있습니다. 예를 들어, 결제 트랜잭션에는 금액 값(drops 단위 ex> 500,000 drops = 0.5 XRP)과 자금이 입금되는 대상 공개 주소가 필요합니다.

다음은 JSON 형식의 트랜잭션 샘플입니다. 이 트랜잭션은 계정rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn에서 대상 계정ra5nK24KXen9AHvsdFTKHSANinZseWnPcX로 1 XRP를 이체합니다.

```json
{
  "TransactionType": "Payment",
  "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
  "Amount": "1000000",
  "Destination": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX"
}
```



선택 필드는 모든 트랜잭션에 사용할 수 있으며, 특정 트랜잭션에는 추가 필드를 사용할 수 있습니다. 필요한 만큼 선택 필드를 포함할 수 있지만, 모든 트랜잭션에 모든 필드를 포함할 필요는 없습니다.

트랜잭션을 JavaScript, Python, command line, or 호환되는 서비스에서 명령으로 원장에 전송합니다. 리플 서버는 트랜잭션을 XRPL에 제안합니다.\


<figure><img src="../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

검증자의 80%가 현재 제안된 트랜잭션 세트를 승인하면 해당 트랜잭션은 영구 원장의 일부로 기록됩니다. 리플 서버는 사용자가 보낸 트랜잭션의 결과를 반환합니다.

트랜잭션에 대한 자세한 내용은 [Transactions](https://xrpl.org/transactions.html) 참조하세요.



## Requests는 어떻게 진행되나요?

요청은 원장에서 정보를 가져오는 데 사용되지만 원장을 변경하지는 않습니다. 이 정보는 누구나 자유롭게 열람할 수 있으므로 계정 정보로 로그인할 필요가 없습니다.

전송하는 필드는 요청하는 정보 유형에 따라 다릅니다. 일반적으로 여러 개의 선택 필드가 있지만 필수 필드는 몇 개에 불과합니다.

요청을 제출하면 리플 서버 또는 요청에 응답하는 전용 서버인 Clio 서버에서 처리될 수 있습니다.\


<figure><img src="../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

Clio 서버는 처리 속도와 안정성을 개선하기 위해 XRPL의 다른 리플 서버의 부하를 일부 덜어줍니다.

다음은 JSON 형식의 샘플 요청입니다. 이 요청은 사용자가 제공한 계좌 번호에 대한 현재 계좌 정보를 가져옵니다.

```json
{
  "command": "account_info",
  "account": "rG1QQv2nh2gr7RCZ1P8YYcBUKCCN633jCn"
}
```

Request는 다양한 정보를 반환합니다. 다음은 JSON 형식의 계정 정보 요청에 대한 응답 예시입니다.

```json
{
    "result": {
        "account_data": {
            "Account": "rG1QQv2nh2gr7RCZ1P8YYcBUKCCN633jCn",
            "Balance": "999999999960",
            "Flags": 8388608,
            "LedgerEntryType": "AccountRoot",
            "OwnerCount": 0,
            "PreviousTxnID": "4294BEBE5B569A18C0A2702387C9B1E7146DC3A5850C1E87204951C6FDAA4C42",
            "PreviousTxnLgrSeq": 3,
            "Sequence": 6,
            "index": "92FA6A9FC8EA6018D5D16532D7795C91BFB0831355BDFDA177E86C8BF997985F"
        },
        "ledger_current_index": 4,
        "queue_data": {
            "auth_change_queued": true,
            "highest_sequence": 10,
            "lowest_sequence": 6,
            "max_spend_drops_total": "500",
            "transactions": [
                {
                    "auth_change": false,
                    "fee": "100",
                    "fee_level": "2560",
                    "max_spend_drops": "100",
                    "seq": 6
                },
                ... (trimmed for length) ...
                {
                    "LastLedgerSequence": 10,
                    "auth_change": true,
                    "fee": "100",
                    "fee_level": "2560",
                    "max_spend_drops": "100",
                    "seq": 10
                }
            ],
            "txn_count": 5
        },
        "status": "success",
        "validated": false
    }
}
```

계정 레코드의 필드에 대한 자세한 내용은 계정을 참조하세요.\


Next: [Software Ecosystem](https://xrpl.org/software-ecosystem.html)





