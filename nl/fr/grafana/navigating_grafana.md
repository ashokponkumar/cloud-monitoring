---

copyright:
  years: 2017

lastupdated: "2017-05-25"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

# Accès au tableau de bord Grafana
{:#navigating_grafana}

Dans {{site.data.keyword.Bluemix}}, vous pouvez utiliser Grafana, plateforme de visualisation et d'analyse open source, pour surveiller, rechercher, analyser et visualiser vos métriques dans différents graphiques, par exemple, dans des diagrammes et des tableaux. Vous pouvez utiliser Grafana pour effectuer des tâches analytiques avancées.
{:shortdesc}

Vous pouvez lancer Grafana d'une des manières suivantes :

* Depuis {{site.data.keyword.Bluemix_notm}}

    Vous pouvez accéder dans Grafana à vos métriques de conteneur spécifiques à Docker, dans le contexte du conteneur concerné. Cette fonctionnalité ne s'applique qu'aux conteneurs déployés dans l'infrastructure de cloud gérée par {{site.data.keyword.Bluemix_notm}}. 
    
    Pour plus d'informations, voir [Accès au tableau de bord Grafana depuis le tableau de bord {{site.data.keyword.Bluemix_notm}}](navigating_grafana.html#launch_grafana_from_bluemix).

* A partir d'un lien de navigateur direct

    Vous pouvez lancer Grafana afin que les données affichées agrègent les journaux des services dans l'espace {{site.data.keyword.Bluemix_notm}} désigné.
    
    Pour plus d'informations, voir [Accès au tableau de bord Grafana depuis un navigateur Web](navigating_grafana.html#launch_grafana_from_browser).
    
Pour plus d'informations sur Grafana, reportez-vous au manuel [Grafana User Guide ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://docs.grafana.org/guides/getting_started/){: new_window}.


##  Accès au tableau de bord Grafana depuis le tableau de bord Bluemix
{: #launch_grafana_from_bluemix}

**Remarque :** cette fonctionnalité ne s'applique qu'aux conteneurs déployés dans l'infrastructure de cloud gérée par {{site.data.keyword.Bluemix_notm}}. 

La requête utilisée pour filtrer les données affichées dans Grafana extrait les données pour le conteneur {{site.data.keyword.Bluemix_notm}} depuis lequel vous lancez Kibana. 

Pour examiner les métriques d'un conteneur Docker dans Grafana, procédez comme suit :

1. Connectez-vous à {{site.data.keyword.Bluemix_notm}}, puis cliquez sur le conteneur depuis le tableau de bord {{site.data.keyword.Bluemix_notm}}. 
    
2. Dans la barre de navigation, cliquez sur **Surveillance et journaux**. L'onglet Surveillance s'ouvre. 
    
3. Cliquez sur **Vue avancée**. Le tableau de bord **Grafana** s'ouvre.


##  Accès au tableau de bord Grafana depuis un navigateur Web
{: #launch_grafana_from_browser}

La requête utilisée pour filtrer les données affichées dans Grafana extrait les données relatives à un espace dans l'organisation {{site.data.keyword.Bluemix_notm}}. Les informations de métriques affichées par Grafana couvrent les enregistrements pour toutes les ressources déployées dans l'espace de l'organisation {{site.data.keyword.Bluemix_notm}} à laquelle vous êtes connecté.

Pour lancer Grafana depuis un navigateur, procédez comme suit :

1. Ouvrez l'URL [https://metrics.ng.bluemix.net](https://metrics.ng.bluemix.net) pour vous connecter à l'interface utilisateur de Grafana. 
2. Sélectionnez **Grafana**.
     

