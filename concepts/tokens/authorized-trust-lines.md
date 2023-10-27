# 승인된 신뢰선(Authorized Trust Lines)

승인된 신뢰선 기능을 사용하면 발행자가 특정한 승인된 계정에만 보유할 수 있는 토큰을 생성할 수 있습니다. 이 기능은 토큰에만 적용되며 XRP에는 적용되지 않습니다.

승인된 신뢰선 기능을 사용하려면 발행 계정에서 **Require Auth** 플래그를 활성화하세요. 이 설정이 활성화되어 있는 동안 다른 계정은 발행 계정으로부터 승인받은 신뢰선만 보유할 수 있습니다.

신뢰선을 승인하기 위해 발행 주소에서 [TrustSet 트랜잭션](../../references/xrp-ledger/undefined-1/undefined-1/trustset.md)을 보내어 자신의 계정과 승인할 계정 간의 신뢰선을 구성합니다. 한번 신뢰선을 승인하면 해당 승인을 취소할 수 없습니다. (필요한 경우 신뢰선을 [동결](freezing-tokens/)할 수는 있습니다.)

신뢰선을 승인하는 트랜잭션은 발행 주소로부터 서명되어야 합니다. 이는 해당 주소에 대한 위험 요소가 증가한다는 점에 주의해야 합니다.

{% hint style="info" %}
Caution:

계정에 신뢰선이 없고 XRP Ledger에 제안 없는 경우에만 Require Auth를 활성화할 수 있으므로 토큰 발행을 _시작하기 전_에 Require Auth를 사용할지 여부를 결정해야 합니다.
{% endhint %}

## 스테이블코인 발행

XRP Ledger에서 스테이블코인을 사용하고 승인된 신뢰선을 사용하는 경우, 새로운 고객을 온보딩하는 과정은 다음과 같이 진행될 수 있습니다:

1. 고객이 스테이블코인 발행자의 시스템에 등록하고, 신원 증명(일명 "고객 식별(KYC)" 정보)을 보냅니다.
2. 고객과 스테이블코인 발행자는 서로의 XRP Ledger 주소를 알려줍니다.
3. 고객은 발행자 주소로 향하는 신뢰선을 생성하기 위해 [TrustSet 트랜잭션](../../references/xrp-ledger/undefined-1/undefined-1/trustset.md)을 보냅니다. 이때 신뢰선의 한도를 양수로 설정합니다.
4. 발행자는 고객의 신뢰선을 승인하기 위해 TrustSet 트랜잭션을 보냅니다.

{% hint style="info" %}
Tip:

신뢰선을 승인하는 두 개의 TrustSet 트랜잭션(3단계와 4단계)는 순서에 상관없이 발생할 수 있습니다. 발행자가 먼저 신뢰선을 승인하면, 한도가 0으로 설정된 신뢰선이 생성되고, 고객의 TrustSet 트랜잭션이 미리 승인된 신뢰선에 한도를 설정합니다. _(_[_TrustSetAuth 수정안_](../xrp-ledger/amendments/undefined.md)_으로 추가됨.)_
{% endhint %}

## 예방 조치&#x20;

승인된 신뢰선을 사용하지 않을 경우에도 [운영 및 대기 계정](../undefined-2/undefined-4.md)에 Require Auth 설정을 활성화하고, 해당 계정에서 어떤 신뢰선도 승인하지 않도록 할 수 있습니다. 이렇게 하면 실수로 토큰을 발행하지 않도록 예방할 수 있습니다(예를 들어 사용자가 잘못된 주소를 신뢰하는 경우). 이는 예방 조치일 뿐이며, 운영 및 대기 계정이 _발행자_의 토큰을 전송하는 것을 방지하지는 않습니다.

## 기술적 세부 사항&#x20;

### Require Auth 활성화

다음은 <mark style="background-color:yellow;">asfRequireAuth</mark> 플래그를 사용하여 Require Auth를 활성화하는 [AccountSet 트랜잭션](../../references/xrp-ledger/undefined-1/undefined-1/accountset.md)을 로컬 호스트의 <mark style="background-color:yellow;">rippled</mark> [제출 메소드를](../../references/http-websocket-apis/api-1/undefined-1/submit.md) 예시로 나타낸 것입니다. (주소가 발행 주소, 운영 주소 또는 대기 주소인지와 관계없이 방법은 동일하게 작동합니다.)

요청:

```
POST http://localhost:5005/
{
    "method": "submit",
    "params": [
        {
            "secret": "s████████████████████████████",
            "tx_json": {
                "Account": "rUpy3eEg8rqjqfUoLeBnZkscbKbFsKXC3v",
                "Fee": "15000",
                "Flags": 0,
                "SetFlag": 2,
                "TransactionType": "AccountSet"
            }
        }
    ]
}
```

{% hint style="info" %}
Caution:

제어하지 않는 서버에 비밀 키를 제출하지 마십시오. 네트워크를 통해 암호화되지 않은 비밀 키를 전송하지 마십시오.
{% endhint %}

## Require Auth가 활성화된 계정인지 확인&#x20;

Require Auth 설정이 활성화되어 있는지 확인하려면 account\_info 메소드를 사용하여 계정을 조회하십시오. <mark style="background-color:yellow;">result.account\_data</mark> 객체의 <mark style="background-color:yellow;">플래그</mark> 필드의 값과 [AccountRoot ledger 객체에 정의된 비트별 플래그와 비교합니다.](../../references/xrp-ledger/ledger/ledger-1/accountroot.md)

플래그 값의 결과와 lsfRequireAuth <mark style="background-color:yellow;">플래그</mark> 값 (<mark style="background-color:yellow;">0x00040000</mark>)의 bitwise-AND 결과가 0이 아닌 경우 계정에 <mark style="background-color:yellow;">lsfRequireAuth</mark>가 활성화되어 있습니다. 결과가 0인 경우 계정에 Require Auth가 비활성화되어 있습니다.

## 신뢰선 승인

승인된 신뢰선 기능을 사용하는 경우 다른 사람은 승인되지 않은 신뢰선을 보유할 수 없습니다. 여러 통화를 발행한다면 각 통화에 대해 별도로 신뢰선을 승인해야 합니다.

신뢰선을 승인하기 위해 발행 주소에서 <mark style="background-color:yellow;">LimitAmount</mark>의 <mark style="background-color:yellow;">발행자</mark>로서 사용자를 설정한 [TrustSet 트랜잭션](../../references/xrp-ledger/undefined-1/undefined-1/trustset.md)을 제출하고, <mark style="background-color:yellow;">값</mark>을(신뢰할 금액) 0으로 설정하고 [<mark style="background-color:yellow;">tfSetfAuth</mark>](../../references/xrp-ledger/undefined-1/undefined-1/trustset.md) 플래그를 트랜잭션에 활성화합니다.

다음은 로컬 호스트의 <mark style="background-color:yellow;">rippled</mark> [제출 메소드](../../references/http-websocket-apis/api-1/undefined-1/submit.md)를 예시로 나타낸 것입니다. 이 예시에서는 발행 주소 <mark style="background-color:yellow;">rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW</mark>가 발행한 USD를 신뢰하는 고객 주소 <mark style="background-color:yellow;">rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn</mark>에 대한 TrustSet 트랜잭션을 보내는 것입니다.

요청:

```
POST http://localhost:8088/

{
    "method": "submit",
    "params": [
        {
            "secret": "s████████████████████████████",
            "tx_json": {
                "Account": "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW",
                "Fee": "15000",
                "TransactionType": "TrustSet",
                "LimitAmount": {
                    "currency": "USD",
                    "issuer": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
                    "value": 0
                },
                "Flags": 65536
            }
        }
    ]
}
```

{% hint style="info" %}
Caution:

제어하지 않는 서버에 비밀 키를 제출하지 마십시오. 네트워크를 통해 암호화되지 않은 비밀 키를 전송하지 마십시오.
{% endhint %}

## 신뢰선이 승인되었는지 확인하기

신뢰선이 승인되었는지 확인하려면 [account\_lines 메소드](../../references/http-websocket-apis/api-1/undefined/account\_lines.md)를 사용하여 신뢰선을 조회하십시오. 요청에서는 고객의 주소를 <mark style="background-color:yellow;">계정</mark> 필드에, 발행자의 주소를 <mark style="background-color:yellow;">피어</mark> 필드에 제공하십시오.

응답의 <mark style="background-color:yellow;">result.lines</mark> 배열에서 통화 필드가 원하는 <mark style="background-color:yellow;">화폐</mark>의 신뢰선을 나타내는 객체를 찾으십시오. 해당 객체에 <mark style="background-color:yellow;">peer\_authorized</mark> 필드가 <mark style="background-color:yellow;">true</mark>로 설정되어 있는 경우 발행자(요청의 <mark style="background-color:yellow;">peer</mark> 필드로 사용한 주소)가 신뢰선을 승인한 것입니다.
