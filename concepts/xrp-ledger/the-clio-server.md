# 클리오 서버(The Clio Server)

클리오는 검증된 ledger 데이터에 대한 WebSocket 또는 HTTP API 호출을 위해 최적화된 XRP Ledger API 서버입니다.

클리오 서버는 P2P 네트워크에 연결하지 않습니다. 대신, P2P 네트워크에 연결된 지정된 <mark style="background-color:yellow;">rippled</mark> 서버에서 데이터를 추출합니다. API 호출을 효율적으로 처리함으로써, 클리오 서버는 P2P 모드로 실행되는 <mark style="background-color:yellow;">rippled</mark> 서버의 부하를 줄이는 데 도움이 됩니다.

클리오는 검증된 히스토리 ledger와 트랜잭션 데이터를 공간 효율적인 형식으로 저장하며, <mark style="background-color:yellow;">rippled</mark>보다 최대 4배 적은 공간을 사용합니다. 클리오는 Cassandra 또는 ScyllaDB를 사용하여 확장 가능한 읽기 처리량을 제공합니다. 여러 클리오 서버는 동일한 데이터 세트에 대한 액세스를 공유할 수 있으므로, 중복 데이터 저장 또는 계산이 필요하지 않은 고가용성 클리오 서버 클러스터를 구축할 수 있습니다.

클리오는 클리오와 동일한 컴퓨터에서 실행되거나 별도로 실행되는 <mark style="background-color:yellow;">rippled</mark> 서버에 대한 액세스가 필요합니다.

클리오는 완전한 [HTTP / WebSocket APIs](https://xrpl.org/http-websocket-apis.html)를 제공하지만, 기본적으로 유효 데이터만 반환합니다. P2P 네트워크에 액세스가 필요한 요청의 경우, 클리오는 자동으로 P2P 네트워크의 <mark style="background-color:yellow;">rippled</mark> 서버로 요청을 전달하고 응답을 반환합니다.

## 클리오 서버를 실행해야 하는 이유는 무엇인가요? (Why Should I Run a Clio Server?)

클리오 서버를 자체 실행하는 이유는 여러 가지가 있을 수 있지만, 대부분 다음과 같이 요약할 수 있습니다: P2P 네트워크에 연결된 <mark style="background-color:yellow;">rippled</mark> 서버의 부하 감소, 낮은 메모리 사용량 및 저장 공간 오버헤드, 쉬운 수평 확장 및 API 요청에 대한 높은 처리량입니다.

* <mark style="background-color:yellow;">rippled</mark> 서버의 부하 감소 - 클리오 서버는 P2P 네트워크에 연결하지 않습니다. 대신, P2P 네트워크에 연결된 하나 이상의 신뢰할 수 있는 <mark style="background-color:yellow;">rippled</mark> 서버에서 검증된 데이터를 가져오기 위해 gRPC를 사용합니다. 따라서 클리오 서버는 요청을 더 효율적으로 처리하고, P2P 모드로 실행되는 <mark style="background-color:yellow;">rippled</mark> 서버의 부하를 줄입니다.
* 낮은 메모리 사용량 및 저장 공간 오버헤드 - 클리오는 Cassandra를 데이터베이스로 사용하며, 데이터를 공간 효율적인 형식으로 저장하여 <mark style="background-color:yellow;">rippled</mark>에 비해 최대 4배 적은 공간을 사용합니다.
* 쉬운 수평 확장 - 여러 클리오 서버가 동일한 데이터 세트에 액세스를 공유할 수 있도록 함으로써, 고가용성 클리오 서버 클러스터를 구축할 수 있습니다.
* API 요청에 대한 높은 처리량 - 클리오 서버는 하나 이상의 신뢰할 수 있는 <mark style="background-color:yellow;">rippled</mark> 서버에서 검증된 데이터를 추출하고 이 데이터를 효율적으로 저장합니다. 따라서 API 호출을 효율적으로 처리하여 높은 처리량과 때로는 낮은 지연시간을 제공합니다.

## 클리오 서버는 어떻게 작동하나요?(How does a Clio Server Work?)

<figure><img src="https://xrpl.org/img/clio-basic-architecture.svg" alt="" width="563"><figcaption></figcaption></figure>

클리오 서버는 트랜잭션 메타데이터, 계정 상태, ledger 헤더와 같은 검증된 ledger 데이터를 영속적인 데이터 스토어에 저장합니다.

클리오 서버가 API 요청을 받으면 이러한 데이터 스토어에서 데이터를 조회합니다. P2P 네트워크에서 데이터가 필요한 요청의 경우, 클리오 서버는 P2P 서버로 요청을 전달하고 응답을 클라이언트에게 반환합니다.

### 참고 <a href="#see-also" id="see-also"></a>

* [Clio source code ](https://github.com/XRPLF/clio)
* **Tutorials:**
  * [Install Clio server on Ubuntu](https://xrpl.org/install-clio-on-ubuntu.html)

&#x20;
