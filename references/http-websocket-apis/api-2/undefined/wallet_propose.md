# wallet\_propose

wallet\_propose 메소드를 사용해 키 쌍과 XRP Ledger 주소를 생성합니다. 이 명령은 키와 주소 값만 생성하며, XRP Ledger 자체에는 어떤 영향도 미치지 않습니다. ledger에 저장된 자금 지원 주소가 되려면 해당 주소가 준비금 요건을 충족하기에 충분한 XRP를 제공하는 결제 트랜잭션을 수신해야 합니다.

wallet\_propose 메소드는 권한이 없는 사용자가 실행할 수 없는 관리자 메소드입니다! (관리자 명령은 일반적으로 외부 네트워크를 통해 전송되지 않기 때문에 계정 비밀을 위해 네트워크 트래픽을 스니핑하는 사람들로부터 보호하기 위해 이 명령은 제한되어 있습니다).

[![Updated in: rippled 0.31.0](https://img.shields.io/badge/Updated%20in-rippled%200.31.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/0.31.0)

## 요청 형식

요청 형식의 예시입니다:

{% tabs %}
{% tab title="WebSocket (with key type)" %}
```json
{
    "command": "wallet_propose",
    "seed": "snoPBrXtMeMyMHUVTgbuqAfg1SUTb",
    "key_type": "secp256k1"
}
```
{% endtab %}

{% tab title="WebSocket (no key type)" %}
```json
{
    "command": "wallet_propose",
    "passphrase": "masterpassphrase"
}
```
{% endtab %}

{% tab title="JSON-RPC (with key type)" %}
```json
{
    "method": "wallet_propose",
    "params": [
        {
            "seed": "snoPBrXtMeMyMHUVTgbuqAfg1SUTb",
            "key_type": "secp256k1"
        }
    ]
}
```
{% endtab %}

{% tab title="JSON-RPC (no key type)" %}
```json
{
    "method": "wallet_propose",
    "params": [
        {
            "passphrase": "snoPBrXtMeMyMHUVTgbuqAfg1SUTb"
        }
    ]
}
```
{% endtab %}

{% tab title="Commandline" %}
```
#Syntax: wallet_propose [passphrase]
rippled wallet_propose masterpassphrase
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함될 수 있습니다:

| Field      | 유형  | 설명                                                                                                                                                                                                                                                                  |
| ---------- | --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| key\_type  | 문자열 | 이 키 쌍을 파생하는 데 사용할 서명 알고리즘 입니다. 유효한 값은 ed25519 와secp256k1(모두 소문자)입니다. 기본값은 secp256k1입니다.                                                                                                                                                                             |
| passphrase | 문자열 | (선택 사항) 이 시드 값에서 키 쌍과 주소를 생성합니다. [이 값은 16진수](https://en.wikipedia.org/wiki/Hexadecimal), XRP Ledger의 [base58](https://xrpl.org/base58-encodings.html) 형식, [RFC-1751](https://tools.ietf.org/html/rfc1751) 또는 임의 문자열로 형식화될 수 있습니다. seed 또는 seed\_hex와 함께 사용할 수 없습니다. |
| seed       | 문자열 | (선택 사항) XRP Ledger의 [base58](https://xrpl.org/base58-encodings.html) 인코딩 형식으로 이 시드 값에서 키 쌍과 주소를 생성합니다. `passphrase` 또는 seed\_hex와 함께 사용할 수 없습니다.                                                                                                                    |
| seed\_hex  | 문자열 | (선택 사항) 이 시드 값에서 [16진수](https://en.wikipedia.org/wiki/Hexadecimal) 형식 의 키 쌍과 주소를 생성합니다. `passphrase` 또는 `seed`와 함께 사용할 수 없습니다.                                                                                                                                      |

`passphrase`, `seed`, 또는 `seed_hex` 중 하나만 제공해야 합니다. 세 가지를 모두 생략하면 Ripple은 임의의 시드를 사용합니다.

{% hint style="info" %}
Note:

이 명령의 커맨드라인 버전은 Ed25519 키를 생성할 수 없습니다.
{% endhint %}

## 시드 지정하기

대부분의 경우 강력한 무작위성 소스에서 생성된 시드 값을 사용해야 합니다. 주소의 시드 값을 아는 사람은 누구나 해당 주소로 서명된 트랜잭션을 보낼 수 있는 모든 권한을 갖습니다. 일반적으로 이 명령을 매개변수 없이 실행하는 것이 무작위 시드를 생성하는 좋은 방법입니다.

알려진 시드를 지정하는 경우는 다음과 같습니다:

* 해당 주소와 연관된 시드만 알고 있는 경우 주소를 다시 계산하는 경우
* rippled 기능 테스트

시드를 지정하는 경우 다음 형식 중 하나로 지정할 수 있습니다:

* XRP Ledger의 base58 형식의 비밀 키 문자열로. 예: snoPBrXtMeMyMHUVTgbuqAfg1SUTb.
* RFC-1751 형식 문자열(secp256k1 키 쌍만 해당). 예시: 아이어 본드 보우 트리오 레이드 시트 골 헨 아이비스 아이비스 데어.
* 128비트 16진수 문자열. 예: DEDCE9CE67B451D852FD4E846FCDE31C.
* 시드 값으로 사용할 임의의 문자열입니다. 예: masterpassphrase.

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 2,
  "status": "success",
  "type": "response",
  "result": {
    "account_id": "rHb9CJAWyB4rj91VRWn96DkukG4bwdtyTh",
    "key_type": "secp256k1",
    "master_key": "I IRE BOND BOW TRIO LAID SEAT GOAL HEN IBIS IBIS DARE",
    "master_seed": "snoPBrXtMeMyMHUVTgbuqAfg1SUTb",
    "master_seed_hex": "DEDCE9CE67B451D852FD4E846FCDE31C",
    "public_key": "aBQG8RQAzjs1eTKFEAQXr2gS4utcDiEC9wmi7pfUPTi27VCahwgw",
    "public_key_hex": "0330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD020"
  }
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "result": {
        "account_id": "rHb9CJAWyB4rj91VRWn96DkukG4bwdtyTh",
        "key_type": "secp256k1",
        "master_key": "I IRE BOND BOW TRIO LAID SEAT GOAL HEN IBIS IBIS DARE",
        "master_seed": "snoPBrXtMeMyMHUVTgbuqAfg1SUTb",
        "master_seed_hex": "DEDCE9CE67B451D852FD4E846FCDE31C",
        "public_key": "aBQG8RQAzjs1eTKFEAQXr2gS4utcDiEC9wmi7pfUPTi27VCahwgw",
        "public_key_hex": "0330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD020",
        "status": "success"
    }
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
Loading: "/etc/rippled.cfg"
Connecting to 127.0.0.1:5005

{
   "result" : {
      "account_id" : "rHb9CJAWyB4rj91VRWn96DkukG4bwdtyTh",
      "key_type" : "secp256k1",
      "master_key" : "I IRE BOND BOW TRIO LAID SEAT GOAL HEN IBIS IBIS DARE",
      "master_seed" : "snoPBrXtMeMyMHUVTgbuqAfg1SUTb",
      "master_seed_hex" : "DEDCE9CE67B451D852FD4E846FCDE31C",
      "public_key" : "aBQG8RQAzjs1eTKFEAQXr2gS4utcDiEC9wmi7pfUPTi27VCahwgw",
      "public_key_hex" : "0330E7FC9D56BB25D6893BA3F317AE5BCF33B3291BD63DB32654A313222F7FD020",
      "status" : "success"
   }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드를 포함하여 새(잠재적) 계정에 대한 다양한 중요 정보가 포함됩니다:

| Field             | 유형  | 설명                                                                                                                                                                                                                                                                                                                 |
| ----------------- | --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| key\_type         | 문자열 | 이 키 쌍을 생성하는 데 사용된 서명 알고리즘입니다. 유효한 값은 ed25519와 secp256k1(모두 소문자)입니다.                                                                                                                                                                                                                                                |
| master\_seed      | 문자열 | XRP Ledger의 base58 인코딩 문자열 형식의 마스터 시드입니다. 일반적으로 이 형식의 키를 사용하여 트랜잭션에 서명합니다. 마스터 시드 문자열, 16진수 형식의 마스터 시드입니다.                                                                                                                                                                                                         |
| master\_seed\_hex | 문자열 | 16진수 형식의 마스터 시드입니다.                                                                                                                                                                                                                                                                                                |
| master\_key       | 문자열 | **DEPRECATED** 마스터 시드로, RFC-1751 형식입니다. 참고: 리플 구현은 RFC-1751에서 디코딩한 후 RFC-1751로 인코딩하기 전에 키의 바이트 순서를 반대로 합니다. 다른 RFC-1751 구현을 사용하여 XRP 레저에 사용하기 위해 키를 읽거나 쓰는 경우, 리플의 RFC-1751 인코딩과 호환되도록 동일하게 수행해야 합니다.                                                                                                              |
| account\_id       | 문자열 | [XRP Ledger의 ](https://xrpl.org/base58-encodings.html)base58 형식의 계정 주소입니다. 이것은 공개 키가 아니라 해시의 해시입니다. 또한 체크섬이 있으므로 오타가 있을 경우 유효하지만 다른 주소가 아닌 잘못된 주소가 될 가능성이 거의 확실합니다. 이는 XRP 원장에 있는 계정의 기본 식별자입니다. 돈을 받기 위해 사람들에게 이 주소를 알려주고, 거래에서 이 주소를 사용해 내가 누구인지, 누구에게 돈을 지불하고 신뢰하는지 등을 표시합니다. 다중 서명 목록에서도 다른 서명자를 식별하는 데 사용합니다. |
| public\_key       | 문자열 | XRP Ledger의 base58 인코딩된 문자열 형식입니다. master\_seed에서 파생됩니다.                                                                                                                                                                                                                                                           |
| public\_key\_hex  | 문자열 | 키 쌍의 공개 키로, 16진수입니다. 마스터\_시드에서 파생됩니다. 트랜잭션의 서명을 검증하기 위해 리플은 이 공개키가 필요합니다. 그렇기 때문에 서명된 트랜잭션의 형식에는 SigningPubKey 필드에 공개 키가 포함됩니다.                                                                                                                                                                                    |
| warning           | 문자열 | (생략 가능) 요청에서 시드 값을 지정한 경우, 이 필드는 안전하지 않을 수 있다는 경고를 제공합니다.                                                                                                                                                                                                                                                          |

이 방법을 사용하여 계정의 일반 키 쌍으로 사용할 키 쌍을 생성할 수도 있습니다. 계정에 일반 키 쌍을 할당하면 대부분의 거래에 서명할 수 있으며, 마스터 키 쌍은 가능한 한 오프라인 상태로 유지할 수 있습니다.

일반 키 쌍으로 사용하는 것 외에도 다중 서명 목록(SignerList)의 구성원으로 사용할 수도 있습니다.

마스터 및 일반 키 쌍에 대한 자세한 내용은 암호화 키를 참조하세요.

다중 서명 및 서명자 목록에 대한 자세한 내용은 다중 서명을 참조하세요.

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었습니다.
* badSeed - 요청이 빈 문자열 또는 XRP Ledger 주소와 유사한 문자열과 같이 허용되지 않는 시드 값(암호문, 시드 또는 시드\_헥스 필드에)을 지정했습니다.
