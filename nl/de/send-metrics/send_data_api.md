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


# Daten an den Überwachungsservice senden
{: #send_data_api}

Sie können Metriken an den {{site.data.keyword.monitoringshort}}-Service senden, indem Sie die Metrik-API verwenden. Sie können Metriken an einen Bereich von {{site.data.keyword.Bluemix_notm}} senden.
{:shortdesc}


Bei {{site.data.keyword.Bluemix_notm}} Docker-Containern werden die Systemmetriken automatisch erfasst. Bei Cloud Foundry-Anwendungen und Apps, die auf einer virtuellen Maschine ausgeführt werden, müssen Metriken direkt von der App über die [Metrik-API](https://console.bluemix.net/apidocs/927-ibm-cloud-monitoring-rest-api?&language=node#introduction){: new_window} gesendet werden. 



## Metriken mithilfe der UAA an einen Bereich senden
{: #uaa}

Führen Sie die folgenden Schritte aus, um Metriken an einen {{site.data.keyword.Bluemix_notm}}-Bereich zu senden:

1. Melden Sie sich bei einer Region, einer Organisation und einem Bereich von {{site.data.keyword.Bluemix_notm}} an. Führen Sie den folgenden Befehl aus:

    Führen Sie zum Beispiel den folgenden Befehl aus, um sich beim Bereich 'US South' anzumelden: 
	
	```
    cf login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Befolgen Sie die Anweisungen. Geben Sie Ihre {{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise ein, wählen Sie eine Organisation und einen Bereich aus.

2. Rufen Sie das Authentifizierungstoken ab.

    Weitere Informationen zur UAA-Authentifizierung finden Sie in [UAA-Token mithilfe der Bluemix-CLI abrufen](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) oder [UAA-Token mithilfe der REST-API abrufen](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

	Führen Sie zum Beispiel den folgenden Befehl aus, um das UAA-Token zu verwenden:

    ```
	cf oauth-token
	```
	{: codeblock}
	
	Das Ergebnis dieses Befehls lautet wie folgt:
	
	```
	bearer eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8
	```
	{: screen}
	
	Exportieren Sie dann die Variable *Token*. Beispiel:
	
	```
	export Token="eyJhbGciOiJI....cGlxryWS8"
	```
	{: screen}
	
	**Hinweis:** Das Token schließt *Bearer* (Träger) aus
		
2. Rufen Sie die Bereichs-GUID ab. Die GUID muss das Präfix *s-* aufweisen, um einen Bereich zu kennzeichnen.

    Führen Sie den folgenden Befehl aus:
	
	```
	cf space SpaceName --guid
	```
	{: codeblock}
	
	Dabei ist *SpaceName* der Name des Bereichs, in dem Sie eine Benachrichtigung definieren werden.
	
	Beispiel: Führen Sie folgenden Befehl aus, um die GUID eines Bereichs mit dem Namen *dev* abzurufen:
	
	```
	cf space dev --guid
	```
	{: screen}
	
	Das Ergebnis dieses Befehls lautet wie folgt:
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	Exportieren Sie dann die Variable *Space*. Beispiel:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. Führen Sie den folgenden cURL-Befehl aus, um Metriken zu senden:

    ```
	curl -XPOST -d @Metric_File --header "X-Auth-User-Token:uaa ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics
	```
	{: codeblock}
	
	dabei gilt:
	
	* Metrics_File stellt die JSON-Datei dar, die die Definition der Metriken enthält, die an den {{site.data.keyword.monitoringshort}}-Service gesendet wurden.
	
	* *X-Auth-User-Token* ist ein Parameter, der das UAA-Token von {{site.data.keyword.Bluemix_notm}} übergibt.
	
	* *Auth_Type* ist das Präfix, das den Typ des Tokens definiert. *uaa* gibt an, dass das angegebene Token ein generiertes UAA-Token ist.
	
	* *X-Auth-Scope-Id* ist ein Parameter, der die Bereichs-GUID übergibt. Die GUID muss das Präfix *s-* aufweisen, um einen Bereich zu kennzeichnen.
	
	* Token stellt das UAA-Token dar.
	
	* Space (Bereich) stellt die GUID des Bereichs dar. 
	
	Sie können zum Beispiel den folgenden Befehl verwenden, um die beiden Metriken `myhost.cpu.idle` und `myapp.login.attempts` an den {{site.data.keyword.monitoringshort}}-Service zu senden:
	
	```
	curl -XPOST @metric.json --header "X-Auth-User-Token:uaa ${Token}" https://metrics.ng.bluemix.net/v1/metrics
	```
	{: screen}
	
	Die Beispieldatei *metric.json* ist wie folgt definiert:

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

 
## Metriken mithilfe von IAM oder eines API-Schlüssels an einen Bereich senden.
{: #iam}

Führen Sie die folgenden Schritte aus, um Metriken an einen {{site.data.keyword.Bluemix_notm}}-Bereich zu senden:

1. Melden Sie sich bei einer Region, einer Organisation und einem Bereich von {{site.data.keyword.Bluemix_notm}} an. Führen Sie den folgenden Befehl aus:

    Führen Sie zum Beispiel den folgenden Befehl aus, um sich beim Bereich 'US South' anzumelden:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Befolgen Sie die Anweisungen. Geben Sie Ihre {{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise ein, wählen Sie eine Organisation und einen Bereich aus.

2. Rufen Sie das Authentifizierungstoken oder den API-Schlüssel ab.

    Weitere Informationen zur IAM-Authentifizierung finden Sie in [IAM-Token mithilfe der Bluemix-CLI abrufen](/docs/services/cloud-monitoring/security/auth_iam.html#iam_cli) oder [Einen IAM API-Schlüssel mithilfe der Bluemix-UI generieren.](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).

	Führen Sie zum Beispiel den folgenden Befehl aus, um das IAM-Token zu verwenden:

    ```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	Das Ergebnis dieses Befehls lautet wie folgt:
	
	```
	IAM token:  Bearer djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ
    UAA token:  Bearer eyJhbGciOiJIUz..Ky6vagp3k_QcIcKJ-td83qXhO5Uze43KcplG6PzcGs8
	```
	{: screen}
	
	Exportieren Sie dann die Variable *Token*. Beispiel:
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Hinweis:** Das Token schließt *Bearer* (Träger) aus
		
3. Rufen Sie die Bereichs-GUID ab.

    Um die Bereichs-GUID abzurufen, siehe [Wie erhalte ich die GUID eines Bereichs?](/docs/service/cloud-monitoring/qa/cli_qa.html#space_guid). Die GUID muss das Präfix *s-* aufweisen, um einen Bereich zu kennzeichnen.
    
    Führen Sie zum Beispiel den folgenden Befehl aus:
	
	```
	bx iam space NAME --guid
	```
	{: codeblock}
	
	Dabei ist *SpaceName* der Name des Bereichs, in dem Sie eine Benachrichtigung definieren werden.
	
	Beispiel: Führen Sie folgenden Befehl aus, um die GUID eines Bereichs mit dem Namen *dev* abzurufen:
	
	```
	bx iam space dev --guid
	```
	{: screen}
	
	Das Ergebnis dieses Befehls lautet wie folgt:
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	Exportieren Sie dann die Variable *Space*. Beispiel:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. Führen Sie einen cURL-Befehl aus, um Metriken zu senden.

    ```
	curl -XPOST -d @Metric_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics
	```
	{: codeblock}
	
	dabei gilt:
	
	* Metrics_File stellt die JSON-Datei dar, die die Definition der Metriken enthält, die an den {{site.data.keyword.monitoringshort}}-Service gesendet wurden.
	
	* *X-Auth-User-Token* ist ein Parameter, der das {{site.data.keyword.Bluemix_notm}} IAM-Token oder den API-Schlüssel übergibt.
	
	* *Auth_Type* ist das Präfix, das den Typ des Tokens oder den API-Schlüssel definiert. Die folgende Liste enthält die gültigen Werte: *apikey*, *iam* oder *uaa*, wobei
  * *apikey* angibt, dass das Token ein API-Schlüssel ist.
		* *iam* angibt, dass das Token ein IAM-generiertes Token ist.
	
	* *X-Auth-Scope-Id* ist ein Parameter, der die Bereichs-GUID übergibt. Die GUID muss das Präfix *s-* aufweisen, um einen Bereich zu kennzeichnen. 
	
	* Token stellt das IAM-Token oder den API-Schlüssel dar.
	
	* Space (Bereich) stellt die GUID des Bereichs dar. 

	
Sie können zum Beispiel den folgenden Befehl ausführen, um zwei Metriken, `myhost.cpu.idle` und `myapp.login.retries`, an einen Bereich im {{site.data.keyword.monitoringshort}}-Service zu senden:
	
```
curl -XPOST @metric.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/metrics
```
{: screen}
	
Die Beispieldatei *metric.json* ist wie folgt definiert:

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


















 
