# 토큰 환수(Clawing Back Tokens)

규제의 목적으로, 발행자들은 토큰을 발행한 후에 그 토큰들을 다시 회수할 능력이 필요할 때가 있습니다. 예를 들어 발행자가 본인이 발행한 토큰이 불법적인 활동에 의해 규제된 계좌에 전송되었던 것을 발견하면, 발행자는 그 자산을 다시 회수, 환수할 수 있습니다.

발행자는 해당 권한을 그들의 발행 계좌에 <mark style="background-color:yellow;">**Allow Clawback flag**</mark>를 설정하여 얻을 수 있습니다. 해당 flag는 발행자가 이미 대상토큰을 발행했었다면 효력이 없습니다.

{% hint style="info" %}
주의:

환수의 대상은 사용자가 사용자의 계좌에서 발행한 토큰이어야 합니다. 즉,  XRP를 이 방법으로 환수할 수 없습니다.
{% endhint %}

기본값으로, 환수는 불가능하게 설정되어있습니다. 환수를 사용하기 위해선 <mark style="background-color:yellow;">신뢰선 환수 허락</mark>을 설정해야 하므로 사용자는 [AccountSet 트랜잭션](../../references/xrp-ledger/undefined-1/undefined-1/accountset.md)을 보내야합니다. <mark style="background-color:yellow;">임의의 토큰을 가진 발행자는 환수 설정을 할 수 없습니다.</mark> 사용자는 오직 소유자 디렉토리가 완벽히 비었을 때만 <mark style="background-color:yellow;">신뢰선 환수 허락 설정</mark>을 할 수 있습니다. 이는 사용자가 임의의  신뢰선,  에스크로, 지불 채널, 체크, 서명자 리스트를 구축하기 전에 설정해야함을 의미합니다.
