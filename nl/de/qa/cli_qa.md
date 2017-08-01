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


# Häufige Fragen und Antworten bei der Verwendung der Bluemix-CLI
{: #cli_qa}

Hier sind die Antworten auf häufig gestellte Fragen zur Verwendung der {{site.data.keyword.Bluemix}}-CLI mit dem {{site.data.keyword.monitoringshort}}-Service. 
{:shortdesc}

* [ Wie installiere ich die {{site.data.keyword.Bluemix_notm}}-Befehlszeilenschnittstelle?](#install_bmx_cli)
* [Wie erhalte ich die GUID eines Kontos?](#account_guid)
* [Wie erhalte ich die GUID einer Organisation?](#org_guid)
* [Wie erhalte ich die GUID eines Bereichs?](#space_guid)


## Wie installiere ich die Bluemix-Benutzerschnittstelle?
{: #install_bmx_cli}

Führen Sie die folgenden Schritte aus, um die {{site.data.keyword.Bluemix_notm}}-Benutzerschnittstelle zu installieren:

1. Laden Sie die Benutzerschnittstelle herunter.

    Beispiel: Zur Installation des {{site.data.keyword.Bluemix_notm}}-CLI-Pakets auf einem Ubuntu-System laden Sie das [{{site.data.keyword.Bluemix_notm}}-CLI-Paket ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](http://clis.ng.bluemix.net/ui/home.html "Symbol für externen Link"){: new_window} herunter. 

2. Führen Sie den folgenden Befehl aus, um das {{site.data.keyword.Bluemix_notm}}-CLI-Paket zu extrahieren:
    
    ```
    tar -xvf Bluemix_CLI_0.5.4_amd64.tar.gz
    ```
    {: codeblock}
    
3. Wechseln Sie in das Verzeichnis *Bluemix_CLI* und führen Sie den Befehl `./install_bluemix_cli` mit der Rootberechtigung aus, um das CF-Plug-in zu installieren. Sie können den Befehl als Rootbenutzer ausführen oder den Befehl sudo verwenden, um die Rootberechtigung zu erhalten. Beispiel:
    
    ```
    cd Bluemix_CLI
    ```
    {: codeblock}
    
    ```
    sudo ./install_bluemix_cli
    ```
    {: codeblock}
    
4. Überprüfen Sie die Installation des CF-Plug-ins. Prüfen Sie zum Beispiel die Version des CF-Plug-ins. Führen Sie folgenden Befehl aus:
    
    ```
    cf -v
    ```
    {: codeblock}
    
    Die Ausgabe sieht wie folgt aus:
    
    ```
    cf version 6.25.0+787326d.2017-02-28
    ```
    {: screen}
    
5. Überprüfen Sie die Installation der {{site.data.keyword.Bluemix_notm}}-CLI. Überprüfen Sie zum Beispiel die Version des Plug-ins. Führen Sie folgenden Befehl aus:
    
    ```
    bx -version
    ```
    {: codeblock}
    
    Die Ausgabe sieht wie folgt aus:
    
    ```
    bx version 0.5.4+ae22935-2017-05-18T03:55:55+00:00
    ```
    {: screen}
	
## Wie erhalte ich die GUID eines Kontos?
{: #account_guid}
	
Führen Sie die folgenden Schritte aus, um die GUID eines Kontos zu erhalten:
	
1. Melden Sie sich bei einer Region, einer Organisation und einem Bereich von {{site.data.keyword.Bluemix_notm}} an. Führen Sie den folgenden Befehl aus:

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    Befolgen Sie die Anweisungen. Geben Sie Ihre {{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise ein, wählen Sie eine Organisation und einen Bereich aus.
	
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
	
1. Melden Sie sich bei einer Region, einer Organisation und einem Bereich von {{site.data.keyword.Bluemix_notm}} an. Führen Sie den folgenden Befehl aus:

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    Befolgen Sie die Anweisungen. Geben Sie Ihre {{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise ein, wählen Sie eine Organisation und einen Bereich aus.

2. Führen Sie den Befehl `bx iam org` aus, um die GUID der Organisation zu erhalten. 

    ```
    bx iam org NAME --guid
    ```
    {: codeblock}
	
    dabei ist NAME der Name der {{site.data.keyword.Bluemix_notm}}-Organisation.
		
## Wie erhalte ich die GUID eines Bereichs?
{: #space_guid}
	
Führen Sie die folgenden Schritte aus, um die GUID eines Bereichs abzurufen:
	
1. Melden Sie sich bei einer Region, einer Organisation und einem Bereich von {{site.data.keyword.Bluemix_notm}} an. Führen Sie den folgenden Befehl aus:

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    Befolgen Sie die Anweisungen. Geben Sie Ihre {{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise ein, wählen Sie eine Organisation und einen Bereich aus.
	
2. Führen Sie den Befehl `bx iam space` aus, um die GUID des Bereichs abzurufen. 

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




		
		
