---

copyright:
  years: 2017

lastupdated: "2017-07-14"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Häufige Fragen und Antworten
{: #qa}

Hier sind die Antworten auf häufig gestellte Fragen zum {{site.data.keyword.monitoringshort}}-Service. 
{:shortdesc}

* [Warum kann ich meine alten Daten für eine Metrik nicht sehen, an die ich kürzlich keine Metrikwerte gesendet habe?](#qa3)


## Warum kann ich meine alten Daten für eine Metrik nicht sehen, an die ich kürzlich keine Metrikwerte gesendet habe?
{: #qa3}

{{site.data.keyword.monitoringshort}} löscht alle Daten für einen Metrikpfad, der transient zu sein scheint, indem Metriken identifiziert werden, in die in den letzten 7 Tagen nicht geschrieben wurde.  

Beispiel:

* Wenn ein Container gelöscht wurde, bleiben die Metriken, die zu diesem Container gehörten, 7 Tage nach dem Löschen der Metriken erhalten.
* Wenn Sie eine statsd-Messanzeige mit dem Namen `<space_id>.test.statsd.gauge-hello` haben und eine Woche lang nicht in diese schreiben, wird diese Metrik zusammen mit allen archivierten Informationen gelöscht. 

