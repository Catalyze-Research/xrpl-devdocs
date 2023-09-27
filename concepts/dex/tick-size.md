# Tick Size

_(TickSize 수정안에 의해 추가됨.)_

제안이 오더북에 배치될 때, 그 환율은 제안에 관련된 화폐의 발행자가 설정한 <mark style="background-color:yellow;">TickSize</mark> 값에 따라 절사됩니다. XRP와 토큰을 거래할 때는 토큰 발행자의 <mark style="background-color:yellow;">TickSize</mark>가 적용됩니다. 두 토큰을 거래할 때는 제안은 더 작은 <mark style="background-color:yellow;">TickSize</mark> 값을 (즉, 더 적은 유효 숫자를 가진 값을) 사용합니다. 토큰 중 어느 것도 <mark style="background-color:yellow;">TickSize</mark>가 설정되지 않은 경우, 기본 동작이 적용됩니다.

<mark style="background-color:yellow;">TickSize</mark> 값은 오더북에 배치될 때 제안의 환율에서 유효 숫자의 수를 절사합니다. 발행자는 AccountSet 트랜잭션을 사용하여 <mark style="background-color:yellow;">TickSize</mark>를 <mark style="background-color:yellow;">3</mark>에서 <mark style="background-color:yellow;">15</mark>까지의 정수로 설정할 수 있습니다. 환율은 유효 숫자와 지수로 표시되며; <mark style="background-color:yellow;">TickSize</mark>는 지수에 영향을 주지 않습니다. 이를 통해 XRP Ledger는 가치가 크게 다른 자산 간의 환율을 표현할 수 있습니다 (예: 높은 인플레이션 화폐와 희귀한 상품). 발행자가 설정하는 TickSize가 낮을수록, 거래자가 기존의 제안보다 높은 환율로 간주되기 위해 제안해야 하는 증가분은 커집니다.

<mark style="background-color:yellow;">TickSize</mark>는 즉시 실행될 수 있는 제안의 부분에는 영향을 주지 않습니다. (그러므로 <mark style="background-color:yellow;">tfImmediateOrCancel</mark>이 있는 OfferCreate 트랜잭션은 <mark style="background-color:yellow;">TickSize</mark> 값의 영향을 받지 않습니다.) 제안이 완전히 실행될 수 없는 경우, 트랜잭션 처리 엔진은 환율을 계산하고 <mark style="background-color:yellow;">TickSize</mark>에 따라 절사합니다. 그런 다음 엔진은 "덜 중요한" 측에서 제안의 남은 금액을 절사된 환율에 맞게 반올림합니다. 기본적인 OfferCreate 트랜잭션(즉 "구매" 제안)의 경우, <mark style="background-color:yellow;">TakerPays</mark> 금액(구매되는 금액)이 반올림됩니다. <mark style="background-color:yellow;">tfSell</mark> 플래그가 활성화된 경우("판매" 제안), <mark style="background-color:yellow;">TakerGets</mark> 금액(판매되는 금액)이 반올림됩니다.

발행자가 <mark style="background-color:yellow;">TickSize</mark>를 활성화, 비활성화, 변경할 때, 이전 설정 하에 배치된 제안은 영향을 받지 않습니다.
