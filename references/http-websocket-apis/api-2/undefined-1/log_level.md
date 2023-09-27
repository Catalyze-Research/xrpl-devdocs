# log\_level

문자열문자열log\_level 명령은 rippled 서버의 로깅 상세도를 변경하거나 로그 메시지의 각 카테고리(파티션이라고 함)에 대한 현재 로깅 레벨을 반환합니다.

log\_level 메소드는 권한이 없는 사용자가 실행할 수 없는 관리자 메소드입니다.

## 요청 형식

요청 형식의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": "ll1",
    "command": "log_level",
    "severity": "debug",
    "partition": "PathRequest"
}
```
{% endtab %}

{% tab title="Commandline" %}
```
#Syntax: log_level [[partition] severity]
rippled log_level PathRequest debug
```
{% endtab %}
{% endtabs %}

요청에는 다음 매개변수가 포함됩니다:

| `Field`     | 유형  | 설명                                                                                                                              |
| ----------- | --- | ------------------------------------------------------------------------------------------------------------------------------- |
| `severity`  | 문자열 | (선택 사항) 로깅을 설정할 상세도 수준입니다. 유효한 값은 치명적, 오류, 경고, 정보, 디버그, 추적 순으로 가장 상세하지 않은 것부터 가장 상세한 것까지입니다. 생략하면 모든 카테고리에 대한 현재 로그 상세도를 반환합니다. |
| `partition` | 문자열 | (선택 사항) 심각도를 제공하지 않으면 무시됩니다. 수정할 로깅 카테고리입니다. 생략하거나 값 기반과 함께 제공된 경우 모든 카테고리에 대한 로깅 수준을 설정합니다                                     |

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="Commandline (set log level)" %}
```json
Field	Type	Description
severity	String	(Optional) What level of verbosity to set logging at. Valid values are, in order from least to most verbose: fatal, error, warn, info, debug, and trace. If omitted, return current log verbosity for all categories.
partition	String	(Optional) Ignored unless severity is provided. Which logging category to modify. If omitted, or if provided with the value base, set logging level for all categories.
```
{% endtab %}

{% tab title="Commandline (check log level)" %}
```json
Field	Type	Description
severity	String	(Optional) What level of verbosity to set logging at. Valid values are, in order from least to most verbose: fatal, error, warn, info, debug, and trace. If omitted, return current log verbosity for all categories.
partition	String	(Optional) Ignored unless severity is provided. Which logging category to modify. If omitted, or if provided with the value base, set logging level for all categories.
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따릅니다. 응답 형식은 요청에 심각도가 지정되었는지 여부에 따라 달라집니다. 요청이 심각도를 지정한 경우 로그 수준이 변경되고 성공적인 결과에는 추가 필드가 포함되지 않습니다.

그렇지 않으면 응답에 다음 필드가 포함됩니다:

| `Field` | 유형 | 설명                                                                                                        |
| ------- | -- | --------------------------------------------------------------------------------------------------------- |
| `level` | 객체 | 각 카테고리의 현재 로그 수준입니다. 이 카테고리 목록은 향후 릴리스에서 예고 없이 변경될 수 있습니다. 필드 이름을 이 명령에 대한 요청에서 파티션을 지정하는 값으로 사용할 수 있습니다. |

## 가능한 오류

* 일반적인 오류 유형입니다.
* invalidParams - 하나 이상의 필드가 잘못 지정되었거나 하나 이상의 필수 필드가 누락되었습니다.

&#x20;
