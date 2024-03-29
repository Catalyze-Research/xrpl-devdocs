# 피어링 구성

XRP Ledger의 P2P 프로토콜은 대부분의 경우 피어 연결을 자동으로 관리합니다. 경우에 따라 서버의 가용성과 나머지 네트워크와의 연결을 극대화하기 위해 서버가 연결할 피어를 수동으로 조정하고 싶을 수도 있습니다.

동일한 데이터센터에서 여러 서버를 실행하는 경우 효율성을 높이기 위해 서버를 클러스터링할 수 있습니다. P2P 네트워크 토폴로지에서 중요한 허브와 같이 실행하지는 않지만 연결 상태를 유지하려는 서버에는 예약된 피어 슬롯을 사용할 수 있습니다. 다른 피어에 대해서는 서버가 자동으로 피어를 찾아 연결을 관리할 수 있지만, 때때로 바람직하지 않은 동작을 하는 피어를 차단하기 위해 개입해야 할 수도 있습니다.

## 클러스터 rippled 서버&#x20;

효율성을 높이기 위해 작업을 공유하는 서버 그룹을 설정하세요.

## 비공개 서버 구성&#x20;

신뢰할 수 있는 특정 피어에만 연결하도록 서버를 설정하세요.

## 피어 크롤러 구성&#x20;

rippled 서버가 상태 및 피어에 대해 공개적으로 보고하는 정보의 양을 구성합니다.

## 링크 압축 사용&#x20;

피어 간 통신을 압축하여 대역폭을 절약하세요.

## 피어링을 위한 포트 포워드

포트 rippled 서버로 들어오는 피어를 허용하도록 방화벽을 구성하세요.

## 특정 피어에 수동으로 연결&#x20;

rippled 서버를 특정 피어에 연결합니다.

## 최대 피어 수 설정&#x20;

rippled 서버가 연결할 수 있는 최대 피어 수를 설정합니다.

## 피어 예약 사용&#x20;

피어 예약을 사용하여 특정 피어에 보다 안정적으로 연결하도록 설정합니다.
