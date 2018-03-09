---

copyright:
  years: 2017, 2018

lastupdated: "2018-02-07"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Verlaufsprotokoll eines Alerts mithilfe der Alert-API abrufen
{: #retrieve_history}

Verwenden Sie die Alert-API, um das Verlaufsprotokoll eines Alerts abzurufen.
{:shortdesc}


Führen Sie die folgenden Schritte aus, um das Verlaufsprotokoll eines Alerts mithilfe der Alert-API abzurufen:

1. Melden Sie sich bei einer Region, einer Organisation und einem Bereich in {{site.data.keyword.Bluemix_notm}} an. 

    Weitere Informationen finden Sie unter [Wie melde ich mich bei {{site.data.keyword.Bluemix_notm}} an?](/docs/services/cloud-monitoring/qa/cli_qa.html#login).

2. Rufen Sie das Sicherheitstoken ab. Sie können ein UAA-Token, ein IAM-Token oder einen API-Schlüssel verwenden. Wählen Sie eine der folgenden Methoden aus, um das Sicherheitstoken abzurufen:
	
	* Informationen zum Abrufen eines UAA-Tokens finden Sie unter [UAA-Token über die {{site.data.keyword.Bluemix_notm}}-CLI abrufen](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli).
	
	* Informationen zum Abrufen eines IAM-Tokens finden Sie unter [IAM-Token über die {{site.data.keyword.Bluemix_notm}}-CLI abrufen](/docs/services/cloud-monitoring/security/auth_iam.html#auth_iam).
	
	* Informationen zum Abrufen eines API-Schlüssels finden Sie unter [API-Schlüssel abrufen](/docs/services/cloud-monitoring/security/auth_api_key.html#auth_api_key).
	
	Legen Sie auf demselben Terminal, von dem aus Sie sich bei {{site.data.keyword.Bluemix_notm}} angemeldet haben, die Token-Variable für das Token fest.

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
	
	Anschließend exportieren Sie die Variable *Token*:
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Hinweis:** Das Token schließt *Bearer* (Träger) aus.
	
3. Rufen Sie die Bereichs-GUID ab. Die GUID muss das Präfix *s-* aufweisen, um einen Bereich zu kennzeichnen.

    Führen Sie den folgenden Befehl aus:
	
	```
	bx iam space SpaceName --guid
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
	
	Exportieren Sie dann die Variable *Space*. **Hinweis:** Die GUID muss das Präfix *s-* aufweisen, um einen Bereich zu kennzeichnen.
	
	Beispiel:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. Rufen Sie das Verlaufsprotokoll einer Regel ab.

    * Informationen zum Abrufen des Verlaufsprotokolls einer Regel nach ihrem Namen finden Sie unter [Verlaufsprotokoll einer Regel über ihren Namen abrufen](/docs/services/cloud-monitoring/alerts/retrieve_history.html#by_name).
	* Informationen zum Abrufen des Verlaufsprotokolls einer Regel über einen bestimmten Zeitraum finden Sie unter [Verlaufsprotokoll einer Regel für die letzten 48 Stunden abrufen](/docs/services/cloud-monitoring/alerts/retrieve_history.html#by_time).
	* Informationen zum Abrufen einer Anzahl von Einträgen aus dem Verlaufsprotokoll einer Regel finden Sie unter [Letzte 3 Einträge im Verlaufsprotokoll einer Regel abrufen](/docs/services/cloud-monitoring/alerts/retrieve_history.html#number).
	
	
## Verlaufsprotokoll einer Regel nach Regelnamen abrufen
{: #by_name}
	
Führen Sie den folgenden cURL-Befehl aus, um das Verlaufsprotokoll eines Alerts abzurufen:

```
curl -XGET --header "X-Auth-User-Token: Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" METRICS_ENDPOINT/v1/alert/history?rule_name=RULE_NAME
```
{: codeblock}
	
wobei gilt:
	
* *X-Auth-User-Token* ist ein Parameter, der das UAA-Token, das IAM-Token oder den API-Schlüssel übergibt.
	
* *Auth_Type* ist das Präfix, das den Typ des Tokens oder den API-Schlüssel definiert. Die folgende Liste enthält die gültigen Werte: *apikey*, *iam* oder *uaa*, wobei
  * *apikey* angibt, dass das Token ein API-Schlüssel ist.
	* *iam* angibt, dass das Token ein IAM-generiertes Token ist.
	* *uaa* angibt, dass das Token ein UAA-generiertes Token ist.
	
* *X-Auth-Scope-Id* ist ein Parameter, der die Bereichs-GUID übergibt. Die GUID muss das Präfix *s-* aufweisen, um einen Bereich zu kennzeichnen. Dieser Header ist erforderlich, wenn Sie eine UAA-Tokenauthentifizierung verwenden. Wenn Sie ein IAM-Authentifizierungstoken verwenden, dann ist dieser Header optional und die Domäneninformationen, die an das Token gebunden sind, werden verwendet.
	
* *Token* ist das UAA- oder IAM-Authentifizierungstoken oder der API-Schlüssel.
	
* *Space* ist die GUID des Bereichs. 
	
* *RULE_NAME* ist der Name der Regel, die zum Auslösen des Alerts verwendet wird. Der Wert ist der im Feld *name* festgelegte Wert.
	
* *METRICS_ENDPOINT* stellt den Eingangspunkt für den Service dar. Jede Region verfügt über eine andere URL. Informationen zum Abrufen der Liste der Endpunkte nach Region finden Sie unter [Endpunkte](/docs/services/cloud-monitoring/send_retrieve_metrics_ov.html#endpoints).
	
    
Beispiel: Das Verlaufsprotokoll der Regel `highNginxCPU` ist nachfolgend dargestellt:

```
curl -XGET --header "X-Auth-User-Token: iam ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/history?rule_name=highNginxCPU
```
{: screen}

```
[
 {
 "rule": "highNginxCPU",
 "timestamp": "2017-04-17T22.23.21.000",
 "value": 99.5,
 "from_level": "OK",
 "to_level": "ERROR"
 },
 {
 "rule": "highNginxCPU",
 "timestamp": "2017-04-17T10.11.12.123",
 "value": 98.5,
 "from_level" : "OK",
 "to_level": "WARN"
 },
 {
 "rule": "highNginxCPU",
 "timestamp": "2017-04-17T10.12.14.000",
 "value": 97.0,
 "from_level" : "WARN",
 "to_level": "OK"
 }
]
```
{: screen}


## Verlaufsprotokoll einer Regel für die letzten 48 Stunden abrufen
{: #by_time}

Führen Sie den folgenden cURL-Befehl aus, um das Verlaufsprotokoll eines Alerts für die letzten 48 Stunden abzurufen:

```
curl -H "X-Auth-User-Token: iam $Token" -H "X-Auth-Scope-Id:$Space" METRICS_ENDPOINT/v1/alert/history?start=-48h
```
{: codeblock}

Dabei gilt Folgendes:
	
* *X-Auth-User-Token* ist ein Parameter, der das UAA-Token, das IAM-Token oder den API-Schlüssel übergibt.
	
* *Auth_Type* ist das Präfix, das den Typ des Tokens oder den API-Schlüssel definiert. Die folgende Liste enthält die gültigen Werte: *apikey*, *iam* oder *uaa*, wobei
  * *apikey* angibt, dass das Token ein API-Schlüssel ist.
	* *iam* angibt, dass das Token ein IAM-generiertes Token ist.
	* *uaa* angibt, dass das Token ein UAA-generiertes Token ist.
	
* *X-Auth-Scope-Id* ist ein Parameter, der die Bereichs-GUID übergibt. Die GUID muss das Präfix *s-* aufweisen, um einen Bereich zu kennzeichnen. Dieser Header ist erforderlich, wenn Sie eine UAA-Tokenauthentifizierung verwenden. Wenn Sie ein IAM-Authentifizierungstoken verwenden, dann ist dieser Header optional und die Domäneninformationen, die an das Token gebunden sind, werden verwendet.
	
* *Token* ist das UAA- oder IAM-Authentifizierungstoken oder der API-Schlüssel.
	
* *Space* ist die GUID des Bereichs. 
	
* *RULE_NAME* ist der Name der Regel, die zum Auslösen des Alerts verwendet wird. Der Wert ist der im Feld *name* festgelegte Wert.
	
* *METRICS_ENDPOINT* stellt den Eingangspunkt für den Service dar. Jede Region verfügt über eine andere URL. Informationen zum Abrufen der Liste der Endpunkte nach Region finden Sie unter [Endpunkte](/docs/services/cloud-monitoring/send_retrieve_metrics_ov.html#endpoints).
	

## Letzte 3 Einträge im Verlaufsprotokoll einer Regel abrufen
{: #number}

Führen Sie den folgenden cURL-Befehl aus, um die letzten 3 Einträge im Verlaufsprotokoll eines Alerts abzurufen:

```
curl -H "X-Auth-User-Token: iam $Token" -H "X-Auth-Scope-Id:$Space" METRICS_ENDPOINT/v1/alert/history?max_results=3
```
{: codeblock}

Dabei gilt Folgendes:
	
* *X-Auth-User-Token* ist ein Parameter, der das UAA-Token, das IAM-Token oder den API-Schlüssel übergibt.
	
* *Auth_Type* ist das Präfix, das den Typ des Tokens oder den API-Schlüssel definiert. Die folgende Liste enthält die gültigen Werte: *apikey*, *iam* oder *uaa*, wobei
  * *apikey* angibt, dass das Token ein API-Schlüssel ist.
	* *iam* angibt, dass das Token ein IAM-generiertes Token ist.
	* *uaa* angibt, dass das Token ein UAA-generiertes Token ist.
	
* *X-Auth-Scope-Id* ist ein Parameter, der die Bereichs-GUID übergibt. Die GUID muss das Präfix *s-* aufweisen, um einen Bereich zu kennzeichnen. Dieser Header ist erforderlich, wenn Sie eine UAA-Tokenauthentifizierung verwenden. Wenn Sie ein IAM-Authentifizierungstoken verwenden, dann ist dieser Header optional und die Domäneninformationen, die an das Token gebunden sind, werden verwendet.
	
* *Token* ist das UAA- oder IAM-Authentifizierungstoken oder der API-Schlüssel.
	
* *Space* ist die GUID des Bereichs. 
	
* *RULE_NAME* ist der Name der Regel, die zum Auslösen des Alerts verwendet wird. Der Wert ist der im Feld *name* festgelegte Wert.
	
* *METRICS_ENDPOINT* stellt den Eingangspunkt für den Service dar. Jede Region verfügt über eine andere URL. Informationen zum Abrufen der Liste der Endpunkte nach Region finden Sie unter [Endpunkte](/docs/services/cloud-monitoring/send_retrieve_metrics_ov.html#endpoints).
	
