# 공개 서명 사용

기본적으로 rippled에 대한 서명 방법은 관리 연결로 제한됩니다. 서명 메소드를 공개 API 메소드로 사용하도록 허용하려면(예: v1.1.0 이전 버전의 rippled에서와 같이) 구성을 변경하여 활성화할 수 있습니다. [![New in: rippled 1.1.0](https://img.shields.io/badge/New%20in-rippled%201.1.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/1.1.0)

이렇게 하면 서버에서 허용하는 경우 "공용" JSON-RPC 및 WebSocket 연결에서 다음 메소드를 사용할 수 있습니다:

* [sign](https://xrpl.org/sign.html)
* [sign\_for](https://xrpl.org/sign\_for.html)
* [submit](https://xrpl.org/submit.html) ("sign-and-submit" 모드에서)

관리자 연결에서 이러한 메소드를 사용하기 위해 공개 서명을 활성화할 필요는 없습니다.

{% hint style="info" %}
Caution:

rippled은 공개 서명을 활성화하는 것을 권장하지 않습니다. wallet\_propose 메소드와 마찬가지로 서명 명령은 관리자 수준의 권한이 필요한 작업을 수행하지 않지만, 관리자 연결로 제한하면 사용자가 보안되지 않은 통신이나 제어하지 않는 서버를 통해 무책임하게 비밀 키를 보내거나 받지 않도록 보호할 수 있습니다.&#x20;
{% endhint %}

공개 서명을 사용 설정하려면 다음 단계를 수행하세요:

1. rippled의 구성 파일을 편집합니다.

```
vim /etc/opt/ripple/rippled.cfg
```

권장 설치는 기본적으로 구성 파일 /etc/opt/ripple/rippled.cfg를 사용합니다. 구성 파일을 저장할 수 있는 다른 위치로는 $HOME/.config/ripple/rippled.cfg(여기서 $HOME은 ripppled를 실행하는 사용자의 홈 디렉터리), $HOME/.local/ripple/rippled.cfg 또는 ripppled를 시작한 현재 작업 디렉터리 등이 있습니다.

2. 구성 파일에 다음 구절을 추가하고 변경 사항을 저장합니다:

```
[signing_support]
true
```

3. rippled 서버를 다시 시작합니다:

```
systemctl restart rippled
```
