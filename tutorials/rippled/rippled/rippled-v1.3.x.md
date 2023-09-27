# rippled v1.3.x 마이그레이션 지침

이 문서는 rippled 1.2.4 이전 버전에서 rippled 1.3 이상으로 업그레이드하기 위한 마이그레이션 프로세스에 대해 설명합니다. 이 마이그레이션 프로세스는 rippled 설치 프로세스가 버전 1.3부터 변경되었기 때문에 필요합니다.

이 문서에서는 지원되는 플랫폼에서 업그레이드하기 위한 마이그레이션 단계를 제공합니다:

* CentOS 또는 Red Hat 엔터프라이즈 리눅스(RHEL).&#x20;
* 우분투 리눅스.&#x20;

다른 플랫폼의 경우 소스에서 컴파일하기 위한 업데이트된 지침을 참조하세요. (우분투, macOS 또는 Windows).

## CentOS 또는 Red Hat 엔터프라이즈 리눅스(RHEL)에서 마이그레이션하기&#x20;

rippled의 공식 RPM 저장소및 사용 지침이 변경되었습니다. 자동 업데이트를 사용하도록 설정한 경우, 시스템에서 마이그레이션을 자동으로 수행합니다. 이전 저장소에서 새 저장소로 수동으로 마이그레이션하려면 다음 단계를 완료하세요:

1. rippled 서버를 중지합니다.

```
$ sudo systemctl stop rippled.service
```

2. 기존 Ripple 저장소 패키지를 제거합니다.

```
$ sudo rpm -e ripple-repo
```

rippled-repo 패키지는 이제 사용되지 않습니다. 이 패키지는 1.3.1 버전에 마지막으로 업데이트 되었습니다. 앞으로 저장소를 변경하려면 ripple.repo 파일을 수동으로 변경해야 합니다.

3. Ripple의 새로운 yum 저장소를 추가합니다:

```
$ cat << REPOFILE | sudo tee /etc/yum.repos.d/ripple.repo
[ripple-stable]
name=XRP Ledger Packages
baseurl=https://repos.ripple.com/repos/rippled-rpm/stable/
enabled=1
gpgcheck=0
gpgkey=https://repos.ripple.com/repos/rippled-rpm/stable/repodata/repomd.xml.key
repo_gpgcheck=1
REPOFILE
```

4. 새 rippled 패키지를 설치합니다:

```
$ sudo yum install rippled
```

버전 1.3.1에서는 구성 파일(rippled.cfg 및 validators.txt)을 변경할 필요가 없습니다. 이 업데이트 절차는 기존 구성 파일을 그대로 유지합니다.

5. systemd 단위 파일을 다시 로드합니다:

```
$ sudo systemctl daemon-reload
```

6. rippled 서비스를 시작합니다:

```
$ sudo systemctl start rippled.service
```

{% hint style="info" %}
Warning:

자동 업데이트를 사용하는 경우, 이 마이그레이션 프로세스를 수행한 후에도 계속 작동해야 합니다. 그러나 ripple-repo 패키지는 이제 더 이상 사용되지 않습니다. 따라서 앞으로 rippled의 저장소를 변경하려면 저장소 파일을 수동으로 업데이트해야 할 수 있습니다.
{% endhint %}

## 우분투 리눅스에서의 마이그레이션&#x20;

버전 1.3 이전에는 우분투 리눅스에 rippled을 설치하기 위해 지원되는 방법은 Alien을 사용하여 RPM 패키지를 설치하는 것이었습니다. rippled 1.3.1 버전부터 rippled은 우분투 및 데비안 리눅스용 기본 패키지를 제공하며, 이 패키지를 설치하는 것이 권장됩니다. 이미 RPM 패키지가 설치되어 있는 경우, 설치 단계를 완료하여 패키지를 업그레이드하고 네이티브 APT(.deb) 패키지로 전환하세요.

구성 파일(/opt/ripple/etc/rippled.cfg 및 /opt/ripple/etc/validators.txt)을 변경한 경우, 설치 중에 구성 파일을 패키지의 최신 버전으로 덮어쓸 것인지 묻는 메시지가 표시될 수 있습니다. 버전 1.3은 구성 파일을 변경할 필요가 없으므로 기존 구성 파일을 변경하지 않고 안전하게 유지할 수 있습니다.

1.3용 기본 APT 패키지를 설치한 후에는 서비스를 다시 로드/재시작해야 합니다:

1. 시스템 단위 파일을 다시 로드합니다:

```
$ sudo systemctl daemon-reload
```

2. rippled된 서비스를 재시작합니다:

```
$ sudo systemctl restart rippled.service// Some code
```

다른 패키지에 더 이상 Alien이 필요하지 않은 경우, 다음 단계에 따라 선택적으로 Alien과 해당 종속성을 제거할 수 있습니다:

1. Alien을 제거합니다:

```
$ sudo apt -y remove alien
```

2. 사용하지 않는 dependencies를 제거합니다:

```
$ sudo apt -y autoremove// Some code
```

## 자동 업데이트&#x20;

rippled v1.3 패키지에는 우분투 및 데비안 리눅스에서 작동하는 업데이트된 자동 업데이트 스크립트가 포함되어 있습니다. 자세한 내용은 리눅스에서 자동으로 rippled 업데이트하기를 참조하세요.
