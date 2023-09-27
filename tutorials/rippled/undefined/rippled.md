# 클러스터 rippled 서버

동일한 데이터 센터에서 여러 rippled 서버를 실행하는 경우 클러스터로 구성하여 효율성을 극대화할 수 있습니다. 클러스터링을 구성하려면 다음과 같이 하세요:

1. 각 서버에 대해 서버의 IP 주소를 기록합니다.
2. 각 서버에 대해 validation\_create 메소드를 사용하여 고유한 시드를 생성합니다.\
   예를 들어, 커맨드라인 인터페이스를 사용합니다:

```
$ rippled validation_create

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

각 응답에서 validation\_seed 및 validation\_public\_key 파라미터를 안전한 곳에 저장합니다.

3. 각 서버에서 구성 파일을 편집하여 다음 섹션을 수정합니다:
   1. \[ips\_fixed] 섹션에 클러스터의 다른 구성원의 IP 주소와 포트를 나열합니다. 각 서버의 포트 번호는 해당 서버의 rippled.cfg에서 프로토콜 = 피어 포트(일반적으로 51235)와 일치해야 합니다. 예를 들어:

```
[ips_fixed]
192.168.0.1 51235
192.168.0.2 51235
```

이는 이 서버가 항상 직접 P2P 연결을 유지하려고 시도해야 하는 특정 피어 서버를 정의합니다.

{% hint style="info" %}
Note:

포트 번호를 생략하면 서버는 XRP Ledger 프로토콜을 위해 IANA에서 할당된 포트인 2459 포트를 사용합니다. [![New in: rippled 1.6.0](https://img.shields.io/badge/New%20in-rippled%201.6.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/1.6.0)
{% endhint %}

2. \[node\_seed] 섹션에서 서버의 노드 시드를 2단계의 validation\_create 메소드를 사용하여 생성한 validation\_seed 값 중 하나로 설정합니다. 각 서버는 고유한 노드 시드를 사용해야 합니다. 예를 들어:

```
[node_seed]
ssZkdwURFMBXenJPbrpE14b6noJSu
```

이는 서버가 유효성 검사 메시지를 제외하고 P2P 통신에 서명하는 데 사용하는 키 쌍을 정의합니다.

3. \[cluster\_nodes] 섹션에서 서버의 클러스터 멤버를 validation\_public\_key 값으로 식별하여 설정합니다. 각 서버는 여기에 클러스터의 다른 모든 구성원의 공개 키를 나열해야 합니다. 선택 사항으로 각 서버에 사용자 지정 이름을 추가합니다. 예를 들어:

```
[cluster_nodes]
n9McNsnzzXQPbg96PEUrrQ6z3wrvgtU4M7c97tncMpSoDzaQvPar keynes
n94UE1ukbq6pfZY9j54sv2A1UrEeHZXLbns3xK5CzU9NbNREytaa friedman
```

이것은 서버가 클러스터의 구성원을 인식하는 데 사용하는 키 쌍을 정의합니다.

4. 구성 파일을 저장한 후 각 서버에서 rippled를 재시작합니다.

```
# systemctl restart rippled
```

5. 각 서버가 이제 클러스터의 멤버인지 확인하려면 peers 메소드를 사용합니다. 클러스터 필드에 공개 키와 (구성된 경우) 각 서버의 사용자 지정 이름이 나열되어야 합니다.\
   예를 들어, 커맨드라인 인터페이스를 사용합니다:

```
$ rippled peers

Loading: "/etc/rippled.cfg"
Connecting to 127.0.0.1:5005
{
  "result" : {
    "cluster" : {
        "n9McNsnzzXQPbg96PEUrrQ6z3wrvgtU4M7c97tncMpSoDzaQvPar": {
          "tag": "keynes",
          "age": 1
        },
        "n94UE1ukbq6pfZY9j54sv2A1UrEeHZXLbns3xK5CzU9NbNREytaa": {
          "tag": "friedman",
          "age": 1
        }
    },
    "peers" : [
      ... (omitted) ...
    ],
    "status" : "success"
  }
}
```
