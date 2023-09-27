# xrp-ledger.toml File

XRP Ledger의 유효성 검증인을 실행하거나 비즈니스에 XRP Ledger을 사용하는 경우, 기계가 읽을 수 있는 xrp-ledger.toml 파일로 XRP Ledger의 사용에 대한 정보를 전 세계에 제공할 수 있습니다. 스크립트 및 애플리케이션은 XRP Ledger에서 사용자를 더 잘 이해하고 표현하기 위해 xrp-ledger.toml 파일에 포함된 정보를 사용할 수 있습니다. 경우에 따라 사람이 동일한 파일을 읽는 것이 유용할 수도 있습니다.

xrp-ledger.toml 파일의 주요 사용 사례 중 하나는 도메인 확인입니다.

## 표기법 규칙

이 문서에서 "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "권장", "MAY" 및 "OPTIONAL" 키워드는 RFC 2119에 설명된 대로 해석해야 합니다.

## 파일 제공

xrp-ledger.toml 파일은 웹 서버에서 제공되어야 합니다. 이 파일은 다음 URL에서 사용할 수 있어야 합니다:

```
https://{DOMAIN}/.well-known/xrp-ledger.toml
```

{DOMAIN}은 하위 도메인을 포함한 도메인 이름입니다. 예를 들어 다음 URL 중 하나에서 파일을 제공할 수 있습니다:

```
https://example.com/.well-known/xrp-ledger.toml
https://xrp.services.example.com/.well-known/xrp-ledger.toml
```

## 프로토콜

콘텐츠는 보안을 위해 잘 알려진 인증 기관에서 서명한 유효한 인증서를 사용하여 최신의 안전한 버전의 TLS를 사용하는 HTTPS 프로토콜을 통해 제공되어야 합니다. (참고: TLS는 이전에는 SSL이라고 불렸지만 해당 버전은 더 이상 안전하지 않습니다.) 즉, xrp-ledger.toml 파일을 호스팅하기 위한 전제 조건은 제대로 구성된 HTTPS 웹 서버가 있어야 한다는 것입니다.

예를 들어, 일부 인터넷 서비스는 일반 HTTP를 통해 검색된 콘텐츠를 수정하여 자체 광고를 삽입하는 것으로 알려져 있습니다. 유사한 기법으로 xrp-ledger.toml 파일의 내용을 잘못 표시하여 스크립트가 잘못 작동하거나 기만적으로 작동하는 것을 방지하려면 일반 HTTP를 통해 제공되는 xrp-ledger.toml 파일의 내용을 신뢰해서는 안 됩니다.

## 도메인

xrp-ledger.toml 파일을 제공하는 도메인은 소유권을 증명하는 도메인입니다. 파일의 콘텐츠는 그 자체로는 유용하거나 신뢰할 수 없습니다. 현실적인 이유로 기본 도메인에서 파일을 제공하는 것이 바람직하지 않을 수 있으므로 하위 도메인을 얼마든지 사용할 수 있습니다. XRP Ledger 계정의 도메인 필드를 설정할 때는 사용한 모든 하위 도메인을 포함한 전체 도메인을 제공해야 합니다. 자세한 내용은 계정 확인을 참조하세요.

원하는 경우 여러 하위 도메인에서 동일한 파일을 제공할 수 있습니다. 예를 들어 www.example.com 하위 도메인이 example.com과 동일한 웹사이트로 연결되는 경우 두 위치 모두에서 파일을 제공할 수 있습니다. 웹사이트에 www 접두사가 필요한 경우, 도메인을 지정할 때 반드시 포함해야 합니다(예: XRP Ledger 계정의 도메인 필드 설정 시).

사람이 읽을 수 있는 웹사이트는 xrp-ledger.toml 파일과 동일한 도메인에서 서비스하는 것이 좋습니다. 웹사이트는 귀하의 신원과 XRP Ledger의 사용 방법에 대한 추가 정보를 제공할 수 있으며, 이는 귀하와 귀하의 서비스에 대한 신뢰를 구축하는 데 도움이 됩니다.

## 경로

RFC5785에 따라 경로는 /.well-known/로 시작해야 합니다. 파일은 /.well-known/xrp-ledger.toml 경로에서 정확히 사용할 수 있어야 합니다(대소문자 구분, 모두 소문자).

원하는 경우 /.wellknown/XRP-Ledger.TOML과 같이 대소문자가 다른 경로에서 동일한 파일을 제공할 수 있습니다. 경로의 대소문자 표기 방식에 따라 다른 콘텐츠를 제공해서는 안 됩니다.

## 헤더

## 콘텐츠 유형

xrp-ledger.toml 파일에 권장되는 콘텐츠 유형은 application/toml입니다. 그러나 파일을 소비하는 애플리케이션은 콘텐츠 유형 값도 텍스트/일반을 허용해야 합니다.

## CORS

다른 웹사이트의 스크립트가 파일을 쿼리할 수 있도록 하려면 파일에 대해 CORS를 활성화해야 합니다. 구체적으로, 서버는 xrp-ledger.toml을 제공할 때 다음 헤더를 제공해야 합니다:

```
Access-Control-Allow-Origin: *
```

이 헤더를 제공하도록 서버를 구성하는 방법에 대한 자세한 내용은 CORS 설정을 참조하세요.

## 기타 헤더

서버는 압축, 캐시 제어, 리디렉션 및 관련 리소스 연결을 위한 헤더를 포함하여 원하는 대로 다른 표준 HTTP 헤더를 사용할 수 있습니다.

## 생성

xrp-ledger.toml 파일은 웹 서버에 저장된 실제 파일일 수도 있고, 웹 서버에서 온디맨드 방식으로 생성할 수도 있습니다. 파일에 제공된 내용이나 웹사이트 구성에 따라 후자의 경우가 더 바람직할 수 있습니다.

## 콘텐츠

xrp-ledger.toml 파일의 콘텐츠는 반드시 TOML로 포맷되어야 합니다. 모든 내용은 선택 사항입니다. 주석은 선택 사항이지만 가독성을 위해 추가하는 것이 좋습니다.

콘텐츠 예시:

```json
# Example xrp-ledger.toml file. These contents should not be considered
# authoritative for any real entity or business.
# Note: all fields and all sections are optional.

[METADATA]
modified = 2019-01-22T00:00:00.000Z
expires = 2019-03-01T00:00:00.000Z

[[VALIDATORS]]
public_key = "nHBtDzdRDykxiuv7uSMPTcGexNm879RUUz5GW4h1qgjbtyvWZ1LE"
attestation = "A59AB577E14A7BEC053752ABFE78C3DED6DCEC81A7C41DF1931BC61742BB4FAEAA0D4F1C1EAE5BC74F6D68A3B26C8A223EA2492A5BD18D51F8AC7F4A97DFBE0C"
network = "main"
owner_country = "us"
server_country = "us"
unl = "https://vl.ripple.com"

[[VALIDATORS]]
public_key = "nHB57Sey9QgaB8CubTPvMZLkLAzfJzNMWBCCiDRgazWJujRdnz13"
attestation = "A59AB577E14A7BEC053752FBFE78C3DED6DCEC81A7C41DF1931BC61742BB4FAEAA0D4F1C1EAE5BC74F6D68A3B26C8A223EA249BA5BD18D51F8AC7F4A97DFBE0C"
network = "testnet"
owner_country = "us"
server_country = "us"
unl = "https://vl.testnet.rippletest.net"

# Note: the attestions above are only examples and are not real.

[[ACCOUNTS]]
address = "r3kmLJN5D28dHuH8vZNUZpMC43pEHpaocV"
desc = "Ripple-owned address from old ripple.txt file"
# Note: This doesn't prove ownership of an account unless the
#   "Domain" field of the account in the XRP Ledger matches the
#   domain this file was served from.

[[SERVERS]]
json_rpc = "https://s1.ripple.com:51234/"
ws = "wss://s1.ripple.com/"
peer = "https://s1.ripple.com:51235/"
desc = "General purpose server cluster"

[[SERVERS]]
json_rpc = "https://s2.ripple.com:51234/"
ws = "wss://s2.ripple.com/"
peer = "https://s2.ripple.com:51235/"
desc = "Full-history server cluster"

[[SERVERS]]
json_rpc = "https://s.testnet.rippletest.net:51234/"
ws = "wss://s.testnet.rippletest.net:51233/"
peer = "https://s.testnet.rippletest.net:51235/"
network = "testnet"
desc = "Test Net public server cluster"

[[PRINCIPALS]]
name = "Rome Reginelli" # Primary spec author
email = "rome@example.com" # Not my real email address

[[CURRENCIES]]
code = "LOL"
issuer = "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn"
network = "testnet"
display_decimals = 2
symbol = "😆" # In practical situations, it may be unwise to use emoji

# End of file
```

## 메타데이터

메타데이터 섹션은 xrp-ledger.toml 파일 자체에 대한 정보를 제공합니다. 존재하는 경우, 이 섹션은 반드시 단일 대괄호를 사용하여 \[METADATA] 줄로 시작하는 단일 테이블로 표시되어야 합니다. (xrp-ledger.toml 파일의 다른 대부분의 섹션은 정보 배열을 위해 대괄호를 사용하지만, \[METADATA] 섹션은 최대 하나만 있습니다). 다음 필드 중 하나를 제공할 수 있습니다(대소문자 구분):

| 필드         | 유형        | 설명                                                          |
| ---------- | --------- | ----------------------------------------------------------- |
| `modified` | 오프셋 날짜-시간 | xrp-leder.toml 파일이 마지막으로 수정된 시간입니다.                         |
| `expires`  | 오프셋 날짜-시간 | 현재 시간이 이 시간보다 크거나 같으면 xrp-ledger.toml 파일이 만료된 것으로 간주해야 합니다. |

사양에는 도메인 필드가 정의되어 있지 않으므로 파일을 제공하는 사이트에서 필드를 결정해야 합니다.

{% hint style="info" %}
Tip:

오프셋 날짜-시간 값의 경우, Ripple에서는 오프셋 Z를 사용하고 최대 밀리초 단위의 정밀도를 제공할 것을 권장합니다. (예: 2019-01-22T22:26:58.027Z) 파일을 직접 편집하는 경우 시, 분, 초, 밀리초의 0을 입력하여 대략적인 시간을 구할 수 있습니다. (예: 2019-01-22T00:00:00.000Z)
{% endhint %}

## 검증인

유효성 검증인 목록은 실행 중인 서버 유효성 검사에 대한 정보를 제공합니다. 유효성 검증인 목록은 반드시 테이블 배열로 표시되어야 하며, 각 항목은 이중 대괄호를 포함하여 \[\[VALIDATORS]] 헤더를 사용해야 합니다. 각 항목은 별도의 유효성 검사 서버를 설명합니다.

파일의 첫 번째\[\[VALIDATORS]] 항목이 기본 유효성 검증인으로 취급됩니다. 프로덕션 XRP Ledger에 대해 하나 이상의 검증자를 실행하는 경우, 다른 사람들이 신뢰할 수 있는 검증자를 우선으로 설정해야 합니다.

각 \[\[VALIDATORS]] 항목에 대해 다음 필드 중 하나를 제공할 수 있습니다:

| 필드              | 유형  | 설명                                                                                                                                                                                                                       |
| --------------- | --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| public\_key     | 문자열 | 기본 유효성 검사기의 마스터 공개 키로, XRP Ledger의 base58 형식으로 인코딩됩니다(일반적으로 n으로 시작).                                                                                                                                                     |
| attestation     | 문자열 | 동일한 엔티티가 이 유효성 검사기와 이 TOML 파일을 제공하는 도메인을 실행한다는 것을 나타내는 16진수로 서명된 메시지입니다. 자세한 내용은 도메인 검증을 참조하십시오.                                                                                                                         |
| network         | 문자열 | 이 유효성 검사기가 따르는 네트워크 체인입니다. 생략하면 클라이언트는 검증자가 프로덕션 XRP Ledger을 따른다고 가정해야 합니다. 프로덕션 XRP Ledger을 명시적으로 지정하려면 main을 사용합니다. Ripple의 XRP Ledger testnet을 위해 testnet을 사용합니다. 다른 testnet이나 비표준 네트워크 체인을 설명하기 위해 다른 값을 제공할 수 있습니다. |
| owner\_country  | 문자열 | 귀하(검증자 소유자)가 적용되는 주요 법적 관할권을 설명하는 두 글자 ISO-3166-2 국가 코드입니다.                                                                                                                                                              |
| server\_country | 문자열 | 이 유효성 검사 서버가 있는 물리적 위치를 설명하는 두 글자로 된 ISO-3166-2 국가 코드입니다.                                                                                                                                                                |
| unl             | 문자열 | 이 유효성 검사기가 신뢰하는 다른 유효성 검사기 목록을 찾을 수 있는 HTTPS URL입니다. 유효성 검사기가 UNL 권장 사항에 대해 유효성 검사기 목록 사이트를 사용하도록 구성된 경우, 이는 서버의 구성과 일치해야 합니다. 프로덕션 XRP 레저 네트워크의 경우 https://vl.ripple.com(후행 슬래시 옵션)를 사용합니다.                             |

## 계정

계정 목록은 사용자가 소유한 XRP Ledger 계정에 대한 정보를 제공합니다. 계정 목록은 반드시 테이블 배열로 제공되어야 하며, 각 항목은 이중 대괄호를 포함한 \[\[ACCOUNTS]] 헤더를 사용해야 합니다. 각 항목은 별도의 계정을 설명합니다. 각 \[\[ACCOUNTS]] 항목에 대해 다음 필드 중 하나를 제공할 수 있습니다:

| 필드        | 유형  | 설명                                                                                                                                                                                                                        |
| --------- | --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `address` | 문자열 | 계정의 공개 주소로, XRP Ledger의 base58 형식으로 인코딩됩니다(일반적으로 r로 시작).                                                                                                                                                                  |
| `network` | 문자열 | 이 계정이 주로 사용되는 네트워크 체인입니다. 생략할 경우, 클라이언트는 해당 계정이 프로덕션 XRP Ledger와 다른 네트워크 체인에서 클레임된 것으로 간주해야 합니다. 프로덕션 XRP Ledger에 메인 사용. Ripple의 XRP Ledger testnet에는 testnet을 사용합니다. 다른 testnet이나 비표준 네트워크 체인을 설명하기 위해 다른 값을 제공할 수 있습니다. |
| `desc`    | 문자열 | 이 계정의 목적 또는 사용 방법에 대한 사람이 읽을 수 있는 설명입니다.                                                                                                                                                                                  |

{% hint style="info" %}
Caution:

누구나 xrp-ledger.toml 파일을 호스팅하여 계정의 소유권을 주장할 수 있으므로, XRP Ledger의 해당 계정에 대한 도메인 필드가 이 xrp-ledger.toml 파일이 제공된 도메인과 일치하지 않는 한 여기에 계정이 있다고 해서 권한이 있는 것으로 간주해서는 안 됩니다. 자세한 내용은 계정 확인을 참조하세요.&#x20;
{% endhint %}

## 주체

주체 목록은 XRP Ledger의 비즈니스와 서비스에 관여하는 사람(또는 비즈니스 주체)에 대한 정보를 제공합니다. 존재하는 경우, 주요인 목록은 반드시 테이블 배열로 표시되어야 하며, 각 항목은 이중 대괄호를 포함한 \[\[PRINCIPALS]] 헤더를 사용해야 합니다. 각 항목은 서로 다른 연락처를 설명합니다. 각 \[\[PRINCIPALS]] 항목에 대해 다음 필드 중 하나를 제공할 수 있습니다:

| 필드      | 유형  | 설명                         |
| ------- | --- | -------------------------- |
| `name`  | 문자열 | 이 주체의 이름입니다.               |
| `email` | 문자열 | 이 주체에게 연락할 수 있는 이메일 주소입니다. |

원하는 경우 다른 연락처 정보를 제공할 수 있습니다. (사용자 지정 필드에 대한 자세한 내용은 사용자 지정 필드를 참조하세요.)

## 서버

서버 목록은 공개 액세스로 실행하는 XRP Ledger 서버(rippled)에 대한 정보를 제공합니다. 서버 목록은 반드시 테이블 배열로 표시되어야 하며, 각 항목은 이중 대괄호를 포함한 \[\[SERVERS]] 헤더를 사용해야 합니다. 각 항목은 서로 다른 서버 또는 서버 클러스터를 설명합니다. 각 \[\[SERVERS]] 항목에 대해 다음 필드 중 하나를 제공할 수 있습니다:

| 필드         | 유형       | 설명                                                                                                                                                                                                           |
| ---------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `json_rpc` | 문자열(URL) | 공개 JSON-RPC API를 제공하는 URL입니다. http:// 또는 https:// 로 시작해야 합니다. 공개 API에는 HTTPS를 사용하는 것이 좋습니다.                                                                                                                  |
| `ws`       | 문자열(URL) | 공개 웹소켓 API를 제공하는 URL입니다. 반드시 ws:// 또는 wss://로 시작해야 합니다. 공개 API에는 WSS를 사용하는 것이 좋습니다.                                                                                                                          |
| `peer`     | 문자열(URL) | 서버가 XRP Ledger 피어 프로토콜을 수신 대기하는 URL입니다. 다른 XRP Ledger 서버는 이 URL에서 연결할 수 있습니다. 서버가 피어 크롤러 응답을 제공하는 경우, 크롤링이 추가된 이 URL에서 응답이 제공됩니다.                                                                            |
| `network`  | 문자열      | 이 서버가 따르는 네트워크 체인입니다. 생략하면 클라이언트는 서버가 프로덕션 XRP Ledger를 따른다고 가정해야 합니다. 프로덕션 XRP Ledger를 명시적으로 지정하려면 main을 사용합니다. 리플의 XRP Ledger testnet에는 testnet을 사용합니다. 다른 testnet이나 비표준 네트워크 체인을 설명하기 위해 다른 값을 제공할 수 있습니다. |

이 섹션의 모든 URL에 대해 후행 슬래시는 권장됩니다. 생략할 경우 클라이언트 애플리케이션은 후행 슬래시가 암시된 것으로 간주해야 합니다.

## 화폐

XRP Ledger에서 자산, 토큰 또는 화폐를 발행하는 경우 \[\[CURRENCIES]] 목록에서 해당 화폐에 대한 정보를 제공할 수 있습니다. 존재하는 경우, 화폐 목록은 반드시 테이블 배열로 표시되어야 하며, 각 항목은 이중 대괄호를 포함한 \[\[CURRENCIES]] 헤더를 사용해야 합니다. 각 항목은 개별 토큰 또는 자산을 설명합니다. 각 \[\[CURRENCIES]] 항목에 대해 다음 필드 중 하나를 제공할 수 있습니다:

| 필드                 | 유형  | 설명                                                                                                                                                                                                                       |
| ------------------ | --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `code`             | 문자열 | XRP Ledger에서 이 토큰의 (대소문자를 구분하는) 티커 심볼입니다. 3자리 코드, 40자 16진수 코드 또는 사용자 지정 형식(XRP Ledger에서 비표준 코드를 표현하는 방법을 알고 있는 클라이언트의 경우)일 수 있습니다. XRP Ledger의 통화 코드 형식에 대한 정보는 통화 코드 참조를 참조하세요.                                         |
| `display_decimals` | 숫자  | 클라이언트 애플리케이션이 이 통화의 금액을 표시하는 데 사용해야 하는 소수점 수입니다.                                                                                                                                                                         |
| `issuer`           | 문자열 | 이 통화를 발행하는 XRP Ledger의 계정 주소로, XRP Ledger의 base58 형식으로 인코딩됩니다(일반적으로 r로 시작). 이 주소는 \[\[계정]] 목록에도 기재해야 합니다. (주의: 여기에 주소가 있다고 해서 그 자체로 공신력이 있는 것은 아닙니다. 자세한 내용은 계정 확인을 참조하세요.)                                              |
| `network`          | 문자열 | 이 토큰을 발급하는 네트워크 체인입니다. 프로덕션 XRP Ledger를 명시적으로 지정하려면 main을 사용합니다. 생략하면 클라이언트는 해당 통화가 프로덕션 XRP Ledger에서 발행된 것으로 간주해야 합니다. Ripple의 XRP Ledger  testnet에 testnet을 사용합니다. 다른 testnet이나 비표준 네트워크 체인을 설명하기 위해 다른 값을 제공할 수 있습니다. |
| `symbol`           | 문자열 | 유니코드 표준에 기호가 있는 경우 이 자산 또는 통화의 금액과 함께 사용해야 하는 "$" 또는 "€"와 같은 텍스트 기호입니다.                                                                                                                                                  |

## 사용자 정의 필드

xrp-ledger.toml 파일은 다른 사용자, 스크립트, 애플리케이션에 정보를 제공하기 위해 XRP Ledger을 사용하는 사용자를 위한 것입니다. 따라서 전달하는 데 유용하지만 이 사양에 설명되어 있지 않은 많은 종류의 정보가 있을 수 있습니다. 사용자는 관련 정보를 전달하기 위해 원하는 대로 xrp-ledger.toml 파일의 모든 레벨에 다른 필드를 추가할 것을 권장합니다.

xrp-ledger.toml 파일을 구문 분석하는 도구는 애플리케이션이 익숙하지 않은 다른 필드가 포함된 문서를 허용해야 합니다. 이러한 도구는 이러한 추가 필드를 호출하는 상위 수준의 애플리케이션에서 사용할 수 있도록 하거나 해당 필드를 삭제할 수 있습니다. 이 사양의 향후 버전과의 호환성을 유지하기 위해 도구는 이 표준에 지정된 필드를 삭제할 수도 있습니다. 도구는 xrp-ledger.toml 파일에 인식할 수 없는 필드가 포함된 경우 오류를 반환하지 않아야 합니다. 오타를 감지하기 위해 도구는 인식할 수 없는 필드에 대해 경고를 제공할 수 있으며, 특히 해당 필드 이름이 표준 필드 이름과 유사한 경우 더욱 그렇습니다.

인식한 필드가 이 사양에 정의되어 있지 않더라도 예상대로 서식이 지정되지 않은 경우 도구에서 오류를 반환할 수 있습니다.

사용자 정의 필드를 만들 때는 필드 이름 선택에 유의하세요. 매우 일반적인 필드 이름을 사용하면 다른 사용자가 같은 이름을 다른 의미로 사용하거나 서로 상충되는 방식으로 서식을 지정할 수 있습니다. 다른 사람들이 유용하게 사용할 수 있는 사용자 정의 필드를 사용하는 경우에는 이 문서의 관리자에게 필드에 대한 사양을 제공하세요.

## CORS 설정

xrp-ledger.toml 파일에 대해 CORS(교차 출처 리소스 공유)를 허용하도록 웹 서버를 구성해야 합니다. 이 구성은 웹 서버에 따라 다릅니다.

Apache HTTP 서버를 실행하는 경우 구성 파일에 다음을 추가합니다:

```
<Location "/.well-known/xrp-ledger.toml">
    Header set Access-Control-Allow-Origin "*"
</Location>
```

또는 서버의 /.well-known/ 디렉터리에 있는 .htaccess 파일에 다음을 추가할 수 있습니다:

```
<Files "xrp-ledger.toml">
    Header set Access-Control-Allow-Origin "*"
</Files>
```

nginx를 사용하는 경우 구성 파일에 다음을 추가합니다:

```
location /.well-known/xrp-ledger.toml {
    add_header 'Access-Control-Allow-Origin' '*';
}
```

다른 웹 서버의 경우 내 서버에 CORS 지원을 추가하고 싶습니다. 관리 호스팅을 사용하는 경우 웹 호스트의 문서에서 특정 경로에서 CORS를 사용하도록 설정하는 방법을 참조하세요. (전체 웹사이트에 대해 CORS를 사용 설정하고 싶지 않을 수도 있습니다.)

## 도메인 확인

xrp-ledger.toml 파일의 한 가지 용도는 유효성 검증인의 공개 키로 식별되는 것처럼 특정 도메인을 실행하는 동일한 엔티티가 특정 유효성 검증인도 실행하는지 확인하는 것입니다. 도메인과 유효성 검증인을 동일한 주체가 소유하고 있는지 확인하면 유효성 검증인 운영자의 신원을 더 확실하게 보장할 수 있으며 신뢰할 수 있는 유효성 검증인이 되기 위해 권장되는 단계입니다. (다른 권장 사항은 좋은 유효성 검증인의 속성을 참조하세요.)

도메인 확인을 사용하려면 도메인 운영자와 유효성 검증인 간에 양방향 링크를 설정해야 합니다:

1. 도메인이 유효성 검증인의 소유권을 주장합니다:

* 이 문서에 설명된 모든 요구 사항에 따라 해당 도메인에서 xrp-ledger.toml 파일을 제공합니다.
* 해당 xrp-ledger.toml 파일의 public\_key 필드에 유효성 검증인의 마스터 공개 키가 있는 \[\[VALIDATORS]] 항목을 제공합니다.

2. 검증인은 도메인의 소유권을 주장합니다:

* 유효성 검증인을 처음 설정할 때 만든 유효성 검증인-키.json 파일에 액세스할 수 있는지 확인합니다. 키를 분실했거나 키가 유출된 경우 키를 해지하고 새 키를 생성하세요.\
  참고: 유효성 검증인-키.json 파일은 유효성 검증인이 아닌 다른 위치에 저장해야 한다는 점을 기억하세요.
* 유효성 검증인이 아닌 위치에서 유효성 검증인-키 도구 를 빌드합니다.
* 다음 명령을 실행하여 도메인을 통합하는 새 유효성 검증인 토큰을 생성하고 xrp-ledger.toml 및 rippled.cfg 파일을 업데이트합니다:

```
$./validator-keys set_domain example.com
```

{% hint style="info" %}
Warning:

이 명령은 validator-keys.json 파일을 업데이트합니다. 유효성 validator-keys.json 파일을 안전한 위치에 저장해야 합니다.
{% endhint %}

샘플 출력:

```json
The domain name has been set to: example.com

The domain attestation for validator nHDG5CRUHp17ShsEdRweMc7WsA4csiL7qEjdZbRVTr74wa5QyqoF is:

attestation="A59AB577E14A7BEC053752FBFE78C3DE
             D6DCEC81A7C41DF1931BC61742BB4FAE
             AA0D4F1C1EAE5BC74F6D68A3B26C8A22
             3EA2492A5BD18D51F8AC7F4A97DFBE0C"

You should include it in your xrp-ledger.toml file in the
section for this validator.

You also need to update the rippled.cfg file to add a new
validator token and restart rippled:

# validator public key: nHDG5CRUHp17ShsEdRweMc7WsA4csiL7qEjdZbRVTr74wa5QyqoF

[validator_token]
eyJ2YWxpZGF0aW9uX3NlY3J|dF9rZXkiOiI5ZWQ0NWY4NjYyNDFjYzE4YTI3NDdiNT
QzODdjMDYyNTkwNzk3MmY0ZTcxOTAyMzFmYWE5Mzc0NTdmYT|kYWY2IiwibWFuaWZl
c3QiOiJKQUFBQUFGeEllMUZ0d21pbXZHdEgyaUNjTUpxQzlnVkZLaWxHZncxL3ZDeE
hYWExwbGMyR25NaEFrRTFhZ3FYeEJ3RHdEYklENk9NU1l1TTBGREFscEFnTms4U0tG
bjdNTzJmZGtjd1JRSWhBT25ndTlzQUtxWFlvdUorbDJWMFcrc0FPa1ZCK1pSUzZQU2
hsSkFmVXNYZkFpQnNWSkdlc2FhZE9KYy9hQVpva1MxdnltR21WcmxIUEtXWDNZeXd1
NmluOEhBU1FLUHVnQkQ2N2tNYVJGR3ZtcEFUSGxHS0pkdkRGbFdQWXk1QXFEZWRGdj
VUSmEydzBpMjFlcTNNWXl3TFZKWm5GT3I3QzBrdzJBaVR6U0NqSXpkaXRROD0ifQ==     
```

증명 블록으로 xrp-ledger.toml 파일의 내용을 업데이트하고, 샘플 출력의 \[validator\_token] 블록으로 rippled.cfg 파일을 업데이트합니다.

{% hint style="info" %}
Warning:

유효성 검증인 토큰은 비밀로 유지되어야 합니다. xrp-ledger.toml 파일이나 다른 곳에 공유하지 마세요.
{% endhint %}

## 계정 확인

계정 확인은 도메인 확인과 마찬가지로 동일한 주체가 특정 도메인과 특정 XRP Ledger 주소를 제어한다는 것을 증명하는 개념입니다. 계정 확인은 XRP Ledger을 사용하거나 xrp-ledger.toml 파일을 제공하는 데 필요하지 않지만, 투명성을 위해 계정 확인을 원할 수 있습니다.

계정을 확인하려면 도메인 운영자와 주소 사이에 양방향 링크를 설정해야 합니다:

1. 도메인이 주소의 소유권을 주장합니다.

* 이 문서에 설명된 모든 요구 사항에 따라 해당 도메인에서 xrp-ledger.toml 파일을 제공합니다.
* 해당 xrp-ledger.toml 파일에 확인하려는 계정의 주소와 함께 \[\[ACCOUNTS]] 항목을 제공합니다. 이 주소에서 화폐를 발행하는 경우 \[\[CURRENCIES]] 항목의 발행자 필드에 이 계정을 제공할 수도 있습니다.

2. 이 주소는 도메인이 소유권을 주장합니다.\
   계정의 도메인 필드를 이 xrp-ledger.toml 파일이 제공된 도메인과 일치하도록 설정합니다. 도메인 값(ASCII로 디코딩할 때)은 www와 같은 모든 하위 도메인을 포함하여 정확히 일치해야 합니다. 국제화된 도메인 이름의 경우 RFC3492에 설명된 대로 도메인 값을 도메인의 Punycode로 설정합니다.\
   도메인을 설정하려면 트랜잭션을 전송해야 하므로 도메인 값을 설정한 사람은 트랜잭션을 전송할 때 계정의 비밀 키를 가지고 있어야 합니다.

이 두 링크 중 하나만으로는 권한이 있는 것으로 간주해서는 안 됩니다. 누구나 계정의 소유권을 주장하는 xrp-ledger.toml 파일을 호스팅할 수 있으며, 계정 운영자는 도메인 필드를 원하는 문자열로 설정할 수 있습니다. 이 둘이 일치하면 동일한 주체가 두 계정을 모두 제어한다는 강력한 증거가 됩니다.

## 감사

이 사양은 원래의 ripple.txt 사양에서 파생되었으며, stellar.toml 파일에서 영감을 얻었습니다. 이 사양은 또한 XRP 커뮤니티 회원과 많은 전현직 Ripple 직원들의 피드백을 통합합니다.
