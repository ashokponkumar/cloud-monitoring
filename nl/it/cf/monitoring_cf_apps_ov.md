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

# Monitoraggio delle applicazioni in esecuzione in Cloud Foundry
 {:#monitoring_bluemix_apps}

Per inviare le metriche nel servizio {{site.data.keyword.monitoringshort}}, un'applicazione Cloud Foundry (CF) può inviare le metriche direttamente dall'applicazione utilizzando l'API REST {{site.data.keyword.monitoringshort}}.
{:shortdesc}

Quando utilizzi l'infrastruttura Cloud Foundry per eseguire le tue applicazioni in {{site.data.keyword.Bluemix_notm}}, potresti voler monitorare lo stato di integrità dell'applicazione, l'utilizzo delle risorse e le metriche di traffico. Mediante queste informazioni sulle prestazioni, puoi prendere decisioni o agire di conseguenza.


Un'applicazione in esecuzione su Cloud Foundry non può eseguire collectd. I dati devono essere inviati invece dall'applicazione stessa, utilizzando un client lumberjack o un'API RESTful. 

##Ulteriori modi per inviare dati da un'applicazione CF [@lindj ha creato un pacchetto di build collectd e statsd] (https://github.com/jimlindeman/python-buildpack-collectd)

Si tratta di un clone della release python-buildpack in cui è installato collectd per l'ascolto su localhost:8125 con il plug-in di input statsd e il plug-in di output Graphite a più tenant Bluemix. 

Puoi utilizzare un pacchetto di build per distribuire un'applicazione CF in Bluemix. Il pacchetto di build fornisce il framework e il supporto di runtime per l'applicazione. Puoi installare e configurare collectd per l'ascolto su localhost:8125 con il plug-in di input statsd e il plug-in di output Graphite a più tenant Bluemix. 
