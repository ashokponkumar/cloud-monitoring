---

copyright:
  years: 2017

lastupdated: "2017-07-18"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# 경보 구성
{: #configure_alerts}

조회에 경보를 구성할 수 있습니다. 경보는 알림 이메일을 발송하거나, 웹훅을 호출하거나, PagerDuty 알림을 작성할 수 있습니다.
{:shortdesc}


## 이메일 발송
{: #send_email}

이메일을 발송하는 경보를 작성하려면 다음 단계를 완료하십시오. 

1. [이메일 발송을 위한 알림 템플리트를 작성하거나 가져오십시오. ](/docs/services/cloud-monitoring/alerts/notifications.html#template) 

    예를 들면, Ubuntu 시스템에서는 다음 명령을 실행하십시오. 
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
    알림 템플리트를 저장할 디렉토리(예: *notification-templates*)를 작성하십시오. 

    ```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	이 디렉토리로 이동하십시오.
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
    시스템의 로컬 디렉토리에서 알림 템플리트를 작성하십시오.

    예를 들면, vi 편집기를 사용하여 파일 **email-template.json**을 작성하십시오.
	
	```
	{
    "name": "email_template",
    "type": "Email",
    "description" : "Send email to manager of department X when ....",
    "detail": "xxx@yyy"
    }
	```
	{: codeblock}	
	
2. 템플리트를 `~/cloud-monitoring/notifications/` 디렉토리에 복사하고 name, description 및 detail 필드를 수정하십시오.

    ```
	{
    "name": "email_xxx",
    "type": "Email",
    "description" : "Send email to manager of department X for an infrastructure problem.",
    "detail": "xxx@yyy.com"
    }
	```
	{: screen}

    ```
	cp ~/cloud-monitoring/notifications/email-template.json ~/cloud-monitoring/notifications/s-email-xxx.json
	```
	{: screen}
	
3. [알림을 작성하십시오.](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    UAA 인증 토큰, IAM 인증 토큰 또는 API 키를 사용하여 알림을 작성할 수 있습니다. 표시된 예는 IAM 토큰을 사용한 예입니다.

    ```
	curl -XPOST -d @s-email-xxx.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
    알림이 작성되었는지 확인하려면 다음 명령을 실행하십시오.

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/email_xxx
	```
	{: screen}
	
4. [규칙 파일을 작성하십시오.](/docs/services/cloud-monitoring/alerts/rules.html#create)

    예를 들면, 규칙 파일 `s-rule-1.json`을 `~/cloud-monitoring/rules/` 디렉토리에 작성하십시오.

    ```
	{
    "name": "checkbytesin",
    "description": "MH check Bytes In per second",
    "expression": "movingAverage(messagehub.65qser11-8034-1234-5678-c82fb42wdfgh.1.kafka-java-console-sample-topic.BytesInPerSec.15MinuteRate,\"5min\")",
    "enabled": true,
    "from": "-5min",
    "until": "now",
    "comparison": "above",
    "comparison_scope": "last",
    "error_level" : 27,
    "warning_level" : 25,
    "frequency": "1min",
    "dashboard_url": "https://metrics.ng.bluemix.net",
    "notifications": [
      "email_xxx"
    ]
    }
    ```
	{: screen}
	
	그 후 다음 명령을 실행하여 이를 사용으로 설정하십시오.
	
    ```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	규칙이 작성되었는지 확인하려면 다음 명령을 실행하십시오.

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}
   
   

## PagerDuty 경보 구성
{: #config_alert_pagerduty}

PagerDuty 알림을 전송하는 경보를 작성하려면 다음 단계를 완료하십시오.

1. [알림 템플리트를 작성하거나 가져오십시오.](/docs/services/cloud-monitoring/alerts/notifications.html#template) 

    예를 들면, Ubuntu 시스템에서는 다음 명령을 실행하십시오.
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
    알림 템플리트를 저장할 디렉토리(예: *notification-templates*)를 작성하십시오.

    ```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	이 디렉토리로 이동하십시오.
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
    시스템의 로컬 디렉토리에서 알림 템플리트를 작성하십시오.

    예를 들면, vi 편집기를 사용하여 파일 **pagerduty-template.json**을 작성하십시오.
	
	```
	{
    "name": "pagerduty_template",
    "type": "PagerDuty",
    "description" : "Send notification to PagerDuty when ....",
    "detail": "PagerDutyKey"
    }
	```
	{: codeblock}	
	
2. 템플리트를 `~/cloud-monitoring/notifications/` 디렉토리에 복사하고 name, description 및 detail 필드를 수정하십시오.

    ```
	{
    "name": "pagerduty_notification_1",
    "type": "PagerDuty",
    "description" : "Send notification to PagerDuty for an infrastructure problem.",
    "detail": "abcd1234"
    }
	```
	{: screen}
	
	PagerDuty 알림은 *detail* 필드에 고유 API 키를 필요로 합니다. 이 API 키는 PagerDuty 계정 관리자 또는 소유자에 의해 생성됩니다.

    ```
	cp ~/cloud-monitoring/notifications/email-template.json ~/cloud-monitoring/notifications/s-pagerduty_notification_1.json
	```
	{: screen}
	
3. [알림을 작성하십시오.](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    UAA 인증 토큰, IAM 인증 토큰 또는 API 키를 사용하여 알림을 작성할 수 있습니다. 표시된 예는 IAM 토큰을 사용한 예입니다.

    ```
	curl -XPOST -d @s-pagerduty_notification_1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
    알림이 작성되었는지 확인하려면 다음 명령을 실행하십시오.

    ```
	curl -XGET --header "X-Auth-User-Token:uaa ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/pagerduty_notification_1
	```
	{: screen}
	
4. [규칙 파일 `s-rule-1.json`을 `~/cloud-monitoring/rules/` 디렉토리에 작성하십시오.](/docs/services/cloud-monitoring/alerts/rules.html#create)

    ```
	{
    "name": "checkbytesin",
    "description": "MH check Bytes In per second",
    "expression": "movingAverage(messagehub.65qser11-8034-1234-5678-c82fb42wdfgh.1.kafka-java-console-sample-topic.BytesInPerSec.15MinuteRate,\"5min\")",
    "enabled": true,
    "from": "-5min",
    "until": "now",
    "comparison": "above",
    "comparison_scope": "last",
    "error_level" : 27,
    "warning_level" : 25,
    "frequency": "1min",
    "dashboard_url": "https://metrics.ng.bluemix.net",
    "notifications": [
      "pagerduty_notification_1"
    ]
    }
    ```
	{: screen}
	
	그 후 다음 명령을 실행하여 이를 사용으로 설정하십시오.
	
    ```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	규칙이 작성되었는지 확인하려면 다음 명령을 실행하십시오.

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}


 




## 웹훅 구성
{: #config_webhook}

웹훅을 호출하는 경보를 작성하려면 다음 단계를 완료하십시오.

1. [알림 템플리트를 작성하거나 가져오십시오.](/docs/services/cloud-monitoring/alerts/notifications.html#template) 

    예를 들면, Ubuntu 시스템에서는 다음 명령을 실행하십시오.
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
    알림 템플리트를 저장할 디렉토리(예: *notification-templates*)를 작성하십시오.

    ```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	이 디렉토리로 이동하십시오.
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
    시스템의 로컬 디렉토리에서 알림 템플리트를 작성하십시오.

    예를 들면, vi 편집기를 사용하여 파일 **webhook-template.json**을 작성하십시오.
	
	```
	{
    "name": "webhook_template",
    "type": "Webhook",
    "description" : "Fire this webhook when ....",
    "detail": "https://myendpoint.bluemix.net?key=abcd1234"
    }
	```
	{: codeblock}	
	
	웹훅을 호출하려면 필드 *detail*을 POST를 수행해야 하는 URL로 설정해야 합니다. https 엔드포인트에 대한 웹훅을 구성하고 매개변수를 추가하십시오.
	
2. 템플리트를 `~/cloud-monitoring/notifications/` 디렉토리에 복사하고 name, description 및 detail 필드를 수정하십시오.

    ```
	{
    "name": "webhook_1",
    "type": "Webhook",
    "description" : "Invoke webhook for an infrastructure problem.",
    "detail": "https://myendpoint.bluemix.net?key=abcd1234"
    }
	```
	{: screen}

    ```
	cp ~/cloud-monitoring/notifications/email-template.json ~/cloud-monitoring/notifications/s-webhook_1.json
	```
	{: screen}
	
3. [알림을 작성하십시오.](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    UAA 인증 토큰, IAM 인증 토큰 또는 API 키를 사용하여 알림을 작성할 수 있습니다. 표시된 예는 IAM 토큰을 사용한 예입니다.

    ```
	curl -XPOST -d @s-webhook_1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
    알림이 작성되었는지 확인하려면 다음 명령을 실행하십시오.

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/webhook_1
	```
	{: screen}
	
4. [규칙 파일 `s-rule-1.json`을 `~/cloud-monitoring/rules/` 디렉토리에 작성하십시오.](/docs/services/cloud-monitoring/alerts/rules.html#create)

    ```
	{
    "name": "checkbytesin",
    "description": "MH check Bytes In per second",
    "expression": "movingAverage(messagehub.65qser11-8034-1234-5678-c82fb42wdfgh.1.kafka-java-console-sample-topic.BytesInPerSec.15MinuteRate,\"5min\")",
    "enabled": true,
    "from": "-5min",
    "until": "now",
    "comparison": "above",
    "comparison_scope": "last",
    "error_level" : 27,
    "warning_level" : 25,
    "frequency": "1min",
    "dashboard_url": "https://metrics.ng.bluemix.net",
    "notifications": [
      "webhook_1"
    ]
    }
    ```
	{: screen}
	
	그 후 다음 명령을 실행하여 이를 사용으로 설정하십시오.
	
    ```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	규칙이 작성되었는지 확인하려면 다음 명령을 실행하십시오.

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}



 
	  
