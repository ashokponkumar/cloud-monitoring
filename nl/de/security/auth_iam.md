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


# IAM-Token abrufen
{: #auth_iam}

Verwenden Sie das {{site.data.keyword.Bluemix}} IAM-Modell, um ein Authentifizierungstoken abzurufen, das Sie verwenden können, um mit dem {{site.data.keyword.monitoringshort}}-Service zu arbeiten. Sie können das Authentifizierungstoken abrufen, indem Sie die {{site.data.keyword.Bluemix_notm}}-CLI verwenden.
{:shortdesc}

Das Token hat eine Ablaufzeit. 

## IAM-Token über die IBM Cloud-CLI abrufen 
{: #iam_token_cli}

Führen Sie die folgenden Schritte aus, um das Berechtigungstoken über die {{site.data.keyword.Bluemix_notm}}-CLI abzurufen:

1. (Pre-req) Installieren Sie die {{site.data.keyword.Bluemix_notm}}-CLI.

   Weitere Informationen finden Sie unter [{{site.data.keyword.Bluemix_notm}}-CLI installieren](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa).
   
   Wenn die CLI installiert ist, fahren Sie mit dem nächsten Schritt fort.
    
2. Melden Sie sich bei einer Region, einer Organisation und einem Bereich in {{site.data.keyword.Bluemix_notm}} an. 

    Weitere Informationen finden Sie unter [Wie melde ich mich bei {{site.data.keyword.Bluemix_notm}} an?](/docs/services/cloud-monitoring/qa/cli_qa.html#login).
	
3. Führen Sie den Befehl `bx iam oauth-tokens` aus, um das IAM-Token abzurufen.

    ```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	Die Ausgabe gibt das IAM-Token zurück.

**Hinweis:** Wenn Sie das Token verwenden, entfernen Sie *Bearer* (Träger) aus dem Wert des Tokens, das Sie in API-Aufrufen übergeben.
		



	

	
	
	
	
	
	
