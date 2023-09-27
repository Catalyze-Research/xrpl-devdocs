# ledger

ledger 명령은 공개 ledger에 대한 정보를 검색합니다. [![New in: Clio v1.0.0](https://img.shields.io/badge/New%20in-Clio%20v1.0.0-blue.svg)](https://github.com/XRPLF/clio/releases/tag/1.0.0)

클리오 서버는 기본적으로 검증된 ledger 데이터를 반환합니다.

## 요청 형식

요청 형식의 예시입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": 14,
    "command": "ledger",
    "ledger_index": "validated",
    "full": false,
    "accounts": false,
    "transactions": false,
    "expand": false,
    "owner_funds": false,
    "diff": false
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "ledger",
    "params": [
        {
            "ledger_index": "validated",
            "accounts": false,
            "full": false,
            "transactions": false,
            "expand": false,
            "owner_funds": false,
            "diff": false
        }
    ]
}
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함될 수 있습니다:

| Field         | 유형              | 설명                                                                                                                                                                                |
| ------------- | --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ledger\_hash  | 문자열             | (선택 사항) 사용할 원장 버전에 대한 20바이트 16진수 문자열입니다. ([원장 지정](https://xrpl.org/basic-data-types.html#specifying-ledgers) 참조)                                                                  |
| ledger\_index | 문자열 또는 부호 없는 정수 | (선택 사항) 사용할 원장의 [원장 인덱스](https://xrpl.org/basic-data-types.html#ledger-index) 또는 자동으로 원장을 선택하는 단축 문자열입니다. ([원장 지정](https://xrpl.org/basic-data-types.html#specifying-ledgers) 참조) |
| transactions  | Boolean         | (선택 사항) true인 경우 지정된 원장 버전의 거래 정보를 반환합니다. 기본값은 false 입니다. 원장 버전을 지정하지 않은 경우 무시됩니다.                                                                                                |
| expand        | Boolean         | (선택 사항) 거래/계좌 정보에 대해 해시만 제공하는 대신 JSON 형식의 전체 정보를 제공합니다. 기본값은 false. 거래, 계정 또는 둘 다를 요청하지 않는 한 무시됩니다.                                                                               |
| owner\_funds  | Boolean         | (선택 사항) true인 경우 오퍼 크리에이트 트랜잭션의 메타데이터에 소유자\_자금 필드를 응답에 포함시킵니다. 기본값은 false입니다. 트랜잭션이 포함되지 않고 expand가 true인 경우 무시됩니다.                                                               |
| binary        | Boolean         | (선택 사항) true이고 트랜잭션과 확장도 모두 true인 경우 트랜잭션 정보를 JSON 형식 대신 이진 형식(16진수 문자열)으로 반환합니다.                                                                                                 |
| queue         | Boolean         | (선택 사항) 참이고 명령이 현재 원장을 요청하는 경우, 대기 중인 트랜잭션 배열을 결과에 포함합니다.                                                                                                                         |
| diff          | Boolean         | (선택 사항) 참이면 지정된 원장에 트랜잭션을 적용하는 과정에서 추가, 수정 또는 삭제된 모든 객체를 반환합니다.                                                                                                                   |

ledger 필드는 더 이상 사용되지 않으며 추가 공지 없이 제거될 수 있습니다.

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": 1,
    "result": {
        "ledger": {
            "accepted": true,
            "account_hash": "10EFE192F59B3DE2A2BE5BCE2CA5DC83D066105696FCFC24C055359AAEBD6941",
            "close_flags": 0,
            "close_time": 711134782,
            "close_time_human": "2022-Jul-14 17:26:22.000000000 UTC",
            "close_time_resolution": 10,
            "closed": true,
            "hash": "D3878EF6C92B84678AE2FBADC40961A161A128EA54AE59C2775CE076C2AE7A85",
            "ledger_hash": "D3878EF6C92B84678AE2FBADC40961A161A128EA54AE59C2775CE076C2AE7A85",
            "ledger_index": "19977716",
            "parent_close_time": 711134781,
            "parent_hash": "D6DE54039FE5A22D86CD522F1A9B7794E487B74D9B6B8CBDE23F240F434B6749",
            "seqNum": "19977716",
            "totalCoins": "99987079398940307",
            "total_coins": "99987079398940307",
            "transaction_hash": "0000000000000000000000000000000000000000000000000000000000000000"
        },
        "ledger_hash": "D3878EF6C92B84678AE2FBADC40961A161A128EA54AE59C2775CE076C2AE7A85",
        "ledger_index": 19977716,
        "validated": true
    },
    "status": "success",
    "type": "response",
    "warnings": [
        {
            "id": 2001,
            "message": "This is a clio server. clio only serves validated data. If you want to talk to rippled, include ledger_index:current in your request"
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
200 OK

{
    "result": {
        "ledger": {
            "accepted": true,
            "account_hash": "10EFE192F59B3DE2A2BE5BCE2CA5DC83D066105696FCFC24C055359AAEBD6941",
            "close_flags": 0,
            "close_time": 711134782,
            "close_time_human": "2022-Jul-14 17:26:22.000000000 UTC",
            "close_time_resolution": 10,
            "closed": true,
            "hash": "D3878EF6C92B84678AE2FBADC40961A161A128EA54AE59C2775CE076C2AE7A85",
            "ledger_hash": "D3878EF6C92B84678AE2FBADC40961A161A128EA54AE59C2775CE076C2AE7A85",
            "ledger_index": "19977716",
            "parent_close_time": 711134781,
            "parent_hash": "D6DE54039FE5A22D86CD522F1A9B7794E487B74D9B6B8CBDE23F240F434B6749",
            "seqNum": "19977716",
            "totalCoins": "99987079398940307",
            "total_coins": "99987079398940307",
            "transaction_hash": "0000000000000000000000000000000000000000000000000000000000000000"
        },
        "ledger_hash": "D3878EF6C92B84678AE2FBADC40961A161A128EA54AE59C2775CE076C2AE7A85",
        "ledger_index": 19977716,
        "validated": true
    },
     "status": "success",
    "type": "response",
    "warnings": [
        {
            "id": 2001,
            "message": "This is a clio server. clio only serves validated data. If you want to talk to rippled, include ledger_index:current in your request"
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

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드를 포함하여 ledger에 대한 정보가 포함됩니다:

| Field                          | 유형      | 설명                                                                                                                                                                                |
| ------------------------------ | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ledger                         | 객체      | 이 원장의 전체 헤더 데이터입니다.                                                                                                                                                               |
| ledger.account\_hash           | 문자열     | 이 원장에 있는 모든 계정 상태 정보의 해시(16진수)                                                                                                                                                    |
| ledger.accountState            | 배열      | (요청하지 않는 한 생략) 이 원장의 모든 계정 상태 정보입니다.                                                                                                                                              |
| ledger.close\_flags            | 정수      | 이 원장의 마감과 관련된 플래그 의 비트맵입니다.                                                                                                                                                       |
| ledger.close\_time             | 정수      | Ripple 에포크 이후 이 원장이 닫힌 시간(초)입니다.                                                                                                                                                  |
| ledger.close\_time\_human      | 문자열     | 이 원장이 닫힌 시간으로, 사람이 읽을 수 있는 형식입니다. 항상 UTC 시간대를 사용합니다.                                                                                                                              |
| ledger.close\_time\_resolution | 정수      | 원장 마감 시간은 이 초 이내로 반올림됩니다.                                                                                                                                                         |
| ledger.closed                  | Boolean | 이 원장의 폐쇄 여부                                                                                                                                                                       |
| ledger.ledger\_hash            | 문자열     | 전체 원장의 고유 식별 해시입니다.                                                                                                                                                               |
| ledger.ledger\_index           | 문자열     | 인용된 정수로 표시된 이 원장의 [원장](https://xrpl.org/basic-data-types.html#ledger-index) 색인                                                                                                    |
| ledger.parent\_close\_time     | 정수      | 이전 원장이 마감된 시간입니다.                                                                                                                                                                 |
| ledger.parent\_hash            | 문자열     | 이 원장 바로 앞에 나온 원장의 고유 식별 해시입니다.                                                                                                                                                    |
| ledger.total\_coins            | 문자열     | 네트워크의 총 XRP 드롭 수(인용된 정수) (이는 거래 비용이 XRP를 파괴함에 따라 감소합니다.)                                                                                                                          |
| ledger.transaction\_hash       | 문자열     | 이 원장에 포함된 거래 정보의 해시(16진수)                                                                                                                                                         |
| ledger.transactions            | 배열      | (요청하지 않는 한 생략) 이 원장 버전에 적용된 트랜잭션입니다. 기본적으로 멤버는 트랜잭션의 식별 해시 문자열입니다. 요청에서 확장을 참으로 지정한 경우, 요청에서 바이너리를 참으로 지정했는지 여부에 따라 멤버가 대신 트랜잭션의 전체 표현(JSON 또는 바이너리)이 됩니다.                        |
| ledger\_hash                   | 문자열     | 전체 원장의 고유 식별 해시입니다.                                                                                                                                                               |
| ledger\_index                  | 숫자      | 이 원장의 [원장 색인입니다](https://xrpl.org/basic-data-types.html#ledger-index).                                                                                                            |
| validated                      | Boolean | (생략 가능) true인 경우 검증된 원장 버전입니다. 생략하거나 로 설정하면 false이 원장의 데이터가 최종 데이터가 아닙니다.                                                                                                         |
| queue\_data                    | 배열      | (queue 매개변수로 요청하지 않는 한 생략) 큐에 대기 중인 트랜잭션을 큐와 동일한 순서로 설명하는 객체 배열입니다. 요청이 확장됨을 참으로 지정한 경우, 멤버는 요청에서 바이너리를 참으로 지정했는지 여부에 따라 트랜잭션의 전체 표현을 JSON 또는 바이너리로 포함합니다. 수수료 확장 개정에 의해 추가되었습니다. |
| diff                           | 객체      | (diff 매개변수와 함께 요청하지 않는 한 생략) 원장에 트랜잭션을 적용하는 과정에서 추가, 수정 또는 삭제된 해시 배열을 포함하는 객체입니다.                                                                                                 |

다음 필드는 더 이상 사용되지 않으며 추가 공지 없이 제거될 수 있습니다: accepted, hash(대신 ledger\_hash 사용), seqNum(대신 ledger\_index 사용), totalCoins(대신 total\_coins 사용).

queue\_data 배열의 각 멤버는 대기열에 있는 트랜잭션 하나를 나타냅니다. 이 객체의 일부 필드는 아직 계산되지 않았기 때문에 생략될 수 있습니다. 이 객체의 필드는 다음과 같습니다:

| 필드                 | 값         | 설명                                                                                                                                                                                                                                                                              |
| ------------------ | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| account            | 문자열       | 이 대기 중인 트랜잭션에 대한 보낸 사람의 주소[입니다.](https://xrpl.org/basic-data-types.html#addresses)                                                                                                                                                                                              |
| tx                 | 문자열 또는 객체 | 기본적으로 이는 트랜잭션의 [식별 해시가](https://xrpl.org/basic-data-types.html#hashes) 포함된 문자열입니다. tx\_blob트랜잭션이 이진 형식으로 확장된 경우 이는 트랜잭션의 이진 형식을 10진수 문자열로 포함하는 필드만 있는 개체입니다. 트랜잭션이 JSON 형식으로 확장된 경우 필드 에 트랜잭션 식별 해시를 포함하는 [트랜잭션 개체가](https://xrpl.org/transaction-formats.html) hash 포함된 개체입니다. |
| retries\_remaining | 숫자        | 트랜잭션이 삭제되기 전에 재시도할 수 있는 횟수입니다.                                                                                                                                                                                                                                                  |
| preflight\_result  | 문자열       | 예비 트랜잭션 확인의 잠정 결과입니다. 이 값은 항상 tesSUCCESS입니다.                                                                                                                                                                                                                                    |
| last\_result       | 문자열       | (생략 가능) 이 트랜잭션이 재시도 가능한(ter) 결과를 받은 후 대기열에 남아 있는 경우, 이 트랜잭션이 받은 정확한 ter 결과 코드입니다.                                                                                                                                                                                               |
| auth\_change       | Boolean   | (생략 가능) 이 트랜잭션이 이 주소의 트랜잭션 승인 방식을 변경하는지 여부입니다.                                                                                                                                                                                                                                  |
| fee                | 문자열       | (생략 가능) 이 트랜잭션의 트랜잭션 비용(XRP 드롭 단위)입니다.                                                                                                                                                                                                                                          |
| fee\_level         | 문자열       | (생략 가능) 이 트랜잭션의 트랜잭션 비용(해당 트랜잭션 유형의 최소 비용과 비교하여 수수료 수준 단위로 표시).                                                                                                                                                                                                                 |
| max\_spend\_drops  | 문자열       | (생략 가능) 이 트랜잭션이 잠재적으로 전송하거나 소멸할 수 있는 최대 XRP 금액(방울 단위)입니다.                                                                                                                                                                                                                       |

요청에 "owner\_funds": true 및 확장 트랜잭션이 지정된 경우, 응답에는 각 [OfferCreate](https://xrpl.org/offercreate.html) 트랜잭션의 메타데이터 객체에 소유자 자금 필드가 있습니다. 이 필드의 목적은 유효성이 검증된 새로운 ledger가 추가될 때마다 오퍼의 펀딩 상태를 더 쉽게 추적할 수 있도록 하기 위한 것입니다. 이 필드는 오더북 가입 스트림에서 이 필드의 버전과 약간 다르게 정의됩니다:

| Field        | 값   | 설명                                                                                                                                         |
| ------------ | --- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| owner\_funds | 문자열 | 이 원장의 모든 트랜잭션이 실행된 후 이 OfferCreate 트랜잭션을 보내는 TakerGets통화 의 숫자 금액입니다. Account통화 금액이 [동결](https://xrpl.org/freezes.html) 되었는지 여부는 확인하지 않습니다. |

요청에 "diff: true"가 지정되면 응답에 객체 diff가 있습니다. 이 객체의 필드는 다음과 같습니다:

| Field      | 값              | 설명                                                                                                |
| ---------- | -------------- | ------------------------------------------------------------------------------------------------- |
| object\_id | 문자열            | 객체 식별자입니다.                                                                                        |
| Hashes     | 객체 또는 16진수 문자열 | 요청이 binarytrue 또는 false로 설정되었는지 여부에 따라 이 필드는 생성된 개체의 콘텐츠, 수정된 개체의 새 값 또는 개체가 삭제된 경우 빈 문자열을 반환합니다. |

## diff가 참인 경우 응답

```json
{
    "result": {
        "ledger": {
            "accepted": true,
            "account_hash": "29E8799E80245DA68AE2D9C4A454DF3E8FEF5B66C520E644DE0756E548C6897B",
            "close_flags": 0,
            "close_time": 711135301,
            "close_time_human": "2022-Jul-14 17:35:01.000000000 UTC",
            "close_time_resolution": 10,
            "closed": true,
            "hash": "C769F00409D574A982DC0CC071A244C5507D9E2269404957B78081ED8F2FCB1C",
            "ledger_hash": "C769F00409D574A982DC0CC071A244C5507D9E2269404957B78081ED8F2FCB1C",
            "ledger_index": "19977887",
            "parent_close_time": 711135300,
            "parent_hash": "29BAC714E9BB7DFE9BE3C8B4AACFDB184389388340A4F3B09CC61A39BAFC3705",
            "seqNum": "19977887",
            "totalCoins": "99987079398939527",
            "total_coins": "99987079398939527",
            "transaction_hash": "0000000000000000000000000000000000000000000000000000000000000000",
            "diff": [
                {
                    "object_id": "B4979A36CDC7F3D3D5C31A4EAE2AC7D7209DDA877588B9AFC66799692AB0D66B",
                    "object": {
                        "Flags": 0,
                        "Hashes": [
                            "A17FED77F200BF8FC66050BCFCC87241DB53C418B3B3C8794C857AAAB76C21EB",
                            "F991F764BA7B44B5B28F7AC8F03F2BC03B1647AD355CE0F0F91D3B500F862959",
                            "06523FF3694A078F2DD073A728145455EB451651180C6B0EED1557B657C6C07D",
                            "69D5C7A206A34CC4A1D21A9D2453E6D72526EA0DA712009F215AA614C259A333",
                            "94453F6FA048C153A7124C249A7C3393D1A3B4E34A508872A3AA4FE89021221C",
                            "406FABC4A1C9A30300C814F6B083C00F16568840BBFDCA04369A545CF248472C",
                            "9396907A543546ECF48C6417373705514653ACB4E585C009EEF525B51CE1F155",
                            "14082ABB45AFED62BFCDA3CD0465BD17545281C3D671DEC82A837CBEE36E470A",
                            "1D4D19B06024BE49333EA6C010F9721C5C1A36A5C08D22160945247D3FF9F552",
                            "91095028948A56A16D8E17488057C27BED696B8E359BAB0D7B8510BBD5F0CFB7",
                            "FC073AAF3B07260545DCAF0BA0FCFBF7673302D972A534CC8EAB1CD5C4CC73EE",
                            "4DB17A22F0857992E2BA75C4C15C39B0D420EA26AB6FA862B54E2D65C739117B",
                            "CB44073EB0A174C4757F453335A5A84848AD899AD09574883946F37DCDEA90B8",
                            "0B285623C693432532418BA783CF6D731CAFFC312566398B27B67C1B856AACE1",
                            "5024CD26CE82294E15656CCA0E350284ED6EF70DBD4E4791017F3E4B396D42F5",
                            "F0E4B4331D5B5B9BB1B43232C8B50E9EB17CD4F6817B04C3AC263C773B44E222",
                            "7CE5AA21BAEF20DCA4384D469451F6B9BA7AFBA265FCA83B48045C7D994E8BE8",
                            "91F8A45B27C8DC825157633ED5AE3435F51D5325D29F2F8547D3849F59812CDD",
                            "520B119EF4ECF9AC3F1B247621A04F0DC187CA6D756B6614E6BCE05A22839C8C",
                            "EE87D8EB08BCE989C87D530EE4497F5B59B82F859C687E49C6629F34623CF4AE",
                            "965933B82140D8BC04C324E075AB3758F82F17BE610D12306614A4B1F3695270",
                            "EA1EF4CECA2EBABBA5A09041C186352B3A57F4496676DF0E4E743794CF2D7ED5",
                            "A261123CFC1675958992C604778535208FE9C783AA77107393EC1E290D7D3ECF",
                            "5F3F22362E969A5E7479BB4A9FD31BCFBCCDB66741F49CB72755BC2B76A1D310",
                            "1BF9D357835E4D8FA82AE99FA13F389F502401E59600F2B72E4B1CB703A54363",
                            "59A17903D6C8FF3F36C327E207AB54293163A8AA4A3DD515F2C677B736396BAB",
                            "308905A9E623788DE1170B396AD379863D5492B490D6D56D68F3DDA41CED7F39",
                            "B686A8CBD0B5464C74B7D7E3AD04D055E59C7FB558B4F65D61FDC005AB91950D",
                            "81831141523FDEEEF91BA883B0B394D8D608C2353936123F88B4C27F823E5F32",
                            "DE2BE267BF14EBB26696070989BD229603985CDA105B186C2987C6CC9A0CB83E",
                            "E54358313465A1701F94806969E07D392AA5E9A6DE99A37E4328B9C9736630C1",
                            "02190060960D241F3182D4E8CC154D2DAA8224CE3CC1DCDB1E21CAC65B56D9B0",
                            "B02916AF6E46353828988E785C2F0A26F8CD6196F5905AB4DEDF7034432FBC3E",
                            "3E95827426FC91549A8E9A80D6CC99997646F6B26CD3F625D94064BA16DE97E1",
                            "1B83060E2DE91E889FFCE8D0DAEC810197C026EE0F037E612732639EDED3EBCD",
                            "A8EC7EE25A6B6C13AC6A006F3397EFC8FE38829EF273469186CABE5B529A40EC",
                            "B00AA09BC4A6180967DD6BA88BDB13F5F59EA4E9C074B752A04DF7C515DF8504",
                            "45CD713F10A5ECD06B32A2B15E63FEF211852E8BD56B813362D16C7413948785",
                            "A749FC031CC69E10476B7AD751B068F32DB9192C9CBCBD7CDB018FCF5C1B0934",
                            "28BC94EB8BC5A0A3313CD825FFC824454ED64C60988D530B334C3112621005AA",
                            "18DA83251BF498CEFDD189B7894E70ED2BDE832183C4C846B4D6E886B0749CD6",
                            "4DB4B88C5F4638816E06D880E55DD83C61747D0235C96E1CE1FD709777C20BEA",
                            "CED44414A9AA99BFD709B7A9072170CB2040EF31D5E84552A930E02CAAB2301C",
                            "9566A3A6E02FCA0648B67CDACE941BCA7D243D1E2F18742E50EE83C1C8950A19",
                            "0100F9B48D0999BEBBD18500036741B34E7FC395F155FFA81263EB772C2E1178",
                            "867719FDA972F549F2149341FE8089A09DFA590C55B7F682A4BDEAEC14A32EEC",
                            "12C96E5C7D29649AA86FF17969B13978DCBA004F691D476A8AA88EFDA318E5C1",
                            "8001BD1ED9A0772DA5C89144B56B2504F90329C06E05F4052862EA5560021448",
                            "D3F85FA0E6B948F05578B99306247B569548A976773DBD0A7811CC8FD62A5033",
                            "A2B15A5E7B8C3912C7FE9BB93B98333B00AB82322F9759CC43ED6D7BC25F48DF",
                            "3C7D539FB2D34B20B3BBFFBEA05738560A3811AC496BCB7D504041250D4C393B",
                            "D5B91302F8146E5BC0FAAF08BE2C228CCC17EF192A423CCEDE1A38A604D9E842",
                            "F4DBF016BA3836F3B6B5971D1308C04C531FADBD6E74EDB03822851D8C1D5A84",
                            "EDA6E3DE8138D6129B3A11EBE98F676F66943158E2ADB0E5E548EFCA124C7F7D",
                            "19EBC639A176F903A739C1C375C240ED5EB58B5D1FB6DF15B3CE4AD6B04689B8",
                            "E015FE6FCE6C587605A7918ABC25CB37BFABCD7D49A2C7EBDCABC490AEDD90AB",
                            "9684F16CA2DE7C8056F5125D2AF167E8D3CB5FD8F9A12D87D9D60CE46F098BB3",
                            "EA9160D3B05F0DFF835AE2AF054BA0FED3DE6A381E11AED991E0850EA1D28A7B",
                            "2483BA35393AA70856554220E0A3AF07C33C6DB03BE12611456E202A0549FE4D",
                            "FD0113CC43B968461C3F2747BAFD05878A406BFB77B2DC871CA38F437BF210AB",
                            "F0DACE2C90812B32803F5D5BEFBACA52CF2F356D7DABD617D9EBBA97328FA785",
                            "3A0AF0AD0C70DCA0133030A00AAB6B988C898B85A46280D8E76D276CA338424F",
                            "44EECEB56CD25464A250C2D1A6F8229A2764BBEF7D0055136BCEF30891C38EDA",
                            "877D98B96F97C3E60F2943D1B8549FE4AE5028404595E5BBB202944AC6F01A60",
                            "2D315749EF112D77ED3678AB490EF8A57DE306CA8FBACF92C488768A96A9BCFA",
                            "4369B3388623807E1FC6CDF6903B52603FE4CB6B9BF077C59F8B13B51977AA69",
                            "56B0C8A81165767A2ABBA57DFAE643EDF4939E90812357C57D771494BF53842B",
                            "6A6BC82CE98722D24EA37A5BD6147964B80EAB15FA92040ABBE5EDC2091926AB",
                            "49AF43F65FCA39766EE5A8E1A350728188EC7484415A08C99E1B5D20D3CCB02C",
                            "8EEB0C0B13C8F4AFC1290108A76A694657CBD88D386C01FAE14DDA3375500B2E",
                            "3B1005BDD07BD695EDD5E03F82B491D716E1385667403E71359CF4AD1E1A16A9",
                            "75D9C054146CA170317679319D5B4EABEB43A5BC54DC8C76436E3FDA419FFD02",
                            "28B85444D0F90113D9DCF445EE1988EE228980103F3E285ABEF1D74EA8D739F4",
                            "086064E478FE929117878CB7BFCDB615E3E9F2386DBA414922121C026B112A57",
                            "AD958731124D7ECCA778B9D5671C61E6D7F45FB9DFED3ED088A48BDC9549BF64",
                            "209D7728CBFA4071209906222B90EE625E4C2204FC26DDE4C07C23079AF75A9A",
                            "565907A56C26BD5A51CC030F43A06E3A147D468108115D5FA77E345A6E565F6F",
                            "1D1DE732390C20EDBDDAC5DFC6818A6F0E016C50DC376AD2CDD3F24EF1FA1855",
                            "A864A423696798807F2FB532F1708FA35C1DAF6C419E67E6E773A6BB129F8266",
                            "C47D3FFB2F918FA865597A668ECA393F9FC2E429CF0751E970A03C7F48554982",
                            "D033B2D1E8A2CC77FBE576D074294E80E6C6018802E62FF7BC56701E54A00BD9",
                            "25D569F5D84842FFEED3A05178B6B9E582E922B43A8CC29F1146E34FAEBBDDD4",
                            "C56A6197504AD536CFD9A0C26599F6D3E305CF77354CF2D7A33546DA24439B16",
                            "28F8DB51BED3DDD90194D1452B18C2612C65FC09D77FE31B271C59291C0C2E06",
                            "D6DE54039FE5A22D86CD522F1A9B7794E487B74D9B6B8CBDE23F240F434B6749",
                            "D3878EF6C92B84678AE2FBADC40961A161A128EA54AE59C2775CE076C2AE7A85",
                            "6DF891006E4D4840263C561C54135BFE78FE766A5CC241DDC7A0563BCA4E4C5E",
                            "FE2D5BB3051F24F6114A7DA14CD393E4187219F52DB83CC0438E0F0B1B350874",
                            "39292A3C8E56BEA306D83441ADF3F0E65C02BE1DAF3F66F8B95EF46FF44D2487",
                            "506E1F0A25BE23A46C7483D23CC94583837411D79DD446219964DCE2C0807B0C",
                            "1D4327D8480B5EA7A61AFB1DBA977D2621372227D5FC55FC90081B3675082DC2",
                            "39A957F6982825A34ED12AD63BB1DB76BB6C9E725C28BCDA198F62876E0FEE44",
                            "64C05FA4D16991A613F13655474ABC52C49093334A1C844F5F0682560B0BEE70",
                            "A4E4AF4BC5B670F2726146233CCA99226EDE61468A26EC4378C919A1D0CC7162",
                            "6AD5D147ACBA2AA94D19AA4CF8FE8A92F11D57DB7AE21CE2226103CF1B4D49EE",
                            "7764C4EB977E2E40825593AAC2B51FF8D2DC6E29FD8F257C235D009D24238860",
                            "DECBDC6150DA4CA92272AE45CBAE9D8174E84FD2D0053707778A3FEA4FCE8961",
                            "BFED2884BB6E0DCD3BC0E91326CAF674A449D05C7AFAC95BDAB3D51FFBF369E9",
                            "ADF69FD7A3273D709B2FDD7FC4A2C43D6D0D6BCCC0AF65FDC584B543FEF68240",
                            "FF3667A56CD39221B8F2B22A479345CBAB9E7CDBA43A0356E694AF58D347D2C9",
                            "4DD651F3A6E1B3EAEFF384F21C04B2BC39EEEBBD016180781CC007842F5C9DC5",
                            "B7FF750CAB3F806DD13ED55CB54443BFBA616762DC32891CFF55885407844911",
                            "24CDF209FBCFA4F10D46CCD4B05C8A80E030A212C6F53FD2DEFE93A0E64A2848",
                            "E3EA5323ACA4611D84415B787CA74B0BFD9FFC37E1BD862498A8370CEBAF6AA8",
                            "8D1A5630B0871D4E27F5B13E73CE313C7D65E4694541877940E6D113CBE6D5C1",
                            "FADA7F6865EF29AF7C9485E170D25C03B96354E8E419532090A33FE11E5C536F",
                            "A13BFBD7E04AE200E7BF91159B63B8BB6B867B07905DE927515178DB786ABB10",
                            "0C7B5ECEF3C99096D29307EFCF2FF2C058EA8CAB8E8CE45602C877EE06C8E428",
                            "B58FF112CCAAAADBBE9CF2B986C84F6F9E63E21AED202DD6A8154287F1374540",
                            "0466E0A51610D718CCBEB76DD5375B8D19401F14F0D1FC43BADBDBB0CBC2E252",
                            "5D78C471A058971A621AAA03AC8B48629BBDCBA566729BFB9ABCD57F34C7431A",
                            "52154C4764859925DCEE21722E782B9661751CA5E12D5C6F20CB25329946FF42",
                            "D1157510A1F7E60DEEA252C1556D442529BB85FD663ABBE0AB50FDBEB2E6B102",
                            "AFD343935B0A415478B1B939F7F65B01ACE8697E600D2D4876C44322B4F1C7E5",
                            "7009160F2CEB809C9D3579471F2AA4375CB898AEE4D75610A1597A231DDCE48D",
                            "91908F58250C01130E5DCC2F4E27C7B1AC4F3DD2ED37D89903EA1C4814610215",
                            "F91F97F8B7ADC47DF97C57DBBD9172F6F474519BF98A3F191C36EDD5FED60FA1",
                            "681992EE99027FB7083F6801F4FC0C42342188D4C19977A723F922DCDD7D96C2",
                            "431EB9959F3D1FF3902BD92710E9A82D2F2FF2E21551A9A4704CC5D021612CB4",
                            "94AD216A9B3080BC850B1E4A2F3FDA73000806070FEF53A63EB993BCDC89C380",
                            "E1CA581E35FD56A476A09FB1FB69E0F756538FBFE8C7784C38042511AB9C98DE",
                            "11C388D35E712ADC97A3D0AE46FC0C0CAD8F4719255A19A3882194FB2DF5F7F2",
                            "68D34928D77FCB509A8A06B6EB7C674CDF21E53BC5BED834FEE948F2BE3234AC",
                            "451A0520DFAD303D5564D9E383C4335EC457C870D4BBC62C33F3B229C41599AF",
                            "183155CAC5C01A29209CD866BBCCDF2B50EA3D60450AB8E6B69B6F85509C60F2",
                            "02494D95424824D3F83C9916CDF0C1CA0FAAA1C22B8E7647D16CE84C15E9850D",
                            "4D140E49AE22792B21B6B96C7DF003954B34393BFB915C18AF1CA7B98C83DC45",
                            "21735614AFBBCF9D109DCD83424024EE8009292DD8D75715C3A6E1F96250B7DB",
                            "82EFBE40A7B810369E7C39C199D77061E129CAE36FE4A0A000811A23A7AD7E63",
                            "AF3CB3F6F3B9D086390BBFC6FD3F8FF2918D58E2B2032D89E83879174F480E3D",
                            "21848D2B5F50AAF2044662483386A629EA8162995E58A479625F9A1C617A01E7",
                            "BB8B7E9CAF46E29403C649613AF16FE8AF37478AFCE240BE6C0BC3F54A6EB4F1",
                            "F2874B80E64C57F8FD1DDA598FBEFFB4B2E6CECF3A984DAEC60F184ABFD2C03A",
                            "15CDBBDA11931E1D24D37218E65CB3CF08F0E539DD9AB942214C05B3E494B6F0",
                            "9C210C63F0679B59DA6F003A0C5D6F7E2DB4EA1406BE6EACF449D217C285E037",
                            "192C9D0C5A49989BCA788F1BBE63B58556BA0E433BBD8ADD4B863D1C0FC356AF",
                            "03F177E6517BB75C395B4E6A7AB45B8AFE0E974210828713AA02DD47ABD1408A",
                            "7DDEBAF7B185001AE2BBE964380753231869474CBDA1EC95600DA8E1A02646B7",
                            "A85AB657FAEDBEB7131FEACDC65347EE08B4ED13D1C72A49CC2B5F8508A5B0CB",
                            "9C6E9109DA0B5A513DF5E73392AADD7C6C0496C940BE60A9B8416C3D4B556EEB",
                            "99E68186022471A787D7A8CDA6F75F067925A374B8A429EDC7F36E064AEA4199",
                            "A4AE86CA5A78E2145856B3A1D3AB7C6F0907BA46978F50633E48049180D031C0",
                            "EBC6B139FB1EC3E9E24132689CA092C8672EDF356398D29100D9F56BEA81BABB",
                            "C595DF4DE3C2127C9A6AF2136E6179341D9DF5155A4AC514B2CDD2C29C4AA07D",
                            "29612714CE0AAAEB97B287F0C5ECD38782FF9BDB2B18D482679E64557223D380",
                            "DD025B92754F403EA9BCCC588CEE41F7AB64A7458757EAFED1025E193BAA8CFC",
                            "0E32604081E3D7349225F7F9379E291A69CD78D0C65D332DEA30FEE3795E0F4D",
                            "CA6F59B04FAE47133310217A832300402B947163AD630365E9BADBCB1F839634",
                            "1170E9481115804233BB48DECE4A40FC3822D90B304C1842FCF31185982C6A26",
                            "1F5C266D7C98931AE2FE24C540D62691534CE19D3E0705866F2BCB4A9A7DC96D",
                            "ED5EE281D36BFD1AF424C9893D81FC89A7B7904B46CBCC73878FDAB881D34104",
                            "126EFAA40EE6813C63D0C98776E3C1D367CDF082A1CD4BEAD9AEEA0F5B381BF6",
                            "41B9BAB0370A20827E8D982CBD998A9F3061DBF4E8D8D5FB9A94AB88ECDFA9E0",
                            "AD84AC38922A4F7419454303357C7F35694D428C4DBA055A73318647A28BBA22",
                            "F1E13F085E07E8B7B096A94046254D2C753F0A897228F82FA8E8E4470E8EBC84",
                            "E56D730E2A64F28B5D8F1F622224CA3DAFC9E8977C2D7F6916B668E6F5AC5FEA",
                            "5176BCD5B49F51EF0357BCDFA99393FF870C956548183C45CCABE3C45A6E0210",
                            "D0BAF08FBA5DBB1C56B962D799F5A540227A0A730DA01A29709F1069036C0E4C",
                            "4B6864224578CC821F3FDD8481C9898BE2D6F7AA57E0C088A4190F97361066EE",
                            "8C82AF844701C4A460E6977AC3138FCC0A16DEFB86C30A1DA5D850E79F4346EA",
                            "DC494D03BDC80BDB671DC34580C85101B4757C6FABE07B96CD50DCA08F870EF1",
                            "7E01B6E68F72C59B819D9FE3A427896EAF6976F54D11DBAF4E4B78AD0F4F94CE",
                            "65DB13E8EF98054BF5EB1B7B458B37D09F062F1ED5AD29CBCD655E082CFDF948",
                            "87377F31ECBB1BA1FC8D2B78EBDC3BC63F034C81490097C445A43B571C24CE41",
                            "895B2CD5B1F669A35FBDEF28224126F433758E4A95A5D2503D9058BB70ED676A",
                            "503D2DA830018B8F422A32409D71DEFFE1F4DA682236B8218607191EC01A3A7D",
                            "A86D61D03AAB6D516C93F90C80C227A137776B7FEBE3E7BA6F7DBCB18FA800B9",
                            "E0E88839EFBF8E78AD5981928FC6B75BF1B037534CB20D8A19B173183F726D96",
                            "1E64DCEF5EAD556D1243CD60AAC9DC8F5B6335ECDD3E2A606A2C504BDF24A752",
                            "4E3426C8BF92F0B713D59381A865EC646733BA6504907F43D3899E0437783D0F",
                            "47A43F68C85B560877398BF1F762B69517C43842EDF90D1869C5FFA3D334DFD2",
                            "1D2077E03065C4B1B893B09A1F9ACFAF4CEFB8EFA48110ECFA88BFC83EBDD02F",
                            "FA46B288FEAD5D7314855929BEE8E7E6FEED199B4A8EA45DCB026DE0A2F85DEB",
                            "CDEE9069327F829ECB476103E859033481B1ACA692527B2A117F5188259CB642",
                            "8C892E4EC406900DC3632A872849F2D58A248E8089EEA5F95B4650F2B8140037",
                            "9F5C5E1017556B907E60342E8D146556C41A8E125F62101F3E11023FCAB0FA0D",
                            "A97DFE953D243B9534C4CF446D58928B3DA526A52739C65C218D98E89AB8F154",
                            "EBF542E9C22DD349ECE2BC90BFA78412F97E45315349F7D6EC530F266A10874D",
                            "C20330EDC46B8C3DC3841AE2141BF523E5D82BA7453E7C80772EC545B8D74D68",
                            "5AF5470AD82C035CAEE8822AB8E23F86C87A9304069BF4DDB7B4329C9D99CC95",
                            "899D94ED8FCA53556E2B679F007298FAD93FC7E948337918154907E8AF44BF88",
                            "0CD167820E508D58E0EAA828C611BA83E483F31316262AF4DA215A6BC0E049F9",
                            "D7F686491E837A42A82CEB1209344EA906ABE1D5D2A88A09B3DAFAA41D174CEC",
                            "B2BD977BE83845585E5BF100DD8F1D074B97B863EF026E5B3E5CF58610A49B76",
                            "85ABA1F4F4EDC96E21491BDA3B0FBF9D24F85CA031C2CD7885F1EFA70D32B4DE",
                            "4F88CAC8248F7BDFFBC6FA84FF90041CEEB7E6F37480BFEE121CF3E56D3A79AE",
                            "5D2BDDB71EE422BDDC673E435121257D4A0603ECDD53A00FE2A5F7F2D0301173",
                            "198493B9CAAF02641DF7BC6C0F18B45C12E036EAE757F3465EF973924DF8C77D",
                            "E4AE4E3F8C34122B47E1D5B860911CDC767DAFB039C005961175E2439BF78DC5",
                            "B5BB5640A47DA626D65122018BD3C415DDBE2A1CAA4407B8B079CEF0C304AD95",
                            "BDB7AD110926AA63846409D5FD1A99AA1BE868AF245389ABEE31F14EDD15EBB1",
                            "D8495120CB075C928C72CD926B94C99F5254AAD2D6E244BAFA9A63BA52BD960E",
                            "D2557CD093DCBFBD3AFDB8EFD11FC1E6F413EEB2761749EB200A872A3D2694DE",
                            "571BC9B0877BBD6367B06671C27EAF34F816CCA2C21DCE5897B0EFCD0D43A2A3",
                            "04B1D993665ED4D933932058FD8FC2B94DCDFDE047548FCB3A01DBD14635D077",
                            "62C1D9B265994D0E7771EF89E862EA78DDD92393AFE0CE8AD8CA88CB6704D1E0",
                            "900D28DB3B3D291923AEF1D21A4C479C03351C5F8BAD5466B8342D7DA27D8CBC",
                            "1DE86AADC71CA909F70481703077EE060173D6A29793C310C580B98ECC874201",
                            "4E0370E993DD3582AF5E6E4305DEDDB827FD54CD724FB1D8C0C9D3233E47BE14",
                            "A3DB94469E15C3F6929B2D32F738175094927B5BBBB0B604C2D53CB06D346127",
                            "CAC99E91ED58500BDB307D5DE552CC8C47904475C80BBE11541E893BB1E12B4B",
                            "4765C07192C891F5241B66A30A97C97FBCC8F4CC69BCB1AF6E3300785623F3A6",
                            "45BF472F5B15A434F566CBB2FB47C5831D11E4B9B223541368E8548D4C55B59F",
                            "3FC3D33C9C4A183DA4637E4EF274E8FD33B875BA0FCC04C651FC7C4232B16FAA",
                            "A98C78E925763C6C1E5042C84A67D41F88270ED729D69A26201E07A5DDA5230F",
                            "50C0C03269F1F6F7A2D043E87D77E33C90A1DFA89D1ECDEAF4A60C58F35B5A88",
                            "DE165AA3966F0563450D0574E640299CE74F7F34DCC91B6611F28D5B4F4787C3",
                            "5991D443A65155D51146CDEB92B20DC6741F5DC69CF9BB09D2143B1FB6CA5DEB",
                            "7B277027ADA02DE4A2ED276DDFC25A24DC192C4621C690388214EA3A3702D160",
                            "6DF9246E01CCBB2EB84D45042759DE38626E31FF13C96B96D7B3D810B913C267",
                            "E57254EC7240FC6EDEDC06472720C85BA29D7767B558D80912153A9AC80B73C9",
                            "D82D0B64692718A90F92DEA9B86DD919CAC9D00E81D67016622D9EC45ED881A9",
                            "E7422CE9EEB7CC82B3D4DF2B269EEC0B1D4600DC6CEB4378331724D9B27E498A",
                            "D7B00E067380396A385C16E83B269DAA401D37983B88C2170F3F5868A8AB12E5",
                            "E5C8DB25AD8CA4227DD22B37A85ABC550CF10EB22107B9300ACA996CDE0FAD43",
                            "149EA8F908855BEC67176C2FDBEE3DF60A96996AFFC3CA29BCC19A0E2612A3ED",
                            "7D30BE677279B82584374E603FD195AA8141F2E7F4027178C86B7FB4489FC824",
                            "B3516D84A78F96AAE074D01FAA58711E79705D12164894B89A5C65FF8A459379",
                            "149E3515ACADD2514E3C452CC02A0043F064FC245E559A6F184F60E0B0910ECE",
                            "7D58A24F7D64BBE0B394F416F332363FEAF205B0094347C4EC036D3B3945103D",
                            "1433C2844DFB3910310FF1E1C33DFA6F6B687ACCC6D306B3D5EBEA4221B377EB",
                            "A1AB11FE6E3AF3610BBB0FD4294A6B0DEB8064FEEFC082AB3D02F7A9FC455715",
                            "E68A28C04129B4193082270FA2CCB8C5616EA265F43CDF3875C7FDB28D6E7FCD",
                            "05C99D4ED3F3817CC771B5C6C44A5C8D1DC12E104E2EE9D68FDED0A09AE04496",
                            "998D87EDA73CA171AAC58BFD81AF491655D35A56F39499808F0D615457DD15A3",
                            "DBB89E5A3DC843EBE9FD76D2900A5EAE13B1F0DD94C4ABD79419BDA49C0D198F",
                            "1B8CF3D3641353C5B542028D693EB48B234481EBBCDBFFBB4296E2F3F27EF73C",
                            "F4E458B405F45F4EEC826BE48C5C1DCAE63793D5BB9F594F92C67A3BADA0C22B",
                            "4C7E38C43A22C6B24060DCDC082F8A870A9B7F503863913AA231CB1464EDEBBA",
                            "BA1B4558AA11B62016A63D9A62C70D9F1F3D63596EA1FEB48B762C88E499A276",
                            "5008C69B9FFE9228582C1885E376DDB7F97A32F9D30320A3B6643703E9B36755",
                            "B8A8DF116E86B84732A2CDD730CB869783581CFF552EBE29B76D249BC03CFBCC",
                            "64CB6269F83A4E04B1484EFEEF680DF4FD53DEC015E04DAEC3B8A1AC320E64B9",
                            "15FB1B492A8F53CA3E3BA2DE62504586A895E3F1D9941A491CE44B5676355DA8",
                            "B815A256260EF5B646309EAF63057C70B83F82927D39E2E8869A4D4D9E4FFD21",
                            "5F559A5FE8A29C9959508AA33684F71BFD387395330988055C0062CA756DA44B",
                            "6A4E1C27AB06D35675648C9FC6609D55E778972F70FCD6F28852116BCE56DCB9",
                            "0390537EF63BF56857F6252E0CD6151C0321AC683AD91C5CBB59A1FE93285506",
                            "9717BEB3BF2DBFB82E7572458553D0E24C9DEA2B752711813AFA7E36235B6EEE",
                            "BAE3CFD5371A27B256970B4D94E104D65440EEA94EEF55FE2FB8063D0BA31776",
                            "21CA517FFA45C251FCD86ABCCBB906BE365B5A597BF6283BEEFF99F562ACA922",
                            "5336C2C2A8602FE29AA1E67F628CEDC3392B66320A995694176BA34D4CB17C38",
                            "70C73DC0C72B1D95821F0EA13D4473341BB470044662025D6A228D084DA3859B",
                            "93BFEB7E283134FB6D4DA2C8AF413CCA5E7EE8903ABA5F6D48BE9BA00798852E",
                            "3A4B964920627F66A11249A5EF68A9FAC468A2076FC12BE725A5C343E83BCE95",
                            "13C233A526E35A73DEE0AC36909CC7522C39AED9D42C59D4D7CF646B3F0A09DE",
                            "94F66D4C66295A3ABEC3D30279F1BD734E09407384B6845770AFBE24E2CA233F",
                            "C5F9578F580F0111C8728740A39D07957DB8611769676A2ED02E6648B847D865",
                            "D2BFD4D322A5F37CDCAD66952742943EA9D29D66D22EA0294781FFFF8DE8BD74",
                            "CF5B95D57193FDD94C3788BC9BED36762CD6B0231CAF5F38423BC7B9B8CCD625",
                            "FB311E71ED73477183EF1578D152626FCA7E7B2A93785AABF29C5A6267E46953",
                            "D00F187A37A8E4BBAF6E9041893D30B432CAAFC1B175A27B45C5344AFE3FE462",
                            "0E9E27E3AA04E9DD3F77146FBE4065CF877EB1DE0555D9FB9EE776EDD7F9B78C",
                            "7A9D5ED47D42269852BA4AFA373C8BED88F99BB7C4ED9CC4900D2585A3832DD6",
                            "D7F92889EEC05282AE1BB4375FAC645B968EC23B0AE47E2206496D3E12770BA6",
                            "29BAC714E9BB7DFE9BE3C8B4AACFDB184389388340A4F3B09CC61A39BAFC3705"
                        ],
                        "LastLedgerSequence": 19977886,
                        "LedgerEntryType": "LedgerHashes",
                        "index": "B4979A36CDC7F3D3D5C31A4EAE2AC7D7209DDA877588B9AFC66799692AB0D66B"
                    }
                }
            ]
        },
        "ledger_hash": "C769F00409D574A982DC0CC071A244C5507D9E2269404957B78081ED8F2FCB1C",
        "ledger_index": 19977887,
        "validated": true,
        "status": "success",
        "warnings": [
            "This is a clio server. clio only serves validated data. If you want to talk to rippled, include 'ledger_index':'current' in your request"
        ]
    },
    "warnings": [
        "This is a clio server. clio only serves validated data. If you want to talk to rippled, include 'ledger_index':'current' in your request",
        "Too many requests"
    ]
}
```

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.
* lgrNotFound - ledger\_hash 또는 ledger\_index로 지정한 레저가 존재하지 않거나, 존재하지만 서버에 없습니다.
* noPermission - 전체 또는 계정을 true로 지정했지만 관리자로 서버에 연결되어 있지 않은 경우(일반적으로 관리자는 로컬 포트에 연결해야 함).
