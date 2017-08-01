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


# Häufige Fragen und Antworten bei der Verwendung von Grafana
{: #grafana_qa}

Hier sind die Antworten auf häufig gestellte Fragen zur Verwendung von Grafana mit dem {{site.data.keyword.monitoringshort}}-Service. 
{:shortdesc}

* [Ich bekomme den Fehler 404, wenn ich mich bei der Webbenutzerschnittstelle des Überwachungsservice anmelde.](/docs/services/cloud-monitoring/qa/grafana_qa.html#404)
* [Ich habe gerade JSON-Daten für ein Grafana-Dashboard hochgeladen. Warum wird meine Bildlaufleiste nicht mehr angezeigt?](/docs/services/cloud-monitoring/qa/grafana_qa.html#2)


## Ich bekomme den Fehler 404, wenn ich mich bei der Webbenutzerschnittstelle des Überwachungsservice mit dem UUA-Authentifizierungsmodell anmelde.
{: #404}

Wenn Sie den Fehler 404 erhalten, nachdem Sie versucht haben, sich bei der Webbenutzerschnittstelle (Grafana) des {{site.data.keyword.monitoringshort}}-Service anzumelden, überprüfen Sie, dass der Bereich vorhanden ist und dass Sie darauf zugreifen dürfen.

Wenn Sie das [UAA-Authentifizierungsmodell](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_uaa) für den Zugriff auf den {{site.data.keyword.monitoringshort}}-Service verwenden, müssen Sie dieselbe Benutzer-ID und dasselbe Kennwort wie für die Anmeldung bei der {{site.data.keyword.Bluemix_notm}}-Konsole verwenden. 

Um zu überprüfen, dass Sie Zugriff auf das Konto, die Organisation und den Bereich haben, in dem Sie sich anmelden möchten, melden Sie sich an der {{site.data.keyword.Bluemix_notm}}-Konsole an und wechseln in den betreffenden Bereich. 

Sie können auch die Befehlszeile verwenden, um zu überprüfen, ob Sie Zugriff auf diesen Bereich haben. Führen Sie folgenden Befehl aus, um sich in einer Region, einer Organisation und einem Bereich von {{site.data.keyword.Bluemix_notm}} anzumelden:

```
bx login -a https://api.ng.bluemix.net
```
{: codeblock}

Befolgen Sie die Anweisungen. Geben Sie Ihre {{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise ein, wählen Sie eine Organisation und einen Bereich aus.


## Ich habe gerade JSON-Daten für ein Grafana-Dashboard hochgeladen. Warum wird meine Bildlaufleiste nicht mehr angezeigt?
{: #2}

Dies ist ein Fehler in Grafana, der die Bildlaufleiste nach dem Import eines Dashboards ausblendet. 

Speichern Sie das Dashboard nach dem Importieren, um die Bildlaufleiste wieder einzublenden. 








