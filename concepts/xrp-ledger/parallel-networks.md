# 병렬 네트워크(Parallel Networks)

XRP Ledger에는 하나의 본래 네트워크(메인넷)가 있으며, XRP Ledger에서 발생하는 모든 업무는 본래 네트워크인 Mainnet에서 처리됩니다.

XRP Ledger 커뮤니티 구성원이 mainnet에 영향을 미치지 않고 XRP Ledger 기술과 상호작용할 수 있도록 대안 네트워크인 "알트넷"이 제공됩니다. 아래는 일부 공개 알트넷에 대한 설명입니다:

| 네트워크                                                         | 케이던스 업그레이드                                                                                 | 설명                                                                                                                                                                                                                            |
| ------------------------------------------------------------ | ------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Mainnet                                                      | Stable releases                                                                            | 안정 버전의 XRP Ledger로, XRP의 홈인 분산 암호화된 ledger가 동작하는 네트워크입니다.                                                                                                                                                                     |
| Testnet                                                      | Stable releases                                                                            | 안정 버전의 XRP Ledger로, 실제 XRP Ledger 사용자에게 영향을 주지 않고 XRP Ledger 기반 소프트웨어를 테스트하는 용도의 "대안 우주" 네트워크입니다. Testnet의 개정 상태는 Mainnet과 유사하게 유지되지만, 탈중앙화된 시스템의 예측 불가능한 특성으로 인해 약간의 타이밍 차이가 발생할 수 있습니다.                                     |
| Devnet                                                       | Beta releases                                                                              | 베타 버전의 XRP Ledger로, 핵심 XRP Ledger 소프트웨어의 불안정한 변경 사항을 테스트할 수 있는 미리보기 네트워크입니다. 개발자들은 이 알트넷을 통해 계획 중인 새로운 XRP Ledger 기능과 개정 사항에 대해 상호작용하고 학습할 수 있습니다. 이러한 변경 사항은 아직 Mainnet에서 활성화되지 않았습니다.                                       |
| [Hooks Testnet V2 ](https://hooks-testnet-v2.xrpl-labs.com/) | [Hooks server ](https://github.com/XRPL-Labs/xrpld-hooks)                                  | Hooks를 사용한 체인 상 스마트 계약 기능을 미리보기하는 네트워크입니다.                                                                                                                                                                                    |
| AMM-Devnet                                                   | [XLS-30d pre-release ](https://github.com/gregtatcam/rippled/tree/amm-core-functionality/) | XRP Ledger의 자동화된 시장 메이커를 위한 XLS-30d 표준 미리보기 네트워크입니다. 테스트용 바이너리 빌드도 제공됩니다. 라이브러리 지원: [xrpl.js 2.6.0-beta.0 ](https://www.npmjs.com/package/xrpl/v/2.6.0-beta.0), [xrpl-py 1.8.0b0 ](https://pypi.org/project/xrpl-py/1.8.0b0/) |

각 알트넷은 자체적인 테스트용 XRP를 제공하며, 이는 XRP Ledger와 애플리케이션 및 통합을 실험하고 개발하는 데 관심 있는 당사자들에게 무료로 제공됩니다. 테스트용 XRP는 실제 가치가 없으며, 네트워크가 재설정되면 소실됩니다.

{% hint style="warning" %}
Caution:

XRP Ledger mainnet과는 달리, 테스트 네트워크는 일반적으로 _중앙집중화_되어 있으며 안정성과 가용성에 대한 보장이 없습니다. 이러한 네트워크는 서버 구성, 네트워크 토폴로지 및 네트워크 성능의 다양성을 테스트하기 위해 사용되었고 계속해서 사용되고 있습니다.
{% endhint %}

## 병렬 네트워크와 컨센서스(Parallel Networks and Consensus)

서버가 어떤 네트워크를 따르는지 결정하는 주요 요소는 구성된 UNL, 즉 담합하지 않을 것으로 신뢰하는 검증인 목록입니다. 각 서버는 구성된 UNL을 사용해 어떤 ledger 진실로 받아들일지 결정합니다. <mark style="background-color:yellow;">rippled</mark> 인스턴스의 서로 다른 합의 그룹이 같은 그룹의 다른 구성원만 신뢰하는 경우, 각 그룹은 병렬 네트워크로 계속 유지됩니다. 악의적이거나 오작동하는 컴퓨터가 두 네트워크에 모두 연결되더라도 각 네트워크의 구성원이 쿼럼 설정을 초과하여 다른 네트워크의 구성원을 신뢰하도록 구성되지 않는 한 컨센서스 프로세스는 혼란을 피합니다.

Ripple은 testnet과 devnet에서 메인 서버를 실행하며, 사용자는 [자신의 <mark style="background-color:yellow;">rippled</mark> 서버를 이러한 네트워크에 연결](https://xrpl.org/connect-your-rippled-to-the-xrp-test-net.html)할 수도 있습니다. 테스트넷과 개발넷은 검열에 저항하는 다양한 검증자 세트를 사용하지 않습니다. 따라서 Ripple은 언제든지 testnet이나 devnet을 재설정할 수 있습니다.



### 참고 <a href="#see-also" id="see-also"></a>

* **Tools:**
  * [XRP Testnet Faucet](https://xrpl.org/xrp-test-net-faucet.html)
* **Concepts:**
  * [Consensus](https://xrpl.org/consensus.html)
  * [Amendments](https://xrpl.org/amendments.html)
* **Tutorials:**
  * [Connect Your `rippled` to the XRP Testnet](https://xrpl.org/connect-your-rippled-to-the-xrp-test-net.html)
  * [Use rippled in Stand-Alone Mode](https://xrpl.org/use-stand-alone-mode.html)
* **References:**
  * [server\_info method](https://xrpl.org/server\_info.html)
  * [consensus\_info method](https://xrpl.org/consensus\_info.html)
  * [validator\_list\_sites method](https://xrpl.org/validator\_list\_sites.html)
  * [validators method](https://xrpl.org/validators.html)
  * [Daemon Mode Options](https://xrpl.org/commandline-usage.html#daemon-mode-options)
