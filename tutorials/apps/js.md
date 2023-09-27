# JS에서 브라우저 지갑 구축

이 튜토리얼은 자바스크립트 프로그래밍 언어와 다양한 라이브러리를 사용하여 XRP Ledger를 위한 브라우저 월렛을 구축하는 방법을 보여줍니다. 이 애플리케이션은 더 완전하고 강력한 애플리케이션을 구축하기 위한 출발점으로 사용하거나 비슷한 앱을 구축하는 참고 자료로 사용하거나 XRP Ledger 기능을 더 큰 프로젝트에 통합하는 방법을 더 잘 이해하기 위한 학습 경험으로 활용할 수 있습니다.

## 요구 사항

이 튜토리얼을 완료하기 위해 다음 가이드라인을 충족해야 합니다:

1. Node.js v14 이상이 설치되어 있어야 합니다.
2. Yarn (v1.17.3 이상)이 설치되어 있어야 합니다.
3. JavaScript로 코딩에 어느 정도 익숙하고 Get Started Using JavaScript 튜토리얼을 완료한 경험이 있어야 합니다.

### 소스 코드

이 튜토리얼의 모든 예제의 완전한 소스 코드는 이 웹사이트의 저장소의 코드 샘플 섹션에서 찾을 수 있습니다.

### 목표

이 튜토리얼의 끝에서는 아래에 표시된 간단한 XRP 월렛을 구축할 수 있어야 합니다.

![Home Page Screenshot](https://xrpl.org/img/js-wallet-home.png)

이 애플리케이션은 다음을 수행할 수 있습니다:

* XRP Ledger의 업데이트를 실시간으로 표시합니다.
* 모든 XRP Ledger 계정의 활동을 볼 수 있으며, 각 거래에 의해 전달된 XRP의 양을 표시합니다.
* 계정의 reserve requirements으로 설정된 XRP 양을 표시합니다.
* 직접 XRP 결제를 보낼 수 있으며, 목적지 주소의 의도를 확인하는 데 대한 피드백을 제공합니다. 이에는 다음이 포함됩니다:
  * 계정의 사용 가능한 잔액 표시.
  * 목적지 주소가 유효한지 확인.
  * 보낼 충분한 XRP 잔액이 있는지 확인.
  * 목적지 태그를 지정할 수 있도록 허용.

### 단계

시작하기 전에 요구 사항이 설치되어 있는지 확인하십시오. node -v 명령을 실행하여 노드 버전을 확인할 수 있습니다. 필요한 경우 Node.js를 다운로드하세요.

{% hint style="info" %}
**Tip:**

이 튜토리얼이나 다른 프로젝트 작업 중에 도움이 필요한 경우, XRPL의 개발자 Discord에서 도움을 요청하십시오.
{% endhint %}

### 1. 프로젝트 설정 <a href="#1-set-up-the-project" id="1-set-up-the-project"></a>

1. 프로젝트를 생성할 디렉토리로 이동하세요.
2. 다음 명령어를 사용하여 새로운 Vite 프로젝트를 생성하세요:

```
yarn create vite simple-xrpl-wallet --template vanilla
```

3. package.json 파일을 생성하거나 수정하여 다음 내용을 추가하세요:

```json
{
    "name": "simple-xrpl-wallet",
    "type": "module",
    "scripts": {
        "dev": "vite"
    },
    "devDependencies": {
        "@esbuild-plugins/node-globals-polyfill": "^0.2.3",
        "crypto-browserify": "^3.12.0",
        "events": "^3.3.0",
        "https-browserify": "^1.0.0",
        "rollup-plugin-polyfill-node": "^0.12.0",
        "stream-browserify": "^3.0.0",
        "stream-http": "^3.2.0",
        "vite": "^4.1.5"
    },
    "dependencies": {
        "dotenv": "^16.0.3",
        "xrpl": "^2.7.0-beta.2"
    }
}
```

* 또는 개별 패키지마다 `yarn add <package-name>` 을 사용하여 package.json 파일에 추가할 수도 있습니다.

4. Dependencies를 설치하세요.

```
yarn
```

5. 프로젝트의 루트 디렉토리에 .env라는 새 파일을 생성하고 다음 변수를 추가하세요:

```
CLIENT="wss://s.altnet.rippletest.net:51233/"
EXPLORER_NETWORK="testnet"
SEED="s████████████████████████████"
```

6. Seed를 자체 Seed로 변경하세요. Testnet faucet에서 자격 증명을 얻을 수 있습니다.
7. Vite 번들러를 설정하세요. 프로젝트의 루트 디렉토리에 vite.config.js라는 파일을 생성하고 다음 코드로 채워넣으세요:

```javascript
import { defineConfig, loadEnv } from 'vite';

import { NodeGlobalsPolyfillPlugin } from '@esbuild-plugins/node-globals-polyfill';
import polyfillNode from 'rollup-plugin-polyfill-node';

const viteConfig = ({ mode }) => {
    process.env = { ...process.env, ...loadEnv(mode, '', '') };
    return defineConfig({
        define: {
            'process.env': process.env,
        },
        optimizeDeps: {
            esbuildOptions: {
                define: {
                    global: 'globalThis',
                },
                plugins: [
                    NodeGlobalsPolyfillPlugin({
                        process: true,
                        buffer: true,
                    }),
                ],
            },
        },
        build: {
            rollupOptions: {
                plugins: [polyfillNode()],
            },
        },
        resolve: {
            alias: {
                events: 'events',
                crypto: 'crypto-browserify',
                stream: 'stream-browserify',
                http: 'stream-http',
                https: 'https-browserify',
                ws: 'xrpl/dist/npm/client/WSWrapper',
            },
        },
    });
};

export default viteConfig;
```

이 예제에는 xrpl.js가 Vite와 함께 작동하도록하는 필요한 구성이 포함되어 있습니다.

8. package.json에 스크립트 추가

package.json 파일에 아래와 같은 섹션을 추가하세요:

```json
"scripts": {
    "dev": "vite"
}
```

### 2. 홈 페이지 만들기 (계정 및 레저 세부 정보 표시) <a href="#2-create-the-home-page-displaying-account-ledger-details" id="2-create-the-home-page-displaying-account-ledger-details"></a>

이 단계에서는 계정 및 레저 세부 정보를 표시하는 홈 페이지를 만듭니다.

![Home Page Screenshot](https://xrpl.org/img/js-wallet-home.png)

1. 이미 존재하지 않는 경우 루트 폴더에 index.html, index.js 및 index.css라는 새 파일을 만드세요.
2. 루트 디렉토리에 src라는 새 폴더를 만드세요.
3. index.html의 내용을 코드로 복사하세요.
4. index.css 파일에 스타일을 추가하세요. 링크에서 스타일을 가져옵니다.

이 기본 설정은 홈 페이지를 만들고 시각적 스타일을 적용합니다. 홈 페이지의 목표는 다음과 같습니다:

* 계정 정보 표시.
* ledger에서 발생하는 사항 표시.
* 재미를 위해 작은 로고 추가.

이를 위해 XRP Ledger에 연결하고 계정 및 최신 유효한 ledger를 조회해야 합니다.

5. src/ 디렉토리에 helpers라는 새 폴더를 만드세요. 그리고 그 안에 get-wallet-details.js라는 새 파일을 생성하고 getWalletDetails라는 함수를 정의하세요. 이 함수는 account\_info 메소드를 사용하여 계정 세부 정보를 가져오고 server\_info 메소드를 사용하여 현재 reserve requirements를 계산합니다. 이 모든 작업을 수행하기 위한 코드는 다음과 같습니다:

```javascript
import { Client, Wallet, classicAddressToXAddress } from 'xrpl';

export default async function getWalletDetails({ client }) {
    try {
        const wallet = Wallet.fromSeed(process.env.SEED); // Convert the seed to a wallet : https://xrpl.org/cryptographic-keys.html

        // Get the wallet details: https://xrpl.org/account_info.html
        const {
            result: { account_data },
        } = await client.request({
            command: 'account_info',
            account: wallet.address,
            ledger_index: 'validated',
        });

        const ownerCount = account_data.OwnerCount || 0;

        // Get the reserve base and increment
        const {
            result: {
                info: {
                    validated_ledger: { reserve_base_xrp, reserve_inc_xrp },
                },
            },
        } = await client.request({
            command: 'server_info',
        });

        // Calculate the reserves by multiplying the owner count by the increment and adding the base reserve to it.
        const accountReserves = ownerCount * reserve_inc_xrp + reserve_base_xrp;

        console.log('Got wallet details!');

        return { 
            account_data, 
            accountReserves, 
            xAddress: classicAddressToXAddress(wallet.address, false, false), // Learn more: https://xrpaddress.info/
            address: wallet.address 
        };
    } catch (error) {
        console.log('Error getting wallet details', error);
        return error;
    }
}
```

6. 이제 index.js 파일에 계정 및 레저 세부 정보를 가져와 홈 페이지에 표시하는 코드를 추가하세요. 여기서는 get-wallet-details.js에서 정의한 함수를 사용하여 월렛 세부 정보를 렌더링합니다. 최신 유효한 ledger 데이터를 가져오기 위해 ledger 스트림을 수신하는 데 사용합니다.

```javascript
import { Client, dropsToXrp, rippleTimeToISOTime } from 'xrpl';

import addXrplLogo from './src/helpers/render-xrpl-logo';
import getWalletDetails from './src/helpers/get-wallet-details.js';

// Optional: Render the XRPL logo
addXrplLogo();

const client = new Client(process.env.CLIENT); // Get the client from the environment variables

// Get the elements from the DOM
const sendXrpButton = document.querySelector('#send_xrp_button');
const txHistoryButton = document.querySelector('#transaction_history_button');
const walletElement = document.querySelector('#wallet');
const walletLoadingDiv = document.querySelector('#loading_wallet_details');
const ledgerLoadingDiv = document.querySelector('#loading_ledger_details');

// Add event listeners to the buttons
sendXrpButton.addEventListener('click', () => {
    window.location.pathname = '/src/send-xrp/send-xrp.html';
});

txHistoryButton.addEventListener('click', () => {
    window.location.pathname = '/src/transaction-history/transaction-history.html';
});

// Self-invoking function to connect to the client
(async () => {
    try {
        await client.connect(); // Connect to the client

        // Subscribe to the ledger stream
        await client.request({
            command: 'subscribe',
            streams: ['ledger'],
        });

        // Fetch the wallet details
        getWalletDetails({ client })
            .then(({ account_data, accountReserves, xAddress, address }) => {
                walletElement.querySelector('.wallet_address').textContent = `Wallet Address: ${account_data.Account}`;
                walletElement.querySelector('.wallet_balance').textContent = `Wallet Balance: ${dropsToXrp(account_data.Balance)} XRP`;
                walletElement.querySelector('.wallet_reserve').textContent = `Wallet Reserve: ${accountReserves} XRP`;
                walletElement.querySelector('.wallet_xaddress').textContent = `X-Address: ${xAddress}`;

                // Redirect on View More link click
                walletElement.querySelector('#view_more_button').addEventListener('click', () => {
                    window.open(`https://${process.env.EXPLORER_NETWORK}.xrpl.org/accounts/${address}`, '_blank');
                });
            })
            .finally(() => {
                walletLoadingDiv.style.display = 'none';
            });


        // Fetch the latest ledger details
        client.on('ledgerClosed', (ledger) => {
            ledgerLoadingDiv.style.display = 'none';
            const ledgerIndex = document.querySelector('#ledger_index');
            const ledgerHash = document.querySelector('#ledger_hash');
            const closeTime = document.querySelector('#close_time');
            ledgerIndex.textContent = `Ledger Index: ${ledger.ledger_index}`;
            ledgerHash.textContent = `Ledger Hash: ${ledger.ledger_hash}`;
            closeTime.textContent = `Close Time: ${rippleTimeToISOTime(ledger.ledger_time)}`;
        });

    } catch (error) {
        await client.disconnect();
        console.log(error);
    }
})();
```

7. helpers 폴더에 render-xrpl-logo.js를 추가하여 로고를 표시하는 기능을 처리합니다.
8. 마지막으로 src/ 디렉토리에 assets라는 새 폴더를 만들고 거기에 xrpl.svg 파일을 추가하세요.

이 파일은 미적인 목적을 위해 XRPL 로고를 렌더링하는 데 사용됩니다.

이곳에서 해야 할 다른 작업은 XRP를 보내는 버튼과 트랜잭션 기록을 보는 버튼에 이벤트를 추가하는 것입니다. 아직 작동하지 않을 것이므로 다음 단계에서 구현할 것입니다.&#x20;

이제 애플리케이션을 실행할 준비가 되었습니다. 다음 명령을 사용하여 개발 모드에서 시작할 수 있습니다.

```
yarn dev
```

터미널에는 코드를 변경하면 반영되는 브라우저에서 앱을 열 수 있는 URL이 출력됩니다.

### 3. XRP 보내기 페이지 생성 <a href="#3-create-the-send-xrp-page" id="3-create-the-send-xrp-page"></a>

이제 홈 페이지를 만들었으므로 "XRP 보내기" 페이지로 넘어갈 수 있습니다. 이것이 이 지갑이 계정 자금을 관리할 수 있게 해주는 기능입니다.

![Send XRP Page Screenshot](https://xrpl.org/img/js-wallet-send-xrp.png)

1. src 폴더에 send-xrp라는 이름의 폴더를 생성합니다.&#x20;
2. send-xrp 폴더 안에 send-xrp.js와 send-xrp.html이라는 두 개의 파일을 생성합니다. send-xrp.html 파일의 내용을 send-xrp.html 파일로 복사합니다. 제공된 HTML 코드에는 대상 주소, 금액 및 대상 태그에 대한 세 가지 입력 필드와 해당 레이블이 포함되어 있습니다.&#x20;
3. 이제 HTML 코드가 있으므로 JavaScript 코드를 추가해 보겠습니다. helpers 폴더에서 submit-transaction.js라는 새 파일을 만들고 아래에 작성된 코드를 파일에 복사합니다.&#x20;
4. 이 파일에서는 XRPL에 트랜잭션을 제출하기 위해 submit 메소드를 사용합니다. 모든 트랜잭션은 먼저 지갑에 의해 서명되어야 합니다. 트랜잭션에 대한 서명에 대해서는 자세히 알아보세요.

```javascript
import { Wallet } from 'xrpl';

export default async function submitTransaction({ client, tx }) {
    try {
        // Create a wallet using the seed
        const wallet = await Wallet.fromSeed(process.env.SEED);
        tx.Account = wallet.address;

        // Sign and submit the transaction : https://xrpl.org/send-xrp.html#send-xrp
        const response = await client.submit(tx, { wallet });
        console.log(response);

        return response;
    } catch (error) {
        console.log(error);
        return null;
    }
}
```

5. 이제 send-xrp.js 파일로 돌아가 아래에 작성된 코드를 파일에 복사합니다. 이 코드 조각에서는 먼저 HTML에서 모든 DOM 요소를 가져와 사용자 입력에 따라 필드를 업데이트하고 유효성을 검사하는 이벤트 리스너를 추가합니다. renderAvailableBalance 메소드를 사용하여 현재 사용 가능한 잔액을 지갑에 표시합니다. validateAddress 함수는 사용자 주소의 유효성을 검사하고 정규식을 사용하여 금액의 유효성을 검증합니다. 모든 필드에 올바른 입력이 채워지면 submitTransaction 함수를 호출하여 트랜잭션을 ledger에 제출합니다.

```javascript
import { Client, Wallet, dropsToXrp, isValidClassicAddress, xrpToDrops } from 'xrpl';

import getWalletDetails from '../helpers/get-wallet-details';
import renderXrplLogo from '../helpers/render-xrpl-logo';
import submitTransaction from '../helpers/submit-transaction';

// Optional: Render the XRPL logo
renderXrplLogo();

const client = new Client(process.env.CLIENT); // Get the client from the environment variables

// Self-invoking function to connect to the client
(async () => {
    try {
        await client.connect(); // Connect to the client   

        const wallet = Wallet.fromSeed(process.env.SEED); // Convert the seed to a wallet : https://xrpl.org/cryptographic-keys.html

        // Subscribe to account transaction stream
        await client.request({
            command: 'subscribe',
            accounts: [wallet.address],
        });

        // Fetch the wallet details and show the available balance
        await getWalletDetails({ client }).then(({ accountReserves, account_data }) => {
            availableBalanceElement.textContent = `Available Balance: ${dropsToXrp(account_data.Balance) - accountReserves} XRP`;
        });

    } catch (error) {
        await client.disconnect();
        console.log(error);
    }
})();

// Get the elements from the DOM
const homeButton = document.querySelector('#home_button');
const txHistoryButton = document.querySelector('#transaction_history_button');
const destinationAddress = document.querySelector('#destination_address');
const amount = document.querySelector('#amount');
const destinationTag = document.querySelector('#destination_tag');
const submitTxBtn = document.querySelector('#submit_tx_button');
const availableBalanceElement = document.querySelector('#available_balance');

// Disable the submit button by default
submitTxBtn.disabled = true;
let isValidDestinationAddress = false;
const allInputs = document.querySelectorAll('#destination_address, #amount');

// Add event listener to the redirect buttons
homeButton.addEventListener('click', () => {
    window.location.pathname = '/index.html';
});

txHistoryButton.addEventListener('click', () => {
    window.location.pathname = '/src/transaction-history/transaction-history.html';
});

// Update the account balance on successful transaction
client.on('transaction', (response) => {
    if (response.validated && response.transaction.TransactionType === 'Payment') {
        getWalletDetails({ client }).then(({ accountReserves, account_data }) => {
            availableBalanceElement.textContent = `Available Balance: ${dropsToXrp(account_data.Balance) - accountReserves} XRP`;
        });
    }
});

const validateAddress = () => {
    destinationAddress.value = destinationAddress.value.trim();
    // Check if the address is valid
    if (isValidClassicAddress(destinationAddress.value)) {
        // Remove the invalid class if the address is valid
        destinationAddress.classList.remove('invalid');
        isValidDestinationAddress = true;
    } else {
        // Add the invalid class if the address is invalid
        isValidDestinationAddress = false;
        destinationAddress.classList.add('invalid');
    }
};

// Add event listener to the destination address
destinationAddress.addEventListener('input', validateAddress);

// Add event listener to the amount input
amount.addEventListener('keydown', (event) => {
    const codes = [8, 190];
    const regex = /^[0-9\b.]+$/;

    // Allow: backspace, delete, tab, escape, enter and .
    if (!(regex.test(event.key) || codes.includes(event.keyCode))) {
        event.preventDefault();
        return false;
    }

    return true;
});

// NOTE: Keep this code at the bottom of the other input event listeners
// All the inputs should have a value to enable the submit button
for (let i = 0; i < allInputs.length; i++) {
    allInputs[i].addEventListener('input', () => {
        let values = [];
        allInputs.forEach((v) => values.push(v.value));
        submitTxBtn.disabled = !isValidDestinationAddress || values.includes('');
    });
}

// Add event listener to the submit button
submitTxBtn.addEventListener('click', async () => {
    try {
        console.log('Submitting transaction');
        submitTxBtn.disabled = true;
        submitTxBtn.textContent = 'Submitting...';

        // Create the transaction object: https://xrpl.org/transaction-common-fields.html
        const txJson = {
            TransactionType: 'Payment',
            Amount: xrpToDrops(amount.value), // Convert XRP to drops: https://xrpl.org/basic-data-types.html#specifying-currency-amounts
            Destination: destinationAddress.value,
        };

        // Get the destination tag if it exists
        if (destinationTag?.value !== '') {
            txJson.DestinationTag = destinationTag.value;
        }

        // Submit the transaction to the ledger
        const { result } = await submitTransaction({ client, tx: txJson });
        const txResult = result?.meta?.TransactionResult || result?.engine_result || ''; // Response format: https://xrpl.org/transaction-results.html

        // Check if the transaction was successful or not and show the appropriate message to the user
        if (txResult === 'tesSUCCESS') {
            alert('Transaction submitted successfully!');
        } else {
            throw new Error(txResult);
        }
    } catch (error) {
        alert('Error submitting transaction, Please try again.');
        console.error(error);
    } finally {
        // Re-enable the submit button after the transaction is submitted so the user can submit another transaction
        submitTxBtn.disabled = false;
        submitTxBtn.textContent = 'Submit Transaction';
    }
});
```

이제 'XRP 보내기'를 클릭하여 나만의 트랜잭션을 만들어 볼 수 있습니다! 이 예제를 사용하여 XRP를 testnet의 faucet에 보내보세요.

testnet faucet 계정: `rHbZCHJSGLWVMt8D6AsidnbuULHffBFvEN`

Amount: 9

데스티네이션 태그: (일반적으로 거래소에 연결된 계정을 지불하는 경우에만 필요합니다.)

![Send XRP Transaction Screenshot](https://xrpl.org/img/js-wallet-send-xrp-transaction-details.png)

### 4. 트랜잭션 페이지 생성 <a href="#4-create-the-transactions-page" id="4-create-the-transactions-page"></a>

이제 홈 페이지와 XRP 보내기 페이지를 만들었으므로 계정의 트랜잭션 기록을 표시할 트랜잭션 페이지를 만들어 보겠습니다. Ledger에서 발생하는 일을 확인하기 위해 다음과 같은 필드를 표시할 것입니다:

* 계정: 트랜잭션을 보낸 계정입니다.&#x20;
* 대상: 트랜잭션을 받은 계정입니다.&#x20;
* 금액: 트랜잭션에서 보낸 XRP의 금액입니다.&#x20;
* 트랜잭션 유형: 트랜잭션의 유형입니다.&#x20;
* 결과: 트랜잭션의 결과입니다.&#x20;
* 링크: XRP Ledger Explorer에서 트랜잭션으로 이동하는 링크입니다.

![Transactions Page Screenshot](https://xrpl.org/img/js-wallet-transaction.png)

1. src 디렉토리에 transaction-history라는 이름의 폴더를 생성합니다.
2. transaction-history.js라는 파일을 생성하고 아래에 작성된 코드를 복사합니다.

```javascript
import { Client, Wallet, convertHexToString, dropsToXrp } from 'xrpl';

import renderXrplLogo from '../helpers/render-xrpl-logo';

// Optional: Render the XRPL logo
renderXrplLogo();

// Declare the variables
let marker = null;

// Get the elements from the DOM
const txHistoryElement = document.querySelector('#tx_history_data');
const sendXrpButton = document.querySelector('#send_xrp_button');
const homeButton = document.querySelector('#home_button');
const loadMore = document.querySelector('#load_more_button');

// Add event listeners to the buttons
sendXrpButton.addEventListener('click', () => {
    window.location.pathname = '/src/send-xrp/send-xrp.html';
});

homeButton.addEventListener('click', () => {
    window.location.pathname = '/index.html';
});

// Add the header to the table
const header = document.createElement('tr');
header.innerHTML = `
    <th>Account</th>
    <th>Destination</th>
    <th>Fee (XRP)</th>
    <th>Amount</th>
    <th>Transaction Type</th>
    <th>Result</th>
    <th>Link</th>
`;
txHistoryElement.appendChild(header);

// Converts the hex value to a string
const getTokenName = (value) => (value.length === 40 ? convertHexToString(value).replaceAll('\u0000', '') : value);

function renderTokenValueColumn(value) {
    return value.Amount
        ? `<td>${
              typeof value.Amount === 'object' ? `${value.Amount.value} ${getTokenName(value.Amount.currency)}` : `${dropsToXrp(value.Amount)} XRP`
          }</td>`
        : '-';
}

// Fetches the transaction history from the ledger
async function fetchTxHistory() {
    try {
        loadMore.textContent = 'Loading...';
        loadMore.disabled = true;
        const wallet = Wallet.fromSeed(process.env.SEED);
        const client = new Client(process.env.CLIENT);

        // Wait for the client to connect
        await client.connect();

        // Get the transaction history
        const payload = {
            command: 'account_tx',
            account: wallet.address,
            limit: 10,
        };

        if (marker) {
            payload.marker = marker;
        }

        // Wait for the response: use the client.request() method to send the payload
        const { result } = await client.request(payload);

        const { transactions, marker: nextMarker } = result;

        // Add the transactions to the table
        const values = transactions.map((transaction) => {
            const { meta, tx } = transaction;
            return {
                Account: tx.Account,
                Destination: tx.Destination,
                Fee: tx.Fee,
                Amount: tx.Amount,
                Hash: tx.hash,
                TransactionType: tx.TransactionType,
                result: meta?.TransactionResult,
            };
        });

        // If there are no more transactions, hide the load more button
        loadMore.style.display = nextMarker ? 'block' : 'none';

        // If there are no transactions, show a message
        // Create a new row: https://developer.mozilla.org/en-US/docs/Web/API/Document/createElement
        // Add the row to the table: https://developer.mozilla.org/en-US/docs/Web/API/Node/appendChild

        if (values.length === 0) {
            const row = document.createElement('tr');
            row.innerHTML = `<td colspan="6">No transactions found</td>`;
            txHistoryElement.appendChild(row);
        } else {
            // Otherwise, show the transactions by iterating over each transaction and adding it to the table
            values.forEach((value) => {
                const row = document.createElement('tr');
                // Add the transaction details to the row
                row.innerHTML = `
                ${value.Account ? `<td>${value.Account}</td>` : '-'}
                ${value.Destination ? `<td>${value.Destination}</td>` : '-'}
                ${value.Fee ? `<td>${dropsToXrp(value.Fee)}</td>` : '-'}
                ${renderTokenValueColumn(value)}
                ${value.TransactionType ? `<td>${value.TransactionType}</td>` : '-'}
                ${value.result ? `<td>${value.result}</td>` : '-'}
                ${value.Hash ? `<td><a href="https://${process.env.EXPLORER_NETWORK}.xrpl.org/transactions/${value.Hash}" target="_blank">View</a></td>` : '-'}`;
                // Add the row to the table
                txHistoryElement.appendChild(row);
            });
        }

        // Disconnect
        await client.disconnect();

        // Enable the load more button only if there are more transactions
        loadMore.textContent = 'Load More';
        loadMore.disabled = false;

        // Return the marker
        return nextMarker ?? null;
    } catch (error) {
        console.log(error);
        return null;
    }
}

// Render the transaction history
async function renderTxHistory() {
    // Fetch the transaction history
    marker = await fetchTxHistory();
    loadMore.addEventListener('click', async () => {
        const nextMarker = await fetchTxHistory();
        marker = nextMarker;
    });
}

// Call the renderTxHistory() function
renderTxHistory();
```

이 코드는 account\_tx를 사용하여 계정에서 보내고 받은 트랜잭션을 가져옵니다. 불완전한 트랜잭션 목록을 페이징하기 위해 marker 매개변수를 사용하여 모든 결과를 가져올 때까지 페이지별로 순회합니다.

3. transaction-history.html이라는 파일을 생성하고 transaction-history.html에서 제공된 코드를 복사하여 붙여넣습니다.

transaction-history.html은 위에서 언급한 필드를 표시하는 테이블을 정의합니다.

이 코드를 사용하여 계정의 트랜잭션 기록을 표시하는 출발점으로 사용할 수 있습니다. 추가적인 도전 과제로 trustset와 같은 다양한 트랜잭션 유형을 지원하도록 확장해 볼 수 있습니다. 이를 처리하는 방법에 대한 영감을 얻으려면 XRP Ledger Explorer를 확인하여 트랜잭션 세부 정보가 어떻게 표시되는지 확인할 수 있습니다.

### 다음 단계 <a href="#next-steps" id="next-steps"></a>

이제 기능적인 지갑이 생겼으므로 다양한 방향으로 발전시킬 수 있습니다. 몇 가지 아이디어를 소개합니다.

* 토큰 및 크로스 화폐 지불을 포함한 XRP Ledger의 더 많은 트랜잭션 유형을 지원할 수 있습니다.
* XRP 이외의 여러 토큰을 표시할 수 있도록 확장할 수 있습니다.
* 탈중앙화 거래소에서 거래 생성을 지원할 수 있습니다.
* QR 코드나 지갑에서 열리는 URI와 같은 새로운 결제 요청 방법을 지원할 수 있습니다.
* 정규 키 쌍을 설정하거나 다중 서명을 처리하여 계정 보안을 강화할 수 있습니다.
* 또는 Vite를 사용한 프로덕션 빌드에 따라 코드를 실제 환경으로 가져갈 수 있습니다.
