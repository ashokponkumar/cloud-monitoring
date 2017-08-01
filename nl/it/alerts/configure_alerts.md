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


# Configurazione degli avvisi
{: #configure_alerts}

Puoi configurare un avviso su una query. L'avviso può inviare un'e-mail di notifica, richiamare un webhook o creare una notifica PagerDuty.
{:shortdesc}


## Invio di un'e-mail
{: #send_email}

Per creare un avviso che invia un'e-mail, completa la seguente procedura:

1. [Crea o ottieni un template di notifica per l'invio di e-mail.](/docs/services/cloud-monitoring/alerts/notifications.html#template) 

    Ad esempio, in un sistema Ubuntu, immetti i seguenti comandi:
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
    Crea una directory per memorizzare i tuoi template di notifica, ad esempio, *notification-templates*.

    ```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	Passa a questa directory:
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
    Crea un template di notifica in una directory locale del tuo sistema.

    Ad esempio, crea il file **email-template.json** utilizzando l'editor vi: 
	
	```
	{
    "name": "email_template",
    "type": "Email",
    "description" : "Invia e-mail al responsabile del dipartimento X quando...",
    "detail": "xxx@yyy"
    }
	```
	{: codeblock}	
	
2. Copia il template nella directory `~/cloud-monitoring/notifications/` e modifica i campi di nome, descrizione e dettagli:

    ```
	{
    "name": "email_xxx",
    "type": "Email",
    "description" : "Invia e-mail al responsabile del dipartimento X per un problema di infrastruttura.",
    "detail": "xxx@yyy.com"
    }
	```
	{: screen}

    ```
	cp ~/cloud-monitoring/notifications/email-template.json ~/cloud-monitoring/notifications/s-email-xxx.json
	```
	{: screen}
	
3. [Crea una notifica.](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    Puoi creare una notifica utilizzando un token di autenticazione UAA, un token di autenticazione IAM o una chiave API. L'esempio mostrato è per un token IAM:

    ```
	curl -XPOST -d @s-email-xxx.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
    Per verificare che la notifica sia stata creata correttamente, esegui questo comando:

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/email_xxx
	```
	{: screen}
	
4. [Crea un file delle regole.](/docs/services/cloud-monitoring/alerts/rules.html#create)

    Ad esempio, crea il file di regole `s-rule-1.json` nella directory `~/cloud-monitoring/rules/`. 

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
	
	Immetti quindi il seguente comando per abilitarla:
	
    ```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	Per verificare che la regola sia stata creata correttamente, esegui questo comando:

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}
   
   

## Configurazione di un avviso PagerDuty
{: #config_alert_pagerduty}

Per creare un avviso che invia una notifica PagerDuty, completa la seguente procedura:

1. [Crea o ottieni un template di notifica.](/docs/services/cloud-monitoring/alerts/notifications.html#template) 

    Ad esempio, in un sistema Ubuntu, immetti i seguenti comandi:
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
    Crea una directory per memorizzare i tuoi template di notifica, ad esempio, *notification-templates*.

    ```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	Passa a questa directory:
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
    Crea un template di notifica in una directory locale del tuo sistema.

    Ad esempio, crea il file **pagerduty-template.json** utilizzando l'editor vi: 
	
	```
	{
    "name": "pagerduty_template",
    "type": "PagerDuty",
    "description" : "Invia notifica a PagerDuty quando...",
    "detail": "PagerDutyKey"
    }
	```
	{: codeblock}	
	
2. Copia il template nella directory `~/cloud-monitoring/notifications/` e modifica i campi di nome, descrizione e dettagli:

    ```
	{
    "name": "pagerduty_notification_1",
    "type": "PagerDuty",
    "description" : "Invia notifica a PagerDuty per un problema di infrastruttura.",
    "detail": "abcd1234"
    }
	```
	{: screen}
	
	Le notifiche PagerDuty richiedono una chiave API univoca nel campo *detail*. Questa chiave API viene generata da un amministratore o proprietario dell'account PagerDuty.

    ```
	cp ~/cloud-monitoring/notifications/email-template.json ~/cloud-monitoring/notifications/s-pagerduty_notification_1.json
	```
	{: screen}
	
3. [Crea una notifica.](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    Puoi creare una notifica utilizzando un token di autenticazione UAA, un token di autenticazione IAM o una chiave API. L'esempio mostrato è per un token IAM:

    ```
	curl -XPOST -d @s-pagerduty_notification_1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
    Per verificare che la notifica sia stata creata correttamente, esegui questo comando:

    ```
	curl -XGET --header "X-Auth-User-Token:uaa ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/pagerduty_notification_1
	```
	{: screen}
	
4. [Crea una regola](/docs/services/cloud-monitoring/alerts/rules.html#create) nel file `s-rule-1.json` nella directory `~/cloud-monitoring/rules/`. 

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
	
	Immetti quindi il seguente comando per abilitarla:
	
    ```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	Per verificare che la regola sia stata creata correttamente, esegui questo comando:

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}


 




## Configurazione di un webhook
{: #config_webhook}

Per creare un avviso che richiama un webhook, completa la seguente procedura:

1. [Crea o ottieni un template di notifica.](/docs/services/cloud-monitoring/alerts/notifications.html#template) 

    Ad esempio, in un sistema Ubuntu, immetti i seguenti comandi:
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
    Crea una directory per memorizzare i tuoi template di notifica, ad esempio, *notification-templates*.

    ```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	Passa a questa directory:
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
    Crea un template di notifica in una directory locale del tuo sistema.

    Ad esempio, crea il file **webhook-template.json** utilizzando l'editor vi: 
	
	```
	{
    "name": "webhook_template",
    "type": "Webhook",
    "description" : "Attiva questo webhook quando...",
    "detail": "https://myendpoint.bluemix.net?key=abcd1234"
    }
	```
	{: codeblock}	
	
	Per richiamare un webhook, il campo *detail* deve essere impostato sull'URL in cui effettuare il POST. Configura un webhook per gli endpoint https e aggiungi un parametro.
	
2. Copia il template nella directory `~/cloud-monitoring/notifications/` e modifica i campi di nome, descrizione e dettagli:

    ```
	{
    "name": "webhook_1",
    "type": "Webhook",
    "description" : "Richiama webhook per un problema di infrastruttura.",
    "detail": "https://myendpoint.bluemix.net?key=abcd1234"
    }
	```
	{: screen}

    ```
	cp ~/cloud-monitoring/notifications/email-template.json ~/cloud-monitoring/notifications/s-webhook_1.json
	```
	{: screen}
	
3. [Crea una notifica.](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    Puoi creare una notifica utilizzando un token di autenticazione UAA, un token di autenticazione IAM o una chiave API. L'esempio mostrato è per un token IAM:

    ```
	curl -XPOST -d @s-webhook_1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
    Per verificare che la notifica sia stata creata correttamente, esegui questo comando:

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/webhook_1
	```
	{: screen}
	
4. [Crea una regola](/docs/services/cloud-monitoring/alerts/rules.html#create) nel file `s-rule-1.json` nella directory `~/cloud-monitoring/rules/`. 

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
	
	Immetti quindi il seguente comando per abilitarla:
	
    ```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	Per verificare che la regola sia stata creata correttamente, esegui questo comando:

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}



 
	  
