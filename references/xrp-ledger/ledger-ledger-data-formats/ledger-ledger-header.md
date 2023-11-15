# Ledger 헤더(Ledger Header)

모든 ledger 버전에는 내용을 설명하는 고유한 헤더가 있습니다. ledger 메소드를 사용하여 ledger의 헤더 정보를 조회할 수 있습니다. ledger 헤더의 내용은 다음과 같습니다:

| 필드                      | JSON 유형 | 내부 유형   | 설명                                                                                                                       |
| ----------------------- | ------- | ------- | ------------------------------------------------------------------------------------------------------------------------ |
| `ledger_index`          | 문자열     | UInt32  | ledger의 ledger 인덱스입니다. 일부 API 메소드는 이것을 인용된 정수로 표시합니다. 일부는 기본 JSON 번호로 표시합니다.                                             |
| `ledger_hash`           | 문자열     | 해시256   | 이 ledger 버전의 SHA -512Half입니다. 이것은 이 ledger와 모든 내용에 대한 고유 식별자 역할을 합니다.                                                    |
| `account_hash`          | 문자열     | 해시256   | SHA-512는 ledger의 상태 트리 정보 절반입니다.                                                                                         |
| `close_time`            | 숫자      | Uint32  | 2000-01-01 00:00:00의 Ripple 에포크 이후 초 단위로 이 ledger 버전이 닫힌 대략적인 시간입니다. 이 값은 `close_time_resolution`를 기준으로 반올림됩니다.          |
| `closed`                | boolean | boolean | `true`인 경우 이 ledger 버전은 더 이상 새 거래를 수락하지 않습니다. (단, 이 ledger 버전의 유효성이 검증되지 않으면 다른 거래 집합을 가진 다른 ledger 버전으로 대체될 수 있습니다.)    |
| `parent_hash`           | 문자열     | 해시256   | 이 ledger의 직접 이전 버전인 이전 원장 버전의 ledger\_hash 값입니다. 이전 ledger 인덱스의 버전이 다른 경우, 이는 ledger이 어느 버전에서 파생되었는지를 나타냅니다.             |
| `total_coins`           | 문자열     | UInt64  | ledger의 계정이 소유한 XRP의 총 드롭 수입니다 . 이것은 거래 수수료로 소실된 XRP를 생략합니다. 일부 계정은 누구에게도 키가 알려지지 않은 "블랙홀"이기 때문에 실제 유통되는 XRP의 양은 더 적습니다. |
| `transaction_hash`      | 문자열     | 해시256   | SHA-512가 ledger에 포함된 거래의 절반입니다.                                                                                          |
| `close_time_resolution` | 숫자      | Uint8   | 반올림할 수 있는 최대 `close_time`시간(초)을 나타내는 \[2,120] 범위의 정수입니다.                                                                 |
| `closeFlags`            | (생략)    | Uint8   | 이 ledger의 마감과 관련된 플래그의 비트맵입니다.                                                                                           |

## Ledger 인덱스(Ledger Index)

ledger 인덱스는 ledger을 식별하는 데 사용되는 32비트 부호 없는 정수입니다. ledger 인덱스는 ledger의 시퀀스 번호라고도 합니다. (이는 계정 시퀀스와는 다릅니다.) 첫 번째 ledger은 ledger 인덱스 1이며, 각각의 새 ledger은 바로 앞 ledger의 ledger 인덱스보다 1 높은 ledger 인덱스를 갖습니다.

ledger 인덱스는 ledger의 순서를 나타내며, 해시 값은 ledger의 정확한 내용을 식별합니다. 동일한 해시를 가진 두 개의 ledger은 항상 동일합니다. 검증된 ledger의 경우 해시값과 ledger 인덱스는 동일하게 유효하며 1:1의 상관관계를 갖습니다. 그러나 진행 중인 ledger의 경우 그렇지 않습니다:

* 네트워크 전체에 트랜잭션을 전파하는 데 걸리는 지연 시간으로 인해 두 개의 서로 다른 rippled 서버는 동일한 ledger 인덱스를 가진 현재 ledger에 대해 서로 다른 내용을 가질 수 있습니다.
* 컨센서스를 통해 유효성을 검증받기 위해 경쟁하는 여러 개의 폐쇄 ledger 버전이 있을 수 있습니다. 이러한 ledger 버전은 ledger 인덱스는 같지만 내용이 다르고 해시가 다릅니다. 이러한 폐쇄 ledger 중 하나만 검증될 수 있습니다.
* 현재 오픈 ledger의 해시는 계산되지 않습니다. 이는 현재 ledger의 내용이 시간이 지남에 따라 변경되어 ledger 인덱스가 동일하게 유지되더라도 해시가 변경될 수 있기 때문입니다. ledger의 해시는 ledger이 닫힐 때만 계산됩니다.

## 플래그 닫기(Close Flags)

ledger closeFlags에 대해 정의된 플래그가 하나뿐입니다: `sLCF_NoConsensusTime`(값 `1`). 이 플래그가 활성화된 경우, 검증인들이 ledger에 대해 서로 다른 마감 시간을 가졌지만 그 외에는 동일한 ledger을 만들었으므로, 마감 시간에 대해 "동의하지 않기로 동의"하면서 합의를 선언했음을 의미합니다. 이 경우, ledger의 공식 `close_time` 값은 부모 ledger의 1초 후입니다.

`closeFlags` 필드는 ledger의 JSON 표현에는 포함되지 않지만, ledger의 바이너리 표현에는 포함되며 ledger의 해시를 결정하는 필드 중 하나입니다.

### 참고(See Also) <a href="#see-also" id="see-also"></a>

분산 원장의 기초에 대해서는 [Ledgers](../../../concepts/undefined-1/ledgers.md)를 참고하세요.

&#x20;
