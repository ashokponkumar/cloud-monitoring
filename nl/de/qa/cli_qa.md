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



# Häufig gestellte Fragen zur Verwendung der IBM Cloud-CLI
{: #cli_qa}

Hier sind die Antworten auf häufig gestellte Fragen zur Verwendung der {{site.data.keyword.Bluemix}}-CLI mit dem {{site.data.keyword.monitoringshort}}-Service.
{:shortdesc}

* [Wie melde ich mich bei {{site.data.keyword.Bluemix_notm}} an?](/docs/services/cloud-monitoring/qa/cli_qa.html#login)
* [ Wie installiere ich die {{site.data.keyword.Bluemix_notm}}-Befehlszeilenschnittstelle?](/docs/services/cloud-monitoring/qa/cli_qa.html#install_bmx_cli)
* [Wie erhalte ich die GUID eines Kontos?](/docs/services/cloud-monitoring/qa/cli_qa.html#account_guid)
* [Wie erhalte ich die GUID einer Organisation?](/docs/services/cloud-monitoring/qa/cli_qa.html#org_guid)
* [Wie erhalte ich die GUID eines Bereichs?](/docs/services/cloud-monitoring/qa/cli_qa.html#space_guid)

## Wie melde ich mich bei IBM Cloud an?
{: #login}

Führen Sie folgenden Befehl aus, um sich in einer Region, einer Organisation und einem Bereich von {{site.data.keyword.Bluemix_notm}} anzumelden:

```
bx login -a Endpoint
```
{: codeblock}
	
Hierbei ist *Endpoint* (Endpunkt) die URL für die Anmeldung bei {{site.data.keyword.Bluemix_notm}}. Diese URL ist je nach Region unterschiedlich.
	
<table>
    <caption>Liste der Endpunkte für den Zugriff auf {{site.data.keyword.Bluemix_notm}}</caption>
	<tr>
	  <th>Region</th>
	  <th>URL</th>
	</tr>
	<tr>
	  <td>Deutschland</td>
	  <td>api.eu-de.bluemix.net</td>
	</tr>
	<tr>
	  <td>Sydney</td>
	  <td>api.au-syd.bluemix.net</td>
	</tr>
	<tr>
	  <td>Vereinigtes Königreich</td>
	  <td>api.eu-gb.bluemix.net</td>
	</tr>
	<tr>
	  <td>USA (Süden)</td>
	  <td>api.ng.bluemix.net</td>
	</tr>
</table>

Führen Sie zum Beispiel den folgenden Befehl aus, um sich bei der Region 'USA (Süden)' anzumelden:
	
```
bx login -a https://api.ng.bluemix.net
```
{: codeblock}

Befolgen Sie die Anweisungen. 

Anschließend legen Sie die Organisation und den Bereich fest. Führen Sie folgenden Befehl aus:

```
bx target -o OrgName -s SpaceName
```
{: codeblock}

Dabei gilt Folgendes:

* OrgName ist der Name der Organisation.
* SpaceName ist der Name des Bereichs.

	
## Wie installiere ich die IBM Cloud-Benutzerschnittstelle?
{: #install_bmx_cli}

Weitere Informationen finden Sie unter [{{site.data.keyword.Bluemix_notm}}-CLI herunterladen und installieren](/docs/cli/reference/bluemix_cli/download_cli.html#download_install). 

## Wie erhalte ich die GUID eines Kontos?
{: #account_guid}
	
Führen Sie die folgenden Schritte aus, um die GUID eines Kontos zu erhalten:
	
1. Melden Sie sich bei einer Region, einer Organisation und einem Bereich in {{site.data.keyword.Bluemix_notm}} an.Führen Sie den folgenden Befehl aus:

    ```
	bx login -a Endpoint
	```
	{: codeblock}
	
	Hierbei ist *Endpoint* (Endpunkt) die URL für die Anmeldung bei {{site.data.keyword.Bluemix_notm}}. Diese URL ist je nach Region unterschiedlich.
	
	<table>
	    <caption>Liste der Endpunkte für den Zugriff auf {{site.data.keyword.Bluemix_notm}}</caption>
		<tr>
		  <th>Region</th>
		  <th>URL</th>
		</tr>
		<tr>
		  <td>USA (Süden)</td>
		  <td>api.ng.bluemix.net</td>
		</tr>
		<tr>
		  <td>Vereinigtes Königreich</td>
		  <td>api.eu-gb.bluemix.net</td>
		</tr>
	</table>

    Führen Sie zum Beispiel den folgenden Befehl aus, um sich bei der Region 'USA (Süden)' anzumelden:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Befolgen Sie die Anweisungen. Geben Sie Ihre {{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise ein, wählen Sie eine Organisation und einen Bereich aus.
	
	Führen Sie für **eingebundene Benutzer-IDs** den folgenden Befehl aus und befolgen Sie die Anweisungen:
	
	```
    bx login -a https://api.ng.bluemix.net --sso
    ```
    {: codeblock}
	
2. Führen Sie den Befehl `bx iam accounts` aus, um die GUID eines Kontos abzurufen.

    ```
	bx iam accounts
	```
	{: codeblock} 
	
	Um zum Beispiel alle Konten mit deren entsprechenden GUIDs für den Benutzer xxx@yyy.com abzurufen, führen Sie den folgenden Befehl aus.
	
	```
	bx iam accounts
	Retrieving all accounts of xxx@yyy.com...
    OK
    Account GUID                       Name                               Type    State    Owner User ID   
    12345123451234512345123451234512   A Account                          TRIAL   ACTIVE   xxx@yyy.com   
    23456234562345622456234561234561   B Account                          TRIAL   ACTIVE   zzz@yyy.com   
	```
	{: screen}

	
## Wie erhalte ich die GUID einer Organisation?
{: #org_guid}

Führen Sie die folgenden Schritte aus, um die GUID einer Organisation abzurufen:
	
1. Melden Sie sich bei einer Region, einer Organisation und einem Bereich in {{site.data.keyword.Bluemix_notm}} an. Führen Sie den folgenden Befehl aus:

    ```
	bx login -a Endpoint
	```
	{: codeblock}
	
	Hierbei ist *Endpoint* (Endpunkt) die URL für die Anmeldung bei {{site.data.keyword.Bluemix_notm}}. Diese URL ist je nach Region unterschiedlich.
	
	<table>
	    <caption>Liste der Endpunkte für den Zugriff auf {{site.data.keyword.Bluemix_notm}}</caption>
		<tr>
		  <th>Region</th>
		  <th>URL</th>
		</tr>
		<tr>
		  <td>USA (Süden)</td>
		  <td>api.ng.bluemix.net</td>
		</tr>
		<tr>
		  <td>Vereinigtes Königreich</td>
		  <td>api.eu-gb.bluemix.net</td>
		</tr>
	</table>

    Führen Sie zum Beispiel den folgenden Befehl aus, um sich bei der Region 'USA (Süden)' anzumelden:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Befolgen Sie die Anweisungen. Geben Sie Ihre {{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise ein, wählen Sie eine Organisation und einen Bereich aus.
	
	Führen Sie für eingebundene Benutzer-IDs den folgenden Befehl aus und befolgen Sie die Anweisungen:
	
	```
    bx login -a https://api.ng.bluemix.net --sso
    ```
    {: codeblock}

2. Führen Sie den Befehl `bx iam org` aus, um die GUID der Organisation abzurufen. 

    ```
    bx iam org NAME --guid
    ```
    {: codeblock}
	
    Hierbei ist NAME der Name der {{site.data.keyword.Bluemix_notm}}-Organisation.        
		
## Wie erhalte ich die GUID eines Bereichs?
{: #space_guid}
	
Führen Sie die folgenden Schritte aus, um die GUID eines Bereichs abzurufen:
	
1. Melden Sie sich bei einer Region, einer Organisation und einem Bereich in {{site.data.keyword.Bluemix_notm}} an.Führen Sie den folgenden Befehl aus:

    ```
	bx login -a Endpoint
	```
	{: codeblock}
	
	Hierbei ist *Endpoint* (Endpunkt) die URL für die Anmeldung bei {{site.data.keyword.Bluemix_notm}}. Diese URL ist je nach Region unterschiedlich.
	
	<table>
	    <caption>Liste der Endpunkte für den Zugriff auf {{site.data.keyword.Bluemix_notm}}</caption>
		<tr>
		  <th>Region</th>
		  <th>URL</th>
		</tr>
		<tr>
		  <td>USA (Süden)</td>
		  <td>api.ng.bluemix.net</td>
		</tr>
		<tr>
		  <td>Vereinigtes Königreich</td>
		  <td>api.eu-gb.bluemix.net</td>
		</tr>
	</table>

    Führen Sie zum Beispiel den folgenden Befehl aus, um sich bei der Region 'USA (Süden)' anzumelden:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Befolgen Sie die Anweisungen. Geben Sie Ihre {{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise ein, wählen Sie eine Organisation und einen Bereich aus.
	
	Führen Sie für eingebundene Benutzer-IDs den folgenden Befehl aus und befolgen Sie die Anweisungen:
	
	```
    bx login -a https://api.ng.bluemix.net --sso
    ```
    {: codeblock}
	
2. Führen Sie den Befehl `bx iam space` aus, um die Bereichs-GUID abzurufen. 

    ```
    bx iam space NAME --guid
    ```
    {: codeblock}
	
    dabei ist NAME der Name eines {{site.data.keyword.Bluemix_notm}}-Bereichs. 
	
    Um beispielsweise die GUID für den Bereich *dev* zu erhalten, führen Sie den folgenden Befehl aus:
	
    ```
    bx iam space dev --guid
    e03afff1-3456-4af6-1234-59treg1b0abc
    ```
    {: screen}




		
		
