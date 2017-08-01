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


# Sicherheit
{: #security_ov}

Sie können verschiedene Authentifizierungsmethoden verwenden, um Metriken an den {{site.data.keyword.monitoringshort}}-Service zu senden. Berechtigungen für den Zugriff auf die Metriken sowie zum Ändern und Löschen von Metriken werden durch die Verwendung von Rollen gesteuert.
{:shortdesc}

   
## Authentifizierungsmodelle
{: #auth}

Für den Zugriff auf die Metriken, die im {{site.data.keyword.monitoringshort}}-Service für einen {{site.data.keyword.Bluemix_notm}}-Bereich gespeichert sind, benötigen Sie ein Authentifizierungstoken oder einen API-Schlüssel. 

Der {{site.data.keyword.monitoringshort}}-Service unterstützt die folgenden Authentifizierungsmodelle:

* [{{site.data.keyword.Bluemix_notm}}-UAA-Authentifizierung](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_uaa)
* [{{site.data.keyword.Bluemix_notm}}-IAM-Authentifizierung](/docs/services/cloud-monitoring/security/auth_iam.html#auth_iam)

Ein UAA-Token und ein IAM-Token laufen nach einem bestimmten Zeitraum ab. Der API-Schlüssel läuft nicht ab. 

Wenn die Sicherheit des API-Schlüssels beeinträchtigt ist, können Sie ihn widerrufen, indem Sie ihn löschen. Anschließend können Sie einen neuen erstellen. Weitere Informationen finden Sie in [API-Schlüssel über die Bluemix-Benuzterschnittstelle widerrufen](/docs/services/cloud-monitoring/security/auth_iam.html#revoke_ui). 

Das IAM-Authentifizierungsmodell bietet Benutzerschnittstellen-, Befehlszeilenschnittstellen- und API-Verwaltungsfunktionen. Sie können nur die Befehlszeilenschnittstelle zum Verwalten von UAA-Token verwenden.

In der folgenden Tabelle werden die Authentifizierungsmodelle aufgelistet, die für den jeweiligen Typ von Domäne unterstützt werden:

<table>
  <caption>Tabelle 1. Authentifizierungsmodelle, die für die jeweilige Domäne unterstützt werden</caption>
  <tr>
    <th></th>
	<th align="right">Konto</th>
    <th align="right">Organisation</th>
    <th align="right">Bereich</th>	
  </tr>
  <tr>
    <th align="left">UAA</th>
	<td align="center">Nein</td>
	<td align="center">Ja</td>
	<td align="center">Ja</td>
  </tr>
  <tr>
    <th align="left">IAM</th>
	<td align="center">Ja</td>
	<td align="center">Ja</td>
	<td align="center">Ja</td>
  </tr>
</table>

Der {{site.data.keyword.monitoringshort}}-Service verwendet die IAM-Zugriffssteuerungsfunktion, um zu regeln, welche Aktionen oder Operationen ein Benutzer oder Service mit der Domäne ausführen kann. Sie können zum Beispiel eine IAM-Richtlinie definieren, um einem Benutzer das Senden und Erstellen von Dashboards für eine Domäne zu erlauben, jedoch nicht das Abrufen der Metriken aus der Domäne.



## Bluemix-UAA-Rollen
{: #bmx_roles}

In der folgenden Tabelle sind die Zugriffsrechte für jede {{site.data.keyword.Bluemix_notm}}-Rolle für die Arbeit mit dem {{site.data.keyword.monitoringshort}}-Service aufgelistet:

<table>
  <caption>Tabelle 2. {{site.data.keyword.Bluemix_notm}}-Rollen und Zugriffsrechte für die Arbeit mit dem {{site.data.keyword.monitoringshort}}-Service.</caption>
  <tr>
    <th>Rolle</th>
	<th>Domäne</th>
	<th>Zugriffsberechtigungen</th>
  </tr>
  <tr>
    <td>Manager</td>
	<td>Organisation <br>Bereich</td>
	<td>Alle REST-APIs</td>
  </tr>
  <tr>
    <td>Entwickler</td>
	<td>Bereich</td>
	<td>Alle REST-APIs</td>
  </tr>
  <tr>
    <td>Auditor</td>
	<td>Organisation <br>Bereich</td>
	<td>Nur RESTful-APIs, die schreibgeschützte Operationen wie Abfragen von Metriken ausführen.</td>
  </tr>
</table>


## Bluemix-IAM-Rollen
{: #iam_roles}

In der folgenden Tabelle wird die Beziehung zwischen der API, einer Serviceaktion und einer IAM-Rolle aufgelistet, die vom {{site.data.keyword.monitoringshort}}-Service verwendet wird.

<table>
  <caption>Tabelle 3. Beziehung zwischen der API, einer Serviceaktion und einer IAM-Rolle. </caption>
  <tr>
    <th>API</th>
	<th>Aktion</th>
	<th>IAM-Rolle</th>
	<th>Beschreibung</th>
  </tr>
  <tr>
    <td>POST /v1/metrics</td>
    <td>domain.write</td>
	<td>Administrator, Editor, Operator</td>
	<td>Metriken an die Domäne senden</td>
  </tr>
  <tr>
    <td>GET /v1/metrics</td>
    <td>domain.render</td>
	<td>Administrator, Editor, Viewer</td>
	<td>Metriken abrufen/abfragen</td>
  </tr>
  <tr>
    <td>GET /v1/metrics/list</td>
    <td>domain.find</td>
	<td>Administrator, Editor</td>
	<td>In der Domäne nach Metriken suchen</td>
  </tr>
</table>

## Sicherheitstoken oder API-Schlüssel abrufen
{: #get_token}

Verwenden Sie das {{site.data.keyword.Bluemix_notm}}-UAA-Modell, um ein Authentifizierungstoken abzurufen, das Sie für den Zugriff auf Daten verwenden können, die im {{site.data.keyword.monitoringshort}}-Service für einen Bereich in {{site.data.keyword.Bluemix_notm}} gespeichert sind. Sie können das Authentifizierungstoken mithilfe der {{site.data.keyword.Bluemix_notm}}-Befehlszeilenschnittstelle oder der `Anmelde-REST-API` abrufen. Weitere Informationen finden Sie in [UAA-Token über die {{site.data.keyword.Bluemix_notm}}-Befehlszeilenschnittstelle abrufen](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_cli) und [UAA-Token über die API abrufen](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_api).

Verwenden Sie das {{site.data.keyword.Bluemix_notm}}-IAM-Modell, um ein Authentifizierungstoken abzurufen, das Sie für den Zugriff auf Daten verwenden können, die im {{site.data.keyword.monitoringshort}}-Service gespeichert sind. Oder verwenden Sie das Modell, um einen API-Schlüssel abzurufen. Das Token hat eine Ablaufzeit. Der API-Schlüssel läuft nicht ab. Weitere Informationen finden Sie in [IAM-Token über die {{site.data.keyword.Bluemix_notm}}-Befehlszeilenschnittstelle abrufen](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli), [IAM-API-Schlüssel über die {{site.data.keyword.Bluemix_notm}}-Befehlszeilenschnittstelle generieren](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli) oder [IAM-API-Schlüssel über die {{site.data.keyword.Bluemix_notm}}-Benutzerschnittstelle generieren](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_ui).



