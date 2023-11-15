# EscrowCreate

금액_에스크로 수정안에서 추가되었습니다._

에스크로 프로세스가 완료되거나 취소될 때까지 XRP를 격리합니다.

## EscrowCreate JSON 예시

```
{
    "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "TransactionType": "EscrowCreate",
    "Amount": "10000",
    "Destination": "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW",
    "CancelAfter": 533257958,
    "FinishAfter": 533171558,
    "Condition": "A0258020E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855810100",
    "DestinationTag": 23480,
    "SourceTag": 11747
}
```

## EscrowCreate 필드

일반적인 필드 외에도 EscrowCreate 트랜잭션은 다음 필드를 사용합니다:

| 필드             | JSON 유형 | 내부 유형  | 설명                                                                                                                       |
| -------------- | ------- | ------ | ------------------------------------------------------------------------------------------------------------------------ |
| Amount         | 문자열     | 금액     | 발신자의 잔액과 에스크로에서 차감할 XRP 금액(드롭 단위)입니다. 에스크로 처리된 XRP는 대상 주소로 이동하거나(FinishAfter 시간 이후) 발신자에게 반환될 수 있습니다(CancelAfter 시간 이후). |
| Destination    | 문자열     | 계정 ID  | 에스크로된 XRP를 받을 주소.                                                                                                        |
| CancelAfter    | 숫자      | UInt32 | (선택 사항) 이 에스크로가 만료되는 Ripple 에포크 이후의 시간(초) 입니다. 이 값은 변경할 수 없습니다. 자금은 이 시간 이후에만 송금인에게 반환될 수 있습니다.                          |
| FinishAfter    | 숫자      | UInt32 | (선택 사항) 에스크로된 XRP가 수신자에게 릴리스될 수 있는 Ripple 에포크 이후의 시간(초)입니다. 이 값은 변경할 수 없습니다. 자금은 이 시간에 도달할 때까지 이동할 수 없습니다.               |
| Condition      | 문자열     | blob   | (선택 사항) PREIMAGE-SHA-256 암호화 조건을 나타내는 16진수 값입니다. 자금은 이 조건이 충족된 경우에만 수취인에게 전달될 수 있습니다.                                    |
| DestinationTag | 숫자      | UInt32 | (선택 사항) 대상 주소의 호스트 수신자와 같이 에스크로 결제의 대상을 추가로 지정하는 임의의 태그입니다.                                                              |

CancelAfter 또는 FinishAfter 중 하나를 지정해야 합니다. 두 필드가 모두 포함된 경우 FinishAfter 시간이 CancelAfter 시간보다 앞서야 합니다.

fix1571 수정안이 활성화된 경우 FinishAfter, Condition 또는 둘 다를 제공해야 합니다. [![New in: rippled 1.0.0](https://img.shields.io/badge/New%20in-rippled%201.0.0-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.0.0)
