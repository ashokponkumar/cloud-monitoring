---

copyright:
  years: 2017

lastupdated: "2017-07-10"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Provisioning del servizio di monitoraggio
{: #provision}

Puoi eseguire il provisioning del servizio {{site.data.keyword.monitoringshort}} dall'IU {{site.data.keyword.Bluemix}} o dalla riga di comando.
{:shortdesc}


## Provisioning dall'IU
{: #ui}

Completa la seguente procedura per eseguire il provisioning di un'istanza del servizio {{site.data.keyword.monitoringshort}} in {{site.data.keyword.Bluemix_notm}}:

1. Accedi al tuo account {{site.data.keyword.Bluemix_notm}}.

    Il dashboard {{site.data.keyword.Bluemix_notm}} è disponibile all'indirizzo: [http://bluemix.net ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](http://bluemix.net "Icona link esterno"){:new_window}.
    
	Dopo aver effettuato l'accesso con il tuo ID utente e la tua password, viene aperta l'IU {{site.data.keyword.Bluemix_notm}}.

2. Fai clic su **Catalogo**. Viene aperto l'elenco dei servizi disponibili su {{site.data.keyword.Bluemix_notm}}.

3. Seleziona la categoria **DevOps** per filtrare l'elenco di servizi che viene visualizzato.

4. Fai clic sul tile **Monitoraggio**.

5. Seleziona un piano di servizio. Per raccogliere le metriche e accedere ad esse, fino a un massimo di 45 giorni, e per definire le regole di avviso, comprese le regole con caratteri jolly, seleziona il piano **Premium**. Per impostazione predefinita, è impostato il piano **Lite**, che dà diritto alla raccolta delle metriche di piattaforma nello spazio dove stai eseguendo il provisioning del servizio e un periodo di conservazione di 15 giorni per tali metriche. 

    Per ulteriori informazioni sui piani di servizio, vedi [Piani di servizio](/docs/services/cloud-monitoring/monitoring_ov.html#plans).
	
6. Fai clic su **Crea** per eseguire il provisioning del servizio {{site.data.keyword.monitoringshort}} nello spazio {{site.data.keyword.Bluemix_notm}} in cui hai eseguito l'accesso.
  
 

## Provisioning dalla CLI
{: #cli}

Completa la seguente procedura per eseguire il provisioning di un'istanza del servizio in {{site.data.keyword.monitoringshort}} service in {{site.data.keyword.Bluemix_notm}} mediante la riga di comando:

1. [Prerequisito] Installa la CLI {{site.data.keyword.Bluemix_notm}}.

   Per ulteriori informazioni, vedi [Installazione della CLI {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa).
   
   Se la CLI è installata, vai al passo successivo.
    
2. Accedi a una regione, un'organizzazione e uno spazio {{site.data.keyword.Bluemix_notm}}. Esegui il comando:

    ```
    cf login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    Segui le istruzioni. Immetti le tue credenziali {{site.data.keyword.Bluemix_notm}}, seleziona un'organizzazione e uno spazio.
	
3. Esegui il comando `cf create-service` per eseguire il provisioning di un'istanza.

    ```
	cf create-service nome_servizio piano_servizio nome_istanza_servizio
	```
	{: codeblock}
	
	Dove
	
	* nome_servizio è il nome del servizio, ossia **Monitoring**.
	* piano_servizio è il nome del piano di servizio. Per raccogliere le metriche e accedere ad esse, fino a un massimo di 45 giorni, e per definire le regole di avviso, comprese le regole con caratteri jolly, seleziona il piano **Premium**. Per impostazione predefinita, è impostato il piano **Lite**, che dà diritto alla raccolta delle metriche di piattaforma nello spazio dove stai eseguendo il provisioning del servizio e un periodo di conservazione di 15 giorni per tali metriche.Per un elenco dei piani, vedi [Piani di servizio {{site.data.keyword.monitoringshort}}](/docs/services/cloud-monitoring/monitoring_ov.html#plan).
	* nome_istanza_servizio è il nome che desideri utilizzare per la nuova istanza di servizio che hai creato.

	Per ulteriori informazioni sul comando cf, vedi [cf create-service](/docs/cli/reference/cfcommands/index.html#cf_create-service).

	Ad esempio, per creare un'istanza del servizio {{site.data.keyword.monitoringshort}} con un piano premium, esegui questo comando:
	
	```
	cf create-service Monitoring premium my_monitoring_svc
	```
	{: codeblock}
	
4. Verifica che il servizio venga creato correttamente. Immetti il seguente comando:

    ```	
	cf services
	```
	{: codeblock}
	
	L'output dell'esecuzione del comando è simile al seguente:
	
	```
    Richiamo dei servizi nell'organizzazione MyOrg / space MySpace come xxx@yyy.com...
    OK
    
    name                           service                  plan                   bound apps              last operation
    my_monitoring_svc              Monitoring               premium                                        create succeeded
	```
	{: screen}

	



