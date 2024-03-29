# Crypto Wallets

암호화폐 지갑은 XRP Ledger에서 계정과 자금을 관리할 수 있는 방법을 제공합니다. 선택할 수 있는 지갑은 많습니다. 올바른 지갑을 선택하는 것은 궁극적으로 XRP를 사용하는 데 있어 사용자의 필요와 편의성에 따라 결정됩니다.



## 수탁형 지갑 vs 비수탁형 지갑(Custodial vs Non-custodial Wallets)

지갑을 선택할 때 가장 중요한 요소는 수탁형 지갑을 원하는지, 비수탁형 지갑을 원하는지 여부입니다.

수탁형 지갑은 제3자가 일반적으로 XRP Ledger에서 관리하는 계좌에 자금을 보관하는 것을 의미합니다. 수탁형 지갑은 돈을 안전하게 보관하기 위해 다른 기관을 신뢰하는 은행과 같다고 생각하시면 됩니다. 많은 중앙화된 거래소가 수탁형 지갑을 제공하므로, 해당 거래소에서 계정을 생성하고 앱을 사용해도 기술적으로 Ledger에 계정이 있는 것은 아닙니다.

이러한 유형의 지갑은 사용자 친화적이며 비밀번호를 잊어버렸을 때 일반적으로 재설정할 수 있기 때문에 일상적인 결제의 경우 이 방식이 더 바람직할 수 있습니다. 또한, 개별 XRP Ledger 계정이 없는 경우 Ledger의 준비금 요건이 적용되지 않습니다. 수탁자는 XRP Ledger에서 발생하는 모든 문제에 대한 완충 역할을 하며, 어떻게 해야 할지 잘 모르는 경우 지원이나 도움을 제공할 수 있습니다.

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

[XUMM](https://xumm.app/)과 같은 비수탁 지갑은 계정의 비밀키를 회원님이 가지고 있는 지갑입니다. 즉, 계정의 보안을 관리할 책임은 궁극적으로 회원님에게 있습니다.

\***주의: 키를 분실하면 XRP Ledger 계정이 잠기게 되며 복구 옵션은 없습니다.**

비수탁형 지갑은 더 많은 자유를 허용합니다. 사용자가 직접 XRP Ledger과 상호작용하기 때문에 누구의 제한 없이 원하는 모든 유형의 거래를 처리할 수 있습니다. 원장이 허용한다면 무엇이든 할 수 있습니다. 또한 비수탁형 지갑은 기관에 돈을 맡길 필요가 없으므로 통제할 수 없는 시장 요인으로부터 사용자를 보호할 수 있습니다.

수탁형 지갑과 비수탁형 지갑 사용자 모두 자금을 훔치려는 악의적인 사용자로부터 자신을 보호해야 합니다. 수탁형 지갑의 경우 앱이나 사이트의 로그인과 비밀번호를 관리해야 하고, 비수탁형 지갑의 경우 온렛저 계정에 대한 비밀 키를 관리해야 합니다. 두 경우 모두 공격자가 소프트웨어 업데이트나 종속성을 통해 지갑에 악성 코드를 로드하는 공급망 공격과 같은 취약성으로부터 사용자를 보호하기 위해 지갑 제공업체의 자체 보안 관행도 중요합니다. 하지만 수탁형 지갑은 여러 고객의 자금에 즉시 액세스할 수 있기 때문에 공격자의 더 큰 표적이 될 수 있습니다.



## Hardware vs Software Wallets <a href="#hardware-vs-software-wallets" id="hardware-vs-software-wallets"></a>

지갑을 선택할 때 또 다른 결정적인 요소는 하드웨어 지갑과 소프트웨어 지갑 중 하나를 선택하는 것입니다.

하드웨어 지갑은 개인/비밀 키를 저장하는 물리적 장치입니다. 하드웨어 지갑을 사용하면 사용하지 않을 때는 인터넷 연결을 끊어 정보를 안전하게 보호할 수 있고, 해킹하기 쉬운 컴퓨터나 스마트폰으로부터 키를 완전히 분리할 수 있다는 장점이 있습니다.

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

반면 소프트웨어 지갑은 전적으로 디지털 방식입니다. 따라서 사용하기 쉽지만 두 가지 방법 중 보안성이 떨어지지만, 일반적으로 사용 환경을 개선하기 위한 추가 기능이 제공됩니다. 궁극적으로 두 가지 중 어떤 것을 선택할지는 사용 편의성이 얼마나 중요한지에 따라 결정됩니다.

## 나만의 지갑 만들기

XRP Ledger은 공개적으로 사용 가능한 클라이언트 라이브러리와 API 메서드를 갖춘 오픈소스 프로젝트입니다. 기술적으로 HTTP/웹소켓 도구를 사용해 원장과 상호작용할 수는 있지만, 일상적인 사용에는 실용적이지 않습니다. 원장과 상호작용할 수 있는 지갑을 직접 만들 수도 있지만, 이 옵션을 사용하기 전에 계정, 트랜잭션, 원장이 어떻게 함께 작동하는지 정확히 이해해야 합니다.



Next: [Transactions and Requests](https://xrpl.org/txn-and-requests.html)

\
