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



# Verlaufsprotokoll einer Regel abrufen
{: #retrieve_history}


Führen Sie die folgenden Schritte aus, um das Verlaufsprotokoll eines Alerts abzurufen:

1. Melden Sie sich bei einer Region, einer Organisation und einem Bereich von {{site.data.keyword.Bluemix_notm}} an. Führen Sie den folgenden Befehl aus:

    Führen Sie zum Beispiel den folgenden Befehl aus, um sich beim Bereich 'US South' anzumelden: 
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Befolgen Sie die Anweisungen. Geben Sie Ihre {{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise ein, wählen Sie eine Organisation und einen Bereich aus.

2. Rufen Sie das Authentifizierungstoken oder den API-Schlüssel ab.

    * Informationen zur IAM-Authentifizierung finden Sie in [IAM-Token über die Bluemix-Befehlzeilenschnittstelle abrufen](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) oder [IAM-API-Schlüssel über die Bluemix-Befehlzeilenschnittstelle generieren](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	                                          
	* Informationen zur UAA-Authentifizierung finden Sie in [UAA-Token über die Bluemix-Befehlzeilenschnittstelle abrufen](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) oder [UAA-Token über die REST-API abrufen](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

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
	
	**Hinweis:** Das Token schließt *Bearer* (Träger) aus
	
	
2. Rufen Sie die Bereichs-GUID ab. Die GUID muss das Präfix *s-* aufweisen, um einen Bereich zu kennzeichnen.

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
	
	Exportieren Sie anschließend die Variable *Space*:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. Führen Sie den folgenden cURL-Befehl aus, um das Verlaufsprotokoll eines Alerts abzurufen:

    ```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/history?rule=Rule_Name
	```
	{: codeblock}
	
	dabei gilt:
	
	* Das *X-Auth-User-Token* ist ein Parameter, der das UAA-Token, IAM-Token oder den API-Schlüssel von {{site.data.keyword.Bluemix_notm}} übergibt.
	
	* *Auth_Type* ist das Präfix, das den Typ des Tokens oder den API-Schlüssel definiert. Die folgende Liste enthält die gültigen Werte: *apikey*, *iam* oder *uaa*, wobei
  * *apikey* angibt, dass das Token ein API-Schlüssel ist.
		* *iam* angibt, dass das Token ein IAM-generiertes Token ist.
		* *uaa* angibt, dass das Token ein UAA-generiertes Token ist.
	
	* *X-Auth-Scope-Id* ist ein Parameter, der die Bereichs-GUID übergibt. Die GUID muss das Präfix *s-* aufweisen, um einen Bereich zu kennzeichnen. Dieser Header ist erforderlich, wenn Sie eine UAA-Tokenauthentifizierung verwenden. Wenn Sie ein IAM-Authentifizierungstoken verwenden, dann ist dieser Header optional und die Domäneninformationen, die an das Token gebunden sind, werden verwendet.
	
	* Token ist das UAA- oder IAM-Authentifizierungstoken oder der API-Schlüssel.
	
	* Space (Bereich) ist die GUID des Bereichs. Die GUID ist nur erforderlich, wenn Sie ein UAA-Token verwenden.
	
	* Rule_Name ist der Name der Regel, mit der der Alert ausgelöst wird. Der Wert ist der im Feld *name* festgelegte Wert.
	
    
Beispiel: Das Verlaufsprotokoll der Regel `highNginxCPU` ist nachfolgend dargestellt:

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


