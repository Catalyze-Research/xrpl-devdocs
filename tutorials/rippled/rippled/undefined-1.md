# 우분투 또는 데비안 리눅스에 설치

이 페이지는 우분투 리눅스 18.04 이상 또는 데비안 10 이상에서 apt 유틸리티를 사용하여 최신 안정 버전의 rippled을 설치하기 위한 권장 지침을 설명합니다.

이 지침은 Ripple에서 컴파일한 바이너리를 설치합니다.

## 요구 조건

rippled을 설치하기 전에 시스템 요구 사항을 충족해야 합니다.

## 설치 단계

1. 저장소를 업데이트합니다:

```
sudo apt -y update
```

2. 유틸리티를 설치합니다:

```
sudo apt -y install apt-transport-https ca-certificates wget gnupg
```

3. 신뢰할 수 있는 키 목록에 Ripple의 패키지 서명 GPG 키를 추가합니다:

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

5. 운영 체제 버전에 맞는 Ripple 저장소를 추가합니다:

```
echo "deb [signed-by=/usr/local/share/keyrings/ripple-key.gpg] https://repos.ripple.com/repos/rippled-deb focal stable" | \
    sudo tee -a /etc/apt/sources.list.d/ripple.list
```

위의 예는 우분투 20.04 포컬 포사에 적합합니다. 다른 운영 체제의 경우 focal이라는 단어를 다음 중 하나로 바꾸세요:

* **Ubuntu 18.04 Bionic Beaver**용 bionic.
* **Debian 10 Buster**용 buster.
* **Debian 11 Bullseye**용 bullseye.
* **Ubuntu 22.04 Jammy Jellyfish**용 jammy.

rippled의 개발 또는 사전 릴리스 버전에 액세스하려면 안정 버전 대신 다음 중 하나를 사용하세요:

* unstable - 사전 릴리스 빌드 (릴리스 브랜치).
* nightly- 실험/개발 빌드 (개발 브랜치).

{% hint style="info" %}
Warning:

불안정 및 야간 빌드는 언제든지 중단될 수 있습니다. 이러한 빌드를 프로덕션 서버에 사용하지 마세요.
{% endhint %}

6. Ripple 저장소를 가져옵니다.

```
sudo apt -y update
```

7. rippled 소프트웨어 패키지를 설치합니다:

```
sudo apt -y install rippled
```

8. rippled 서비스의 상태를 확인합니다:

```
systemctl status rippled.service// Some codec
```

rippled 서비스가 자동으로 시작되어야 합니다. 그렇지 않은 경우 수동으로 시작할 수 있습니다:

```
sudo systemctl start rippled.service
```

9. 선택 사항: rippled가 권한이 있는 포트에 바인딩하도록 허용합니다.\
   이렇게 하면 포트 80 또는 443에서 들어오는 API 요청을 처리할 수 있습니다. (이렇게 하려면 구성 파일의 포트 설정도 업데이트해야 합니다.)

```
sudo setcap 'cap_net_bind_service=+ep' /opt/ripple/bin/rippled
```

## 다음 단계

나머지 XRP Ledger 네트워크와 동기화하는 데 몇 분 정도 걸릴 수 있으며, 이 시간 동안 서버는 다양한 경고를 출력합니다. 로그 메시지에 대한 자세한 내용은 로그 메시지 이해를 참조하세요.

rippled 커맨드라 인터페이스를 사용하여 서버가 네트워크와 동기화되었는지 확인할 수 있습니다:

```
/opt/ripple/bin/rippled server_info
```

응답의 server\_state가 가득 찼거나 제안 중이면 서버가 네트워크와 완전히 동기화된 것입니다. 그렇지 않으면 더 오래 기다려야 할 수 있습니다. 새 서버는 보통 15분 이내에 동기화되며, 이미 ledger 기록이 저장되어 있는 서버는 더 오래 걸릴 수 있습니다.

서버가 나머지 네트워크와 동기화되면, 트랜잭션을 제출하거나 XRP Ledger에 대한 API 액세스를 얻는 데 사용할 수 있는 완전한 기능의 XRP Ledger P2P 서버를 갖게 됩니다. 서버와 통신하는 다양한 방법은 클라이언트 라이브러리 또는 HTTP/웹소켓 API를 참조하세요.

비즈니스에 XRP Ledger를 사용하거나 네트워크의 안정성에 기여하고 싶다면 하나의 서버를 검증인을 실행해야 합니다. 서버 검증과 서버를 실행해야 하는 이유에 대한 자세한 내용은 rippled를 검증인으로 실행하기를 참조하세요.

서버를 시작하는 데 문제가 있나요? rippled 서버가 시작되지 않음을 참조하세요.

## 추가 구성

rippled는 기본 구성으로 XRP Ledger에 연결해야 합니다. 하지만 rippled.cfg 파일을 편집하여 설정을 변경할 수 있습니다. 구성 설정에 대한 권장 사항은 용량 계획을 참조하세요.

권장 설치는 기본적으로 구성 파일 /etc/opt/ripple/rippled.cfg를 사용합니다. 구성 파일을 저장할 수 있는 다른 위치로는 $HOME/.config/ripple/rippled.cfg(여기서 $HOME은 rippled을 실행하는 사용자의 홈 디렉터리), $HOME/.local/ripple/rippled.cfg 또는 rippled을 시작한 현재 작업 디렉터리 등이 있습니다.

모든 구성 옵션에 대한 설명은 rippled된 GitHub 저장소를 참조하세요.

구성 변경 사항을 적용하려면 rippled을 다시 시작해야 합니다:

```
sudo systemctl restart rippled.service
```

\[debug\_logfile] 또는 \[database\_path] 섹션을 변경하는 경우, rippled을 실행하는 사용자에게 새로 구성된 경로에 대한 소유권을 부여해야 할 수 있습니다.

## 업데이트

rippled를 정기적으로 업데이트해야 나머지 XRP Ledger 네트워크와 동기화 상태를 유지할 수 있습니다. rippled Google 그룹을 구독하여 새로운 rippled 릴리스에 대한 알림을 받을 수 있습니다.

rippled 패키지에는 리눅스에서 자동 업데이트를 활성화하는 데 사용할 수 있는 스크립트가 포함되어 있습니다. 다른 플랫폼에서는 수동으로 업데이트해야 합니다.
