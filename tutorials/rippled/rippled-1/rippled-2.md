# rippled를 병렬 네트워크에 연결하기

개발자가 실제 돈을 걸지 않고 앱을 테스트하거나 기능을 실험할 수 있는 다양한 대체 테스트 및 개발 네트워크가 존재합니다. 이러한 네트워크에서 사용되는 자금은 실제 자금이 아니며 테스트용으로만 사용됩니다. rippled 서버를 이러한 테스트 네트워크 중 하나에 연결할 수 있습니다.

{% hint style="info" %}
Caution:

새롭고 실험적인 기능이 있는 테스트 네트워크에서는 네트워크와 동기화하기 위해 서버의 사전 프로덕션 릴리스를 실행해야 할 수도 있습니다. 각 네트워크에 필요한 코드 버전에 대한 정보는 병렬 네트워크 페이지를 참조하세요.
{% endhint %}

## 단계

rippled 서버를 XRP testnet 또는 devnet에 연결하려면 다음 단계를 완료하세요. testnet 또는 devnet에 있다가 프로덕션 mainnet으로 다시 전환할 때도 이 단계를 사용할 수 있습니다.

## 1. 올바른 허브에 연결하도록 서버를 구성합니다.

rippled.cfg 파일을 편집합니다.

권장 설치는 기본적으로 구성 파일 /etc/opt/ripple/rippled.cfg를 사용합니다. 구성 파일을 저장할 수 있는 다른 위치로는 $HOME/.config/ripple/rippled.cfg(여기서 $HOME은 ripppled를 실행하는 사용자의 홈 디렉터리), $HOME/.local/ripple/rippled.cfg 또는 ripppled를 시작한 현재 작업 디렉터리 등이 있습니다.

1. 연결하려는 네트워크의 허브로 \[ips] 구절을 설정합니다:

{% tabs %}
{% tab title="Testnet" %}
```
[ips]
s.altnet.rippletest.net 51235
```
{% endtab %}

{% tab title="Devnet" %}
```
[ips]
s.devnet.rippletest.net 51235
```
{% endtab %}

{% tab title="Mainnet" %}
```
# No [ips] stanza. Use the default hubs to connect to Mainnet.
```
{% endtab %}

{% tab title="AMM-Devnet" %}
```
[ips]
amm.devnet.rippletest.net 51235
```
{% endtab %}
{% endtabs %}

2. 이전 \[ips] 구절이 있는 경우 주석 처리합니다:

```
# [ips]
# r.ripple.com 51235
# zaphod.alloy.ee 51235
# sahyadri.isrdc.in 51235
```

3. 적절한 값으로 \[network\_id] 구절을 추가합니다:

{% tabs %}
{% tab title="Testnet" %}
```
[network_id]
testnet
```
{% endtab %}

{% tab title="Devnet" %}
```
[network_id]
devnet
```
{% endtab %}

{% tab title="Mainnet" %}
```
[network_id]
main
```
{% endtab %}

{% tab title="AMM-Devnet" %}
```
[network_id]
25
```
{% endtab %}
{% endtabs %}

사용자 지정 네트워크의 경우, 네트워크에 연결하는 모든 사람은 해당 네트워크에 고유한 값을 사용해야 합니다. 새 네트워크를 만들 때는 11에서 4,294,967,295 사이의 정수 중에서 무작위로 네트워크 ID를 선택합니다.

{% hint style="info" %}
Note:

이 설정은 서버가 동일한 네트워크에 있는 피어를 찾는 데 도움이 되지만, 서버가 어떤 네트워크를 따르는지에 대한 엄격한 제어는 아닙니다. 다음 단계의 UNL/신뢰할 수 있는 유효성 검증인 설정은 서버가 실제로 어떤 네트워크를 따르는지 정의하는 것입니다.
{% endhint %}

## 2. 신뢰할 수 있는 유효성 검증인 목록을 설정합니다.

validators.txt 파일을 편집합니다. 이 파일은 rippled.cfg 파일과 같은 폴더에 있으며 서버가 담합하지 않도록 신뢰하는 유효성 검증인을 정의합니다.

1. 연결하려는 네트워크에 대한 \[validator\_list\_sites] 및 \[validator\_list\_keys] 구문을 주석 처리하거나 추가합니다:

{% tabs %}
{% tab title="Testnet" %}
```
[validator_list_sites]
https://vl.altnet.rippletest.net

[validator_list_keys]
ED264807102805220DA0F312E71FC2C69E1552C9C5790F6C25E3729DEB573D5860
```
{% endtab %}

{% tab title="Devnet" %}
```
[validator_list_sites]
https://vl.devnet.rippletest.net

[validator_list_keys]
EDDF2F53DFEC79358F7BE76BC884AC31048CFF6E2A00C628EAE06DB7750A247B12
```
{% endtab %}

{% tab title="Mainnet" %}
```
[validator_list_sites]
https://vl.ripple.com

[validator_list_keys]
ED2677ABFFD1B33AC6FBC3062B71F1E8397C1505E1C42C64D11AD1B28FF73F4734
```
{% endtab %}

{% tab title="AMM-Devnet" %}
```
[validator_list_sites]
http://vlamm.devnet.rippletest.net/

[validator_list_keys]
03553F67DC5A6FE0EBFE1B3B4742833D14AF7C65E79E5760EC76EC56EAFD254CE9
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Tip:

미리보기 패키지에 필요한 구문이 미리 구성되어 있을 수 있지만, 만약을 대비해 확인하시기 바랍니다.
{% endhint %}

2. 이전 \[validator\_list\_sites], \[validator\_list\_keys] 또는 \[validators] 구문을 모두 주석 처리하세요.\
   예를 들어:

```
    # [validator_list_sites]
    # https://vl.ripple.com
    #
    # [validator_list_keys]
    # ED2677ABFFD1B33AC6FBC3062B71F1E8397C1505E1C42C64D11AD1B28FF73F4734

    # Old hard-coded List of Devnet Validators
    # [validators]
    # n9Mo4QVGnMrRN9jhAxdUFxwvyM4aeE1RvCuEGvMYt31hPspb1E2c
    # n9MEwP4LSSikUnhZJNQVQxoMCgoRrGm6GGbG46AumH2KrRrdmr6B
    # n9M1pogKUmueZ2r3E3JnZyM3g6AxkxWPr8Vr3zWtuRLqB7bHETFD
    # n9MX7LbfHvPkFYgGrJmCyLh8Reu38wsnnxA4TKhxGTZBuxRz3w1U
    # n94aw2fof4xxd8g3swN2qJCmooHdGv1ajY8Ae42T77nAQhZeYGdd
    # n9LiE1gpUGws1kFGKCM9rVFNYPVS4QziwkQn281EFXX7TViCp2RC
    # n9Jq9w1R8UrvV1u2SQqGhSXLroeWNmPNc3AVszRXhpUr1fmbLyhS
```

## 3. 기능 활성화(또는 비활성화)

실험적 기능을 사용하는 일부 테스트 네트워크의 경우 구성 파일에서 해당 기능을 강제로 활성화해야 합니다. 다른 네트워크의 경우 \[features] 구절을 사용해서는 안 됩니다. 다음과 같이 구성 파일의 \[features] 구절을 추가하거나 수정합니다:

{% tabs %}
{% tab title="Testnet" %}
```
# [features]
# Delete or comment out. Don't force-enable features on Testnet.
```
{% endtab %}

{% tab title="Devnet" %}
```
# [features]
# Delete or comment out. Don't force-enable features on Devnet.
```
{% endtab %}

{% tab title="Mainnet" %}
```
# [features]
# Delete or comment out. Don't force-enable features on Mainnet.
```
{% endtab %}

{% tab title="AMM-Devnet" %}
```
[features]
AMM
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Warning:

mainnet 또는 testnet에 연결할 때는 \[features] 구절을 사용하지 마세요. 다른 네트워크와 다른 기능을 강제로 활성화하면 서버가 네트워크에서 분리될 수 있습니다.
{% endhint %}

## 4. 서버를 재시작합니다.

```
$ sudo systemctl restart rippled
```

## 5. 서버가 동기화되는지 확인합니다.

다시 시작한 후 네트워크에 동기화하려면 약 5\~15분이 소요됩니다. 서버가 동기화되면 server\_info 메소드에 연결된 네트워크에 따라 validated\_ledger 객체가 표시됩니다.

rippled이 올바른 네트워크에 연결되었는지 확인하려면 서버의 결과를 testnet 또는 devnet의 퍼블릭 서버와 비교하세요. validated\_ledger 객체의 seq 필드는 두 서버에서 모두 동일해야 합니다(확인 중에 변경된 경우 1\~2개 정도 차이가 있을 수 있음).

다음 예시는 커맨드라인에서 서버의 최신 검증 ledger을 확인하는 방법을 보여줍니다:

```
rippled server_info | grep seq
```

웹소켓 도구에서 server\_info를 사용하여 원하는 네트워크의 최신 ledger 인덱스(seq)를 조회할 수 있습니다.
