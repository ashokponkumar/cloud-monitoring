---

copyright:
  years: 2017
lastupdated: "2017-06-26"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Initiation à IBM Cloud Monitoring dans Bluemix
{: #getting-started-with-ibm-cloud-monitoring}

Ce tutoriel d'initiation va vous guider tout au long des étapes d'analyse d'un conteneur à l'aide du service {{site.data.keyword.monitoringlong}}. Apprenez à rechercher et à analyser des métriques de conteneur pour une application déployée dans un cluster Kubernetes.
{:shortdesc}

## Avant de commencer
{: #prereqs}

Créez un [compte Bluemix](https://console.bluemix.net/registration/). Votre ID utilisateur doit être membre ou propriétaire d'un compte Bluemix disposant des droits qui permettent de créer des clusters Kubernetes, de déployer des applications dans des clusters, et d'interroger les journaux dans Bluemix en vue d'une analyse approfondie dans Kibana.

Ouvrez une session de terminal à partir de laquelle vous allez gérer le cluster Kubernetes et déployer des applications depuis la ligne de commande. Les exemples de ce tutoriel utilisent un système Ubuntu Linux.

[Installez les plug-in d'interface de ligne de commande](/docs/containers/cs_cli_install.html#cs_cli_install_steps) dans votre environnement local afin de gérer le service IBM Bluemix Container depuis la ligne de commande. 



## Etape 1 : Déploiement d'une application dans un conteneur
{: #step1}

Procédez comme suit pour déployer un conteneur dans un cluster Kubernetes :

1. [Créez un cluster Kubernetes](/docs/containers/cs_cluster.html#cs_cluster_ui).

2. [Configurez le contexte de cluster](/docs/containers/cs_cli_install.html#cs_cli_configure) dans un terminal Linux. Une fois le contexte défini, vous pouvez gérer le cluster Kubernetes et déployer l'application dans ce cluster.

3. Déployez et exécutez un modèle d'application dans le cluster Kubernetes. [Exécutez les étapes de la leçon 1](/docs/containers/cs_tutorials.html#cs_apps_tutorial).

    L'application est une application Hello World Node.js :

    ```
    var express = require('express')
var app = express()

    app.get('/', function(req, res) {
      res.send('Hello world! Your app is up and running in a cluster!\n')
    })
    app.listen(8080, function() {
      console.log('Sample app is listening on port 8080.')
    })
    ```

    Lorsque l'application est déployée, la collecte de métriques est automatiquement activée. 



## Etape 2 : Accès au tableau de bord Grafana
{: #step2}

Lancez Grafana depuis un navigateur. 

Pour analyser les métriques pour un cluster, vous devez accéder à Grafana dans la région cloud Public où le cluster a été créé. 
    
Ensuite, depuis un navigateur, ouvrez l'adresse URL de Grafana : `https://metrics.ng.bluemix.net/` 
    
    
## Etape 3 : Analyse des métriques dans Grafana
{: #step3}

Pour créer un tableau de bord Grafana, procédez comme suit :
    
1. Créez un tableau de bord.

    * Sélectionnez le bouton de basculement de la barre de menus latérale ![Barre de menus latérale de Grafana](images/grafana_settings.gif "Barre de menus latérale de Grafana"). 
    * Sélectionnez **Dashboards**. 
    * Cliquez sur **New**.
    
    Un tableau de bord s'ouvre. Il comporte une ligne vide prête pour la configuration. 
    
    ![Tableau de bord Grafana](images/grafana4_f1.gif "Tableau de bord Grafana")
    
     Dans Grafana, ajoutez des lignes pour diviser le tableau de bord en sections. Une ligne regroupe un ou plusieurs panneaux. Dans une ligne, un panneau constitue l'unité de visualisation la plus petite que vous pouvez configurer pour l'affichage des données d'une métrique. Par exemple, vous pouvez choisir un panneau de graphique ou un panneau de tableau. Vous pouvez faire glisser des panneaux et les déposer à l'emplacement de votre choix afin de les réorganiser dans un tableau de bord. Les données affichées dans un panneau sont configurées via des requêtes. Vous pouvez définir une ou plusieurs requêtes dans un panneau. Chaque requête représente un ensemble différent de données. Vous pouvez aussi définir l'intervalle pour un panneau. Normalement, il est défini par le sélecteur de temps du *tableau de bord*.
    
2. Ajoutez un panneau *Graph* afin de surveiller les nanosecondes de temps UC sur tous les coeurs d'un conteneur.
    
    1. Sélectionnez **Graph**.
    
    2. Cliquez sur le titre du graphique, puis sélectionnez **edit**.
    
        L'onglet *Metrics* s'ouvre. Il affiche la source de données par défaut.
    
        ![Panneau du graphique comportant des onglets de configuration](images/grafana4_f2.gif "Panneau du graphique comportant des onglets de configuration")
    
3. Définissez la requête qui filtre les données affichées dans le graphique. 

    Le tableau suivant présente les différentes zones requises pour configurer une requête qui filtre les données pour une métrique de conteneur :

    <table>
      <caption>Tableau 1. Zones de requête Grafana pour les conteneurs</caption>
      <tr>
        <th align="center">Zone</th>
        <th align="center">Description</th>
        <th align="center">Valeurs valides</th>
      </tr>
      <tr>
        <td>Prefix</td>
        <td>Préfixe pour les métriques des conteneurs. <br><br>Ce préfixe s'applique aux données collectées pour les conteneurs qui sont déployés dans un cluster Kubernetes.</td>
        <td>`crn`</td>
      </tr>
      <tr>
        <td>Version</td>
        <td>Version des données de métrique collectées.</td>
        <td>`v1`</td>
      </tr>
      <tr>
        <td>Provider</td>
        <td>Fournisseur de cloud dans lequel les données sont collectées.</td>
        <td>`bluemix`</td>
      </tr>
      <tr>
        <td>Type</td>
        <td>Environnement de cloud dans lequel les données sont collectées.</td>
        <td>`public`</td>
      </tr>
      <tr>
        <td>Source</td>
        <td>Infrastructure de cloud dans laquelle les métriques sont collectées.</td>
        <td>`containers-kubernetes`</td>
      </tr>
      <tr>
        <td>Region</td>
        <td>Région de cloud dans laquelle les métriques sont collectées.</td>
        <td>* `ng` <br>* `eu-gb` <br>* `eu-de` </td>
      </tr>
      <tr>
        <td>Account</td>
        <td>Identificateur global unique du compte sur lequel les métriques sont collectées. <br>Le format de cette zone est le suivant : `a_*ID*` où ID est l'identificateur global unique du compte.</td>
        <td></td>
      </tr>
      <tr>
        <td>Cluster</td>
        <td>Identificateur global unique du cluster dans lequel les métriques sont collectées.</td>
        <td></td>
      </tr>
      <tr>
        <td>Container metric</td>
        <td>Métriques collectées pour un conteneur.</td>
        <td>* `memory_current` <br>* `memory_limit` <br>* `cpu_usage` <br>* `cpu_usage_pct` <br>* `cpu_num_cores`</td>
      </tr>
      <tr>
        <td>Container in a pod</td>
        <td>Combinaison d'identificateurs globaux uniques et de noms de ressource Kubernetes requis pour identifier de façon unique un conteneur s'exécutant dans un pod. <br> Le format de cette zone est le suivant : *{namespace}_#{pod_name}_#{container_name}_#{container_id}* <br><br>**Remarque :** lorsque vous consultez la liste des options disponibles pour cette entrée dans la requête, notez qu'il existe également une entrée au format : *{namespace}_#{pod_name}_#{container_name}_POD_#{container_id}*. Elle correspond aux ID de conteneur internes qui sont créés par Kubernetes.</td>
        <td></td>
      </tr>
      <tr>
        <td>Functions</td>
        <td>Fonction de requête que vous pouvez sélectionner pour visualiser une métrique de conteneur dans le panneau. <br>Pour plus d'informations, voir [Functions ![(External link icon)](../icons/launch-glyph.svg "External link icon")](http://graphite.readthedocs.io/en/latest/functions.html){: new_window}</td>
        <td></td>
      </tr>
    </table>
    
    L'image suivante représente la façon dont la requête est affichée dans Grafana une fois que vous l'avez configurée :
    
    ![Exemple de requête](images/grafana4_query_f3.gif "Exemple de requête")
    
    Procédez comme suit pour définir la requête :
    
    Dans l'onglet *Metrics*, sélectionnez **Add query**. <br>Une entrée de requête est ajoutée. Chaque requête est associée à une lettre.
    
    ![Nouvelle entrée de requête](images/grafana4_query_f1.gif "Nouvelle entrée de requête")
        
    1. Cliquez sur **Select metric**, puis choisissez `crn`.
    
    2. Cliquez sur **Select metric**, puis choisissez `v1`.
    
    3. Cliquez sur **Select metric**, puis choisissez `bluemix`.
    
    4. Cliquez sur **Select metric**, puis choisissez `public`.
    
    5. Cliquez sur **Select metric**, puis choisissez `containers-kubernetes`.
    
    6. Cliquez sur **Select metric**, puis choisissez la région que vous utilisez, par exemple `us-south`.
    
    7. Cliquez sur **Select metric**, puis choisissez l'ID de compte pour lequel afficher des données, par exemple `a_91d1d1exxxxxxx4df920bbd06461b066`.
    
    8. Cliquez sur **Select metric**, puis choisissez l'ID de cluster.
    
    9. Cliquez sur **Select metric**, puis choisissez une métrique de conteneur. Pour surveiller l'*utilisation de l'unité centrale* d'un conteneur, choisissez `container-metric-cpu_usage`.
    
    10. Cliquez sur **Select metric**, puis choisissez l'ID qui correspond au conteneur pour lequel surveiller l'utilisation de l'unité centrale, par exemple `default_hello-world-deployment-3355293961-0fwkg_hello-world-deployment_ad5eb446a493db31f1d9eb158f5de915fc063d6c136823488b694e63bb00aa57`.
    
    11. Cliquez sur le signe plus ![Icône Ajouter](images/grafana_plus_image.gif "Signe Plus"), puis choisissez une fonction. Vous pouvez ajouter une fonction pour transformer et combiner des données disponibles pour une métrique, et effectuer des calculs sur ces données.
        
        Par exemple, vous pouvez ajouter la fonction **alias(newName)** afin d'ajouter un alias pour une métrique. Cet alias sera utilisé pour afficher une chaîne à la place du nom de la métrique dans la légende du graphique.
        
        Afin d'ajouter un alias pour votre métrique, procédez comme suit :
        
        1. Cliquez sur le signe plus.
        2. Sélectionnez **Special**. 
        3 Sélectionnez **alias**.
        4. Entrez une chaîne, par exemple `Mon exemple de métrique`.
        
4. Sauvegardez le tableau de bord en vue de sa réutilisation ultérieure. 

    1. Cliquez sur l'icône de sauvegarde du tableau de bord ![Icône Sauvegarder le tableau de bord](images/grafana_save_image.gif "Icône Sauvegarder le tableau de bord"). 
    
        ![Sauvegarder le tableau de bord](images/grafana_save_dashboard.gif "Sauvegarder le tableau de bord")
    
    2. Entrez le nom du tableau de bord.
    3. Cliquez sur **Save**.


## Etapes suivantes
{: #next_steps}

Définissez une alerte pour une métrique. Pour plus d'informations, voir [Configuration d'alertes](/docs/services/cloud-monitoring/config_alerts_ov.html#config_alerts_ov).



