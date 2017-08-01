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


# Questions fréquentes sur l'utilisation de Grafana et réponses
{: #grafana_qa}

Vous trouverez ci-après les réponses aux questions fréquentes concernant l'utilisation de Grafana avec le service {{site.data.keyword.monitoringshort}}. 
{:shortdesc}

* [J'obtiens une erreur 404 lorsque je me connecte à l'interface utilisateur Web du service Monitoring](/docs/services/cloud-monitoring/qa/grafana_qa.html#404)
* [J'ai téléchargé des données json pour un tableau de bord Grafana ; pourquoi ai-je perdu ma barre de défilement ?](/docs/services/cloud-monitoring/qa/grafana_qa.html#2)


## J'obtiens une erreur 404 lorsque je me connecte à l'interface utilisateur Web du service Monitoring
{: #404}

Si vous obtenez une erreur 404 lorsque vous tentez de vous connecter à l'interface utilisateur Web du service {{site.data.keyword.monitoringshort}} (Grafana), vérifiez que l'espace existe et que vous y avez accès.

Lorsque vous utilisez le [modèle d'authentification UAA](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_uaa) pour accéder au service {{site.data.keyword.monitoringshort}}, vous devez vous servir de l'ID utilisateur et du mot de passe que vous avez utilisés pour vous connecter à la console {{site.data.keyword.Bluemix_notm}}. 

Pour vérifier que vous avez accès au compte, à l'organisation et à l'espace auxquels vous voulez vous connecter, connectez-vous à la console {{site.data.keyword.Bluemix_notm}} et accédez à l'espace. 

Vous pouvez aussi utiliser la ligne de commande pour vérifier que vous avez accès à cet espace. Exécutez la commande suivante pour vous connecter à une région, une organisation et un espace {{site.data.keyword.Bluemix_notm}} :

```
    bx login -a https://api.ng.bluemix.net
    ```
{: codeblock}

Suivez les instructions. Entrez vos données d'identification {{site.data.keyword.Bluemix_notm}} et sélectionnez une organisation et un espace.


## Je viens de télécharger des données JSON pour un tableau de bord Grafana ; pourquoi ai-je perdu ma barre de défilement ?
{: #2}

Un bogue dans Grafana masque la barre de défilement après l'importation d'un tableau de bord. 

Pour afficher la barre de défilement, sauvegardez le tableau de bord après l'avoir importé. 








