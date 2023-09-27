# nft\_buy\_offers

nft\_buy\_offers 메소드는 주어진 NFToken 객체에 대한 구매 오퍼 목록을 반환합니다.

_(NonFungibleTokenV1\_1 수정에 의해 추가되었습니다.)_

## 요청 형식

요청 형식의 예시입니다:

{% hint style="info" %}
Note:

이 메소드에 대한 커맨드라인 구문은 없습니다. 대신 json 메소드를 사용하여 커맨드라인에서 이 메소드에 액세스할 수 있습니다.
{% endhint %}

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "command": "nft_buy_offers",
  "nft_id": "00090000D0B007439B080E9B05BF62403911301A7B1F0CFAA048C0A200000007",
  "ledger_index": "validated"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
  "method": "nft_buy_offers",
  "params": [{
    "nft_id": "00090000D0B007439B080E9B05BF62403911301A7B1F0CFAA048C0A200000007",
    "ledger_index": "validated"
  }]
}
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| 필드             | 유형        | 설명                                                                                                                                                                                |
| -------------- | --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `nft_id`       | 문자열       | [NFToken](https://xrpl.org/nftoken.html) 객체의 고유 식별자입니다.                                                                                                                           |
| `ledger_hash`  | 문자열       | (선택 사항) 사용할 원장 버전에 대한 20바이트 16진수 문자열입니다. ([원장 지정](https://xrpl.org/basic-data-types.html#specifying-ledgers) 참조)                                                                  |
| `ledger_index` | 문자열 또는 숫자 | (선택 사항) 사용할 원장의 [원장 인덱스](https://xrpl.org/basic-data-types.html#ledger-index) 또는 자동으로 원장을 선택하는 단축 문자열입니다. ([원장 지정](https://xrpl.org/basic-data-types.html#specifying-ledgers) 참조) |
| `limit`        | 정수        | (선택 사항) 검색할 NFT 구매 제안 수를 제한합니다. 이 값은 50보다 작거나 500보다 클 수 없습니다. 이 범위를 벗어나는 양수 값은 가장 가까운 유효한 옵션으로 대체됩니다. 기본값은 250입니다.                                                                |
| `marker`       | marker    | (선택 사항) 페이지를 매긴 이전 응답의 값입니다. 해당 응답이 중단된 데이터 검색을 재개합니다.                                                                                                                            |

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "result": {
    "nft_id": "00090000D0B007439B080E9B05BF62403911301A7B1F0CFAA048C0A200000007",
    "offers": [
      {
        "amount": "1500",
        "flags": 0,
        "nft_offer_index": "3212D26DB00031889D4EF7D9129BB0FA673B5B40B1759564486C0F0946BA203F",
        "owner": "rsuHaTvJh1bDmDoxX9QcKP7HEBSBt4XsHx"
      }
    ]
  },
  "status": "success",
  "type": "response"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
  "result": {
    "nft_id": "00090000D0B007439B080E9B05BF62403911301A7B1F0CFAA048C0A200000007",
    "offers": [
      {
        "amount": "1500",
        "flags": 0,
        "nft_offer_index": "3212D26DB00031889D4EF7D9129BB0FA673B5B40B1759564486C0F0946BA203F",
        "owner": "rsuHaTvJh1bDmDoxX9QcKP7HEBSBt4XsHx"
      }
    ],
    "status": "success"
  }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`  | 유형     | 설명                                                                                                        |
| -------- | ------ | --------------------------------------------------------------------------------------------------------- |
| `nft_id` | 문자열    | 요청에 지정된 대로 이러한 제안이 제공되는 NFToken입니다.                                                                       |
| `offers` | 배여ㄹ    | 토큰에 대한 구매 제안 목록입니다. 이들 각각은 **구매 제안** 형식으로 되어 있습니다 (아래 참조).                                                |
| `limit`  | 숫자     | (생략 가능)`limit` 요청서에 명시된 대로 입니다.                                                                           |
| `marker` | marker | (생략 가능) 응답이 페이지로 매겨졌음을 나타내는 서버 정의 값입니다. 이 통화가 중단된 곳에서 재개하려면 이를 다음 통화에 전달하세요. 이 페이지 이후에 정보 페이지가 없으면 생략됩니다. |

## 오퍼 구매

오퍼 배열의 각 멤버는 해당 NFT를 구매할 수 있는 하나의 NFTokenOffer 객체를 나타내며, 다음과 같은 필드를 가집니다:

| `Field`           | 유형        | 설명                                                                                           |
| ----------------- | --------- | -------------------------------------------------------------------------------------------- |
| `amount`          | 문자열 또는 객체 | NFT를 구매하기 위해 제안한 금액으로, XRP 드롭 단위의 금액을 나타내는 문자열 또는 대체 가능한 토큰의 금액을 나타내는 객체입니다. (통화 금액 지정하기 참조) |
| `flags`           | 숫자        | 이 오퍼에 대한 비트 플래그 집합입니다. 가능한 값은 NFTokenOffer 플래그를 참조하세요.                                       |
| `nft_offer_index` | 문자열       | 이 제안의 원장 [개체 ID입니다.](https://xrpl.org/ledger-object-ids.html)                                |
| `owner`           | 문자열       | 이 제안을 제출한 계정입니다.                                                                             |

## 발생 가능한 오류

* 모든 범용 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* actNotFound - 요청의 계정 필드에 지정된 주소가 장부에 있는 계정과 일치하지 않습니다.
* lgrNotFound - ledger\_hash 또는 ledger\_index로 지정한 ledger이 존재하지 않거나, 존재하지만 서버에 없습니다.
