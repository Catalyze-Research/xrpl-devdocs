# 피어 예약 사용

피어 예약은 rippled 서버가 예약과 일치하는 피어로부터의 연결을 항상 수락하도록 하는 설정입니다. 이 페이지에서는 두 서버 관리자의 협조를 받아 피어 예약을 사용하여 두 서버 간에 일관된 P2P 연결을 유지하는 방법에 대해 설명합니다.

피어 예약은 두 서버가 서로 다른 당사자에 의해 운영되고 들어오는 연결을 받는 서버가 많은 피어를 가진 허브 서버일 때 가장 유용합니다. 명확성을 위해 이 지침에서는 다음 용어를 사용합니다:

* 나가는 연결을 만드는 서버는 스톡 서버입니다. 이 서버는 허브 서버의 피어 예약을 사용합니다.
* 들어오는 연결을 받는 서버는 허브 서버입니다. 관리자가 이 서버에 피어 예약을 추가합니다.

그러나 이 지침을 사용하여 한 서버 또는 두 서버가 허브, 유효성 검증인 또는 스톡 서버인지 여부에 관계없이 피어 예약을 설정할 수 있습니다. 더 바쁜 서버가 나가는 연결을 만드는 서버인 경우에도 피어 예약을 사용할 수 있지만 이 프로세스에서는 해당 구성을 설명하지 않습니다.

## 요구 사항

이 단계를 완료하려면 다음 사전 요구 사항을 충족해야 합니다:

* 두 서버의 관리자가 모두 rippled를 설치하여 실행 중이어야 합니다.
* 두 서버의 관리자가 협력하는 데 동의해야 하며 통신할 수 있어야 합니다. 비밀 정보를 공유할 필요가 없으므로 공개 커뮤니케이션 채널이 좋습니다.
* 허브 서버는 들어오는 피어 연결을 수신할 수 있어야 합니다. 이를 허용하도록 방화벽을 구성하는 방법에 대한 지침은 피어링을위한 포트 포워드를 참조하세요.
* 두 서버는 프로덕션 XRP Ledger, testnet 또는 devnet과 같은 동일한 XRP Ledger 네트워크와 동기화되도록 구성해야 합니다.

## 단계

피어 예약을 사용하려면 다음 단계를 완료합니다:

## 1. (스톡 서버) 영구 노드 키 쌍을 설정합니다.

스톡 서버의 관리자가 이 단계를 완료합니다.

이미 영구 노드 키 쌍 값으로 서버를 구성한 경우, [2단계: 피어 관리자에게 노드 공개 키 전달](undefined-6.md#2.-.)로 건너뛸 수 있습니다. (예를 들어, 각 서버에 영구 노드 키 쌍을 설정하는 것은 서버 클러스터 설정 과정의 일부입니다).

{% hint style="info" %}
Tip:

영구 노드 키 쌍을 설정하는 것은 선택 사항이지만, 서버의 데이터베이스를 지우거나 새 컴퓨터로 이동해야 하는 경우 피어 예약 설정을 유지하기가 더 쉬워집니다. 영구 노드 키 쌍을 설정하지 않으려면 server\_info 메소드 응답의 pubkey\_node 필드에 보고된 대로 서버에서 자동으로 생성된 노드 공개 키를 사용할 수 있습니다.
{% endhint %}

1. validation\_create 메소드를 사용하여 새로운 임의의 키 쌍을 생성합니다. (비밀 값은 생략합니다.)\
   예를 들어:&#x20;

```
rippled validation_create

Loading: "/etc/rippled.cfg"
Connecting to 127.0.0.1:5005
{
   "result" : {
      "status" : "success",
      "validation_key" : "FAWN JAVA JADE HEAL VARY HER REEL SHAW GAIL ARCH BEN IRMA",
      "validation_public_key" : "n9Mxf6qD4J55XeLSCEpqaePW4GjoCR5U1ZeGZGJUCNe3bQa4yQbG",
      "validation_seed" : "ssZkdwURFMBXenJPbrpE14b6noJSu"
   }
}
```

validation\_seed(노드 시드 값)와 validation\_public\_key 값(노드 공개키)을 저장합니다.

2. rippled의 설정 파일을 편집합니다.

```
vim /etc/opt/ripple/rippled.cfg
```

권장 설치는 기본적으로 /etc/opt/ripple/rippled.cfg 구성 파일을 사용합니다. 구성 파일을 저장할 수 있는 다른 위치로는 $HOME/.config/ripple/rippled.cfg(여기서 $HOME은 rippled를 실행하는 사용자의 홈 디렉터리), $HOME/.local/ripple/rippled.cfg 또는 rippled를 시작한 현재 작업 디렉터리 등이 있습니다.

3. 앞에서 생성한 validation\_seed 값을 사용하여 \[node\_seed] 구절을 추가합니다.\
   예를 들어:

```
[node_seed]
ssZkdwURFMBXenJPbrpE14b6noJSu
```

{% hint style="info" %}
Warning:

모든 서버는 고유한 \[node\_seed] 값을 가져야 합니다. 구성 파일을 다른 서버로 복사하는 경우 반드시 \[node\_seed] 값을 제거하거나 변경해야 합니다. 악의적인 공격자가 이 값에 액세스하면 XRP Ledger P2P 통신에서 서버를 사칭하는 데 사용할 수 있으므로 \[node\_seed]를 비밀로 유지하세요.
{% endhint %}

4. rippled 서버를 재시작합니다:

```
systemctl restart rippled
```

## 2. 스톡 서버의 노드 공개키를 전달합니다.

스톡 서버의 관리자가 허브 서버의 관리자에게 스톡 서버의 노드 공개키를 알려줍니다. (1단계의 validation\_public\_key를 사용합니다.) 허브 서버의 관리자는 다음 단계를 위해 이 값이 필요합니다.

## 3. (허브 서버) 피어 예약 추가하기

허브 서버의 관리자가 이 단계를 완료합니다.

이전 단계에서 받은 노드 공개키를 사용하여 peer\_reservations\_add 메소드를 사용하여 예약을 추가합니다. 예를 들면 다음과 같습니다:

```
$ rippled peer_reservations_add n9Mxf6qD4J55XeLSCEpqaePW4GjoCR5U1ZeGZGJUCNe3bQa4yQbG "Description here"

Loading: "/etc/opt/ripple/rippled.cfg"
Connecting to 127.0.0.1:5005

{
  "result": {
    "status": "success"
  }
}
```

{% hint style="info" %}
Tip:

설명은 이 예약이 누구를 위한 것인지 사람이 읽을 수 있는 메모를 추가하기 위해 제공할 수 있는 선택적 필드입니다.
{% endhint %}

## 4. 허브 서버의 현재 IP 주소 및 피어 포트 전달하기

허브 서버의 관리자는 서버의 현재 IP 주소와 피어 포트를 스톡 서버의 관리자에게 알려야 합니다. 허브 서버가 NAT(네트워크 주소 변환)를 수행하는 방화벽 뒤에 있는 경우 서버의 _외부_ IP 주소를 사용합니다. 기본 구성 파일은 피어 프로토콜에 포트 51235를 사용합니다.

## 5. (스톡 서버) 피어 서버에 연결하기

스톡 서버의 관리자가 이 단계를 완료합니다.

연결 방법을 사용하여 서버를 허브 서버에 연결합니다. 예를 들어:

{% tabs %}
{% tab title="WebSocket" %}
```
{
    "command": "connect",
    "ip": "169.54.2.151",
    "port": 51235
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```
{
    "method": "connect",
    "params": [
        {
            "ip": "169.54.2.151",
            "port": 51235
        }
    ]
}
```
{% endtab %}

{% tab title="Commandline" %}
```
rippled connect 169.54.2.151 51235
```
{% endtab %}
{% endtabs %}

허브 서버의 관리자가 이전 단계에서 설명한 대로 피어 예약을 설정한 경우 자동으로 연결되고 가능한 한 오랫동안 연결 상태를 유지해야 합니다.

## 다음 단계

서버 관리자는 서버가 다른 피어에 대해 설정한 예약을 관리할 수 있습니다. (내 서버에 예약이 있는 다른 서버는 확인할 수 없습니다.) 다음을할 수 있습니다:

* 피어 예약을 더 추가하거나 피어 예약의 설명을 업데이트합니다(peer\_reservations\_add 메소드 사용).
* peer\_reservations\_list 메소드를 사용하여 예약을 구성한 서버를 확인합니다.
* peer\_reservations\_del 메소드를 사용하여 예약 중 하나를 제거합니다.
* peers 메소드를 사용하여 현재 연결되어 있는 피어와 사용한 대역폭을 확인합니다.

{% hint style="info" %}
Tip:

원치 않는 피어와의 연결을 즉시 끊을 수 있는 API 메소드는 없지만, firewalld와 같은 소프트웨어 방화벽을 사용하여 원치 않는 피어가 내 서버에 연결하지 못하도록 차단할 수 있습니다. 예제는 커뮤니티에서 기여한 rbh 스크립트를 참조하세요.
{% endhint %}
