# json

json 메소드는 다른 명령을 실행하기 위한 프록시이며, 명령의 매개변수를 JSON 값으로 받아들입니다. 이 메소드는 커맨드라인 클라이언트 전용이며, 매개변수 지정을 위한 커맨드라인 구문이 부적절하거나 바람직하지 않은 경우에 사용할 수 있습니다.

## 요청 형식

요청 형식의 예시입니다:

{% tabs %}
{% tab title="Commandline" %}
```
# Syntax: json method json_stanza
rippled -q json ledger_closed '{}'
```
{% endtab %}
{% endtabs %}

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```
{
   "result" : {
      "ledger_hash" : "8047C3ECF1FA66326C1E57694F6814A1C32867C04D3D68A851367EE2F89BBEF3",
      "ledger_index" : 390308,
      "status" : "success"
   }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 명령 유형에 적합한 필드를 포함합니다.
