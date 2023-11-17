# CheckCreate

_(수표 수정안에 의해 추가되었습니다.)_

의도한 목적지에서 현금화할 수 있는 이연 결제인 수표 객체를 ledger에 생성합니다. 이 트랜잭션의 발신자는 수표의 발신자입니다.

## CheckCreate JSON 예시

```
{
  "TransactionType": "CheckCreate",
  "Account": "rUn84CUYbNjRoTQ6mSW7BVJPSVJNLb1QLo",
  "Destination": "rfkE1aSy9G8Upk4JssnwBxhEv5p4mn2KTy",
  "SendMax": "100000000",
  "Expiration": 570113521,
  "InvoiceID": "6F1DFD1D0FE8A32E40E1F2C05CF1C15545BAB56B617F9C6C2D63A6B704BEF59B",
  "DestinationTag": 1,
  "Fee": "12"
}
```

## CheckCreate 필드

일반적인 필드 외에도 CheckCreate 트랜잭션은 다음 필드를 사용합니다:

| 필드               | JSON 유형 | [내부 유형](https://xrpl.org/serialization.html) | 설명                                                                                                                                                          |
| ---------------- | ------- | -------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Destination`    | 문자열     | 계정 ID                                        | 수표를 현금화할 수 있는 [계정](https://xrpl.org/accounts.html)의 고유 주소입니다.                                                                                               |
| `SendMax`        | 화폐 금액   | 금액                                           | 수표가 송금인에게 인출할 수 있는 최대 출처 통화 금액(비XRP 통화에 대한 이체 수수료 포함)입니다. 수표는 동일한 통화로만 수취인에게 입금할 수 있습니다(비XRP 통화의 경우 동일한 발행자가 발행한 통화). 비XRP 금액의 경우 중첩된 필드 이름은 반드시 소문자여야 합니다. |
| `DestinationTag` | 숫자      | UInt32                                       | (선택 사항) 수표의 사유 또는 결제할 호스팅된 수취인을 식별하는 임의 태그입니다.                                                                                                              |
| `Expiration`     | 숫자      | UInt32                                       | (선택 사항) Ripple 에포크 이후 수표가 더 이상 유효하지 않게 되는 시간(초)입니다.                                                                                                         |
| `InvoiceID`      | 문자열     | 해시256                                        | (선택 사항) 이 확인의 특정 사유 또는 식별자를 나타내는 임의의 256비트 해시입니다.                                                                                                           |

## 오류 사례

* 목적지 계정이 수신 수표를 차단하는 경우 트랜잭션이 실패하고 결과 코드가 tecNO\_PERMISSION으로 표시됩니다. (수신 거부 수정안이 필요합니다.)
* 목적지 계정이 트랜잭션의 발신자인 경우, 트랜잭션은 결과 코드 temREDUNDANT와 함께 실패합니다.
* 목적지 계정이 ledger에 존재하지 않는 경우 트랜잭션은 결과 코드 tecNO\_DST와 함께 실패합니다.
* 목적지 계정에 RequireDest 플래그가 활성화되어 있지만 트랜잭션에 데스티네이션 태그필드가 포함되어 있지 않은 경우 트랜잭션은 결과 코드 tecDST\_TAG\_NEEDED와 함께 실패합니다.
* SendMax가 동결된 토큰을 지정하면 트랜잭션이 실패하고 결과 코드가 tecFROZEN으로 표시됩니다.
* 트랜잭션의 만료가 과거인 경우, 트랜잭션은 tecEXPIRED라는 결과와 함께 실패합니다.
* 발신자가 수표를 추가한 후 소유자 reserve를 충족하기에 충분한 XRP를 보유하고 있지 않은 경우, 트랜잭션은 tecINSUFFICIENT\_RESERVE라는 결과와 함께 실패합니다.
* 발신자 또는 수표 수신자가 ledger에 더 많은 개체를 소유할 수 없는 경우, 트랜잭션은 tecDIR\_FULL이라는 결과와 함께 실패합니다.
