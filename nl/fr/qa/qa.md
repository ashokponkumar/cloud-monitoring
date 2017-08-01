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


# Questions fréquentes et réponses
{: #qa}

Vous trouverez ci-après les réponses aux questions fréquentes concernant le service {{site.data.keyword.monitoringshort}}. 
{:shortdesc}

* [Pourquoi ne puis-je pas voir mes anciennes données pour une métrique pour laquelle je n'ai pas envoyé de valeurs récemment ?](#qa3)


## Pourquoi ne puis-je pas voir mes anciennes données pour une métrique pour laquelle je n'ai pas envoyé de valeurs récemment ?
{: #qa3}

{{site.data.keyword.monitoringshort}} supprime toutes les données pour un chemin de métrique qui semble transitoire par nature en identifiant les métriques qui n'ont pas été actualisées au cours des 7 derniers jours.  

Par exemple :

* Lorsqu'un conteneur est supprimé, les métriques qui lui sont associées sont conservées pendant une période de 7 jours, puis sont supprimées.
* Si vous disposez d'une jauge statsd appelée `<space_id>.test.statsd.gauge-hello` mais que vous ne l'actualisez pas pendant une semaine, la métrique est identifiée comme transitoire, puis supprimée, ainsi que toutes ses informations d'historique. 

