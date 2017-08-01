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

# Surveillance des applications s'exécutant dans Cloud Foundry
 {:#monitoring_bluemix_apps}

Une application Cloud Foundry (CF) peut envoyer des métriques à {{site.data.keyword.monitoringshort}} directement depuis l'application à l'aide de l'API REST {{site.data.keyword.monitoringshort}}.
{:shortdesc}

Si vous utilisez l'infrastructure Cloud Foundry pour exécuter vos applications dans {{site.data.keyword.Bluemix_notm}}, vous pouvez décider de surveiller l'état de santé de l'application, l'utilisation des ressources et le trafic. Ces informations sur les performances vous permettent de prendre des décisions ou des mesures en conséquence.


Une application qui s'exécute dans Cloud Foundry ne peut pas exécuter collectd. A la place, les données doivent être envoyées depuis l'application elle-même, à l'aide d'un client lumberjack ou d'une API RESTful. 

##Autres moyens d'envoyer des données depuis une application CF [@lindj a créé un pack de construction collectd et statsd] (https://github.com/jimlindeman/python-buildpack-collectd)

Il s'agit d'un clone de l'édition python-buildpack où collectd est installé pour une écoute sur localhost:8125 avec le plug-in d'entrée statsd et le plug-in de sortie graphite à service partagé de Bluemix. 

Vous pouvez utiliser un pack de construction pour déployer une application CF dans Bluemix. Le pack de construction fournit l'infrastructure et le support de contexte d'exécution pour l'application. Vous pouvez installer et configurer collectd pour une écoute sur localhost:8125 avec le plug-in d'entrée statsd et le plug-in de sortie graphique à service partagé de Bluemix. 
