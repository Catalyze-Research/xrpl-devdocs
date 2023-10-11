# P2P 결제(Peer-to-Peer Payments)

XRP Ledger는 결제 처리를 위한 효율적이고 국경 없는 솔루션을 제공합니다. 기존 결제 방법과 달리 자산을 보관하고 가치를 전송하기 위해 금융 기관이 필요하지 않습니다. 인터넷에 접속할 수만 있다면 현금을 주는 것만큼이나 쉽게 XRP Ledger로 직접 결제할 수 있습니다. 친구 사이든, 구매자와 판매자 사이든, XRP Ledger를 사용하면 낮은 네트워크 수수료로 신속하게 직접(P2P) 결제를 처리할 수 있습니다.



## 지갑(Wallets)

직접 결제를 처리하기 위해 XRP Ledger를 사용하기 전에 사용할 암호화폐 지갑을 결정해야 합니다. 암호화폐 지갑을 사용하면 원장과 더 쉽게 상호작용하고 자금을 관리할 수 있으며, 필요에 따라 선택할 수 있는 다양한 지갑이 있고 직접 만들 수도 있습니다. 참조: [Crypto Wallets](https://xrpl.org/crypto-wallets.html).



## 계정 생성(Account Creation)

계정을 만들기 전에 사용할 XRP Ledger 네트워크를 결정해야 합니다. 다양한 사용 사례를 위한 여러 네트워크가 있지만, 기본 XRP 트랜잭션은 메인넷에서만 발생합니다. 참조: [Parallel Networks](https://xrpl.org/parallel-networks.html)



대부분의 공개적으로 사용 가능한 지갑은 계정을 생성할 수 있는 기능을 제공하며 공개 키와 개인 키를 생성할 수 있습니다. 그렇지 않은 경우, 수학적으로 유효한 계정이라면 직접 계정을 만들 수 있습니다. 참조: [Creating Accounts](https://xrpl.org/accounts.html#creating-accounts).



## 계정 자금 조달(Fund Your Account)

계정은 자금이 입금되고 최소 준비금 요건을 충족할 때까지 XRP Ledger에서 활성화되지 않습니다. 참조: [Reserves](https://xrpl.org/reserves.html).

사용 중인 지갑에서 XRP를 구매할 수 있는 옵션이 제공되지 않는 경우, 타사 거래소를 찾아서 계정으로 송금해야 합니다. 또는 지인이 내 계정으로 XRP를 보낼 수도 있습니다. 참조: [Payment](https://xrpl.org/payment.html).

계정에 자금을 입금한 후에는 XRP 원장에서 계정이 존재하고 자금이 입금되었는지 확인해야 합니다. 아래 링크를 통해 확인해보세요.

* [XRPL Explorer](https://livenet.xrpl.org/)
* [`account_info` command](https://xrpl.org/account\_info.html)

## 결제 처리(Handling Payments)

### XRP 직접 결제(Direct XRP Payments)

XRP 결제는 XRP Ledger에서 누군가에게 지불하는 가장 간단한 방법입니다. 수표나 에스크로를 사용할 수도 있지만, 완료하려면 여러 번의 트랜잭션이 필요합니다. XRP 직접 결제는 단 한 번의 트랜잭션만 필요하므로 일상적인 활동에 적합한 옵션입니다. 대량의 거래를 처리하는 판매자라면 빠르고 간편하며 수수료가 가장 낮기 때문에 이 방법이 적합한 선택일 수 있습니다. 참조: [Direct XRP Payments](https://xrpl.org/direct-xrp-payments.html).

XRP 직접 결제를 사용하려면 수취인의 주소만 알면 됩니다.

## Cross-Currency Payments(교차 통화 결제)

XRP Ledger를 사용하면 XRP와 토큰의 교차 통화 결제를 할 수 있습니다. XRP Ledger 내 교차 통화 결제는 완전한 단일 결제로, 결제가 완전히 실행되거나 결제의 일부가 전혀 실행되지 않습니다.

교차 통화 결제는 목적지에 고정된 금액을 전달하지만, 송금인은 네트워크에서 트랜잭션이 작동하는 경로에 따라 수수료 비용이 가변적으로 발생할 수 있습니다. 참조:  [Cross-currency Payments](https://xrpl.org/cross-currency-payments.html).

송금인이나 수취인이 기본 XRP 통화 대신 특정 토큰을 선호하는 경우, 이 결제 옵션은 훌륭한 결제 옵션입니다.

