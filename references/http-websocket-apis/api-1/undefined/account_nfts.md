# account\_nfts

account\_nfts 메소드는 지정된 계정에 대한 NFToken 객체 목록을 반환합니다.

(NonFungibleTokenV1\_1 개정에 의해 추가되었습니다.)

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
  "command": "account_nfts",
  "account": "rsuHaTvJh1bDmDoxX9QcKP7HEBSBt4XsHx",
  "ledger_index": "validated"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
  "method": "account_nfts",
  "params": [{
    "account": "rsuHaTvJh1bDmDoxX9QcKP7HEBSBt4XsHx",
    "ledger_index": "validated"
  }]
}
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| 필드             | 유형        | 설명                                                                                                        |
| -------------- | --------- | --------------------------------------------------------------------------------------------------------- |
| `account`      | 문자열       | 계정의 고유 식별자(일반적으로 계정의 주소)입니다. 요청은 이 계정이 소유한 NFT 목록을 반환합니다.                                                 |
| `ledger_hash`  | 문자열       | (선택 사항) 사용할 원장 버전에 대한 20바이트 16진수 문자열입니다. (원장 지정하기 참조)                                                     |
| `ledger_index` | 문자열 또는 숫자 | (선택 사항) 사용할 원장의 원장 인덱스 또는 원장을 자동으로 선택하기 위한 단축 문자열입니다. (원장 지정하기 참조)                                        |
| `limit`        | 정수        | (선택 사항) 검색할 토큰 페이지 수를 제한합니다. 각 페이지에는 최대 32개의 NFT를 포함할 수 있습니다. 제한 값은 20보다 작거나 400보다 클 수 없습니다. 기본값은 100입니다. |
| `marker`       | marker    | (선택 사항) 이전 페이지 매김된 응답의 값입니다. 해당 응답이 중단된 부분부터 데이터 검색을 다시 시작합니다.                                            |

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "result": {
    "account": "rsuHaTvJh1bDmDoxX9QcKP7HEBSBt4XsHx",
    "account_nfts": [
      {
        "Flags": 1,
        "Issuer": "rGJUF4PvVkMNxG6Bg6AKg3avhrtQyAffcm",
        "NFTokenID": "00010000A7CAD27B688D14BA1A9FA5366554D6ADCF9CE0875B974D9F00000004",
        "NFTokenTaxon": 0,
        "URI": "697066733A2F2F62616679626569676479727A74357366703775646D37687537367568377932366E6634646675796C71616266336F636C67747179353566627A6469",
        "nft_serial": 4
      },
      {
        "Flags": 1,
        "Issuer": "rGJUF4PvVkMNxG6Bg6AKg3avhrtQyAffcm",
        "NFTokenID": "00010000A7CAD27B688D14BA1A9FA5366554D6ADCF9CE087727D1EA000000005",
        "NFTokenTaxon": 0,
        "URI": "697066733A2F2F62616679626569676479727A74357366703775646D37687537367568377932366E6634646675796C71616266336F636C67747179353566627A6469",
        "nft_serial": 5
      }
    ],
    "ledger_hash": "7971093E67341E325251268A5B7CD665EF450B126F67DF8384D964DF834961E8",
    "ledger_index": 2380540,
    "validated": true
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
    "account": "rsuHaTvJh1bDmDoxX9QcKP7HEBSBt4XsHx",
    "account_nfts": [
      {
        "Flags": 1,
        "Issuer": "rGJUF4PvVkMNxG6Bg6AKg3avhrtQyAffcm",
        "NFTokenID": "00010000A7CAD27B688D14BA1A9FA5366554D6ADCF9CE0875B974D9F00000004",
        "NFTokenTaxon": 0,
        "URI": "697066733A2F2F62616679626569676479727A74357366703775646D37687537367568377932366E6634646675796C71616266336F636C67747179353566627A6469",
        "nft_serial": 4
      },
      {
        "Flags": 1,
        "Issuer": "rGJUF4PvVkMNxG6Bg6AKg3avhrtQyAffcm",
        "NFTokenID": "00010000A7CAD27B688D14BA1A9FA5366554D6ADCF9CE087727D1EA000000005",
        "NFTokenTaxon": 0,
        "URI": "697066733A2F2F62616679626569676479727A74357366703775646D37687537367568377932366E6634646675796C71616266336F636C67747179353566627A6469",
        "nft_serial": 5
      }
    ],
    "ledger_hash": "46497E9FF17A993324F1A0A693DC068B467184023C7FD162812265EAAFEB97CB",
    "ledger_index": 2380559,
    "status": "success",
    "validated": true
  }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`                | 유형                                                                | 설명                                                                      |
| ---------------------- | ----------------------------------------------------------------- | ----------------------------------------------------------------------- |
| `account`              | 문자열                                                               | NFT 목록을 소유한 계정                                                          |
| `account_nfts`         | 배열                                                                | 계정이 소유한 NFT 목록으로, NFT 오브젝트로 형식이 지정됩니다(아래 참조).                           |
| `ledger_hash`          | 문자열                                                               | (생략 가능) 이 응답을 생성하는 데 사용된 원장의 식별 해시입니다.                                  |
| `ledger_index`         | 번호 - [원장 색인](https://xrpl.org/basic-data-types.html#ledger-index) | (생략 가능) 이 응답을 생성하는 데 사용된 원장의 원장 인덱스입니다.                                 |
| `ledger_current_index` | 번호 - [원장 색인](https://xrpl.org/basic-data-types.html#ledger-index) | (생략 가능) 이 응답을 생성하는 데 사용된 현재 진행 중인 원장 버전의 원장 인덱스입니다.                     |
| `validated`            | Boolean                                                           | 포함되고 참으로 설정되면 이 응답의 정보는 유효성이 검사된 원장 버전에서 가져옵니다. 그렇지 않으면 정보가 변경될 수 있습니다. |

## NFT 객체

계정\_nfts 배열의 각 객체는 하나의 NFT 토큰을 나타내며 다음과 같은 필드를 갖습니다:

| `Field`        | 유형                                                           | 설명                                                                      |
| -------------- | ------------------------------------------------------------ | ----------------------------------------------------------------------- |
| `Flags`        | 숫자                                                           | 이 NFToken에 활성화된 부울 플래그의 비트맵입니다. 사용 가능한 값은 NFToken 플래그를 참고하세요.           |
| `Issuer`       | 문자열 - [주소](https://xrpl.org/basic-data-types.html#addresses) | 이 NFToken을 발행한 계정입니다.                                                   |
| `NFTokenID`    | 문자열                                                          | 이 NFToken의 고유 식별자(16진수)입니다.                                             |
| `NFTokenTaxon` | 숫자                                                           | 이 토큰의 스크램블되지 않은 분류군 버전입니다. 동일한 분류군을 가진 여러 토큰은 제한된 계열의 인스턴스를 나타낼 수 있습니다. |
| `URI`          | 문자열                                                          | 이 NFToken과 연관된 URI 데이터(16진수)입니다.                                        |
| `nft_serial`   | 숫자                                                           | 발급자에 고유한 이 NFToken의 토큰 시퀀스 번호입니다.                                       |

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* actNotFound - 요청의 계정 필드에 지정된 주소가 장부에 있는 계정과 일치하지 않습니다.
* lgrNotFound - ledger\_hash 또는 ledger\_index로 지정한 ledger이 존재하지 않거나, 존재하지만 서버에 없습니다.
