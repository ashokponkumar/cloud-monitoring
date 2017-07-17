---

copyright:
  years: 2017

lastupdated: "2017-07-12"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Configuring alerts
{: #configure_alerts}

You can configure an alert on a query. The alert can send a notification email, invoke a webhook, or create a PagerDuty notification.
{:shortdesc}


## Sending an email
{: #send_email}

To create an alert that send an email, complete the following steps:

1. [Create or get a notification template for sending emails.](/docs/services/cloud-monitoring/alerts/notifications.html#template) 

    For example, in an Ubuntu system, run the following commands:
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
    Create a directory to store your notification templates, for example, *notification-templates*.

    ```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	Change to this directory:
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
    Create a notification template in a local directory of your system.

    For example, create the file **email-template.json** by using the vi editor: 
	
	```
	{
    "name": "email_template",
    "type": "Email",
    "description" : "Send email to manager of department X when ....",
    "detail": "xxx@yyy"
    }
	```
	{: codeblock}	
	
2. Copy the template into the `~/cloud-monitoring/notifications/` directory and modify the name, description, and detail fields:

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
	
3. [Create a notification.](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    You can create a notification by using a UAA authentication token, an IAM authentication token, or an API key. The example that is shown is for an IAM token:

    ```
	curl -XPOST -d @s-email-xxx.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
    To verify that the notification is created successfully, run the following command:

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/email_xxx
	```
	{: screen}
	
4. [Create a rule file.](/docs/services/cloud-monitoring/alerts/rules.html#create)

    For example, create the rule file `s-rule-1.json` into the `~/cloud-monitoring/rules/` directory. 

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
	
	Then, run the following command to enable it:
	
    ```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	To verify that the rule is created successfully, run the following command:

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}
   
   

## Configuring a PagerDuty alert
{: #config_alert_pagerduty}

To create an alert that sends a PagerDuty notification, complete the following steps:

1. [Create or get a notification template.](/docs/services/cloud-monitoring/alerts/notifications.html#template) 

    For example, in an Ubuntu system, run the following commands:
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
    Create a directory to store your notification templates, for example, *notification-templates*.

    ```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	Change to this directory:
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
    Create a notification template in a local directory of your system.

    For example, create the file **pagerduty-template.json** by using the vi editor: 
	
	```
	{
    "name": "pagerduty_template",
    "type": "PagerDuty",
    "description" : "Send notification to PagerDuty when ....",
    "detail": "PagerDutyKey"
    }
	```
	{: codeblock}	
	
2. Copy the template into the `~/cloud-monitoring/notifications/` directory and modify the name, description, and detail fields:

    ```
	{
    "name": "pagerduty_notification_1",
    "type": "PagerDuty",
    "description" : "Send notification to PagerDuty for an infrastructure problem.",
    "detail": "abcd1234"
    }
	```
	{: screen}
	
	PagerDuty notifications require a unique API key in the *detail* field. This API key is generated by a PagerDuty account admin or owner.

    ```
	cp ~/cloud-monitoring/notifications/email-template.json ~/cloud-monitoring/notifications/s-pagerduty_notification_1.json
	```
	{: screen}
	
3. [Create a notification.](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    You can create a notification by using a UAA authentication token, an IAM authentication token, or an API key. The example that is shown is for an IAM token:

    ```
	curl -XPOST -d @s-pagerduty_notification_1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
    To verify that the notification is created successfully, run the following command:

    ```
	curl -XGET --header "X-Auth-User-Token:uaa ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/pagerduty_notification_1
	```
	{: screen}
	
4. [Create a rule](/docs/services/cloud-monitoring/alerts/rules.html#create) file `s-rule-1.json` into the `~/cloud-monitoring/rules/` directory. 

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
	
	Then, run the following command to enable it:
	
    ```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	To verify that the rule is created successfully, run the following command:

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}


 




## Configuring a webhook
{: #config_webhook}

To create an alert that invokes a webhook, complete the following steps:

1. [Create or get a notification template.](/docs/services/cloud-monitoring/alerts/notifications.html#template) 

    For example, in an Ubuntu system, run the following commands:
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
    Create a directory to store your notification templates, for example, *notification-templates*.

    ```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	Change to this directory:
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
    Create a notification template in a local directory of your system.

    For example, create the file **webhook-template.json** by using the vi editor: 
	
	```
	{
    "name": "webhook_template",
    "type": "Webhook",
    "description" : "Fire this webhook when ....",
    "detail": "https://myendpoint.bluemix.net?key=abcd1234"
    }
	```
	{: codeblock}	
	
	In order to invoke a webhook, the field *detail* must be set to the URL where the POST should be made. Configure a webhook for https endpoints and add a parameter.
	
2. Copy the template into the `~/cloud-monitoring/notifications/` directory and modify the name, description, and detail fields:

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
	
3. [Create a notification.](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    You can create a notification by using a UAA authentication token, an IAM authentication token, or an API key. The example that is shown is for an IAM token:

    ```
	curl -XPOST -d @s-webhook_1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
    To verify that the notification is created successfully, run the following command:

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/webhook_1
	```
	{: screen}
	
4. [Create a rule](/docs/services/cloud-monitoring/alerts/rules.html#create) file `s-rule-1.json` into the `~/cloud-monitoring/rules/` directory. 

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
	
	Then, run the following command to enable it:
	
    ```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	To verify that the rule is created successfully, run the following command:

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}



 
	  