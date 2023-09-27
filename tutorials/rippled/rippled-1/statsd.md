# StatsD 구성

rippled은 자신에 대한 건강 및 행동 정보를 StatsD 형식으로 내보낼 수 있습니다. 이러한 메트릭은 rippledmon 또는 StatsD 형식의 메트릭을 허용하는 다른 수집기를 통해 소비하고 시각화할 수 있습니다.

## 구성 단계

rippled 서버에서 StatsD를 활성화하려면 다음 단계를 수행하세요:

1. 통계를 수신하고 집계할 다른 머신에 rippledmon 인스턴스를 설정합니다.

```
$ git clone https://github.com/ripple/rippledmon.git
$ cd rippledmon
$ docker-compose up
```

위 단계를 수행할 때 컴퓨터에 Docker 및 Docker Compose가 설치되어 있는지 확인하세요. rippledmon 구성에 대한 자세한 내용은 rippledmon 저장소를 참조하세요.

2. rippled의 구성 파일에 \[insight] 구절을 추가합니다.

```
[insight]
server=statsd
address=192.0.2.0:8125
prefix=my_rippled
```

* 주소는 rippledmon이 수신 대기 중인 IP 주소와 포트를 사용합니다. 기본적으로 이 포트는 8125입니다.
* 접두사에는 구성 중인 rippled 서버를 식별할 수 있는 이름을 선택합니다. 접두사에는 공백, 콜론 ":" 또는 세로 막대 "|"를 포함하지 않아야 합니다. 접두사는 이 서버에서 내보낸 모든 StatsD 메트릭에 표시됩니다.

권장되는 설치는 기본적으로 구성 파일 /etc/opt/ripple/rippled.cfg를 사용합니다. 구성 파일을 저장할 수 있는 다른 위치로는 $HOME/.config/ripple/rippled.cfg(여기서 $HOME은 ripppled를 실행하는 사용자의 홈 디렉터리), $HOME/.local/ripple/rippled.cfg 또는 ripppled를 시작한 현재 작업 디렉터리 등이 있습니다.

3. rippled 서비스를 다시 시작합니다.

```
$ sudo systemctl restart rippled
```

4. 메트릭이 내보내지고 있는지 확인합니다:

```
$ tcpdump -i en0 | grep UDP
```

en0을 머신에 적합한 네트워크 인터페이스로 바꿉니다. 컴퓨터의 전체 인터페이스 목록을 보려면 $ tcpdump -D를 사용하세요.\
샘플 출력:

```
00:41:53.066333 IP 192.0.2.2.63409 > 192.0.2.0.8125: UDP, length 196
```

rippledmon 인스턴스의 구성된 주소와 포트로의 아웃바운드 트래픽을 나타내는 메시지가 주기적으로 표시되어야 합니다.

각 StatsD 메트릭에 대한 설명은 rippledmon 저장소를 참조하세요.
