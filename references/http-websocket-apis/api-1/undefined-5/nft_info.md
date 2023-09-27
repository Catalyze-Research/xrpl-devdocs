# nft\_info

문자열문자열nft\_info 명령은 클리오 서버에 쿼리 중인 NFT에 대한 정보를 요청합니다. [![New in: Clio v1.1.0](https://img.shields.io/badge/New%20in-Clio%20v1.1.0-blue.svg)](https://github.com/XRPLF/clio/releases/tag/1.1.0)

## 요청 형식

요청 형식의 예시입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 1,
  "command": "nft_info",
  "nft_id": "00080000B4F4AFC5FBCBD76873F18006173D2193467D3EE70000099B00000000"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "nft_info",
    "params": [
      {
          "nft_id": "00080000B4F4AFC5FBCBD76873F18006173D2193467D3EE70000099B00000000"
      }
    ]
}
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| `Field`        | 유형              | 설명                                                                                                                                                                             |
| -------------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `nft_id`       | 문자열             | 대체 불가능한 토큰(NFT)의 고유 식별자입니다.                                                                                                                                                    |
| `ledger_hash`  | 문자열             | (선택 사항) 사용할 원장 버전에 대한 20바이트 16진수 문자열입니다. ([원장 지정](https://xrpl.org/basic-data-types.html#specifying-ledgers) 참조)                                                               |
| `ledger_index` | 문자열 또는 부호 없는 정수 | (선택 사항) 사용할 원장의 원장 인덱스 또는 원장을 자동으로 선택하기 위한 바로가기 문자열입니다. ledger\_index를 닫힘 또는 현재로 지정하지 마세요. 이렇게 하면 P2P rippled 서버로 요청이 전달되며, rippled에서는 nft\_info API를 사용할 수 없습니다. (원장 지정하기 참조) |

ledger 버전을 지정하지 않으면 클리오는 검증된 최신 ledger을 사용합니다.

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "result": {
    "nft_id": "00080000B4F4AFC5FBCBD76873F18006173D2193467D3EE70000099B00000000",
    "ledger_index": 269,
    "owner": "rG9gdNygQ6npA9JvDFWBoeXbiUcTYJnEnk",
    "is_burned": false,
    "flags": 8,
    "transfer_fee": 0,
    "issuer": "rHVokeuSnjPjz718qdb47bGXBBHNMP3KDQ",
    "nft_taxon": 0,
    "nft_sequence": 0,
    "uri": "https://xrpl.org",
    "validated": true,
    "status": "success"
  },
  "warnings": [
    {
      "id": 2001,
      "message": "This is a clio server. clio only serves validated data. If you want to talk to rippled, include 'ledger_index':'current' in your request"
    },
    {
      "id": 2002,
      "message": "This server may be out of date"
    }
  ]
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
  "result": {
    "nft_id": "00080000B4F4AFC5FBCBD76873F18006173D2193467D3EE70000099B00000000",
    "ledger_index": 269,
    "owner": "rG9gdNygQ6npA9JvDFWBoeXbiUcTYJnEnk",
    "is_burned": false,
    "flags": 8,
    "transfer_fee": 0,
    "issuer": "rHVokeuSnjPjz718qdb47bGXBBHNMP3KDQ",
    "nft_taxon": 0,
    "nft_sequence": 0,
    "uri": "https://xrpl.org",
    "validated": true,
    "status": "success"
  },
  "warnings": [
    {
      "id": 2001,
      "message": "This is a clio server. clio only serves validated data. If you want to talk to rippled, include 'ledger_index':'current' in your request"
    },
    {
      "id": 2002,
      "message": "This server may be out of date"
    }
  ]
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 일부 배열된 nft\_info 응답 객체가 포함됩니다:

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
