# logrotate

logrotate 명령은 로그 파일을 닫았다가 다시 엽니다. 이 명령은 Linux 파일 시스템에서 로그 로테이션을 돕기 위한 것입니다.

대부분의 Linux 시스템에는 이 명령과는 별도로 logrotate 프로그램이 사전 설치되어 있습니다. 애플리케이션별 로그 로테이션 스크립트는 /etc/logrotate.d에 있습니다.

다음 스크립트는 /etc/logrotate.d/rippled로 생성할 수 있는 샘플입니다.

```
/var/log/rippled/*.log {
  daily
  minsize 200M
  rotate 7
  nocreate
  missingok
  notifempty
  compress
  compresscmd /usr/bin/nice
  compressoptions -n19 ionice -c3 gzip
  compressext .gz
  postrotate
    /opt/ripple/bin/rippled --conf /opt/ripple/etc/rippled.cfg logrotate
  endscript
}
```

보관하는 로그의 양에 따라 최소 크기 및 회전과 같은 매개 변수를 구성할 수 있습니다. 서버 로그의 상세 수준을 구성하려면 rippled.cfg 파일의 log\_level 설정을 사용하세요. 이 샘플 스크립트는 표준 log\_level을 기반으로 하며 약 2주 분량의 로그를 압축 형식으로 저장합니다.

CentOS/Red Hat, Ubuntu 또는 Debian용 공식 패키지는 기본적으로 /etc/logrotate.d/rippled 스크립트를 제공합니다. 필요에 따라 이 스크립트를 수정할 수 있습니다. 수정 내용은 패키지 업그레이드 시 덮어쓰지 않습니다.

{% hint style="info" %}
Note:

애플리케이션당 시스템 로그 로테이션 스크립트는 하나만 사용해야 합니다. 동일한 디렉터리를 처리하는 다른 로그 로테이션이 없는지 확인하세요.
{% endhint %}

_`logrotate`_ 메소드는 권한이 없는 사용자가 실행할 수 없는 관리자 메소드입니다.

## 요청 형식

요청 형식의 예입니다:

{% tabs %}
{% tab title="WebSocket" %}
```json
{
    "id": "lr1",
    "command": "logrotate"
}
```
{% endtab %}

{% tab title="Commandline" %}
```
#Syntax: logrotate
rippled logrotate
```
{% endtab %}
{% endtabs %}

요청에 매개변수가 포함되어 있지 않습니다.

## 응답 형식

성공적인 응답의 예입니다:

{% tabs %}
{% tab title="JSON-RPC" %}
```json
200 OK

{
   "result" : {
      "message" : "The log file was closed and reopened.",
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
      "message" : "The log file was closed and reopened.",
      "status" : "success"
   }
}
```
{% endtab %}
{% endtabs %}

응답은 표준 형식을 따르며, 성공적인 결과에는 다음 필드가 포함됩니다:

## 발생 가능한 오류

* 일반적인 오류 유형입니다.
