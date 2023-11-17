# 주소(Addresses)

분산 원장에서 계정은 [base58](../../references/xrp-ledger-xrp-ledger-protocol-reference/basic-data-types/base58-base58-encodings.md) 포맷을 통해 생성된 주소를 통해 고유성을 가진다. 주소는 계정 주인의 공개키를 통해 도출되며, 그 공개키는 또한 비밀키를 통해 도출된다. 주소는 JSON 형식에서 문자열로 표현된다. 그리고 다음과 같은 특성을 가진다.

* 25\~35 길이의 문자열이다.
* 문자 <mark style="background-color:yellow;">r</mark>로 시작한다.
* &#x20;alphanumeric 숫자를 사용하며, 숫자 "0", 대문자 "O", 대문자 "$$I$$", 소문자 "l"은 제외한다.(base58)
* 대소문자를 구분한다.
* $$1/2^{32}$$ 에 근사한 확률로 검출되는 유효      한 주소인지 구분하기 위해 4byte의 체크섬을 포함한다.&#x20;

{% hint style="info" %}
X-주소 형식은 주소에 목적지 태그를 "함축"하고 있습니다. 이 주소는 X(메인넷 기) 또는 T(테스트넷 기준)로 시작합니다. 이런 하나의 값을 통해 소비자가 알아야할 모든 정보를 표현하기 위해 거래소나 지갑은 X-주소 형식을  사용할 수 있습니다. 더 많은 정보를 얻으려면 [X-주소 형식 ](https://xrpaddress.info/)그리고 [코덱](https://github.com/xrp-community/xrpl-tagged-address-codec)을 참고하세요.                                                                                                                 분산원장 프로토콜은 기본적으로 오직 "클래식" 주소들만 허용하지만, 많은 [클라이언트 라이브러리](../../references/undefined/)는 X-주소 또한 지원합니다.
{% endhint %}

더 많은 정보를 얻으려면 [계정](./) 그리고[ base58 인코딩](../../references/xrp-ledger-xrp-ledger-protocol-reference/basic-data-types/base58-base58-encodings.md)을 참고하세요.

타당한 주소는 펀딩을 받음으로 인해서 [분산원장의 계정이 될 수 있습니다](./). 또한, 펀딩 받지못한 주소는 [일반 키](cryptographic-keys.md)를 표현하기 위해 사용될 수 있습니다. 또는 [서명자 목록](undefined-1.md)의 멤버가 될 수 있습니다. 오직 펀딩 받은 계정만이 트랜잭션의 송신자가 될 수 있습니다.

타당한 주소를 생성하는 것은 키 쌍을 가지고 하는 딱딱한 수학적인 작업입니다. 사용자는 키 쌍을 만들고 이것의 주소를 생성하는 일을 완전히 오프라인 환경에서 분산원장 혹은 다른 집단과의 통신없이 진행할 수 있습니다. 공개 키로부터 주소를 얻는 것은 일-방향 해쉬 함수를 포함하므로, 공개 키가 주소와 매치가 되는 것인지 확인할 수 있습니다. 하지만, 주소만으로 공개 키를 도출할 수는 없습니다.( 이 내용은 왜 서명된 트랜잭션이 공개 키와 송신자의 주소를 포함하는지에 대한 이유파트입니다.)

## 특별 주소(Special Addresses)

몇 가지 주소는 분산원장에서 특별한 의미를 가지고 있거나, 역사적으로 사용되고 있습니다. 많은 경우, 이 주소들은 "블랙홀" 주소인데, 이 주소들은 알려진 비밀키에서 도출되지 않는 주소입니다. 주소만으로는 비밀 키를 추측하는 것이 사실상 불가능하므로 블랙홀 주소가 소유한 모든 XRP는 영원히 손실됩니다.

<table><thead><tr><th width="403.3333333333333">주소</th><th>이름</th><th width="166">의미</th><th>블랙홀인가?</th></tr></thead><tbody><tr><td><mark style="background-color:yellow;"><strong>rrrrrrrrrrrrrrrrrrrrrhoLvTp</strong></mark></td><td>ACCOUNT_ZERO</td><td><mark style="background-color:yellow;">0</mark>에 분산원장의 base58 인코딩을 실행한 주소입니다. P2P 통신에서, <mark style="background-color:yellow;">rippled</mark>는 이 주소를 XRP의 발행자 주소로 사용합니다.</td><td>Yes</td></tr><tr><td><mark style="background-color:yellow;"><strong>rrrrrrrrrrrrrrrrrrrrBZbvji</strong></mark></td><td>ACCOUNT_ONE</td><td><mark style="background-color:yellow;">1</mark>에 분산 원장의 base58 인코딩을 실행한 주소입니다. 분산원장에서, <a href="../../references/xrp-ledger-xrp-ledger-protocol-reference/ledger-ledger-data-formats/ledger/ripplestate.md">RippleStateentries</a>는 이 주소를 신뢰선의 잔액 필드에서 발행자 멤버의 자리표시자로 사용합니다.</td><td>Yes</td></tr><tr><td><mark style="background-color:yellow;"><strong>rHb9CJAWyB4rj91VRWn96DkukG4bwdtyTh</strong></mark></td><td>The genesis account</td><td>rippled가 새로운 gensis 원장을 처음부터 시작할 때(예를 들면, 독립 실행 모드에서), 이 계정은 모든 XRP를 보유합니다<a href="https://github.com/XRPLF/rippled/blob/94ed5b3a53077d815ad0dd65d490c8d37a147361/src/ripple/app/ledger/Ledger.cpp#L184">. 이 주소는 하드코딩  된 </a> masterpassphrase의 시드로부터 생성되었습니다.</td><td>No</td></tr><tr><td><mark style="background-color:yellow;"><strong>rrrrrrrrrrrrrrrrrNAMEtxvNvQ</strong></mark></td><td>Ripple Name reservation black-hole</td><td>과거에, 리플은 Ripple Names를 예약하기 위해 이 계정에 XRP를 송금하라 요청했습니다.</td><td>Yes</td></tr><tr><td><mark style="background-color:yellow;"><strong>rrrrrrrrrrrrrrrrrrrn5RM1rHd</strong></mark></td><td>NaN Address</td><td><a href="https://github.com/XRPLF/xrpl.js">ripple-lib</a>의 이전 버전에서 <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/NaN">NaN</a>값에 분산 원장의 <a href="../../references/xrp-ledger-xrp-ledger-protocol-reference/basic-data-types/base58-base58-encodings.md">base58 </a>인코딩을 실행해서 이 주소를 생성했습니다.</td><td>Yes</td></tr></tbody></table>

## 주소 인코딩(Address Encoding)

{% hint style="success" %}
팁:

이 기술적인 디테일들은 오직 분산원장과의 호환성을 위한low-level library 소프트웨어 개발자들과만 연관이 있습니다!
{% endhint %}

분산 원장의 주소는                                                                                                                  [\[Source\] ](https://github.com/XRPLF/rippled/blob/35fa20a110e3d43ffc1e9e664fc9017b6f2747ae/src/ripple/protocol/impl/AccountID.cpp#L109-L140)<mark style="background-color:yellow;">rpshnaf39wBUDNEGHJKLM4PQRST7VWXYZ2bcdeCg65jkm8oFqi1tuvAxyz</mark> 딕셔너리를 사용하는[ base58](../../references/xrp-ledger-xrp-ledger-protocol-reference/basic-data-types/base58-base58-encodings.md)을 사용하여 인코딩됩니다. 분산원장이 여러가지 타입의 키를 base58을 이용하여 인코딩하기 때문에, 각 타입들을 구분하기 위해서 인코딩된 데이터에 1byte 크기의 "type 지정자"(버전 지정자라고도 부릅니다.)를 붙입니다. 타입 지정자는 보통의 경우 base58 형식에서 주소들이 각기 다른 문자로 시작하도록 유도합니다.

다음 다이어그램은 키와 주소 사이의 관계를 설명합니다.

<figure><img src="../../.gitbook/assets/image (24) (1).png" alt=""><figcaption></figcaption></figure>

공개 키로부터 분산 원장의 주소를 계산하는 공식은 다음과 같습니다. 완전한 예시 코드를 보려면,[ encode\_address.js](https://github.com/XRPLF/xrpl-dev-portal/blob/master/content/\_code-samples/address\_encoding/js/encode\_address.js)를 보세요. 공개 키로부터 seed value 혹은 passphrase를 도출하는 과정을 보려면 [키 파생](cryptographic-keys.md)을 참고하세요.

&#x20;   1\. 필요한 알고리즘들을 import하세요: SHA-256, RIPEMD160, base58.   &#x20;

```javascript
'use strict';
const assert = require('assert');
const crypto = require('crypto');
const R_B58_DICT = 'rpshnaf39wBUDNEGHJKLM4PQRST7VWXYZ2bcdeCg65jkm8oFqi1tuvAxyz';
const base58 = require('base-x')(R_B58_DICT);

assert(crypto.getHashes().includes('sha256'));
assert(crypto.getHashes().includes('ripemd160'));
```

&#x20;   2\. 33-byte ECDSA secp256k1 공개 키 혹은 32-byte Ed25519 공개키를 사용해서 시작합니다. Ed25519에 대해서는 앞에 1byte <mark style="background-color:yellow;">0xED</mark>로 고정합니다.

```javascript
const pubkey_hex =
  'ED9434799226374926EDA3B54B1B461B4ABF7237962EAE18528FEA67595397FA32';
const pubkey = Buffer.from(pubkey_hex, 'hex');
assert(pubkey.length == 33);
```

&#x20;   3\. 공개키에 SHA-256 를 적용하여 얻은 해시값에 RIPEMD160 연산을 하여 해시를 얻습니다. 이 값은 "계정 ID"입니다.

```javascript
const pubkey_inner_hash = crypto.createHash('sha256').update(pubkey);
const pubkey_outer_hash = crypto.createHash('ripemd160');
pubkey_outer_hash.update(pubkey_inner_hash.digest());
const account_id = pubkey_outer_hash.digest();
```

&#x20;   4\. 계정 ID에 SHA-256을 두번 연산하여 해시를 얻습니다. 그리고 앞자리 4 byte를 가져옵니다. 이 값은 "체크섬"입니다.

```javascript
const address_type_prefix = Buffer.from([0x00]);
const payload = Buffer.concat([address_type_prefix, account_id]);
const chksum_hash1 = crypto.createHash('sha256').update(payload).digest();
const chksum_hash2 = crypto.createHash('sha256').update(chksum_hash1).digest();
const checksum =  chksum_hash2.slice(0,4);
```

&#x20;   5\. payload와 체크섬을 연결합니다. concatenated buffer에 대해 base58 변환을 수행합니다. 이 결과값이 주소입니다.

```javascript
const dataToEncode = Buffer.concat([payload, checksum]);
const address = base58.encode(dataToEncode);
console.log(address);
// rDTXLQ7ZKZVKz33zJbHjgVShjsBnqMBhmN
```
