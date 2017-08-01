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


# Alerts konfigurieren
{: #configure_alerts}

Sie können einen Alert für eine Abfrage konfigurieren. Der Alert kann eine Benachrichtigungs-E-Mail senden, einen Webhook aufrufen oder eine PagerDuty-Benachrichtigung erstellen.
{:shortdesc}


## Eine E-Mail senden
{: #send_email}

Führen Sie die folgenden Schritte aus, um eine E-Mail zu senden:

1. [Eine Benachrichtigungsvorlage zum Senden von E-Mails erstellen oder abrufen](/docs/services/cloud-monitoring/alerts/notifications.html#template) 

    Führen Sie unter dem Betriebssystem Ubuntu zum Beispiel die folgenden Befehle aus:
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
    Erstellen Sie ein Verzeichnis zum Speichern Ihrer Benachrichtigungsvorlagen, z. B. *notification-templates*.

    ```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	Wechseln Sie in dieses Verzeichnis:
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
    Erstellen Sie eine Benachrichtigungsvorlage in einem lokalen Verzeichnis auf Ihrem System.

    Erstellen Sie zum Beispiel die Datei **email-template.json** mit dem Editor vi: 
	
	```
	{
    "name": "email_template",
    "type": "Email",
    "description" : "Send email to manager of department X when ....",
    "detail": "xxx@yyy"
    }
	```
	{: codeblock}	
	
2. Kopieren Sie die Vorlage in das Verzeichnis `~/cloud-monitoring/notifications/` und ändern Sie die Angaben in den Feldern für den Namen, für die Beschreibung und für Details.

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
	
3. [Erstellen Sie eine Benachrichtigung.](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    Sie können eine Benachrichtigung mithilfe eines UAA-Authentifizierungstokens, eines IAM-Authentifizierungstoken oder eines API-Schlüssels erstellen. Das angezeigte Beispiel verwendet einen IAM-Token:

    ```
	curl -XPOST -d @s-email-xxx.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
    Führen Sie folgenden Befehl aus, um zu überprüfen, ob die Benachrichtigung erfolgreich erstellt wurde:

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/email_xxx
	```
	{: screen}
	
4. [Erstellen Sie eine Regeldatei.](/docs/services/cloud-monitoring/alerts/rules.html#create)

    Erstellen Sie zum Beispiel die Regeldatei `s-rule-1.json` im Verzeichnis `~/cloud-monitoring/rules/`. 

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
	
	Führen Sie anschließend den folgenden Befehl aus, um sie zu aktivieren:
	
    ```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	Um zu überprüfen, ob die Regel erfolgreich erstellt wurde, führen Sie den folgenden Befehl aus:

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}
   
   

## Einen PagerDuty-Alert konfigurieren
{: #config_alert_pagerduty}

Führen Sie die folgenden Schritte aus, um einen Alert zu erstellen, der eine PagerDuty-Benachrichtigung sendet:

1. [Erstellen Sie eine Benachrichtigungsvorlage oder rufen Sie eine solche ab.](/docs/services/cloud-monitoring/alerts/notifications.html#template) 

    Führen Sie unter dem Betriebssystem Ubuntu zum Beispiel die folgenden Befehle aus:
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
    Erstellen Sie ein Verzeichnis zum Speichern Ihrer Benachrichtigungsvorlagen, z. B. *notification-templates*.

    ```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	Wechseln Sie in dieses Verzeichnis:
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
    Erstellen Sie eine Benachrichtigungsvorlage in einem lokalen Verzeichnis auf Ihrem System.

    Erstellen Sie zum Beispiel die Datei **pagerduty-template.json** mit dem Editor vi: 
	
	```
	{
    "name": "pagerduty_template",
    "type": "PagerDuty",
    "description" : "Send notification to PagerDuty when ....",
    "detail": "PagerDutyKey"
    }
	```
	{: codeblock}	
	
2. Kopieren Sie die Vorlage in das Verzeichnis `~/cloud-monitoring/notifications/` und ändern Sie die Angaben in den Feldern für den Namen, für die Beschreibung und für Details.

    ```
	{
    "name": "pagerduty_notification_1",
    "type": "PagerDuty",
    "description" : "Send notification to PagerDuty for an infrastructure problem.",
    "detail": "abcd1234"
    }
	```
	{: screen}
	
	PagerDuty-Benachrichtigungen erfordern einen eindeutigen API-Schlüssel im Feld *detail*. Dieser API-Schlüssel wird vom Administrator oder Eigner eines PagerDuty-Kontos generiert.

    ```
	cp ~/cloud-monitoring/notifications/email-template.json ~/cloud-monitoring/notifications/s-pagerduty_notification_1.json
	```
	{: screen}
	
3. [Erstellen Sie eine Benachrichtigung.](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    Sie können eine Benachrichtigung mithilfe eines UAA-Authentifizierungstokens, eines IAM-Authentifizierungstoken oder eines API-Schlüssels erstellen. Das angezeigte Beispiel verwendet einen IAM-Token:

    ```
	curl -XPOST -d @s-pagerduty_notification_1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
    Führen Sie folgenden Befehl aus, um zu überprüfen, ob die Benachrichtigung erfolgreich erstellt wurde:

    ```
	curl -XGET --header "X-Auth-User-Token:uaa ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/pagerduty_notification_1
	```
	{: screen}
	
4. [Erstellen Sie eine Regel.](/docs/services/cloud-monitoring/alerts/rules.html#create) Datei `s-rule-1.json` in Verzeichnis `~/cloud-monitoring/rules/`. 

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
	
	Führen Sie anschließend den folgenden Befehl aus, um sie zu aktivieren:
	
    ```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	Um zu überprüfen, ob die Regel erfolgreich erstellt wurde, führen Sie den folgenden Befehl aus:

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}


 




## Einen Webhook konfigurieren
{: #config_webhook}

Führen Sie die folgenden Schritte aus, um einen Alert zu erstellen, der einen Webhook aufruft:

1. [Erstellen Sie eine Benachrichtigungsvorlage oder rufen Sie eine solche ab.](/docs/services/cloud-monitoring/alerts/notifications.html#template) 

    Führen Sie unter dem Betriebssystem Ubuntu zum Beispiel die folgenden Befehle aus:
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
    Erstellen Sie ein Verzeichnis zum Speichern Ihrer Benachrichtigungsvorlagen, z. B. *notification-templates*.

    ```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	Wechseln Sie in dieses Verzeichnis:
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
    Erstellen Sie eine Benachrichtigungsvorlage in einem lokalen Verzeichnis auf Ihrem System.

    Erstellen Sie zum Beispiel die Datei **webhook-template.json** mit dem Editor vi: 
	
	```
	{
    "name": "webhook_template",
    "type": "Webhook",
    "description" : "Fire this webhook when ....",
    "detail": "https://myendpoint.bluemix.net?key=abcd1234"
    }
	```
	{: codeblock}	
	
	Um einen Webhook aufzurufen, muss für das Feld *detail* die URL festgelegt sein, bei der der POST ausgeführt werden soll. Konfigurieren Sie einen Webhook für HTTPS-Endpunkte und fügen Sie einen Parameter hinzu.
	
2. Kopieren Sie die Vorlage in das Verzeichnis `~/cloud-monitoring/notifications/` und ändern Sie die Angaben in den Feldern für den Namen, für die Beschreibung und für Details.

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
	
3. [Erstellen Sie eine Benachrichtigung.](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    Sie können eine Benachrichtigung mithilfe eines UAA-Authentifizierungstokens, eines IAM-Authentifizierungstoken oder eines API-Schlüssels erstellen. Das angezeigte Beispiel verwendet einen IAM-Token:

    ```
	curl -XPOST -d @s-webhook_1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
    Führen Sie folgenden Befehl aus, um zu überprüfen, ob die Benachrichtigung erfolgreich erstellt wurde:

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/webhook_1
	```
	{: screen}
	
4. [Erstellen Sie eine Regel.](/docs/services/cloud-monitoring/alerts/rules.html#create) Datei `s-rule-1.json` in Verzeichnis `~/cloud-monitoring/rules/`. 

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
	
	Führen Sie anschließend den folgenden Befehl aus, um sie zu aktivieren:
	
    ```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	Um zu überprüfen, ob die Regel erfolgreich erstellt wurde, führen Sie den folgenden Befehl aus:

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}



 
	  
