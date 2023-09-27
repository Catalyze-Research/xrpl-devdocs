# consensus\_info

consensus\_info 커맨드는 디버깅 목적으로 컨센서스 프로세스에 대한 정보를 제공합니다.

consensus\_info 메소드는 권한이 없는 사용자가 실행할 수 없는 관리자 메소드입니다.

## 요청 형식

요청 형식의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": 99,
    "command": "consensus_info"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
    "method": "consensus_info",
    "params": [
        {}
    ]
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
#Syntax: consensus_info
rippled consensus_info
```
{% endtab %}
{% endtabs %}

요청에 매개변수가 없습니다.

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="JSON-RPC" %}
```json
{
   "result" : {
      "info" : {
         "acquired" : {
            "4BC2CE596CBD1321775320E2067F9C06D3862826212C16EF42ABB6A2B0414306" : "acquired"
         },
         "close_granularity" : 10,
         "close_percent" : 50,
         "close_resolution" : 10,
         "close_times" : {
            "486082972" : 1,
            "486082973" : 4
         },
         "current_ms" : 1003,
         "have_time_consensus" : false,
         "ledger_seq" : 13701086,
         "our_position" : {
            "close_time" : 486082973,
            "previous_ledger" : "0BB01379B51234BAAF501A71C7AB147F595460B689BB9E8252A0B87B5A483623",
            "propose_seq" : 0,
            "transaction_hash" : "4BC2CE596CBD1321775320E2067F9C06D3862826212C16EF42ABB6A2B0414306"
         },
         "peer_positions" : {
            "0A2EAF919033A036D363D4E5610A66209DDBE8EE" : {
               "close_time" : 486082972,
               "peer_id" : "n9KiYM9CgngLvtRCQHZwgC2gjpdaZcCcbt3VboxiNFcKuwFVujzS",
               "previous_ledger" : "0BB01379B51234BAAF501A71C7AB147F595460B689BB9E8252A0B87B5A483623",
               "propose_seq" : 0,
               "transaction_hash" : "4BC2CE596CBD1321775320E2067F9C06D3862826212C16EF42ABB6A2B0414306"
            },
            "1567A8C953A86F8428C7B01641D79BBF2FD508F3" : {
               "close_time" : 486082973,
               "peer_id" : "n9LdgEtkmGB9E2h3K4Vp7iGUaKuq23Zr32ehxiU8FWY7xoxbWTSA",
               "previous_ledger" : "0BB01379B51234BAAF501A71C7AB147F595460B689BB9E8252A0B87B5A483623",
               "propose_seq" : 0,
               "transaction_hash" : "4BC2CE596CBD1321775320E2067F9C06D3862826212C16EF42ABB6A2B0414306"
            },
            "202397A81F20B44CF44EA99AF761295E5A8397D2" : {
               "close_time" : 486082973,
               "peer_id" : "n9MD5h24qrQqiyBC8aeqqCWvpiBiYQ3jxSr91uiDvmrkyHRdYLUj",
               "previous_ledger" : "0BB01379B51234BAAF501A71C7AB147F595460B689BB9E8252A0B87B5A483623",
               "propose_seq" : 0,
               "transaction_hash" : "4BC2CE596CBD1321775320E2067F9C06D3862826212C16EF42ABB6A2B0414306"
            },
            "5C29005CF4FB479FC49EEFB4A5B075C86DD963CC" : {
               "close_time" : 486082973,
               "peer_id" : "n9L81uNCaPgtUJfaHh89gmdvXKAmSt5Gdsw2g1iPWaPkAHW5Nm4C",
               "previous_ledger" : "0BB01379B51234BAAF501A71C7AB147F595460B689BB9E8252A0B87B5A483623",
               "propose_seq" : 0,
               "transaction_hash" : "4BC2CE596CBD1321775320E2067F9C06D3862826212C16EF42ABB6A2B0414306"
            },
            "EFC49EB648E557CC50A72D715249B80E071F7705" : {
               "close_time" : 486082973,
               "peer_id" : "n949f75evCHwgyP4fPVgaHqNHxUVN15PsJEZ3B3HnXPcPjcZAoy7",
               "previous_ledger" : "0BB01379B51234BAAF501A71C7AB147F595460B689BB9E8252A0B87B5A483623",
               "propose_seq" : 0,
               "transaction_hash" : "4BC2CE596CBD1321775320E2067F9C06D3862826212C16EF42ABB6A2B0414306"
            }
         },
         "previous_mseconds" : 2005,
         "previous_proposers" : 5,
         "proposers" : 5,
         "proposing" : false,
         "state" : "consensus",
         "synched" : true,
         "validating" : false
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
         "acquired" : {
            "4BC2CE596CBD1321775320E2067F9C06D3862826212C16EF42ABB6A2B0414306" : "acquired"
         },
         "close_granularity" : 10,
         "close_percent" : 50,
         "close_resolution" : 10,
         "close_times" : {
            "486082972" : 1,
            "486082973" : 4
         },
         "current_ms" : 1003,
         "have_time_consensus" : false,
         "ledger_seq" : 13701086,
         "our_position" : {
            "close_time" : 486082973,
            "previous_ledger" : "0BB01379B51234BAAF501A71C7AB147F595460B689BB9E8252A0B87B5A483623",
            "propose_seq" : 0,
            "transaction_hash" : "4BC2CE596CBD1321775320E2067F9C06D3862826212C16EF42ABB6A2B0414306"
         },
         "peer_positions" : {
            "0A2EAF919033A036D363D4E5610A66209DDBE8EE" : {
               "close_time" : 486082972,
               "peer_id" : "n9KiYM9CgngLvtRCQHZwgC2gjpdaZcCcbt3VboxiNFcKuwFVujzS",
               "previous_ledger" : "0BB01379B51234BAAF501A71C7AB147F595460B689BB9E8252A0B87B5A483623",
               "propose_seq" : 0,
               "transaction_hash" : "4BC2CE596CBD1321775320E2067F9C06D3862826212C16EF42ABB6A2B0414306"
            },
            "1567A8C953A86F8428C7B01641D79BBF2FD508F3" : {
               "close_time" : 486082973,
               "peer_id" : "n9LdgEtkmGB9E2h3K4Vp7iGUaKuq23Zr32ehxiU8FWY7xoxbWTSA",
               "previous_ledger" : "0BB01379B51234BAAF501A71C7AB147F595460B689BB9E8252A0B87B5A483623",
               "propose_seq" : 0,
               "transaction_hash" : "4BC2CE596CBD1321775320E2067F9C06D3862826212C16EF42ABB6A2B0414306"
            },
            "202397A81F20B44CF44EA99AF761295E5A8397D2" : {
               "close_time" : 486082973,
               "peer_id" : "n9MD5h24qrQqiyBC8aeqqCWvpiBiYQ3jxSr91uiDvmrkyHRdYLUj",
               "previous_ledger" : "0BB01379B51234BAAF501A71C7AB147F595460B689BB9E8252A0B87B5A483623",
               "propose_seq" : 0,
               "transaction_hash" : "4BC2CE596CBD1321775320E2067F9C06D3862826212C16EF42ABB6A2B0414306"
            },
            "5C29005CF4FB479FC49EEFB4A5B075C86DD963CC" : {
               "close_time" : 486082973,
               "peer_id" : "n9L81uNCaPgtUJfaHh89gmdvXKAmSt5Gdsw2g1iPWaPkAHW5Nm4C",
               "previous_ledger" : "0BB01379B51234BAAF501A71C7AB147F595460B689BB9E8252A0B87B5A483623",
               "propose_seq" : 0,
               "transaction_hash" : "4BC2CE596CBD1321775320E2067F9C06D3862826212C16EF42ABB6A2B0414306"
            },
            "EFC49EB648E557CC50A72D715249B80E071F7705" : {
               "close_time" : 486082973,
               "peer_id" : "n949f75evCHwgyP4fPVgaHqNHxUVN15PsJEZ3B3HnXPcPjcZAoy7",
               "previous_ledger" : "0BB01379B51234BAAF501A71C7AB147F595460B689BB9E8252A0B87B5A483623",
               "propose_seq" : 0,
               "transaction_hash" : "4BC2CE596CBD1321775320E2067F9C06D3862826212C16EF42ABB6A2B0414306"
            }
         },
         "previous_mseconds" : 2005,
         "previous_proposers" : 5,
         "proposers" : 5,
         "proposing" : false,
         "state" : "consensus",
         "synched" : true,
         "validating" : false
      },
      "status" : "success"
   }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

| Field | 유형 | 설명                                                    |
| ----- | -- | ----------------------------------------------------- |
| info  | 객체 | 컨센서스를 디버깅하는 데 유용할 수 있는 정보입니다. 이 출력은 예고 없이 변경될 수 있습니다. |

다음은 정보 객체에 포함될 수 있는 필드에 대한 불완전한 요약입니다:

| Field           | 유형      | 설명                                                                   |
| --------------- | ------- | -------------------------------------------------------------------- |
| ledger\_seq     | 숫자      | 현재 합의 프로세스에 있는 원장의 원장 인덱스입니다.                                        |
| our\_position   | 객체      | 합의 프로세스에서 원장에 대한 서버의 기대치입니다.                                         |
| peer\_positions | 객체      | 합의 프로세스에서 피어와 이들이 제안한 원장의 버전 맵 오브젝트입니다.                              |
| proposers       | 숫자      | 이 합의 프로세스에 참여하는 신뢰할 수 있는 검증자의 수입니다. 신뢰할 수 있는 검증자는 이 서버의 구성에 따라 다릅니다. |
| synched         | Boolean | 이 서버가 네트워크와 동기화되었다고 생각하는지 여부입니다.                                     |
| state           | 문자열     | 합의 과정 중 현재 진행 중인 부분은 open, consensus, finished또는  accepted입니다.       |

정보에 포함된 유일한 필드가 "컨센서스"인 경우 최소한의 결과가 나오는 것도 정상입니다: "none". 이는 서버가 컨센서스 라운드 사이에 있음을 나타냅니다.

consensus\_info 명령을 여러 번, 심지어 짧은 연속으로 실행하면 결과가 크게 달라질 수 있습니다.

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* reportingUnsupported - (리포팅 모드 서버만 해당) 이 메소드는 리포팅 모드에서 사용할 수 없습니다.
