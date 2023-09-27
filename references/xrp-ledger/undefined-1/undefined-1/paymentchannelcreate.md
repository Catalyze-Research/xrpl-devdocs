# PaymentChannelCreate

페이찬 수정안에 의해 추가되었습니다.

결제 채널을 생성하고 XRP로 자금을 조달합니다. 이 트랜잭션을 전송하는 주소가 결제 채널의 "소스 주소"가 됩니다.

## PaymentChannelCreate JSON 예시

```
{
    "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "TransactionType": "PaymentChannelCreate",
    "Amount": "10000",
    "Destination": "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW",
    "SettleDelay": 86400,
    "PublicKey": "32D2471DB72B27E3310F355BB33E339BF26F8392D5A93D3BC0FC3B566612DA0F0A",
    "CancelAfter": 533171558,
    "DestinationTag": 23480,
    "SourceTag": 11747
}
```

## PaymentChannelCreate 필드

일반적인 필드 외에도 PaymentChannelCreate 트랜잭션은 다음 필드를 사용합니다:

| 필드             | JSON 유형 | [내부 유형](https://xrpl.org/serialization.html) | 설명                                                                                                                                                         |
| -------------- | ------- | -------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Amount         | 문자열     | 양                                            | 발신자의 잔액에서 차감하여 이 채널에 따로 보관할 XRP의 양(드롭 단위)입니다. 채널이 열려 있는 동안에는 XRP가 대상 주소로만 이동할 수 있습니다. 채널이 닫히면 청구되지 않은 XRP는 모두 발신자 주소의 잔액으로 반환됩니다.                          |
| Destination    | 문자열     | 계정 ID                                        | 이 채널에 대한 XRP 청구를 받을 주소입니다. 채널의 "대상 주소"라고도 합니다. 발신자(계정)와 동일할 수 없습니다.                                                                                        |
| SettleDelay    | 숫자      | UInt32                                       | 청구되지 않은 XRP가 있는 경우 채널을 닫기 전에 소스 주소가 대기해야 하는 시간입니다.                                                                                                         |
| PublicKey      | 문자열     | blob                                         | 소스가 이 채널에 대한 클레임에 서명하는 데 사용할 키 쌍의 33바이트 공개 키(16진수)입니다. 이 키는 secp256k1 또는 Ed25519 공개 키일 수 있습니다. 키 쌍에 대한 자세한 내용은 키 도출을 참조하세요.                                |
| CancelAfter    | 숫자      | UInt32                                       | Ripple 에포크 이후 이 채널이 만료되는 시간(초)입니다. 이 시간 이후에 채널을 수정하는 모든 트랜잭션은 채널에 다른 영향을 주지 않고 채널을 닫습니다. 이 값은 변경할 수 없으며, 이 시간보다 일찍 채널을 닫을 수는 있지만 이 시간 이후에는 채널을 열어둘 수 없습니다. |
| DestinationTag | 숫자      | UInt32                                       | (선택 사항) 대상 주소에 호스팅된 수취인 등 이 결제 채널의 대상을 추가로 지정하는 임의의 태그입니다.                                                                                                 |

대상 계정이 수신 결제 채널을 차단하고 있는 경우 트랜잭션은 결과 코드 tecNO\_PERMISSION과 함께 실패합니다. (수신 거부 수정안이 필요합니다.)
