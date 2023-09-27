# 오프라인 계정 설정 튜토리얼

매우 안전한 서명 구성에는 XRP Ledger 계정의 암호화 키를 오프라인의 에어갭 머신에 안전하게 보관하는 것이 포함됩니다. 이 구성을 설정한 후 다양한 트랜잭션에 서명하고 서명된 트랜잭션만 온라인 컴퓨터로 전송하고 비밀 키를 온라인에서 악의적인 행위자에게 노출하지 않고 XRP Ledger 네트워크에 제출할 수 있습니다.

{% hint style="info" %}
Caution:

오프라인 시스템을 보호하려면 적절한 운영 보안이 필요합니다. 예를 들어 오프라인 시스템은 신뢰할 수 없는 사람이 액세스할 수 없는 물리적 위치에 있어야 하며 신뢰할 수 있는 운영자는 손상된 소프트웨어를 시스템으로 전송하지 않도록 주의해야 합니다. (예를 들어, 이전에 네트워크로 연결된 컴퓨터에 연결된 USB 드라이브를 사용하지 마세요.)
{% endhint %}

## 요구 사항 <a href="#prerequisites" id="prerequisites"></a>

오프라인 서명을 사용하려면 다음 요구 사항을 충족해야 합니다.

* 오프라인 컴퓨터로 사용할 컴퓨터가 한 대 있어야 합니다. 이 시스템은 지원되는 운영 체제로 설정되어야 합니다. 오프라인 설정 지침은 운영 체제의 지원을 참조하세요. (예: Red Hat Enterprise Linux DVD ISO 설치 지침.) 사용하는 소프트웨어 및 물리적 미디어가 malware에 감염되지 않았는지 확인하세요.
* 온라인 컴퓨터로 사용하려면 별도의 컴퓨터가 있어야 합니다. 이 머신은 실행할 필요는 없지만 `rippled`XRP Ledger 네트워크에 연결하고 공유 ledger의 상태에 대한 정보를 받을 수 있어야 합니다. 예를 들어 공용 서버에 대한 WebSocket 연결을 사용할 수 있습니다.
* 온라인 컴퓨터로 사용하려면 별도의 컴퓨터가 있어야 합니다. 이 머신은 `rippled`를 실행할 필요는 없지만 XRP Ledger 네트워크에 연결하고 공유 ledger의 상태에 대한 정보를 수신할 수 있어야 합니다. 예를 들어 공용 서버에 대한 WebSocket 연결을 사용할 수 있습니다.
* 오프라인 시스템에서 온라인 시스템으로 서명된 트랜잭션 바이너리 데이터를 안전하게 전송할 수 있는 방법이 있어야 합니다.
  * 이를 수행하는 한 가지 방법은 오프라인 컴퓨터에서 QR 코드 생성기를 사용하고 온라인 컴퓨터에서 QR 코드 스캐너를 사용하는 것입니다. (이 경우 "온라인 머신"은 스마트폰과 같은 휴대용 장치일 수 있습니다.)
  * 또 다른 방법은 물리적 미디어를 사용하여 오프라인 시스템에서 온라인 시스템으로 파일을 복사하는 것입니다. 이 방법을 사용하는 경우 오프라인 시스템을 악성 소프트웨어로 감염시킬 수 있는 물리적 미디어를 사용하지 않도록 합니다. (예를 들어, 온라인 및 오프라인 시스템에서 동일한 USB 드라이브를 재사용하지 마십시오.)
  * 데이터를 온라인 컴퓨터에 수동으로 입력할 수 있지만 그렇게 하면 지루하고 오류가 발생하기 쉽습니다_._

## 단계 <a href="#steps" id="steps"></a>

### 1. 오프라인 머신 설정 <a href="#1-set-up-offline-machine" id="1-set-up-offline-machine"></a>

오프라인 시스템에는 안전한 영구 스토리지(예: 암호화된 디스크 드라이브)와 트랜잭션 서명 방법이 필요합니다. 오프라인 시스템의 경우 일반적으로 물리적 미디어를 사용하여 온라인 시스템에서 다운로드한 후 필요한 소프트웨어를 전송합니다. 온라인 컴퓨터, 물리적 미디어 및 소프트웨어 자체가 malware에 감염되지 않았는지 확인해야 합니다.

XRP Ledger에 서명하기 위한 소프트웨어 옵션은 다음과 같습니다.

* `rippled`패키지(`.deb`또는 `.rpm`사용하는 Linux 배포판에 따라 다름) 파일 에서 설치한 다음 그것을 stand-alone 모드에서 실행합니다.
* xrpl.js (또는 다른 클라이언트 라이브러리 ) 및 해당 종속 항목을 오프라인으로 설치합니다. 예를 들어 Yarn 패키지 관리자에는 오프라인 사용에 대한 권장 지침이 있습니다.
* 참조: 보안 서명 설정

오프라인 시스템에서 트랜잭션 지침을 구성하는 데 도움이 되는 사용자 정의 소프트웨어를 설정할 수 있습니다. 예를 들어 소프트웨어는 다음에 사용할 시퀀스 번호를 추적하거나 전송하려는 특정 유형의 트랜잭션에 대한 사전 설정 템플릿을 포함할 수 있습니다.

### 2. 암호화 키 생성 <a href="#2-generate-cryptographic-keys" id="2-generate-cryptographic-keys"></a>

오프라인 컴퓨터에서 계정에 사용할 암호화 키 쌍을 생성합니다. 키는 짧은 암호 구문이나 엔트로피가 충분하지 않은 다른 소스가 아닌 안전하게 임의의 절차로 생성해야 합니다. 예를 들어 다음과 같은 `rippled` 의 wallet\_propose 메소드를 사용할 수 있습니다.

```json
$ ./rippled wallet_propose
Loading: "/etc/opt/ripple/rippled.cfg"
2019-Dec-09 22:58:24.110862955 HTTPClient:NFO Connecting to 127.0.0.1:5005

{
   "result" : {
      "account_id" : "r4MRc4BArFPXmiDjmLdrufyFManSYhfKE6",
      "key_type" : "secp256k1",
      "master_key" : "JANE GIBE LIST TEND NU RUDE JIG PA FLOG DEFT SAME NASH",
      "master_seed" : "shYHSiJod8CLPTj1SNJ2PdUFj4pFk",
      "master_seed_hex" : "8465FDB80B2E2620A7D58274C26291A0",
      "public_key" : "aBQLW8imt7VChRJU1NMVCB7fE3jSL3VNEgLDKf88ygAhnfuZh3oo",
      "public_key_hex" : "03396074ED4B8155ACF9A8DC3665EFA53B5CFA0A1E91C3879303D37721EB222644",
      "status" : "success"
   }
}
```

다음 값에 유의하세요:

* **`account_id`**. 이 주소는 키 쌍과 연결된 주소가 XRP로 자금을 조달한 후(이 프로세스의 후반부) XRP Ledger에서 사용자의 계정 주소가 됩니다. `account_id`를 공개적으로 공유하는 것은 안전합니다.
* **`master_seed`**. 이것은 계정에서 트랜잭션에 서명하는 데 사용할 키 쌍의 비밀 시드 값입니다. 최상의 보안을 위해 이 값을 오프라인 시스템의 디스크에 쓰기 전에 암호화하십시오. 암호화 키로 적절하게 가중된 주사위로 생성된 diceware passphrase와 같이 작업자가 기억하거나 물리적으로 안전한 곳에 기록할 수 있는 보안 암호를 사용합니다. 물리적 보안 키를 두 번째 요소로 사용할 수도 있습니다. 이 단계에서 취해야 할 예방 조치의 범위는 당신에게 달려 있습니다.
* **`key_type`**. 이것은 이 키 쌍에 사용되는 암호화 알고리즘입니다. 가지고 있는 키 쌍의 유형을 알아야 합니다. 기본값은 `rippled`이지만 `secp256k1`일부 클라이언트 라이브러리는 `Ed25519`기본적으로 사용합니다.

`master_key`, `master_seed`또는 `master_seed_hex`값을 어디에도 공유 하지 마십시오. 이 주소와 관련된 개인 키를 재구성하는 데 사용할 수 있습니다.

### 3. 새 주소 자금 조달 <a href="#3-fund-the-new-address" id="3-fund-the-new-address"></a>

온라인 머신 1단계에서 기록한 계정 주소로 충분한 XRP를 보냅니다. 자세한 내용은 계정 만들기를 참조하세요.

{% hint style="info" %}
Tip:

테스트 목적으로 Testnet Faucet을 사용하여 Test XRP로 새 계정을 얻은 다음 해당 계정을 사용하여 오프라인에서 생성한 주소에 자금을 지원할 수 있습니다.
{% endhint %}

### 4. 계정 정보 확인 <a href="#4-confirm-account-details" id="4-confirm-account-details"></a>

이전 단계의 트랜잭션이 컨센서스에 의해 검증되면 계정이 생성됩니다. 온라인 기기에서 account\_info 메소드로 계정 상태를 확인할 수 있습니다. 응답에 `"validated": true`이 결과가 최종인지 확인하는 내용의 포함 여부를 확인하세요.

`Sequence`결과 필드 에 있는 계정의 시퀀스 번호를 기록해 둡니다 `account_data`. 이후 단계에서 계정에서 트랜잭션에 서명하려면 시퀀스 번호를 알아야 합니다.

`Sequence`새로 자금이 조달된 계정의 번호는 자금 이 조달되었을 때의 ledger 인덱스 와 일치합니다. DeletableAccounts 수정 이전에는 새로 입금된 계정의 `Sequence`번호가 항상 1이었습니다.

새로 자금 지원된 계정의 `Sequence`번호가 자금 지원 당시의 ledger 색인과 일치합니다. DeletableAccounts amendment 전에는 새로 자금을 지원받은 계정의 시퀀스 번호가 항상 1이었습니다.

```json
$ ./rippled account_info rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn

Loading: "/etc/opt/ripple/rippled.cfg"
2019-Dec-11 01:06:21.728637950 HTTPClient:NFO Connecting to 127.0.0.1:5005
{
   "result" : {
      "account_data" : {
         "Account" : "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
         "Balance" : "5000000000000",
         "Flags" : 0,
         "LedgerEntryType" : "AccountRoot",
         "OwnerCount" : 0,
         "PreviousTxnID" : "00C5B713B11DA775C6F932D38CE162C16FA88B7269BAFC6FDF4C6ADB74419670",
         "PreviousTxnLgrSeq" : 3,
         "Sequence" : 1,
         "index" : "13F1A95D7AAB7108D5CE7EEAF504B2894B8C674E6D68499076441C4837282BF8"
      },
      "ledger_current_index" : 4,
      "status" : "success",
      "validated" : false
   }
}
```

## 5. 오프라인 머신에서 시퀀스 번호를 입력합니다. <a href="#5-enter-the-sequence-number-on-the-offline-machine" id="5-enter-the-sequence-number-on-the-offline-machine"></a>

계정의 시작 시퀀스 번호를 오프라인 시스템에 저장합니다. 오프라인 머신을 사용하여 트랜잭션을 준비할 때마다 저장된 시퀀스 번호를 사용한 다음 시퀀스 번호를 1씩 증가시키고 새 값을 저장합니다.

이렇게 여러 거래를 미리 준비한 후 서명된 거래를 온라인 기계로 한꺼번에 전송하여 제출할 수 있습니다. 각 트랜잭션이 유효하게 형성되고 충분한 높은 트랜잭션 비용을 지불하는 한, XRP Ledger 네트워크는 최종적으로 이러한 트랜잭션을 유효한 레지스터에 포함시켜야 하며, 오프라인 컴퓨터에서 추적 중인 "현재" 시퀀스 번호와 동기화하여 공유 XRP Ledger에 있는 계정의 시퀀스 번호를 유지해야 합니다. (대부분의 트랜잭션은 네트워크에 제출된 후 15초 이내에 최종적으로 검증된 결과를 얻습니다.)

선택적으로 현재 ledger 인덱스를 오프라인 시스템에 저장합니다. `LastLedgerSequence`이 값을 사용하여 예정된 트랜잭션에 적절한 값을 선택할 수 있습니다.

## 6. 초기 설정 트랜잭션이 있는 경우 서명합니다. <a href="#6-sign-initial-setup-transactions-if-any" id="6-sign-initial-setup-transactions-if-any"></a>

오프라인 머신에서 계정 구성을 위한 트랜잭션을 준비하고 서명합니다. 세부 사항은 계정 사용 방법에 따라 다릅니다. 수행할 수 있는 작업의 몇 가지 예는 다음과 같습니다.

* 정기적으로 교체할 수 있는 일반 키 쌍을 할당합니다.
* 사용자가 결제를 보내는 조건 또는 대상 고객을 태그하지 않고 결제를 보낼 수 없도록 데스티네이션 태그를 요구합니다.
* 더 높은 수준의 계정 보안을 위해 다중 서명을 설정하십시오.
* DepositAuth를 활성화하면 귀하가 명시적으로 수락하거나 사전 승인한 당사자로부터만 지불을 받을 수 있습니다.
* 사용자가 허가 없이 신뢰선을 열 수 없도록 인증이 필요합니다. XRP Ledger의 분산형 교환 또는 토큰 기능을 사용할 계획이 없다면 예방 차원에서 이를 수행하는 것이 좋습니다.
* 토큰 발급자는 다음과 같은 추가 설정이 있을 수 있습니다.
  * 토큰을 전송하는 사용자에 대한 전송 수수료를 설정합니다.
  * 이 주소를 토큰에만 사용하려는 경우 XRP 지불을 허용하지 마십시오.

이 단계에서는 트랜잭션에 서명만 하고 제출은 하지 않습니다. 각 트랜잭션에 대해 수수료(거래 비용) 및 시퀀스(시퀀스 번호)와 같이 일반적으로 자동으로 입력할 수 있는 필드를 포함하여 모든 필드를 제공해야 합니다. 동시에 여러 트랜잭션을 준비하는 경우에는 트랜잭션을 실행할 순서대로 순차적으로 증가하는 시퀀스 번호를 사용해야 합니다.

예(Require Auth 활성화):

```json
$ rippled sign sn3nxiW7v8KXzPzAqzyHXbSSKNuN9 '{"Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn", "Fee": "12", "Sequence": 1, "TransactionType": "AccountSet", "SetFlag": 2}' offline

Loading: "/etc/opt/ripple/rippled.cfg"
2019-Dec-11 00:18:31.865955978 HTTPClient:NFO Connecting to 127.0.0.1:5005
{
   "result" : {
      "deprecated" : "This command has been deprecated and will be removed in a future version of the server. Please migrate to a standalone signing tool.",
      "status" : "success",
      "tx_blob" : "1200032280000000240000000120210000000268400000000000000C7321039543A0D3004CDA0904A09FB3710251C652D69EA338589279BC849D47A7B019A174473045022100D5C92D7705036CD7EBB601C8DFCD90927FA591A62AF832C489E9C898EC8E2FA0022052F1819340EB73E9749B8930A6935727362B8E141D1B2E246B49F912223FFD4381144B4E9C06F24296074F7BC48F92A97916C6DC5EA9",
      "tx_json" : {
         "Account" : "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
         "Fee" : "12",
         "Flags" : 2147483648,
         "Sequence" : 1,
         "SetFlag" : 2,
         "SigningPubKey" : "039543A0D3004CDA0904A09FB3710251C652D69EA338589279BC849D47A7B019A1",
         "TransactionType" : "AccountSet",
         "TxnSignature" : "3045022100D5C92D7705036CD7EBB601C8DFCD90927FA591A62AF832C489E9C898EC8E2FA0022052F1819340EB73E9749B8930A6935727362B8E141D1B2E246B49F912223FFD43",
         "hash" : "F81C34E7F05423DC1C973CB5008CA41AE984DE142EAA3975A749FABF0D08FA63"
      }
   }
}
```

_모든_ 트랜잭션이 제한된 시간 내에 최종 결과를 얻도록 하려면 `LastLedgerSequence`필드를 제공하세요. 이 값은 현재 ledger 인덱스(온라인 시스템에서 조회해야 함)와 트랜잭션이 유효한 상태로 유지되는 시간을 기반으로 해야 합니다. `LastLedgerSequence`온라인 컴퓨터에서 오프라인 컴퓨터로 전환하는 데 걸리는 시간을 허용할 수 있을 만큼 충분히 큰 값을 설정해야 합니다. 예를 들어 현재 ledger 인덱스보다 256 높은 값은 트랜잭션이 약 15분 동안 유효함을 의미합니다. 자세한 내용은 결과의 최종성 및 신뢰할 수 있는 트랜잭션 제출을 참조하십시오.

모든 트랜잭션이 제한된 시간 내에 최종 결과를 얻으려면 LastLeggerSequence 필드를 제공해야 합니다 이 값은 현재 ledger 인덱스(온라인 컴퓨터에서 조회해야 함)와 트랜잭션을 유효하게 유지할 시간을 기준으로 해야 합니다. 온라인 컴퓨터에서 오프라인 컴퓨터로 전환하거나 다시 전환하는 데 소요되는 시간을 허용할 만큼 충분히 큰 `LastLedgerSequence`값을 설정해야 합니다. 예를 들어, 현재 ledger 지수보다 256이 높으면 약 15분 동안 트랜잭션이 유효하다는 것을 의미합니다. 자세한 내용은 결과의 최종성 및 신뢰할 수 있는 트랜잭션 제출을 참조하세요.

## 7. 트랜잭션을 온라인 머신에 복사합니다. <a href="#7-copy-transactions-to-online-machine" id="7-copy-transactions-to-online-machine"></a>

트랜잭션에 서명한 후 다음 단계는 서명된 트랜잭션 데이터를 온라인 컴퓨터로 가져오는 것입니다. 이를 수행하는 방법에 대한 몇 가지 예는 요구 조건을 참조하세요.

## 8. 설정 트랜잭션을 제출합니다. <a href="#8-submit-setup-transactions" id="8-submit-setup-transactions"></a>

다음 단계는 트랜잭션을 제출하는 것입니다. 대부분의 트랜잭션은 제출 후(약 4초 후) 다음 검증된 ledger에 최종 결과가 있거나 대기 중인 경우(10초 미만) 그 이후의 ledger에 최종 결과가 있어야 합니다. 트랜잭션의 최종 결과를 추적하는 자세한 단계는 신뢰할 수 있는 트랜잭션 제출을 참조하세요.

트랜잭션 제출의 예:

```json
$ rippled submit 1200032280000000240000000120210000000268400000000000000C7321039543A0D3004CDA0904A09FB3710251C652D69EA338589279BC849D47A7B019A174473045022100D5C92D7705036CD7EBB601C8DFCD90927FA591A62AF832C489E9C898EC8E2FA0022052F1819340EB73E9749B8930A6935727362B8E141D1B2E246B49F912223FFD4381144B4E9C06F24296074F7BC48F92A97916C6DC5EA9

Loading: "/etc/opt/ripple/rippled.cfg"
2019-Dec-11 01:14:25.988839227 HTTPClient:NFO Connecting to 127.0.0.1:5005

{
   "result" : {
      "deprecated" : "Signing support in the 'submit' command has been deprecated and will be removed in a future version of the server. Please migrate to a standalone signing tool.",
      "engine_result" : "tesSUCCESS",
      "engine_result_code" : 0,
      "engine_result_message" : "The transaction was applied. Only final in a validated ledger.",
      "status" : "success",
      "tx_blob" : "1200032280000000240000000120210000000268400000000000000C7321039543A0D3004CDA0904A09FB3710251C652D69EA338589279BC849D47A7B019A174473045022100D5C92D7705036CD7EBB601C8DFCD90927FA591A62AF832C489E9C898EC8E2FA0022052F1819340EB73E9749B8930A6935727362B8E141D1B2E246B49F912223FFD4381144B4E9C06F24296074F7BC48F92A97916C6DC5EA9",
      "tx_json" : {
         "Account" : "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
         "Fee" : "12",
         "Flags" : 2147483648,
         "Sequence" : 1,
         "SetFlag" : 2,
         "SigningPubKey" : "039543A0D3004CDA0904A09FB3710251C652D69EA338589279BC849D47A7B019A1",
         "TransactionType" : "AccountSet",
         "TxnSignature" : "3045022100D5C92D7705036CD7EBB601C8DFCD90927FA591A62AF832C489E9C898EC8E2FA0022052F1819340EB73E9749B8930A6935727362B8E141D1B2E246B49F912223FFD43",
         "hash" : "F81C34E7F05423DC1C973CB5008CA41AE984DE142EAA3975A749FABF0D08FA63"
      }
   }
}
```

{% hint style="info" %}
Tip:

한 번에 10개 이상의 트랜잭션을 제출하는 경우 트랜잭션 대기열이 한 번에 동일한 보낸 사람의 트랜잭션을 10개로 제한하므로 한 번에 10개 이하의 그룹으로 제출하면 더 많은 성공을 거둘 수 있습니다. 10개 트랜잭션으로 구성된 각 그룹이 다음 그룹을 제출하기 전에 모든 트랜잭션
{% endhint %}

최종이 아닌 결과로 실패한 트랜잭션을 다시 제출해 보세요. 동일한 트랜잭션이 두 번 이상 처리될 가능성은 없습니다.

## 9. 거래의 최종 상태를 확인합니다. <a href="#9-confirm-the-final-status-of-the-transactions" id="9-confirm-the-final-status-of-the-transactions"></a>

제출한 각 거래에 대해 거래의 최종 결과 (예: tx 방법 사용)를 기록합니다. 예를 들어:

제출한 각 트랜잭션에 대해 tx 메소소드를 사용하는 등 트랜잭션의 최종 결과를 기록합니다. 예를 들어:

```json
$ ./rippled tx F81C34E7F05423DC1C973CB5008CA41AE984DE142EAA3975A749FABF0D08FA63

Loading: "/etc/opt/ripple/rippled.cfg"
2019-Dec-11 01:38:30.124771464 HTTPClient:NFO Connecting to 127.0.0.1:5005
{
   "result" : {
      "Account" : "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
      "Fee" : "12",
      "Flags" : 2147483648,
      "Sequence" : 1,
      "SetFlag" : 2,
      "SigningPubKey" : "039543A0D3004CDA0904A09FB3710251C652D69EA338589279BC849D47A7B019A1",
      "TransactionType" : "AccountSet",
      "TxnSignature" : "3045022100D5C92D7705036CD7EBB601C8DFCD90927FA591A62AF832C489E9C898EC8E2FA0022052F1819340EB73E9749B8930A6935727362B8E141D1B2E246B49F912223FFD43",
      "date" : 629343510,
      "hash" : "F81C34E7F05423DC1C973CB5008CA41AE984DE142EAA3975A749FABF0D08FA63",
      "inLedger" : 4,
      "ledger_index" : 4,
      "meta" : {
         "AffectedNodes" : [
            {
               "ModifiedNode" : {
                  "FinalFields" : {
                     "Account" : "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
                     "Balance" : "4999999999988",
                     "Flags" : 262144,
                     "OwnerCount" : 0,
                     "Sequence" : 2
                  },
                  "LedgerEntryType" : "AccountRoot",
                  "LedgerIndex" : "13F1A95D7AAB7108D5CE7EEAF504B2894B8C674E6D68499076441C4837282BF8",
                  "PreviousFields" : {
                     "Balance" : "5000000000000",
                     "Flags" : 0,
                     "Sequence" : 1
                  },
                  "PreviousTxnID" : "00C5B713B11DA775C6F932D38CE162C16FA88B7269BAFC6FDF4C6ADB74419670",
                  "PreviousTxnLgrSeq" : 3
               }
            }
         ],
         "TransactionIndex" : 0,
         "TransactionResult" : "tesSUCCESS"
      },
      "status" : "success",
      "validated" : true
   }
}
```

또한 모든 트랜잭션이 처리된 후 발송 계정의 account\_info를 확인하는 것이 유용할 수 있습니다. 계정의 현재 시퀀스 번호(시퀀스 필드)와 XRP 잔액(선택 사항)을 기록합니다.

실패한 모든 트랜잭션에 대해 수행할 작업을 결정해야 합니다:

* tefMAX\_LEDGER 코드로 트랜잭션이 실패한 경우 트랜잭션을 처리하려면 더 높은 트랜잭션 비용을 지정해야 할 수 있습니다. (이는 XRP Ledger 네트워크에 부하가 걸려 있음을 나타냅니다.) 트랜잭션을 더 높은 비용을 지불하고 `LastLedgerSequence` 매개 변수(있는 경우)가 더 높은 새 버전으로 바꿀 수 있습니다.
* 트랜잭션이 `tem-class`코드 로 인해 실패한 경우 트랜잭션 구성에서 오타나 다른 오류를 범했을 수 있습니다. 유효한 형식의 트랜잭션으로 바꿀 수 있도록 트랜잭션을 다시 확인하세요.
* 트랜잭션이 `tec-class` 코드 와 함께 실패한 경우 실패한 정확한 이유에 따라 사례 별로 해결해야 합니다.

조정하거나 교체하기로 결정한 트랜잭션의 경우 오프라인 시스템으로 돌아갈 때의 세부 정보를 기록해 두세요.

## 10. 오프라인 머신 상태를 조정합니다. <a href="#10-reconcile-offline-machine-status" id="10-reconcile-offline-machine-status"></a>

오프라인 컴퓨터로 돌아가서 다음과 같은 사용자 지정 서버의 저장된 설정에 필요한 변경 사항을 적용합니다:

* 계정의 현재 `Sequence`번호를 업데이트합니다. 모든 트랜잭션이 검증된 ledger에 포함된 경우(성공적으로 또는 `tec`코드 포함) 오프라인 시스템의 저장된 시퀀스 번호가 이미 정확해야 합니다. 그렇지 않으면 이전 단계에서 기록한 `Sequence`값과 일치하도록 저장된 시퀀스 번호를 변경해야 할 수 있습니다 .
* 새 트랜잭션에서 적절한 `LastLedgerSequence`값을 사용할 수 있도록 현재 ledger 인덱스를 업데이트합니다. (새 트랜잭션을 구성하기 직전에 항상 이 작업을 수행해야 합니다.)
* _(선택 사항)_ 오프라인 시스템에서 추적하는 경우 사용 가능한 실제 XRP 양을 업데이트합니다.

그런 다음 이전 단계에서 실패한 트랜잭션에 대한 대체 트랜잭션을 조정하고 서명합니다. 오프라인 시스템에서 트랜잭션 구성, 전송 및 온라인 시스템에서 제출에 대한 이전 단계를 반복합니다.
