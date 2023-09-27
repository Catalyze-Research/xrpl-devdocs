# 피어링을 위한 포트 포워드

XRP Ledger P2P 네트워크의 서버는 피어 프로토콜을 통해 통신합니다. 보안과 나머지 네트워크와의 연결성을 최적으로 조합하려면 방화벽을 사용해 대부분의 포트로부터 서버를 보호하되 피어 프로토콜 포트를 열거나 포워딩해야 합니다.

rippled 서버가 실행되는 동안 server\_info 메소드를 실행하여 보유한 피어 수를 확인할 수 있습니다. 정보 객체의 피어 필드에는 현재 서버에 연결된 피어 수가 표시됩니다. 이 숫자가 정확히 10 또는 11이면 일반적으로 방화벽이 들어오는 연결을 차단하고 있다는 뜻입니다.

방화벽이 들어오는 피어 연결을 차단하고 있기 때문에 10개의 피어만 표시되는 server\_info 결과의 예입니다:

```
$ ./rippled server_info
Loading: "/etc/opt/ripple/rippled.cfg"
2019-Dec-23 22:15:09.343961928 HTTPClient:NFO Connecting to 127.0.0.1:5005

{
   "result" : {
      "info" : {
         ... (trimmed) ...
         "load_factor" : 1,
         "peer_disconnects" : "0",
         "peer_disconnects_resources" : "0",
         "peers" : 10,
         "pubkey_node" : "n9KUjqxCr5FKThSNXdzb7oqN8rYwScB2dUnNqxQxbEA17JkaWy5x",
         "pubkey_validator" : "n9KM73uq5BM3Fc6cxG3k5TruvbLc8Ffq17JZBmWC4uP4csL4rFST",
         "published_ledger" : "none",
         "server_state" : "connected",
         ... (trimmed) ...
      },
      "status" : "success"
   }
}
```

들어오는 연결을 허용하려면 기본 구성 파일의 포트 51235에서 제공되는 피어 프로토콜 포트에서 들어오는 트래픽을 허용하도록 방화벽을 구성하세요. 포트를 여는 방법은 방화벽에 따라 다릅니다. 서버가 NAT(네트워크 주소 변환)를 수행하는 라우터 뒤에 있는 경우 포트를 서버로 전달하도록 라우터를 구성해야 합니다.

Red Hat Enterprise Linux에서 firewalld 소프트웨어 방화벽을 사용하는 경우, firewall-cmd 도구를 사용하여 들어오는 모든 트래픽에 대해 포트 51235를 열 수 있습니다.

_`--zone=public`_이 공용 영역이라고 가정합니다.

```
$ sudo firewall-cmd --zone=public --add-port=51235/tcp
```

그런 다음 rippled 서버를 다시 시작합니다:

```
$ sudo systemctl restart rippled.service
```

영구적으로 설정하려면:

```
$ sudo firewall-cmd --zone=public --permanent --add-port=51235/tcp
```

기타 소프트웨어 및 하드웨어 방화벽에 대해서는 해당 제조업체의 공식 설명서를 참조하세요.

가상 방화벽이 있는 호스팅 서비스(예: AWS 보안 그룹 )를 사용하는 경우에는 firewalld를 사용할 필요는 없지만 피어 포트에서 개방형 인터넷의 인바운드 트래픽을 허용해야 합니다. 호스트 또는 가상 머신에 관련 규칙을 적용해야 합니다.
