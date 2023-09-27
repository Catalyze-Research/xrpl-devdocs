# CentOS/Red Hat에서 수동 업데이트

이 페이지는 CentOS 또는 Red Hat 엔터프라이즈 리눅스에서 rippled의 최신 릴리스로 수동 업데이트하는 방법을 설명합니다. rippled은 가능한 경우 자동 업데이트를 설정할 것을 권장합니다.

이 지침은 yum 저장소에서 이미 rippled을 설치했다고 가정합니다.

{% hint style="info" %}
Tip:

이러한 단계를 한 번에 수행하려면 rippled 패키지에 포함된 /opt/ripple/bin/update-rippled.sh 스크립트를 실행하면 됩니다. 이 스크립트는 sudo 사용자로 실행해야 합니다.
{% endhint %}

수동으로 업데이트하려면 다음 단계를 완료하세요:

1. 이전 버전에서 rippled 1.7.0으로 업그레이드하는 경우, 저장소를 다시 추가하여 rippled의 업데이트된 GPG 키를 받습니다. \
   그렇지 않으면 이 단계를 건너뛰세요:

```
$ cat << REPOFILE | sudo tee /etc/yum.repos.d/ripple.repo
[ripple-stable]
name=XRP Ledger Packages
enabled=1
gpgcheck=0
repo_gpgcheck=1
baseurl=https://repos.ripple.com/repos/rippled-rpm/stable
gpgkey=https://repos.ripple.com/repos/rippled-rpm/stable/repodata/repomd.xml.key
REPOFILE
```

2. 최신 rippled 패키지를 다운로드하여 설치합니다:

```
$ sudo yum update rippled
```

이 업데이트 절차는 기존 구성 파일을 그대로 유지합니다.

3. systemd 단위 파일을 다시 로드합니다:

```
$ sudo systemctl daemon-reload
```

4. rippled 서비스를 다시 시작합니다:

```
$ sudo service rippled restart
```
