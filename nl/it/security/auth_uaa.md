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


# Autenticazione mediante il modello Bluemix UAA
{: #auth_uaa}

Utilizza il modello {{site.data.keyword.Bluemix}} UAA per ottenere un token di autenticazione che puoi utilizzare per accedere alle metriche memorizzate nel servizio {{site.data.keyword.monitoringshort}} per uno spazio in {{site.data.keyword.Bluemix_notm}}. Puoi ottenere il token di autenticazione utilizzando la CLI {{site.data.keyword.Bluemix_notm}} o l'API REST `Login`.
{:shortdesc}

Per accedere alle metriche utilizzando un token di autenticazione UAA, hai bisogno delle seguenti informazioni:

* Un token UAA per accedere al servizio {{site.data.keyword.monitoringshort}} utilizzando le API RESTful.
* Il GUID dello spazio.

		
## Ottenimento del token UAA utilizzando la CLI Bluemix
{: #uaa_cli}


Per ottenere il token di autorizzazione, completa la seguente procedura:

1. Installa la CLI {{site.data.keyword.Bluemix_notm}}.

   Per ulteriori informazioni, vedi [Installazione della CLI {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa).
   
   Se la CLI è installata, vai al passo successivo.
    
2. Accedi a una regione, un'organizzazione e uno spazio {{site.data.keyword.Bluemix_notm}}. Esegui il comando:

    ```
    cf login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    Segui le istruzioni. Immetti le tue credenziali {{site.data.keyword.Bluemix_notm}}, seleziona un'organizzazione e uno spazio.
	
3. Esegui il comando `cf oauth-token` per ottenere il token {{site.data.keyword.Bluemix_notm}} UAA.

    ```
	cf oauth-token
	```
	{: codeblock}
	
	L'output restituisce il token UAA che devi utilizzare per autenticare il tuo ID utente in tale spazio e organizzazione.

4. Ottieni il GUID per lo spazio dell'organizzazione per cui hai ottenuto un token di autenticazione.

   Per ulteriori informazioni, vedi [Come ottengo il GUID di uno spazio](/docs/services/cloud-monitoring/qa/cli_qa.html#space_guid).
	
5. Esporta le seguenti variabili: TOKEN e SPACE.

    * *TOKEN* è il token oauth che hai ottenuto nel passo precedente escludendo Bearer.
	
	* *SPACE* è il GUID dello spazio che hai ottenuto nel passo precedente.
		
	Ad esempio,
	
	```
	export TOKEN="eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8"
	export SPACE="667fb895-abcd-defg-aaaa-cf4587341095"
	```
	{: screen}
	
6. Immetti il seguente comando per ottenere il token UAA per lavorare con il servizio {{site.data.keyword.monitoringshort}}:
 
    ```
	curl -k -X GET  --header "X-Auth-Token: ${TOKEN}"  --header "X-Auth-Project-Id: ${SPACE}"  METRICS_ENDPOINT
    ```
    {: codeblock}	
	
	dove
	* SPACE è il GUID dello spazio in cui è in esecuzione il servizio.
	* TOKEN è il token {{site.data.keyword.Bluemix_notm}} UAA che hai ottenuto nel passo precedente senza il prefisso bearer.
	* METRICS_ENDPOINT è l'endpoint di metriche (https://metrics.ng.bluemix.net/token) per la regione {{site.data.keyword.Bluemix_notm}} in cui sono disponibili l'organizzazione e lo spazio.
	
## Ottenimento del token UAA utilizzando l'API
{: #uaa_api}

Per ottenere il token di autorizzazione, esegui il seguente comando cURL:

```
curl -XPOST -d 'user=USERNAME&passwd=PASSWORD&space=SPACE_NAME&organization=ORG_NAME' METRICS_ENDPOINT
```
{: codeblock}

dove

* USERNAME è un ID {{site.data.keyword.Bluemix_notm}} per il quale vuoi ottenere il token di autenticazione per lavorare con il servizio {{site.data.keyword.monitoringshort}}.
* PASSWORD è la password dell'ID utente utilizzato per accedere a {{site.data.keyword.Bluemix_notm}}.
* SPACE_NAME è il nome dello spazio in cui vengono raccolte le metriche.
* ORG_NAME è il nome dell'organizzazione in {{site.data.keyword.Bluemix_notm}} in cui si trova lo spazio.
* METRICS_ENDPOINT è l'endpoint di metriche (https://metrics.ng.bluemix.net/login) per la regione {{site.data.keyword.Bluemix_notm}} in cui sono disponibili l'organizzazione e lo spazio.
	
L'output è un documento JSON che contiene il token UAA. Ottieni il valore per il token dal campo **access-token**.

Un documento JSON di esempio è simile al seguente:

```
{
    "access_token": "eyJhbGc...",
    "logging_token": "xxxxxxxxxxxx",
    "org_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "space_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```
{: screen}

Il valore di *logging_token* corrisponde al token UAA di cui hai bisogno per lavorare con il servizio {{site.data.keyword.monitoringshort}}.
 
**Nota:**

* Se non utilizzi cURL, devi impostare l'intestazione **Content-Type: application/x-www-form-urlencoded**.
* Se ricevi il codice di errore *BXNMS0122E: User credentials are invalid*, verifica di utilizzare un ID {{site.data.keyword.IBM_notm}} valido.


