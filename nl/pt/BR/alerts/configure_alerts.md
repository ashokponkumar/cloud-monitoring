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


# Configurar alertas
{: #configure_alerts}

É possível configurar um alerta em uma consulta. O alerta pode enviar um e-mail de notificação, chamar um webhook ou criar uma notificação do PagerDuty.
{:shortdesc}


## Enviando um e-mail
{: #send_email}

Para criar um alerta que envia um e-mail, conclua as etapas a seguir:

1. [Criar ou obter um modelo de notificação para enviar e-mails.](/docs/services/cloud-monitoring/alerts/notifications.html#template) 

    Por exemplo, em um sistema Ubuntu, execute os comandos a seguir:
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
    Crie um diretório para armazenar seus modelos de notificação, por exemplo, *notification-templates*.

    ```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	Altere para este diretório:
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{
	
    Crie um modelo de notificação em um diretório local de seu sistema.

    Por exemplo, crie o arquivo **email-template.json** usando o editor de vi: 
	
	```
	{
    "name": "email_template", "type": "Email", "description" : "Send email to manager of department X when....", "detail": "xxx@yyy" }
	```
	{	
	
2. Copie o modelo no diretório `~/cloud-monitoring/notifications/` e modifique os campos de nome, de descrição e de detalhes:

    ```
	{
    "name": "email_xxx",
    "type": "Email",
    "description" : "Send email to manager of department X for an infrastructure problem.", "detalhe": "xxx@yyy.com " }
	```
	{: screen}

    ```
	Cp ~/cloud-monitoring/notifications/email-template.json ~/cloud-monitoring/notifications/s-email-xxx.json
	```
	{: screen}
	
3. [Crie uma notificação.](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    É possível criar uma notificação usando um token de autenticação UAA, um token de autenticação IAM ou uma chave API. O exemplo mostrado é para um token IAM:

    ```
	curl -XPOST -d @s-email-xxx.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
    Para verificar se a notificação foi criada com sucesso, execute o comando a seguir:

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/email_xxx
	```
	{: screen}
	
4. [Criar um arquivo de regras.](/docs/services/cloud-monitoring/alerts/rules.html#create)

    Por exemplo, crie o arquivo de regras `s-rule-1.json` no diretório `~/cloud-monitoring/rules/`. 

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
      "Email_xxx" ]
    }
    ```
	{: screen}
	
	Em seguida, execute o comando a seguir para ativá-la:
	
    ```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	Para verificar se a regra foi criada com sucesso, execute o comando a seguir:

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}
   
   

## Configurando um alerta do PagerDuty
{: #config_alert_pagerduty}

Para criar um alerta que envia uma notificação do PagerDuty, conclua as etapas a seguir:

1. [Crie ou obtenha um modelo de notificação.](/docs/services/cloud-monitoring/alerts/notifications.html#template) 

    Por exemplo, em um sistema Ubuntu, execute os comandos a seguir:
	
	```
	mkdir ~/cloud-monitoring
	```
	{
	
    Crie um diretório para armazenar seus modelos de notificação, por exemplo, *notification-templates*.

    ```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	Altere para este diretório:
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{
	
    Crie um modelo de notificação em um diretório local de seu sistema.

    Por exemplo, crie o arquivo **pagerduty-template.json** usando o editor de vi: 
	
	```
	{
    "name": "pagerduty_template",
    "type": "PagerDuty",
    "description" : "Send notification to PagerDuty when ....",
    "detalhe": "PagerDutyKey"
    }
	```
	{	
	
2. Copie o modelo no diretório `~/cloud-monitoring/notifications/` e modifique os campos de nome, de descrição e de detalhes:

    ```
	{
    "name": "pagerduty_notification_1",
    "type": "PagerDuty",
    "description" : "Send notification to PagerDuty for an infrastructure problem.", 	"detail": "abcd1234" 	}
	```
	{: screen}
	
	As notificações do PagerDuty requerem uma chave API exclusiva no campo *detalhe*. Essa chave API é gerada por um administrador ou um proprietário de conta do PagerDuty.

    ```
	~/cloud-monitoring/notifications/email-template.json cp ~/cloud-monitoring/notifications/s-pagerduty_notification_1.json
	```
	{: screen}
	
3. [Crie uma notificação.](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    É possível criar uma notificação usando um token de autenticação UAA, um token de autenticação IAM ou uma chave API. O exemplo mostrado é para um token IAM:

    ```
	curl -XPOST -d @s-pagerduty_notification_1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
    Para verificar se a notificação foi criada com sucesso, execute o comando a seguir:

    ```
	curl -XGET --header "X-Auth-User-Token:uaa ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/pagerduty_notification_1
	```
	{: screen}
	
4. Arquivo [Crie uma regra](/docs/services/cloud-monitoring/alerts/rules.html#create) `s-rule-1.json` no diretório `~/cloud-monitoring/rules/`. 

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
      "Pagerduty_notification_1" ]
    }
    ```
	{: screen}
	
	Em seguida, execute o comando a seguir para ativá-la:
	
    ```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	Para verificar se a regra foi criada com sucesso, execute o comando a seguir:

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}


 




## Configurando um webhook
{: #config_webhook}

Para criar um alerta que chama um webhook, conclua as etapas a seguir:

1. [Crie ou obtenha um modelo de notificação.](/docs/services/cloud-monitoring/alerts/notifications.html#template) 

    Por exemplo, em um sistema Ubuntu, execute os comandos a seguir:
	
	```
	mkdir ~/cloud-monitoring
	```
	{
	
    Crie um diretório para armazenar seus modelos de notificação, por exemplo, *notification-templates*.

    ```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	Altere para este diretório:
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{
	
    Crie um modelo de notificação em um diretório local de seu sistema.

    Por exemplo, crie o arquivo **webhook-template.json** usando o editor de vi: 
	
	```
	{
    "name": "webhook_template",
    "type": "Webhook",
    "description" : "Fire this webhook when ....", 	"detail": "https://myendpoint.bluemix.net?key=abcd1234" 	}
	```
	{: codeblock}	
	
	Para chamar um webhook, o campo *detalhe* deve ser configurado para a URL na qual o POST deve ser feito. Configure um webhook para terminais https e inclua um parâmetro.
	
2. Copie o modelo no diretório `~/cloud-monitoring/notifications/` e modifique os campos de nome, de descrição e de detalhes:

    ```
	{
    "name": "webhook_1",
    "type": "Webhook",
    "description" : "Invoke webhook for an infrastructure problem.", 	"detail": "https://myendpoint.bluemix.net?key=abcd1234" 	}
	```
	{: screen}

    ```
	~/cloud-monitoring/notifications/email-template.json cp ~/cloud-monitoring/notifications/s-webhook_1.json
	```
	{: screen}
	
3. [Crie uma notificação.](/docs/services/cloud-monitoring/alerts/notifications.html#create)

    É possível criar uma notificação usando um token de autenticação UAA, um token de autenticação IAM ou uma chave API. O exemplo mostrado é para um token IAM:

    ```
	curl -XPOST -d @s-webhook_1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: screen}
	
    Para verificar se a notificação foi criada com sucesso, execute o comando a seguir:

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/webhook_1
	```
	{: screen}
	
4. Arquivo [Crie uma regra](/docs/services/cloud-monitoring/alerts/rules.html#create) `s-rule-1.json` no diretório `~/cloud-monitoring/rules/`. 

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
      "Webhook_1" ]
    }
    ```
	{: screen}
	
	Em seguida, execute o comando a seguir para ativá-la:
	
    ```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}
	
	Para verificar se a regra foi criada com sucesso, execute o comando a seguir:

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}



 
	  
