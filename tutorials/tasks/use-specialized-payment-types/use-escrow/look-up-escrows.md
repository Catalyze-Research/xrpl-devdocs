# 에스크로 조회(Look up Escrows)

보류 중인 모든 에스크로는 ledger에 에스크로 객체로 저장됩니다.

Account\_objects 메소드를 사용하여 발신자 주소 또는 수신자 주소로 에스크로 객체를 조회할 수 있습니다.

{% hint style="info" %}
Tip:

2017-11-14에 fix1523 개정이 활성화된 이후에 생성된 에스크로만 해당 객체를 수신 주소로 조회할 수 있습니다.
{% endhint %}

account\_objects 메소드를 사용하여 발신자 주소로 에스크로 객체를 조회할 수 있습니다.

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

응답에는 송신자 주소가 Account 값이거나 대상 주소가 Destination 값인 rfztBskAVszuS3s5Kq7zDS74QtHrw893fm의 모든 보류 중개인 객체가 포함됩니다.

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

