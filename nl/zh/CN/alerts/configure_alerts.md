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


# 配置警报
{: #configure_alerts}

可以在查询上配置警报。警报可以发送通知电子邮件、调用 Webhook 或创建 PagerDuty 通知。
{:shortdesc}


## 发送电子邮件
{: #send_email}

要创建用于发送电子邮件的警报，请完成以下步骤：

1. [创建或获取用于发送电子邮件的通知模板](/docs/services/cloud-monitoring/alerts/notifications.html#template)。 

    例如，在 Ubuntu 系统中，运行以下命令：
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
    创建一个目录以用于存储通知模板，例如 *notification-templates*。

    ```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	切换到此目录：
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
    在系统的本地目录中创建通知模板。

    例如，使用 vi 编辑器创建 **email-template.json** 文件：
	
	```
	{
    "name": "email_template",
    "type": "Email",
    "description" : "Send email to manager of department X when ....",
    "detail": "xxx@yyy"
    }
	```
	{: codeblock}	
	
2. 将模板复制到“~/cloud-monitoring/notifications/”目录中，然后修改 name、description 和 detail 字段：

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
	
3. [创建通知。](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    可以使用 UAA 认证令牌、IAM 认证令牌或 API 密钥来创建通知。显示的示例是针对 IAM 令牌的：

    ```
	curl -XPOST -d @s-email-xxx.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
要验证通知是否已成功创建，请运行以下命令：
    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/email_xxx
	```
	{: screen}
	
4. [创建规则文件。](/docs/services/cloud-monitoring/alerts/rules.html#create)

    例如，在“~/cloud-monitoring/rules/”目录中创建“s-rule-1.json”规则文件。
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
	
	然后，运行以下命令来启用该文件：
	
    ```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	要验证规则是否已成功创建，请运行以下命令：

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}
   
   

## 配置 PagerDuty 警报
{: #config_alert_pagerduty}

要创建用于发送 PagerDuty 通知的警报，请完成以下步骤：

1. [创建或获取通知模板。](/docs/services/cloud-monitoring/alerts/notifications.html#template) 

    例如，在 Ubuntu 系统中，运行以下命令：
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
    创建一个目录以用于存储通知模板，例如 *notification-templates*。
	
	   ```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	切换到此目录：
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
    在系统的本地目录中创建通知模板。

    例如，使用 vi 编辑器创建 **pagerduty-template.json** 文件：
	
	```
	{
    "name": "pagerduty_template",
    "type": "PagerDuty",
    "description" : "Send notification to PagerDuty when ....",
    "detail": "PagerDutyKey"
    }
	```
	{: codeblock}	
	
2. 将模板复制到“~/cloud-monitoring/notifications/”目录中，然后修改 name、description 和 detail 字段：

    ```
	{
    "name": "pagerduty_notification_1",
    "type": "PagerDuty",
    "description" : "Send notification to PagerDuty for an infrastructure problem.",
    "detail": "abcd1234"
    }
	```
	{: screen}
	
	PagerDuty 通知需要在 *detail* 字段中提供唯一 API 密钥。此 API 密钥由 PagerDuty 帐户管理员或所有者生成。

    ```
	cp ~/cloud-monitoring/notifications/email-template.json ~/cloud-monitoring/notifications/s-pagerduty_notification_1.json
	```
	{: screen}
	
3. [创建通知。](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    可以使用 UAA 认证令牌、IAM 认证令牌或 API 密钥来创建通知。显示的示例是针对 IAM 令牌的：

    ```
	curl -XPOST -d @s-pagerduty_notification_1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
    要验证通知是否已成功创建，请运行以下命令：
    ```
	curl -XGET --header "X-Auth-User-Token:uaa ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/pagerduty_notification_1
	```
	{: screen}
	
4. 在“~/cloud-monitoring/rules/”目录中[创建规则](/docs/services/cloud-monitoring/alerts/rules.html#create)文件“s-rule-1.json”。
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
	
	然后，运行以下命令来启用该文件：
	
    ```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	要验证规则是否已成功创建，请运行以下命令：

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}


 




## 配置 Webhook
{: #config_webhook}

要创建用于调用 Webhook 的警报，请完成以下步骤：

1. [创建或获取通知模板。](/docs/services/cloud-monitoring/alerts/notifications.html#template) 

    例如，在 Ubuntu 系统中，运行以下命令：
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
    创建一个目录以用于存储通知模板，例如 *notification-templates*。
	
	   ```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	切换到此目录：
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
    在系统的本地目录中创建通知模板。

    例如，使用 vi 编辑器创建 **webhook-template.json** 文件：
	
	```
	{
    "name": "webhook_template",
    "type": "Webhook",
    "description" : "Fire this webhook when ....",
    "detail": "https://myendpoint.bluemix.net?key=abcd1234"
    }
	```
	{: codeblock}	
	
	为了调用 Webhook，必须将 *detail* 字段设置为应该执行 POST 操作的 URL。为 HTTPS 端点配置 Webhook 并添加参数。
	
2. 将模板复制到“~/cloud-monitoring/notifications/”目录中，然后修改 name、description 和 detail 字段：

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
	
3. [创建通知。](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    可以使用 UAA 认证令牌、IAM 认证令牌或 API 密钥来创建通知。显示的示例是针对 IAM 令牌的：

    ```
	curl -XPOST -d @s-webhook_1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
    要验证通知是否已成功创建，请运行以下命令：
    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/webhook_1
	```
	{: screen}
	
4. 在“~/cloud-monitoring/rules/”目录中[创建规则](/docs/services/cloud-monitoring/alerts/rules.html#create)文件“s-rule-1.json”。
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
	
	然后，运行以下命令来启用该文件：
	
    ```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	要验证规则是否已成功创建，请运行以下命令：

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}



 
	  
