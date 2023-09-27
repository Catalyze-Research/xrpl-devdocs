# NegativeUNL

ㅐ_(NegativeUNL 수정안에 의해 추가되었습니다.)_

NegativeUNL 객체 유형에는 현재 오프라인 상태인 것으로 추정되는 신뢰할 수 있는 검증인 목록인 부정적 UNL의 현재 상태가 포함됩니다.

각 ledger 버전에는 최대 하나의 부정적 UNL 객체가 포함됩니다. 현재 비활성화되었거나 비활성화될 예정인 유효성 검사기가 없는 경우, ledger에 NegativeUNL 객체가 없습니다.

## NegativeUNL JSON 예시

```
{
  "DisabledValidators": [
    {
      "DisabledValidator": {
        "FirstLedgerSequence": 1609728,
        "PublicKey": "ED6629D456285AE3613B285F65BBFF168D695BA3921F309949AFCD2CA7AFEC16FE"
      }
    }
  ],
  "Flags": 0,
  "LedgerEntryType": "NegativeUNL",
  "index": "2E8A59AA9D3B5B186B0B9E0F62E6C02587CA74A4D778938E957B6357D364B244"
}
```

NegativeUNL 객체에는 다음과 같은 필드가 있습니다:

| 이름                    | JSON 유형 | 내부 유형  | 필수 여부 | 설명                                                                       |
| --------------------- | ------- | ------ | ----- | ------------------------------------------------------------------------ |
| `DisabledValidators`  | 정렬      | 정렬     | 아니요   | 현재 비활성화된 신뢰할 수 있는 유효성 검사기를 나타내는 DisabledValidator 객체 목록(아래 참조)입니다.       |
| `Flags`               | 숫자      | UInt32 | 예     | Boolean 플래그의 비트맵입니다. NegativeUNL 객체 유형에는 플래그가 정의되어 있지 않으므로 이 값은 항상 0입니다. |
| `LedgerEntryType`     | 문자열     | UInt16 | 예     | 0x004E 값은 NegativeUNL 문자열에 매핑되며, 이 개체가 부정적 UNL임을 나타냅니다.                  |
| `ValidatorToDisable`  | 문자열     | Blob   | 아니요   | 다음 플래그 ledger에서 비활성화할 예정인 신뢰할 수 있는 유효성 검사기의 공개 키입니다.                     |
| `ValidatorToReEnable` | 문자열     | Blob   | 아니요   | 다음 플래그 ledger에서 다시 활성화될 예정인 Negative UNL에 있는 신뢰할 수 있는 유효성 검사기의 공개 키입니다.  |

## DisabledValidator 객체

각 DisabledValidator 객체는 비활성화된 유효성 검사기 하나를 나타냅니다. JSON에서 DisabledValidator 객체에는 DisabledValidator라는 필드가 하나 있으며, 이 필드에는 다음 필드가 있는 다른 객체가 포함됩니다:

| 이름                    | JSON 유형 | 내부 유형  | 설명                                         |
| --------------------- | ------- | ------ | ------------------------------------------ |
| `FirstLedgerSequence` | 숫자      | UInt32 | 검증자가 Negative UNL에 추가되었을 때의 ledger 인덱스입니다. |
| `PublicKey`           | 문자열     | Blob   | 유효성 검사기의 마스터 공개 키(16진수)입니다.                |

## NegativeUNL ID 형식

NegativeUNL 객체 ID는 NegativeUNL 스페이스 키(0x004E)의 해시일 뿐입니다. 즉, ledger에 있는 NegativeUNL 객체의 ID는 항상 0입니다:

```
2E8A59AA9D3B5B186B0B9E0F62E6C02587CA74A4D778938E957B6357D364B244
```
