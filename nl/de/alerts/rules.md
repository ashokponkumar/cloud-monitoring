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


# Mithilfe der Alerts-API mit Regeln arbeiten
{: #rules}

Verwenden Sie die Alerts-API, um eine Regel zu erstellen, zu löschen oder zu aktualisieren, um die Details einer Regel anzuzeigen und um die Regeln aufzulisten, die in Ihrem {{site.data.keyword.Bluemix_notm}}-Bereich definiert sind.
{:shortdesc}


## Eine Regel erstellen
{: #create}

Führen Sie die folgenden Schritte aus, um eine Regel zu erstellen:

1. Erstellen Sie eine Regeldatei mit Daten im JSON-Format. Speichern Sie die Datei. 

    Verwenden Sie zum Speichern der Datei beispielsweise ein Präfix wie *s-* für Regeln, die Sie für Abfragen definieren, die in einem {{site.data.keyword.Bluemix_notm}}-Bereich ausgeführt werden.
	
	**Tipps:** 
	
	* Definieren Sie verschiedene Regeln für eine Abfrage, um auf Fehler oder Warnungen hinzuweisen, indem Sie eine andere Benachrichtigungsmethode verwenden. Sie können nur genau eine Benachrichtigungsmethode pro Regel festlegen. 
	* Erstellen Sie Ihre Regeldatei im Verzeichnis *~/cloud-monitoring/rules/*, um die Ressourcen des {{site.data.keyword.monitoringshort_notm}}-Service in einer Gruppe zusammenzufassen. 

    Geben Sie die folgenden Informationen in der Regeldatei ein:
	
	* * name *: Geben Sie einen eindeutigen Namen ein. Der Wert für dieses Feld wird später für die Aktualisierung, das Löschen und das Anzeigen von Details für die Regel verwendet.
	* *description*: Geben Sie eine Beschreibung ein.
	* *expression*: Geben Sie die Abfrage, die Sie überwachen möchten, ein.
	* *enabled*: Legen Sie für die Eigenschaft *true* fest, um die Regel zu aktivieren.
	* *from*: Anfangszeitpunkt, der für die Analyse der Daten auf Grundlage von Grenzwerten verwendet wird.
	* *until*: Exitzeitpunkt, der für die Analyse der Daten auf Grundlage von Grenzwerten verwendet wird.
	* *comparison*: Vergleichsoperation, die zur Identifizierung der durchzuführenden Art der Überprüfung verwendet wird. Beispiel: *below*.
	* *error_level*: Definiert den Schwellenwert, den Sie als Auslöser für einen Fehleralert festlegen.
	* *warning_level*: Definiert den Schwellenwert, den Sie als Auslöser für einen Warnungsalert festlegen.
	* *frequency*: Definiert wie oft Sie die Daten überprüfen.
	* *dashboard_url*: Definiert eine URL, die in Grafana ein Dashboard mit der Abfrage anzeigt.
	* *notifications*: Die Benachrichtigungsmethode für den Fall, dass der durch diese Regel beschriebene Alert ausgelöst wird.
	
	Beispiel: 
	
	```
	{
    "name": "checkbytesin",
    "description": "MH check Bytes In per second",
    "expression": "movingAverage(messagehub.65qser11-8034-1234-5678-c82fb42wdfgh.1.kafka-java-console-sample-topic.BytesInPerSec.15MinuteRate,\"5min\")",
    "enabled": true,
    "from": "-5min",
    "until": "now",
    "comparison": "above",
    "comparison_scope": "last",
    "error_level" : 27,
    "warning_level" : 25,
    "frequency": "1min",
    "dashboard_url": "https://metrics.ng.bluemix.net",
    "notifications": [
      "emailXXX"
    ]
    }
    ```
	{: screen}
	
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
	
	**Hinweis:** Das Token schließt *Bearer* (Träger) aus.

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
	
5. Führen Sie den folgenden cURL-Befehl aus, um eine Regel zu erstellen:

    ```
	curl -XPOST -d @Rule_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: codeblock}
	
	dabei gilt:
	
	* Rule_File ist die JSON-Datei, die Ihre Alertregel definiert.
	
	* Das *X-Auth-User-Token* ist ein Parameter, der das UAA-Token, das IAM-Token oder den API-Schlüssel von {{site.data.keyword.Bluemix_notm}} übergibt.
	
	* *Auth_Type* ist das Präfix, das den Typ des Tokens oder den API-Schlüssel definiert. Die folgende Liste enthält die gültigen Werte: *apikey*, *iam* oder *uaa*, wobei
  * *apikey* angibt, dass das Token ein API-Schlüssel ist.
		* *iam* angibt, dass das Token ein IAM-generiertes Token ist.
		* *uaa* angibt, dass das Token ein UAA-generiertes Token ist.
	
	* *X-Auth-Scope-Id* ist ein Parameter, der die Bereichs-GUID übergibt. Die GUID muss das Präfix *s-* aufweisen, um einen Bereich zu kennzeichnen. Dieser Header ist erforderlich, wenn Sie eine UAA-Tokenauthentifizierung verwenden. Wenn Sie ein IAM-Authentifizierungstoken verwenden, dann ist dieser Header optional und die Domäneninformationen, die an das Token gebunden sind, werden verwendet.
	
	* Token ist das UAA- oder IAM-Authentifizierungstoken oder der API-Schlüssel.
	
	* Space (Bereich) ist die GUID des Bereichs. Die GUID ist nur erforderlich, wenn Sie ein UAA-Token verwenden.
	
    Beispiel: 	
	
	```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}

## Eine Regel löschen
{: #delete}

Führen Sie folgende Schritte aus, um eine Regel zu löschen:

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
	
4. Führen Sie den folgenden cURL-Befehl aus, um eine Regel zu löschen:

    ```
	curl -XDELETE --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/rule/Rule_Name
	```
	{: codeblock}
	
	wobei gilt:
	
    * Das *X-Auth-User-Token* ist ein Parameter, der das  UAA-Token, IAM-Token oder den API-Schlüssel von {{site.data.keyword.Bluemix_notm}} übergibt.
	
	* *Auth_Type* ist das Präfix, das den Typ des Tokens oder den API-Schlüssel definiert. Die folgende Liste enthält die gültigen Werte: *apikey*, *iam* oder *uaa*, wobei
  * *apikey* angibt, dass das Token ein API-Schlüssel ist.
		* *iam* angibt, dass das Token ein IAM-generiertes Token ist.
		* *uaa* angibt, dass das Token ein UAA-generiertes Token ist.
	
	* *X-Auth-Scope-Id* ist ein Parameter, der die Bereichs-GUID übergibt. Die GUID muss das Präfix *s-* aufweisen, um einen Bereich zu kennzeichnen. Dieser Header ist erforderlich, wenn Sie eine UAA-Tokenauthentifizierung verwenden. Wenn Sie ein IAM-Authentifizierungstoken verwenden, dann ist dieser Header optional und die Domäneninformationen, die an das Token gebunden sind, werden verwendet.
	
	* Token ist das UAA- oder IAM-Authentifizierungstoken oder der API-Schlüssel.
	
	* Space (Bereich) ist die GUID des Bereichs. Die GUID ist nur erforderlich, wenn Sie ein UAA-Token verwenden.
	
	* Rule_Name ist der Name der Regel, die im Feld *name* angegeben ist.
	
    
	
## Listing all rules
{: #list}

Führen Sie die folgenden Schritte aus, um alle Regeln aufzulisten:

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
	
4. Führen Sie den folgenden cURL-Befehl aus, um alle Regeln in einem Bereich aufzulisten:

    ```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/rules
	```
	{: codeblock}
	
	dabei gilt:
	
	* Das *X-Auth-User-Token* ist ein Parameter, der das  UAA-Token, IAM-Token oder den API-Schlüssel von {{site.data.keyword.Bluemix_notm}} übergibt.
	
	* *Auth_Type* ist das Präfix, das den Typ des Tokens oder den API-Schlüssel definiert. Die folgende Liste enthält die gültigen Werte: *apikey*, *iam* oder *uaa*, wobei
  * *apikey* angibt, dass das Token ein API-Schlüssel ist.
		* *iam* angibt, dass das Token ein IAM-generiertes Token ist.
		* *uaa* angibt, dass das Token ein UAA-generiertes Token ist.
	
	* *X-Auth-Scope-Id* ist ein Parameter, der die Bereichs-GUID übergibt. Die GUID muss das Präfix *s-* aufweisen, um einen Bereich zu kennzeichnen. Dieser Header ist erforderlich, wenn Sie eine UAA-Tokenauthentifizierung verwenden. Wenn Sie ein IAM-Authentifizierungstoken verwenden, dann ist dieser Header optional und die Domäneninformationen, die an das Token gebunden sind, werden verwendet.
	
	* Token ist das UAA- oder IAM-Authentifizierungstoken oder der API-Schlüssel.
	
	* Space (Bereich) ist die GUID des Bereichs. Die GUID ist nur erforderlich, wenn Sie ein UAA-Token verwenden.
	

	
	

## Details einer Regel anzeigen
{: show}

Führen Sie folgende Schritte aus, um die Details einer Regel anzuzeigen:

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
	
4. Führen Sie den folgenden cURL-Befehl aus, um die Details für eine Regel anzuzeigen:

    ```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/rule/Rule_Name
	```
	{: codeblock}
	
	dabei gilt:
	
	* Das *X-Auth-User-Token* ist ein Parameter, der das  UAA-Token, IAM-Token oder den API-Schlüssel von {{site.data.keyword.Bluemix_notm}} übergibt.
	
	* *Auth_Type* ist das Präfix, das den Typ des Tokens oder den API-Schlüssel definiert. Die folgende Liste enthält die gültigen Werte: *apikey*, *iam* oder *uaa*, wobei
  * *apikey* angibt, dass das Token ein API-Schlüssel ist.
		* *iam* angibt, dass das Token ein IAM-generiertes Token ist.
		* *uaa* angibt, dass das Token ein UAA-generiertes Token ist.
	
	* *X-Auth-Scope-Id* ist ein Parameter, der die Bereichs-GUID übergibt. Die GUID muss das Präfix *s-* aufweisen, um einen Bereich zu kennzeichnen. Dieser Header ist erforderlich, wenn Sie eine UAA-Tokenauthentifizierung verwenden. Wenn Sie ein IAM-Authentifizierungstoken verwenden, dann ist dieser Header optional und die Domäneninformationen, die an das Token gebunden sind, werden verwendet.
	
	* Token ist das UAA- oder IAM-Authentifizierungstoken oder der API-Schlüssel.
	
	* Space (Bereich) ist die GUID des Bereichs. Die GUID ist nur erforderlich, wenn Sie ein UAA-Token verwenden.
	
	* Rule_Name ist der Name der Regel, die im Feld *name* angegeben ist.
	

## Eine Regel aktualisieren
{: #update}

Führen Sie die folgenden Schritte aus, um eine Regel zu aktualisieren:

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
	
4. Führen Sie den folgenden cURL-Befehl aus, um eine Regel zu aktualisieren:

    ```
	curl -XPUT `-d @Rule_File` --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: codeblock}
	
	dabei gilt:
	
	* Rule_File ist die JSON-Datei, die Ihre Alertregel definiert.
	
	* Das *X-Auth-User-Token* ist ein Parameter, der das  UAA-Token, IAM-Token oder den API-Schlüssel von {{site.data.keyword.Bluemix_notm}} übergibt.
	
	* *Auth_Type* ist das Präfix, das den Typ des Tokens oder den API-Schlüssel definiert. Die folgende Liste enthält die gültigen Werte: *apikey*, *iam* oder *uaa*, wobei
  * *apikey* angibt, dass das Token ein API-Schlüssel ist.
		* *iam* angibt, dass das Token ein IAM-generiertes Token ist.
		* *uaa* angibt, dass das Token ein UAA-generiertes Token ist.
	
	* *X-Auth-Scope-Id* ist ein Parameter, der die Bereichs-GUID übergibt. Die GUID muss das Präfix *s-* aufweisen, um einen Bereich zu kennzeichnen. Dieser Header ist erforderlich, wenn Sie eine UAA-Tokenauthentifizierung verwenden. Wenn Sie ein IAM-Authentifizierungstoken verwenden, dann ist dieser Header optional und die Domäneninformationen, die an das Token gebunden sind, werden verwendet.
	
	* Token ist das UAA- oder IAM-Authentifizierungstoken oder der API-Schlüssel.
	
	* Space (Bereich) ist die GUID des Bereichs. Die GUID ist nur erforderlich, wenn Sie ein UAA-Token verwenden.

	
	
