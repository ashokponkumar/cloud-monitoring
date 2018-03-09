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



# Bereitstellungs- und Überwachungsservice
{: #provision}

Der {{site.data.keyword.monitoringshort}}-Service kann über die {{site.data.keyword.Bluemix}}-Benutzerschnittstelle oder über die Befehlszeile bereitgestellt werden.
{:shortdesc}


## Bereitstellung über die Benutzerschnittstelle
{: #ui}

Führen Sie die folgenden Schritte aus, um eine Instanz des {{site.data.keyword.monitoringshort}}-Service in {{site.data.keyword.Bluemix_notm}} bereitzustellen:

1. Melden Sie sich bei Ihrem {{site.data.keyword.Bluemix_notm}}-Konto an.

    Das {{site.data.keyword.Bluemix_notm}}-Dashboard finden Sie hier: [http://bluemix.net ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](http://bluemix.net){:new_window}.
    
	Nach der Anmeldung mit Ihrer Benutzer-ID und Ihrem Kennwort wird die {{site.data.keyword.Bluemix_notm}}-Benutzerschnittstelle geöffnet.

2. Klicken Sie auf **Katalog**. Die Liste der Services, die in {{site.data.keyword.Bluemix_notm}} zur Verfügung stehen, wird geöffnet.

3. Wählen Sie die Kategorie **DevOps** aus, um die Liste der angezeigten Services zu filtern.

4. Klicken Sie auf die Kachel **Überwachung**.

5. Wählen Sie einen Serviceplan aus. 

    * Für die Erfassung und den Zugriff auf Metriken für bis zu 45 Tage und für die Definition von Alertregeln, einschließlich Regeln mit Platzhaltern, wählen Sie den **Premium**-Plan aus. 
	
	* Standardmäßig ist der **Lite**-Plan festgelegt, der Sie zur Erfassung von Plattformmetriken in dem Bereich, in dem Sie den Service bereitstellen, und zu einer Aufbewahrungsdauer von 15 Tagen für diese Metriken berechtigt. 

    Weitere Informationen zu den Serviceplänen finden Sie in [Servicepläne](/docs/services/cloud-monitoring/monitoring_ov.html#plan).
	
6. Klicken Sie auf **Erstellen**, um den {{site.data.keyword.monitoringshort}}-Service in dem {{site.data.keyword.Bluemix_notm}}-Bereich bereitzustellen, bei dem Sie angemeldet sind.
  
 

## Bereitstellung über die Befehlszeilenschnittstelle
{: #cli}

Führen Sie die folgenden Schritte aus, um eine Instanz des {{site.data.keyword.monitoringshort}}-Service über die Befehlszeile in {{site.data.keyword.Bluemix_notm}} bereitzustellen:

1. [Voraussetzung] Installieren Sie die {{site.data.keyword.Bluemix_notm}}-CLI.

   Weitere Informationen dazu finden Sie unter [Installing the {{site.data.keyword.Bluemix_notm}} CLI](/docs/cli/reference/bluemix_cli/download_cli.html#download_install).
   
   Wenn die CLI installiert ist, fahren Sie mit dem nächsten Schritt fort.
    
2. Melden Sie sich bei einer Region, einer Organisation und einem Bereich in {{site.data.keyword.Bluemix_notm}} an. 

    Weitere Informationen finden Sie unter [Wie melde ich mich bei {{site.data.keyword.Bluemix_notm}} an?](/docs/services/cloud-monitoring/qa/cli_qa.html#login).
	
3. Führen Sie den Befehl `bx cf create-service` aus, um eine Instanz bereitzustellen.

    ```
	bx cf create-service service_name service_plan service_instance_name
	```
	{: codeblock}
	
	Dabei gilt:
	
	* service_name ist **Monitoring**.
	* service_plan ist der Name des Serviceplans. Für die Erfassung und den Zugriff auf Metriken für bis zu 45 Tage und für die Definition von Alertregeln, einschließlich Regeln mit Platzhaltern, wählen Sie den **Premium**-Plan aus. Standardmäßig ist der **Lite**-Plan festgelegt, der Sie zur Erfassung von Plattformmetriken in dem Bereich, in dem Sie den Service bereitstellen, und zu einer Aufbewahrungsdauer von 15 Tagen für diese Metriken berechtigt. Eine Liste der Pläne finden Sie in [{{site.data.keyword.monitoringshort}}-Servicepläne] (/docs/services/cloud-monitoring/monitoring_ov.html#plan).
	* service_instance_name ist der Name, der für die neue Serviceinstanz verwendet werden soll, die Sie erstellen.
	
	Weitere Informationen zum Befehl 'bx cf' finden Sie unter [cf create-service](/docs/cli/reference/cfcommands/index.html#cf_create-service).

	Führen Sie zum Beispiel den folgenden Befehl aus, um eine Instanz des {{site.data.keyword.monitoringshort}}-Service mit einem Premium-Plan zu erstellen:
	
	```
	cf create-service Monitoring premium my_monitoring_svc
	```
	{: codeblock}
	
4. Vergewissern Sie sich, dass der Service erfolgreich erstellt wurde. Führen Sie den folgenden Befehl aus:

    ```	
	cf services
	```
	{: codeblock}
	
	Die Ausgabe der Befehlsausführung sieht wie folgt aus:
	
	```
    Getting services in org MyOrg / space MySpace as xxx@yyy.com...
    OK
    
    name                           service                  plan                   bound apps              last operation
    my_monitoring_svc              Monitoring               premium                                        create succeeded
	```
	{: screen}

	



