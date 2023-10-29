# 다중 서명 설정

다중 서명은 일반 키 및 마스터 키로 서명하는 것과 함께 XRP Ledger에 대한 트랜잭션을 승인하는 세 가지 방법 중 하나입니다. 거래를 승인하는 세 가지 방법의 조합을 허용하도록 주소를 구성할 수 있습니다.

이 튜토리얼에서는 주소에 대해 다중 서명을 활성화하는 방법을 보여줍니다.

## 요구 사항 <a href="#prerequisites" id="prerequisites"></a>

* 트랜잭션을 전송하고 새 서명자 목록의 [reserve requirement](https://xrpl.org/reserves.html)을 충족하기에 충분한 여유 XRP가 있는 자금 지원 XRP Ledger 주소가 있어야 합니다.
  * MultiSignReserve 수정안이 활성화된 상태에서 다중 서명에는 사용하는 서명자 및 서명 수에 관계없이 계정 reserve을 위해 2 XRP가 필요합니다. **(MultiSignReserve 수정안은 2019년 4월 7일** 부터 생산 XRP Ledger에서 활성화되었습니다.)
  * MultiSignReserve 수정이 활성화 되지 않은 테스트 네트워크에 있는 경우 다중 서명 에는 목록의 서명자 수에 따라 증가하는 계정 reserve을 위한 일반적인 XRP보다 더 많은 XRP가 필요합니다.
* XRP Ledger 형식으로 키 쌍을 생성할 수 있는 도구에 대한 액세스 권한이 있어야 합니다. 이를 위해 서버를 사용하는 경우 wallet\_propose 메소드가 관리자 전용 `rippled`이므로 관리자 액세스 권한이 있어야 합니다.
  * 또는 이미 XRP Ledger 주소를 가지고 있는 다른 사람을 귀하의 주소에 대한 서명자로 승인하는 경우 해당 사람 또는 법인의 계정 주소만 알면 됩니다.
* 다중 서명을 사용할 수 있어야 합니다. **(MultiSign 개정안은 2016-06-27** 이후 프로덕션 XRP Ledger에서 활성화되었습니다.)

## 1. 구성 설계 <a href="#1-design-your-configuration" id="1-design-your-configuration"></a>

포함할 서명자 수를 결정합니다(최대 8명). 지정된 거래에 필요한 서명 수에 따라 서명자 목록의 정족수와 서명자의 가중치를 선택합니다. 간단한 "M-of-N" 서명 설정을 위해 각 서명자 가중치 **`1`**를 할당하고 목록의 정족수를 필요한 서명 수인 "M"으로 설정합니다.

## 2. 멤버키 준비 <a href="#2-prepare-member-keys" id="2-prepare-member-keys"></a>

서명자 목록의 구성원으로 포함하려면 하나 이상의 유효한 형식의 XRP Ledger 주소가 필요합니다. 귀하 또는 귀하가 선택한 서명자는 이러한 주소와 연결된 비밀 키를 알고 있어야 합니다. 주소는 ledger에 존재하는 자금 조달 계정일 수 있지만 반드시 그럴 필요는 없습니다.

wallet\_propose 메소드를 사용하여 새 주소를 생성할 수 있습니다. 예를 들어:

```json
$ rippled wallet_propose
Loading: "/etc/opt/ripple/rippled.cfg"
Connecting to 127.0.0.1:5005
{
    "result" : {
        "account_id" : "rnRJ4dpSBKDR2M1itf4Ah6tZZm5xuNZFPH",
        "key_type" : "secp256k1",
        "master_key" : "FLOG SEND GOES CUFF GAGE FAT ANTI DEL GUM TIRE ISLE BEAR",
        "master_seed" : "snheH5UUjU4CWqiNVLny2k21TyKPC",
        "master_seed_hex" : "A9F859765EB8614D26809836382AFB82",
        "public_key" : "aBR4hxFXcDNHnGYvTiqb2KU8TTTV1cYV9wXTAuz2DjBm7S8TYEBU",
        "public_key_hex" : "03C09A5D112B393D531E4F092E3A5769A5752129F0A9C55C61B3A226BB9B567B9B",
        "status" : "success"
    }
}
```

생성한 각 항목에 대한 `account_id`(XRP Ledger 주소) 및 `master_seed`(비밀 키)를 기록해 두세요.

## 3. SignerListSet 트랜잭션 보내기 <a href="#3-send-signerlistset-transaction" id="3-send-signerlistset-transaction"></a>

일반적인(단일 서명) 방식으로 SignerListSet 트랜잭션에 서명 하고 제출합니다. 이렇게 하면 서명자 목록이 XRP Ledger 주소와 연결되어 해당 서명자 목록 구성원의 서명 조합이 사용자를 대신하여 이후 거래에 다중 서명할 수 있습니다.

이 예에서 서명자 목록에는 3명의 구성원이 있으며 다중 서명 트랜잭션에는 `rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW`목록의 다른 두 구성원의 서명과 적어도 하나의 서명이 필요하도록 가중치와 정족수가 설정되어 있습니다.

{% hint style="info" %}
Caution:

제어하지 않는 서버에 비밀 키를 제출하지 마십시오. 네트워크를 통해 암호화되지 않은 비밀 키를 보내지 마십시오.
{% endhint %}

```json
$ rippled submit shqZZy2Rzs9ZqWTCQAdqc3bKgxnYq '{
>     "Flags": 0,
>     "TransactionType": "SignerListSet",
>     "Account": "rnBFvgZphmN39GWzUJeUitaP22Fr9be75H",
>     "Fee": "10000",
>     "SignerQuorum": 3,
>     "SignerEntries": [
>         {
>             "SignerEntry": {
>                 "Account": "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW",
>                 "SignerWeight": 2
>             }
>         },
>         {
>             "SignerEntry": {
>                 "Account": "rUpy3eEg8rqjqfUoLeBnZkscbKbFsKXC3v",
>                 "SignerWeight": 1
>             }
>         },
>         {
>             "SignerEntry": {
>                 "Account": "raKEEVSGnKSD9Zyvxu4z6Pqpm4ABH8FS6n",
>                 "SignerWeight": 1
>             }
>         }
>     ]
> }'
Loading: "/etc/opt/ripple/rippled.cfg"
Connecting to 127.0.0.1:5005
{
   "result" : {
      "engine_result" : "tesSUCCESS",
      "engine_result_code" : 0,
      "engine_result_message" : "The transaction was applied. Only final in a validated ledger.",
      "status" : "success",
      "tx_blob" : "12000C2200000000240000000120230000000368400000000000271073210303E20EC6B4A39A629815AE02C0A1393B9225E3B890CAE45B59F42FA29BE9668D74473045022100BEDFA12502C66DDCB64521972E5356F4DB965F553853D53D4C69B4897F11B4780220595202D1E080345B65BAF8EBD6CA161C227F1B62C7E72EA5CA282B9434A6F04281142DECAB42CA805119A9BA2FF305C9AFA12F0B86A1F4EB1300028114204288D2E47F8EF6C99BCC457966320D12409711E1EB13000181147908A7F0EDD48EA896C3580A399F0EE78611C8E3E1EB13000181143A4C02EA95AD6AC3BED92FA036E0BBFB712C030CE1F1",
      "tx_json" : {
         "Account" : "rnBFvgZphmN39GWzUJeUitaP22Fr9be75H",
         "Fee" : "10000",
         "Flags" : 0,
         "Sequence" : 1,
         "SignerEntries" : [
            {
               "SignerEntry" : {
                  "Account" : "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW",
                  "SignerWeight" : 2
               }
            },
            {
               "SignerEntry" : {
                  "Account" : "rUpy3eEg8rqjqfUoLeBnZkscbKbFsKXC3v",
                  "SignerWeight" : 1
               }
            },
            {
               "SignerEntry" : {
                  "Account" : "raKEEVSGnKSD9Zyvxu4z6Pqpm4ABH8FS6n",
                  "SignerWeight" : 1
               }
            }
         ],
         "SignerQuorum" : 3,
         "SigningPubKey" : "0303E20EC6B4A39A629815AE02C0A1393B9225E3B890CAE45B59F42FA29BE9668D",
         "TransactionType" : "SignerListSet",
         "TxnSignature" : "3045022100BEDFA12502C66DDCB64521972E5356F4DB965F553853D53D4C69B4897F11B4780220595202D1E080345B65BAF8EBD6CA161C227F1B62C7E72EA5CA282B9434A6F042",
         "hash" : "3950D98AD20DA52EBB1F3937EF32F382D74092A4C8DF9A0B1A06ED25200B5756"
      }
   }
}
```

트랜잭션 결과가 **`tesSUCCESS`**인지 확인하세요. 그렇지 않으면 트랜잭션이 실패한 것입니다. stand-alone 모드 또는 비생산 네트워크에 문제가 있는 경우 다중 서명이 활성화되어 있는지 확인하세요.

{% hint style="info" %}
Note:

MultiSignReserve 수정안이 없으면 서명자 목록에 구성원이 많을수록 owner reserve를 위해 주소에 더 많은 XRP가 있어야 합니다. 주소에 충분한 XRP가 없으면 tecINSUFFICIENT\_RESERVE와 함께 트랜잭션이 실패합니다. MultiSignReserve amendment이 활성화된 상태에서 owner reserve을 위해 주소에 있어야 하는 XRP는 서명자 목록의 구성원 수에 관계없이 5 XRP입니다. 참조: 서명자 목록 및 예약.
{% endhint %}

## 4. 확인 대기 <a href="#4-wait-for-validation" id="4-wait-for-validation"></a>

라이브 네트워크(mainnet, testnet 또는 devnet)에서 ledger가 자동으로 닫힐 때까지 4-7초를 기다릴 수 있습니다.

`rippled` stand-alone 모드에서 실행 중인 경우 ledger\_accept 메소드를 사용하여 ledger를 수동으로 닫습니다.

## 5. 새 서명자 목록 확인 <a href="#5-confirm-the-new-signer-list" id="5-confirm-the-new-signer-list"></a>

[account\_objects 메소드를](https://xrpl.org/account\_objects.html) 사용하여 서명자 목록이 최근에 검증된 ledger의 주소와 연결되어 있는지 확인합니다.

일반적으로 계정은 다양한 유형의 많은 개체(예: 신뢰선 및 제안)를 소유할 수 있습니다. 이 자습서를 위해 새 주소에 자금을 지원한 경우 서명자 목록이 응답의 유일한 개체입니다.

```json
$ rippled account_objects rEuLyBCvcw4CFmzv8RepSiAoNgF8tTGJQC validated
Loading: "/etc/opt/ripple/rippled.cfg"
Connecting to 127.0.0.1:5005
{
   "result" : {
      "account" : "rEuLyBCvcw4CFmzv8RepSiAoNgF8tTGJQC",
      "account_objects" : [
         {
            "Flags" : 0,
            "LedgerEntryType" : "SignerList",
            "OwnerNode" : "0000000000000000",
            "PreviousTxnID" : "8FDC18960455C196A8C4DE0D24799209A21F4A17E32102B5162BD79466B90222",
            "PreviousTxnLgrSeq" : 5,
            "SignerEntries" : [
               {
                  "SignerEntry" : {
                     "Account" : "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW",
                     "SignerWeight" : 2
                  }
               },
               {
                  "SignerEntry" : {
                     "Account" : "raKEEVSGnKSD9Zyvxu4z6Pqpm4ABH8FS6n",
                     "SignerWeight" : 1
                  }
               },
               {
                  "SignerEntry" : {
                     "Account" : "rUpy3eEg8rqjqfUoLeBnZkscbKbFsKXC3v",
                     "SignerWeight" : 1
                  }
               }
            ],
            "SignerListID" : 0,
            "SignerQuorum" : 3,
            "index" : "79FD203E4DDDF2EA78B798C963487120C048C78652A28682425E47C96D016F92"
         }
      ],
      "ledger_hash" : "56E81069F06492FB410A70218C08169BE3AB3CFD5AEA20E999662D81DC361D9F",
      "ledger_index" : 5,
      "status" : "success",
      "validated" : true
   }
}
```

서명자 목록에 예상 내용이 있으면 주소가 다중 서명될 준비가 된 것입니다.

## 6. 추가 단계 <a href="#6-further-steps" id="6-further-steps"></a>

이 시점에서 귀하의 주소는 다중 서명 트랜잭션을 보낼 준비가 되었습니다. 다음을 수행할 수도 있습니다.

* 주소의 마스터 키 쌍을 비활성화합니다.
* SetRegularKey 트랜잭션을 전송하여 주소의 일반 키 쌍 (이전에 설정한 경우)을 제거합니다.

\
