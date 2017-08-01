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


# Configuration d'alertes
{: #configure_alerts}

Vous pouvez configurer une alerte pour une requête. Celle-ci peut envoyer une notification par courrier électronique, appeler un webhook ou créer une notification PagerDuty.
{:shortdesc}


## Envoi d'un courrier électronique
{: #send_email}

Pour créer une alerte qui envoie un courrier électronique, procédez comme suit :

1. [Créez ou procurez-vous un modèle de notification pour l'envoi de courriers électroniques.](/docs/services/cloud-monitoring/alerts/notifications.html#template) 

    Par exemple, sur un système Ubuntu, exécutez les commandes suivantes :
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
    Créez un répertoire pour stocker vos modèles de notification, par exemple *notification-templates*.

    ```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	Placez-vous dans ce répertoire :
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
    Créez un modèle de notification dans un répertoire local sur votre système.

    Par exemple, créez le fichier **email-template.json** à l'aide de l'éditeur vi : 
	
	```
	{
    "name": "email_template",
    "type": "Email",
    "description" : "Send email to manager of department X when ....",
    "detail": "xxx@yyy"
    }
	```
	{: codeblock}	
	
2. Copiez le modèle dans le répertoire `~/cloud-monitoring/notifications/` et modifiez les zones name, description et detail :

    ```
	{
    "name": "email_xxx",
    "type": "Email",
    "description" : "Send email to manager of department X for an infrastructure problem.",
	"detail": "xxx@yyy.com"}
	```
	{: screen}

    ```
	cp ~/cloud-monitoring/notifications/email-template.json ~/cloud-monitoring/notifications/s-email-xxx.json
	```
	{: screen}
	
3. [Créez une notification.](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    Vous pouvez créer une notification à l'aide d'un jeton d'authentification UAA, d'un jeton d'authentification IAM ou d'une clé d'API. L'exemple présenté ici concerne un jeton IAM :

    ```
	curl -XPOST -d @s-email-xxx.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
    Pour vérifier que la notification a été créée, exécutez la commande suivante :

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/email_xxx
	```
	{: screen}
	
4. [Créez un fichier de règle.](/docs/services/cloud-monitoring/alerts/rules.html#create)

    Par exemple, créez le fichier de règle `s-rule-1.json` dans le répertoire `~/cloud-monitoring/rules/`. 

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
	
	Ensuite, exécutez la commande suivante pour l'activer :
	
    ```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	Pour vérifier que la règle a été créée, exécutez la commande suivante :

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}
   
   

## Configuration d'une alerte PagerDuty
{: #config_alert_pagerduty}

Pour créer une alerte qui envoie une notification PagerDuty, procédez comme suit :

1. [Créez ou procurez-vous un modèle de notification.](/docs/services/cloud-monitoring/alerts/notifications.html#template) 

    Par exemple, sur un système Ubuntu, exécutez les commandes suivantes :
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
    Créez un répertoire pour stocker vos modèles de notification, par exemple *notification-templates*.

    ```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	Placez-vous dans ce répertoire :
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
    Créez un modèle de notification dans un répertoire local sur votre système.

    Par exemple, créez le fichier **pagerduty-template.json** à l'aide de l'éditeur vi : 
	
	```
	{
    "name": "pagerduty_template",
    "type": "PagerDuty",
    "description" : "Send notification to PagerDuty when ....",
    "detail": "PagerDutyKey"
    }
	```
	{: codeblock}	
	
2. Copiez le modèle dans le répertoire `~/cloud-monitoring/notifications/` et modifiez les zones name, description et detail :

    ```
	{
    "name": "pagerduty_notification_1",
    "type": "PagerDuty",
    "description" : "Send notification to PagerDuty for an infrastructure problem.",
	"detail": "abcd1234"
	}
	```
	{: screen}
	
	Les notifications PagerDuty requièrent une clé d'API unique dans la zone *detail*. Celle-ci est générée par un administrateur ou un propriétaire de compte PagerDuty.

    ```
	cp ~/cloud-monitoring/notifications/email-template.json ~/cloud-monitoring/notifications/s-pagerduty_notification_1.json
	```
	{: screen}
	
3. [Créez une notification.](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    Vous pouvez créer une notification à l'aide d'un jeton d'authentification UAA, d'un jeton d'authentification IAM ou d'une clé d'API. L'exemple présenté ici concerne un jeton IAM :

    ```
	curl -XPOST -d @s-pagerduty_notification_1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
    Pour vérifier que la notification a été créée, exécutez la commande suivante :

    ```
	curl -XGET --header "X-Auth-User-Token:uaa ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/pagerduty_notification_1
	```
	{: screen}
	
4. [Créez un fichier de règle](/docs/services/cloud-monitoring/alerts/rules.html#create) `s-rule-1.json` dans le répertoire `~/cloud-monitoring/rules/`. 

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
	
	Ensuite, exécutez la commande suivante pour l'activer :
	
    ```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	Pour vérifier que la règle a été créée, exécutez la commande suivante :

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}


 




## Configuration d'un webhook
{: #config_webhook}

Pour créer une alerte qui appelle un webhook, procédez comme suit :

1. [Créez ou procurez-vous un modèle de notification.](/docs/services/cloud-monitoring/alerts/notifications.html#template) 

    Par exemple, sur un système Ubuntu, exécutez les commandes suivantes :
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
    Créez un répertoire pour stocker vos modèles de notification, par exemple *notification-templates*.

    ```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	Placez-vous dans ce répertoire :
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
    Créez un modèle de notification dans un répertoire local sur votre système.

    Par exemple, créez le fichier **webhook-template.json** à l'aide de l'éditeur vi : 
	
	```
	{
    "name": "webhook_template",
    "type": "Webhook",
    "description" : "Fire this webhook when ....",
	"detail": "https://myendpoint.bluemix.net?key=abcd1234"
	}
	```
	{: codeblock}	
	
	Pour qu'un webhook soit appelé, la zone *detail* doit comporter l'adresse URL à laquelle la demande POST doit être émise. Configurez un webhook pour les noeuds finaux https et ajoutez un paramètre.
	
2. Copiez le modèle dans le répertoire `~/cloud-monitoring/notifications/` et modifiez les zones name, description et detail :

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
	
3. [Créez une notification.](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    Vous pouvez créer une notification à l'aide d'un jeton d'authentification UAA, d'un jeton d'authentification IAM ou d'une clé d'API. L'exemple présenté ici concerne un jeton IAM :

    ```
	curl -XPOST -d @s-webhook_1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
    Pour vérifier que la notification a été créée, exécutez la commande suivante :

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/webhook_1
	```
	{: screen}
	
4. [Créez un fichier de règle](/docs/services/cloud-monitoring/alerts/rules.html#create) `s-rule-1.json` dans le répertoire `~/cloud-monitoring/rules/`. 

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
	
	Ensuite, exécutez la commande suivante pour l'activer :
	
    ```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	Pour vérifier que la règle a été créée, exécutez la commande suivante :

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}



 
	  
