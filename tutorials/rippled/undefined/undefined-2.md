# 링크 압축 사용

rippled 서버는 P2P 통신을 압축하여 대역폭을 절약할 수 있지만, CPU 사용량이 늘어나는 대가를 치르게 됩니다. 링크 압축을 활성화하면, 서버는 링크 압축을 활성화한 피어 서버와의 통신을 자동으로 압축합니다. [![New in: rippled 1.6.0](https://img.shields.io/badge/New%20in-rippled%201.6.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/1.6.0)

## 단계

서버에서 링크 압축을 사용 설정하려면 다음 단계를 완료하세요:

## 1. rippled 서버의 구성 파일을 편집합니다.

```
$ vim /etc/opt/ripple/rippled.cfg
```

권장 설치는 기본적으로 구성 파일 /etc/opt/ripple/rippled.cfg를 사용합니다. 구성 파일을 저장할 수 있는 다른 위치로는 $HOME/.config/ripple/rippled.cfg(여기서 $HOME은 rippled를 실행하는 사용자의 홈 디렉터리), $HOME/.local/ripple/rippled.cfg 또는 rippled를 시작한 현재 작업 디렉터리 등이 있습니다.

## 2. 구성 파일에서`[compression]`구절을 추가하거나 주석 처리 해제합니다.

압축을 활성화하려면:

```
[compression]
true
```

압축을 비활성화하려면 false를 사용합니다(기본값).

3\. rippled 서버를 다시 시작합니다.

```
$ sudo systemctl restart rippled.service
```

다시 시작하면 서버가 링크 압축을 사용하도록 설정한 다른 피어와 자동으로 링크 압축을 사용합니다.
