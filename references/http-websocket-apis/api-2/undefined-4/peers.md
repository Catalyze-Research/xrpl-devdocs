# peers

peers 커맨드는 현재 피어 프로토콜을 통해 이 서버에 연결된 다른 모든 rippled 서버의 목록과 해당 서버의 연결 및 동기화 상태를 반환합니다.

peers 메소드는 권한이 없는 사용자가 실행할 수 없는 관리자 메소드입니다!

## 요청 형식

요청 형식의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": 2,
    "command": "peers"
}
```
{% endtab %}

{% tab title="Commandline" %}
```json
rippled peers
```
{% endtab %}
{% endtabs %}

요청에는 추가 매개변수가 포함되지 않습니다.

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
  "id": "peers_example",
  "result": {
    "cluster": {},
    "peers": [
      {
        "address": "5.189.239.203:51235",
        "complete_ledgers": "51813132 - 51815132",
        "ledger": "99A1E29C9F235DCCBB087F85F11756BECA606A756C22AB826AB1F319C470C3E3",
        "load": 157,
        "metrics": {
          "avg_bps_recv": "10255",
          "avg_bps_sent": "2015",
          "total_bytes_recv": "356809",
          "total_bytes_sent": "74208"
        },
        "public_key": "n94ht2A9aBoARRhk1rwypZNVXJDiMN4qzs1Bd5KsQaSnN3WVy8Tw",
        "uptime": 2,
        "version": "rippled-1.4.0"
      },
      {
        "address": "[::ffff:50.22.123.222]:51235",
        "complete_ledgers": "32570 - 51815131",
        "ledger": "99A1E29C9F235DCCBB087F85F11756BECA606A756C22AB826AB1F319C470C3E3",
        "load": 219,
        "metrics": {
          "avg_bps_recv": "7223",
          "avg_bps_sent": "6742",
          "total_bytes_recv": "593148",
          "total_bytes_sent": "204540"
        },
        "public_key": "n9LbkoB9ReSbaA9SGL317fm6CvjLcFG8hGoierLYfwiCDsEXHcP3",
        "uptime": 3,
        "version": "rippled-1.3.1"
      },
      {
        "address": "51.89.153.154:51235",
        "complete_ledgers": "51814130 - 51815130",
        "ledger": "808800914218F5622ED5F639BC0EEDF9530E47C6F81CD1EB3866FA1496F62B9C",
        "load": 27,
        "metrics": {
          "avg_bps_recv": "172748",
          "avg_bps_sent": "7355",
          "total_bytes_recv": "5455031",
          "total_bytes_sent": "234276"
        },
        "public_key": "n944vHEtDhPm4Bd9rPowZMJduR9XrpxKS9AKHFfcJtKRVAdhfFfD",
        "uptime": 8,
        "version": "rippled-1.4.0"
      },
      {
        "address": "192.151.157.20:51235",
        "complete_ledgers": "51813128 - 51815128",
        "latency": 8000,
        "load": 70,
        "metrics": {
          "avg_bps_recv": "463258",
          "avg_bps_sent": "21954",
          "total_bytes_recv": "13910029",
          "total_bytes_sent": "678908"
        },
        "public_key": "n9JbUWaFZDi1UxFexJXf1D9dRpn8UK6pTNwRxBCjEvLEwQa384uP",
        "uptime": 19,
        "version": "rippled-1.3.1"
      },
      {
        "address": "[::ffff:94.237.45.66]:51235",
        "complete_ledgers": "51815004 - 51815131",
        "ledger": "99A1E29C9F235DCCBB087F85F11756BECA606A756C22AB826AB1F319C470C3E3",
        "load": 202,
        "metrics": {
          "avg_bps_recv": "18258",
          "avg_bps_sent": "1903",
          "total_bytes_recv": "1184272",
          "total_bytes_sent": "65101"
        },
        "public_key": "n9Lg83FYh8YDivG9TcgXhq5Y3PwunmRqVfvibd19Ko9uu3DtqLBM",
        "uptime": 2,
        "version": "rippled-1.3.1"
      },
      {
        "address": "[::ffff:149.56.21.37]:51235",
        "complete_ledgers": "51478129 - 51815129",
        "ledger": "462FA0B34723C12EBE4DC9974B397D6A21D4F1770DE3CD584D4E33DBC83FC247",
        "load": 182,
        "metrics": {
          "avg_bps_recv": "132648",
          "avg_bps_sent": "13935",
          "total_bytes_recv": "3983628",
          "total_bytes_sent": "433757"
        },
        "public_key": "n94Dutms7xoSgWjbjYftDJGXvc5jaeYZq3J7o1Jwo35dZAyuzNWT",
        "uptime": 14,
        "version": "rippled-1.3.1"
      },
      {
        "address": "77.117.40.191:51235",
        "complete_ledgers": "51812267 - 51815071",
        "latency": 30555,
        "ledger": "0562C7A1898196FDC60B8DB1838961BFBA6B9304B1510E6030B32A4FB38400FB",
        "load": 3783,
        "metrics": {
          "avg_bps_recv": "495673",
          "avg_bps_sent": "57527",
          "total_bytes_recv": "17137378",
          "total_bytes_sent": "4140188"
        },
        "public_key": "n9LELaTdzKwfzhvHZKKrD3ZEJtWABSg4ocAT4dz5Vg5JgAVGwVdd",
        "uptime": 73,
        "version": "rippled-1.4.0"
      },
      {
        "address": "[::ffff:94.237.49.50]:51235",
        "complete_ledgers": "51815002 - 51815130",
        "latency": 8000,
        "ledger": "C6030B52471F8076F88A90C6FC2B2998794B32023FAC69FF573437D6D1470961",
        "load": 1563,
        "metrics": {
          "avg_bps_recv": "215539",
          "avg_bps_sent": "25705",
          "total_bytes_recv": "23134383",
          "total_bytes_sent": "2738880"
        },
        "public_key": "n9MLzBWq7WNM2sdpRKY2Tr3EJfxwSwsnDEpxU3auGJHgvq3Bit6S",
        "uptime": 112,
        "version": "rippled-1.3.1"
      }
    ]
  },
  "status": "success",
  "type": "response"
}
```
{% endtab %}

{% tab title="JSON-RPC" %}
```json
{
   "result" : {
      "cluster" : {},
      "peers" : [
         {
            "address" : "50.22.123.222:51235",
            "complete_ledgers" : "32570 - 51815097",
            "ledger" : "223DB74FE021AB1A4AA9E1CC588E0DBCC3FC7C080B93C01C30C246D89F951EA2",
            "load" : 7,
            "metrics" : {
               "avg_bps_recv" : "1152",
               "avg_bps_sent" : "332",
               "total_bytes_recv" : "96601",
               "total_bytes_sent" : "45322"
            },
            "public_key" : "n9LbkoB9ReSbaA9SGL317fm6CvjLcFG8hGoierLYfwiCDsEXHcP3",
            "uptime" : 1,
            "version" : "rippled-1.3.1"
         },
         {
            "address" : "212.83.147.67:51235",
            "complete_ledgers" : "51815014 - 51815040",
            "ledger" : "223DB74FE021AB1A4AA9E1CC588E0DBCC3FC7C080B93C01C30C246D89F951EA2",
            "load" : 1,
            "metrics" : {
               "avg_bps_recv" : "0",
               "avg_bps_sent" : "1490",
               "total_bytes_recv" : "18348",
               "total_bytes_sent" : "46013"
            },
            "public_key" : "n94s5V53w1g4HdEdHdUU1FVrqHTVDbcb7bt44ib9JcM3c281LoDr",
            "sanity" : "unknown",
            "uptime" : 2,
            "version" : "rippled-1.3.1"
         },
         {
            "address" : "158.69.24.50:51235",
            "complete_ledgers" : "51478098 - 51815098",
            "ledger" : "223DB74FE021AB1A4AA9E1CC588E0DBCC3FC7C080B93C01C30C246D89F951EA2",
            "load" : 55,
            "metrics" : {
               "avg_bps_recv" : "88080",
               "avg_bps_sent" : "2703",
               "total_bytes_recv" : "2786780",
               "total_bytes_sent" : "89368"
            },
            "public_key" : "n9KfEhmmdxmjJdpbpRHGJ9ezoNzdyUepA11cT71jmq1fMDsZAcSh",
            "uptime" : 3,
            "version" : "rippled-1.3.1"
         },
         {
            "address" : "[::ffff:174.64.99.193]:51235",
            "complete_ledgers" : "51813091 - 51815091",
            "latency" : 16000,
            "ledger" : "CF72319DC762355C92BDD29E4CE066CEB03FF2A077A511D586B9FD7B74F55D94",
            "load" : 325,
            "metrics" : {
               "avg_bps_recv" : "19012",
               "avg_bps_sent" : "52053",
               "total_bytes_recv" : "586809",
               "total_bytes_sent" : "1678192"
            },
            "public_key" : "n9MH4Xu8FYPPoUFs679NQp7F6epFznM7x6bF4sAJWQvKkPBUHgd3",
            "uptime" : 26,
            "version" : "rippled-1.4.0-b8"
         },
         {
            "address" : "[::ffff:94.237.45.66]:51235",
            "complete_ledgers" : "51814966 - 51815093",
            "latency" : 8773,
            "ledger" : "61CF015A709122917B001367EE81E5E0D56E485A0BCAB53785A1CB830E0F9589",
            "load" : 3522,
            "metrics" : {
               "avg_bps_recv" : "368875",
               "avg_bps_sent" : "59308",
               "total_bytes_recv" : "11558753",
               "total_bytes_sent" : "2257872"
            },
            "public_key" : "n9Lg83FYh8YDivG9TcgXhq5Y3PwunmRqVfvibd19Ko9uu3DtqLBM",
            "uptime" : 37,
            "version" : "rippled-1.3.1"
         }
      ],
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
      "cluster" : {},
      "peers" : [
         {
            "address" : "50.22.123.222:51235",
            "complete_ledgers" : "32570 - 51815097",
            "ledger" : "223DB74FE021AB1A4AA9E1CC588E0DBCC3FC7C080B93C01C30C246D89F951EA2",
            "load" : 7,
            "metrics" : {
               "avg_bps_recv" : "1152",
               "avg_bps_sent" : "332",
               "total_bytes_recv" : "96601",
               "total_bytes_sent" : "45322"
            },
            "public_key" : "n9LbkoB9ReSbaA9SGL317fm6CvjLcFG8hGoierLYfwiCDsEXHcP3",
            "uptime" : 1,
            "version" : "rippled-1.3.1"
         },
         {
            "address" : "212.83.147.67:51235",
            "complete_ledgers" : "51815014 - 51815040",
            "ledger" : "223DB74FE021AB1A4AA9E1CC588E0DBCC3FC7C080B93C01C30C246D89F951EA2",
            "load" : 1,
            "metrics" : {
               "avg_bps_recv" : "0",
               "avg_bps_sent" : "1490",
               "total_bytes_recv" : "18348",
               "total_bytes_sent" : "46013"
            },
            "public_key" : "n94s5V53w1g4HdEdHdUU1FVrqHTVDbcb7bt44ib9JcM3c281LoDr",
            "sanity" : "unknown",
            "uptime" : 2,
            "version" : "rippled-1.3.1"
         },
         {
            "address" : "158.69.24.50:51235",
            "complete_ledgers" : "51478098 - 51815098",
            "ledger" : "223DB74FE021AB1A4AA9E1CC588E0DBCC3FC7C080B93C01C30C246D89F951EA2",
            "load" : 55,
            "metrics" : {
               "avg_bps_recv" : "88080",
               "avg_bps_sent" : "2703",
               "total_bytes_recv" : "2786780",
               "total_bytes_sent" : "89368"
            },
            "public_key" : "n9KfEhmmdxmjJdpbpRHGJ9ezoNzdyUepA11cT71jmq1fMDsZAcSh",
            "uptime" : 3,
            "version" : "rippled-1.3.1"
         },
         {
            "address" : "[::ffff:174.64.99.193]:51235",
            "complete_ledgers" : "51813091 - 51815091",
            "latency" : 16000,
            "ledger" : "CF72319DC762355C92BDD29E4CE066CEB03FF2A077A511D586B9FD7B74F55D94",
            "load" : 325,
            "metrics" : {
               "avg_bps_recv" : "19012",
               "avg_bps_sent" : "52053",
               "total_bytes_recv" : "586809",
               "total_bytes_sent" : "1678192"
            },
            "public_key" : "n9MH4Xu8FYPPoUFs679NQp7F6epFznM7x6bF4sAJWQvKkPBUHgd3",
            "uptime" : 26,
            "version" : "rippled-1.4.0-b8"
         },
         {
            "address" : "[::ffff:94.237.45.66]:51235",
            "complete_ledgers" : "51814966 - 51815093",
            "latency" : 8773,
            "ledger" : "61CF015A709122917B001367EE81E5E0D56E485A0BCAB53785A1CB830E0F9589",
            "load" : 3522,
            "metrics" : {
               "avg_bps_recv" : "368875",
               "avg_bps_sent" : "59308",
               "total_bytes_recv" : "11558753",
               "total_bytes_sent" : "2257872"
            },
            "public_key" : "n9Lg83FYh8YDivG9TcgXhq5Y3PwunmRqVfvibd19Ko9uu3DtqLBM",
            "uptime" : 37,
            "version" : "rippled-1.3.1"
         }
      ],
      "status" : "success"
   }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함된 JSON 객체가 포함됩니다:

| Field   | 유형 | 설명                                              |
| ------- | -- | ----------------------------------------------- |
| cluster | 객체 | 클러스터로 구성된 경우 동일한 클러스터에 있는 다른 rippled 서버의 요약입니다. |
| peers   | 정렬 | 피어 객체의 배열입니다.                                   |

클러스터 객체의 각 필드는 해당 rippled 서버의 식별 키 쌍의 공개 키입니다. (이 값은 해당 서버가 server\_info 메소드에서 pubkey\_node로 반환하는 값과 동일합니다.) 해당 필드의 내용은 다음 필드를 가진 객체입니다:

| Field | 유형  | 설명                                         |
| ----- | --- | ------------------------------------------ |
| tag   | 문자열 | 구성 파일에 정의된 이 클러스터 멤버의 표시 이름입니다.            |
| fee   | 숫자  | (생략 가능) 이 클러스터 멤버가 트랜잭션 비용에 적용하는 부하 승수입니다. |
| age   | 숫자  | 이 클러스터 멤버의 마지막 클러스터 보고서 이후 경과한 시간(초)입니다.   |

피어 배열의 각 멤버는 다음 필드를 가진 피어 객체입니다:

| Field             | 유형      | 설명                                                                                                                                       |
| ----------------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| address           | 문자열     | 이 피어가 연결된 IP 주소 및 포트입니다.                                                                                                                 |
| cluster           | Boolean | (생략 가능) 참이면 현재 서버와 피어 서버가 동일한 rippled 클러스터의 일부입니다.                                                                                       |
| name              | 문자열     | (생략 가능) 피어가 동일한 클러스터의 일부인 경우, 구성 파일에 정의된 해당 서버의 표시 이름입니다.                                                                                |
| complete\_ledgers | 문자열     | 피어 rippled 서버가 사용할 수 있는 원장 버전의 원장 인덱스를 나타내는 범위 표현식입니다.                                                                                   |
| inbound           | Boolean | (생략 가능) 참이면 피어가 로컬 서버에 연결 중입니다.                                                                                                          |
| latency           | 숫자      | 피어에 대한 네트워크 지연 시간(밀리초)                                                                                                                   |
| ledger            | 문자열     | 피어에서 가장 최근에 닫은 원장의 식별 해시입니다.                                                                                                             |
| load              | 숫자      | 피어 서버가 로컬 서버에 가하는 부하량을 측정한 값입니다. 숫자가 클수록 부하가 많음을 나타냅니다. (부하를 측정하는 단위는 공식적으로 정의되어 있지 않습니다.)                                               |
| protocol          | 문자열     | (생략 가능) 로컬 서버와 동일하지 않은 경우 피어에서 사용 중인 프로토콜 버전입니다.                                                                                         |
| metrics           | 객체      | 이 피어와 주고받은 데이터의 양에 대한 세부 정보입니다. 자세한 내용은 아래 메트릭 객체 설명을 참조하세요.                                                                             |
| public\_key       | 문자열     | (생략 가능) 피어 메시지의 무결성을 확인하는 데 사용할 수 있는 공개 키입니다. 유효성 검사에 사용되는 키와 동일하지는 않지만 동일한 형식을 따릅니다.                                                    |
| sanity            | 문자열     | (생략 가능) 이 피어가 현재 서버와 동일한 규칙 및 원장 기록을 따르고 있는지 여부입니다. 값이 insane이면 피어가 병렬 네트워크의 일부임을 나타냅니다. 알 수 없음 값은 현재 서버가 피어가 호환되는지 여부를 확신할 수 없음을 나타냅니다. |
| status            | 문자열     | (생략 가능) 피어의 가장 최근 상태 메시지입니다. 연결 중, 연결됨, 모니터링 중, 유효성 검사 중 또는 종료 중일 수 있습니다.                                                                |
| uptime            | 숫자      | rippled 서버가 이 피어에 계속 연결되어 있는 시간(초)입니다.                                                                                                   |
| version           | 문자열     | (생략 가능) 피어 서버의 rippled 버전 번호입니다.                                                                                                         |

메트릭 객체에는 다음 필드가 포함됩니다:

| Field              | 유형  | 설명                             |
| ------------------ | --- | ------------------------------ |
| avg\_bps\_recv     | 문자열 | 이 피어로부터 수신된 데이터의 초당 평균 바이트입니다. |
| avg\_bps\_sent     | 문자열 | 이 피어로 전송된 데이터의 초당 평균 바이트 수입니다. |
| total\_bytes\_recv | 문자열 | 이 피어로부터 수신된 데이터의 총 바이트 수입니다.   |
| total\_bytes\_sent | 문자열 | 이 피어로 전송된 데이터의 총 바이트 수입니다.     |

{% hint style="info" %}
Note:

메트릭 객체의 모든 필드는 JSON 인코딩/디코딩에서 정밀도를 잃지 않도록 문자열 형식으로 직렬화된 64비트 부호 없는 정수입니다.
{% endhint %}

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
* `reportingUnsupported` - (리포팅 모드 서버만 해당) 이 메소드는 리포팅 모드에서 사용할 수 없습니다
