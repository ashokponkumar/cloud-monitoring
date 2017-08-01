---

copyright:
  years: 2017

lastupdated: "2017-07-18"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Mithilfe der Alerts-API mit Benachrichtigungen arbeiten
{: #notifications}

Verwenden Sie die Alerts-API, um eine Benachrichtigung zu erstellen, zu löschen oder zu aktualisieren, um die Details einer Benachrichtigung anzuzeigen und um die Benachrichtigungen aufzulisten, die in Ihrem {{site.data.keyword.Bluemix_notm}}-Bereich definiert sind.
{:shortdesc}

## Benachrichtigungsvorlage erstellen
{: #template}

Eine Benachrichtigung ist eine JSON-Datei. 

Sie können eine beliebige Anzahl an Benachrichtigungsvorlagen erstellen und diese dann verwenden, um Benachrichtigungen dieses Typs in Ihrer Organisation zu erstellen. 

Sie können einen beliebigen der folgenden Typen von Benachrichtigungen definieren:

* Email: Definieren Sie eine Benachrichtigung vom Typ *Email*, um eine E-Mail an eine gültige E-Mail-Adresse zu senden. 
* Webhook: Definieren Sie eine Benachrichtigung vom Typ *Webhook* nur für HTTPS-Endpunkte. Fügen Sie einen Parameter zum Endpunkt hinzu, um die Wahrscheinlichkeit zu verringern, dass ein anderer Benutzer Ihren Endpunkt aufzurufen versucht.
* Pagerduty: Definieren Sie eine Benachrichtigung vom Typ *PagerDuty*, um die Alertdaten für eine Metrik an Ihr PagerDuty Incident Management System zu senden. 

Die folgende Tabelle enthält Beispiele für Benachrichtigungsvorlagen:

<table>
  <caption></caption>
  <tr>
    <th>Typ</th>
	<th>Vorlage</th>
	<th>Beispiel</th>
  </tr>
  <tr>
    <td>E-Mail</td>
	<td>
	```
	{
	"name": "Template_Name",
	"type": "Email",
	"description" : "Description",
	"detail": "EmailAddress"
	}
	```
	{: codeblock}
	</td>
	<td>
	```
	{
	"name": "my-email",
	"type": "Email",
	"description" : "Send email notification when there is an infrastructure problem.",
	"detail": "xxx@yyy.com"
	}
	```
	{: screen}
	</td>
  </tr>
  <tr>
    <td>Web-Hook</td>
	<td>
	```
	{
	"name": "Template_Name",
	"type": "Webhook",
	"description" : "Description",
	"detail": "Endpoint"
	}
	```
	{: codeblock}
	</td>
	<td>
	```
	{
	"name": "my-webhook",
	"type": "Webhook",
	"description" : "Fire a webhook when there is an infrastructure problem..",
	"detail": "https://myendpoint.bluemix.net?key=abcd1234"
	}
	```
	{: screen}
	</td>
  </tr>
  <tr>
    <td>PagerDuty</td>
	<td>
	```
	{
	"name": "Template_Name",
	"type": "PagerDuty",
	"description" : "Description",
	"detail": "Pagerduty_APIkey"
	}
	```
	{: codeblock}
	</td>
	<td>
	```
	{
	"name": "my-pagerduty",
	"type": "PagerDuty",
	"description" : "Fire a PagerDuty alert when there is an infrastructure problem..",
	"detail": "abcd1234"
	}
	```
	{: screen}
	</td>
  </tr>
</table>

Dabei gilt:

* *Template_Name* definiert den Namen der Benachrichtigungsvorlage.
* *Description* erläutert, wann dieser Typ von Benachrichtigung verwendet wird.
* *EmailAddress* definiert die E-Mail-Adresse des Empfängers der Benachrichtigung.
* *Endpoint* definiert die URL, bei der der POST vorgenommen werden soll. Dies ist eine URL, die POST-Anforderungen annehmen kann. 
* *Pagerduty_APIkey* definiert einen eindeutigen API-Schlüssel. Dieser API-Schlüssel wird vom Administrator oder Eigner eines PagerDuty-Kontos generiert.


Führen Sie die folgenden Schritte aus, um eine Benachrichtigungsvorlage zu erstellen:

1. Erstellen Sie ein Verzeichnis zum Speichern der {{site.data.keyword.monitoringshort}}-Serviceressourcen, z. B. *cloud-monitoring*.

    Führen Sie unter dem Betriebssystem Ubuntu zum Beispiel den folgenden Befehl aus:
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
2. Erstellen Sie ein Verzeichnis zum Speichern Ihrer Benachrichtigungsvorlagen, z. B. *notification-templates*.

    Führen Sie unter dem Betriebssystem Ubuntu zum Beispiel den folgenden Befehl aus:
	
	```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	Wechseln Sie in dieses Verzeichnis:
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
3. Erstellen Sie eine Benachrichtigungsvorlage in einem lokalen Verzeichnis auf Ihrem System.

    Erstellen Sie zum Beispiel die Datei **email-template.json** für eine E-Mail-Benachrichtigungsvorlage. Verwenden Sie dazu beispielsweise den Editor vi: 
	
	```
	{
    "name": "email_template",
    "type": "Email",
    "description" : "Send email to manager of department X when ....",
    "detail": "xxx@yyy"
    }
	```
	{: codeblock}
	


## Eine Benachrichtigung erstellen
{: #create}

Führen Sie die folgenden Schritte aus, um eine Benachrichtigung zu erstellen:

1. Erstellen Sie eine Benachrichtigungsdatei.

    1. Verwenden Sie eine Benachrichtigungsvorlage zum Erstellen der Datei. Weitere Informationen finden Sie in [Benachrichtigungsvorlage erstellen](#template).
	
	2. Aktualisieren Sie die Datei:
	
	    * Geben Sie einen eindeutigen Namen in das Feld *Name* ein. Der Wert für dieses Feld wird später für die Aktualisierung, das Löschen und das Anzeigen von Details für die Benachrichtigung verwendet.
		* Geben Sie eine Beschreibung ein.
		* Geben Sie eine gültige E-Mail-Adresse, einen URL-Endpunkt oder einen PagerDuty-Schlüssel ein.
		
	3. Speichern Sie die Datei. Verwenden Sie beispielsweise ein Präfix wie *s-* für Benachrichtigungen, die Sie für Abfragen definieren, die in einem {{site.data.keyword.Bluemix_notm}}-Bereich ausgeführt werden.
	
	    **Tipp:** Erstellen Sie Ihre Benachrichtigungsdatei im Verzeichnis *~/cloud-monitoring/notifications/*, um die Ressourcen des {{site.data.keyword.monitoringshort_notm}}-Service in einer Gruppe zusammenzufassen. 
	
2. Melden Sie sich bei einer Region, einer Organisation und einem Bereich von {{site.data.keyword.Bluemix_notm}} an. Führen Sie den folgenden Befehl aus:

    Führen Sie zum Beispiel den folgenden Befehl aus, um sich beim Bereich 'US South' anzumelden: 
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Befolgen Sie die Anweisungen. Geben Sie Ihre {{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise ein, wählen Sie eine Organisation und einen Bereich aus.

3. Rufen Sie das Authentifizierungstoken oder den API-Schlüssel ab.

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
		
4. Rufen Sie die Bereichs-GUID ab. Die GUID muss das Präfix *s-* aufweisen, um einen Bereich zu kennzeichnen.

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
	
5. Führen Sie den folgenden cURL-Befehl aus, um eine Benachrichtigung zu erstellen:

    ```
	curl -XPOST -d @Notification_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: codeblock}
	
	dabei gilt:
	
	* Notification_File ist die JSON-Datei, die Ihre Benachrichtigung definiert.
	
	* *X-Auth-User-Token* ist ein Parameter, der das {{site.data.keyword.Bluemix_notm}} UAA-Token, das IAM-Token oder den API-Schlüssel übergibt.
	
	* *Auth_Type* ist das Präfix, das den Typ des Tokens oder den API-Schlüssel definiert. Die folgende Liste enthält die gültigen Werte: *apikey*, *iam* oder *uaa*, wobei
  * *apikey* angibt, dass das Token ein API-Schlüssel ist.
		* *iam* angibt, dass das Token ein IAM-generiertes Token ist.
		* *uaa* angibt, dass das Token ein UAA-generiertes Token ist.
	
	* *X-Auth-Scope-Id* ist ein Parameter, der die Bereichs-GUID übergibt. Die GUID muss das Präfix *s-* aufweisen, um einen Bereich zu kennzeichnen. Dieser Header ist erforderlich, wenn Sie eine UAA-Tokenauthentifizierung verwenden. Wenn Sie ein IAM-Authentifizierungstoken verwenden, dann ist dieser Header optional und die Domäneninformationen, die an das Token gebunden sind, werden verwendet.
	
	* Token ist das UAA-Token, das IAM-Token oder der API-Schlüssel.
	
	* Space (Bereich) ist die GUID des Bereichs. Die GUID ist nur erforderlich, wenn Sie ein UAA-Token verwenden.
	
    Beispiel: 	
	
	```
	curl -XPOST -d @s-email-dep-A.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.{DomainName}/v1/alert/notification
	```
	{: screen}

## Eine Benachrichtigung löschen
{:#delete}

Führen Sie folgende Schritte aus, um eine Benachrichtigung zu löschen:

1. Melden Sie sich bei einer Region, einer Organisation und einem Bereich von {{site.data.keyword.Bluemix_notm}} an. Führen Sie den folgenden Befehl aus:

    Führen Sie zum Beispiel den folgenden Befehl aus, um sich beim Bereich 'US South' anzumelden:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Befolgen Sie die Anweisungen. Geben Sie Ihre {{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise ein, wählen Sie eine Organisation und einen Bereich aus.

2. Rufen Sie das Authentifizierungstoken oder den API-Schlüssel ab.

    * Weitere Informationen zur IAM-Authentifizierung finden Sie in [IAM-Token über die Bluemix-Befehlszeilenschnittstelle abrufen](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) oder [IAM-API-Schlüssel über die Bluemix-Befehlszeilenschnittstelle generieren](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Weitere Informationen zur UAA-Authentifizierung finden Sie in [UAA-Token über die Bluemix-Befehlszeilenschnittstelle abrufen](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) oder [UAA-Token über die REST-API abrufen](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

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
	
4. Führen Sie den folgenden cURL-Befehl aus, um eine Benachrichtigung zu löschen:

    ```
	curl -XDELETE --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification/Notification_Name
	```
	{: codeblock}
	
	dabei gilt:
	
	* *Auth_Type* ist das Präfix, das den Typ des Tokens oder den API-Schlüssel definiert. Die folgende Liste enthält die gültigen Werte: *apikey*, *iam* oder *uaa*, wobei
  * *apikey* angibt, dass das Token ein API-Schlüssel ist.
		* *iam* angibt, dass das Token ein IAM-generiertes Token ist.
		* *uaa* angibt, dass das Token ein UAA-generiertes Token ist.
	
	* *X-Auth-User-Token* ist ein Parameter, der das {{site.data.keyword.Bluemix_notm}} UAA-Token, das IAM-Token oder den API-Schlüssel übergibt. Das Token oder der API-Schlüssel muss als Präfix einen der folgenden Werte ausweisen: *apikey*, *iam* oder *uaa*, wobei

  * *apikey* angibt, dass das Token ein API-Schlüssel ist.
		* *iam* angibt, dass das Token ein IAM-generiertes Token ist.
		* *uaa* angibt, dass das Token ein UAA-generiertes Token ist.
		
	* Token ist das UAA-Token, das IAM-Token oder der API-Schlüssel.
	
	* Space (Bereich) ist die GUID des Bereichs. Die GUID ist nur erforderlich, wenn Sie ein UAA-Token verwenden.
	
	* Notification_Name ist der Name der Benachrichtigung, d. h., der Wert für das Feld *name*, der beim Auflisten der Eigenschaften einer Benachrichtigung angezeigt wird.
	


## Alle Benachrichtigungen auflisten
{: #list}

Führen Sie folgende Schritte aus, um alle Benachrichtigungen aufzulisten:

1. Melden Sie sich bei einer Region, einer Organisation und einem Bereich von {{site.data.keyword.Bluemix_notm}} an. Führen Sie den folgenden Befehl aus:

    Führen Sie zum Beispiel den folgenden Befehl aus, um sich beim Bereich 'US South' anzumelden:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Befolgen Sie die Anweisungen. Geben Sie Ihre {{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise ein, wählen Sie eine Organisation und einen Bereich aus.

2. Rufen Sie das Authentifizierungstoken oder den API-Schlüssel ab.

    * Weitere Informationen zur IAM-Authentifizierung finden Sie in [IAM-Token über die Bluemix-Befehlszeilenschnittstelle abrufen](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) oder [IAM-API-Schlüssel über die Bluemix-Befehlszeilenschnittstelle generieren](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Weitere Informationen zur UAA-Authentifizierung finden Sie in [UAA-Token über die Bluemix-Befehlszeilenschnittstelle abrufen](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) oder [UAA-Token über die REST-API abrufen](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

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
	
4. Führen Sie den folgenden cURL-Befehl aus, um alle Benachrichtigungen aufzulisten:

    ```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notifications
	```
	{: codeblock}
	
	dabei gilt:
	
	* *Auth_Type* ist das Präfix, das den Typ des Tokens oder den API-Schlüssel definiert. Die folgende Liste enthält die gültigen Werte: *apikey*, *iam* oder *uaa*, wobei
  * *apikey* angibt, dass das Token ein API-Schlüssel ist.
		* *iam* angibt, dass das Token ein IAM-generiertes Token ist.
		* *uaa* angibt, dass das Token ein UAA-generiertes Token ist.
		
	* *X-Auth-User-Token* ist ein Parameter, der das {{site.data.keyword.Bluemix_notm}} UAA-Token, das IAM-Token oder den API-Schlüssel übergibt. Das Token oder der API-Schlüssel muss als Präfix einen der folgenden Werte ausweisen: *apikey*, *iam* oder *uaa*, wobei

  * *apikey* angibt, dass das Token ein API-Schlüssel ist.
		* *iam* angibt, dass das Token ein IAM-generiertes Token ist.
		* *uaa* angibt, dass das Token ein UAA-generiertes Token ist.
		
	* Token ist das UAA-Token, das IAM-Token oder der API-Schlüssel.
	
	* Space (Bereich) ist die GUID des Bereichs. Die GUID ist nur erforderlich, wenn Sie ein UAA-Token verwenden.


			

## Details einer Benachrichtigung anzeigen
{: #show}


Führen Sie folgende Schritte aus, um die Details einer Benachrichtigung anzuzeigen:

1. Melden Sie sich bei einer Region, einer Organisation und einem Bereich von {{site.data.keyword.Bluemix_notm}} an. Führen Sie den folgenden Befehl aus:

    Führen Sie zum Beispiel den folgenden Befehl aus, um sich beim Bereich 'US South' anzumelden:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Befolgen Sie die Anweisungen. Geben Sie Ihre {{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise ein, wählen Sie eine Organisation und einen Bereich aus.

2. Rufen Sie das Authentifizierungstoken oder den API-Schlüssel ab.

    * Weitere Informationen zur IAM-Authentifizierung finden Sie in [IAM-Token über die Bluemix-Befehlszeilenschnittstelle abrufen](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) oder [IAM-API-Schlüssel über die Bluemix-Befehlszeilenschnittstelle generieren](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Weitere Informationen zur UAA-Authentifizierung finden Sie in [UAA-Token über die Bluemix-Befehlszeilenschnittstelle abrufen](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) oder [UAA-Token über die REST-API abrufen](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

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
	
4. Führen Sie den folgenden cURL-Befehl aus, um die Details einer Benachrichtigung anzuzeigen:

    ```
	curl -XGET --header "X-Auth-User-Token:uaa ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification/Notification_Name
	```
	{: codeblock}
	
	dabei gilt:
	
	* *X-Auth-User-Token* ist ein Parameter, der das {{site.data.keyword.Bluemix_notm}} UAA-Token, das IAM-Token oder den API-Schlüssel übergibt. Das Token oder der API-Schlüssel muss als Präfix einen der folgenden Werte ausweisen: *apikey*, *iam* oder *uaa*, wobei

  * *apikey* angibt, dass das Token ein API-Schlüssel ist.
		* *iam* angibt, dass das Token ein IAM-generiertes Token ist.
		* *uaa* angibt, dass das Token ein UAA-generiertes Token ist.
		
	* Token ist das UAA-Token, das IAM-Token oder der API-Schlüssel.
	
	* Space (Bereich) ist die GUID des Bereichs. Die GUID ist nur erforderlich, wenn Sie ein UAA-Token verwenden.
	
	* Notification_Name ist der Name der Benachrichtigung, d. h., der Wert für das Feld *name*, der beim Auflisten der Eigenschaften einer Benachrichtigung angezeigt wird.
	
     
		

			
## Eine Benachrichtigung testen
{: #test}	
			
Führen Sie die folgenden Schritte aus, um eine Benachrichtigung zu testen:

1. Melden Sie sich bei einer Region, einer Organisation und einem Bereich von {{site.data.keyword.Bluemix_notm}} an. Führen Sie den folgenden Befehl aus:

    Führen Sie zum Beispiel den folgenden Befehl aus, um sich beim Bereich 'US South' anzumelden:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Befolgen Sie die Anweisungen. Geben Sie Ihre {{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise ein, wählen Sie eine Organisation und einen Bereich aus.

2. Rufen Sie das Authentifizierungstoken oder den API-Schlüssel ab.

    * Weitere Informationen zur IAM-Authentifizierung finden Sie in [IAM-Token über die Bluemix-Befehlszeilenschnittstelle abrufen](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) oder [IAM-API-Schlüssel über die Bluemix-Befehlszeilenschnittstelle generieren](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Weitere Informationen zur UAA-Authentifizierung finden Sie in [UAA-Token über die Bluemix-Befehlszeilenschnittstelle abrufen](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) oder [UAA-Token über die REST-API abrufen](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

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
	
4. Führen Sie den folgenden cURL-Befehl aus, um eine Benachrichtigung zu testen:

    ```
	curl -XPOST --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification/test/Notification_Name
	```
	{: codeblock}
	
	dabei gilt:
	
	* *Auth_Type* ist das Präfix, das den Typ des Tokens oder den API-Schlüssel definiert. Die folgende Liste enthält die gültigen Werte: *apikey*, *iam* oder *uaa*, wobei
  * *apikey* angibt, dass das Token ein API-Schlüssel ist.
		* *iam* angibt, dass das Token ein IAM-generiertes Token ist.
		* *uaa* angibt, dass das Token ein UAA-generiertes Token ist.
	
	* *X-Auth-User-Token* ist ein Parameter, der das {{site.data.keyword.Bluemix_notm}} UAA-Token, das IAM-Token oder den API-Schlüssel übergibt. Das Token oder der API-Schlüssel muss als Präfix einen der folgenden Werte ausweisen: *apikey*, *iam* oder *uaa*, wobei

  * *apikey* angibt, dass das Token ein API-Schlüssel ist.
		* *iam* angibt, dass das Token ein IAM-generiertes Token ist.
		* *uaa* angibt, dass das Token ein UAA-generiertes Token ist.
		
	* Token ist das UAA-Token, das IAM-Token oder der API-Schlüssel.
	
	* Space (Bereich) ist die GUID des Bereichs. Die GUID ist nur erforderlich, wenn Sie ein UAA-Token verwenden.
	
	* Notification_Name ist der Name der Benachrichtigung, d. h., der Wert für das Feld *name*, der beim Auflisten der Eigenschaften einer Benachrichtigung angezeigt wird.
	


## Benachrichtigung aktualisieren
{: #update}

Führen Sie die folgenden Schritte aus, um eine Benachrichtigung zu aktualisieren:

1. Melden Sie sich bei einer Region, einer Organisation und einem Bereich von {{site.data.keyword.Bluemix_notm}} an. Führen Sie den folgenden Befehl aus:

    Führen Sie zum Beispiel den folgenden Befehl aus, um sich beim Bereich 'US South' anzumelden:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Befolgen Sie die Anweisungen. Geben Sie Ihre {{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise ein, wählen Sie eine Organisation und einen Bereich aus.

2. Rufen Sie das Authentifizierungstoken oder den API-Schlüssel ab.

    * Weitere Informationen zur IAM-Authentifizierung finden Sie in [IAM-Token über die Bluemix-Befehlszeilenschnittstelle abrufen](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) oder [IAM-API-Schlüssel über die Bluemix-Befehlszeilenschnittstelle generieren](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Weitere Informationen zur UAA-Authentifizierung finden Sie in [UAA-Token über die Bluemix-Befehlszeilenschnittstelle abrufen](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) oder [UAA-Token über die REST-API abrufen](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

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
	
4. Führen Sie den folgenden cURL-Befehl aus, um eine Benachrichtigung zu aktualisieren:

    ```
	curl -XPUT -d @Notification_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: codeblock}
	
	dabei gilt:
	
	* Notification_File ist die JSON-Datei, die Ihre Benachrichtigung definiert.
	
	* *Auth_Type* ist das Präfix, das den Typ des Tokens oder den API-Schlüssel definiert. Die folgende Liste enthält die gültigen Werte: *apikey*, *iam* oder *uaa*, wobei
  * *apikey* angibt, dass das Token ein API-Schlüssel ist.
		* *iam* angibt, dass das Token ein IAM-generiertes Token ist.
		* *uaa* angibt, dass das Token ein UAA-generiertes Token ist.
	
	* *X-Auth-User-Token* ist ein Parameter, der das Bluemix UAA-Token, das IAM-Token oder den API-Schlüssel übergibt. Das Token oder der API-Schlüssel muss als Präfix einen der folgenden Werte ausweisen: *apikey*, *iam* oder *uaa*, wobei

  * *apikey* angibt, dass das Token ein API-Schlüssel ist.
		* *iam* angibt, dass das Token ein IAM-generiertes Token ist.
		* *uaa* angibt, dass das Token ein UAA-generiertes Token ist.
		
	* Token ist das UAA-Token, das IAM-Token oder der API-Schlüssel.
	
	* Space (Bereich) ist die GUID des Bereichs. Die GUID ist nur erforderlich, wenn Sie ein UAA-Token verwenden.

	
        

