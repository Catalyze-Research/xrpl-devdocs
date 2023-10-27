# 주소(Addresses)

분산 원장에서 계정은 [base58](../../references/xrp-ledger/undefined/base58.md) 포맷을 통해 생성된 주소를 통해 고유성을 가진다. 주소는 계정 주인의 공개키를 통해 도출되며, 그 공개키는 또한 비밀키를 통해 도출된다. 주소는 JSON 형식에서 문자열로 표현된다. 그리고 다음과 같은 특성을 가진다.

* 25\~35 길이의 문자열이다.
* 문자 <mark style="background-color:yellow;">r</mark>로 시작한다.
* &#x20;alphanumeric 숫자를 사용하며, 숫자 "0", 대문자 "O", 대문자 "$$I$$", 소문자 "l"은 제외한다.(base58)
* 대소문자를 구분한다.
* $$1/2^{32}$$ 에 근사한 확률로 검출되는 유효      한 주소인지 구분하기 위해 4byte의 체크섬을 포함한다.&#x20;

{% hint style="info" %}
X-주소 형식은 주소에 목적지 태그를 "함축"하고 있습니다. 이 주소는 X(메인넷 기) 또는 T(테스트넷 기준)로 시작합니다. 이런 하나의 값을 통해 소비자가 알아야할 모든 정보를 표현하기 위해 거래소나 지갑은 X-주소 형식을                                    사용할 수 있습니다. 더 많은 정보를 얻으려면 [X-주소 형식 ](https://xrpaddress.info/)그리고 [코덱](https://github.com/xrp-community/xrpl-tagged-address-codec)을 참고하세요.

&#x20;                                                                                                                                                                   분산원장 프로토콜은 기본적으로 오직 "클래식" 주소들만 허용하지만, 많은 [클라이언트 라이브러리](../../references/undefined/)는 X-주소 또한 지원합니다.
{% endhint %}

더 많은 정보를 얻으려면 [계정](./) 그리고[ base58 인코딩](../../references/xrp-ledger/undefined/base58.md)을 참고하세요.
