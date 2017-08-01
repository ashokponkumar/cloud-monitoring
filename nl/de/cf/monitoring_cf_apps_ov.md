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

# Überwachung von Apps, die in Cloud Foundry ausgeführt werden
 {:#monitoring_bluemix_apps}

Um Metriken an den {{site.data.keyword.monitoringshort}}-Service zu senden, kann eine Cloud Foundry-Anwendung (CF-Anwendung) die Metriken mithilfe der {{site.data.keyword.monitoringshort}}-REST-API direkt von der App senden.
{:shortdesc}

Wenn Sie die Cloud Foundry-Infrastruktur verwenden, um Ihre Apps in {{site.data.keyword.Bluemix_notm}} auszuführen, möchten Sie möglicherweise den Status der App, die Ressourcennutzung und die Metriken des Datenverkehrs überwachen. Mit diesen Leistungsdaten können Sie Entscheidungen treffen oder entsprechende Maßnahmen ergreifen.


Eine App, die in Cloud Foundry ausgeführt wird, kann nicht mit 'collectd' ausgeführt werden. Die Daten müssen stattdessen von der App selbst mithilfe eines Lumberjack-Client oder einer RESTful-API gesendet werden. 

##Weitere Möglichkeiten zum Senden von Daten von einer CF-App [@lindj erstellte ein 'collectd'- und 'statsd'-Buildpack] (https://github.com/jimlindeman/python-buildpack-collectd)

Dies ist ein Klon des Python-Buildpack-Releases, bei dem 'collectd' installiert ist, um an localhost:8125 mit dem 'statsd'-Eingabe-Plug-in und dem Multi-Tenant-Graphite-Ausgabe-Plug-in von Bluemix empfangsbereit zu sein. 

Sie können ein Buildpack verwenden, um eine CF-App in Bluemix zu implementieren. Das Buildpack bietet das Framework und die Laufzeitunterstützung für die App. Sie können 'collectd' installieren und so konfigurieren, dass es an localhost:8125 mit dem statsd-Eingabe-Plug-in und dem Multi-Tenant-Ausgabe-Plug-in von Bluemix empfangsbereit ist. 
