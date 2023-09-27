# Ticket

([_TicketBatch_](https://xrpl.org/known-amendments.html#ticketbatch) 수정안에 의해 추가되었습니다.)

티켓 객체 유형은 나중에 사용할 수 있도록 따로 설정된 계정 시퀀스 번호를 추적하는 티켓을 나타냅니다. TicketCreate 트랜잭션으로 새 티켓을 만들 수 있습니다. [![New in: rippled 1.7.0](https://img.shields.io/badge/New%20in-rippled%201.7.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/1.7.0)

## 티켓 JSON 예시

```
{
  "Account": "rEhxGqkqPPSxQ3P25J66ft5TwpzV14k2de",
  "Flags": 0,
  "LedgerEntryType": "Ticket",
  "OwnerNode": "0000000000000000",
  "PreviousTxnID": "F19AD4577212D3BEACA0F75FE1BA1644F2E854D46E8D62E9C95D18E9708CBFB1",
  "PreviousTxnLgrSeq": 4,
  "TicketSequence": 3
}
```

## 티켓 필드

티켓 객체에는 다음과 같은 필드가 있습니다:

| 이름                | JSON 유형 | 내부 유형  | 필수 여부 | 설명                                                                                                                                            |
| ----------------- | ------- | ------ | ----- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| Account           | 문자열     | 계정 ID  | 예     | 이 티켓을 소유한 계정 입니다.                                                                                                                             |
| Flags             | 숫자      | UInt32 | 예     | 이 개체에 대해 활성화된 부울 플래그의 비트맵입니다. 현재 프로토콜은 Ticket개체에 대한 플래그를 정의하지 않습니다. 값은 항상 0입니다.                                                               |
| LedgerEntryType   | 문자열     | UInt16 | 예     | 0x0054문자열에 매핑된 값은 Ticket이 개체가 티켓 개체임을 나타냅니다.                                                                                                  |
| OwnerNode         | 문자열     | UInt64 | 예     | 디렉터리가 여러 페이지로 구성된 경우 소유자 디렉터리의 어느 페이지가 이 개체에 연결되는지 나타내는 힌트입니다. **참고:** 해당 값은 계정에서 파생될 수 있으므로 객체에는 객체가 포함된 소유자 디렉터리로 직접 연결되는 링크가 포함되어 있지 않습니다. |
| PreviousTxnID     | 문자열     | 해시256  | 예     | 가장 최근에 이 개체를 수정한 트랜잭션 의 식별 해시입니다.                                                                                                             |
| PreviousTxnLgrSeq | 숫자      | UInt32 | 예     | 가장 최근에 이 개체를 수정한 트랜잭션이 포함된 ledger의 인덱스입니다.                                                                                                    |
| TicketSequence    | 숫자      | UInt32 | 예     | 이 티켓이 별도로 설정하는 시퀀스 번호 입니다.                                                                                                                    |

## 티켓 ID 형식

티켓 객체의 ID는 다음 값의 절반을 순서대로 연결한 SHA-512입니다:

* 티켓 스페이스 키(0x0054)
* 티켓 소유자의 계정 ID
* 티켓의 티켓 시퀀스 번호
