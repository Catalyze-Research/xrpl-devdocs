# deposit\_authorized

deposit\_authorized 명령은 한 계정이 다른 계정으로 직접 송금할 수 있는 권한이 있는지 여부를 나타냅니다. 계정으로 송금하기 위해 승인을 요청하는 방법에 대한 자세한 내용은 deposit\_authorized을 참조하세요.

## 요청 형식

요청 형식의 예시입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 1,
  "command": "deposit_authorized",
  "source_account": "rEhxGqkqPPSxQ3P25J66ft5TwpzV14k2de",
  "destination_account": "rsUiUMpnrgxQp24dJYZDhmV4bE3aBtQyt8",
  "ledger_index": "validated"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
  "method": "deposit_authorized",
  "params": [
    {
      "source_account": "rEhxGqkqPPSxQ3P25J66ft5TwpzV14k2de",
      "destination_account": "rsUiUMpnrgxQp24dJYZDhmV4bE3aBtQyt8",
      "ledger_index": "validated"
    }
  ]
}
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| `Field`               | 유형                                                           | 설명                                                                                                                                                                             |
| --------------------- | ------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `source_account`      | 문자열 - [주소](https://xrpl.org/basic-data-types.html#addresses) | 가능한 지불의 발신자.                                                                                                                                                                   |
| `destination_account` | 문자열 - [주소](https://xrpl.org/basic-data-types.html#addresses) | 가능한 지불의 수령인.                                                                                                                                                                   |
| `ledger_hash`         | 문자열                                                          | (선택 사항) 사용할 원장 버전에 대한 20바이트 16진수 문자열입니다. ([원장 지정](https://xrpl.org/basic-data-types.html#specifying-ledgers) 참조)                                                               |
| `ledger_index`        | 문자열 또는 부호 없는 정수                                              | (선택) 사용할 원장의 [원장 인덱스](https://xrpl.org/basic-data-types.html#ledger-index) 또는 자동으로 원장을 선택하는 단축 문자열입니다. ([원장 지정](https://xrpl.org/basic-data-types.html#specifying-ledgers) 참조) |

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 1,
  "result": {
    "deposit_authorized": true,
    "destination_account": "rsUiUMpnrgxQp24dJYZDhmV4bE3aBtQyt8",
    "ledger_hash": "BD03A10653ED9D77DCA859B7A735BF0580088A8F287FA2C5403E0A19C58EF322",
    "ledger_index": 8,
    "source_account": "rEhxGqkqPPSxQ3P25J66ft5TwpzV14k2de",
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
    "deposit_authorized": true,
    "destination_account": "rsUiUMpnrgxQp24dJYZDhmV4bE3aBtQyt8",
    "ledger_hash": "BD03A10653ED9D77DCA859B7A735BF0580088A8F287FA2C5403E0A19C58EF322",
    "ledger_index": 8,
    "source_account": "rEhxGqkqPPSxQ3P25J66ft5TwpzV14k2de",
    "status": "success",
    "validated": true
  }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| `Field`                | 유형                                                                | 설명                                                                                                                                        |
| ---------------------- | ----------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `deposit_authorized`   | Boolean                                                           | 지정된 원본 계정이 대상 계정으로 직접 결제를 보낼 수 있는 권한이 있는지 여부입니다. `true`이면 대상 계좌에 [입금 승인이](https://xrpl.org/depositauth.html) 필요하지 않거나 원본 계좌가 사전 승인된 것입니다. |
| `destination_account`  | 문자열 - [주소](https://xrpl.org/basic-data-types.html#addresses)      | 요청에 지정된 대상 계정입니다.                                                                                                                         |
| `ledger_hash`          | 문자열                                                               | (생략 가능) 이 응답을 생성하는 데 사용된 원장의 식별 해시입니다.                                                                                                    |
| `ledger_index`         | 번호 - [원장 색인](https://xrpl.org/basic-data-types.html#ledger-index) | (생략 가능) 이 응답을 생성하는 데 사용된 원장 버전의 원장 인덱스입니다.                                                                                                |
| `ledger_current_index` | 번호 - [원장 색인](https://xrpl.org/basic-data-types.html#ledger-index) | (생략 가능) 이 응답을 생성하는 데 사용된 현재 진행 중인 원장 버전의 원장 인덱스입니다.                                                                                       |
| `source_account`       | 문자열 - [주소](https://xrpl.org/basic-data-types.html#addresses)      | 요청에 지정된 원본 계정입니다.                                                                                                                         |
| `validated`            | Boolean                                                           | (생략 가능) `true`인 경우 해당 정보는 검증된 원장 버전에서 가져온 것입니다.                                                                                           |

{% hint style="info" %}
Note:

deposit\_authorized 상태가 true라고 해서 지정된 출처에서 지정된 목적지로 결제를 보낼 수 있다는 보장은 없습니다. 예를 들어, 목적지 계좌에 지정한 통화에 대한 신탁 계좌가 없거나 송금할 수 있는 유동성이 충분하지 않을 수 있습니다.
{% endhint %}

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* actMalformed - 요청의 source\_account 또는 destination\_account 필드에 지정된 주소의 형식이 올바르지 않습니다. (오타가 있거나 길이가 잘못되어 체크섬에 실패했을 수 있습니다.)
* dstActNotFound - 요청의 대상\_계정 필드가 ledger에 있는 계정과 일치하지 않습니다.
* lgrNotFound - ledger\_hash 또는 ledger\_index로 지정한 ledger이 존재하지 않거나 존재하지만 서버에 없습니다.
* srcActNotFound - 요청의 source\_account 필드가 ledger의 계정과 일치하지 않습니다.
