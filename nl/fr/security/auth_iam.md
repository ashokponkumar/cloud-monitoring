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


# Authentification avec le modèle Bluemix IAM
{: #auth_iam}

Utilisez le modèle {{site.data.keyword.Bluemix}} IAM afin d'obtenir un jeton d'authentification que vous pouvez utiliser pour accéder aux métriques qui sont stockées dans le service {{site.data.keyword.monitoringshort}} ou afin d'obtenir une clé d'API. Le jeton possède un délai d'expiration. La clé d'API n'arrive jamais à expiration.
{:shortdesc}


## Obtention du jeton IAM via l'interface de ligne de commande Bluemix 
{: #iam_token_cli}

Pour obtenir le jeton d'autorisation via l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}, procédez comme suit :

1. Installez l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}.

   Pour plus d'informations, voir [Installation de l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}.](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa)
   
   Si l'interface de ligne de commande est installée, passez à l'étape suivante.
    
2. Connectez-vous à une région, une organisation et un espace {{site.data.keyword.Bluemix_notm}}. Exécutez la commande :

    Par exemple, pour la région du sud des États-Unis, exécutez la commande suivante :

	
    ```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Suivez les instructions. Entrez vos données d'identification {{site.data.keyword.Bluemix_notm}} et sélectionnez une organisation et un espace.
	
3. Exécutez la commande `bx iam oauth-tokens` pour obtenir le jeton IAM.

    ```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	La sortie renvoie le jeton IAM que vous devez utiliser pour authentifier votre ID utilisateur dans cet espace et dans cette organisation.

**Remarque :** lorsque vous utilisez le jeton, retirez *Bearer* de la valeur du jeton que vous avez transmis dans un appel d'API.
		
		
## Génération d'une clé d'API IAM via l'interface de ligne de commande Bluemix
{: #iam_apikey_cli}

Pour générer une clé d'API IAM à l'aide de l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}, procédez comme suit :

1. Installez l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}.

   Pour plus d'informations, voir [Installation de l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa).

   Si l'interface de ligne de commande est installée, passez à l'étape suivante.
	
2. Connectez-vous à {{site.data.keyword.Bluemix_notm}} avec la commande `bx login` dans l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}} :

    Par exemple, pour la région du sud des États-Unis, exécutez la commande suivante :

```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Suivez les instructions. Entrez vos données d'identification {{site.data.keyword.Bluemix_notm}} et sélectionnez une organisation et un espace.
 
3. Exécutez la commande `bx iam api-key-create` pour créer une clé d'API IAM.

    ```
    bx iam api-key-create NAME [-d DESCRIPTION][-f, --file FILE]
	```
	{: codeblock} 
	
	où
	
	* NAME est le nom de la clé d'API à créer.
	* DESCRIPTION est un texte décrivant la clé d'API.
	* FILE est le nom du fichier dans lequel la clé est sauvegardée.
	
    Par exemple, pour créer la clé d'API *MyKey*, exécutez la commande suivante :
	
	```
	bx iam api-key-create MyKey -d "This is my API key for service X" 
	```
	{: screen}
	
	
	
	
## Génération d'une clé d'API IAM via l'interface utilisateur Bluemix
{: #iam_apikey_ui}

Pour générer une clé d'API IAM à l'aide de l'interface utilisateur {{site.data.keyword.Bluemix_notm}}, procédez comme suit :

1. Connectez-vous à la console {{site.data.keyword.Bluemix_notm}}. 

2. Dans la barre de menus de la console, cliquez sur **Gérer > Sécurité > Clés d'API Bluemix**.

3. Cliquez sur **Créer une clé d'API**.

4. Entrez un nom et une description pour votre clé d'API. Ensuite, cliquez sur **Créer une clé d'API**.

5. Sauvegardez la clé d'API. Cliquez sur **Télécharger une clé d'API**.

    Cliquez sur **Afficher** pour afficher la clé d'API.  

**Remarque :** la clé d'API ne peut être affichée ou téléchargée qu'au moment de sa création. Si elle est perdue, vous devez en créer une autre.  


	
## Révocation d'une clé d'API à l'aide de l'interface utilisateur Bluemix
{: #revoke_ui}
	
Pour révoquer une clé d'API IAM via l'interface utilisateur {{site.data.keyword.Bluemix_notm}}, procédez comme suit :

1. Connectez-vous à la console {{site.data.keyword.Bluemix_notm}}.

2. Dans la barre de menus de la console, cliquez sur **Gérer > Sécurité > Clés d'API Bluemix**.

3. Cliquez sur **Actions**, puis sur **Supprimer une clé**.





	

	
	
	
	
	
	
