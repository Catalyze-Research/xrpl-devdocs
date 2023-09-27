# 우분투 리눅스에 클리오 설치

이 페이지는 **우분투 리눅스 20.04 이상**에서 apt 유틸리티를 사용하여 최신 안정 버전의 클리오를 설치하기 위한 권장 지침을 설명합니다.

이 지침은 Ripple에서 컴파일한 바이너리를 설치합니다. 소스에서 클리오를 빌드하는 방법에 대한 지침은 클리오 소스 코드 저장소를 참조하십시오.

## 요구 조건

클리오를 설치하기 전에 다음 요구 사항을 충족해야 합니다.

* 시스템이 시스템 요구 사항을 충족하는지 확인합니다.

{% hint style="info" %}
Note:

클리오는 rippled 서버와 시스템 요구 사항이 동일하지만, 동일한 양의 ledger 기록을 저장하는 데 필요한 디스크 공간이 더 적습니다.
{% endhint %}

* 호환되는 버전의 CMake와 Boost가 필요합니다. 클리오는 C++20 및 Boost 1.75.0 이상이 필요합니다.
* 로컬 또는 원격으로 실행 중인 Cassandra 클러스터에 액세스할 수 있어야 합니다. Cassandra 설치 지침에 따라 수동으로 Cassandra 클러스터를 설치 및 구성하거나 다음 명령 중 하나를 사용하여 Docker 컨테이너에서 Cassandra를 실행하도록 선택할 수 있습니다.
  * 클리오 데이터를 유지하기로 선택한 경우, Docker 컨테이너에서 Cassandra를 실행하고 클리오 데이터를 저장할 빈 디렉터리를 지정하세요:

```
docker run --rm -it --network=host --name cassandra  -v $PWD/cassandra_data:/var/lib/
cassandra cassandra:4.0.4
```

* 클리오 데이터를 유지하지 않으려면 다음 명령을 실행하세요:

```
docker run --rm -it --network=host --name cassandra cassandra:4.0.4
```

* P2P 모드에서 하나 이상의 rippled 서버에 대한 gRPC 액세스가 필요합니다. rippled 서버는 로컬 또는 원격 서버일 수 있지만 반드시 신뢰해야 합니다. 가장 신뢰할 수 있는 방법은 rippled를 직접 설치하는 것입니다.

## 설치 단계

1. 저장소를 업데이트합니다:

```
sudo apt -y update
```

{% hint style="info" %}
Tip:

동일한 컴퓨터에 이미 최신 버전의 rippled를 설치한 경우, rippled의 패키지 저장소와 서명 키를 추가하는 다음 단계를 건너뛸 수 있으며, 이는 rippled 설치 과정과 동일합니다. 5단계 "rippled 저장소 가져오기"부터 다시 시작합니다.
{% endhint %}

2. 유틸리티를 설치합니다:

```
sudo apt -y install apt-transport-https ca-certificates wget gnupg
```

3. 신뢰할 수 있는 키 목록에 Ripple 패키지 서명 GPG 키를 추가합니다:

```
sudo mkdir /usr/local/share/keyrings/
wget -q -O - "https://repos.ripple.com/repos/api/gpg/key/public" | gpg --dearmor > ripple-key.gpg
sudo mv ripple-key.gpg /usr/local/share/keyrings
```

4. 새로 추가한 키의 지문을 확인합니다:

```
gpg /usr/local/share/keyrings/ripple-key.gpg
```

출력에는 다음과 같은 Ripple에 대한 항목이 포함되어야 합니다:

```
gpg: WARNING: no command supplied.  Trying to guess what you mean ...
pub   rsa3072 2019-02-14 [SC] [expires: 2026-02-17]
    C0010EC205B35A3310DC90DE395F97FFCCAFD9A2
uid           TechOps Team at Ripple <techops+rippled@ripple.com>
sub   rsa3072 2019-02-14 [E] [expires: 2026-02-17]
```

특히 지문이 일치하는지 확인하세요. (위의 예에서 지문은 C001로 시작하는 세 번째 줄에 있습니다.)

5. 운영 체제 버전에 적합한 Ripple 저장소를 추가합니다:

```
echo "deb [signed-by=/usr/local/share/keyrings/ripple-key.gpg] https://repos.ripple.com/repos/rippled-deb focal stable" | \
    sudo tee -a /etc/apt/sources.list.d/ripple.list
```

위의 예는 **우분투 20.04 포컬 포사**에 적합합니다.

6. Ripple 저장소를 가져옵니다.

```
sudo apt -y update
```

7. 클리오 소프트웨어 패키지를 설치합니다. 두 가지 옵션이 있습니다:

* 동일한 컴퓨터에서 rippled를 실행하려면 두 서버를 모두 설정하는 클리오 패키지를 설치합니다:

```
sudo apt -y install clio
```

* rippled와 별도의 컴퓨터에서 클리오를 실행하려면, 클리오만 설정하는 클리오-서버 패키지를 설치합니다:

```
sudo apt -y install clio-server
```

8. 별도의 컴퓨터에서 rippled를 실행하는 경우, 해당 컴퓨터가 rippled를 가리키도록 클리오 구성 파일을 수정합니다. 동일한 머신에 두 가지를 모두 설치하기 위해 클리오 패키지를 사용한 경우 이 단계를 건너뛸 수 있습니다.
   1. 클리오 서버의 구성 파일을 편집하여 rippled 서버에 대한 연결 정보를 수정합니다. 패키지는 이 파일을 `/opt/clio/etc/config.json`에 설치합니다.

```
"etl_sources":
[
    {
        "ip":"127.0.0.1",
        "ws_port":"6006",
        "grpc_port":"50051"
    }
]
```

여기에는 다음이 포함됩니다:

* rippled 서버의 IP.
* rippled 서버가 암호화되지 않은 웹소켓 연결을 수락하는 포트.
* rippled이 gRPC 요청을 수락하는 포트.

{% hint style="info" %}
Note:

etl\_sources 섹션에 항목을 더 추가하여 여러 rippled 서버를 데이터 소스로 사용할 수 있습니다. 이렇게 하면 클리오는 목록에 있는 모든 서버에 걸쳐 요청을 로드 밸런싱하며, rippled 서버 중 하나 이상이 동기화되어 있는 한 네트워크를 따라잡을 수 있습니다.
{% endhint %}

예제 구성 파일은 로컬 루프백 네트워크(127.0.0.1)에서 실행 중인 rippled 서버에 액세스하며, 포트 6006의 웹 소켓(WS)과 포트 50051의 gRPC를 사용합니다.

2. rippled 서버의 구성 파일을 업데이트하여 클리오 서버가 rippled 서버에 연결할 수 있도록 합니다. 패키지는 이 파일을 /etc/opt/ripple/rippled.cfg에 설치합니다.

* 암호화되지 않은 웹소켓 연결을 허용할 포트를 엽니다.

```
[port_ws_public]
port = 6005
ip = 0.0.0.0
protocol = ws
```

* gRPC 요청을 처리할 포트를 열고 secure\_gateway 항목에 클리오 서버의 IP를 지정합니다.

```
[port_grpc]
port = 50051
ip = 0.0.0.0
secure_gateway = 127.0.0.1
```

{% hint style="info" %}
Tip:

rippled와 동일한 시스템에서 클리오를 실행하지 않는 경우, 예제 구절의 secure\_gateway를 클리오 서버의 IP 주소를 사용하도록 변경하세요.
{% endhint %}

9. 클리오 systemd 서비스를 활성화하고 시작합니다.

```
sudo systemctl enable clio
```

10. rippled 및 클리오 서버를 시작합니다.

```
sudo systemctl start rippled
sudo systemctl start clio
```

새로운 데이터베이스로 시작하는 경우, 클리오는 전체 ledger를 다운로드해야 합니다. 시간이 다소 걸릴 수 있습니다. 두 서버를 처음 시작하는 경우, 클리오는 ledger을 추출하기 전에 rippled에 동기화될 때까지 기다리기 때문에 시간이 더 오래 걸릴 수 있습니다.
