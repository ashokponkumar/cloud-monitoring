---

copyright:
  years: 2017

lastupdated: "2017-07-12"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Authentifizierung mit dem Bluemix UAA-Modell
{: #auth_uaa}

Verwenden Sie das {{site.data.keyword.Bluemix}} UAA-Modell, um ein Authentifizierungstoken abzurufen, das Sie für den Zugriff auf Metriken verwenden können, die im {{site.data.keyword.monitoringshort}}-Service für einen Bereich in {{site.data.keyword.Bluemix_notm}} gespeichert sind. Sie können das Authentifizierungstoken mithilfe der {{site.data.keyword.Bluemix_notm}} CLI oder durch ein `Login` bei der REST-API abrufen.
{:shortdesc}

Um mithilfe des UAA-Authentifizierungstokens auf Metriken zuzugreifen, benötigen Sie die folgenden Informationen:

* Ein UAA-Token für den Zugriff auf den {{site.data.keyword.monitoringshort}}-Service mithilfe der RESTful-APIs.
* Die GUID des Bereichs.

		
## UAA-Token über die Bluemix-Befehlszeilenschnittstelle abrufen
{: #uaa_cli}


Führen Sie die folgenden Schritte aus, um das Berechtigungstoken abzurufen:

1. Installieren Sie die {{site.data.keyword.Bluemix_notm}}-Befehlzeilenschnittstelle (CLI). 

   Weitere Informationen dazu finden Sie unter [Installing the {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa).
   
   Wenn die CLI installiert ist, fahren Sie mit dem nächsten Schritt fort.
    
2. Melden Sie sich bei einer Region, einer Organisation und einem Bereich von {{site.data.keyword.Bluemix_notm}} an. Führen Sie den folgenden Befehl aus:

    ```
    cf login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    Befolgen Sie die Anweisungen. Geben Sie Ihre {{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise ein, wählen Sie eine Organisation und einen Bereich aus.
	
3. Führen Sie den Befehl `cf oauth-token` aus, um das {{site.data.keyword.Bluemix_notm}} UAA-Token abzurufen.

    ```
	cf oauth-token
	```
	{: codeblock}
	
	Die Ausgabe gibt das UAA-Token zurück, das Sie für die Authentifizierung Ihrer Benutzer-ID in dem betreffenden Bereich und der betreffenden Organisation verwenden müssen.

4. Rufen Sie die GUID für den Bereich der Organisation ab, für die Sie ein Authentifizierungstoken erhalten haben.

   Weitere Informationen finden Sie in [Wie erhalte ich die GUID eines Bereichs?](/docs/services/cloud-monitoring/qa/cli_qa.html#space_guid).
	
5. Exportieren Sie die Variablen TOKEN und SPACE.

    * *TOKEN* ist das OAuth-Token, das Sie im vorherigen Schritt ohne Bearer abgerufen haben.
	
	* *SPACE* ist die GUID des Bereichs, die Sie im vorherigen Schritt abgerufen haben.
		
	Beispiel:
	
	```
	export TOKEN="eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8"
	export SPACE="667fb895-abcd-defg-aaaa-cf4587341095"
	```
	{: screen}
	
6. Führen Sie den folgenden Befehl aus, um das UAA-Token abzurufen, das für die Arbeit mit dem {{site.data.keyword.monitoringshort}}-Service erforderlich ist:
 
    ```
	curl -k -X GET  --header "X-Auth-Token: ${TOKEN}"  --header "X-Auth-Project-Id: ${SPACE}"  METRICS_ENDPOINT
    ```
    {: codeblock}	
	
	dabei gilt:
	* SPACE ist die GUID des Bereichs, in dem der Service ausgeführt wird.
	* TOKEN ist das {{site.data.keyword.Bluemix_notm}} UAA-Token, das Sie im vorherigen Schritt ohne das Trägerpräfix erhalten haben.
	* METRICS_ENDPOINT ist der Metrikendpunkt (https://metrics.ng.bluemix.net/token) für die {{site.data.keyword.Bluemix_notm}}-Region, in der die Organisation und der Bereich verfügbar sind.

	
## UAA-Token über die API abrufen
{: #uaa_api}

Führen Sie den folgenden cURL-Befehl aus, um das Berechtigungstoken abzurufen:

```
curl -XPOST -d 'user=USERNAME&passwd=PASSWORD&space=SPACE_NAME&organization=ORG_NAME' METRICS_ENDPOINT
```
{: codeblock}

dabei gilt

* USERNAME ist eine {{site.data.keyword.Bluemix_notm}}-ID für die Sie das Authentifizierungstoken abrufen möchten, um mit dem {{site.data.keyword.monitoringshort}}-Service zu arbeiten.
* PASSWORD ist das Kennwort der Benutzer-ID, die für die Anmeldung bei {{site.data.keyword.Bluemix_notm}} verwendet wurde.
* SPACE_NAME ist der Name des Bereichs, in dem Metriken erfasst werden.
* ORG_NAME ist der Name der Organisation in {{site.data.keyword.Bluemix_notm}}, in der der Bereich gehostet ist.
* METRICS_ENDPOINT ist der Metrikendpunkt (https://metrics.ng.bluemix.net/login) für die {{site.data.keyword.Bluemix_notm}}-Region, in der die Organisation und der Bereich verfügbar sind.

Die Ausgabe ist ein JSON-Dokument, das das UAA-Token enthält. Den Wert für das Token können Sie dem Feld **access-token** entnehmen.

Ein JSON-Dokument sieht wie im folgenden Beispiel aus:

```
{
    "access_token": "eyJhbGc...",
    "logging_token": "xxxxxxxxxxxx",
    "org_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "space_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```
{: screen}

Der Wert für *logging_token* entspricht dem UAA-Token, das Sie für die Arbeit mit dem {{site.data.keyword.monitoringshort}}-Service benötigen.
 
**Hinweis:**

* Wenn Sie keinen cURL-Befehl verwenden, müssen Sie für den Header **Content-Type: application/x-www-form-urlencoded** festlegen.
* Wenn Sie den Fehlercode *BXNMS0122E: User credentials are invalid* (Benutzerberechtigungsnachweise sind ungültig.) erhalten, überprüfen Sie, dass Sie eine gültige {{site.data.keyword.IBM_notm}}-ID verwenden.


