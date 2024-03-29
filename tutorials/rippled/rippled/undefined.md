# 시스템 요구 사항

## 권장 사양&#x20;

프로덕션 환경에서 안정적인 성능을 얻으려면 다음 특성 이상을 갖춘 베어메탈에서 XRP Ledger(rippled) 서버를 실행하는 것이 좋습니다:

* 운영 체제: 우분투(LTS) 또는 CentOS 또는 레드햇 엔터프라이즈 리눅스(최신 릴리스).&#x20;
* CPU: 코어가 8개 이상이고 하이퍼스레딩이 활성화된 인텔 제온 3+GHz 프로세서.&#x20;
* 디스크: SSD/NVMe(버스트 또는 피크가 아닌 10,000 IOPS 지속) 또는 그 이상. 데이터베이스 파티션의 경우 최소 50GB. 지연 시간이 너무 길어 안정적으로 동기화할 수 없으므로 AWS EBS(Amazon Elastic Block Store)는 사용하지 마세요.&#x20;
* RAM: 64GB.&#x20;
* 네트워크: 호스트에 기가비트 네트워크 인터페이스가 있는 엔터프라이즈 데이터 센터 네트워크.&#x20;

## 최소 사양

테스트 목적이나 가끔씩 사용하는 경우, 상용 하드웨어에서 XRP Ledger 서버를 실행할 수 있습니다. 다음 최소 요구 사항은 대부분의 경우 작동하지만, 항상 네트워크와 동기화 상태를 유지하는 것은 아닙니다:

* 운영 체제: 맥OS, 윈도우(64비트) 또는 대부분의 리눅스 배포판(레드햇, 우분투, 데비안 지원).&#x20;
* CPU: 64비트 x86\_64, 4코어 이상.&#x20;
* 디스크: SSD/NVMe(버스트 또는 피크가 아닌 10,000 IOPS 지속) 또는 그 이상. 데이터베이스 파티션의 경우 최소 50GB. 지연 시간이 너무 길어 안정적으로 동기화할 수 없는 AWS EBS(Amazon Elastic Block Store)는 사용하지 마세요.&#x20;
* RAM: 16GB 이상.&#x20;

워크로드에 따라 Amazon EC2의 i3.2xlarge VM 크기가 적합할 수 있습니다. 빠른 네트워크 연결이 바람직합니다. 서버의 클라이언트 처리 부하가 증가하면 리소스 필요량이 증가합니다.

유효성 검사기의 경우, 로깅 및 코어 덤프 스토리지를 위한 추가 1TB 디스크가 있는 z1d.2xlarge를 고려하세요.

## 시스템 시간&#x20;

rippled 서버는 정확한 시간을 유지하는 데 의존합니다. ntpd 또는 chrony와 같은 데몬으로 네트워크 시간 프로토콜(NTP)을 사용하여 시간을 동기화하는 것이 좋습니다.
