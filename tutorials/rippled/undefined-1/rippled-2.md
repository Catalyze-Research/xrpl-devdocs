# rippled 서버가 수정이 차단됨

rippled 서버가 수정이 차단되었다는 첫 번째 신호 중 하나는 트랜잭션을 제출할 때 반환되는 amendmentBlocked 오류입니다. 다음은 amendmentBlocked 오류의 예시입니다:

```json
{
   "result":{
      "error":"amendmentBlocked",
      "error_code":14,
      "error_message":"Amendment blocked, need upgrade.",
      "request":{
         "command":"submit",
         "tx_blob":"479H0KQ4LUUXIHL48WCVN0C9VD7HWSX0MG1UPYNXK6PI9HLGBU2U10K3HPFJSROFEG5VD749WDPHWSHXXO72BOSY2G8TWUDOJNLRTR9LTT8PSOB9NNZ485EY2RD9D80FLDFRBVMP1RKMELILD7I922D6TBCAZK30CSV6KDEDUMYABE0XB9EH8C4LE98LMU91I9ZV2APETJD4AYFEN0VNMIT1XQ122Y2OOXO45GJ737HHM5XX88RY7CXHVWJ5JJ7NYW6T1EEBW9UE0NLB2497YBP9V1XVAEK8JJYVRVW0L03ZDXFY8BBHP6UBU7ZNR0JU9GJQPNHG0DK86S4LLYDN0BTCF4KWV2J4DEB6DAX4BDLNPT87MM75G70DFE9W0R6HRNWCH0X075WHAXPSH7S3CSNXPPA6PDO6UA1RCCZOVZ99H7968Q37HACMD8EZ8SU81V4KNRXM46N520S4FVZNSJHA"
      },
      "status":"error"
   }
}
```

다음 rippled 로그 메시지도 서버가 수정이 차단되었음을 나타냅니다:

```
2018-Feb-12 19:38:30 LedgerMaster:ERR One or more unsupported amendments activated: server blocked.
```

server\_info 명령을 사용하여 rippled 서버가 수정이 차단되었는지 확인할 수 있습니다. 응답에서 result.info.amendment\_blocked를 찾습니다. amendment\_blocked가 true로 설정되어 있으면 서버가 수정이 차단된 것입니다.

**JSON-RPC 응답 예시:**

```json
{
    "result": {
        "info": {
            "amendment_blocked": true,
            "build_version": "0.80.1",
            "complete_ledgers": "6658438-6658596",
            "hostid": "ip-10-30-96-212.us-west-2.compute.internal",
            "io_latency_ms": 1,
            "last_close": {
                "converge_time_s": 2,
                "proposers": 10
            },
...
        },
        "status": "success"
    }
}
```

## 서버 차단 해제

가장 쉬운 해결책은 최신 버전의 rippled 버전으로 업데이트하는 것이지만, 시나리오에 따라 서버를 차단하는 수정 사항이 있는 이전 버전으로 업데이트해야 할 수도 있습니다.

{% hint style="info" %}
Warning:

최신 rippled 버전에 보안 또는 기타 긴급한 수정 사항이 있는 경우 가능한 한 빨리 최신 버전으로 업그레이드해야 합니다.
{% endhint %}

최신 버전보다 오래된 버전으로 업그레이드하여 rippled 서버의 차단을 해제할 수 있는지 확인하려면 서버를 차단하는 기능을 찾은 다음 해당 차단 기능을 지원하는 rippled 버전을 찾아보세요.

rippled 서버를 차단하는 기능을 찾으려면 기능 관리자 명령을 사용하세요. 다음과 같은 기능이 있는 기능을 찾습니다:

```
"enabled" : true
"supported" : false
```

이 값은 최신 ledger에 수정이 필요하지만 서버에서 해당 구현을 지원하지 않는다는 의미입니다.

**JSON-RPC 응답 예시:**

```json
{
    "result": {
        "features": {
            "07D43DCE529B15A10827E5E04943B496762F9A88E3268269D69C44BE49E21104": {
                "enabled": true,
                "name": "Escrow",
                "supported": true,
                "vetoed": false
            },
            "08DE7D96082187F6E6578530258C77FAABABE4C20474BDB82F04B021F1A68647": {
                "enabled": true,
                "name": "PayChan",
                "supported": true,
                "vetoed": false
            },
            "1562511F573A19AE9BD103B5D6B9E01B3B46805AEC5D3C4805C902B514399146": {
                "enabled": false,
                "name": "CryptoConditions",
                "supported": true,
                "vetoed": false
            },
            "157D2D480E006395B76F948E3E07A45A05FE10230D88A7993C71F97AE4B1F2D1": {
                "enabled": true,
                "supported": false,
                "vetoed": false
            },
...
            "67A34F2CF55BFC0F93AACD5B281413176FEE195269FA6D95219A2DF738671172": {
                "enabled": true,
                "supported": false,
                "vetoed": false
            },
...
            "F64E1EABBE79D55B3BB82020516CEC2C582A98A6BFE20FBE9BB6A0D233418064": {
                "enabled": true,
                "supported": false,
                "vetoed": false
            }
        },
        "status": "success"
    }
}
```

이 예에서는 다음 기능과의 충돌로 인해 rippled 서버의 수정이 차단되었습니다:

* 157D2D480E006395B76F948E3E07A45A05FE10230D88A7993C71F97AE4B1F2D1
* 67A34F2CF55BFC0F93AACD5B281413176FEE195269FA6D95219A2DF738671172
* F64E1EABBE79D55B3BB82020516CEC2C582A98A6BFE20FBE9BB6A0D233418064

이러한 기능을 지원하는 rippled 버전을 찾으려면 알려진 수정 사항을 참조하세요.

&#x20;
