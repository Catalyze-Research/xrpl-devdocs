# 피어 크롤러 구성

기본적으로 rippled 서버는 피어 크롤러 API를 사용해 요청하는 모든 사람에게 통계를 공개적으로 제공하여 XRP Ledger의 P2P 네트워크의 상태와 토폴로지를 더 쉽게 추적할 수 있도록 합니다. 더 많은 또는 더 적은 정보를 제공하거나 피어 크롤러 요청을 완전히 거부하도록 서버를 구성할 수 있습니다. [![New in: rippled 1.2.0](https://img.shields.io/badge/New%20in-rippled%201.2.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/1.2.0)

이 문서에는 두 가지 옵션에 대한 단계가 포함되어 있습니다:

* 피어 크롤러가 보고하는 정보 변경하기.
* 피어 크롤러 비활성화하기.

## 피어 크롤러가 보고하는 정보 변경하기

피어 크롤러 요청에 대한 응답으로 서버에서 제공하는 정보의 양을 구성하려면 다음 단계를 완료하세요:

1. rippled의 설정 파일을 편집합니다.

```
vim /etc/opt/ripple/rippled.cfg
```

권장 설치는 기본적으로 구성 파일 /etc/opt/ripple/rippled.cfg를 사용합니다. 구성 파일을 저장할 수 있는 다른 위치로는 $HOME/.config/ripple/rippled.cfg(여기서 $HOME은 rippled를 실행하는 사용자의 홈 디렉터리), $HOME/.local/ripple/rippled.cfg 또는 rippled를 시작한 현재 작업 디렉터리 등이 있습니다.

2. 구성 파일에 \[crawl] 구절을 추가하거나 업데이트하고 변경 사항을 저장합니다:

```
[crawl]
overlay = 1
server = 1
counts = 0
unl = 1
```

이 구절의 필드는 서버가 피어 크롤러 응답에서 반환하는 필드를 제어합니다. 구성 필드의 이름은 API 응답의 필드와 일치합니다. 값이 1인 설정은 응답에 해당 필드를 포함한다는 의미입니다. 값이 0이면 응답에서 해당 필드를 생략합니다. 이 예는 각 설정의 기본값을 보여줍니다.

3. 구성 파일에 변경 사항을 저장한 후 rippled 서버를 다시 시작하여 업데이트된 구성을 적용합니다:

```
systemctl restart rippled
```

## 피어 크롤러 비활성화

서버에서 피어 크롤러 API를 비활성화하여 피어 크롤러 요청에 전혀 응답하지 않도록 하려면 다음 단계를 완료하세요:

1. rippled의 구성 파일을 편집합니다.

```
vim /etc/opt/ripple/rippled.cfg// Some code
```

권장 설치는 기본적으로 /etc/opt/ripple/rippled.cfg 구성 파일을 사용합니다. 구성 파일을 저장할 수 있는 다른 위치로는 $HOME/.config/ripple/rippled.cfg(여기서 $HOME은 rippled를 실행하는 사용자의 홈 디렉터리), $HOME/.local/ripple/rippled.cfg 또는 rippled를 시작한 현재 작업 디렉터리 등이 있습니다.

2. 구성 파일에 \[crawl] 구절을 추가하거나 업데이트하고 변경 사항을 저장합니다:

```
[crawl]
0
```

크롤링 구절의 다른 모든 내용을 제거하거나 주석 처리합니다.

3. 구성 파일에 변경 사항을 저장한 후 rippled 서버를 다시 시작하여 업데이트된 구성을 적용합니다:

```
systemctl restart rippled
```
