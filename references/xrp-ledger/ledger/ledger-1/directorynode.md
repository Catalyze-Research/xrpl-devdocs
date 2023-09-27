# DirectoryNode

배DirectoryNode 객체 유형은 ledger의 상태 트리에서 다른 객체에 대한 링크 목록을 제공합니다. 단일 개념의 디렉터리는 이중으로 연결된 목록의 형태를 취하며, 하나 이상의 DirectoryNode 객체는 각각 최대 32개의 다른 객체 ID를 포함합니다. 첫 번째 객체를 디렉터리의 루트라고 하며, 루트 객체 이외의 모든 객체는 필요에 따라 추가하거나 삭제할 수 있습니다.

디렉터리에는 두 가지 종류가 있습니다:

* 소유자 디렉터리에는 RippleState(신뢰선) 또는 제안 객체와 같이 계정이 소유한 다른 객체가 나열됩니다.
* 제안 디렉터리에는 탈중앙화 거래소에서 사용 가능한 제안이 나열됩니다. 하나의 제안 디렉터리에는 동일한 토큰(화폐 코드 및 발행자)에 대해 동일한 환율을 가진 모든 제안이 포함됩니다.

## DirectoryNode JSON 예시

{% tabs %}
{% tab title="Offer Directory" %}
```
{
    "ExchangeRate": "4F069BA8FF484000",
    "Flags": 0,
    "Indexes": [
        "AD7EAE148287EF12D213A251015F86E6D4BD34B3C4A0A1ED9A17198373F908AD"
    ],
    "LedgerEntryType": "DirectoryNode",
    "RootIndex": "1BBEF97EDE88D40CEE2ADE6FEF121166AFE80D99EBADB01A4F069BA8FF484000",
    "TakerGetsCurrency": "0000000000000000000000000000000000000000",
    "TakerGetsIssuer": "0000000000000000000000000000000000000000",
    "TakerPaysCurrency": "0000000000000000000000004A50590000000000",
    "TakerPaysIssuer": "5BBC0F22F61D9224A110650CFE21CC0C4BE13098",
    "index": "1BBEF97EDE88D40CEE2ADE6FEF121166AFE80D99EBADB01A4F069BA8FF484000"
}
```
{% endtab %}

{% tab title="Owner Directory" %}
```
{
    "Flags": 0,
    "Indexes": [
        "AD7EAE148287EF12D213A251015F86E6D4BD34B3C4A0A1ED9A17198373F908AD",
        "E83BBB58949A8303DF07172B16FB8EFBA66B9191F3836EC27A4568ED5997BAC5"
    ],
    "LedgerEntryType": "DirectoryNode",
    "Owner": "rpR95n1iFkTqpoy1e878f4Z1pVHVtWKMNQ",
    "RootIndex": "193C591BF62482468422313F9D3274B5927CA80B4DD3707E42015DD609E39C94",
    "index": "193C591BF62482468422313F9D3274B5927CA80B4DD3707E42015DD609E39C94"
}
```
{% endtab %}
{% endtabs %}

## DirectoryNode 필드 <a href="#directorynode-fields" id="directorynode-fields"></a>

| 이름                  | JSON 유형 | 내부 유형  | 필수 여부 | 설명                                                                                              |
| ------------------- | ------- | ------ | ----- | ----------------------------------------------------------------------------------------------- |
| `ExchangeRate`      | 문자열     | Uint64 | 아니요   | (디렉터리만 제공) **DEPRECATED**. 사용하지 마세요.                                                            |
| `Flags`             | 숫자      | Uint32 | 예     | 이 개체에 대해 활성화된 boolean 플래그의 비트맵입니다. 현재 프로토콜은 `DirectoryNode`개체에 대한 플래그를 정의하지 않습니다. 값은 항상 `0`입니다. |
| `Indexes`           | 배열      | 벡터256  | 예     | 이 디렉터리의 내용: 다른 객체의 ID 배열.                                                                       |
| `IndexNext`         | 숫자      | Uint64 | 아니요   | 이 디렉터리가 여러 페이지로 구성되어 있는 경우 이 ID는 체인의 다음 개체에 연결되어 끝에서 래핑됩니다.                                     |
| `IndexPrevious`     | 숫자      | Uint64 | 아니요   | 이 디렉터리가 여러 페이지로 구성된 경우 이 ID는 체인의 이전 개체에 연결되어 시작 부분에서 줄 바꿈됩니다.                                   |
| `LedgerEntryType`   | 문자열     | Uint16 | 예     | `0x0064`문자열에 매핑된 값은 `DirectoryNode`이 개체가 디렉터리의 일부임을 나타냅니다.                                      |
| `Owner`             | 문자열     | 계정 ID  | 아니요   | (소유자 디렉터리만 해당) 이 디렉터리의 개체를 소유하는 계정의 주소입니다.                                                      |
| `RootIndex`         | 문자열     | 해시256  | 예     | 이 디렉터리에 대한 루트 개체의 ID입니다.                                                                        |
| `TakerGetsCurrency` | 문자열     | 해시160  | 아니요   | `TakerGets`(오퍼 디렉터리만 해당)는 디렉터리에 있는 오퍼 금액 의 화폐 코드입니다.                                            |
| `TakerGetsIssuer`   | 문자열     | 해시160  | 아니요   | `TakerGets`(오퍼 디렉터리만 해당)는 디렉터리에 있는 오퍼 금액 의 화폐 코드입니다.                                            |
| `TakerPaysCurrency` | 문자열     | 해시160  | 아니요   | `TakerPays`(오퍼 디렉터리만 해당)는 디렉터리에 있는 오퍼 금액 의 화 코드입니다 .                                            |
| `TakerPaysIssuer`   | 문자열     | 해시160  | 아니요   | `TakerPays`(오퍼 디렉터리만 해당)는 디렉터리에 있는 오퍼 금액 의 화 코드입니다 .                                            |

## 디렉터리 ID 형식 <a href="#directorynode-fields" id="directorynode-fields"></a>

DirectoryNode가 다음 중 어떤 것을 나타내는지에 따라 DirectoryNode의 ID를 생성하는 세 가지 공식이 있습니다:

* 소유자 디렉터리의 첫 페이지(루트라고도 함).
* 제안 디렉터리의 첫 페이지.
* 두 유형의 이후 페이지.

소유자 디렉터리의 첫 페이지에는 다음 값의 절반인 SHA-512가 순서대로 연결된 ID가 있습니다:

* 소유자 디렉터리 스페이스 키(0x004F).
* 소유자 필드의 계정 ID.

제안 디렉터리의 첫 페이지에는 특별한 ID가 있는데, 상위 192비트는 오더북을 정의하고 나머지 64비트는 해당 디렉터리에 있는 제안의 환율을 정의합니다. (ID는 빅 엔디안이므로 주문서는 더 중요한 비트가 먼저 오고, 품질은 덜 중요한 비트가 나중에 옵니다.) 이를 통해 오더북을 최상의 제안부터 최악의 제안까지 반복해서 살펴볼 수 있습니다. 구체적으로, 처음 192비트는 다음 값의 절반인 SHA-512의 처음 192비트를 순서대로 연결한 것입니다:

* 예약 디렉터리 공백 키(0x0042).
* TakerPaysCurrency의 160비트 화폐 코드.
* TakerGetsCurrency의 160비트 화폐 코드.
* TakerPaysIssuer의 계정 ID.
* TakerGetsIssuer에게서 받은 계정 ID.

제안 디렉터리 ID의 하단 64비트는 해당 디렉터리에 있는 제안의 테이커페이 금액을 테이커갓 금액으로 나눈 값을 XRP Ledger의 내부 금액 형식인 64비트 숫자로 나타냅니다.

디렉터리 노드가 디렉터리의 첫 페이지가 아닌 경우(소유자 디렉터리든 제안 디렉터리든 상관없습니다), 다음 값의 절반을 순서대로 연결한 SHA-512의 절반인 ID를 갖습니다:

* DirectoryNode 스페이스 키(0x0064).
* 루트 DirectoryNode의 ID.
* 이 객체의 페이지 번호. (0이 루트 DirectoryNode이므로 이 값은 정수 1 이상입니다.)
