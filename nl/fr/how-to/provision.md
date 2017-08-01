---

copyright:
  years: 2017

lastupdated: "2017-07-10"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Mise à disposition du service Monitoring
{: #provision}

Vous pouvez mettre à disposition le service {{site.data.keyword.monitoringshort}} à partir de l'interface utilisateur {{site.data.keyword.Bluemix}} ou de la ligne de commande.
{:shortdesc}


## Mise à disposition à partir de l'interface utilisateur
{: #ui}

Pour mettre à disposition une instance du service {{site.data.keyword.monitoringshort}} dans {{site.data.keyword.Bluemix_notm}} :

1. Connectez-vous à votre compte {{site.data.keyword.Bluemix_notm}}.

    Le tableau de bord {{site.data.keyword.Bluemix_notm}} se trouve à l'adresse suivante : [http://bluemix.net ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://bluemix.net){:new_window}.
    
	Après que vous vous êtes connecté avec votre ID utilisateur et votre mot de passe, l'interface utilisateur {{site.data.keyword.Bluemix_notm}} s'ouvre. 

2. Cliquez sur **Catalogue**. La liste des services disponibles sur {{site.data.keyword.Bluemix_notm}} s'ouvre. 

3. Sélectionnez la catégorie **DevOps** pour filtrer la liste de services affichée. 

4. Cliquez sur la mosaïque **Monitoring**. 

5. Sélectionnez un plan de service. Pour collecter et accéder à des métriques pendant 45 jours au maximum, et pour définir des règles d'alerte, y compris des règles avec des caractères génériques, sélectionnez le plan **Premium**. Par défaut, le plan **Lite** est défini, ce qui vous autorise à utiliser la collection de métriques de plateforme dans l'espace où vous mettez à disposition le service, pendant une durée de conservation de 15 jours pour ces métriques.  

    Pour plus d'informations sur les plans de service, voir [Plans de service](/docs/services/cloud-monitoring/monitoring_ov.html#plans).
	
6. Cliquez sur **Créer** pour mettre à disposition le service {{site.data.keyword.monitoringshort}} dans l'espace {{site.data.keyword.Bluemix_notm}} auquel vous êtes connecté. 
  
 

## Mise à disposition à partir de l'interface de ligne de commande
{: #cli}

Procédez comme suit pour mettre à disposition une instance du service {{site.data.keyword.monitoringshort}} dans {{site.data.keyword.Bluemix_notm}} via la ligne de commande : 

1. [Prérequis] Installez l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}.

   Pour plus d'informations, voir [Installation de l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}.](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa)
   
   Si l'interface de ligne de commande est installée, passez à l'étape suivante.
    
2. Connectez-vous à une région, une organisation et un espace {{site.data.keyword.Bluemix_notm}}. Exécutez la commande :

    ```
    cf login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    Suivez les instructions. Entrez vos données d'identification {{site.data.keyword.Bluemix_notm}} et sélectionnez une organisation et un espace.
	
3. Exécutez la commande `cf create-service` pour mettre à disposition une instance. 

    ```
	cf create-service service_name service_plan service_instance_name
	```
	{: codeblock}
	
	Où
	
	* service_name est le nom du service, en l'occurrence, **Monitoring**.
	* service_plan est le nom du plan de service. Pour collecter et accéder à des métriques pendant 45 jours au maximum, et pour définir des règles d'alerte, y compris des règles avec des caractères génériques, sélectionnez le plan **Premium**. Par défaut, le plan **Lite** est défini, ce qui vous autorise à utiliser la collection de métriques de plateforme dans l'espace où vous mettez à disposition le service, pendant une durée de conservation de 15 jours pour ces métriques. Pour obtenir une liste de plans, voir  [Plans de service {{site.data.keyword.monitoringshort}}](/docs/services/cloud-monitoring/monitoring_ov.html#plan).
	* service_instance_name est le nom que vous souhaitez utiliser pour la nouvelle instance de service que vous créez.
	
	Pour plus d'informations sur la commande cf, voir [cf create-service](/docs/cli/reference/cfcommands/index.html#cf_create-service).

	Par exemple, pour créer une instance du service {{site.data.keyword.monitoringshort}} avec un plan Premium, exécutez la commande suivante :
	
	```
	cf create-service Monitoring premium my_monitoring_svc
	```
	{: codeblock}
	
4. Vérifiez que le service a bel et bien été créé. Exécutez la commande suivante :

    ```	
	cf services
	```
	{: codeblock}
	
	Le résultat de l'exécution de la commande se présente comme suit :
	
	```
    Getting services in org MyOrg / space MySpace as xxx@yyy.com...
    OK
    
    name                           service                  plan                   bound apps              last operation
    my_monitoring_svc              Monitoring               premium                                        create succeeded
	```
	{: screen}

	



