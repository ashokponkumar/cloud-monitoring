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


# Daten vom Überwachungsservice abrufen
{: #retrieve_data_api}

Sie können Metriken vom {{site.data.keyword.monitoringshort}}-Service abrufen, indem Sie die [Metrik-API](https://console.bluemix.net/apidocs/927-ibm-cloud-monitoring-rest-api?&language=node#introduction){: new_window} verwenden. Sie können Metriken aus einem {{site.data.keyword.Bluemix_notm}}-Bereich abrufen.
{:shortdesc}


Zum Definieren der Gruppe von Daten, die Sie abrufen möchten, beachten Sie die folgenden Informationen: 

* Sie müssen den {{site.data.keyword.Bluemix_notm}}-Bereich festlegen, aus dem die Daten abgerufen werden sollen.

* Sie müssen ein Token oder einen API-Schlüssel für die Verwendung des {{site.data.keyword.monitoringshort}}-Service angeben. 

* Sie müssen einen Pfad für eine oder mehrere Metriken angeben. Weitere Informationen finden Sie in [Metriken definieren](#metrics).

* Optional können Sie einen benutzerdefinierten Zeitraum angeben. Wenn Sie keinen Zeitraum angeben, entsprechen die abgerufenen Daten standardmäßig den letzten 24 Stunden. Weitere Informationen finden Sie in [Zeitraum konfigurieren](#time).

**Hinweis:** Pro Anforderung können maximal 5 Ziele abgerufen werden. 


## Metriken definieren
{: #metrics}

Zum Abrufen von Daten für eine Metrik müssen Sie den Pfad der Metrik vom Stammverzeichnis der Metrikverzeichnisstruktur bis zur betreffenden Metrik angeben; dabei müssen die einzelnen Schritte durch einen Punkt (.) voneinander getrennt werden.

Der Pfad wird im Parameter **Target** angegeben. Pro Anforderung können maximal 5 Ziele angegeben werden.  

Optional können Sie Funktionen für die Metrik anwenden. Weitere Informationen zu unterstützten Funktionen finden Sie in [Graphite 0.9.15-Funktionen ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](http://graphite.readthedocs.io/en/latest/functions.html "Symbol für externen Link").

Für die Definition eines Pfads können Platzhalter verwendet werden. Mithilfe von Platzhaltern können Sie mehrere Metriken in einem einzelnen Pfad angeben.  

* **Stern**: Sie können einen Stern (*) als Platzhalter für null, ein oder mehrere Zeichen angeben. 

    Innerhalb eines einzelnen Pfadelements können mehrere Sterne verwendet werden. Beispiel: `servers.sp*aeg*r.cpu.total.*`. Mit diesem Beispiel werden Daten zur gesamten CPU-Nutzung für alle Server, die dem Muster entsprechen, zurückgegeben.

* **Liste oder Bereich mit Zeichen**: Sie können Zeichen in eckigen Klammern ([...]) definieren, um eine einzelne Zeichenposition in der Pfadzeichenfolge anzugeben. 

    Sie können einen einschließenden Bereich von Zeichen festlegen, indem Sie zwei Zeichen angeben, die durch einen Bindestrich getrennt sind. Alle Zeichen zwischen diesen beiden Zeichen werden für die Pfadangabe verwendet. 
	
	Sie können innerhalb der eckigen Klammern einen oder mehrere Zeichenbereiche definieren. 
	
	Wenn die Zeichen innerhalb der eckigen Klammern nicht als Bereich gelesen werden können, werden sie als Liste mit Werten behandelt. 
	
	Sie können einen Bindestrich (-) als Zeichen in einer Werteliste angeben, indem Sie ihn an die erste oder letzte Stelle innerhalb der eckigen Klammern setzen.

* **Werteliste**: Sie können durch Kommas getrennte Werte innerhalb von geschweiften Klammern festlegen, um Wertelisten zu definieren. 

    Beispiel: Zum Herunterladen von Metriken für die Pfade `servers.myserver.cpu.total.user` und `servers.myserver.cpu.total.system` können Sie einen einzelnen Pfad für beide Typen festlegen: `servers.myserver.cpu.total.{user,system}`.



## Zeitraum festlegen
{: #time}

Sie können optional einen Zeitraum angeben, indem Sie eine relative Zeit oder eine absolute Zeit verwenden. Sie müssen die Abfrageparameter **From** und **Until** definieren.  

* Wenn die Startzeit des Zeitraums nicht angegeben wird, wird standardmäßig eine Startzeit vor 24 Stunden verwendet. 

* Wenn die Endzeit des Zeitraums nicht angegeben wird, ist der Standardwert die aktuelle Uhrzeit (*jetzt*).

Der **relativen Zeit** wird stets ein Minuszeichen (-) vorangestellt und eine Zeiteinheit angefügt. 

In der folgenden Tabelle sind die verschiedenen Abkürzungen für die relative Zeit aufgeführt, die verwendet werden können:


| Abkürzung    | Beschreibung|
|--------------|-------------|
| s            | Sekunden    |
| min          | Minuten     |
| h            | Stunden     |
| d            | Tage        |
| w            | Wochen      |
| mon          | Monate      |
| now          | Aktuelle Zeit|


Sie können eines der folgenden Formate verwenden, um eine **absolute Zeit** zu definieren: `HH:MM_JJMMTT`, `JJJJMMTT`, `MM/TT/JJ`.

| Abkürzung    | Beschreibung|
|--------------|-------------|
| JJJJ         | Vierstellige Jahresangabe|
| MM           | Zweistellige Monatsangabe (01 = Januar usw.)|
| TT           | Zweistellige Tagesangabe (01 bis 31)|
| hh           | Zweistellige Stundenangabe (00 bis 23)|
| mm           | Zweistellige Minutenangabe (00 bis 59)|
| ss           | Zweistellige Sekundenangabe (00 bis 59)|

**Beispiel**

In der folgenden Tabelle sind verschiedene Beispiele für das Festlegen unterschiedlicher Zeiträume mit den Parametern *from* und *until* aufgeführt:

<table>
  <caption>Tabelle 1. Verschiedene Beispiele für das Festlegen unterschiedlicher Zeiträume mit den Parametern *from* und *until*</caption>
  <tr>
    <th>Beispiel</th>
	<th>Beschreibung</th>
  </tr>
  <tr>
    <td>&from=monday</td>
	<td>Zeitraum, der Daten vom letzten Montag bis jetzt umfasst.</td>
  </tr>
  <tr>
    <td>&from=20171101&until=20171130</td>
	<td>Zeitraum, für den ein Monat festgelegt wird, z. B. November.</td>
  </tr>
  <tr>
    <td>&from=02:00_20170701&until=14:00_20170701</td>
	<td>Anzeigen von Daten in einem 12 Stunden umfassenden Zyklus, z. B. von 02:00 Uhr bis 14:00 Uhr am 01. Juli 2017.</td>
  </tr>
  <tr>
    <td>&from=-8d&until=-7d</td>
	<td>Anzeigen von Daten für denselben Wochentag, z. B. letzte Woche.</td>
  </tr>
  
</table>


## Bereich festlegen
{: #domain}

Wenn Sie das {{site.data.keyword.Bluemix_notm}}-UAA-Authentifizierungsmodell oder das {{site.data.keyword.Bluemix_notm}}-IAM-Authentifizierungsmodell für die Authentifizierung beim {{site.data.keyword.monitoringshort}}-Service verwenden, ist das Headerfeld **X-Auth-Scope-Id** erforderlich; als Wert muss die Bereichs-GUID angegeben werden. Die GUID muss das Präfix *s-* aufweisen, um einen Bereich zu kennzeichnen.



## Berechtigungstoken oder API-Schlüssel festlegen
{: #security}
  
Das Feld **X-Auth-User-Token** muss festgelegt werden, um das UAA-Token, das IAM-Token oder den API-Schlüssel für die Authentifizierung festzulegen. 

Ein Token oder API-Schlüssel muss als Präfix einen der folgenden Werte ausweisen: `apikey`, `iam` oder `uaa`. 

Dabei gilt Folgendes: 

* *apikey* gibt an, dass es sich bei dem Token um einen API-Schlüssel handelt.
* *iam* gibt an, dass es sich bei dem angegebenen Token um ein IAM-generiertes Token handelt.
* *uaa* gibt an, dass es sich bei dem angegebenen Token um ein UAA-generiertes Token handelt.

Sie können zum Beispiel den folgenden Header für einen API-Schlüssel festlegen: `X-Auth-User-Token: apikey SomeIAMGeneratedKey`.


## UAA zum Abrufen von Metriken aus einem Bereich verwenden
{: #uaa}

Führen Sie die folgenden Schritte aus, um Metriken von einem {{site.data.keyword.Bluemix_notm}}-Bereich abzurufen:

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
	export Token="eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8"
	```
	{: screen}
	
	**Hinweis:** Das Token schließt *Bearer* (Träger) aus
		
2. Rufen Sie die Bereichs-GUID ab. Die GUID muss das Präfix *s-* aufweisen, um einen Bereich zu kennzeichnen.

    Führen Sie den folgenden Befehl aus:
	
	```
	cf space SpaceName --guid
	```
	{: codeblock}
	
	Dabei ist *SpaceName* der Name des Bereichs, in dem eine Benachrichtigung definiert werden soll. Dem Namen wird das Präfix *s-* vorangestellt.
	
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
	curl -XGET --header "X-Auth-User-Token:uaa ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics?from=Start_Time&until=End_Time&target=string
	```
	{: codeblock}

	Dabei gilt:
	
	* *X-Auth-User-Token* ist ein Parameter, der das UAA-Token von {{site.data.keyword.Bluemix_notm}} übergibt.
	
	* *uaa* ist das Präfix, das angibt, dass es sich bei dem angegebenen Token um ein UAA-generiertes Token handelt.
	
	* *X-Auth-Scope-Id* ist ein Parameter, der die Bereichs-GUID übergibt. Die GUID muss das Präfix *s-* aufweisen, um einen Bereich zu kennzeichnen. Dieser Header ist nur erforderlich, falls Sie die UAA-Authentifizierung verwenden. Wenn Sie ein IAM-Authentifizierungstoken verwenden, dann ist dieser Header optional und die Domäneninformationen, die an das Token gebunden sind, werden verwendet.
	
	* *Token* stellt das UAA-Token dar.
	
	* *Space* stellt die GUID des Bereichs dar. Die GUID ist nur erforderlich, wenn Sie ein UAA-Token verwenden.
	
	* *Start_Time* definiert den relativen oder absoluten Zeitraum, der den Beginn der Anforderung angibt. Der Standardwert ist `-24h`, d. h. vor 24 Stunden. *End_Time* definiert den relativen oder absoluten Zeitraum, der das Ende der Anforderung angibt. Der Standardwert ist `now`, d. h. die aktuelle Uhrzeit. Weitere Informationen finden Sie in [Zeitraum festlegen](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#time).
	
	* *Path* gibt eine oder mehrere Metriken an. Optional können Sie Funktionen für die jeweilige Metrik anwenden. Die Pfadangabe unterstützt auch Platzhalterzeichen, mit denen Sie mehrere Metriken in einem einzelnen Pfad angeben können. Weitere Informationen finden Sie in [Metriken definieren](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#metrics).
	
	
	
## Metriken mithilfe von IAM oder mithilfe eines API-Schlüssels von einem Bereich abrufen
{: #iam}

Führen Sie die folgenden Schritte aus, um Metriken mithilfe von IAM oder mithilfe eines API-Schlüssels von einem {{site.data.keyword.Bluemix_notm}}-Bereich abzurufen:

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
	
	Dabei ist *SpaceName* der Name des Bereichs, in dem eine Benachrichtigung definiert werden soll. Dem Namen wird das Präfix *s-* vorangestellt.
	
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
	
5. Führen Sie einen cURL-Befehl aus, um Metriken abzurufen.

    ```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics?from=Start_Time&until=End_Time&target=string
	```
	{: codeblock}

	Dabei gilt Folgendes:
	
	* *X-Auth-User-Token* ist ein Parameter, der das {{site.data.keyword.Bluemix_notm}} IAM-Token oder den API-Schlüssel übergibt.
	
	* *Auth_Type* ist das Präfix, das den Typ des Tokens oder den API-Schlüssel definiert.

  * *apikey* gibt an, dass es sich bei dem Token um einen API-Schlüssel handelt.

		* *iam* gibt an, dass es sich bei dem angegebenen Token um ein IAM-generiertes Token handelt.
	
	* *X-Auth-Scope-Id* ist ein Parameter, der die Bereichs-GUID übergibt. Die GUID muss das Präfix *s-* aufweisen, um einen Bereich zu kennzeichnen. Dieser Header ist nur erforderlich, falls Sie die UAA-Authentifizierung verwenden. Wenn Sie ein IAM-Authentifizierungstoken verwenden, dann ist dieser Header optional und die Domäneninformationen, die an das Token gebunden sind, werden verwendet.
	
	* *Token* stellt das UAA-Token dar.
	
	* *Space* stellt die GUID des Bereichs dar. Die GUID ist nur erforderlich, wenn Sie ein UAA-Token verwenden.
	
	* *Start_Time* definiert den relativen oder absoluten Zeitraum, der den Beginn der Anforderung angibt. Der Standardwert ist `-24h`, d. h. vor 24 Stunden. *End_Time* definiert den relativen oder absoluten Zeitraum, der das Ende der Anforderung angibt. Der Standardwert ist `now`, d. h. die aktuelle Uhrzeit. Weitere Informationen finden Sie in [Zeitraum festlegen](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#time).
	
	* *Path* gibt eine oder mehrere Metriken an. Optional können Sie Funktionen für die jeweilige Metrik anwenden. Die Pfadangabe unterstützt auch Platzhalterzeichen, mit denen Sie mehrere Metriken in einem einzelnen Pfad angeben können. Weitere Informationen finden Sie in [Metriken definieren](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#metrics).
	
	
	





