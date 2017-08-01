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


# アラートの構成
{: #configure_alerts}

照会にアラートを構成することができます。アラートは、通知 E メールを送信したり、Web フックを起動したり、PagerDuty 通知を作成したりすることができます。
{:shortdesc}


## E メールの送信
{: #send_email}

E メールを送信するアラートを作成するには、以下の手順を実行します。

1. [E メールを送信するための通知テンプレートを作成または取得します。](/docs/services/cloud-monitoring/alerts/notifications.html#template) 

    例えば、Ubuntu システムでは以下のコマンドを実行します。
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
    通知テンプレートを保管するディレクトリーを作成します。例えば、*notification-templates* など。

    ```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	以下を実行して、このディレクトリーに移動します。
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
    システムのローカル・ディレクトリーに通知テンプレートを作成します。

    例えば、以下のように、vi エディターを使用してファイル **email-template.json** を作成します。
	
	```
	{
    "name": "email_template",
    "type": "Email",
    "description" : "Send email to manager of department X when ....",
    "detail": "xxx@yyy"
    }
	```
	{: codeblock}	
	
2. 以下のように、 `~/cloud-monitoring/notifications/` ディレクトリーにテンプレートをコピーし、名前、説明、および詳細の各フィールドを変更します。

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
	
3. [通知を作成します。](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    UAA 認証トークン、IAM 認証トークン、または API キーを使用して通知を作成することができます。以下に示す例は、IAM トークン用のものです。

    ```
	curl -XPOST -d @s-email-xxx.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
通知が正常に作成されていることを検証するには、以下のコマンドを実行します。

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/email_xxx
	```
	{: screen}
	
4. [ルール・ファイルを作成します。](/docs/services/cloud-monitoring/alerts/rules.html#create)

    例えば、ルール・ファイル `s-rule-1.json` を `~/cloud-monitoring/rules/` ディレクトリーに作成します。

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
	
	次に、以下のコマンドを実行してそのファイルを有効にします。
	
    ```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	ルールが正常に作成されていることを検証するには、以下のコマンドを実行します。

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}
   
   

## PagerDuty アラートの作成
{: #config_alert_pagerduty}

PagerDuty 通知を送信するアラートを作成するには、以下の手順を実行します。

1. [通知テンプレートを作成または取得します。](/docs/services/cloud-monitoring/alerts/notifications.html#template)

    例えば、Ubuntu システムでは以下のコマンドを実行します。
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
    通知テンプレートを保管するディレクトリーを作成します。例えば、*notification-templates* など。

    ```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	以下を実行して、このディレクトリーに移動します。
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
    システムのローカル・ディレクトリーに通知テンプレートを作成します。

    例えば、以下のように、vi エディターを使用してファイル **pagerduty-template.json** を作成します。
	
	```
	{
    "name": "pagerduty_template",
    "type": "PagerDuty",
    "description" : "Send notification to PagerDuty when ....",
    "detail": "PagerDutyKey"
    }
	```
	{: codeblock}	
	
2. 以下のように、 `~/cloud-monitoring/notifications/` ディレクトリーにテンプレートをコピーし、名前、説明、および詳細の各フィールドを変更します。

    ```
	{
    "name": "pagerduty_notification_1",
    "type": "PagerDuty",
    "description" : "Send notification to PagerDuty for an infrastructure problem.",
    "detail": "abcd1234"
    }
	```
	{: screen}
	
	PagerDuty 通知では、*detail* フィールドに固有の API キーが必要です。この API キーは、PagerDuty アカウントの管理者または所有者によって生成されます。

    ```
	cp ~/cloud-monitoring/notifications/email-template.json ~/cloud-monitoring/notifications/s-pagerduty_notification_1.json
	```
	{: screen}
	
3. [通知を作成します。](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    UAA 認証トークン、IAM 認証トークン、または API キーを使用して通知を作成することができます。以下に示す例は、IAM トークン用のものです。

    ```
	curl -XPOST -d @s-pagerduty_notification_1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
    通知が正常に作成されていることを検証するには、以下のコマンドを実行します。

    ```
	curl -XGET --header "X-Auth-User-Token:uaa ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/pagerduty_notification_1
	```
	{: screen}
	
4. [ルール・ファイル](/docs/services/cloud-monitoring/alerts/rules.html#create) `s-rule-1.json` を `~/cloud-monitoring/rules/` ディレクトリーに作成します。

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
	
	次に、以下のコマンドを実行してそのファイルを有効にします。
	
    ```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	ルールが正常に作成されていることを検証するには、以下のコマンドを実行します。

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}







## Web フックの構成
{: #config_webhook}

Web フックを起動するアラートを作成するには、以下の手順を実行します。

1. [通知テンプレートを作成または取得します。](/docs/services/cloud-monitoring/alerts/notifications.html#template)

    例えば、Ubuntu システムでは以下のコマンドを実行します。
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
    通知テンプレートを保管するディレクトリーを作成します。例えば、*notification-templates* など。

    ```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	以下を実行して、このディレクトリーに移動します。
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
    システムのローカル・ディレクトリーに通知テンプレートを作成します。

    例えば、以下のように、vi エディターを使用してファイル **webhook-template.json** を作成します。
	
	```
	{
    "name": "webhook_template",
    "type": "Webhook",
    "description" : "Fire this webhook when ....",
    "detail": "https://myendpoint.bluemix.net?key=abcd1234"
    }
	```
	{: codeblock}	
	
	Web フックを起動するには、*detail* フィールドを、POST を実行する URL に設定する必要があります。https エンドポイント用に Web フックを構成し、パラメーターを追加します。
	
2. 以下のように、 `~/cloud-monitoring/notifications/` ディレクトリーにテンプレートをコピーし、名前、説明、および詳細の各フィールドを変更します。

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
	
3. [通知を作成します。](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    UAA 認証トークン、IAM 認証トークン、または API キーを使用して通知を作成することができます。以下に示す例は、IAM トークン用のものです。

    ```
	curl -XPOST -d @s-webhook_1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
    通知が正常に作成されていることを検証するには、以下のコマンドを実行します。

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/webhook_1
	```
	{: screen}
	
4. [ルール・ファイル](/docs/services/cloud-monitoring/alerts/rules.html#create) `s-rule-1.json` を `~/cloud-monitoring/rules/` ディレクトリーに作成します。

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
	
	次に、以下のコマンドを実行してそのファイルを有効にします。
	
    ```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	ルールが正常に作成されていることを検証するには、以下のコマンドを実行します。

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}
	
