# 신뢰할 수 있는 트랜잭션 제출(Reliable Transaction Submission)

XRP Ledger를 사용하는 금융 기관 및 기타 서비스는 여기에 설명된 모범 사례를 사용하여 검증 가능하고 신속한 방법으로 거래가 검증 또는 거부되는지 확인해야 합니다. 신뢰할 수 있는 rippled 서버에 트랜잭션을 제출해야 합니다.

이 문서에 자세히 설명된 모범 사례를 통해 애플리케이션은 다음을 달성하는 동시에 XRP Ledger에 트랜잭션을 제출할 수 있습니다:

1. Idempency - 트랜잭션은 한 번만 처리하거나 한 번만 처리해야 합니다.&#x20;
2. Verifiability - 응용 프로그램이 트랜잭션의 최종 결과를 결정할 수 있습니다.&#x20;

모범 사례를 구현하지 못하는 애플리케이션은 다음과 같은 오류의 위험에 처해 있습니다:

1. 실수로 실행되지 않은 트랜잭션을 제출합니다.&#x20;
2. 임시 트랜잭션 결과를 변경할 수 없는 최종 결과로 착각합니다.&#x20;
3. 이전에 ledger에 적용된 거래에 대한 신뢰할 수 있는 결과를 찾지 못했습니다.&#x20;

이러한 유형의 오류는 잠재적으로 심각한 문제로 이어질 수 있습니다. 예를 들어, 이전에 성공한 결제 트랜잭션을 찾지 못한 응용 프로그램이 다른 거래를 잘못 제출하여 원래 결제가 중복될 수 있습니다. 이는 응용프로그램이 이 문서에 설명된 기술을 사용하여 신뢰할 수 있는 트랜잭션 결과를 기반으로 작업을 수행하는 것의 중요성을 강조합니다.

## 배경&#x20;

XRP Ledger 프로토콜은 네트워크의 모든 서버에서 공유되는 ledger을 제공합니다. 컨센서스와 검증의 과정을 통해, 네트워크는 거래가 ledger에 적용되는 순서에 동의합니다.

신뢰할 수 있는 XRP Ledger 서버에 제출된 올바른 형식의 트랜잭션은 대개 몇 초 만에 검증되거나 거부됩니다. 그러나 제대로 구성된 트랜잭션이 검증되지 않거나 빠르게 거부되는 경우가 있습니다. 애플리케이션이 트랜잭션을 전송한 후 글로벌 트랜잭션 비용이 증가하는 경우 하나의 특정 사례가 발생할 수 있습니다. 거래 비용이 거래에 명시된 것 이상으로 증가하는 경우, 그 거래는 유효성이 확인된 다음 장부에 포함되지 않습니다. 만약 나중에 글로벌 거래 비용이 감소한다면, 그 거래는 나중의 장부에 포함될 수 있습니다. 트랜잭션에 만료가 지정되지 않은 경우에는 만료가 발생할 수 있는 기간에 제한이 없습니다.

정전이나 네트워크 중단이 발생하면 애플리케이션은 제출된 트랜잭션의 상태를 찾는 데 더 많은 어려움을 겪게 됩니다. 응용프로그램은 트랜잭션을 올바르게 제출하고 나중에 신뢰할 수 있는 결과를 올바르게 얻기 위해 주의해야 합니다.

## 트랜잭션 타임라인(Transaction Timeline)

트랜잭션을 XRP Ledger에 제출할 때 HTTP API, 클라이언트 라이브러리 또는 다른 앱을 사용했는지 여부에 관계없이 트랜잭션을 Ledger에 적용하는 프로세스는 동일합니다. 이 프로세스는 다음과 같습니다:

1. 계정 소유자는 트랜잭션을 만들고 서명합니다.&#x20;
2. 소유자는 해당 트랜잭션을 후보 트랜잭션으로 네트워크에 제출합니다.&#x20;
   1. 잘못된 형식의 트랜잭션이나 무의미한 트랜잭션은 즉시 거부됩니다.&#x20;
   2. 올바른 형식의 트랜잭션은 일시적으로 성공했다가 나중에 실패할 수 있습니다.&#x20;
   3. 올바른 형식의 트랜잭션은 일시적으로 실패했다가 나중에 성공할 수 있습니다.&#x20;
   4. 잘 형성된 거래는 잠정적으로 성공했다가 나중에 약간 다른 방식으로 성공할 수 있습니다. (예를 들어, 통화를 거래할 때 환율이 달라질 수 있습니다.)&#x20;
3. 컨센서스와 검증을 통해 거래는 ledger에 적용됩니다. 네트워크를 통해 전파되는 비용을 강제하기 위해 일부 실패한 트랜잭션도 적용됩니다.&#x20;
4. 검증된 ledger는 거래를 포함하며, 그 효과는 ledger 상태에 반영됩니다.&#x20;
   1. 트랜잭션 결과는 더 이상 임시적이지 않으며, 성공 또는 실패는 이제 최종적이고 불변합니다.&#x20;

{% hint style="info" %}
Note:

제출 방법에서 성공적으로 반환된 상태 코드는 서버가 후보 트랜잭션을 수신했음을 나타냅니다. 트랜잭션은 유효성이 확인된 ledger에 적용될 수도 있고 적용되지 않을 수도 있습니다.
{% endhint %}

API는 현재 진행 중인 ledger에 후보 거래를 적용한 결과를 토대로 잠정 결과를 반환할 수 있습니다. 응용 프로그램은 이러한 결과를 트랜잭션의 최종적인 불변 결과와 혼동해서는 안 됩니다. 변경할 수 없는 결과는 검증된 레지스터에서만 발견됩니다. 응용 프로그램은 트랜잭션 결과가 포함된 ledger이 유효성을 검사할 때까지 트랜잭션 상태를 반복적으로 쿼리해야 할 수 있습니다.

트랜잭션을 적용하는 동안, rippled 서버는 전체 네트워크가 검증한 트랜잭션을 기반으로 하는 마지막 유효성 검사 ledger, 즉 ledger 상태의 스냅샷을 사용합니다. 컨센서스 및 유효성 검사 프로세스는 일련의 새로운 트랜잭션을 공식적인 순서로 마지막 유효성 검사 대장에 적용하여 새로운 유효성 검사 ledger를 생성합니다. 이 새로운 검증된 ledger 버전과 그 이전의 ledger 버전은 ledger기록을 형성합니다.

유효성이 확인된 각 ledger 버전에는 이전 ledger 버전의 ledger 인덱스보다 1 큰 ledger 인덱스가 있습니다. 또한 각 ledger에는 내용에서 고유하게 결정되는 식별 해시 값이 있습니다. 진행 중인 여러 버전의 레지스터가 있을 수 있으며, 이들은 동일한 ledger 인덱스를 가지고 있지만 해시 값은 다릅니다. 하나의 버전만 검증할 수 있습니다.

검증된 각 ledger에는 트랜잭션이 적용되는 표준 순서가 있습니다. 이 순서는 ledger의 최종 거래 집합에 기초하여 결정됩니다. 반대로, rippled 형태의 각 서버는 진행 중인 ledger은 트랜잭션이 수신될 때마다 증분 계산됩니다. 트랜잭션이 임시로 실행되는 순서는 일반적으로 새 유효성 검사된 ledger을 작성하기 위해 트랜잭션이 실행되는 순서와 다릅니다. 이것이 거래의 잠정적 결과가 최종 결과와 다를 수 있는 한 가지 이유입니다. 예를 들어, 지급이 동일한 제안을 소비하는 다른 지급 전 또는 후에 실행되는지에 따라 다른 최종 환율을 달성할 수 있습니다.

## LastLedgerSequence&#x20;

LastLedgerSequence는 모든 거래의 선택적 파라미터입니다. 이것은 특정 ledger 버전에서 거래가 유효화되어야 함을 XRP Ledger에 지시합니다. XRP Ledger는 거래의 LastLedgerSequence 파라미터보다 높은 ledger index를 가진 ledger 버전에는 절대로 거래를 포함시키지 않습니다.

LastLedgerSequence 파라미터를 사용하여 거래가 신속하게 확인되지 않지만 미래의 ledger에 포함될 수 있는 불필요한 경우를 방지합니다. 모든 거래에서 LastLedgerSequence 파라미터를 지정해야 합니다. 자동화된 과정은 마지막으로 유효화된 ledger index보다 4만큼 더 큰 값을 사용하여 거래가 예측 가능하고 신속하게 유효화되거나 거부되도록 해야 합니다.

HTTP / WebSocket API를 사용하는 응용 프로그램은 거래를 제출할 때 명시적으로 LastLedgerSequence를 지정해야 합니다. 일부 클라이언트 라이브러리는 LastLedgerSequence에 대해 합리적인 값을 자동으로 채울 수도 있습니다; 세부 사항은 라이브러리에 따라 다릅니다.

## 모범 사례&#x20;

다음 다이어그램은 거래를 제출하고 결과를 결정하는 데 권장되는 흐름을 요약합니다:

<figure><img src="https://xrpl.org/img/reliable-tx-submission.svg" alt=""><figcaption></figcaption></figure>

## 신뢰할 수 있는 거래 제출&#x20;

거래를 제출하는 응용 프로그램은 프로세스가 종료되거나 다른 실패가 발생하더라도 신뢰할 수 있게 제출하기 위해 다음과 같은 방법을 사용해야 합니다. 응용 프로그램의 거래 결과는 검증되어야 하며, 응용 프로그램은 최종적으로 유효화된 결과에 따라 행동할 수 있어야 합니다.

제출과 검증은 이 문서에서 설명된 로직을 사용하여 구현할 수 있는 두 가지 별도의 절차입니다.

1. 제출 - 거래가 네트워크에 제출되고 잠정적 결과가 반환됩니다.&#x20;
2. 검증 - 유효화된 ledger를 검토하여 권위 있는 결과가 결정됩니다.&#x20;

## 제출&#x20;

제출 완료 전에 전력 공급이나 네트워크 실패 등의 사유로 거래의 세부 사항을 제출하기 전에 유지해야 합니다. 재시작 시, 유지된 값들은 거래의 상태를 검증하는데 가능성을 제공합니다.

제출 과정:

1. 거래를 구성하고 서명.&#x20;
   1. LastLedgerSequence 파라미터를 포함.
2. 트랜잭션 세부 사항을 유지하고 저장:&#x20;
   1. 트랜잭션 해시.&#x20;
   2. LastLedgerSequence.&#x20;
   3. 발신자 주소와 sequence 번호.&#x20;
   4. 제출 시점의 최근 유효화된 ledger index.&#x20;
   5. 필요한 경우 응용 프로그램 특정 데이터.&#x20;
3. &#x20;트랜잭션 제출.&#x20;

## 검증&#x20;

정상 작동 중인 응용 프로그램은 해시에 의해 제출된 거래의 상태를 확인하거나, 사용된 API에 따라, 거래가 유효화 되었을 때 (또는 실패했을 때) 알림을 받을 수 있습니다. 이러한 정상 작동은 네트워크 장애나 전력 장애와 같은 사례로 인해 중단될 수 있습니다. 이러한 중단의 경우 응용 프로그램은 중단 전에 네트워크에 제출되었을 수도 있고 아닐 수도 있는 거래의 상태를 신뢰성 있게 검증할 필요가 있습니다.

재시작 시, 또는 새로운 마지막으로 유효화된 ledger가 결정되었을 때 (의사 코드):

```java
For each persisted transaction without validated result:
    Query transaction by hash
    If (result appears in any validated ledger)
        # Outcome is final
        Persist the final result
        If (result code is tesSUCCESS)
            Application may act based on successful transaction
        Else
            The transaction failed (1)
            If appropriate for the application and failure type, submit with
                new LastLedgerSequence and Fee

    Else if (LastLedgerSequence > newest validated ledger)
        # Outcome is not yet final
        Wait for more ledgers to be validated

    Else
        If (server has continuous ledger history from the ledger when the
              transaction was submitted up to and including the ledger
              identified by LastLedgerSequence)

            # Sanity check
            If (sender account sequence > transaction sequence)
                A different transaction with this sequence has a final outcome.
                Manual intervention suggested (3)
            Else
                The transaction failed (2)

        Else
            # Outcome is final, but not known due to a ledger gap
            Wait to acquire continuous ledger history
```

## 거래 실패 케이스(**Failure Cases)**&#x20;

프로그램 코드에서 (1)과 (2)로 표시된 두 거래 실패 사례의 차이는 거래가 유효화된 ledger에 포함되었는지의 여부입니다. 두 경우 모두 어떻게 실패를 처리할지 신중하게 결정해야 합니다.

* 실패 사례 (1)에서는 거래가 ledger에 포함되어 XRP 거래 비용이 소모되었지만 다른 일은 일어나지 않았습니다. 이는 유동성 부족, 잘못 지정된 경로 또는 다른 상황으로 인해 발생할 수 있습니다. 이러한 실패의 많은 경우, 유사한 거래를 즉시 재시도하면 같은 결과가 나올 가능성이 높습니다. 상황이 바뀌기를 기다리면 다른 결과가 나올 수 있습니다.
* 실패 사례 (2)에서는 거래가 유효화된 ledger에 포함되지 않아 전혀 아무런 일도 일어나지 않았습니다. 이는 XRP Ledger의 현재 부하에 비해 거래 비용이 너무 낮았거나, LastLedgerSequence가 너무 이른 경우, 또는 불안정한 네트워크 연결과 같은 다른 조건 때문일 수 있습니다.
  * 실패 사례 (1)와는 대조적으로, LastLedgerSequence와 가능하면 수수료만 변경하고 다시 제출하면 새 거래가 성공할 가능성이 더 높습니다. 원래 거래와 동일한 Sequence 번호를 사용해야 합니다.
  * 또한 거래가 ledger의 상태 때문에 성공하지 못했을 수도 있습니다. 예를 들어, 거래에 서명하는데 사용된 키 쌍을 보내는 주소가 비활성화되었을 수 있습니다. 거래의 임시 결과가 tef-class 코드였다면, 거래가 추가 수정 없이 성공할 가능성이 적습니다.
* 실패 사례 (3)는 예상치 못한 상태를 나타냅니다. 거래가 처리되지 않았을 때, 가장 최근의 유효화된 ledger에서 보내는 계정의 Sequence 번호를 확인해야 합니다. (이를 위해 account\_info 메소드를 사용할 수 있습니다.) 만약 최근 유효화된 ledger의 계정의 Sequence 값이 거래의 Sequence 값보다 높다면, 같은 Sequence 값으로 다른 거래가 유효화된 ledger에 포함되었습니다. 시스템이 다른 거래를 인지하지 못했다면 예상치 못한 상태에 있고, 그 이유를 파악할 때까지 처리를 중지해야 합니다. 그렇지 않으면 시스템은 같은 일을 하려고 여러 거래를 보낼 수 있습니다. 취해야 할 단계는 특정 원인에 따라 다릅니다. 가능성 있는 일부 원인은 다음과 같습니다:
  * 이전에 보낸 거래가 가변성을 가지고 있었고 실제로는 유효화된 ledger에 포함되었지만, 예상한 해시와는 다른 해시로 포함되었습니다. 이는 tfFullyCanonicalSig 플래그를 포함하지 않는 플래그 집합을 지정하거나 거래가 필요한 것보다 더 많은 사인을 가진 경우 발생할 수 있습니다. 이 경우, 다른 해시와 거래의 최종 결과를 저장하고, 일반적인 활동을 재개하세요.
  * 거래를 취소하고 대체하였고, 대체 거래가 대신 처리되었습니다. 가동 중단에서 복구 중이라면, 대체 거래의 기록을 잃어버렸을 수도 있습니다. 이 경우, 원래 찾고 있던 거래는 영구적으로 실패하였고, 대체 거래의 최종 결과는 유효화된 ledger 버전에 기록되어 있습니다. 두 개의 최종 결과를 모두 저장하고, 다른 누락된 또는 대체된 거래가 있는지 확인한 다음, 일반적인 활동을 재개하세요.
  * 활성/수동 장애 복구 설정에서 두 개 이상의 거래 전송 시스템을 가지고 있다면, 수동 시스템이 활성 시스템이 실패했다고 잘못 판단하고, 원래의 활성 시스템이 여전히 거래를 보내는 동안 활성화되었을 수 있습니다. 시스템 간의 연결을 확인하고, 그들 중 최대 하나만 활성 상태인지 확인하세요. 계정의 거래 기록을 확인하고 (예를 들어, account\_tx 메소드를 사용하여) 유효화된 ledger에 포함된 모든 거래의 최종 결과를 기록하세요. 같은 Sequence 번호를 가진 다른 거래는 모두 영구적으로 실패했습니다. 이러한 최종 결과도 모두 저장하세요. 모든 시스템의 차이점을 조정하고 시스템이 동시에 활성화된 문제를 해결했을 때, 일반적인 활동을 재개하세요.

{% hint style="info" %}
Tip:

이 상황에서 AccountTxnID 필드는 중복 거래가 성공하는 것을 방지하는데 도움이 될 수 있습니다.
{% endhint %}

* 악의적인 행위자가 비밀한 키를 사용하여 거래를 보냈을 수 있습니다. 이 경우, 가능하다면 키 쌍을 교체하고 보낸 다른 거래가 있는지 확인하세요. 또한, 비밀 키가 더 큰 침입이나 보안 유출의 일부였는지를 파악하기 위해 네트워크를 감사해야 합니다. 키 쌍을 성공적으로 교체하고 악의적인 행위자가 더 이상 계정과 시스템에 액세스할 수 없다는 것을 확실히 알게 되면, 일반적인 활동을 재개할 수 있습니다.

## Ledger 간격(**Ledger Gaps)**

당신의 서버가 거래가 원래 제출된 시점부터 LastLedgerSequence에 의해 식별된 ledger를 포함하여 연속적인 ledger 기록을 가지고 있지 않다면, 거래의 최종 결과를 알 수 없을 수 있습니다. (서버가 누락한 ledger 버전 중 하나에 포함되었다면, 그것이 성공했는지 실패했는지 알 수 없습니다.)

당신의 rippled 서버는 여유 자원 (CPU/RAM/디스크 IO)이 있을 때 자동으로 누락된 ledger 버전을 획득해야 합니다, 이는 ledger가 서버가 저장하기로 설정된 기록의 범위보다 오래되었을 때에는 예외입니다. 간격의 크기와 서버의 리소스 사용량에 따라, 누락된 ledger를 획득하는 데 몇 분이 소요될 수 있습니다. ledger\_request 메소드를 사용하여 서버에게 역사적인 ledger 버전을 획득하도록 요청할 수 있지만, 서버가 설정한 기록 범위를 벗어나는 ledger 버전의 거래 결과를 조회할 수 없을 수 있습니다.

대안적으로, 이미 필요한 ledger 기록을 가지고 있는 다른 rippled 서버, 예를 들면 Ripple의 전체 이력 서버인 s2.ripple.com에서 거래의 상태를 조회할 수 있습니다. 이 목적으로는 신뢰할 수 있는 서버만을 사용하십시오. 악의적인 서버는 거래의 상태와 결과에 대한 거짓 정보를 제공하도록 프로그래밍될 수 있습니다.

## 기술적 응용(Technical Application)

거래 제출과 검증의 모범 사례를 구현하려면, 응용 프로그램은 다음을 수행해야 합니다:

1. 서명 계정의 다음 순서 번호를 결정.&#x20;
   1. 각 거래는 계정별로 순서 번호를 가지고 있습니다. 이는 계정이 서명한 거래가 실행되는 순서를 보장하고, 거래가 ledger에 한 번 이상 적용되는 위험 없이 거래를 다시 제출할 수 있게 합니다.&#x20;
2. LastLedgerSequence 결정.&#x20;
   1. 트랜잭션의 LastRedgerSequence는 마지막 유효성 검사된 ledger 인덱스에서 계산됩니다.&#x20;
3. 트랜잭션 구성 및 서명&#x20;
   1. 제출하기 전에 서명된 트랜잭션의 세부 정보를 유지합니다.&#x20;
4. 트랜잭션 제출&#x20;
   1. 초기 결과는 잠정적이며 변경될 수 있습니다.&#x20;
5. 트랜잭션의 최종 결과 결정.&#x20;
   1. 최종 결과는 ledger 기록의 불변하는 부분입니다.

응용 프로그램이 이러한 작업을 수행하는 방법은 응용 프로그램이 사용하는 API에 따라 달라집니다. 응용 프로그램은 다음 인터페이스 중 하나를 사용할 수 있습니다:

* XRP Ledger 서버에서 직접 제공하는 HTTP/WebSocket API.&#x20;
* 클라이언트 라이브러리.
* 위의 API 위에 계층화된 다른 미들웨어 또는 API.

## rippled - 거래 제출 및 검증&#x20;

### 계정 순서 결정&#x20;

rippled는 account\_info 메소드를 제공하여 마지막으로 유효화된 ledger에서 계정의 순서 번호를 알 수 있습니다.

JSON-RPC 요청:

```json
{
  "method": "account_info",
  "params": [
    {
      "account": "rG5Ro9e3uGEZVCh3zu5gB9ydKUskCs221W",
      "ledger_index": "validated"
    }
  ]
}
```

body 응답:

```json
{
    "result": {
        "validated": true,
        "status": "success",
        "ledger_index": 10266396,
        "account_data": {
            "index": "96AB97A1BBC37F4F8A22CE28109E0D39D709689BDF412FE8EDAFB57A55E37F38",
            "Sequence": 4,
            "PreviousTxnLgrSeq": 9905632,
            "PreviousTxnID": "CAEE0E34B3DB50A7A0CA486E3A236513531DE9E52EAC47CE4C26332CC847DE26",
            "OwnerCount": 2,
            "LedgerEntryType": "AccountRoot",
            "Flags": 0,
            "Balance": "49975988",
            "Account": "rG5Ro9e3uGEZVCh3zu5gB9ydKUskCs221W"
        }
    }
}
```

이 예시에서, 마지막으로 유효화된 ledger 기준으로 계정의 순서는 4입니다 ("account\_data"에 있는 "Sequence": 4 참고). 유효화된 ledger는 요청에서 "ledger\_index": "validated"를 확인하고 응답에서 "validated": "true"를 확인할 수 있습니다.

이 계정에서 서명된 세 건의 거래를 제출하려는 어플리케이션의 경우, 순서 번호는 4, 5, 6을 사용할 것입니다. 각각의 유효화를 기다리지 않고 여러 거래를 제출하려면, 어플리케이션은 계속되는 계정 순서 번호를 유지해야 합니다.

### 마지막으로 유효화된 Ledger 결정&#x20;

server\_state 메소드는 마지막으로 유효화된 ledger의 ledger 인덱스를 반환합니다.

요청:

```json
{
  "id": "client id 1",
  "method": "server_state"
}
```

응답:

```json
{
    "result": {
        "status": "success",
        "state": {
            "validation_quorum": 3,
            "validated_ledger": {
                "seq": 10268596,
                "reserve_inc": 5000000,
                "reserve_base": 20000000,
                "hash": "0E0901DA980251B8A4CCA17AB4CA6C3168FE83FA1D3F781AFC5B9B097FD209EF",
                "close_time": 470798600,
                "base_fee": 10
            },
            "server_state": "full",
            "published_ledger": 10268596,
            "pubkey_node": "n9LGg37Ya2SS9TdJ4XEuictrJmHaicdgTKiPJYi8QRSdvQd3xMnK",
            "peers": 58,
            "load_factor": 256000,
            "load_base": 256,
            "last_close": {
                "proposers": 5,
                "converge_time": 3004
            },
            "io_latency_ms": 2,
            "fetch_pack": 10121,
            "complete_ledgers": "10256331-10256382,10256412-10268596",
            "build_version": "0.26.4-sp3-private"
        }
    }
}
```

이 예시에서 마지막으로 유효화된 ledger 인덱스는 10268596입니다 (응답 내의 result.state.validated\_ledger에서 확인할 수 있습니다). 또한 이 예시는 ledger 이력에 간격이 있음을 나타냅니다. 여기서 사용된 서버는 그 간격 동안 적용된 거래에 대한 정보를 제공할 수 없습니다 (ledger 10256383부터 10256411까지). 설정이 된다면, 서버는 결국 ledger 이력의 그 부분을 검색합니다.

### 트랜잭션 구성(**Construct the Transaction)**

rippled는 제출을 위한 거래 준비를 위해 sign 메소드를 제공합니다. 이 메소드는 계정 비밀번호를 필요로 합니다, 이는 신뢰하는 rippled 인스턴스에만 전달되어야 합니다. 이 예시는 다른 XRP Ledger 주소로 10 FOO (만들어진 화폐)를 발행합니다.

요청:

```json
{
    "method": "sign",
    "params": [
        {
            "offline": true,
            "secret": "s████████████████████████████",
            "tx_json": {
               "Account": "rG5Ro9e3uGEZVCh3zu5gB9ydKUskCs221W",
                "Sequence": 4,
                "LastLedgerSequence": 10268600,
                "Fee": "10000",
                "Amount": {
                    "currency": "FOO",
                    "issuer": "rG5Ro9e3uGEZVCh3zu5gB9ydKUskCs221W",
                    "value": "10"
                },
                "Destination": "rawz2WQ8i9FdTHp4KSNpBdyxgFqNpKe8fM",
                "TransactionType": "Payment"
            }
        }
    ]
}
```

애플리케이션이 이전의 account\_info 호출에서 배운 계정 시퀀스 "Sequence": 4를 지정하여 tefPAST\_SEQ 오류를 피하는 것에 유의하세요.

server\_state으로부터 학습한 마지막 유효한 ledger에 기반한 LastLedgerSequence에 주목하세요. 백엔드 애플리케이션의 권장사항은 (마지막 유효한 ledger 인덱스 + 4)를 사용하는 것입니다. 또는 (현재 ledger + 3)의 값을 사용할 수도 있습니다. 만약 LastLedgerSequence가 잘못 계산되어 마지막 유효한 ledger보다 작다면, 해당 거래는 tefMAX\_LEDGER 오류로 실패합니다.

응답:

```json
{
    "result": {
        "tx_json": {
            "hash": "395C313F6F11F70FEBAF3785529A6D6DE3F44C7AF679515A7EAE22B30146DE57",
            "TxnSignature": "304402202646962A21EC0516FCE62DC9280F79E7265778C571E9410D795E67BB72A2D8E402202FF4AF7B2E2160F5BCA93011CB548014626CAC7FCBEBDB81FE8193CEFF69C753",
            "TransactionType": "Payment",
            "SigningPubKey": "0267268EE0DDDEE6A862C9FF9DDAF898CF17060A673AF771B565AA2F4AE24E3FC5",
            "Sequence": 4,
            "LastLedgerSequence": 10268600,
            "Flags": 2147483648,
            "Fee": "10000",
            "Destination": "rawz2WQ8i9FdTHp4KSNpBdyxgFqNpKe8fM",
            "Amount": {
                "value": "10",
                "issuer": "rG5Ro9e3uGEZVCh3zu5gB9ydKUskCs221W",
                "currency": "FOO"
            },
            "Account": "rG5Ro9e3uGEZVCh3zu5gB9ydKUskCs221W"
        },
        "tx_blob": "12000022800000002400000004201B009CAFB861D4C38D7EA4C68000000000000000000000000000464F4F0000000000AC5FA3BB28A09BD2EC1AE0EED2315060E83D796A68400000000000271073210267268EE0DDDEE6A862C9FF9DDAF898CF17060A673AF771B565AA2F4AE24E3FC57446304402202646962A21EC0516FCE62DC9280F79E7265778C571E9410D795E67BB72A2D8E402202FF4AF7B2E2160F5BCA93011CB548014626CAC7FCBEBDB81FE8193CEFF69C7538114AC5FA3BB28A09BD2EC1AE0EED2315060E83D796A831438BC6F9F5A6F6C4E474DB0D59892E90C2C7CED5C",
        "status": "success"
    }
}
```

응용 프로그램은 제출하기 전에 트랜잭션의 해시를 유지해야 합니다. 서명 방법의 결과는 tx\_json 아래의 해시를 포함합니다.

## 트랜잭션 제출&#x20;

rippled는 [submit method](https://xrpl.org/submit.html)을 제공하여 서명된 트랜잭션을 제출할 수 있도록 합니다. 이것은 sign 메소드에서 반환된 tx\_blob 매개 변수를 사용합니다.

요청:

```json
{
    "method": "submit",
    "params": [
        {
        "tx_blob": "12000022800000002400000004201B009CAFB861D4C38D7EA4C68000000000000000000000000000464F4F0000000000AC5FA3BB28A09BD2EC1AE0EED2315060E83D796A68400000000000271073210267268EE0DDDEE6A862C9FF9DDAF898CF17060A673AF771B565AA2F4AE24E3FC57446304402202646962A21EC0516FCE62DC9280F79E7265778C571E9410D795E67BB72A2D8E402202FF4AF7B2E2160F5BCA93011CB548014626CAC7FCBEBDB81FE8193CEFF69C7538114AC5FA3BB28A09BD2EC1AE0EED2315060E83D796A831438BC6F9F5A6F6C4E474DB0D59892E90C2C7CED5C"
        }
    ]
}
```

응답:

```json
{
    "result": {
        "tx_json": {
            "hash": "395C313F6F11F70FEBAF3785529A6D6DE3F44C7AF679515A7EAE22B30146DE57",
            "TxnSignature": "304402202646962A21EC0516FCE62DC9280F79E7265778C571E9410D795E67BB72A2D8E402202FF4AF7B2E2160F5BCA93011CB548014626CAC7FCBEBDB81FE8193CEFF69C753",
            "TransactionType": "Payment",
            "SigningPubKey": "0267268EE0DDDEE6A862C9FF9DDAF898CF17060A673AF771B565AA2F4AE24E3FC5",
            "Sequence": 4,
            "LastLedgerSequence": 10268600,
            "Flags": 2147483648,
            "Fee": "10000",
            "Destination": "rawz2WQ8i9FdTHp4KSNpBdyxgFqNpKe8fM",
            "Amount": {
                "value": "10",
                "issuer": "rG5Ro9e3uGEZVCh3zu5gB9ydKUskCs221W",
                "currency": "FOO"
            },
            "Account": "rG5Ro9e3uGEZVCh3zu5gB9ydKUskCs221W"
        },
        "tx_blob": "12000022800000002400000004201B009CAFB861D4C38D7EA4C68000000000000000000000000000464F4F0000000000AC5FA3BB28A09BD2EC1AE0EED2315060E83D796A68400000000000271073210267268EE0DDDEE6A862C9FF9DDAF898CF17060A673AF771B565AA2F4AE24E3FC57446304402202646962A21EC0516FCE62DC9280F79E7265778C571E9410D795E67BB72A2D8E402202FF4AF7B2E2160F5BCA93011CB548014626CAC7FCBEBDB81FE8193CEFF69C7538114AC5FA3BB28A09BD2EC1AE0EED2315060E83D796A831438BC6F9F5A6F6C4E474DB0D59892E90C2C7CED5C",
        "status": "success",
        "engine_result_message": "The transaction was applied.",
        "engine_result_code": 0,
        "engine_result": "tesSUCCESS"
    }
}
```

이것은 예비 결과입니다. 최종 결과는 유효한 ledger으로부터만 사용할 수 있습니다. "validated": true 필드의 부재는 이것이 변경할 수 있는 결과가 아님을 나타냅니다.

## 거래 확인하기&#x20;

거래가 서명될 때 생성된 거래 해시를 tx 메소드에 전달하여 거래의 결과를 검색합니다.

요청:

```json
{
    "method": "tx",
    "params": [
        {
            "transaction": "395C313F6F11F70FEBAF3785529A6D6DE3F44C7AF679515A7EAE22B30146DE57",
            "binary": false
        }
    ]
}
```

응답:

```json
{
    "result": {
        "validated": true,
        "status": "success",
        "meta": {
            "TransactionResult": "tesSUCCESS",
            "TransactionIndex": 2,
            "AffectedNodes": [...]
        },
        "ledger_index": 10268599[d],
        "inLedger": 10268599,
        "hash": "395C313F6F11F70FEBAF3785529A6D6DE3F44C7AF679515A7EAE22B30146DE57",
        "date": 470798270,
        "TxnSignature": "304402202646962A21EC0516FCE62DC9280F79E7265778C571E9410D795E67BB72A2D8E402202FF4AF7B2E2160F5BCA93011CB548014626CAC7FCBEBDB81FE8193CEFF69C753",
        "TransactionType": "Payment",
        "SigningPubKey": "0267268EE0DDDEE6A862C9FF9DDAF898CF17060A673AF771B565AA2F4AE24E3FC5",
        "Sequence": 4,
        "LastLedgerSequence": 10268600,
        "Flags": 2147483648,
        "Fee": "10000",
        "Destination": "rawz2WQ8i9FdTHp4KSNpBdyxgFqNpKe8fM",
        "Amount": {
            "value": "10",
            "issuer": "rG5Ro9e3uGEZVCh3zu5gB9ydKUskCs221W",
            "currency": "FOO"
        },
        "Account": "rG5Ro9e3uGEZVCh3zu5gB9ydKUskCs221W"
    }
}
```

이 예시 응답에서는 "validated": true가 표시되어 있어 거래가 유효한 ledger에 포함되었으며, 따라서 거래 결과는 변경할 수 없습니다. 더 나아가 메타데이터에는 "TransactionResult": "tesSUCCESS"가 포함되어 있어 거래가 ledger에 적용되었음을 나타냅니다.

만약 응답에 "validated": true가 포함되지 않은 경우, 결과는 임시적이며 변경될 수 있습니다. 최종 결과를 검색하기 위해 애플리케이션은 충분한 시간을 확보하여 네트워크가 더 많은 ledger 버전을 유효화할 수 있도록 tx 메소드를 다시 호출해야 합니다. LastLedgerSequence에 지정된 ledger가 유효화되기를 기다려야 할 수도 있지만, 거래가 이전에 유효한 ledger에 포함된 경우 결과는 그 시점부터 변경되지 않습니다.

## 미확인 거래 확인하기(**Verify Missing Transaction)**

응용 프로그램은 tx 메소드에 대한 호출이 txnNotFound 오류를 반환하는 경우를 처리해야 합니다.

```json
{
    "result": {
        "status": "error",
        "request": {
            "transaction": "395C313F6F11F70FEBAF3785529A6D6DE3F44C7AF679515A7EAE22B30146DE56",
            "command": "tx",
            "binary": false
        },
        "error_message": "Transaction not found.",
        "error_code": 24,
        "error": "txnNotFound"
    }
}
```

txnNotFound 결과 코드는 거래가 어떠한 ledger에도 포함되지 않은 경우에 발생합니다. 그러나 rippled 인스턴스가 완전한 ledger 기록을 갖고 있지 않은 경우나 거래가 아직 rippled 인스턴스에 전파되지 않은 경우에도 발생할 수 있습니다. 애플리케이션은 이에 대한 반응 방법을 결정하기 위해 추가적인 쿼리를 수행해야 합니다.

이전에 사용한 server\_state 메소드를 통해 (마지막 유효한 ledger을 확인하는 데 사용한) ledger 기록의 완전성을 확인할 수 있습니다. 이 정보는 result.state.complete\_ledgers에서 확인할 수 있습니다.

```json
{
    "result": {
        "status": "success",
        "state": {
            "validation_quorum": 3,
            "validated_ledger": {
                "seq": 10269447,
                "reserve_inc": 5000000,
                "reserve_base": 20000000,
                "hash": "D05C7ECC66DD6F4FEA3A6394F209EB5D6824A76C16438F562A1749CCCE7EAFC2",
                "close_time": 470802340,
                "base_fee": 10
            },
            "server_state": "full",
            "pubkey_node": "n9LJ5eCNjeUXQpNXHCcLv9PQ8LMFYy4W8R1BdVNcpjc1oDwe6XZF",
            "peers": 84,
            "load_factor": 256000,
            "load_base": 256,
            "last_close": {
                "proposers": 5,
                "converge_time": 2002
            },
            "io_latency_ms": 1,
            "complete_ledgers": "10256331-10256382,10256412-10269447",
            "build_version": "0.26.4-sp3-private"
        }
    }
}
```

저희 예시 거래는 마지막 유효한 ledger을 기준으로 LastLedgerSequence 10268600을 지정했습니다. 이는 해당 시점의 마지막 유효한 ledger 기반으로 +4를 한 값입니다. 누락된 거래가 영구적으로 실패했는지 확인하려면, rippled 서버는 10268597부터 10268600까지의 ledgers 가져야 합니다. 만약 서버가 해당 유효한 ledgers를 기록에 갖고 있고, tx 메소드가 txnNotFound를 반환한다면 거래는 실패한 것이며, 미래의 ledger에 포함될 수 없습니다. 이 경우, 애플리케이션 로직에 따라 동일한 계정 순서와 업데이트된 LastLedgerSequence를 가진 대체 거래를 작성하고 제출해야 할 수 있습니다.

서버는 지정된 LastLedgerSequence보다 작은 마지막 유효한 ledger 인덱스를 보고할 수 있습니다. 이 경우, txnNotFound는 (a) 제출된 거래가 네트워크에 분산되지 않았거나, (b) 거래가 네트워크에 분산되었지만 아직 처리되지 않았음을 나타낼 수 있습니다. 전자의 경우, 애플리케이션은 동일한 서명된 거래를 다시 제출할 수 있습니다. 거래는 고유한 계정 순서 번호를 가지고 있으므로, 최대 한 번만 처리될 수 있습니다.

마지막으로, 서버는 거래 기록에서 하나 이상의 간격을 보여줄 수 있습니다. 위의 응답에서 표시된 completed\_ledgers 필드는 이 rippled 인스턴스에서 ledgers 10256383부터 10256411까지가 누락되었음을 나타냅니다. 저희 예시 거래는 10268597 - 10268600의 ledgers에만 나타날 수 있으므로, 여기에 표시된 간격은 관련이 없습니다. 그러나 간격이 해당 범위의 ledger가 누락되었음을 나타내면, 애플리케이션은 다른 rippled 서버를 쿼리해야 할 수 있습니다 (또는 이 서버가 누락된 ledgers를 검색하기 위해 기다려야 할 수도 있음) 이를 통해 txnNotFound 결과가 변경 불가능한 것임을 확인할 수 있습니다.





## 참고

### See Also <a href="#see-also" id="see-also"></a>

* [Transaction Formats](https://xrpl.org/transaction-formats.html)
* [Transaction Cost](https://xrpl.org/transaction-cost.html)
* [Transaction Malleability](https://xrpl.org/transaction-malleability.html)
* [Overview of XRP Ledger Consensus Process](https://xrpl.org/consensus.html)
* [Consensus Principles and Rules](https://xrpl.org/consensus-principles-and-rules.html)

