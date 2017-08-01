---

copyright:
  years: 2017

lastupdated: "2017-06-19"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Domande e risposte frequenti sull'utilizzo della CLI Bluemix
{: #cli_qa}

Queste sono le risposte alle domande più frequenti sull'utilizzo della CLI {{site.data.keyword.Bluemix}} con il servizio {{site.data.keyword.monitoringshort}}. 
{:shortdesc}

* [Come installo la CLI {{site.data.keyword.Bluemix_notm}}](#install_bmx_cli)
* [Come ottengo il GUID di un account](#account_guid)
* [Come ottengo il GUID di un'organizzazione](#org_guid)
* [Come ottengo il GUID di uno spazio](#space_guid)


## Come installo la CLI Bluemix?
{: #install_bmx_cli}

Completa la seguente procedura per installare la CLI {{site.data.keyword.Bluemix_notm}}:

1. Scarica la CLI.

    Ad esempio, per installare il pacchetto CLI {{site.data.keyword.Bluemix_notm}} in un sistema Ubuntu, scarica il [pacchetto CLI {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](http://clis.ng.bluemix.net/ui/home.html "Icona link esterno"){: new_window}. 

2. Immetti il seguente comando per estrarre il pacchetto CLI {{site.data.keyword.Bluemix_notm}}:
    
    ```
    tar -xvf Bluemix_CLI_0.5.4_amd64.tar.gz
    ```
    {: codeblock}
    
3. Vai alla directory *Bluemix_CLI* ed esegui il comando `./install_bluemix_cli` con l'autorizzazione root per installare il plug-in CF. Puoi eseguire il comando come utente root o utilizzare il comando sudo per ottenere l'autorizzazione root. Ad esempio:
    
    ```
    cd Bluemix_CLI
    ```
    {: codeblock}
    
    ```
    sudo ./install_bluemix_cli
    ```
    {: codeblock}
    
4. Verifica l'installazione del plug-in CF. Ad esempio, controlla la versione del plug-in CF. Immetti il seguente comando:
    
    ```
    cf -v
    ```
    {: codeblock}
    
    L'output è simile al seguente:
    
    ```
    cf version 6.25.0+787326d.2017-02-28
    ```
    {: screen}
    
5. Verifica l'installazione della CLI {{site.data.keyword.Bluemix_notm}}. Ad esempio, controlla la versione del plug-in. Immetti il seguente comando:
    
    ```
    bx -version
    ```
    {: codeblock}
    
    L'output è simile al seguente:
    
    ```
    bx version 0.5.4+ae22935-2017-05-18T03:55:55+00:00
    ```
    {: screen}
	
## Come ottengo il GUID di un account?
{: #account_guid}
	
Completa la seguente procedura per ottenere il GUID di un account:
	
1. Accedi a una regione, un'organizzazione e uno spazio {{site.data.keyword.Bluemix_notm}}. Esegui il comando:

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    Segui le istruzioni. Immetti le tue credenziali {{site.data.keyword.Bluemix_notm}}, seleziona un'organizzazione e uno spazio.
	
2. Esegui il comando `bx iam accounts` per ottenere il GUID di un account.

    ```
	bx iam accounts
	```
	{: codeblock} 
	
	Ad esempio, per richiamare tutti gli account con i GUID corrispondenti per l'utente xxx@yyy.com, esegui il comando:
	
	```
	bx iam accounts
	Retrieving all accounts of xxx@yyy.com...
    OK
    Account GUID                       Name                               Type    State    Owner User ID   
    12345123451234512345123451234512   A Account                          TRIAL   ACTIVE   xxx@yyy.com   
    23456234562345622456234561234561   B Account                          TRIAL   ACTIVE   zzz@yyy.com   
	```
	{: screen}

	
## Come ottengo il GUID di un'organizzazione?
{: #org_guid}

Completa la seguente procedura per ottenere il GUID di un'organizzazione:
	
1. Accedi a una regione, un'organizzazione e uno spazio {{site.data.keyword.Bluemix_notm}}. Esegui il comando:

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    Segui le istruzioni. Immetti le tue credenziali {{site.data.keyword.Bluemix_notm}}, seleziona un'organizzazione e uno spazio.

2. Esegui il comando `bx iam org` per ottenere il GUID dell'organizzazione. 

    ```
    bx iam org NAME --guid
    ```
    {: codeblock}
	
    dove NAME è il nome dell'organizzazione {{site.data.keyword.Bluemix_notm}}.
		
## Come ottengo il GUID di uno spazio?
{: #space_guid}
	
Completa la seguente procedura per ottenere il GUID di uno spazio:
	
1. Accedi a una regione, un'organizzazione e uno spazio {{site.data.keyword.Bluemix_notm}}. Esegui il comando:

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    Segui le istruzioni. Immetti le tue credenziali {{site.data.keyword.Bluemix_notm}}, seleziona un'organizzazione e uno spazio.
	
2. Esegui il comando `bx iam space` per ottenere il GUID dello spazio. 

    ```
    bx iam space NAME --guid
    ```
    {: codeblock}
	
    dove NAME è il nome di uno spazio {{site.data.keyword.Bluemix_notm}}. 
	
    Ad esempio, per ottenere il GUID per lo spazio *dev*, immetti il seguente comando:
	
    ```
    bx iam space dev --guid
    e03afff1-3456-4af6-1234-59treg1b0abc
    ```
    {: screen}




		
		
