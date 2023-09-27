# NFTokenPage

NFTokenPage 오브젝트는 동일한 계정에서 소유한 NFToken 오브젝트의 컬렉션을 나타냅니다. 계정에는 이중으로 연결된 목록을 형성하는 여러 개의 NFTokenPage ledger 객체가 있을 수 있습니다.&#x20;

_(NonFungibleTokenV1\_1 수정안에 의해 추가되었습니다.)_

## NFTokenPage JSON 예시

```
{
  "LedgerEntryType": "NFTokenPage",
  "PreviousPageMin":
    "8A244DD75DAF4AC1EEF7D99253A7B83D2297818B2297818B70E264D2000002F2",
  "NextPageMin":
    "8A244DD75DAF4AC1EEF7D99253A7B83D2297818B2297818BE223B0AE0000010B",
  "PreviousTxnID":
    "95C8761B22894E328646F7A70035E9DFBECC90EDD83E43B7B973F626D21A0822",
  "PreviousTxnLgrSeq":
    42891441,
  "NFTokens": [
    {
      "NFToken": {
        "NFTokenID":
          "000B013A95F14B0044F78A264E41713C64B5F89242540EE208C3098E00000D65",
        "URI": "697066733A2F2F62616679626569676479727A74357366703775646D37687537367568377932366E6634646675796C71616266336F636C67747179353566627A6469"
      }
    },
    /* potentially more objects */
  ]
}
```

페이지 크기를 최소화하고 저장 공간을 최적화하기 위해 소유자 필드는 오브젝트의 ledger 식별자의 일부로 인코딩되므로 존재하지 않습니다.

## NFTokenPage 필드

NFTokenPage 오브젝트는 다음과 같은 필수 및 선택 필드를 가질 수 있습니다:

| 필드 이름               | JSON 유형 | 내부 유형  | 필수 여부 | 설명                                                                                                                                             |
| ------------------- | ------- | ------ | ----- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| `LedgerEntryType`   | 문자열     | UInt16 | 예     | 0x0050 값은 NFTokenPage 문자열에 매핑되며, 이 페이지가 NFToken 객체가 포함된 페이지임을 나타냅니다.                                                                           |
| `NextPageMin`       | 문자열     | 해시256  | 아니요   | 다음 페이지의 로케이터(있는 경우)입니다. 이 필드와 사용 방법에 대한 자세한 내용은 아래에 설명되어 있습니다.                                                                                 |
| `NFTokens`          | 배열      | 배열     | 예     | 이 NFTokenPage 오브젝트에 포함된 NFToken 오브젝트의 컬렉션입니다. 이 사양은 페이지당 32개의 NFToken 오브젝트 상한을 설정합니다. 오브젝트는 정렬 파라미터로 사용된 NFTokenID를 사용하여 낮은 순서부터 높은 순서로 정렬됩니다. |
| `PreviousPageMin`   | 문자열     | 해시256  | 아니요   | 이전 페이지의 로케이터(있는 경우)입니다. 이 필드에 대한 자세한 내용과 사용 방법은 아래에 설명되어 있습니다.                                                                                 |
| `PreviousTxnID`     | 문자열     | 해시256  | 아니요   | 이 NFTokenPage 오브젝트를 가장 최근에 수정한 트랜잭션의 트랜잭션 ID를 식별합니다.                                                                                           |
| `PreviousTxnLgrSeq` | 숫자      | UInt32 | 아니요   | 이 NFTokenPage 오브젝트를 가장 최근에 수정한 트랜잭션이 포함된 ledger의 시퀀스입니다.                                                                                       |

## NFTokenPage ID 형식

NFTokenPage 식별자는 보다 효율적인 페이징 구조를 허용하도록 구성되며, NFToken 오브젝트에 이상적으로 적합합니다.

NFTokenPage의 식별자는 페이지 소유자의 160비트 AccountID를 연결하고, 그 뒤에 특정 NFTokenID가 이 페이지에 포함될 수 있는지 여부를 나타내는 96비트 값을 연결하여 파생됩니다.

좀 더 구체적으로 설명하자면, NFTokenID 값이 A인 NFToken은 low96(A) >= low96(B)인 경우에만 NFTokenPage ID가 B인 페이지에 포함될 수 있습니다.

여기에는 256비트 값의 하위 96비트를 반환하는 low96(x) 함수가 사용됩니다. 예를 들어, 000B013A95F14B0044F78A264E41713C64B5F89242540EE208C3098E00000D65의 NFTokenID에 low96 함수를 적용하면 42540EE208C3098E00000D65라는 값이 반환됩니다.

이러한 설계를 통해 목록에서 각 NFTokenPage를 확인할 필요 없이 개별 NFToken 객체를 효율적으로 조회할 수 있습니다.

## NFToken 찾기

특정 NFToken을 찾으려면 해당 NFTokenID와 현재 소유자를 알아야 합니다. 위에서 설명한 대로 NFTokenPage ID를 계산합니다. 식별자가 이 값보다 작거나 같은 ledger 항목을 검색합니다. 해당 항목이 존재하지 않거나 NFTokenPage가 아닌 경우, 해당 계정은 해당 NFToken을 소유하지 않습니다.

## NFToken 추가하기

NFToken을 추가하려면 NFToken 객체를 검색할 때와 동일한 기법을 사용하여 해당 토큰이 있어야 할 NFTokenPage를 찾아 해당 페이지에 추가합니다. NFTokenPage가 이미 꽉 찬 경우 이전 페이지와 다음 페이지(있는 경우)를 찾아 세 페이지의 균형을 맞추고 필요에 따라 새 NFTokenPage를 삽입합니다.

## NFToken 제거하기

NFToken 오브젝트를 제거하는 것은 추가하는 것과 같습니다. 페이지의 NFToken 객체 수가 특정 임계값 아래로 내려가면 ledger는 가능한 경우 페이지를 이전 또는 다음 페이지와 결합합니다.

## NFTokenPage 객체에 대한 reserve

각 NFTokenPage는 계정 소유자 reserve에 하나의 항목으로 계산됩니다. 각 페이지는 최대 32개의 NFToken 항목을 보유할 수 있으므로, NFToken당 유효 reserve 비용은 R/32만큼 낮을 수 있으며, 여기서 R은 증분 reserve입니다.

## 실제 reserve

페이지 분할과 결합이 작동하는 방식 때문에 페이지당 실제 NFToken 객체 수는 다소 예측할 수 없으며, 관련된 실제 NFTokenID 값에 따라 달라집니다. 실제로는 많은 수의 NFToken 객체를 발행하거나 수신한 후 각 페이지에 적게는 16개, 많게는 32개의 NFToken 객체가 있을 수 있으며, 일반적인 경우 페이지당 약 24개의 NFToken 객체가 있습니다.

현재 항목당 보유량은 2XRP입니다. 아래 표는 다양한 시나리오에서 소유한 다양한 수의 NFToken 객체에 대한 총 소유자 준비금이 얼마인지 보여줍니다:

| 소유한 `NFTokens`  | 우수 사례  | 일반     | 최악의 경우  |
| --------------- | ------ | ------ | ------- |
| 32 or fewer     | 2 XRP  | 2 XRP  | 2 XRP   |
| 50              | 4 XRP  | 6 XRP  | 8 XRP   |
| 200             | 14 XRP | 18 XRP | 26 XRP  |
| 1000            | 64 XRP | 84 XRP | 126 XRP |

이 수치는 추정치이며 실제 수치는 다를 수 있습니다.
