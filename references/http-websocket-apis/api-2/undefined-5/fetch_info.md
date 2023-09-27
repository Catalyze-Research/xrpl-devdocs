# fetch\_info

ㅐfetch\_info 명령은 이 서버가 현재 네트워크에서 가져오는 객체에 대한 정보와 해당 정보를 가지고 있는 피어 수를 반환합니다. 또한 현재 가져오기를 재설정하는 데 사용할 수도 있습니다.

fetch\_info 메소드는 권한이 없는 사용자가 실행할 수 없는 관리자 메소드입니다.

## 요청 형식

요청 형식의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": 91,
    "command": "fetch_info",
    "clear": false
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "fetch_info",
    "params": [
        {
            "clear": false
        }
    ]
}
```
{% endtab %}

{% tab title="Commandline" %}
```
#Syntax: fetch_info [clear]
rippled fetch_info
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| `Field` | 유형      | 설명                                                         |
| ------- | ------- | ---------------------------------------------------------- |
| `clear` | Boolean |  `true`인 경우 현재 가져오기를 재설정합니다. 그렇지 않으면 진행 중인 가져오기 상태만 가져옵니다. |

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="JSON-RPC" %}
```json
{
   "result" : {
      "info" : {
         "348928" : {
            "hash" : "C26D432B06F84861BCACD7942EDC3FE0B2E1DEB966A9E516A0FD275A375C2010",
            "have_header" : true,
            "have_state" : false,
            "have_transactions" : true,
            "needed_state_hashes" : [
               "BF8DC6B1E10D1D3565BF0649075D22EBFD34F751AFCC0E53E81D74786BC88922",
               "34E37A71CB51A12C73A435250E6A6349F7884C7EEBA6B88FA31F0244E967E88F",
               "BFB7D3008A7D61FD6A0538D1C2E70CFB94CE8DC66606319C372F278A48629765",
               "41C0C61D701FB1EA586F0EF1FC7A91FEC476D979589DA60507F05C13F7C21975",
               "6DDE8840A2C3C7FF05E5FFEE4D06408694C16A8357338FE0C4581DC3D8A00BBA",
               "6C69D833B582C849917806FA009518832BB50E900E43716FD7CC1966428DD0CF",
               "1EDC020CFC4AF19B625C52E20B66D6AE672821CCC461E8A9C457A3B2955657F7",
               "FC0616A66A2B0589CA513F3341D4EA51E782C4601E5072308478E3CC19264640",
               "19FC607B5DE1B64681A676EC1ED5507B9555B0E098CD9D898320297DE1A64033",
               "5E128D3FC990074E35687387A14AA12D9FD287E5AB57CB9B2FD83DE635DF5CA9",
               "DE72820F3981770F2AA8770BC233B80661F1A452819D8529008875FF8DED87A9",
               "3ACB84BEE2C45556351FF60FD787D235C9CF5623FB8A35B01446B773598E7CC0",
               "0DD3A8DF69874148057F1F2BF305442FF2E89A76A08B4CC8C051E2ED69B874F3",
               "4AE9A9C4F12A5BD0355037DA40A0B145420A2168A9FEDE43E643BD13062F8ECE",
               "08CBF8CFFEC207F5AC4E4F24BC447011FD8C79D25B344281FBFB4732D7058ED4",
               "779B2577C5C4BAED6657421448EA506BBF50F86BE363E0924127C4EA17A58BBE"
            ],
            "peers" : 2,
            "timeouts" : 0
         }
      },
      "status" : "success"
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
      "info" : {
         "348928" : {
            "hash" : "C26D432B06F84861BCACD7942EDC3FE0B2E1DEB966A9E516A0FD275A375C2010",
            "have_header" : true,
            "have_state" : false,
            "have_transactions" : true,
            "needed_state_hashes" : [
               "BF8DC6B1E10D1D3565BF0649075D22EBFD34F751AFCC0E53E81D74786BC88922",
               "34E37A71CB51A12C73A435250E6A6349F7884C7EEBA6B88FA31F0244E967E88F",
               "BFB7D3008A7D61FD6A0538D1C2E70CFB94CE8DC66606319C372F278A48629765",
               "41C0C61D701FB1EA586F0EF1FC7A91FEC476D979589DA60507F05C13F7C21975",
               "6DDE8840A2C3C7FF05E5FFEE4D06408694C16A8357338FE0C4581DC3D8A00BBA",
               "6C69D833B582C849917806FA009518832BB50E900E43716FD7CC1966428DD0CF",
               "1EDC020CFC4AF19B625C52E20B66D6AE672821CCC461E8A9C457A3B2955657F7",
               "FC0616A66A2B0589CA513F3341D4EA51E782C4601E5072308478E3CC19264640",
               "19FC607B5DE1B64681A676EC1ED5507B9555B0E098CD9D898320297DE1A64033",
               "5E128D3FC990074E35687387A14AA12D9FD287E5AB57CB9B2FD83DE635DF5CA9",
               "DE72820F3981770F2AA8770BC233B80661F1A452819D8529008875FF8DED87A9",
               "3ACB84BEE2C45556351FF60FD787D235C9CF5623FB8A35B01446B773598E7CC0",
               "0DD3A8DF69874148057F1F2BF305442FF2E89A76A08B4CC8C051E2ED69B874F3",
               "4AE9A9C4F12A5BD0355037DA40A0B145420A2168A9FEDE43E643BD13062F8ECE",
               "08CBF8CFFEC207F5AC4E4F24BC447011FD8C79D25B344281FBFB4732D7058ED4",
               "779B2577C5C4BAED6657421448EA506BBF50F86BE363E0924127C4EA17A58BBE"
            ],
            "peers" : 2,
            "timeouts" : 0
         }
      },
      "status" : "success"
   }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| Field | 유형 | 설명                                                                                     |
| ----- | -- | -------------------------------------------------------------------------------------- |
| info  | 객체 | 가져오는 객체의 맵과 가져오는 객체의 상태. 가져오는 원장은 원장 인덱스로 식별할 수 있으며, 가져오는 원장 및 기타 객체는 해시로도 식별할 수 있습니다. |

진행 중인 가져오기를 설명하는 필드는 예고 없이 변경될 수 있습니다. 다음 필드가 포함될 수 있습니다:

| Field                 | 유형          | 설명                                                               |
| --------------------- | ----------- | ---------------------------------------------------------------- |
| hash                  | 문자열         | 가져올 항목의 해시입니다.                                                   |
| have\_header          | Boolean     | 원장의 경우, 이 서버가 원장의 헤더 섹션을 이미 가져왔는지 여부입니다.                         |
| have\_transactions    | Boolean     | 원장의 경우, 이 서버가 해당 원장의 트랜잭션 섹션을 이미 가져왔는지 여부입니다.                    |
| needed\_state\_hashes | (해시) 문자열 배열 | 이 항목에서 아직 필요한 상태 개체의 해시 값입니다. 16개 이상이 필요한 경우 응답에는 처음 16개만 포함됩니다. |
| peers                 | 숫자          | 이 항목을 사용할 수 있는 피어 수입니다.                                          |
| timeouts              | 숫자          | 이 항목을 가져올 때 시간 초과가 발생한 횟수(2.5초)입니다.                              |

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* reportingUnsupported - (리포팅 모드 서버만 해당) 이 메소드는 리포팅 모드에서 사용할 수 없습니다.
