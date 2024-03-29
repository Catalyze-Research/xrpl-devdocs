# 상태 확인

치명적인상태 확인은 개별 rippled 서버의 상태를 보고하기 위한 특수 피어 포트 방법입니다. 이 방법은 중단을 인식하고 서버 재시작과 같은 자동 또는 수동 개입을 유도하기 위해 자동화된 모니터링에 사용하기 위한 것입니다. [![New in: rippled 1.6.0](https://img.shields.io/badge/New%20in-rippled%201.6.0-blue.svg)](https://github.com/ripple/rippled/releases/tag/1.6.0)

이 메소드는 여러 메트릭을 검사하여 일반적으로 정상으로 간주되는 범위에 있는지 확인합니다. 모든 메트릭이 정상 범위에 있으면 이 메소드는 서버가 정상이라고 보고합니다. 메트릭 중 하나라도 정상 범위를 벗어나면 서버가 정상적이지 않다고 보고하고 정상적이지 않은 메트릭을 보고합니다. 일부 메트릭은 비정상적인 범위 안팎으로 급격하게 변동할 수 있으므로 상태 확인이 연속으로 여러 번 실패하지 않는 한 경고를 발생시키지 않아야 합니다.

{% hint style="info" %}
Note:

상태 확인은 피어 포트 방식이므로 stand-alone 모드에서 서버를 테스트할 때는 사용할 수 없습니다.
{% endhint %}

## 요청 형식

상태 확인 정보를 요청하려면 다음 HTTP 요청을 보내세요:

* 프로토콜: https
* HTTP 메소드: GET
* 호스트: (rippled된 모든 서버, 호스트 이름 또는 IP 주소 기준)
* 포트: (rippled된 서버가 피어 프로토콜을 사용하는 포트 번호, 일반적으로 51235)
* 경로: /health
* 보안: 대부분의 rippled 서버는 자체 서명된 인증서를 사용하여 요청에 응답합니다. 기본적으로 대부분의 도구(웹 브라우저 포함)는 이러한 응답을 신뢰할 수 없는 것으로 플래그를 지정하거나 차단합니다. 이러한 서버의 응답을 표시하려면 인증서 검사를 무시해야 합니다(예: cURL을 사용하는 경우 --insecure 플래그 추가).

## 응답 예시

{% tabs %}
{% tab title="Healthy" %}
```json
HTTP/1.1 200 OK
Server: rippled-1.6.0-b8
Content-Type: application/json
Connection: close
Transfer-Encoding: chunked

{
  "info": {}
}
```
{% endtab %}

{% tab title="Warning" %}
```json
HTTP/1.1 503 Service Unavailable
Server: rippled-1.6.0
Content-Type: application/json
Connection: close
Transfer-Encoding: chunked

{
  "info": {
    "server_state": "connected",
    "validated_ledger": -1
  }
}
```
{% endtab %}

{% tab title="Critical" %}
```json
HTTP/1.1 500 Internal Server Error
Server: rippled-1.6.0
Content-Type: application/json
Connection: close
Transfer-Encoding: chunked

{
  "info": {
    "peers": 0,
    "server_state": "disconnected",
    "validated_ledger":-1
  }
}
```
{% endtab %}
{% endtabs %}

## 응답 형식

응답의 HTTP 상태 코드는 서버의 상태를 나타냅니다:

| 상태 코드                    | 상태  | 설명                                                           |
| ------------------------ | --- | ------------------------------------------------------------ |
| **200 확인**               | 좋음  | 모든 상태 지표가 허용 가능한 범위 내에 있습니다.                                 |
| **503 서비스를 사용할 수 없습니다.** | 경고  | 하나 이상의 측정 항목이 경고 범위에 있습니다. 수동 개입이 필요할 수도 있고 필요하지 않을 수도 있습니다. |
| **500 내부 서버 오류**         | 치명적 | 하나 이상의 측정 항목이 위험 범위에 있습니다. 해결하려면 수동 개입이 필요한 심각한 문제가 있습니다.    |

응답 본문은 최상위 레벨에 단일 정보 객체가 있는 JSON 객체입니다. 정보 객체에는 경고 또는 위험 범위에 있는 각 메트릭에 대한 값이 포함되어 있습니다. 정상 범위에 있는 메트릭은 응답에서 생략되므로 완전히 정상인 서버는 빈 객체를 갖습니다.

정보 객체에는 다음 필드가 포함될 수 있습니다:

| `Field`             | 값       | 설명                                                                                                                                                                                                           |
| ------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `amendment_blocked` | Boolean | (생략 가능) 참이면 서버가 수정 차단된 상태이며 네트워크와 동기화 상태를 유지하려면 업그레이드해야 하며 이 상태는 매우 중요합니다. 서버가 수정이 차단되지 않은 경우 이 필드는 생략됩니다.                                                                                                   |
| `load_factor`       | 숫자      | (생략 가능) 서버의 전체 부하를 측정하는 값입니다. 여기에는 I/O, CPU 및 메모리 제한이 반영됩니다. 부하 계수가 100을 초과하면 경고, 부하 계수가 1000 이상이면 위험입니다.                                                                                                    |
| `peers`             | 숫자      | (생략 가능) 이 서버가 연결된 피어 서버의 수입니다. 7개 이하의 피어에 연결되어 있으면 경고, 0개의 피어에 연결되어 있으면 심각입니다.                                                                                                                               |
| `server_state`      | 문자열     | _(생략가능)_ 현재 서버 상태입니다. 서버가 추적, 동기화 또는 연결 상태인 경우 경고입니다. 서버가 연결이 끊긴 상태인 경우 매우 중요합니다.                                                                                                                            |
| `validated_ledger`  | 숫자      | (생략 가능) 합의를 통해 원장이 마지막으로 유효성을 검사된 후 경과한 시간(초)입니다. 서버를 시작할 때 초기 동기화 기간처럼 사용 가능한 유효성 검사된 원장이 없는 경우 이 값은 -1이며 경고로 간주됩니다. 마지막으로 유효성을 검사한 원장이 최소 7초 전인 경우에도 이 메트릭은 경고이며, 마지막으로 유효성을 검사한 원장이 최소 20초 전인 경우에는 위험합니다. |

## 참조

상태 검사 결과를 해석하는 데 대한 지침은 상태 검사 개입을 참조하세요.
