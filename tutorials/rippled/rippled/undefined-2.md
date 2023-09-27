# 리눅스에서 자동 업데이트

리눅스에서는 일회성 크론 구성을 통해 최신 버전으로 자동 업그레이드하도록 rippled을 설정할 수 있습니다. Ripple은 가능하면 자동 업데이트를 활성화할 것을 권장합니다.

이 지침은 yum 저장소(CentOS/RedHat) 또는 apt(Ubuntu/Debian)를 사용하여 이미 rippled을 설치했다고 가정합니다.

자동 업데이트를 설정하려면 다음 단계를 완료하세요:

1. opt/ripple/etc/update-rippled-cron이 존재하는지 확인합니다. 존재하지 않는 경우 수동으로 업데이트합니다(CentOS/Red Hat 또는 Ubuntu/Debian).
2. cron.d 폴더에 /opt/ripple/etc/update-rippled-cron 구성 파일에 대한 심볼릭 링크를 생성합니다:

```
sudo ln -s /opt/ripple/etc/update-rippled-cron /etc/cron.d/
```

이 구성은 스크립트를 실행하여 설치된 rippled 패키지를 새 릴리스가 나올 때마다 한 시간 이내에 업데이트합니다. 너무 많은 서버가 동시에 업데이트되어 네트워크가 불안정해지는 것을 방지하기 위해 이 스크립트는 서버를 자동으로 다시 시작하지 않으므로 서버가 다시 시작될 때까지 이전 버전을 계속 실행합니다. [![Updated in: rippled 1.8.1](https://img.shields.io/badge/Updated%20in-rippled%201.8.1-blue.svg) ](https://github.com/ripple/rippled/releases/tag/1.8.1)

3. 새 릴리스가 나올 때마다 업데이트된 소프트웨어로 전환하려면 rippled 서비스를 수동으로 다시 시작해야 합니다.

```
sudo systemctl restart rippled.service
```

{% hint style="info" %}
Caution:

향후 rippled의 저장소가 변경되면 스크립트가 업데이트를 검색하는 URL을 업데이트하기 위해 수동으로 개입해야 할 수 있습니다. 필요한 변경사항에 대한 공지는 XRP Ledger 블로그 또는 rippled 서버 메일링 리스트에서 계속 지켜봐 주시기 바랍니다.
{% endhint %}
