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


# Autenticazione mediante il modello Bluemix IAM
{: #auth_iam}

Utilizza il modello {{site.data.keyword.Bluemix}} IAM per ottenere un token di autenticazione che puoi utilizzare per accedere alle metriche memorizzate nel servizio {{site.data.keyword.monitoringshort}} o per ottenere una chiave API. Il token ha un tempo di scadenza, la chiave API non scade.
{:shortdesc}


## Ottenimento del token IAM utilizzando la CLI Bluemix 
{: #iam_token_cli}

Per ottenere il token di autorizzazione utilizzando la CLI {{site.data.keyword.Bluemix_notm}}, completa la seguente procedura:

1. Installa la CLI {{site.data.keyword.Bluemix_notm}}.

   Per ulteriori informazioni, vedi [Installazione della CLI {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa).
   
   Se la CLI è installata, vai al passo successivo.
    
2. Accedi a una regione, un'organizzazione e uno spazio {{site.data.keyword.Bluemix_notm}}. Esegui il comando:

    Ad esempio, per la regione Stati Uniti Sud, esegui il seguente comando:
	
    ```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Segui le istruzioni. Immetti le tue credenziali {{site.data.keyword.Bluemix_notm}}, seleziona un'organizzazione e uno spazio.
	
3. Esegui il comando `bx iam oauth-tokens` per ottenere il token IAM.

    ```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	L'output restituisce il token IAM che devi utilizzare per autenticare il tuo ID utente in tale spazio e organizzazione.

**Nota:** quando utilizzi il token, rimuovi *Bearer* dal valore del token che passi in una chiamata API.
		
		
## Generazione di una chiave API IAM utilizzando la CLI Bluemix
{: #iam_apikey_cli}

Completa la seguente procedura per generare una chiave API IAM utilizzando la CLI {{site.data.keyword.Bluemix_notm}}:

1. Installa la CLI {{site.data.keyword.Bluemix_notm}}.

   Per ulteriori informazioni, vedi [Installazione della CLI {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa).

   Se la CLI è installata, vai al passo successivo.
	
2. Accedi a {{site.data.keyword.Bluemix_notm}} utilizzando il comando CLI `bx login` {{site.data.keyword.Bluemix_notm}}:

    Ad esempio, per la regione Stati Uniti Sud, esegui il seguente comando:
	
    ```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Segui le istruzioni. Immetti le tue credenziali {{site.data.keyword.Bluemix_notm}}, seleziona un'organizzazione e uno spazio.
 
3. Esegui il comando `bx iam api-key-create` per creare una chiave API IAM.

    ```
    bx iam api-key-create NAME [-d DESCRIPTION][-f, --file FILE]
	```
	{: codeblock} 
	
	dove
	
	* NAME è il nome della chiave API da creare.
	* DESCRIPTION è il testo utilizzato per descrivere la chiave API.
	* FILE è il nome del file in cui viene salvata la chiave.
	
    Ad esempio, per creare una chiave API*MyKey*, immetti il seguente comando:
	
	```
	bx iam api-key-create MyKey -d "This is my API key for service X" 
	```
	{: screen}
	
	
	
	
## Generazione di una chiave API IAM utilizzando l'IU Bluemix
{: #iam_apikey_ui}

Completa la seguente procedura per generare una chiave API IAM tramite l'IU {{site.data.keyword.Bluemix_notm}}:

1. Accedi alla console {{site.data.keyword.Bluemix_notm}}. 

2. Dalla barra dei menu della console, fai clic su **Gestisci > Sicurezza > Chiavi API Bluemix**.

3. Fai clic su **Crea chiave API**.

4. Immetti un nome e una descrizione per la tua chiave API. Fai quindi clic su **Crea chiave API**.

5. Salva la chiave API. Fai clic su **Scarica chiave API**.

    Fai clic su **Mostra** per visualizzare la chiave API.  

**Nota:** la chiave API è disponibile per la visualizzazione o il download solo durante la fase di creazione. Se la chiave API viene persa, dovrai crearne una nuova.  


	
## Revoca di una chiave API utilizzando l'IU Bluemix
{: #revoke_ui}
	
Completa la seguente procedura per revocare una chiave API IAM tramite l'IU {{site.data.keyword.Bluemix_notm}}:

1. Accedi alla console {{site.data.keyword.Bluemix_notm}}.

2. Dalla barra dei menu della console, fai clic su **Gestisci > Sicurezza > Chiavi API Bluemix**.

3. Fai clic su **Azioni** e quindi su **Elimina chiave**.





	

	
	
	
	
	
	
