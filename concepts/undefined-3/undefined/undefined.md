# 암호화 키

XRP Ledger에서 디지털 서명은 [트랜잭션](../../transactions/)에 특정한 일련의 작업을 수행하도록 _권한_을 부여합니다. 서명된 트랜잭션만 네트워크에 제출하고 검증된 장부에 포함될 수 있습니다.

디지털 서명을 만들기 위해서는 트랜잭션을 보내는 계정과 연관된 암호화 키 쌍을 사용합니다. 키 쌍은 XRP Ledger가 지원하는 [암호화 서명 알고리즘](undefined.md#undefined-10) 중 어떤 것을 사용하여 생성할 수 있습니다. 키 쌍은 그것이 생성된 알고리즘에 관계없이 [마스터 키 쌍](undefined.md#undefined-7), [일반 키 쌍](undefined.md#undefined-9) 또는 서[명자 목록](undefined-1.md)의 구성원으로 사용될 수 있습니다.

{% hint style="info" %}
Warning:

암호화 키에 대한 적절한 보안을 유지하는 것은 중요합니다. 디지털 서명은 XRP Ledger에서 트랜잭션을 승인하는 유일한 방법이며, 이미 적용된 트랜잭션을 취소하거나 반전시킬 수 있는 특권을 가진 관리자가 없습니다. 만약 다른 사람이 당신의 XRP Ledger 계정의 시드 또는 개인 키를 알고 있다면, 그 사람은 당신이 할 수 있는 것처럼 어떤 트랜잭션도 승인하는 데 디지털 서명을 생성할 수 있습니다.
{% endhint %}

## 키 생성&#x20;

많은 [클라이언트 라이브러리](../../../references/undefined/)와 응용 프로그램이 XRP Ledger와 함께 사용하기에 적합한 키 쌍을 생성할 수 있습니다. 그러나, 당신이 신뢰하는 장치와 소프트웨어로 생성된 키 쌍만 사용해야 합니다. 손상된 응용 프로그램은 당신의 비밀을 악의적인 사용자에게 노출시킬 수 있으며, 이러한 사용자는 나중에 당신의 계정에서 트랜잭션을 보낼 수 있습니다.

## 키 컴포넌트&#x20;

암호화 키 쌍은 키 유도 과정을 통해 수학적으로 연결된 **개인 키**와 **공개 키**입니다. 각 키는 숫자이며, 개인 키는 강력한 난수 소스를 사용하여 선택되어야 합니다. [암호화 서명 알고리즘](undefined.md#undefined-10)이 키 유도 과정을 정의하고 암호화 키가 될 수 있는 숫자에 대한 제약을 설정합니다.

XRP Ledger와 관련하여서는 패스프레이즈, 시드, 계정 ID 또는 주소와 같은 관련 값을 사용할 수도 있습니다.

<figure><img src="https://xrpl.org/img/cryptographic-keys.svg" alt=""><figcaption></figcaption></figure>

_암호화 키 값 사이의 관계를 간략히 나타낸 그림._

패스프레이즈, 시드, 그리고 개인 키는 모두 **비밀** 정보입니다: 이러한 값 중 하나라도 알고 있다면, 유효한 서명을 생성하고 그 계정을 완전히 제어할 수 있습니다. 계정의 소유자인 경우, 계정의 비밀 정보를 매우 **주의 깊게** 다루어야 합니다. 그렇지 않으면, 계정을 사용할 수 없습니다. 누군가가 이 정보에 접근할 수 있다면, 그들은 당신의 계정을 제어할 수 있습니다.

반면에, 공개 키, 계정 ID, 그리고 주소는 모두 공개 정보입니다. 일시적으로 공개 키를 비밀로 유지해야 하는 상황도 있지만, 결국에는 XRP Ledger가 서명을 검증하고 거래를 처리할 수 있도록 거래의 일부로 공개해야 합니다.

키 유도가 어떻게 작동하는지에 대한 더 자세한 기술적 세부사항은 [키 파생](undefined.md#undefined-12)을 참조하세요.

## 패스프레이즈

선택적으로 패스프레이즈나 다른 입력을 시드나 개인 키를 선택하는 방법으로 사용할 수 있습니다. 이 방법은 시드나 개인 키를 완전히 무작위로 선택하는 것보다 덜 안전하지만, 이를 원하는 희귀한 경우가 있습니다. (예를 들어, 2018년 "XRPuzzler"는 퍼즐을 처음으로 푼 사람에게 XRP를 선물로 주었는데, 그는 상품인 XRP를 보유한 계정의 패스프레이즈로 [퍼즐의 해답](https://bitcoinexchangeguide.com/cryptographic-puzzle-creator-xrpuzzler-offers-137-xrp-reward-to-anyone-who-can-solve-it/)을 사용하였습니다.)&#x20;

패스프레이즈는 비밀 정보이므로, 매우 주의 깊게 보호해야 합니다. 주소의 패스프레이즈를 알고 있는 사람은 그 주소를 실질적으로 완전히 제어할 수 있습니다.

## 시드

시드 값은 계정의 실제 개인 키와 공개 키를 [유도](undefined.md#undefined-12)하는 데 사용되는 축약된 값입니다. [wallet\_propose 메소드](../../../references/http-websocket-apis/api-2/undefined/wallet\_propose.md) 응답에서, <mark style="background-color:yellow;">master\_key</mark>, <mark style="background-color:yellow;">master\_seed</mark>, 그리고 <mark style="background-color:yellow;">master\_seed\_hex</mark>는 모두 다양한 형식으로 같은 시드 값을 나타냅니다. 이러한 형식 중 어느 것이든 [rippled API](../../../references/http-websocket-apis/) 및 [기타 일부 XRP Ledger 소프트웨어](../../undefined/undefined-1.md)에서 [거래에 서명](../../transactions/)하는 데 사용할 수 있습니다. <mark style="background-color:yellow;">master\_</mark> 접두사가 붙어 있지만, 이 시드가 나타내는 키가 반드시 계정의 마스터 키일 필요는 없습니다; 키 쌍을 일반 키나 다중 서명 목록의 멤버로 사용할 수도 있습니다.

시드 값은 비밀 정보이므로, 매우 주의 깊게 보호해야 합니다. 주소의 시드 값을 알고 있는 사람은 그 주소를 실질적으로 완전히 제어할 수 있습니다.

## 개인 키

_개인 키_는 디지털 서명을 생성하는 데 사용되는 값입니다. 대부분의 XRP Ledger 소프트웨어는 개인 키를 명시적으로 보여주지 않고, 필요한 경우 시드 값에서 [개인 키를 유도](undefined.md#undefined-12)합니다. 기술적으로는 시드 대신 개인 키를 저장하고 이를 직접 거래에 서명하는 데 사용할 수 있지만, 이런 사용법은 드뭅니다.\
\
시드와 마찬가지로, 개인 키는 비밀 정보이므로 매우 주의 깊게 보호해야 합니다. 주소의 개인 키를 알고 있는 사람은 그 주소를 실질적으로 완전히 제어할 수 있습니다.

## 공개 키

공개 키는 디지털 서명의 진위를 검증하는 데 사용되는 값입니다. 공개 키는 키 유도의 일부로 개인 키에서 유도됩니다. [wallet\_propose 메소드](../../../references/http-websocket-apis/api-2/undefined/wallet\_propose.md) 응답에서, <mark style="background-color:yellow;">public\_key</mark>와 <mark style="background-color:yellow;">public\_key\_hex</mark>는 모두 같은 공개 키 값을 나타냅니다.

XRP Ledger에서의 거래는 네트워크가 거래의 서명을 검증할 수 있도록 공개 키를 포함해야 합니다. 공개 키는 유효한 서명을 생성하는 데 사용될 수 없으므로, 공개적으로 공유하는 것이 안전합니다.

## 계정 ID와 주소&#x20;

**계정 ID**는 [계정](./)이나 키 쌍의 핵심 식별자입니다. 이는 공개 키에서 유도됩니다. XRP Ledger 프로토콜에서, 계정 ID는 20바이트의 이진 데이터입니다. 대부분의 XRP Ledger API는 계정 ID를 주소 형태로 나타냅니다, 이는 두 가지 형식 중 하나입니다:

* "classic address"는 계정 ID를 체크섬과 함께 [base58](../../../references/xrp-ledger/undefined/base58.md)로 작성합니다. [wallet\_propose 메소드](../../../references/http-websocket-apis/api-2/undefined/wallet\_propose.md) 응답에서, 이는 <mark style="background-color:yellow;">account\_id</mark> 값입니다.&#x20;
* "X-Address"는 계정 ID와 [데스티네이션 태그](../../transactions/source-and-destination-tags.md)를 결합하고, 결합된 값을 체크섬과 함께 [base58](../../../references/xrp-ledger/undefined/base58.md)로 작성합니다.

두 형식 모두 체크섬이 포함되어 있어서, 작은 변화가 다른 유효한 계정을 참조하는 주소가 아닌 무효한 주소를 결과로 만듭니다. 이렇게 하면, 오타가 나거나 전송 오류가 발생해도 돈을 잘못된 곳으로 보내는 것을 방지할 수 있습니다.

모든 계정 ID(또는 주소)가 Ledger의 계정을 참조하는 것은 아니라는 것을 알아두는 것이 중요합니다. 키와 주소를 유도하는 것은 순전히 수학적 연산입니다. 계정이 XRP Ledger에 기록을 갖기 위해서는, 그 계정의 [reserve requirement](reserves.md) 충족하는 [XRP의 지불을 받아야 합니다](./). 계정이 자금을 받기 전까지는 어떤 거래도 보낼 수 없습니다.

계정 ID나 주소가 자금이 부족한 계정을 참조하지 않더라도, [일반 키 쌍](undefined.md#undefined-9)이나 [서명자 목록의 멤버](undefined-1.md)를 나타내기 위해 그 계정 ID나 주소를 사용할 수 있습니다.

## 키 유형

XRP Ledger는 한 가지 이상의 [암호화 서명 알고리즘](undefined.md#undefined-10)을 지원합니다. 주어진 키 쌍은 특정 암호화 서명 알고리즘에 대해서만 유효합니다. 일부 개인 키는 기술적으로 둘 이상의 알고리즘에 대해 유효한 키가 될 수 있지만, 이러한 개인 키는 각 알고리즘에 대해 다른 공개 키를 갖게 될 것입니다. 또한 개인 키를 재사용해서는 안됩니다.

[wallet\_propose 메소드](../../../references/http-websocket-apis/api-2/undefined/wallet\_propose.md)의 <mark style="background-color:yellow;">key\_type</mark> 필드는 사용할 암호화 서명 알고리즘을 나타냅니다.

## 마스터 키 쌍&#x20;

마스터 키 쌍은 개인 키와 공개 키로 구성됩니다. 계정의 주소는 계정의 마스터 키 쌍에서 유도되므로, 둘은 [본질적으로 관련이 있습니다](./). 마스터 키 쌍을 변경하거나 제거할 수는 없지만, 비활성화는 할 수 있습니다.

[wallet\_propose 메소드](../../../references/http-websocket-apis/api-2/undefined/wallet\_propose.md)는 마스터 키 쌍을 생성하는 방법 중 하나입니다. 이 메소드의 응답은 계정의 시드, 주소, 마스터 공개 키를 함께 보여줍니다. 마스터 키 쌍을 설정하는 다른 방법들에 대해서는 [보안 서명 설정](../../transactions/secure-signing.md)을 참조하세요.

{% hint style="info" %}
Warning:&#x20;

악의적인 행위자가 마스터 개인 키(또는 시드)를 알게 되면, 마스터 키 쌍이 비활성화되지 않은 한 그들은 당신의 계정을 완전히 제어할 수 있습니다. 그들은 당신의 계정이 보유하고 있는 모든 돈을 가져갈 수 있고, 다른 복구할 수 없는 피해를 입힐 수 있습니다. 당신의 비밀 값들을 주의 깊게 다루십시오!
{% endhint %}

마스터 키 쌍을 변경하는 것은 불가능하기 때문에, 그것이 보유하고 있는 가치에 비례하여 주의 깊게 다루어야 합니다. 좋은 관행은 [마스터 키 쌍을 오프라인에서 보관](../../../tutorials/undefined-3/undefined-6.md)하고, 대신에 일반 키 쌍을 설정하여 거래에 서명하는 것입니다. 마스터 키 쌍을 활성화하되 오프라인에서 보관함으로써, 인터넷을 통해 누군가가 접근할 수 없다는 것을 합리적으로 확신할 수 있지만, 긴급 상황에서 사용하기 위해 찾아갈 수는 있습니다.

마스터 키 쌍을 오프라인에서 보관한다는 것은, 악의적인 행위자들이 그것에 접근할 수 없는 곳에 비밀 정보(비밀번호, 시드, 또는 개인 키)를 두는 것을 의미합니다. 일반적으로, 이는 인터넷과 크게 상호작용하는 컴퓨터 프로그램의 접근 범위 내에 있지 않다는 것을 의미합니다. 예를 들어, 인터넷에 연결하지 않는 공기 차단된 기계에 보관하거나, 금고에 보관된 종이 위에 보관하거나, 완전히 기억하고 있을 수 있습니다. (그러나, 기억하는 것에는 몇 가지 단점이 있습니다. 예를 들어, 당신이 죽은 후에 키를 전달하는 것이 불가능하다는 것입니다.)

## 특별한 권한

다음과 같은 특정 작업을 수행하는 거래를 승인할 수 있는 것은 **오직** 마스터 키 쌍뿐입니다:

* 계정의 첫 번째 거래를 보내는 것, 왜냐하면 계정은 다른 방법으로 [트랜잭션을 승인](../../transactions/)하는 것으로 초기화될 수 없기 때문입니다.
* 마스터 키 쌍을 비활성화하는 것.
* 영구적으로 [동결](../../undefined-2/undefined-2/) 기능을 포기하는 것.
* 거래 비용이 0 XRP인 특별한 [키 재설정 거래](../../transactions/transaction-cost.md)를 보내는 것.

일반 키나 [다중 서명](undefined-1.md)은 마스터 키 쌍과 동일하게 다른 모든 것을 할 수 있습니다. 특히, 마스터 키 쌍을 비활성화한 후에는, 일반 키 쌍 또는 다중 서명을 사용하여 다시 활성화할 수 있습니다. 또한, [계정 삭제](./) 요구 사항을 충족하는 경우에 계정을 삭제할 수도 있습니다.

## 일반 키 쌍&#x20;

XRP Ledger 계정은 _일반 키 쌍_이라고 불리는 2차 키 쌍을 승인할 수 있습니다. 이렇게 하면, [마스터 키 쌍](undefined.md#undefined-7)이나 일반 키를 사용하여 거래를 승인할 수 있습니다. 나머지 계정 변경 없이 언제든지 일반 키 쌍을 제거하거나 교체할 수 있습니다.

일반 키 쌍은 [특정 예외](undefined.md#undefined-8)를 제외하고 마스터 키 쌍과 동일한 유형의 트랜잭션 대부분을 승인할 수 있습니다. 예를 들어, 일반 키 쌍은 트랜잭션이 일반 키 쌍을 변경하도록 승인할 수 있습니다.

좋은 보안 관행은 마스터 개인 키를 오프라인의 어딘가에 저장하고, 대부분의 시간 동안 일반 키 쌍을 사용하는 것입니다. 예방책으로, 일반 키 쌍을 정기적으로 변경할 수 있습니다. 악의적인 사용자가 당신의 일반 개인 키를 알게 되면, 마스터 키 쌍을 오프라인 저장소에서 꺼내와서 일반 키 쌍을 변경하거나 제거할 수 있습니다. 이렇게 하면, 계정을 다시 제어할 수 있습니다. 악의적인 사용자가 당신의 돈을 훔치는 것을 막을 수 없어도, 적어도 새로운 계정으로 이동하고 모든 설정과 관계를 처음부터 다시 만들 필요는 없습니다.

일반 키 쌍은 마스터 키 쌍과 같은 형식을 가집니다. (예를 들어, [wallet\_propose 메소드](../../../references/http-websocket-apis/api-2/undefined/wallet\_propose.md)를 사용하여) 동일한 방법으로 그것들을 생성합니다. 유일한 차이점은 일반 키 쌍이 그것이 거래를 서명하는 계정에 본질적으로 연결되어 있지 않다는 것입니다. 한 계정의 마스터 키 쌍을 다른 계정의 일반 키 쌍으로 사용하는 것이 가능하긴 하지만 좋은 아이디어는 아닙니다.

[SetRegularKey 트랜잭션](../../../references/xrp-ledger/undefined-1/undefined-1/setregularkey.md)은 계정의 일반 키 쌍을 지정하거나 변경합니다. 일반 키 쌍을 지정하거나 변경하는 방법에 대한 튜토리얼에서는 [일반 키 쌍 지정](../../../tutorials/undefined-3/undefined.md)을 참조하세요.

## 서명 알고리즘&#x20;

암호화 키 쌍은 항상 특정 서명 알고리즘에 연결되어 있으며, 이는 비밀 키와 공개 키 사이의 수학적 관계를 정의합니다. 암호화 서명 알고리즘은 현재의 암호화 기술 상태를 고려할 때, 비밀 키를 사용하여 매칭되는 공개 키를 계산하는 것은 "쉽지만", 공개 키에서 시작하여 매칭되는 비밀 키를 계산하는 것은 사실상 불가능하다는 특성을 가지고 있습니다.

XRP Ledger는 다음의 암호화 서명 알고리즘을 지원합니다:

| Key Type    | Algorithm                                                                                                                                              | Description                                                                                                                                                                                                                                               |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `secp256k1` | [ECDSA](https://en.wikipedia.org/wiki/Elliptic\_Curve\_Digital\_Signature\_Algorithm)는 타원 곡선 [secp256k1](https://en.bitcoin.it/wiki/Secp256k1)을 사용합니다. | 이것은 비트코인이 사용하는 동일한 체계입니다. XRP Ledger는 기본적으로 이러한 키 유형을 사용합니다.                                                                                                                                                                                              |
| `ed25519`   | [EdDSA](https://datatracker.ietf.org/doc/html/rfc8032)는 타원 곡선 [Ed25519](https://ed25519.cr.yp.to/)를 사용합니다.                                             | 이것은 더 나은 성능과 다른 편리한 특성을 가진 더 새로운 알고리즘입니다. Ed25519의 공개 키는 secp256k1 키보다 바이트가 하나 더 짧기 때문에, <mark style="background-color:yellow;">rippled</mark>는 Ed25519 공개 키 앞에 바이트 <mark style="background-color:yellow;">0xED</mark>를 붙여 두 유형의 공개 키가 모두 33 바이트가 되게 합니다. |

[wallet\_propose 메소드](../../../references/http-websocket-apis/api-2/undefined/wallet\_propose.md)로 키 쌍을 생성할 때, 키를 파생시키는 데 사용할 암호화 서명 알고리즘을 선택하기 위해 <mark style="background-color:yellow;">key\_type</mark>을 지정할 수 있습니다. 만약 당신이 기본적인 키 유형 이외의 것을 생성했다면, 거래를 서명할 때도 <mark style="background-color:yellow;">key\_type</mark>을 지정해야 합니다.

지원되는 키 쌍 유형은 XRP Ledger의 마스터 키 쌍, 일반 키 쌍, 서명자 리스트의 멤버로서 서로 바꿔서 사용할 수 있습니다. [주소를 파생](./)시키는 과정은 secp256k1 및 Ed25519 키 쌍에 대해 동일합니다.

## 미래의 알고리즘들

미래에는, XRP Ledger가 암호학의 발전에 따라 새로운 암호화 서명 알고리즘들이 필요하게 될 가능성이 높습니다. 예를 들어, [쇼어의 알고리즘](https://en.wikipedia.org/wiki/Shor's\_algorithm) (또는 비슷한 것)을 사용하는 양자 컴퓨터가 타원 곡선 암호화를 깨는 것이 실용적인 수준에 도달하게 되면, XRP Ledger 개발자들은 쉽게 깨지지 않는 암호화 서명 알고리즘을 추가할 수 있습니다. 2020년 중반 현재로서는 "양자 저항성"이 있는 서명 알고리즘이 명확히 첫 번째 선택인 것은 없으며, 양자 컴퓨터는 아직도 위협이 될 만큼 실용적이지 않아서 특정 알고리즘을 추가하는 즉각적인 계획은 없습니다.

## 키 파생

키 쌍을 파생시키는 과정은 서명 알고리즘에 따라 다릅니다. 모든 경우에서, 키는 16 바이트 (128 비트)의 길이를 가진 _시드_ 값에서 생성됩니다. 시드 값은 완전히 랜덤일 수도 있고 (권장됨), [SHA-512 해시](../../../references/xrp-ledger/undefined/)를 취하고 출력값의 처음 16 바이트를 유지하여 특정 패스프레이즈에서 파생될 수도 있습니다 ([SHA-512Half](../../../references/xrp-ledger/undefined/)와 같지만, 출력의 256 비트 대신 128 비트만 유지).

## 샘플 코드

여기서 설명한 키 파생 과정은 여러 장소와 프로그래밍 언어에서 구현되어 있습니다:

* rippled 코드 베이스의 C++에서:&#x20;
  * [Seed 정의. ](https://github.com/XRPLF/rippled/blob/develop/src/ripple/protocol/Seed.h)
  * [General & Ed25519 키 파생.](https://github.com/XRPLF/rippled/blob/develop/src/ripple/protocol/impl/SecretKey.cpp)
  * [secp256k1 키 파생. ](https://github.com/XRPLF/rippled/blob/develop/src/ripple/protocol/impl/SecretKey.cpp)
* Python 3에서 [이 저장소의 코드 샘플 섹션](https://github.com/XRPLF/xrpl-dev-portal/blob/master/content/\_code-samples/key-derivation/py/key\_derivation.py).&#x20;
* JavaScript에서 [<mark style="background-color:yellow;">ripple-keypairs</mark>](https://github.com/XRPLF/xrpl.js/tree/main/packages/ripple-keypairs) 패키지.&#x20;

## Ed25519 키 파생



<figure><img src="../../../.gitbook/assets/cryptographic keys_1.png" alt=""><figcaption></figcaption></figure>

1. 시드 값의 [SHA-512Half](../../../references/xrp-ledger/undefined/)를 계산합니다. 결과는 32바이트 비밀 키입니다.

{% hint style="info" %}
Tip:

모든 32바이트 숫자는 유효한 Ed25519 비밀 키입니다. 그러나, 충분히 무작위적으로 선택된 숫자만이 비밀 키로 사용하기 충분히 안전합니다.
{% endhint %}

2. Ed25519 공개 키를 계산하려면, [Ed25519](https://ed25519.cr.yp.to/software.html)의 표준 공개 키 파생을 사용하여 32바이트 공개 키를 파생시킵니다.

{% hint style="info" %}
Caution:

항상 암호화 알고리즘을 사용할 때 가능한 한 표준, 잘 알려진, 공개적으로 감사된 구현을 사용하세요. 예를 들어, [OpenSSL](https://www.openssl.org/)은 핵심 Ed25519 및 secp256k1 함수의 구현을 가지고 있습니다.
{% endhint %}

3. 32바이트 공개 키 앞에 단일 바이트 <mark style="background-color:yellow;">0xED</mark>를 붙여 Ed25519 공개 키를 나타냅니다, 결과는 33 바이트입니다. 거래를 서명하는 코드를 구현하는 경우, <mark style="background-color:yellow;">0xED</mark> 접두사를 제거하고 실제 서명 과정에는 32바이트 키를 사용하세요.
4.  계정 공개 키를 [base58](../../../references/xrp-ledger/undefined/base58.md)로 직렬화 할 때, 계정 공개 키 접두사 <mark style="background-color:yellow;">0x23</mark>을 사용하세요.

    검증자 일시적 키는 Ed25519가 될 수 없습니다.

## secp256k1 키 파생

<figure><img src="../../../.gitbook/assets/cryptographic keys_2.png" alt=""><figcaption></figcaption></figure>

secp256k1 XRP Ledger 계정 키에 대한 키 파생은 몇 가지 이유로 Ed25519 키 파생보다 더 많은 단계를 포함합니다:

* 모든 32바이트 숫자가 유효한 secp256k1 비밀 키는 아닙니다.
* XRP Ledger의 참조 구현에는 단일 시드 값에서 키 쌍의 가족을 파생시키는 미사용, 불완전한 프레임워크가 있습니다.

시드 값에서 XRP Ledger의 secp256k1 계정 키 쌍을 파생시키는 단계는 다음과 같습니다:

1. 시드 값에서 "루트 키 쌍"을 다음과 같이 계산하세요:
   1. 다음을 순서대로 연결하여 총 20바이트를 얻습니다:\
      － 시드 값 (16바이트)\
      － "루트 시퀀스" 값 (4바이트), 빅 엔디안 부호 없는 정수로. 루트 시퀀스의 시작 값으로 0을 사용합니다.
   2. 연결된 (시드+루트 시퀀스) 값의 [SHA-512Half](../../../references/xrp-ledger/undefined/)를 계산합니다。
   3. 만약 결과가 유효한 secp256k1 비밀키가 아닌 경우, 루트 순서를 1 증가시키고 처음부터 다시 시작하세요. [｢출처｣](https://github.com/XRPLF/rippled/blob/fc7ecd672a3b9748bfea52ce65996e324553c05f/src/ripple/crypto/impl/GenerateDeterministicKey.cpp#L103) \
      유효한 secp256k1 키는 0일 수 없으며 secp256k1 그룹 순서보다 숫자가 작아야 합니다. secp256k1 그룹 순서는 상수 값 <mark style="background-color:yellow;">0xFFFFFFFFFFFFFFFFFAEDCE6AF48A03BBFD25E8CD0364141</mark>입니다.
   4. 유효한 secp256k1 비밀키로부터 secp256k1 곡선을 사용하여 표준 ECDSA 공개키 파생을 수행하세요. (암호화 알고리즘의 경우 가능한 경우 표준이고 잘 알려진, 공개적으로 감사된 구현을 사용하세요. 예를 들어, [OpenSSL](https://www.openssl.org/)은 핵심 Ed25519과 secp256k1 함수의 구현을 제공합니다.)

{% hint style="info" %}
Tip:\
검증자(Validators)는 이 루트 키 쌍을 사용합니다. 검증자의 키 쌍을 계산하고 있다면, 여기에서 중단할 수 있습니다. 이 두 가지 다른 유형의 공개키를 구별하기 위해 검증자 공개키의 [base58](../../../references/xrp-ledger/undefined/base58.md) 직렬화는 접두사 <mark style="background-color:yellow;">0x1c</mark>를 사용합니다.
{% endhint %}

2.  루트 공개키를 33바이트 압축 형식으로 변환하세요.\
    ECDSA 공개키의 압축되지 않은 형식은 32바이트 정수 쌍인 X 좌표와 Y 좌표로 구성됩니다. 압축된 형식은 X 좌표와 Y 좌표가 짝수인 경우 <mark style="background-color:yellow;">0x02</mark> 또는 Y 좌표가 홀수인 경우 <mark style="background-color:yellow;">0x03</mark>의 1바이트 접두사와 함께 표현됩니다.\
    <mark style="background-color:yellow;">openssl</mark> 명령줄 도구를 사용하여 압축되지 않은 공개키를 압축 형식으로 변환할 수 있습니다. 예를 들어, 압축되지 않은 공개키가 <mark style="background-color:yellow;">ec-pub.pem</mark> 파일에 있다면 다음과 같이 압축 형식으로 출력할 수 있습니다:\


    ```
    $ openssl ec -in ec-pub.pem -pubin -text -noout -conv_form compressed
    ```
3. 압축된 루트 공개키를 다음과 같이 사용하여 "중간 키 쌍"을 파생시키세요:
   1. 다음을 차례대로 연결하여 총 41바이트를 얻습니다:
      1. 압축된 루트 공개키 (33바이트)
      2. <mark style="background-color:yellow;">0x00000000000000000000000000000000</mark> (4바이트의 0). (이 값은 동일한 패밀리의 다른 구성원을 파생시키기 위해 사용되었지만 실제로는 값 0만 사용됩니다.)
      3. "키 시퀀스" 값 (4바이트), 빅엔디언 부호 없는 정수로 표현합니다. 키 시퀀스의 시작 값으로 0을 사용하세요.
   2. 연결된 값의 [SHA-512Half](../../../references/xrp-ledger/undefined/)를 계산합니다.
   3. 만약 결과가 유효한 secp256k1 비밀키가 아닌 경우, 키 시퀀스를 1 증가시키고 계정의 중간 키 쌍 파생을 재시작하세요.
   4. 유효한 secp256k1 비밀키를 사용하여 표준 ECDSA 공개키 파생을 수행하여 중간 공개키를 얻으세요. (암호화 알고리즘의 경우 가능한 경우 표준이고 잘 알려진, 공개적으로 감사된 구현을 사용하세요. 예를 들어, [OpenSSL](https://www.openssl.org/)은 핵심 Ed25519과 secp256k1 함수의 구현을 제공합니다.)\

4. 중간 공개키를 루트 공개키에 추가하여 마스터 공개키 쌍을 파생시키세요. 마찬가지로 중간 비밀키를 루트 비밀키에 추가하여 비밀키를 파생시키세요.
   1. ECDSA 비밀키는 매우 큰 정수이므로 secp256k1 그룹 순서로 모듈로 연산하여 두 비밀키의 합을 계산할 수 있습니다.
   2. ECDSA 공개키는 타원 곡선 상의 점이므로 타원 곡선 수학을 사용하여 점을 합산해야 합니다.
5. 마스터 공개키를 이전과 같이 33바이트 압축 형식으로 변환하세요.
6.  계정의 공개키를 [base58](../../../references/xrp-ledger/undefined/base58.md) 형식으로 직렬화할 때, 계정 공개키 접두사 <mark style="background-color:yellow;">0x23</mark>을 사용하세요.

    계정의 공개키를 [주소로 변환](./)하는 정보와 샘플 코드는 "Address Encoding"을 참조하세요.

## 참고

* **Concepts:**
  * [Issuing and Operational Addresses](https://xrpl.org/issuing-and-operational-addresses.html)
* **Tutorials:**
  * [Assign a Regular Key Pair](https://xrpl.org/assign-a-regular-key-pair.html)
  * [Change or Remove a Regular Key Pair](https://xrpl.org/change-or-remove-a-regular-key-pair.html)
* **References:**
  * [SetRegularKey transaction](https://xrpl.org/setregularkey.html)
  * [AccountRoot ledger object](https://xrpl.org/accountroot.html)
  * [wallet\_propose method](https://xrpl.org/wallet\_propose.html)
  * [account\_info method](https://xrpl.org/account\_info.html)
