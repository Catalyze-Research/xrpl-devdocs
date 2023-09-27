# stand-alone 모드에서 새 제네시스 ledger 시작하기

stand-alone 모드에서는 rippled가 새로운 제네시스 ledger를 생성하도록 할 수 있습니다. 이렇게 하면 프로덕션 XRP Ledger의 ledger 기록이 없는 알려진 상태를 제공합니다. (이는 무엇보다도 단위 테스트에 매우 유용합니다.)

* 새 제네시스 ledger로 stand-alone 모드에서 rippled를 시작하려면 -a 및 --start 옵션을 사용하세요:

```
rippled -a --start --conf=/path/to/rippled.cfg
```

stand-alone 모드에서 rippled를 시작할 때 사용할 수 있는 옵션에 대한 자세한 내용은 커맨드라인인 사용법을 참조하세요: stand-alone 모드 옵션을 참고하세요.

제네시스 ledger에서 제네시스 주소는 1,000억 개의 rippled를 모두 보유합니다. 제네시스 주소의 키는 다음과 같이 하드코딩되어 있습니다:

**주소:** rHb9CJAWyB4rj91VRWn96DkukG4bwdtyTh

**비밀:** snoPBrXtMeMyMHUVTgbuqAfg1SUTb("마스터 패스프레이즈")

## 새 제네시스 ledger의 설정

새로운 제네시스 ledger에서 하드코딩된 기본 [Reserve](https://xrpl.org/reserves.html) 새 주소에 자금을 조달하기 위한 최소 200 XRP이며, ledger의 객체당 50 XRP씩 증가합니다. 이 값은 프로덕션 네트워크의 현재 reserve requirements보다 높습니다. (참고: 수수료 투표)

기본적으로 새 제네시스 ledger는 수정이 활성화되어 있지 않습니다. --start로 새 제네시스 ledger를 시작하면, 제네시스 ledger에는 구성 파일에서 명시적으로 비활성화한 수정 사항을 제외하고 rippled 서버에서 기본적으로 지원하는 모든 수정 사항을 켜는 EnableAmendment 의사 트랜잭션이 포함됩니다. 이러한 수정의 효과는 바로 다음 ledger 버전부터 사용할 수 있습니다. (참고: stand-alone 모드에서는 ledger를 수동으로 진행해야 합니다.) [![New in: rippled 0.50.0](https://img.shields.io/badge/New%20in-rippled%200.50.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/0.50.0)
