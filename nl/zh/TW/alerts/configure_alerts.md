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


# 配置警示
{: #configure_alerts}

您可以針對查詢配置警示。警示可以傳送通知電子郵件、呼叫 Webhook，或建立 PagerDuty 通知。
{:shortdesc}


## 傳送電子郵件
{: #send_email}

若要建立傳送電子郵件的警示，請完成下列步驟：

1. [建立或取得傳送電子郵件用的通知範本。](/docs/services/cloud-monitoring/alerts/notifications.html#template) 

    例如，在 Ubuntu 系統中，執行下列指令：
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
    建立目錄（例如 *notification-templates*）來儲存通知範本。

	```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	切換至此目錄：
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
    在系統的本端目錄中建立通知範本。

    例如，使用 vi 編輯器建立檔案 **email-template.json**： 
	
	```
	{
    "name": "email_template",
    "type": "Email",
    "description" : "Send email to manager of department X when ....",
    "detail": "xxx@yyy"
    }
	```
	{: codeblock}	
	
2. 將範本複製到 `~/cloud-monitoring/notifications/` 目錄，然後修改 name、description 及 detail 欄位：

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
	
3. [建立通知。](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    您可以使用 UAA 鑑別記號、IAM 鑑別記號或 API 金鑰來建立通知。以下顯示的範例是針對 IAM 記號：

	```
	curl -XPOST -d @s-email-xxx.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
    若要驗證已順利建立通知，請執行下列指令：

	```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/email_xxx
	```
	{: screen}
	
4. [建立規則檔案。](/docs/services/cloud-monitoring/alerts/rules.html#create)

    例如，在 `~/cloud-monitoring/rules/` 目錄建立規則檔案 `s-rule-1.json`。 

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
	
	然後，執行下列指令來啟用它：
	
	```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	若要驗證已順利建立規則，請執行下列指令：

	```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}
   
   

## 配置 PagerDuty 警示
{: #config_alert_pagerduty}

若要建立傳送 PagerDuty 通知的警示，請完成下列步驟：

1. [建立或取得通知範本。](/docs/services/cloud-monitoring/alerts/notifications.html#template) 

    例如，在 Ubuntu 系統中，執行下列指令：
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
    建立目錄（例如 *notification-templates*）來儲存通知範本。

	```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	切換至此目錄：
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
    在系統的本端目錄中建立通知範本。

    例如，使用 vi 編輯器建立檔案 **pagerduty-template.json**： 
	
	```
	{
    "name": "pagerduty_template",
    "type": "PagerDuty",
    "description" : "Send notification to PagerDuty when ....",
    "detail": "PagerDutyKey"
    }
	```
	{: codeblock}	
	
2. 將範本複製到 `~/cloud-monitoring/notifications/` 目錄，然後修改 name、description 及 detail 欄位：

	```
	{
    "name": "pagerduty_notification_1",
	"type": "PagerDuty",
	"description" : "Send notification to PagerDuty for an infrastructure problem.",
	"detail": "abcd1234"
	}
	```
	{: screen}
	
	PagerDuty 通知要求在 *detail* 欄位中有唯一的 API 金鑰。此 API 金鑰是由 PagerDuty 帳戶管理者或擁有者所產生。

	```
	cp ~/cloud-monitoring/notifications/email-template.json ~/cloud-monitoring/notifications/s-pagerduty_notification_1.json
	```
	{: screen}
	
3. [建立通知。](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    您可以使用 UAA 鑑別記號、IAM 鑑別記號或 API 金鑰來建立通知。以下顯示的範例是針對 IAM 記號：

	```
	curl -XPOST -d @s-pagerduty_notification_1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
    若要驗證已順利建立通知，請執行下列指令：

	```
	curl -XGET --header "X-Auth-User-Token:uaa ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/pagerduty_notification_1
	```
	{: screen}
	
4. 在 `~/cloud-monitoring/rules/` 目錄[建立規則](/docs/services/cloud-monitoring/alerts/rules.html#create)檔案 `s-rule-1.json`。 

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
	
	然後，執行下列指令來啟用它：
	
	```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	若要驗證已順利建立規則，請執行下列指令：

	```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}


 




## 配置 Webhook
{: #config_webhook}

若要建立呼叫 Webhook 的警示，請完成下列步驟：

1. [建立或取得通知範本。](/docs/services/cloud-monitoring/alerts/notifications.html#template) 

    例如，在 Ubuntu 系統中，執行下列指令：
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
    建立目錄（例如 *notification-templates*）來儲存通知範本。

	```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	切換至此目錄：
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
    在系統的本端目錄中建立通知範本。

    例如，使用 vi 編輯器建立檔案 **webhook-template.json**： 
	
	```
	{
    "name": "webhook_template",
    "type": "Webhook",
    "description" : "Fire this webhook when ....",
    "detail": "https://myendpoint.bluemix.net?key=abcd1234"
    }
	```
	{: codeblock}	
	
	若要呼叫 Webhook，*detail* 欄位必須設定為應該進行 POST 的 URL。針對 HTTPS 端點配置 Webhook，然後新增參數。
	
2. 將範本複製到 `~/cloud-monitoring/notifications/` 目錄，然後修改 name、description 及 detail 欄位：

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
	
3. [建立通知。](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    您可以使用 UAA 鑑別記號、IAM 鑑別記號或 API 金鑰來建立通知。以下顯示的範例是針對 IAM 記號：

	```
	curl -XPOST -d @s-webhook_1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
    若要驗證已順利建立通知，請執行下列指令：

	```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/webhook_1
	```
	{: screen}
	
4. 在 `~/cloud-monitoring/rules/` 目錄[建立規則](/docs/services/cloud-monitoring/alerts/rules.html#create)檔案 `s-rule-1.json`。 

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
	
	然後，執行下列指令來啟用它：
	
	```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	若要驗證已順利建立規則，請執行下列指令：

	```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}



 
	  
