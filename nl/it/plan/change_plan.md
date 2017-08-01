---

copyright:
  years: 2017
lastupdated: "2017-07-10"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Modifica del piano
{: #change_plan}

Puoi modificare il tuo piano di servizio {{site.data.keyword.monitoringshort}} in {{site.data.keyword.Bluemix}} nel Dashboard del servizio o eseguendo il comando `cf update-service`. Puoi aggiornare o ridurre il tuo piano in qualsiasi momento.
{:shortdesc}

## Modifica del piano di servizio attraverso l'IU
{: #change_plan_ui}

Per modificare il tuo piano di servizio in {{site.data.keyword.Bluemix_notm}} nel Dashboard del servizio, completa la seguente procedura:

1. Accedi a {{site.data.keyword.Bluemix_notm}} e fai clic sul servizio {{site.data.keyword.monitoringshort}} dal dashboard {{site.data.keyword.Bluemix_notm}}. 
    
2. Seleziona la scheda **Piano** nell'IU {{site.data.keyword.Bluemix_notm}}.

    Vengono visualizzate le informazioni sul tuo piano corrente.
	
3. Seleziona un nuovo piano per aggiornare o ridurre il tuo piano. 

4. Fai clic su **Salva**.



## Modifica del piano di servizio attraverso la CLI
{: #change_plan_cli}

Per modificare il tuo piano di servizio in {{site.data.keyword.Bluemix_notm}} servendoti della CLI, completa la seguente procedura:

1. 1. Accedi a una regione, un'organizzazione e uno spazio {{site.data.keyword.Bluemix_notm}}. Esegui il comando:

    ```
    cf login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}
	
2. Esegui il comando `cf services` per controllare il tuo piano corrente e per ottenere il nome del servizio {{site.data.keyword.loganalysisshort}} dall'elenco di servizi disponibile nello spazio. 

    Il valore del campo **name** è quello che devi utilizzare per modificare il piano. 

    Ad esempio,
	
	```
	$ cf services
	Getting services in org MyOrg / space dev as xxx@yyy.com...
	OK
	name            service      plan   bound apps   last operation
	Monitoring-0c   Monitoring   premium             create succeeded
    ```
	{: screen}
    
3. Aggiorna o riduci il tuo piano. Esegui il comando `cf update-service`.
    
	```
	cf update-service service_name [-p new_plan]
	```
	{: codeblock}
	
	dove 
	
	* *service_name* è il nome del tuo servizio. Per ottenere il valore, puoi eseguire il comando `cf services`.
	* *new_plan* è il nome del piano.
	
	La seguente tabella elenca i diversi piani e i relativi valori supportati:
	
	<table>
	  <caption>Tabella 1. Elenco dei piani.</caption>
	  <tr>
	    <th>Piano</th>
		<th>Funzioni</th>
	    <th>Nome</th>
	  </tr>
	  <tr>
	    <td>Lite</td>
	    <td>Monitoraggio gratuito.</td>
		<td>lite</td>
	  </tr>
	  <tr>
	    <td>Premium</td>
	    <td>Monitoraggio aggiuntivo.</td>
		<td>premium</td>
	  </tr>
	</table>
	
	Ad esempio, per ridurre il tuo piano al piano *Lite*, immetti il seguente comando:
	
	```
	cf update-service "Monitoring-0c" -p lite
    Updating service instance Monitoring-0c as xxx@yyy.com...
    OK
	```
	{: screen}

4. Verifica che il nuovo piano sia stato modificato. Esegui il comando `cf services`.

    ```
	cf services
    Getting services in org MyOrg / space dev as xxx@yyy.com...
    OK

    name              service       plan   bound apps   last operation
    Monitoring-0c     Monitoring    lite                create succeeded
	```
	{: screen}






