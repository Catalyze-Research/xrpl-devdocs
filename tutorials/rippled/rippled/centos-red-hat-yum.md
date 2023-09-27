# CentOS/Red Hat에 yum으로 설치하기

이 페이지는 rippled의 yum 저장소를 사용하여 CentOS 7 또는 **Red Hat Enterprise** 리눅스 7에 최신 안정 버전의 rippled을 설치하기 위한 권장 지침을 설명합니다.

이 지침은 Ripple에서 컴파일한 바이너리를 설치합니다.

## 요구 사항

rippled을 설치하기 전에 시스템 요구 사항을 충족해야 합니다.

## 설치 단계&#x20;

1. Ripple RPM 저장소를 설치합니다: 원하는 릴리스의 안정성을 위해 적절한 RPM 저장소를 선택합니다:

* 안정적인 최신 프로덕션 릴리스의 경우(마스터 브랜치).
* 불안정한 사전 릴리스 빌드의 경우(릴리스 브랜치).
* 실험/개발 목적의 나이틀리 빌드(개발 브랜치).

{% tabs %}
{% tab title="Stable" %}
```
cat << REPOFILE | sudo tee /etc/yum.repos.d/ripple.repo
[ripple-stable]
name=XRP Ledger Packages
enabled=1
gpgcheck=0
repo_gpgcheck=1
baseurl=https://repos.ripple.com/repos/rippled-rpm/stable/
gpgkey=https://repos.ripple.com/repos/rippled-rpm/stable/repodata/repomd.xml.key
REPOFILE
```
{% endtab %}

{% tab title="Pre-release" %}
```
cat << REPOFILE | sudo tee /etc/yum.repos.d/ripple.repo
[ripple-unstable]
name=XRP Ledger Packages
enabled=1
gpgcheck=0
repo_gpgcheck=1
baseurl=https://repos.ripple.com/repos/rippled-rpm/unstable/
gpgkey=https://repos.ripple.com/repos/rippled-rpm/unstable/repodata/repomd.xml.key
REPOFILE
```
{% endtab %}

{% tab title="Development" %}
```
cat << REPOFILE | sudo tee /etc/yum.repos.d/ripple.repo
[ripple-unstable]
name=XRP Ledger Packages
enabled=1
gpgcheck=0
repo_gpgcheck=1
baseurl=https://repos.ripple.com/repos/rippled-rpm/unstable/
gpgkey=https://repos.ripple.com/repos/rippled-rpm/unstable/repodata/repomd.xml.key
REPOFILE
```
{% endtab %}
{% endtabs %}

2. 최신 저장소 업데이트를 가져옵니다:

```
sudo yum -y update
```

3. 새 rippled 패키지를 설치합니다:

```
sudo yum install rippled
```

4. systemd 단위 파일을 다시 로드합니다:

```
sudo systemctl daemon-reload
```

5. 부팅 시 rippled 서비스가 시작되도록 구성합니다:

```
sudo systemctl enable rippled.service
```

6. rippled 서비스를 시작합니다:

```
sudo systemctl start rippled.service
```

## 다음 단계&#x20;

나머지 XRP Ledger 네트워크와 동기화 하는데 몇 분이 걸릴 수 있으며, 이 기간 동안 서버는 다양한 경고를 출력합니다. 로그 메시지에 대한 자세한 내용은 로그 메시지 이해를 참조하세요.

rippled 커맨드라인 인터페이스를 사용하여 서버가 네트워크와 동기화 되었는지 확인할 수 있습니다:

```
/opt/ripple/bin/rippled server_info
```

응답의 server\_state가 가득 찼거나 제안 중이면 서버가 네트워크와 완전히 동기화된 것입니다. 그렇지 않으면 더 오래 기다려야 할 수 있습니다. 새 서버는 보통 15분 이내에 동기화되며, 이미 ledger 기록이 저장되어 있는 서버는 더 오래 걸릴 수 있습니다.

서버가 나머지 네트워크와 동기화되면, 트랜잭션을 제출하거나 XRP Ledger에 대한 API 액세스를 얻는 데 사용할 수 있는 완전한 기능의 XRP Ledger P2P 서버를 갖게 됩니다. 서버와 통신하는 다양한 방법은 클라이언트 라이브러리 또는 HTTP/웹소켓 API를 참조하세요.

비즈니스에 XRP Ledger를 사용하거나 네트워크의 안정성에 기여하고 싶다면 하나의 서버를 검증자로 실행해야 합니다. 서버 검증과 서버를 실행해야 하는 이유에 대한 자세한 내용은 rippled을 검증인으로 실행하기를 참조하세요.

서버를 시작하는 데 문제가 있나요? rippled 서버가 시작되지 않음을 참조하세요.

## 추가 구성&#x20;

rippled는 기본 구성으로 XRP Ledger에 연결해야 합니다. 하지만 rippled.cfg 파일을 편집하여 설정을 변경할 수 있습니다. 구성 설정에 대한 권장 사항은 용량 계획을 참조하세요.

권장 설치는 기본적으로 구성 파일 /etc/opt/ripple/rippled.cfg를 사용합니다. 구성 파일을 저장할 수 있는 다른 위치로는 $HOME/.config/ripple/rippled.cfg(여기서 $HOME은 rippled을 실행하는 사용자의 홈 디렉터리), $HOME/.local/ripple/rippled.cfg 또는 rippled을 시작한 현재 작업 디렉터리 등이 있습니다.&#x20;

모든 구성 옵션에 대한 설명은 rippled GitHub 저장소를 참조하세요.

구성 변경 사항을 적용하려면 rippled를 다시 시작해야 합니다:

\[debug\_logfile] 또는 \[database\_path] 섹션을 변경하는 경우, rippled를 실행하는 사용자에게 새로 구성된 경로에 대한 소유권을 부여해야 할 수 있습니다.

## 업데이트&#x20;

rippled를 정기적으로 업데이트해야 나머지 XRP Ledger 네트워크와 동기화 상태를 유지할 수 있습니다. rippled Google 그룹을 구독하여 새로운 rippled 릴리스에 대한 알림을 받을 수 있습니다.

rippled 패키지에는 리눅스에서 자동 업데이트를 활성화하는 데 사용할 수 있는 스크립트가 포함되어 있습니다. 다른 플랫폼에서는 수동으로 업데이트해야 합니다.
