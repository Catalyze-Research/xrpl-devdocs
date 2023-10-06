# XRP

**XRP**는 XRP Ledger의 기본 암호화폐입니다. XRP Ledger의 모든 [계정](../undefined-4/undefined/)은 서로에게 XRP를 보낼 수 있으며, 최소한의 XRP를 [reserve](../undefined-4/undefined/reserves.md)로 보유해야 합니다. XRP는 어떤 XRP Ledger 주소에서 다른 어떤 주소로도 직접 전송될 수 있습니다. 이는 XRP를 편리한 브릿지 화폐로 만듭니다.

XRP Ledger의 일부 고급 기능들, 예를 들어 [에스크로](../undefined-2/undefined-2.md)와 [결제 채널](../undefined-2/undefined-4.md)은 XRP로만 작동합니다. 오더북 [auto-bridging](../dex/auto-bridging.md)은 XRP를 중개자로 사용하여 토큰의 유동성을 높이는 역할을, 하며 이는 분산 거래소에서 XRP를 사용하는 것이 더 저렴할 때 사용됩니다. (예를 들어, auto-bridging은 USD:XRP와 XRP:EUR 주문을 매칭하여 USD:EUR 오더북을 늘립니다.)

XRP는 또한 네트워크의 스팸을 방지하는 보호 조치로서의 역할도 합니다. 모든 XRP Ledger 주소들은 XRP Ledger를 유지하는 비용을 상쇄하기 위해 약간의 XRP가 필요합니다. [트랜잭션 비용](../transactions/transaction-cost.md)과  [reserve](../undefined-4/undefined/reserves.md)는 XRP로 표시된 중립적인 수수료이며, 어떤 당사자에게도 지급되지 않습니다. ledger의 데이터 형식에서 XRP는 [AccountRoot 객체](../../references/xrp-ledger/ledger/ledger-1/accountroot.md)에 저장됩니다.

XRP의 바람직한 속성들 중 일부는 XRP Ledger의 성격과 그 [컨센서스 과정](../undefined-1/undefined.md)에서 비롯됩니다. XRP Ledger는 채굴을 필요로 하지 않으며, 합의 과정은 불변성을 위해 여러 번의 확인을 필요로 하지 않습니다. 이는 XRP Ledger를 비트코인 및 기타 주요 암호화폐보다 트랜잭션 처리에 있어 더 빠르고 효율적으로 만듭니다.

## XRP 속성&#x20;

맨 처음의 ledger에는 1000억의 XRP가 포함되어 있었고, 새로운 XRP를 만들 수는 없습니다. XRP는 [트랜잭션 비용](../transactions/transaction-cost.md)에 의해 소멸되거나, 키를 가진 사람이 없는 주소로 보내짐으로써 잃어버릴 수 있으므로, XRP는 본질적으로 약간 [디플레이션적](https://en.wikipedia.org/wiki/Deflation)입니다. 그러나 XRP가 다 소진될 걱정은 하지 않아도 됩니다: 현재의 소멸 속도로는 모든 XRP를 소멸시키는 데 적어도 7만년이 필요하며, XRP의 총 공급량이 변함에 따라 [XRP의 가격과 수수료는 조정될 수](../undefined-1/undefined-6.md) 있습니다.

기술적인 맥락에서, XRP는 가장 가까운 0.000001 XRP, 즉 "drop"의 XRP로 정확하게 측정됩니다. [rippled API](../../references/http-websocket-apis/)는 모든 XRP 금액이 XRP의 drop으로 명시되도록 요구합니다. 예를 들어, 1 XRP는 1000000 drops로 표현됩니다. 더 자세한 정보를 위해선, [화폐 형식 참조](../../references/xrp-ledger/undefined/undefined.md)를 참고하십시오.

## 역사

## XRP 판매

XRP Ledger는 2011년부터 2012년 초기에 Jed McCaleb, Arthur Britto, David Schwartz에 의해 개발되었습니다. 2012년 9월, Jed와 Arthur는 Chris Larsen과 함께 Ripple이라는 회사(당시 OpenCoin Inc라는 이름으로 불림)를 설립하고, XRP Ledger의 개발을 위해 Ripple에게 800억 XRP를 기부하기로 결정했습니다. 그 후로 회사는 정기적으로 XRP를 판매하였고, 이를 통해 XRP 시장을 강화하고 네트워크 유동성을 향상시키며, 더 큰 생태계의 개발을 촉진하였습니다. 2017년에는 회사가 [550억 XRP를 escrow에 두었으며](https://ripple.com/insights/ripple-escrows-55-billion-xrp-for-supply-predictability/), 이는 앞으로 일반 공급량이 [예측 가능한 방식으로 증가](https://ripple.com/insights/ripple-to-place-55-billion-xrp-in-escrow-to-ensure-certainty-into-total-xrp-supply/)하도록 보장하기 위한 것이었습니다.[ Ripple의 XRP 시장 성과 사이트](https://ripple.com/xrp/)에서는 현재 회사가 보유하고 escrow에 잠긴 XRP의 양을 보고하고 있습니다.

## 명칭

원래 XRP Ledger는기술이 결제를 [여러 홉과 통화를 통해 전파(ripple)](../undefined-3/rippling.md)시킬 수 있도록 하기 때문에 "Ripple"이라고 불렸습니다. Ledger에 내장된 기본 자산의 티커 심볼로는 'ripple credits' 또는 'ripples'라는 용어에서, 그리고 [ISO 4217 표준](https://www.iso.org/iso-4217-currency-codes.html)의 비국가 통화를 위한 X 접두사에서 'XRP'를 선택했습니다. 회사는 'Ripple Labs'라는 이름으로 등록되었습니다. 'XRP'라는 이름은 모든 맥락에서 자산을 참조하는 데 사용되게 되어, 기술과 회사의 유사한 이름으로 인한 혼동을 피하게 되었고, 결국 회사의 이름을 'Ripple'로 줄였습니다. 2018년 5월, [커뮤니티는 XRP를 대표하기 위해 새로운 'X' 심볼](https://twitter.com/xrpsymbol/status/1006925937571713025)을 선택했습니다. 이는 회사와 디지털 자산 양쪽에서 사용되었던 트리스켈리온 로고와 구별하기 위한 것이었습니다.

<table><thead><tr><th width="359.5">XRP "X" Logo</th><th>Ripple triskelion</th></tr></thead><tbody><tr><td><img src="https://xrpl.org/assets/img/xrp-x-logo.png" alt="&#x22;X&#x22; logo"></td><td><img src="https://xrpl.org/img/ripple-triskelion.png" alt="Triskelion"></td></tr></tbody></table>

XRP의 가장 작은 단위는 Ripple 포럼 회원 ThePiachu의 제안에 따라 "drop"이라고 명명되었습니다. 초기의 대안 용어는 Jed McCaleb의 이름을 따서 "jed"였습니다.

## 참고

* [XRP 보내기(Interactive Tutorial)](../../tutorials/undefined-1/xrp.md)
* [거래소에 XRP 상장하기](../../tutorials/xrp-ledger/xrp.md)
* [화폐 형식](../../references/xrp-ledger/undefined/undefined.md)
