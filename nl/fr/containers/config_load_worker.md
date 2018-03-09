---

copyright:
  years: 2017, 2018

lastupdated: "2018-02-07"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}



# Configuration de métriques d'unité centrale pour un agent dans Grafana
{: #config_load_worker}

Dans {{site.data.keyword.Bluemix}}, certaines métriques de charge pour un agent sont automatiquement collectées. Pour les surveiller via {{site.data.keyword.monitoringlong}}, vous devez définir une requête Grafana.
{:shortdesc}

Pour obtenir la liste des métriques de charge qui sont automatiquement collectées, voir [Métriques de charge pour les agents](/docs/services/cloud-monitoring/containers/monitoring_containers_ov.html#load_metrics_workers).


## Etape 1 : Collecte de données pour l'agent à surveiller
{: #step1}

Procurez-vous les informations suivantes pour l'agent que vous souhaitez surveiller :

* **Région** dans laquelle le cluster s'exécute. 
* **Nom du cluster** dans lequel le conteneur s'exécute.  
* **Nom de l'agent** que vous souhaitez surveiller.  

Déterminez si les métriques sont transmises à un domaine d'espace ou au domaine de compte. 

Pour savoir où les métriques sont transmises par votre cluster, exécutez la commande suivante :

```
$ bx cs cluster-get ClusterName --json
```
{: codeblock}

où *ClusterName* est le nom de votre cluster.

Dans la sortie, les zones suivantes indiquent l'emplacement où les métriques sont transmises :

* **logOrg** définit l'ID d'une organisation CF. 
* **logOrgName** définit le nom d'une organisation CF. 
* **logSpace** définit l'ID d'un espace CF. 
* **logSpaceName** définit le nom d'un espace CF. 

Si les zones sont vides, les métriques sont transmises au domaine de compte.
Si une organisation CF et un espace CF sont définis dans ces zones, les métriques sont transmises au domaine d'espace qui est associé à cet espace. 

## Etape 2 : Lancement de Grafana
{: #step2}

Démarrez Grafana depuis un navigateur. Pour plus d'informations, voir [Accès au tableau de bord Grafana depuis un navigateur Web](/docs/services/cloud-monitoring/grafana/navigating_grafana.html#launch_grafana_from_browser).

Assurez-vous que, dans Grafana, vous êtes connecté au compte dans lequel le cluster s'exécute.  

1. A partir d'un navigateur, démarrez Grafana.  

    Entrez l'adresse URL du service {{site.data.keyword.monitoringshort}} pour la région où vous avez créé le cluster. 
    
    Pour obtenir les adresses URL par région, voir [URL pour le service de surveillance](/docs/services/cloud-monitoring/monitoring_ov.html#region).

    Par exemple, pour la région Sud des Etats-Unis, démarrez : [https://metrics.ng.bluemix.net/](https://metrics.ng.bluemix.net/). 

2. Définissez le domaine {{site.data.keyword.monitoringshort} dans lequel vous pourrez visualiser les métriques de cluster. 

    Dans Grafana, sélectionnez votre ID. Ensuite, assurez-vous que vous êtes connecté au compte approprié, puis choisissez un domaine. 

    Les clusters auxquels un espace CF est associé transmettent les métriques au domaine de métriques d'espace. Sélectionnez `Domain = space`, ainsi que l'organisation et l'espace qui sont associés à votre cluster. 

    Les clusters qui sont créés au niveau compte transmettent les métriques au domaine de métriques de compte. Sélectionnez `Domain = account`



## Etape 3 : Définition d'une requête dans Grafana pour une métrique d'unité centrale
{: #step3}

Pour créer un tableau de bord Grafana et définir une requête visant à surveiller l'utilisation de l'unité centrale par un agent, procédez comme suit :

1. Créez un tableau de bord.

    * Sélectionnez le bouton de basculement de la barre de menus latérale ![Barre de menus latérale de Grafana](images/grafana_settings.gif "Barre de menus latérale de Grafana").
    * Sélectionnez **Dashboards**.
    * Cliquez sur **New**.

    Un tableau de bord s'ouvre. Il comporte une ligne vide prête pour la configuration.

2. Ajoutez un panneau *Graph* afin de surveiller une métrique d'unité centrale pour un pod. 

    1. Sélectionnez **Graph**.

    2. Cliquez sur le titre du graphique, puis sélectionnez **edit**.

        L'onglet *Metrics* s'ouvre. Il affiche la source de données par défaut.

3. Définissez la requête qui filtre les données affichées dans le graphique. 

    Pour plus d'informations sur le format de la requête, voir [Format de requête des métriques de charge collectées pour les agents](/docs/services/cloud-monitoring/reference/metrics_format.html#load_workers).

    Dans l'onglet *Metrics*, sélectionnez **Add query**. <br>Une entrée de requête est ajoutée. Chaque requête est associée à une lettre.
	
	Procédez comme suit pour définir la requête :

    1. Cliquez sur **Select metric** pour spécifier la source, puis choisissez `ibmcloud`.
    
    2. Cliquez sur **Select metric** pour spécifier le type de cloud, puis choisissez `public`.
    
    3. Cliquez sur **Select metric** pour indiquer le nom du service, puis sélectionnez `containers-kubernetes`.
	
    4. Cliquez sur **Select metric** pour spécifier la région, puis choisissez la région dans laquelle votre cluster s'exécute. Par exemple, `us-south`.
    
    5. Cliquez sur **Select metric** pour spécifier le nom de cluster, puis choisissez le nom du cluster dans lequel votre conteneur s'exécute. 
		
	6. Cliquez sur **Select metric** pour spécifier le nom de l'agent que vous souhaitez surveiller. 
	
	7. Cliquez sur **Select metric** pour spécifier le type de métrique, puis cliquez sur **Select metric** pour spécifier le sous-type de métrique. 
	
	    Pour obtenir une liste de métriques d'unité cenrale, voir [Métriques d'unité centrale pour les agents](/docs/services/cloud-monitoring/containers/monitoring_containers_ov.html#load_metrics_workers).
	
	10. Cliquez sur le signe plus ![Icône Ajouter](images/grafana_plus_image.gif "Signe Plus"), puis choisissez une fonction. Vous pouvez ajouter une fonction pour transformer et combiner des données disponibles pour une métrique, et effectuer des calculs sur ces données.

        Par exemple, vous pouvez ajouter la fonction **alias(newName)** afin d'ajouter un alias pour une métrique. Cet alias sera utilisé pour afficher une chaîne à la place du nom de la métrique dans la légende du graphique.

        Afin d'ajouter un alias pour votre métrique, procédez comme suit :

        1. Cliquez sur le signe plus.
        2. Sélectionnez **Special**.
        3 Sélectionnez **alias**.
        4. Entrez une chaîne, par exemple `Mon exemple de métrique`.

4. Sauvegardez le tableau de bord en vue de sa réutilisation ultérieure.

    1. Cliquez sur l'icône de sauvegarde du tableau de bord ![Icône Sauvegarder le tableau de bord](images/grafana_save_image.gif "Icône Sauvegarder le tableau de bord").
    2. Entrez le nom du tableau de bord.
    3. Cliquez sur **Save**.

