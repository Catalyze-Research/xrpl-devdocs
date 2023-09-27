# 관리자 API 메소드

다음 관리자 API 메소드를 사용하여 rippled 서버를 관리하세요. 관리자 메소드는 서버 운영을 담당하는 신뢰할 수 있는 담당자만 사용할 수 있습니다. 관리자 메소드에는 서버 관리, 모니터링, 디버깅을 위한 명령이 포함되어 있습니다.

관리자 명령은 rippled.cfg 파일에서 관리자로 식별되는 호스트와 포트에서 rippled에 연결하는 경우에만 사용할 수 있습니다. 기본적으로 커맨드라인 클라이언트는 관리자 연결을 사용합니다. rippled에 연결하는 방법에 대한 자세한 내용은 rippled API 시작하기를 참조하세요.

## 키 생성 방법

다음 메소드를 사용하여 키를 생성하고 관리합니다.

* validation\_create - rippled 노드 키 쌍에 대해 형식이 지정된 키를 생성합니다. (검증인은 이 메소드로 생성된 키 대신 토큰을 사용해야 합니다.)
* wallet\_propose - 새 계정에 대한 키를 생성합니다.

## 로깅 및 데이터 관리 메소드

이 메소드를 사용하여 로그 수준과 ledger 등 기타 데이터를 관리합니다.

* can\_delete - 특정 ledger까지 ledger의 온라인 삭제를 허용합니다.
* download\_shard - 특정 ledger 기록의 샤드를 다운로드합니다.
* ledger\_cleaner - 손상된 데이터를 검증하도록 ledger 클리너 서비스를 구성합니다.
* ledger\_request - 피어 서버에 특정 ledger 버전을 쿼리합니다.
* log\_level - 로그 상세 정보를 가져오거나 수정합니다.
* logrotate - 로그 파일을 다시 엽니다.
* node\_to\_shard - ledger 저장소에서 샤드 저장소로 데이터를 복사합니다.

## 서버 제어 메소드

rippled 서버를 관리하려면 다음 메소드를 사용하세요.

* ledger\_accept - stand-alone 모드에서 ledger을 닫고 진행합니다.
* stop - rippled 서버를 종료합니다.

## 서명 메소드

트랜잭션에 서명하려면 다음 메소드를 사용합니다.

* sign - 트랜잭션에 암호화 방식으로 서명합니다.
* sign\_for - 다중 서명에 기여합니다.

기본적으로 이러한 메소드는 관리자 전용입니다. 서버 관리자가 공개 서명을 사용하도록 설정한 경우 공개 메소드로 사용할 수 있습니다.

## 피어 관리 메소드

이 메소드를 사용해 P2P XRP Ledger 네트워크에서 서버의 연결을 관리할 수 있습니다.

* connect - rippled 서버가 특정 피어에 연결하도록 강제합니다.
* peer\_reservations\_add - 특정 피어를 위해 예약된 슬롯을 추가하거나 업데이트합니다.
* peer\_reservations\_del - 특정 피어에 대한 예약된 슬롯을 제거합니다.
* peer\_reservations\_list - 특정 피어에 대한 예약된 슬롯을 나열합니다.
* peers - 연결된 피어 서버에 대한 정보를 가져옵니다.

## 상태 및 디버깅 방법

이 메소드를 사용하여 네트워크 및 서버의 상태를 확인할 수 있습니다.

* consensus\_info - 컨센서스 상태에 대한 정보를 얻습니다.
* feature - 프로토콜 수정에 대한 정보를 가져옵니다.
* fetch\_info - 서버와 네트워크의 동기화에 대한 정보를 가져옵니다.
* get\_counts - 서버의 내부 및 메모리 사용량에 대한 통계를 가져옵니다.
* manifest - 알려진 유효성 검증인에 대한 최신 공개 키 정보를 가져옵니다.
* print - 내부 하위 시스템에 대한 정보를 가져옵니다.
* validator\_info - 유효성 검증인으로 구성된 경우 서버의 유효성 검증인 설정에 대한 정보를 가져옵니다.
* validator\_list\_sites - 유효성 검증인 목록을 게시하는 사이트에 대한 정보를 가져옵니다.
* validators - 현재 유효성 검증인에 대한 정보를 가져옵니다.

## 더 이상 사용되지 않는 메소드

다음 관리 명령은 더 이상 사용되지 않으며 별도의 공지 없이 제거될 수 있습니다:

* ledger\_header - 대신 ledger 메소드를 사용합니다.
* unl\_add, unl\_delete, unl\_list, unl\_load, unl\_network, unl\_reset, unl\_score - 대신 유효성 검증인 관리를 위해 validators.txt 구성 파일을 사용합니다.
* wallet\_seed - 대신 wallet\_propose 메소드를 사용합니다.
* validation\_seed 메소드는 `rippled` v0.29.1.부터 제거되었습니다.
