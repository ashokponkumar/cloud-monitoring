---

copyright:
  years: 2017

lastupdated: "2017-07-12"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Authentification avec le modèle Bluemix UAA
{: #auth_uaa}

Utilisez le modèle {{site.data.keyword.Bluemix}} afin d'obtenir un jeton d'authentification que vous pouvez utiliser pour accéder aux métriques qui sont stockées dans le service {{site.data.keyword.monitoringshort}} pour un espace dans {{site.data.keyword.Bluemix_notm}}. Vous pouvez vous procurer le jeton d'authentification via l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}} ou l'API REST `Login`.
{:shortdesc}

Pour accéder aux métriques à l'aide d'un jeton d'authentification UAA, vous avez besoin des informations suivantes :

* Un jeton UAA permettant d'accéder au service {{site.data.keyword.monitoringshort}} à l'aide des API RESTful.
* L'identificateur global unique de l'espace.

		
## Obtention du jeton UAA via l'interface de ligne de commande Bluemix
{: #uaa_cli}


Pour obtenir le jeton d'autorisation, procédez comme suit :

1. Installez l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}.

   Pour plus d'informations, voir [Installation de l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}.](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa)
   
   Si l'interface de ligne de commande est installée, passez à l'étape suivante.
    
2. Connectez-vous à une région, une organisation et un espace {{site.data.keyword.Bluemix_notm}}. Exécutez la commande :

    ```
    cf login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    Suivez les instructions. Entrez vos données d'identification {{site.data.keyword.Bluemix_notm}} et sélectionnez une organisation et un espace.
	
3. Exécutez la commande `cf oauth-token` pour obtenir le jeton {{site.data.keyword.Bluemix_notm}} UAA.

    ```
	cf oauth-token
	```
	{: codeblock}
	
	La sortie renvoie le jeton UAA que vous devez utiliser pour authentifier votre ID utilisateur dans cet espace et dans cette organisation.

4. Obtenez l'identificateur global unique pour l'espace de l'organisation pour laquelle vous avez obtenu un jeton d'authentification.

   Pour plus d'informations, voir [Comment obtenir l'identificateur global unique d'un espace](/docs/services/cloud-monitoring/qa/cli_qa.html#space_guid).
	
5. Exportez les variables suivantes : TOKEN et SPACE.

    * *TOKEN* est le jeton oauth que vous avez obtenu à l'étape précédente sans le préfixe Bearer.
	
	* *SPACE* est l'identificateur global unique de l'espace que vous avez obtenu à l'étape précédente.
		
	Par exemple,
	
	```
	export TOKEN="eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8"
	export SPACE="667fb895-abcd-defg-aaaa-cf4587341095"
	```
	{: screen}
	
6. Exécutez la commande suivante pour obtenir le jeton UAA permettant d'utiliser le service {{site.data.keyword.monitoringshort}} :
 
    ```
	curl -k -X GET  --header "X-Auth-Token: ${TOKEN}"  --header "X-Auth-Project-Id: ${SPACE}"  METRICS_ENDPOINT
    ```
    {: codeblock}	
	
	Où
	* SPACE est l'identificateur global unique de l'espace dans lequel le service s'exécute.
	* TOKEN est le jeton {{site.data.keyword.Bluemix_notm}} UAA que vous avez obtenu au cours d'une étape précédente sans le préfixe bearer.
	* METRICS_ENDPOINT est le noeud final des métriques (https://metrics.ng.bluemix.net/token) pour la région {{site.data.keyword.Bluemix_notm}} dans laquelle l'organisation et l'espace sont disponibles.
	
## Obtention du jeton UAA via l'API
{: #uaa_api}

Pour obtenir le jeton d'autorisation, exécutez la commande cURL suivante :

```
curl -XPOST -d 'user=USERNAME&passwd=PASSWORD&space=SPACE_NAME&organization=ORG_NAME' METRICS_ENDPOINT
```
{: codeblock}

où

* USERNAME est un ID {{site.data.keyword.Bluemix_notm}} pour lequel vous voulez obtenir le jeton d'authentification permettant d'utiliser le service {{site.data.keyword.monitoringshort}}.
* PASSWORD est le mot de passe de l'ID utilisateur pour la connexion à {{site.data.keyword.Bluemix_notm}}.
* SPACE_NAME est le nom de l'espace dans lequel les métriques sont collectées.
* ORG_NAME est le nom de l'organisation dans {{site.data.keyword.Bluemix_notm}} qui héberge l'espace.
* METRICS_ENDPOINT est le noeud final des métriques (https://metrics.ng.bluemix.net/login) pour la région {{site.data.keyword.Bluemix_notm}} dans laquelle l'organisation et l'espace sont disponibles.

La sortie est un document JSON qui contient le jeton UAA. La valeur du jeton est indiquée dans la zone **access-token**.

Exemple de document JSON :

```
{
    "access_token": "eyJhbGc...",
    "logging_token": "xxxxxxxxxxxx",
    "org_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "space_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```
{: screen}

La valeur de *logging_token* correspond au jeton UAA dont vous avez besoin pour pouvoir utiliser le service {{site.data.keyword.monitoringshort}}.
 
**Remarque :**

* Si vous n'utilisez pas cURL, vous devez définir l'en-tête **Content-Type: application/x-www-form-urlencoded**.
* Si vous obtenez le code d'erreur *BXNMS0122E: User credentials are invalid*, vérifiez que vous utilisez un ID {{site.data.keyword.IBM_notm}} valide.


