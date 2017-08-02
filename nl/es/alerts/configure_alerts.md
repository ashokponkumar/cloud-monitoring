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


# Configuración de alertas
{: #configure_alerts}

Puede configurar una alerta en una consulta. La alerta puede enviar un correo electrónico de notificación, invocar un webhook o crear una notificación de PagerDuty.
{:shortdesc}


## Enviar un correo electrónico
{: #send_email}

Para crear una alerta que envíe un correo electrónico, siga estos pasos:

1. [Cree u obtenga una plantilla de notificación para enviar mensajes de correo electrónico.](/docs/services/cloud-monitoring/alerts/notifications.html#template) 

    Por ejemplo en un sistema Ubuntu, ejecute los mandatos siguientes:
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
    Cree un directorio para almacenar sus plantillas de notificación, por ejemplo *notification-templates*.

    ```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	Cambie a este directorio:
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
    Crear una plantilla de notificación en un directorio local del sistema.

    Por ejemplo, cree el archivo **email-template.json** con el editor vi: 
	
	```
	{
    "name": "email_template",
    "type": "Email",
    "description" : "Send email to manager of department X when ....",
    "detail": "xxx@yyy"
    }
	```
	{: codeblock}	
	
2. Copie la plantilla en el directorio `~/cloud-monitoring/notifications/` y modifique los campos de nombre, descripción y detalle:

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
	
3. [Cree una notificación.](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    Puede crear una notificación utilizando una señal de autenticación UAA, una señal de autenticación IAM o una clave de API. El ejemplo que se muestra corresponde a una señal IAM:

    ```
	curl -XPOST -d @s-email-xxx.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
    Para verificar que la notificación se ha creado correctamente, ejecute el siguiente mandato:

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/email_xxx
	```
	{: screen}
	
4. [Cree un archivo de reglas.](/docs/services/cloud-monitoring/alerts/rules.html#create)

    Por ejemplo, cree el archivo de reglas `s-rule-1.json` en el directorio `~/cloud-monitoring/rules/`. 

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
	
	A continuación, ejecute el mandato siguiente para habilitarlo:
	
    ```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	Para verificar que la regla se ha creado correctamente, ejecute el siguiente mandato:

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}
   
   

## Configuración de una alerta de PagerDuty
{: #config_alert_pagerduty}

Para crear una alerta que envíe una notificación de PagerDuty, siga estos pasos:

1. [Cree u obtenga una plantilla de notificación.](/docs/services/cloud-monitoring/alerts/notifications.html#template) 

    Por ejemplo en un sistema Ubuntu, ejecute los mandatos siguientes:
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
    Cree un directorio para almacenar sus plantillas de notificación, por ejemplo *notification-templates*.

    ```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	Cambie a este directorio:
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
    Crear una plantilla de notificación en un directorio local del sistema.

    Por ejemplo, cree el archivo **pagerduty-template.json** con el editor vi: 
	
	```
	{
    "name": "pagerduty_template",
    "type": "PagerDuty",
    "description" : "Send notification to PagerDuty when ....",
    "detail": "PagerDutyKey"
    }
	```
	{: codeblock}	
	
2. Copie la plantilla en el directorio `~/cloud-monitoring/notifications/` y modifique los campos de nombre, descripción y detalle:

    ```
	{
    "name": "pagerduty_notification_1",
    "type": "PagerDuty",
    "description" : "Send notification to PagerDuty for an infrastructure problem.",
    "detail": "abcd1234"
    }
	```
	{: screen}
	
	Las notificaciones de PagerDuty necesitan una clave de API exclusiva en el campo *detail*. Esta clave de API la genera el administrador o el propietario de la cuenta de PagerDuty.

    ```
	cp ~/cloud-monitoring/notifications/email-template.json ~/cloud-monitoring/notifications/s-pagerduty_notification_1.json
	```
	{: screen}
	
3. [Cree una notificación.](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    Puede crear una notificación utilizando una señal de autenticación UAA, una señal de autenticación IAM o una clave de API. El ejemplo que se muestra corresponde a una señal IAM:

    ```
	curl -XPOST -d @s-pagerduty_notification_1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
    Para verificar que la notificación se ha creado correctamente, ejecute el siguiente mandato:

    ```
	curl -XGET --header "X-Auth-User-Token:uaa ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/pagerduty_notification_1
	```
	{: screen}
	
4. [Cree una regla](/docs/services/cloud-monitoring/alerts/rules.html#create) archivo `s-rule-1.json` en el directorio `~/cloud-monitoring/rules/`. 

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
	
	A continuación, ejecute el mandato siguiente para habilitarlo:
	
    ```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	Para verificar que la regla se ha creado correctamente, ejecute el siguiente mandato:

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}


 




## Configuración de un webhook
{: #config_webhook}

Para crear una alerta que invoque un webhook, siga estos pasos:

1. [Cree u obtenga una plantilla de notificación.](/docs/services/cloud-monitoring/alerts/notifications.html#template) 

    Por ejemplo en un sistema Ubuntu, ejecute los mandatos siguientes:
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
    Cree un directorio para almacenar sus plantillas de notificación, por ejemplo *notification-templates*.

    ```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	Cambie a este directorio:
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
    Crear una plantilla de notificación en un directorio local del sistema.

    Por ejemplo, cree el archivo **webhook-template.json** con el editor vi: 
	
	```
	{
    "name": "webhook_template",
    "type": "Webhook",
    "description" : "Fire this webhook when ....",
    "detail": "https://myendpoint.bluemix.net?key=abcd1234"
    }
	```
	{: codeblock}	
	
	Para poder invocar un webhook, el campo *detail* debe estar establecido en el URL en el que se debe realizar la acción POST. Configure un webhook para puntos finales https y añada un parámetro.
	
2. Copie la plantilla en el directorio `~/cloud-monitoring/notifications/` y modifique los campos de nombre, descripción y detalle:

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
	
3. [Cree una notificación.](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    Puede crear una notificación utilizando una señal de autenticación UAA, una señal de autenticación IAM o una clave de API. El ejemplo que se muestra corresponde a una señal IAM:

    ```
	curl -XPOST -d @s-webhook_1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
    Para verificar que la notificación se ha creado correctamente, ejecute el siguiente mandato:

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/webhook_1
	```
	{: screen}
	
4. [Cree una regla](/docs/services/cloud-monitoring/alerts/rules.html#create) archivo `s-rule-1.json` en el directorio `~/cloud-monitoring/rules/`. 

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
	
	A continuación, ejecute el mandato siguiente para habilitarlo:
	
    ```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	Para verificar que la regla se ha creado correctamente, ejecute el siguiente mandato:

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}



 
	  
