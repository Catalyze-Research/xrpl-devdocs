# 에스크로 조회

보류 중인 모든 에스크로는 ledger에 에스크로 객체로 저장됩니다.

Account\_objects 메소드를 사용하여 발신자 주소 또는 수신자 주소로 에스크로 객체를 조회할 수 있습니다.

## 발신자 주소로 에스크로 조회하기&#x20;

account\_objects 메소드를 사용하여 발신자 주소로 에스크로 객체를 조회할 수 있습니다.

발신자 주소가 rfztBskAVszuS3s5Kq7zDS74QtHrw893fm인 보류 중인 모든 에스크로 개체를 조회하고자 한다고 가정해 보겠습니다. 발신자 주소가 계정 값인 다음 예제 요청을 사용하여 이 작업을 수행할 수 있습니다.

요청:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 5,
  "command": "account_objects",
  "account": "rfztBskAVszuS3s5Kq7zDS74QtHrw893fm",
  "ledger_index": "validated",
  "type": "escrow"
}
```
{% endtab %}
{% endtabs %}

응답은 다음 예제와 유사합니다. 응답에는 발신자 주소가 계정 값이고 수신자 주소가 목적지 값인 발신자 또는 수신자 주소가 rfztBskAVszuS3s5Kq7zDS74QtHrw893fm인 보류 중인 모든 에스크로 개체가 포함됩니다.

이 예에서 두 번째 및 네 번째 에스크로 개체는 계정(발신자 주소) 값이 rfztBskAVszuS3s5Kq7zDS74QtHrw893fm으로 설정되어 있으므로 조회 기준을 충족합니다.

응답:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 5,
  "result": {
    "account": "rfztBskAVszuS3s5Kq7zDS74QtHrw893fm",
    "account_objects": [{
      "Account": "rafD3taonqdnVpaxCCT6sjnScZUeFGf1JG",
      "Amount": "250",
      "Destination": "rfztBskAVszuS3s5Kq7zDS74QtHrw893fm",
      "DestinationNode": "0000000000000000",
      "FinishAfter": 570672000,
      "Flags": 0,
      "LedgerEntryType": "Escrow",
      "OwnerNode": "0000000000000000",
      "PreviousTxnID": "A0951691DF3BCBEEB3108F2229A702D078BBBF848268BC601E59B68A2E390AAC",
      "PreviousTxnLgrSeq": 4602906,
      "index": "2BF3226ACCA8FF7ACB7201F20A701F51D8666A2FA2FBFBE6A05C9161F9228A18"
    }, {
      "Account": "rfztBskAVszuS3s5Kq7zDS74QtHrw893fm",
      "Amount": "250",
      "Destination": "r9gyNNzhMtfwZara61u3ycfMLdkTpKJZHX",
      "DestinationNode": "0000000000000000",
      "FinishAfter": 570672000,
      "Flags": 0,
      "LedgerEntryType": "Escrow",
      "OwnerNode": "0000000000000000",
      "PreviousTxnID": "463D5A3CF09F4890B8471027F80414B3B438E6907425B71DC324D7118E90A107",
      "PreviousTxnLgrSeq": 4603003,
      "index": "35462CDC28AD830B29D101E8307AF5B6BFBC262F1BDCCA7EB45D1CA3F8B44F53"
    }, {
      "Account": "r9gyNNzhMtfwZara61u3ycfMLdkTpKJZHX",
      "Amount": "250",
      "Destination": "rfztBskAVszuS3s5Kq7zDS74QtHrw893fm",
      "DestinationNode": "0000000000000000",
      "FinishAfter": 570672000,
      "Flags": 0,
      "LedgerEntryType": "Escrow",
      "OwnerNode": "0000000000000000",
      "PreviousTxnID": "08C9B20AC9EB191238038A108CC4CBBC0243672484B466FB42DED0A7DF6A31A1",
      "PreviousTxnLgrSeq": 4602954,
      "index": "A7B0983A1B53D92278E21499064A4F8BBE08CB8D14DB6BBBA8F688AB1D3FDA45"
    }, {
      "Account": "rfztBskAVszuS3s5Kq7zDS74QtHrw893fm",
      "Amount": "250",
      "Destination": "rafD3taonqdnVpaxCCT6sjnScZUeFGf1JG",
      "DestinationNode": "0000000000000000",
      "FinishAfter": 570672000,
      "Flags": 0,
      "LedgerEntryType": "Escrow",
      "OwnerNode": "0000000000000000",
      "PreviousTxnID": "F4778F528AB3CB945BDB88036EF9FE6C0E899F1629D9E51129E3B93CD488395A",
      "PreviousTxnLgrSeq": 4602977,
      "index": "F99A4DDADDDF623908C9A048170AB107AFF78684AB8F3110E9F00BBBC606ABD2"
    }],
    "ledger_hash": "1D4850035F175CA6F1CD5CE3B53C01AA83E4F086C13085E4FBC1EEFCCB345A9B",
    "ledger_index": 4603176,
    "validated": true
  },
  "status": "success",
  "type": "response"
}
```
{% endtab %}
{% endtabs %}

## 목적지 주소로 에스크로 조회&#x20;

account\_objects 메소드를 사용하여 목적지 주소별로 에스크로 객체를 조회할 수 있습니다.

{% hint style="info" %}
Note:

보류 중인 에스크로 개체를 목적지 주소로 조회하려면 해당 에스크로가 2017-11-14에 fix1523 개정안이 활성화된 이후에 생성된 경우에만 조회할 수 있습니다.
{% endhint %}

목적지 주소가 rfztBskAVszuS3s5Kq7zDS74QtHrw893fm인 보류 중인 모든 에스크로 개체를 조회하려고 한다고 가정해 보겠습니다. 목적지 주소가 계정 값인 다음 예제 요청을 사용하여 이 작업을 수행할 수 있습니다.

요청:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 5,
  "command": "account_objects",
  "account": "rfztBskAVszuS3s5Kq7zDS74QtHrw893fm",
  "ledger_index": "validated",
  "type": "escrow"
}
```
{% endtab %}
{% endtabs %}

응답은 다음 예제와 유사합니다. 응답에는 목적지 주소가 대상 값이고 발신자 주소가 계정 값인 rfztBskAVszuS3s5Kq7zDS74QtHrw893fm을 대상 또는 발신자 주소로 사용하는 보류 중인 모든 에스크로 개체가 포함됩니다.

이 예에서 첫 번째 및 세 번째 에스크로 개체는 데스티네이션(목적지 주소) 값이 rfztBskAVszuS3s5Kq7zDS74QtHrw893fm으로 설정되어 있으므로 조회 기준을 충족합니다.

응답:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": 5,
  "result": {
    "account": "rfztBskAVszuS3s5Kq7zDS74QtHrw893fm",
    "account_objects": [{
      "Account": "rafD3taonqdnVpaxCCT6sjnScZUeFGf1JG",
      "Amount": "250",
      "Destination": "rfztBskAVszuS3s5Kq7zDS74QtHrw893fm",
      "DestinationNode": "0000000000000000",
      "FinishAfter": 570672000,
      "Flags": 0,
      "LedgerEntryType": "Escrow",
      "OwnerNode": "0000000000000000",
      "PreviousTxnID": "A0951691DF3BCBEEB3108F2229A702D078BBBF848268BC601E59B68A2E390AAC",
      "PreviousTxnLgrSeq": 4602906,
      "index": "2BF3226ACCA8FF7ACB7201F20A701F51D8666A2FA2FBFBE6A05C9161F9228A18"
    }, {
      "Account": "rfztBskAVszuS3s5Kq7zDS74QtHrw893fm",
      "Amount": "250",
      "Destination": "r9gyNNzhMtfwZara61u3ycfMLdkTpKJZHX",
      "DestinationNode": "0000000000000000",
      "FinishAfter": 570672000,
      "Flags": 0,
      "LedgerEntryType": "Escrow",
      "OwnerNode": "0000000000000000",
      "PreviousTxnID": "463D5A3CF09F4890B8471027F80414B3B438E6907425B71DC324D7118E90A107",
      "PreviousTxnLgrSeq": 4603003,
      "index": "35462CDC28AD830B29D101E8307AF5B6BFBC262F1BDCCA7EB45D1CA3F8B44F53"
    }, {
      "Account": "r9gyNNzhMtfwZara61u3ycfMLdkTpKJZHX",
      "Amount": "250",
      "Destination": "rfztBskAVszuS3s5Kq7zDS74QtHrw893fm",
      "DestinationNode": "0000000000000000",
      "FinishAfter": 570672000,
      "Flags": 0,
      "LedgerEntryType": "Escrow",
      "OwnerNode": "0000000000000000",
      "PreviousTxnID": "08C9B20AC9EB191238038A108CC4CBBC0243672484B466FB42DED0A7DF6A31A1",
      "PreviousTxnLgrSeq": 4602954,
      "index": "A7B0983A1B53D92278E21499064A4F8BBE08CB8D14DB6BBBA8F688AB1D3FDA45"
    }, {
      "Account": "rfztBskAVszuS3s5Kq7zDS74QtHrw893fm",
      "Amount": "250",
      "Destination": "rafD3taonqdnVpaxCCT6sjnScZUeFGf1JG",
      "DestinationNode": "0000000000000000",
      "FinishAfter": 570672000,
      "Flags": 0,
      "LedgerEntryType": "Escrow",
      "OwnerNode": "0000000000000000",
      "PreviousTxnID": "F4778F528AB3CB945BDB88036EF9FE6C0E899F1629D9E51129E3B93CD488395A",
      "PreviousTxnLgrSeq": 4602977,
      "index": "F99A4DDADDDF623908C9A048170AB107AFF78684AB8F3110E9F00BBBC606ABD2"
    }],
    "ledger_hash": "1D4850035F175CA6F1CD5CE3B53C01AA83E4F086C13085E4FBC1EEFCCB345A9B",
    "ledger_index": 4603176,
    "validated": true
  },
  "status": "success",
  "type": "response"
}
```
{% endtab %}
{% endtabs %}
