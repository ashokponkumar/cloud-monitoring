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


# Authentifizierung mit dem Bluemix IAM-Modell
{: #auth_iam}

Verwenden Sie das {{site.data.keyword.Bluemix}} IAM-Modell, um ein Authentifizierungstoken abzurufen, das Sie für den Zugriff auf Metriken verwenden können, die im {{site.data.keyword.monitoringshort}}-Service gespeichert haben oder um einen API-Schlüssel abzurufen. Das Token hat eine Ablaufzeit. Der API-Schlüssel läuft nicht ab.
{:shortdesc}


## IAM-Token mithilfe der Bluemix-CLI abrufen 
{: #iam_token_cli}

Führen Sie die folgenden Schritte aus, um das Berechtigungstoken über die {{site.data.keyword.Bluemix_notm}}-CLI abzurufen:

1. Installieren Sie die {{site.data.keyword.Bluemix_notm}}-CLI.

   Weitere Informationen finden Sie in [{{site.data.keyword.Bluemix_notm}}-Befehlszeilenschnittstelle installieren](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa).
   
   Wenn die CLI installiert ist, fahren Sie mit dem nächsten Schritt fort.
    
2. Melden Sie sich bei einer Region, einer Organisation und einem Bereich von {{site.data.keyword.Bluemix_notm}} an. Führen Sie den folgenden Befehl aus:

    Führen Sie zum Beispiel den folgenden Befehl für den Bereich 'US South' aus: 
	
    ```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Befolgen Sie die Anweisungen. Geben Sie Ihre {{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise ein, wählen Sie eine Organisation und einen Bereich aus.
	
3. Führen Sie den Befehl `bx iam oauth-tokens` aus, um das IAM-Token abzurufen.

    ```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	Die Ausgabe gibt das IAM-Token zurück, das Sie für die Authentifizierung Ihrer Benutzer-ID in dem betreffenden Bereich und der betreffenden Organisation verwenden müssen.

**Hinweis:** Entfernen Sie bei der Verwendung des Tokens *Bearer* aus dem Wert des Tokens, der in einem API-Aufruf übergeben wird.
		
		
## IAM-API-Schlüssel über die Bluemix-Befehlszeilenschnittstelle generieren
{: #iam_apikey_cli}

Führen Sie die folgenden Schritte aus, um einen IAM-API-Schlüssel über die {{site.data.keyword.Bluemix_notm}}-Befehlszeilenschnittstelle zu generieren:

1. Installieren Sie die Befehlszeilenschnittstelle von {{site.data.keyword.Bluemix_notm}}.

   Weitere Informationen finden Sie in [{{site.data.keyword.Bluemix_notm}}-Befehlszeilenschnittstelle installieren](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa).

   Wenn die CLI installiert ist, fahren Sie mit dem nächsten Schritt fort.
	
2. Melden Sie sich bei {{site.data.keyword.Bluemix_notm}} an, indem Sie den CLI-Befehl `bx login` von {{site.data.keyword.Bluemix_notm}} verwenden:

    Führen Sie zum Beispiel den folgenden Befehl für den Bereich 'US South' aus:
	
    ```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Befolgen Sie die Anweisungen. Geben Sie Ihre {{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise ein, wählen Sie eine Organisation und einen Bereich aus.
 
3. Führen Sie den Befehl `bx iam api-key-create` aus, um einen IAM API-Schlüssel zu erstellen.

    ```
    bx iam api-key-create NAME [-d DESCRIPTION][-f, --file FILE]
	```
	{: codeblock} 
	
	dabei gilt:
	
	* NAME ist der Name des zu erstellenden API-Schlüssels.
	* DESCRIPTION ist der Text, den Sie für die Beschreibung des API-Schlüssels verwenden.
	* FILE ist der Name der Datei, in der der Schlüssel gespeichert wird.
	
    Führen Sie zum Beispiel folgenden Befehl aus, um den API-Schlüssel *MyKey* zu erstellen:
	
	```
	bx iam api-key-create MyKey -d "This is my API key for service X" 
	```
	{: screen}
	
	
	
	
## IAM-API-Schlüssel über die Bluemix-Benutzerschnittstelle generieren
{: #iam_apikey_ui}

Führen Sie die folgenden Schritte aus, um einen IAM-API-Schlüssel über die {{site.data.keyword.Bluemix_notm}}-Benutzerschnittstelle zu generieren:

1. Melden Sie sich an der {{site.data.keyword.Bluemix_notm}}-Konsole an. 

2. Klicken Sie in der Menüleiste der Konsole auf **Verwalten > Sicherheit > Bluemix API-Schlüssel**.

3. Klicken Sie auf **API-Schlüssel erstellen**.

2. Geben Sie einen Namen und eine Beschreibung für Ihren API-Schlüssel ein. Klicken Sie anschließend auf **API-Schlüssel erstellen**.

5. Speichern Sie den API-Schlüssel. Klicken Sie auf **API-Schlüssel herunterladen**.

    Klicken Sie auf **Anzeigen**, um den API-Schlüssel anzuzeigen.  

**Hinweis:** Der API-Schlüssel ist während der Erstellung nur zum Anzeigen oder Herunterladen verfügbar. Wenn der API-Schlüssel nicht mehr vorhanden ist, müssen Sie einen neuen API-Schlüssel erstellen.  


	
## API-Schlüssel über die Bluemix-Benutzerschnittstelle widerrufen
{: #revoke_ui}
	
Führen Sie folgende Schritte aus, um einen IAM-API-Schlüssel über die {{site.data.keyword.Bluemix_notm}}-Benutzerschnittstelle zu widerrufen:

1. Melden Sie sich an der {{site.data.keyword.Bluemix_notm}}-Konsole an.

2. Klicken Sie in der Menüleiste der Konsole auf **Verwalten > Sicherheit > Bluemix API-Schlüssel**.

3. Klicken Sie auf **Aktionen** und dann auf **Schlüssel löschen**.
