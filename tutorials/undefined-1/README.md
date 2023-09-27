# 시작하기

\
XRP Ledger는 항상 온라인 상태이며 완전히 공개되어 있습니다. 누구나 이 페이지에 있는 소스 코드와 같이 **웹 브라우저를 통해 직접 접근**할 수 있습니다.

아래 예제는 ledger 메소드를 사용하여 최신 ledger 버전과 그 ledger 버전에서 새로 검증된 트랜잭션 목록을 가져옵니다. 그대로 실행해보거나 코드를 변경하고 결과를 확인해보세요.

{% hint style="info" %}
Tip:

가능하다면, F12를 눌러 브라우저의 개발자 도구를 열어보세요. "Console" 탭은 네이티브 JavaScript 콘솔을 제공하며 어떤 코드가 웹페이지에서 실행되는지에 대한 통찰력을 제공할 수 있습니다.
{% endhint %}

```javascript
async function main() {
  const api = new xrpl.Client('wss://xrplcluster.com');
  await api.connect();
​
  let response = await api.request({
    "command": "ledger",
    "ledger_index": "validated",
    "transactions": true
  });
  console.log(response);
}
main();
```

## 제안사항&#x20;

위 코드를 다른 작업을 수행하도록 변경해보세요:

* Testnet 공개 서버인 wss://s.altnet.rippletest.net/에 연결해보세요.&#x20;
* ledger의 트랜잭션 중 하나의 세부사항을 tx 메소드를 사용하여 찾아보세요.&#x20;
* 응답에서 total\_coins를 10진수 XRP로 변환해보세요.&#x20;

## 설정 단계&#x20;

이 페이지에는 필요한 선행 조건이 이미 로드되어 있지만, 해당 페이지의 HTML에서 xrpl.js를 로드하면 어떤 웹페이지에서든 XRP Ledger에 접근할 수 있습니다. 예를 들어:

```html
<script src="https://unpkg.com/xrpl@2.0.0/build/xrpl-latest-min.js"></script>
```

## 추가 읽기&#x20;

준비가 되었다면, 다음 자원들을 사용하여 XRP Ledger를 계속 이용해보세요:

* XRP를 보내 첫 트랜잭션을 보내보세요.&#x20;
* XRP Ledger의 디자인에 대한 컨셉을 이해하세요.
* rippled를 설치하여 네트워크에 참여하세요.&#x20;
* Testnet XRP를 받아 보내고 받는 것을 시도해보세요.
