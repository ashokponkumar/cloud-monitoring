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


# Invio di dati al servizio di monitoraggio
{: #send_data_api}

Puoi inviare metriche al servizio {{site.data.keyword.monitoringshort}} utilizzando l'API Metriche. Puoi inviare metriche a un spazio {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}


Per i contenitori {{site.data.keyword.Bluemix_notm}} Docker, le metriche del sistema di base vengono raccolte automaticamente. Per le applicazioni Cloud Foundry e per le applicazioni in esecuzione in una macchina virtuale (VM), le metriche devono essere inviate direttamente dall'applicazione utilizzando [l'API Metriche](https://console.bluemix.net/apidocs/927-ibm-cloud-monitoring-rest-api?&language=node#introduction){: new_window}. 



## Invio delle metriche a uno spazio utilizzando UAA
{: #uaa}

Per inviare le metriche a uno spazio {{site.data.keyword.Bluemix_notm}}, completa la seguente procedura:

1. Accedi a una regione, un'organizzazione e uno spazio {{site.data.keyword.Bluemix_notm}}. Esegui il comando:

    Ad esempio, per accedere alla regione Stati Uniti Sud, esegui il seguente comando:
	
	```
    cf login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Segui le istruzioni. Immetti le tue credenziali {{site.data.keyword.Bluemix_notm}}, seleziona un'organizzazione e uno spazio.

2. Ottieni il token di autenticazione UAA.

    Per ulteriori informazioni sull'autenticazione UAA, vedi [Ottenimento del token UAA utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) o [Ottenimento del token UAA utilizzando l'API REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

	Ad esempio, per utilizzare il token UAA, esegui questo comando:

    ```
	cf oauth-token
	```
	{: codeblock}
	
	Il risultato di questo comando è il seguente:
	
	```
	bearer eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8
	```
	{: screen}
	
	Quindi, esporta la variabile *Token*. Ad esempio:
	
	```
	export Token="eyJhbGciOiJI....cGlxryWS8"
	```
	{: screen}
	
	**Nota:** il token esclude *Bearer*.
		
3. Ottieni il GUID dello spazio. Il GUID deve essere preceduto da *s-* per identificare uno spazio.

    Immetti il seguente comando:
	
	```
	cf space SpaceName --guid
	```
	{: codeblock}
	
	Dove *SpaceName* è il nome dello spazio in cui intendi definire una notifica.
	
	Ad esempio, per ottenere il GUID di uno spazio con nome *dev*, immetti il seguente comando:
	
	```
	cf space dev --guid
	```
	{: screen}
	
	Il risultato di questo comando è il seguente:
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	Quindi, esporta la variabile *Space*. Ad esempio:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. Immetti il seguente comando cURL per inviare le metriche:

    ```
	curl -XPOST -d @Metric_File --header "X-Auth-User-Token:uaa ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics
	```
	{: codeblock}
	
	dove
	
	* Metrics_File rappresenta il file JSON che include la definizione delle metriche inviate al servizio {{site.data.keyword.monitoringshort}}.
	
	* *X-Auth-User-Token* è un parametro che passa il token UAA {{site.data.keyword.Bluemix_notm}}.
	
	* *Auth_Type* è il prefisso che definisce il tipo di token. *uaa* identifica che il token specificato è un token generato da UAA.
	
	* *X-Auth-Scope-Id* è un parametro che passa il GUID dello spazio. Il GUID deve essere preceduto da *s-* per identificare uno spazio.
	
	* Token rappresenta il token UAA.
	
	* Space rappresenta il GUID dello spazio. 
	
	Ad esempio, puoi immettere il seguente comando per inviare 2 metriche, `myhost.cpu.idle` e `myapp.login.attempts`, al servizio {{site.data.keyword.monitoringshort}}:
	
	```
	curl -XPOST @metric.json --header "X-Auth-User-Token:uaa ${Token}" https://metrics.ng.bluemix.net/v1/metrics
	```
	{: screen}
	
	Il file di esempio *metric.json* è definito come segue:

    ```
    [
      {
        "name" : "myhost.cpu.idle",
        "value" : 22.37
      },
      {
        "name": "myapp.login.retries",
        "value": 4
      }
    ]
	```
	{: screen}

 
## Invio di metriche a uno spazio utilizzando IAM o una chiave API
{: #iam}

Per inviare le metriche a uno spazio {{site.data.keyword.Bluemix_notm}}, completa la seguente procedura:

1. Accedi a una regione, un'organizzazione e uno spazio {{site.data.keyword.Bluemix_notm}}. Esegui il comando:

    Ad esempio, per accedere alla regione Stati Uniti Sud, esegui il seguente comando:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Segui le istruzioni. Immetti le tue credenziali {{site.data.keyword.Bluemix_notm}}, seleziona un'organizzazione e uno spazio.

2. Ottieni il token di autenticazione o la chiave API.

    Per ulteriori informazioni sull'autenticazione IAM, vedi [Ottenimento del token IAM utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_cli) o [Generazione di una chiave API IAM utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).

	Ad esempio, per utilizzare il token IAM, esegui questo comando:

    ```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	Il risultato di questo comando è il seguente:
	
	```
	IAM token:  Bearer djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ
    UAA token:  Bearer eyJhbGciOiJIUz..Ky6vagp3k_QcIcKJ-td83qXhO5Uze43KcplG6PzcGs8
	```
	{: screen}
	
	Quindi, esporta la variabile *Token*. Ad esempio:
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Nota:** il token esclude *Bearer*.
		
3. Ottieni il GUID dello spazio.

    Per ottenere il GUID dello spazio, vedi [Come ottengo il GUID di uno spazio](/docs/service/cloud-monitoring/qa/cli_qa.html#space_guid). Il GUID deve essere preceduto da *s-* per identificare uno spazio.
    
    Ad esempio, immetti il seguente comando:
	
	```
	bx iam space NAME --guid
	```
	{: codeblock}
	
	Dove *SpaceName* è il nome dello spazio in cui intendi definire una notifica.
	
	Ad esempio, per ottenere il GUID di uno spazio con nome *dev*, immetti il seguente comando:
	
	```
	bx iam space dev --guid
	```
	{: screen}
	
	Il risultato di questo comando è il seguente:
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	Quindi, esporta la variabile *Space*. Ad esempio:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. Esegui un comando cURL per inviare le metriche.

    ```
	curl -XPOST -d @Metric_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics
	```
	{: codeblock}
	
	dove
	
	* Metrics_File rappresenta il file JSON che include la definizione delle metriche inviate al servizio {{site.data.keyword.monitoringshort}}.
	
	* *X-Auth-User-Token* è un parametro che passa il token IAM o la chiave API {{site.data.keyword.Bluemix_notm}}.
	
	* *Auth_Type* è il prefisso che definisce il tipo di token o chiave API. Il seguente elenco descrive i valori validi: *apikey*, *iam* o *uaa*, dove

        * *apikey* identifica che il token è una chiave API.
		* *iam* identifica che il token specificato è un token generato da IAM.
	
	* *X-Auth-Scope-Id* è un parametro che passa il GUID dello spazio. Il GUID deve essere preceduto da *s-* per identificare uno spazio. 
	
	* Token rappresenta il token IAM o la chiave API.
	
	* Space rappresenta il GUID dello spazio. 

	
Ad esempio, puoi immettere il seguente comando per inviare due metriche, `myhost.cpu.idle` e `myapp.login.retries`, a uno spazio nel servizio {{site.data.keyword.monitoringshort}}:
	
	```
curl -XPOST @metric.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/metrics
```
{: screen}
	
Il file di esempio *metric.json* è definito come segue:

```
[
  {
    "name" : "myhost.cpu.idle",
    "value" : 22.37
  },
  {
    "name": "myapp.login.retries",
    "value": 4
  }
]
```
{: screen}


















 
