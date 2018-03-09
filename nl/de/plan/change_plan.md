---

copyright:
  years: 2017, 2018

lastupdated: "2018-02-01"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Plan ändern
{: #change_plan}

Sie können Ihren {{site.data.keyword.monitoringshort}}-Serviceplan über das {{site.data.keyword.monitoringlong_notm}}-Dashboard oder durch Ausführen des Befehls `cf update-service` ändern. Sie können Ihren Plan jederzeit aktualisieren oder reduzieren.
{:shortdesc}

## Serviceplan über die Benutzerschnittstelle ändern
{: #change_plan_ui}

Um Ihren Serviceplan im {{site.data.keyword.monitoringlong_notm}}-Dashboard zu ändern, führen Sie die folgenden Schritte aus:

1. Melden Sie sich bei {{site.data.keyword.Bluemix_notm}} an und klicken Sie anschließend im Dashboard auf den {{site.data.keyword.monitoringshort}}-Service. 

    Das Dashboard für den {{site.data.keyword.monitoringshort}}-Service wird geöffnet.
    
2. Wählen Sie die Registerkarte **Plan** aus.

    Informationen zu Ihrem aktuellen Plan werden angezeigt.
	
3. Wählen Sie entweder einen neuen Plan zur Aktualisierung aus oder reduzieren Sie Ihren Plan. 

4. Klicken Sie auf **Speichern**.



## Serviceplan über die Befehlszeilenschnittstelle (CLI) ändern
{: #change_plan_cli}

Führen Sie die folgenden Schritte aus, um Ihren Serviceplan in {{site.data.keyword.Bluemix_notm}} über die Befehlszeilenschnittstelle (CLI) zu ändern:

1. Melden Sie sich bei einer Region, einer Organisation und einem Bereich in {{site.data.keyword.Bluemix_notm}} an. 

    Weitere Informationen finden Sie unter [Wie melde ich mich bei {{site.data.keyword.Bluemix_notm}} an?](/docs/services/cloud-monitoring/qa/cli_qa.html#login).
	
2. Führen Sie den Befehl `bx cf services` aus, um Ihren aktuellen Plan zu überprüfen und um den {{site.data.keyword.loganalysisshort}}-Servicenamen aus der Liste der Services abzurufen, die im Bereich verfügbar sind. 

    Der Wert für das Feld **name** ist der Name, den Sie zur Änderung des Plans verwenden müssen. 

    Beispiel:
	
	```
	$ bx cf services
	Getting services in org MyOrg / space dev as xxx@yyy.com...
	OK
	name            service      plan   bound apps   last operation
	Monitoring-0c   Monitoring   premium             create succeeded
    ```
	{: screen}
    
3. Aktualisieren Sie Ihren Plan oder reduzieren Sie ihn. Führen Sie den Befehl `bx cf update-service` aus.
    
	```
	bx cf update-service service_name [-p new_plan]
	```
	{: codeblock}
	
	dabei ist 
	
	* *service_name* der Name Ihres Service. Sie können den Befehl `bx cf services` ausführen, um den Wert zu erhalten.
	* *new_plan* der Name des Plans.
	
	In der folgenden Tabelle werden die unterschiedlichen Pläne und die zugehörigen unterstützten Werte aufgelistet:
	
	<table>
	  <caption>Tabelle 1.  Liste der Pläne.</caption>
	  <tr>
	    <th>Plan</th>
		<th>Features</th>
	    <th>Name</th>
	  </tr>
	  <tr>
	    <td>Lite</td>
	    <td>Kostenlose Überwachung</td>
		<td>Lite</td>
	  </tr>
	  <tr>
	    <td>Premium</td>
	    <td>Premium Überwachung</td>
		<td>Premium</td>
	  </tr>
	</table>
	
	Um zum Beispiel Ihren Plan auf den *Lite*-Plan zu reduzieren, führen Sie den folgenden Befehl aus:
	
	```
	bx cf update-service "Monitoring-0c" -p lite
    Updating service instance Monitoring-0c as xxx@yyy.com...
    OK
	```
	{: screen}

4. Überprüfen Sie, dass der neue Plan geändert wurde. Führen Sie den Befehl `bx cf services` aus.

    ```
	bx cf services
    Getting services in org MyOrg / space dev as xxx@yyy.com...
    OK

    name              service       plan   bound apps   last operation
    Monitoring-0c     Monitoring    lite                create succeeded
	```
	{: screen}






