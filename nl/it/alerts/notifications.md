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


# Gestione delle notifiche utilizzando l'API Avvisi
{: #notifications}

Utilizza l'API Avvisi per creare, eliminare e aggiornare una notifica, per visualizzare i dettagli di una notifica e per elencare le notifiche definite nel tuo spazio {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

## Creazione di un template di notifica
{: #template}

Una notifica è un file JSON. 

Puoi creare qualsiasi numero di template di notifica e quindi riutilizzarli per creare notifiche di quel tipo nella tua organizzazione. 

Puoi definire uno dei seguenti tipi di notifiche:

* Email: definisci una notifica di tipo *Email* per inviare un'e-mail a un indirizzo valido. 
* Webhook: definisci una notifica di tipo *Webhook* solo per gli endpoint https. Aggiungi un parametro all'endpoint per ridurre la possibilità che qualcun altro tenti di richiamare il tuo endpoint.
* Pagerduty: definisci una notifica di tipo *PagerDuty* per inviare i dati di avviso per una metrica al tuo sistema di gestione degli incidenti PagerDuty. 

La seguente tabella elenca degli esempi di template di notifica:

<table>
  <caption></caption>
  <tr>
    <th>Tipo</th>
	<th>Template</th>
	<th>Esempio</th>
  </tr>
  <tr>
    <td>Email</td>
	<td>
	```
	{
	"name": "Template_Name",
	"type": "Email",
	"description" : "Description",
	"detail": "EmailAddress"
	}
	```
	{: codeblock}
	</td>
	<td>
	```
	{
	"name": "my-email",
	"type": "Email",
	"description" : "Invia notifica e-mail se esiste un problema di infrastruttura.",
	"detail": "xxx@yyy.com"
	}
	```
	{: screen}
	</td>
  </tr>
  <tr>
    <td>Webhook</td>
	<td>
	```
	{
	"name": "Template_Name",
	"type": "Webhook",
	"description" : "Description",
	"detail": "Endpoint"
	}
	```
	{: codeblock}
	</td>
	<td>
	```
	{
	"name": "my-webhook",
	"type": "Webhook",
	"description" : "Attiva un webhook se esiste un problema di infrastruttura..",
	"detail": "https://myendpoint.bluemix.net?key=abcd1234"
	}
	```
	{: screen}
	</td>
  </tr>
  <tr>
    <td>Pagerduty</td>
	<td>
	```
	{
	"name": "Template_Name",
	"type": "PagerDuty",
	"description" : "Description",
	"detail": "Pagerduty_APIkey"
	}
	```
	{: codeblock}
	</td>
	<td>
	```
	{
	"name": "my-pagerduty",
	"type": "PagerDuty",
	"description" : "Attiva un avviso PagerDuty se esiste un problema di infrastruttura..",
	"detail": "abcd1234"
	}
	```
	{: screen}
	</td>
  </tr>
</table>

Dove

* *Template_Name* definisce il nome del template di notifica.
* *Description* spiega quando viene utilizzato questo tipo di notifica.
* *EmailAddress* definisce l'indirizzo e-mail del destinatario della notifica.
* *Endpoint* definisce l'URL in cui effettuare il POST. Si tratta di un URL che può effettuare la richiesta POST. 
* *Pagerduty_APIkey*  definisce una chiave API univoca. Questa chiave API viene generata da un amministratore o proprietario dell'account PagerDuty.


Completa la seguente procedura per creare un template di notifica:

1. Crea una directory per memorizzare le risorse del servizio {{site.data.keyword.monitoringshort}}, ad esempio *cloud-monitoring*.

    Ad esempio, in un sistema Ubuntu, immetti il seguente comando:
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
2. Crea una directory per memorizzare i tuoi template di notifica, ad esempio, *notification-templates*.

    Ad esempio, in un sistema Ubuntu, immetti il seguente comando:
	
	```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	Passa a questa directory:
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
3. Crea un template di notifica in una directory locale del tuo sistema.

    Ad esempio, crea il file **email-template.json** per un template di notifica e-mail utilizzando l'editor vi: 
	
	```
	{
    "name": "email_template",
    "type": "Email",
    "description" : "Invia e-mail al responsabile del dipartimento X quando...",
    "detail": "xxx@yyy"
    }
	```
	{: codeblock}
	


## Creazione di una notifica
{: #create}

Per creare una notifica, completa la seguente procedura:

1. Crea un file di notifica.

    1. Utilizza un template di notifica per creare il file. Per ulteriori informazioni, vedi [Creazione di un template di notifica](#template).
	
	2. Aggiorna il file:
	
	    * Immetti un nome univoco nel campo *name*. Il valore di questo campo viene utilizzato in seguito per aggiornare, eliminare e mostrare i dettagli della notifica.
		* Immetti una descrizione.
		* Immetti un indirizzo e-mail, un endpoint URL o una chiave PagerDuty validi.
		
	3. Salva il file. Ad esempio, utilizza un prefisso come *s-* per le notifiche che definisci per le query in esecuzione in uno spazio {{site.data.keyword.Bluemix_notm}}.
	
	    **Suggerimento:** crea il tuo file di notifica nella seguente directory: *~/cloud-monitoring/notifications/* per raggruppare le risorse del servizio {{site.data.keyword.monitoringshort_notm}}. 
	
2. Accedi a una regione, un'organizzazione e uno spazio {{site.data.keyword.Bluemix_notm}}. Esegui il comando:

    Ad esempio, per accedere alla regione Stati Uniti Sud, esegui il seguente comando:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Segui le istruzioni. Immetti le tue credenziali {{site.data.keyword.Bluemix_notm}}, seleziona un'organizzazione e uno spazio.

3. Ottieni il token di autenticazione o la chiave API.

    * Per l'autenticazione IAM, consulta [Ottenimento del token IAM utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) oppure [Generazione di una chiave API IAM utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Per l'autenticazione UAA, consulta [Ottenimento del token UAA utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) oppure [Ottenimento del token UAA utilizzando l'API REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

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
	
	Quindi, esporta la variabile *Token*:
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Nota:** il token esclude *Bearer*.
		
4. Ottieni il GUID dello spazio. Il GUID deve essere preceduto da *s-* per identificare uno spazio.

    Immetti il seguente comando:
	
	```
	bx iam space SpaceName --guid
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
	
	Quindi, esporta la variabile *Space*:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. Immetti il seguente comando cURL per creare una notifica:

    ```
	curl -XPOST -d @Notification_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: codeblock}
	
	dove
	
	* Notification_File è il file JSON che definisce la tua notifica.
	
	* *X-Auth-User-Token* è un parametro che passa il token UAA, il token IAM o la chiave API {{site.data.keyword.Bluemix_notm}}.
	
	* *Auth_Type* è il prefisso che definisce il tipo di token o chiave API. Il seguente elenco descrive i valori validi: *apikey*, *iam* o *uaa*, dove

        * *apikey* identifica che il token è una chiave API.
		* *iam* identifica che il token specificato è un token generato da IAM.
		* *uaa* identifica che il token specificato è un token generato da UAA.
	
	* *X-Auth-Scope-Id* è un parametro che passa il GUID dello spazio. Il GUID deve essere preceduto da *s-* per identificare uno spazio. Questa intestazione è obbligatoria se utilizzi un'autenticazione token UAA. Se utilizzi un token di autenticazione IAM, questa intestazione è facoltativa e vengono utilizzate le informazioni di dominio associate al token.
	
	* Token è il token UAA, il token IAM o la chiave API.
	
	* Space è il GUID dello spazio. È obbligatorio solo se utilizzi un token UAA.
	
    Ad esempio, 	
	
	```
	curl -XPOST -d @s-email-dep-A.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.{DomainName}/v1/alert/notification
	```
	{: screen}

## Eliminazione una notifica
{:#delete}

Per eliminare una notifica, completa la seguente procedura:

1. Accedi a una regione, un'organizzazione e uno spazio {{site.data.keyword.Bluemix_notm}}. Esegui il comando:

    Ad esempio, per accedere alla regione Stati Uniti Sud, esegui il seguente comando:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Segui le istruzioni. Immetti le tue credenziali {{site.data.keyword.Bluemix_notm}}, seleziona un'organizzazione e uno spazio.

2. Ottieni il token di autenticazione o la chiave API.

    * Per l'autenticazione IAM, consulta [Ottenimento del token IAM utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) oppure [Generazione di una chiave API IAM utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Per l'autenticazione UAA, vedi [Ottenimento del token UAA utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) oppure [Ottenimento del token UAA utilizzando l'API REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

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
	
	Quindi, esporta la variabile *Token*:
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Nota:** il token esclude *Bearer*.
	
3. Ottieni il GUID dello spazio. Il GUID deve essere preceduto da *s-* per identificare uno spazio.

    Immetti il seguente comando:
	
	```
	bx iam space SpaceName --guid
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
	
	Quindi, esporta la variabile *Space*:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. Immetti il seguente comando cURL per eliminare una notifica:

    ```
	curl -XDELETE --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification/Notification_Name
	```
	{: codeblock}
	
	dove
	
	* *Auth_Type* è il prefisso che definisce il tipo di token o chiave API. Il seguente elenco descrive i valori validi: *apikey*, *iam* o *uaa*, dove

        * *apikey* identifica che il token è una chiave API.
		* *iam* identifica che il token specificato è un token generato da IAM.
		* *uaa* identifica che il token specificato è un token generato da UAA.
	
	* *X-Auth-User-Token* è un parametro che passa il token UAA, il token IAM o la chiave API {{site.data.keyword.Bluemix_notm}}. Il token o l'API devono essere preceduti da uno dei seguenti valori: *apikey*, *iam* o *uaa*, dove

        * *apikey* identifica che il token è una chiave API.
		* *iam* identifica che il token specificato è un token generato da IAM.
		* *uaa* identifica che il token specificato è un token generato da UAA.
		
	* Token è il token UAA, il token IAM o la chiave API.
	
	* Space è il GUID dello spazio. È obbligatorio solo se utilizzi un token UAA.
	
	* Notification_Name è il nome della notifica, ossia, il valore del campo *name* quando elenchi le proprietà di una notifica.
	


## Elenco di tutte le notifiche
{: #list}

Per elencare tutte le notifiche, completa la seguente procedura:

1. Accedi a una regione, un'organizzazione e uno spazio {{site.data.keyword.Bluemix_notm}}. Esegui il comando:

    Ad esempio, per accedere alla regione Stati Uniti Sud, esegui il seguente comando:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Segui le istruzioni. Immetti le tue credenziali {{site.data.keyword.Bluemix_notm}}, seleziona un'organizzazione e uno spazio.

2. Ottieni il token di autenticazione o la chiave API.

    * Per l'autenticazione IAM, consulta [Ottenimento del token IAM utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) oppure [Generazione di una chiave API IAM utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Per l'autenticazione UAA, vedi [Ottenimento del token UAA utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) oppure [Ottenimento del token UAA utilizzando l'API REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

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
	
	Quindi, esporta la variabile *Token*:
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Nota:** il token esclude *Bearer*.
	
3. Ottieni il GUID dello spazio. Il GUID deve essere preceduto da *s-* per identificare uno spazio.

    Immetti il seguente comando:
	
	```
	bx iam space SpaceName --guid
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
	
	Quindi, esporta la variabile *Space*:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. Immetti il seguente comando cURL per elencare tutte le notifiche:

    ```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notifications
	```
	{: codeblock}
	
	dove
	
	* *Auth_Type* è il prefisso che definisce il tipo di token o chiave API. Il seguente elenco descrive i valori validi: *apikey*, *iam* o *uaa*, dove

        * *apikey* identifica che il token è una chiave API.
		* *iam* identifica che il token specificato è un token generato da IAM.
		* *uaa* identifica che il token specificato è un token generato da UAA.
		
	* *X-Auth-User-Token* è un parametro che passa il token UAA, il token IAM o la chiave API {{site.data.keyword.Bluemix_notm}}. Il token o l'API devono essere preceduti da uno dei seguenti valori: *apikey*, *iam* o *uaa*, dove

        * *apikey* identifica che il token è una chiave API.
		* *iam* identifica che il token specificato è un token generato da IAM.
		* *uaa* identifica che il token specificato è un token generato da UAA.
		
	* Token è il token UAA, il token IAM o la chiave API.
	
	* Space è il GUID dello spazio. È obbligatorio solo se utilizzi un token UAA.


			

## Visualizzazione dei dettagli di una notifica
{: #show}


Per visualizzare le informazioni relative a una notifica, completa la seguente procedura:

1. Accedi a una regione, un'organizzazione e uno spazio {{site.data.keyword.Bluemix_notm}}. Esegui il comando:

    Ad esempio, per accedere alla regione Stati Uniti Sud, esegui il seguente comando:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Segui le istruzioni. Immetti le tue credenziali {{site.data.keyword.Bluemix_notm}}, seleziona un'organizzazione e uno spazio.

2. Ottieni il token di autenticazione o la chiave API.

    * Per l'autenticazione IAM, consulta [Ottenimento del token IAM utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) oppure [Generazione di una chiave API IAM utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Per l'autenticazione UAA, vedi [Ottenimento del token UAA utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) oppure [Ottenimento del token UAA utilizzando l'API REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

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
	
	Quindi, esporta la variabile *Token*:
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Nota:** il token esclude *Bearer*.
	
3. Ottieni il GUID dello spazio. Il GUID deve essere preceduto da *s-* per identificare uno spazio.

    Immetti il seguente comando:
	
	```
	bx iam space SpaceName --guid
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
	
	Quindi, esporta la variabile *Space*:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. Immetti il seguente comando cURL per visualizzare i dettagli di una notifica:

    ```
	curl -XGET --header "X-Auth-User-Token:uaa ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification/Notification_Name
	```
	{: codeblock}
	
	dove
	
	* *X-Auth-User-Token* è un parametro che passa il token UAA, il token IAM o la chiave API {{site.data.keyword.Bluemix_notm}}. Il token o l'API devono essere preceduti da uno dei seguenti valori: *apikey*, *iam* o *uaa*, dove

        * *apikey* identifica che il token è una chiave API.
		* *iam* identifica che il token specificato è un token generato da IAM.
		* *uaa* identifica che il token specificato è un token generato da UAA.
		
	* Token è il token UAA, il token IAM o la chiave API.
	
	* Space è il GUID dello spazio. È obbligatorio solo se utilizzi un token UAA.
	
	* Notification_Name è il nome della notifica, ossia, il valore del campo *name* quando elenchi le proprietà di una notifica.
	
     
		

			
## Verifica di una notifica
{: #test}	
			
Per verificare una notifica, completa la seguente procedura:

1. Accedi a una regione, un'organizzazione e uno spazio {{site.data.keyword.Bluemix_notm}}. Esegui il comando:

    Ad esempio, per accedere alla regione Stati Uniti Sud, esegui il seguente comando:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Segui le istruzioni. Immetti le tue credenziali {{site.data.keyword.Bluemix_notm}}, seleziona un'organizzazione e uno spazio.

2. Ottieni il token di autenticazione o la chiave API.

    * Per l'autenticazione IAM, consulta [Ottenimento del token IAM utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) oppure [Generazione di una chiave API IAM utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Per l'autenticazione UAA, vedi [Ottenimento del token UAA utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) oppure [Ottenimento del token UAA utilizzando l'API REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

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
	
	Quindi, esporta la variabile *Token*:
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Nota:** il token esclude *Bearer*.

3. Ottieni il GUID dello spazio. Il GUID deve essere preceduto da *s-* per identificare uno spazio.

    Immetti il seguente comando:
	
	```
	bx iam space SpaceName --guid
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
	
	Quindi, esporta la variabile *Space*:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. Immetti il seguente comando cURL per verificare una notifica:

    ```
	curl -XPOST --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification/test/Notification_Name
	```
	{: codeblock}
	
	dove
	
	* *Auth_Type* è il prefisso che definisce il tipo di token o chiave API. Il seguente elenco descrive i valori validi: *apikey*, *iam* o *uaa*, dove

        * *apikey* identifica che il token è una chiave API.
		* *iam* identifica che il token specificato è un token generato da IAM.
		* *uaa* identifica che il token specificato è un token generato da UAA.
	
	* *X-Auth-User-Token* è un parametro che passa il token UAA, il token IAM o la chiave API {{site.data.keyword.Bluemix_notm}}. Il token o l'API devono essere preceduti da uno dei seguenti valori: *apikey*, *iam* o *uaa*, dove

        * *apikey* identifica che il token è una chiave API.
		* *iam* identifica che il token specificato è un token generato da IAM.
		* *uaa* identifica che il token specificato è un token generato da UAA.
		
	* Token è il token UAA, il token IAM o la chiave API.
	
	* Space è il GUID dello spazio. È obbligatorio solo se utilizzi un token UAA.
	
	* Notification_Name è il nome della notifica, ossia, il valore del campo *name* quando elenchi le proprietà di una notifica.
	


## Aggiornamento di una notifica
{: #update}

Per aggiornare una notifica, completa la seguente procedura:

1. Accedi a una regione, un'organizzazione e uno spazio {{site.data.keyword.Bluemix_notm}}. Esegui il comando:

    Ad esempio, per accedere alla regione Stati Uniti Sud, esegui il seguente comando:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Segui le istruzioni. Immetti le tue credenziali {{site.data.keyword.Bluemix_notm}}, seleziona un'organizzazione e uno spazio.

2. Ottieni il token di autenticazione o la chiave API.

    * Per l'autenticazione IAM, consulta [Ottenimento del token IAM utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) oppure [Generazione di una chiave API IAM utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Per l'autenticazione UAA, vedi [Ottenimento del token UAA utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) oppure [Ottenimento del token UAA utilizzando l'API REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

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
	
	Quindi, esporta la variabile *Token*:
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Nota:** il token esclude *Bearer*.
	
3. Ottieni il GUID dello spazio. Il GUID deve essere preceduto da *s-* per identificare uno spazio.

    Immetti il seguente comando:
	
	```
	bx iam space SpaceName --guid
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
	
	Quindi, esporta la variabile *Space*:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. Immetti il seguente comando cURL per aggiornare una notifica:

    ```
	curl -XPUT -d @Notification_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: codeblock}
	
	dove
	
	* Notification_File è il file JSON che definisce la tua notifica.
	
	* *Auth_Type* è il prefisso che definisce il tipo di token o chiave API. Il seguente elenco descrive i valori validi: *apikey*, *iam* o *uaa*, dove

        * *apikey* identifica che il token è una chiave API.
		* *iam* identifica che il token specificato è un token generato da IAM.
		* *uaa* identifica che il token specificato è un token generato da UAA.
	
	* *X-Auth-User-Token* è un parametro che passa il token UAA, il token IAM o la chiave API Bluemix. Il token o l'API devono essere preceduti da uno dei seguenti valori: *apikey*, *iam* o *uaa*, dove

        * *apikey* identifica che il token è una chiave API.
		* *iam* identifica che il token specificato è un token generato da IAM.
		* *uaa* identifica che il token specificato è un token generato da UAA.
		
	* Token è il token UAA, il token IAM o la chiave API.
	
	* Space è il GUID dello spazio. È obbligatorio solo se utilizzi un token UAA.

	
        

