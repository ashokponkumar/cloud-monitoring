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


# Analyse des métriques dans Grafana pour une application qui est déployée dans un cluster Kubernetes
{: #container_service_metrics}

Utilisez ce tutoriel pour apprendre à utiliser le service {{site.data.keyword.monitoringlong}} afin de surveiller les performances de votre conteneur.
{:shortdesc}


## Objectifs
{: #objectives}

Apprenez à rechercher et à analyser des métriques de conteneur pour une application déployée dans un cluster Kubernetes :


1. Identifiez l'emplacement où les métriques qui sont collectées dans un cluster sont envoyées au service {{site.data.keyword.monitoringshort}}.  
2. Lancez Grafana et définissez le domaine {{site.data.keyword.monitoringshort} dans lequel vous pourrez visualiser les métriques de cluster. 
3. Recherchez et analysez les métriques de conteneur pour une application qui est déployée dans un cluster Kubernetes sous {{site.data.keyword.Bluemix_notm}}.

Ce tutoriel vous guide dans les étapes du scénario de bout en bout suivant dans {{site.data.keyword.Bluemix_notm}} : mise à disposition d'un cluster, identification de l'emplacement où le cluster envoie des métriques au service {{site.data.keyword.monitoringshort}} dans {{site.data.keyword.Bluemix_notm}}, déploiement d'une application dans le cluster et utilisation de Grafana pour visualiser et filtrer des métriques de conteneur pour ce cluster.


**Remarque :** pour suivre ce tutoriel, vous devez remplir les conditions prérequises décrites ci-après et suivre les tutoriels qui sont liés à partir des différentes étapes.


## Conditions prérequises
{: #prereqs}

1. Etre membre ou propriétaire d'un compte {{site.data.keyword.Bluemix_notm}} doté des droits permettant de créer des clusters standard Kubernetes, de déployer des applications dans des clusters et d'interroger les métriques dans {{site.data.keyword.Bluemix_notm}} à des fins de surveillance dans Grafana.

    Les règles suivantes doivent être affectées à votre ID utilisateur pour {{site.data.keyword.Bluemix_notm}} :
    
    * Une règle IAM pour {{site.data.keyword.containershort}} avec les droits *operator* ou *administrator*.
    * Un rôle CF pour l'espace dans lequel le service {{site.data.keyword.monitoringshort}} est mis à disposition, avec les droits *developer*.
    
    Pour plus d'informations, voir [Affectation d'une règle IAM à un utilisateur via l'interface utilisateur IBM Cloud](/docs/services/cloud-monitoring/security/grant_permissions.html#grant_permissions_ui_account) et [Octroi d'un rôle CF à un utilisateur à l'aide de l'interface utilisateur IBM Cloud](/docs/services/cloud-monitoring/security/grant_permissions.html#grant_permissions_ui_space).

2. Disposer d'une session de terminal à partir de laquelle vous pouvez gérer le cluster Kubernetes et déployer des applications depuis la ligne de commande. Les exemples dans ce tutoriel sont valables pour un système Ubuntu Linux.

3. Installez les interfaces de ligne de commande pour gérer {{site.data.keyword.containershort}} dans votre système Ubuntu.

    * Installez l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}. Pour plus d'informations, voir [Installation de l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}](/docs/cli/reference/bluemix_cli/download_cli.html#download_install).
    
    * Installez l'interface de ligne de commande {{site.data.keyword.containershort}} pour créer et gérer vos clusters Kubernetes dans {{site.data.keyword.containershort}} et pour déployer des applications conteneurisées sur votre cluster. Pour plus d'informations, voir [Installation du plug-in CS](/docs/containers/cs_cli_install.html#cs_cli_install_steps).
    

    
 

## Etape 1 : Mise à disposition d'un cluster Kubernetes
{: #step1}

Procédez comme suit :

1. Créez un cluster Kubernetes standard.

   * [Créez un cluster standard Kubernetes à l'aide de l'interface utilisateur](/docs/containers/cs_cluster.html#cs_cluster_ui).
   * [Créez un cluster standard Kubernetes à l'aide de l'interface de ligne de commande](/docs/containers/cs_cluster.html#cs_cluster_cli).

2. Configurez le contexte de cluster dans un terminal. Une fois le contexte défini, vous pouvez gérer le cluster Kubernetes et déployer l'application dans ce cluster. 
    Connectez-vous à la région, l'organisation et l'espace dans l'environnement {{site.data.keyword.Bluemix_notm}} associé au cluster que vous avez créé. Pour plus d'informations, voir [Comment se connecter à {{site.data.keyword.Bluemix_notm}}](/docs/services/CloudLogAnalysis/qa/cli_qa.html#login).

	Initialisez le plug-in du service {{site.data.keyword.containershort}}.

	```
	bx cs init
	```
	{: codeblock}

    Définissez votre cluster comme contexte du terminal.
    
	```
	bx cs cluster-config MyCluster
	```
	{: codeblock}

    La sortie de l'exécution de cette commande fournit la commande que vous devez exécuter dans votre terminal pour définir le chemin d'accès à votre fichier de configuration. Par exemple :

	```
	export KUBECONFIG=/Users/ibm/.bluemix/plugins/container-service/clusters/MyCluster/kube-config-hou02-MyCluster.yml
	```
	{: codeblock}

    Copiez et collez la commande afin de définir la variable d'environnement dans votre terminal et appuyez sur **Entrée**.



## Etape 2 : Identification de l'emplacement où le cluster transmet les métriques dans le service {{site.data.keyword.monitoringshort}}
{: #step2}

Un cluster est une ressource de niveau compte. Lorsque vous mettez à disposition un cluster sur {{site.data.keyword.containershort}}, des clusters peuvent être créés au niveau compte ou avec un espace Cloud Foundry (CF) associé. Dès que le cluster est mis à disposition et prêt, les métriques sont collectées automatiquement et transmises au service {{site.data.keyword.monitoringshort}}.

* Les clusters auxquels un espace CF est associé transmettent les métriques au domaine de métriques d'espace. 
* Les clusters qui sont créés au niveau compte transmettent les métriques au domaine de métriques de compte.

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

Par exemple, la sortie d'un cluster qui transmet des métriques au domaine de compte se présente comme suit :

```
$ bx cs cluster-get MyDemoCluster --json
{
    "id": "f9adabcjhefg745746hgfjbnkdnfsks",
    "name": "MyDemoCluster",
    "region": "eu-gb",
    "dataCenter": "lon02",
    "location": "eu-gb-lon02",
    "serverURL": "https://xxx.xxx.xxx.x:xxxxx",
    "state": "normal",
    "createdDate": "2018-01-30T17:41:14+0000",
    "modifiedDate": "2018-01-30T17:41:14+0000",
    "workerCount": 2,
    "isPaid": true,
    "masterKubeVersion": "1.8.6_1505",
    "targetVersion": "1.8.6_1505",
    "ingressHostname": "mydemocluster.uk-south.containers.mybluemix.net",
    "ingressSecretName": "mydemocluster",
    "ownerEmail": "xxxx@uibm.com",
    "logOrg": "",
    "logOrgName": "",
    "logSpace": "",
    "logSpaceName": "",
    "monitoringURL": "https://metrics.eu-gb.bluemix.net/app/#/grafana4/dashboard/db/a-siuhfieuhf7346586hfrhf_ClusterMonitoringDashboard_v1?scopeId=a-siuhfieuhf7346586hfrhf\u0026?var-Account_ID=a_siuhfieuhf7346586hfrhf\u0026var-Cluster=MyDemoCluster\u0026var-Namespace=default\u0026var-Pod_ID=All",
    "addons": [
        {
            "name": "customer-storage-pod",
            "enabled": true
        },
        {
            "name": "basic-ingress-v2",
            "enabled": true
        },
        {
            "name": "storage-watcher-pod",
            "enabled": true
        }
    ],
    "vlans": null
}
```
{: screen}





## Etape 3 : Octroi des droits permettant à votre utilisateur d'afficher des métriques dans le domaine de métriques
{: #step3}

Pour autoriser un utilisateur à afficher des métriques dans un domaine d'espace, vous devez affecter à cet utilisateur un rôle CF qui décrit les actions qu'il peut exécuter avec le service {{site.data.keyword.monitoringshort}} dans l'espace.

Pour autoriser un utilisateur à visualiser des métriques dans un domaine de compte, vous devez affecter à cet utilisateur une règle IAM qui décrit les actions qu'il peut exécuter avec le service {{site.data.keyword.monitoringshort}} dans l'espace.

### Octroi des droits permettant à votre utilisateur d'afficher des métriques dans un domaine d'espace
{: #space}

Pour autoriser un utilisateur à gérer le service {{site.data.keyword.monitoringshort}}, procédez comme suit :

1. Connectez-vous à la console {{site.data.keyword.Bluemix_notm}}.

    Ouvrez un navigateur Web et démarrez le tableau de bord {{site.data.keyword.Bluemix_notm}} : [http://bluemix.net ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](http://bluemix.net){:new_window}
	
	Après que vous vous êtes connecté avec votre ID utilisateur et votre mot de passe, l'interface utilisateur {{site.data.keyword.Bluemix_notm}} s'ouvre.

2. Dans la barre de menus, cliquez sur **Gérer > Compte > Utilisateurs**.

    La fenêtre *Utilisateurs* affiche une liste d'utilisateurs avec leur adresse électronique pour le compte actuellement sélectionné.
	
3. Si l'utilisateur est membre du compte, sélectionnez le nom de l'utilisateur dans la liste ou cliquez sur **Gérer un utilisateur** dans le menu *Actions*.

    Si l'utilisateur n'est pas membre du compte, voir [Invitation d'utilisateurs](/docs/iam/iamuserinv.html#iamuserinv).

4. Sélectionnez **Accès Cloud Foundry**, puis choisissez **Affecter une organisation**.

5. Entrez les valeurs suivantes :

    <table>
      <caption></caption>
      <tr>
        <th>Zone</th>
        <th>Valeur</th>
      </tr>
      <tr>
        <td>Organisation</td>
        <td>MyOrg</td>
      </tr>
      <tr>
        <td>Rôle d'organisation</td>
        <td>Aucun rôle d'organisation</td>
      </tr>
      <tr>
        <td>Région</td>
        <td>Sud des Etats-Unis</td>
      </tr>
      <tr>
        <td>Espace</td>
        <td>dev</td>
      </tr>
      <tr>
        <td>Rôle d'espace</td>
        <td>Auditeur</td>
      </tr>
	
6. Cliquez sur **Sauvegarder un rôle**.

### Octroi des droits permettant à votre utilisateur d'afficher des métriques dans le domaine de compte
{: #acc}

Pour autoriser un utilisateur à gérer le service {{site.data.keyword.monitoringshort}}, procédez comme suit :

1. Connectez-vous à la console {{site.data.keyword.Bluemix_notm}}.

    Ouvrez un navigateur Web et démarrez le tableau de bord {{site.data.keyword.Bluemix_notm}} : [http://bluemix.net ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](http://bluemix.net){:new_window}
	
	Après que vous vous êtes connecté avec votre ID utilisateur et votre mot de passe, l'interface utilisateur {{site.data.keyword.Bluemix_notm}} s'ouvre.

2. Dans la barre de menus, cliquez sur **Gérer > Compte > Utilisateurs**.

    La fenêtre *Utilisateurs* affiche une liste d'utilisateurs avec leur adresse électronique pour le compte actuellement sélectionné.
	
3. Si l'utilisateur est membre du compte, sélectionnez le nom de l'utilisateur dans la liste ou cliquez sur **Gérer un utilisateur** dans le menu *Actions*.

    Si l'utilisateur n'est pas membre du compte, voir [Invitation d'utilisateurs](/docs/iam/iamuserinv.html#iamuserinv).

4. Sélectionnez **Règles d'accès > Affecter un accès > Affecter l'accès aux ressources**.

5. Choisissez le service **{{site.data.keyword.monitoringlong}}**, sélectionnez la région dans laquelle le cluster est disponible, **US-South** pour ce tutoriel, puis sélectionnez un rôle, **viewer**.



## Etape 4 : Octroi de droits au propriétaire de clé {{site.data.keyword.containershort_notm}}
{: #step4}

Lorsque le cluster transmet des métriques à un domaine d'espace, vous devez également octroyer des droits Cloud Foundry (CF) au propriétaire de clé {{site.data.keyword.containershort}} dans l'organisation et dans l'espace. Le propriétaire de clé doit posséder le rôle *orgManager* pour l'organisation, et les rôles *SpaceManager* et *Developer* pour l'espace.
Lorsque le cluster transmet des métriques au domaine de compte, le propriétaire de clé {{site.data.keyword.containershort}} doit posséder une règle IAM avec des droits de type administrator sur le service {{site.data.keyword.monitoringshort}}.

### Octroi des droits permettant d'afficher des métriques dans un domaine d'espace
{: #space_1}

Octroyez à l'ID utilisateur du propriétaire de clé {{site.data.keyword.containershort}} les droits suivants : le rôle *orgManager* pour l'organisation et les rôles *SpaceManager* et *Developer* pour l'espace. Procédez comme suit :
    
1. Connectez-vous à la console {{site.data.keyword.Bluemix_notm}}.

    Ouvrez un navigateur Web et démarrez le tableau de bord {{site.data.keyword.Bluemix_notm}} : [http://bluemix.net ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](http://bluemix.net){:new_window}
	
	Après que vous vous êtes connecté avec votre ID utilisateur et votre mot de passe, l'interface utilisateur {{site.data.keyword.Bluemix_notm}} s'ouvre.

2. Dans la barre de menus, cliquez sur **Gérer > Compte > Utilisateurs**.

    La fenêtre *Utilisateurs* affiche une liste d'utilisateurs avec leur adresse électronique pour le compte actuellement sélectionné.
	
3. Recherchez l'ID utilisateur du propriétaire de clé {{site.data.keyword.containershort}}.

    Exécutez la commande `bx cs api-key-info ClusterName` pour obtenir l'ID utilisateur du propriétaire de clé {{site.data.keyword.containershort}}.
4. Sélectionnez **Accès Cloud Foundry**, puis choisissez **Affecter une organisation**.

5. Entrez les valeurs suivantes : 

    <table>
      <caption></caption>
      <tr>
        <th>Zone</th>
        <th>Valeur</th>
      </tr>
      <tr>
        <td>Organisation</td>
        <td>MyOrg</td>
      </tr>
      <tr>
        <td>Rôle d'organisation</td>
        <td>Responsable</td>
      </tr>
      <tr>
        <td>Région</td>
        <td>Sud des Etats-Unis</td>
      </tr>
      <tr>
        <td>Espace</td>
        <td>dev</td>
      </tr>
      <tr>
        <td>Rôle d'espace</td>
        <td>Développeur</td>
      </tr>
	
6. Cliquez sur **Sauvegarder un rôle**.


### Octroi des droits permettant d'afficher des métriques dans le domaine de compte
{: #acc_1}

Procédez comme suit :

1. Connectez vous à la console {{site.data.keyword.Bluemix_notm}}. 

    Ouvrez un navigateur Web et lancez le tableau de bord {{site.data.keyword.Bluemix_notm}} : [http://bluemix.net ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](http://bluemix.net){:new_window}
	
	Après que vous vous êtes connecté avec votre ID utilisateur et votre mot de passe, l'interface utilisateur {{site.data.keyword.Bluemix_notm}} s'ouvre.

2. Dans la barre de menus, cliquez sur **Gérer > Compte > Utilisateurs**.  

    La fenêtre *Utilisateurs* affiche une liste d'utilisateurs avec leur adresse électronique pour le compte actuellement sélectionné.
	
3. Recherchez l'ID utilisateur du propriétaire de clé {{site.data.keyword.containershort}}. 

    Exécutez la commande `bx cs api-key-info ClusterName` pour obtenir l'ID utilisateur du propriétaire de clé {{site.data.keyword.containershort}}. 

4. Sélectionnez **Règles d'accès > Affecter un accès > Affecter l'accès aux ressources**.

5. Choisissez le service **{{site.data.keyword.monitoringlong}}**, sélectionnez la région dans laquelle le cluster est disponible, **US-South** pour ce tutoriel, puis sélectionnez un rôle, **administrator**.	

## Etape 5 : Déploiement d'un exemple d'application dans le cluster Kubernetes
{: #step5}

Déployez et exécutez un modèle d'application dans le cluster Kubernetes. Effectuez les étapes du tutoriel suivant pour déployer l'exemple d'application :[Leçon 1 : Déploiement d'applications d'instance uniques sur des clusters Kubernetes](/docs/containers/cs_tutorials_apps.html#cs_apps_tutorial_lesson1).

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
{: screen}

Dans cet exemple d'application, lorsque vous testez votre application dans un navigateur, l'application consigne dans stdout le message suivant : `L'exemple d'application est à l'écoute sur le port 8080.`


## Etape 6 : Lancement de Grafana et définition du domaine des métriques
{: #step6}

Lancez Grafana à partir d'un domaine et définissez le domaine {{site.data.keyword.monitoringshort}} dans lequel vous pourrez visualiser les métriques de cluster. 

Pour analyser les métriques pour un cluster, vous devez accéder à Grafana dans la région publique du cloud où le cluster a été créé. Pour plus d'informations, voir [Accès au tableau de bord Grafana depuis un navigateur Web](/docs/services/cloud-monitoring/grafana/navigating_grafana.html#launch_grafana_from_browser).

1. A partir d'un navigateur, démarrez Grafana.  

    Entrez l'adresse URL du service {{site.data.keyword.monitoringshort}} pour la région où vous avez créé le cluster. 
    
    Pour obtenir les adresses URL par région, voir [URL pour le service de surveillance](/docs/services/cloud-monitoring/monitoring_ov.html#region).

    Par exemple, pour la région Sud des Etats-Unis, démarrez : [https://metrics.ng.bluemix.net/](https://metrics.ng.bluemix.net/). 

2. Définissez le domaine {{site.data.keyword.monitoringshort} dans lequel vous pourrez visualiser les métriques de cluster. 

    Dans Grafana, sélectionnez votre ID. Ensuite, assurez-vous que vous êtes connecté au compte approprié, puis choisissez un domaine. 

    Les clusters auxquels un espace CF est associé transmettent les métriques au domaine de métriques d'espace. Sélectionnez `Domain = space`, ainsi que l'organisation et l'espace qui sont associés à votre cluster. 

    Les clusters qui sont créés au niveau compte transmettent les métriques au domaine de métriques de compte. Sélectionnez `Domain = account`

## Etape 7 : Surveillance du cluster dans Grafana
{: #step7}

{{site.data.keyword.containershort}} fournit un tableau de bord Grafana que vous pouvez utiliser pour surveiller vos métriques de cluster.  

Pour ouvrir l'exemple de tableau de bord, procédez comme suit :

1. Sélectionnez le bouton de basculement de la barre de menus latérale ![Barre de menus latérale de Grafana](images/grafana_settings.gif "Barre de menus latérale de Grafana").
2. Sélectionnez **Dashboards**.
3. Cliquez sur **Open**.
4. Sélectionnez **ClusterMonitoringDashboard_v1**.

L'exemple de tableau de bord s'ouvre.  

![Exemple de tableau de bord Grafana](images/cluster_grafana_sample_dashboard.png "Grafana sample dashboard")



## Etapes suivantes
{: #next_steps}

Définissez une alerte pour une métrique. Pour plus d'informations, voir [Configuration d'alertes](/docs/services/cloud-monitoring/config_alerts_ov.html#config_alerts_ov).
