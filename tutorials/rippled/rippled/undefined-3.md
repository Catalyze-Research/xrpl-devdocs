# 우분투 또는 데비안에서 수동 업데이트

이 페이지는 우분투 리눅스에서 rippled의 최신 버전으로 수동 업데이트하는 방법을 설명합니다. 이 지침은 기본 패키지를 사용하여 이미 rippled을 설치했다고 가정합니다. Ripple은 가능하면 자동 업데이트를 설정할 것을 권장합니다.

{% hint style="info" %}
Caution: Ripple은 v1.7.0 출시 직전에 바이너리 패키지에 서명하는 데 사용되는 GPG 키를 갱신했습니다. 1.7.0 이전 버전에서 업그레이드하는 경우, 먼저 다음과 같이 업데이트된 공개키를 다운로드하고 수동으로 신뢰해야 합니다:

```
wget -q -O - "https://repos.ripple.com/repos/api/gpg/key/public" | \
  sudo apt-key add -
```

자세한 내용은 rippled 1.7.0 릴리스 노트를 참조하세요.
{% endhint %}

{% hint style="info" %}
Tip:\
이러한 단계를 한 번에 수행하려면 rippled 패키지에 포함되어 있으며 Ubuntu 및 Debian과 호환되는 /opt/ripple/bin/update-rippled.sh 스크립트를 실행하면 됩니다. 이 스크립트는 sudo 사용자로 실행해야 합니다.
{% endhint %}

수동으로 업데이트하려면 다음 단계를 완료하세요:

1. 저장소를 업데이트합니다:

```
$ sudo apt -y update
```

2. rippled 패키지를 업그레이드합니다:

```
$ sudo apt -y upgrade rippled
```

3. systemd 단위 파일을 다시 로드합니다:

```
$ sudo systemctl daemon-reload
```
