# 클러스터링(Clustering)

\
클러스터링 하나의 데이터 센터에서 여러 개의 <mark style="background-color:yellow;">rippled</mark> 서버를 실행 중이라면 이러한 서버를 클러스터로 구성하여 효율성을 극대화할 수 있습니다. <mark style="background-color:yellow;">rippled</mark> 서버를 클러스터로 실행하면 다음과 같은 이점을 얻을 수 있습니다:

* 클러스터된 <mark style="background-color:yellow;">rippled</mark> 서버는 암호화 작업을 공유합니다. 하나의 서버가 메시지의 신뢰성을 검증한 경우 클러스터의 다른 서버는 해당 서버를 신뢰하고 재검증하지 않습니다.
* 클러스터된 서버는 네트워크를 오용하거나 악용하는 피어 및 API 클라이언트에 대한 정보를 공유합니다. 이를 통해 클러스터의 모든 서버를 동시에 공격하는 것을 어렵게 만듭니다.
* 클러스터된 서버는 일부 서버에서 현재 부하 기반 트랜잭션 수수료 요구 사항을 충족시키지 못하는 경우에도 트랜잭션을 클러스터 전체로 전파합니다.

검증인을 [개인 피어](peer-protocol.md)로 실행하고 있다면 Ripple은 <mark style="background-color:yellow;">rippled</mark> 서버 클러스터를 프록시 서버로 사용하는 것을 권장합니다.



### 참고 <a href="#see-also" id="see-also"></a>

* **Tutorials:**
  * [Cluster `rippled` Servers](https://xrpl.org/cluster-rippled-servers.html)
  * [Run rippled as a Validator](https://xrpl.org/run-rippled-as-a-validator.html)
* **References:**
  * [peers method](https://xrpl.org/peers.html)
  * [connect method](https://xrpl.org/connect.html)
  * [Peer Crawler](https://xrpl.org/peer-crawler.html)
