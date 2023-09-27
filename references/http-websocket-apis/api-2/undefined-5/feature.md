# feature

이 기능 명령은 서버가 알고 있는 수정사항에 대한 정보를 반환하며, 여기에는 해당 수정사항이 활성화되어 있는지 여부와 서버가 수정사항 프로세스에서 해당 수정사항에 찬성 투표를 하고 있는지 여부가 포함됩니다. [![New in: rippled 0.31.0](https://img.shields.io/badge/New%20in-rippled%200.31.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/0.31.0)

기능 명령을 사용하여 수정안에 대해 반대 또는 찬성 투표를 하도록 서버를 구성할 수 있습니다. 이 변경 사항은 서버를 다시 시작해도 지속됩니다. [![Updated in: rippled 1.7.0](https://img.shields.io/badge/Updated%20in-rippled%201.7.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/1.7.0)

_기능 메소드는 권한이 없는 사용자가 실행할 수 없는 관리자 메소드입니다._

## 요청 형식

요청 형식의 예입니다:

{% tabs %}
{% tab title="WebSocket - list all" %}
```json
{
  "id": "list_all_features",
  "command": "feature"
}
```
{% endtab %}

{% tab title="WebSocket - reject" %}
```json
{
  "id": "reject_multi_sign",
  "command": "feature",
  "feature": "4C97EBA926031A7CF7D7B36FDE3ED66DDA5421192D63DE53FFB46E43B9DC8373",
  "vetoed": true
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "feature",
    "params": [
        {
            "feature": "4C97EBA926031A7CF7D7B36FDE3ED66DDA5421192D63DE53FFB46E43B9DC8373",
            "vetoed": false
        }
    ]
}
```
{% endtab %}

{% tab title="Commandline" %}
```
#Syntax: feature [<feature_id> [accept|reject]]
rippled feature 4C97EBA926031A7CF7D7B36FDE3ED66DDA5421192D63DE53FFB46E43B9DC8373 accept
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| Field   | 유형      | 설명                                                                                                                                                                                              |
| ------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| feature | 문자열     | (선택 사항) 16진수로 된 수정본의 고유 ID 또는 수정본의 짧은 이름입니다. 제공된 경우 응답을 하나의 수정 사항으로 제한합니다. 그렇지 않으면 응답에 모든 수정안이 나열됩니다.                                                                                           |
| vetoed  | Boolean | (선택 사항, 기능도 지정하지 않으면 무시됨) 참이면 서버에 기능별로 지정된 수정안에 대해 투표하도록 지시합니다. 거짓이면 서버가 수정안에 찬성 투표하도록 지시합니다. 명령줄에서 '참' 또는 '거짓' 대신 '수락' 또는 '거부'를 사용합니다. 서버의 소스 코드에서 더 이상 사용되지 않는 것으로 표시된 수정안에는 찬성 투표를 할 수 없습니다. |

{% hint style="info" %}
Note:

기능 필드에 수정안 ID를 지정하여 서버가 현재 해당 수정안을 적용하는 방법을 모르는 경우에도 새 수정안에 찬성 투표하도록 서버를 구성할 수 있습니다. 예를 들어, 수정안을 지원하는 새 rippled 버전으로 곧 업그레이드할 계획이라면 이 작업을 수행할 수 있습니다.
{% endhint %}

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket - list all" %}
```json
{
  "id": "list_all_features",
  "status": "success",
  "type": "response",
  "result": {
    "features": {
      "42426C4D4F1009EE67080A9B7965B44656D7714D104A72F9B4369F97ABF044EE": {
        "enabled": false,
        "name": "FeeEscalation",
        "supported": true,
        "vetoed": false
      },
      "4C97EBA926031A7CF7D7B36FDE3ED66DDA5421192D63DE53FFB46E43B9DC8373": {
        "enabled": false,
        "name": "MultiSign",
        "supported": true,
        "vetoed": false
      },
      "6781F8368C4771B83E8B821D88F580202BCB4228075297B19E4FDC5233F1EFDC": {
        "enabled": false,
        "name": "TrustSetAuth",
        "supported": true,
        "vetoed": false
      },
      "C1B8D934087225F509BEB5A8EC24447854713EE447D277F69545ABFA0E0FD490": {
        "enabled": false,
        "name": "Tickets",
        "supported": true,
        "vetoed": false
      },
      "DA1BD556B42D85EA9C84066D028D355B52416734D3283F85E216EA5DA6DB7E13": {
        "enabled": false,
        "name": "SusPay",
        "supported": true,
        "vetoed": false
      }
    }
  }
}
```
{% endtab %}

{% tab title="WebSocket - reject" %}
```json
{
    "id": "reject_multi_sign",
    "status": "success",
    "type": "response",
    "result": {
        "features": {
            "4C97EBA926031A7CF7D7B36FDE3ED66DDA5421192D63DE53FFB46E43B9DC8373": {
                "enabled": false,
                "name": "MultiSign",
                "supported": true,
                "vetoed": true
            }
        }
    }
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
200 OK

{
    "result": {
        "4C97EBA926031A7CF7D7B36FDE3ED66DDA5421192D63DE53FFB46E43B9DC8373": {
            "enabled": false,
            "name": "MultiSign",
            "supported": true,
            "vetoed": false
        },
        "status": "success"
    }
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
Loading: "/etc/rippled.cfg"
Connecting to 127.0.0.1:5005

{
    "result": {
        "4C97EBA926031A7CF7D7B36FDE3ED66DDA5421192D63DE53FFB46E43B9DC8373": {
            "enabled": false,
            "name": "MultiSign",
            "supported": true,
            "vetoed": false
        },
        "status": "success"
    }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 수정안 맵이 JSON 객체로 포함됩니다. 객체의 키는 수정안 ID입니다. 각 키의 값은 해당 ID를 가진 수정안의 상태를 설명하는 수정안 객체입니다. 요청에서 기능을 지정한 경우 맵에는 요청의 변경 사항을 적용한 후 요청된 수정 객체만 포함됩니다. 각 수정 객체에는 다음과 같은 필드가 있습니다:

| Field     | 유형             | 설명                                                                                                                                                                       |
| --------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| enabled   | Boolean        | 이 수정 사항이 현재 최신 원장에서 활성화되어 있는지 여부입니다.                                                                                                                                     |
| name      | 문자열            | (생략될 수 있음) 알려진 경우 이 개정안의 사람이 읽을 수 있는 이름입니다.                                                                                                                              |
| supported | Boolean        | 서버가 이 수정안을 적용하는 방법을 알고 있는지 여부입니다. 이 필드가 false(서버가 이 수정안을 적용하는 방법을 모름)로 설정되어 있고 enabled(이 수정안이 최신 원장에서 활성화됨)가 true(이 수정안이 활성화됨)로 설정되어 있으면 이 수정안으로 인해 서버가 수정안이 차단될 수 있습니다. |
| vetoed    | Boolean 또는 문자열 | 대부분의 수정안의 경우, 서버가 이 수정안에 대해 투표하도록 지시받았는지 여부를 나타내는 부울 값입니다. 코드에서 더 이상 사용되지 않는 것으로 표시된 수정안의 경우, 이 값은 대신 사용되지 않는 문자열입니다.                                                    |

{% hint style="info" %}
Caution:

수정안 이름에 해당 수정의 기능이 정확히 표시되어 있지는 않습니다. 이 이름은 서버 간에 고유하거나 일관성이 보장되지 않습니다.
{% endhint %}

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* badFeature - 지정한 기능의 형식이 잘못되었거나 서버가 해당 이름의 수정본을 알지 못합니다.
* reportingUnsupported - (리포팅 모드 서버만 해당) 이 메소드는 리포팅 모드에서 사용할 수 없습니다.
