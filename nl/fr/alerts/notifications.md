---

copyright:
  years: 2017

lastupdated: "2017-07-18"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Utilisation de notification à l'aide de l'API Alerts
{: #notifications}

Utilisez l'API Alerts pour créer, supprimer et mettre à jour une notification, afficher les détails d'une notification, et répertorier les notifications qui sont définies dans votre espace {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

## Création d'un modèle de notification
{: #template}

Une notification est un fichier JSON. 

Vous pouvez créer autant de modèles de notification que vous le souhaitez, puis les réutiliser afin de créer des notifications du type en question dans votre organisation. 

Vous pouvez définir les types de notification suivants :

* Email : définissez une notification de type *Email* pour envoyer un courrier électronique à une adresse électronique valide. 
* Webhook : définissez une notification de type *Webhook* pour les noeuds finaux https seulement. Ajoutez un paramètre au noeud final pour réduire la probabilité que quelqu'un d'autre appelle votre noeud final.
* Pagerduty : définissez une notification de type *PagerDuty* afin d'envoyer les données d'alerte pour une métrique à votre système de gestion des incidents PagerDuty. 

Le tableau suivant présente des exemples de modèle de notification :

<table>
  <caption></caption>
  <tr>
    <th>Type</th>
	<th>Modèle</th>
	<th>Exemple</th>
  </tr>
  <tr>
    <td>Email</td>
	<td>
	```
	{
	"name": "Template_Name",
	"type": "Email",
	"description" : "Description",
	"detail": "EmailAddress"
	}
	```
	{: codeblock}
	</td>
	<td>
	```
	{
	"name": "my-email",
	"type": "Email",
	"description" : "Send email notification when there is an infrastructure problem.",
	"detail": "xxx@yyy.com"}
	```
	{: screen}
	</td>
  </tr>
  <tr>
    <td>Webhook</td>
	<td>
	```
	{
	"name": "Template_Name",
	"type": "Webhook",
	"description" : "Description",
	"detail": "Endpoint"
	}
	```
	{: codeblock}
	</td>
	<td>
	```
	{
	"name": "my-webhook",
	"type": "Webhook",
	"description" : "Fire a webhook when there is an infrastructure problem..",
	"detail": "https://myendpoint.bluemix.net?key=abcd1234"
	}
	```
	{: screen}
	</td>
  </tr>
  <tr>
    <td>Pagerduty</td>
	<td>
	```
	{
	"name": "Template_Name",
	"type": "PagerDuty",
	"description" : "Description",
	"detail": "Pagerduty_APIkey"
	}
	```
	{: codeblock}
	</td>
	<td>
	```
	{
	"name": "my-pagerduty",
	"type": "PagerDuty",
	"description" : "Fire a PagerDuty alert when there is an infrastructure problem..",
	"detail": "abcd1234"
	}
	```
	{: screen}
	</td>
  </tr>
</table>

Où

* *Template_Name* correspond au nom du modèle de notification.
* *Description* indique à quel moment ce type de notification est utilisé.
* *EmailAddress* définit l'adresse électronique du destinataire de la notification.
* *Endpoint* définit l'adresse URL à laquelle la demande POST doit être émise. Il s'agit d'une adresse URL admettant les demandes POST. 
* *Pagerduty_APIkey* définit une clé d'API unique. Celle-ci est générée par un administrateur ou un propriétaire de compte PagerDuty.


Procédez comme suit pour créer un modèle de notification :

1. Créez un répertoire pour stocker les ressources du service {{site.data.keyword.monitoringshort}}, par exemple *cloud-monitoring*.

    Par exemple, sur un système Ubuntu, exécutez la commande suivante :
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
2. Créez un répertoire pour stocker vos modèles de notification, par exemple *notification-templates*.

    Par exemple, sur un système Ubuntu, exécutez la commande suivante :
	
	```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	Placez-vous dans ce répertoire :
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
3. Créez un modèle de notification dans un répertoire local sur votre système.

    Ainsi, créez le fichier **email-template.json** pour un modèle de notification par courrier électronique, en utilisant par exemple l'éditeur vi : 
	
	```
	{
    "name": "email_template",
    "type": "Email",
    "description" : "Send email to manager of department X when ....",
    "detail": "xxx@yyy"
    }
	```
	{: codeblock}
	


## Création d'une notification
{: #create}

Pour créer une notification, procédez comme suit :

1. Créez un fichier de notification.

    1. Utilisez un modèle de notification pour créer le fichier. Pour plus d'informations, voir [Création d'un modèle de notification](#template).
	
	2. Mettez à jour le fichier :
	
	    * Entrez un nom unique dans la zone *name*. La valeur de cette zone sera utilisée ultérieurement pour mettre à jour ou supprimer la notification, ou afficher ses détails.
		* Entrez une description.
		* Entrez une adresse électronique, un noeud final d'URL ou une clé PagerDuty valide.
		
	3. Sauvegardez le fichier. Par exemple, utilisez un préfixe tel que *s-* pour les notifications que vous définissez pour les requêtes s'exécutant dans un espace {{site.data.keyword.Bluemix_notm}}.
	
	    **Astuce :** créez votre fichier de notification dans le répertoire *~/cloud-monitoring/notifications/* afin de regrouper les ressources du service {{site.data.keyword.monitoringshort_notm}}. 
	
2. Connectez-vous à une région, une organisation et un espace {{site.data.keyword.Bluemix_notm}}. Exécutez la commande :

    Par exemple, pour vous connecter à la région du sud des États-Unis, exécutez la commande suivante :
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Suivez les instructions. Entrez vos données d'identification {{site.data.keyword.Bluemix_notm}} et sélectionnez une organisation et un espace.

3. Obtenez le jeton d'authentification ou la clé d'API.

    * Pour l'authentification IAM, voir [Obtention du jeton IAM à l'aide de l'interface de ligne de commande Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) ou [Génération d'une clé d'API IAM à l'aide de l'interface de ligne de commande Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Pour l'authentification UAA, voir [Obtention du jeton UAA via l'interface de ligne de commande Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) ou [Obtention du jeton UAA via l'API REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

	Par exemple, pour utiliser le jeton IAM, exécutez la commande suivante :

    ```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	Le résultat de cette commande est le suivant :
	
	```
	IAM token:  Bearer djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ
    UAA token:  Bearer eyJhbGciOiJIUz..Ky6vagp3k_QcIcKJ-td83qXhO5Uze43KcplG6PzcGs8
	```
	{: screen}
	
	Exportez ensuite la variable *Token* :
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Remarque :** le jeton exclut *Bearer*.
		
4. Obtenez l'identificateur global unique de l'espace. Un identificateur global unique identifiant un espace doit avoir le préfixe *s-*.

    Exécutez la commande suivante :
	
	```
	bx iam space SpaceName --guid
	```
	{: codeblock}
	
	Où *SpaceName* est le nom de l'espace dans lequel vous allez définir une notification.
	
	Par exemple, pour obtenir l'identificateur global unique d'un espace dont le nom est *dev*, exécutez la commande suivante :
	
	```
	bx iam space dev --guid
	```
	{: screen}
	
	Le résultat de cette commande est le suivant :
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	Exportez ensuite la variable *Space* :
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. Exécutez la commande cURL suivante pour créer une notification :

    ```
	curl -XPOST -d @Notification_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: codeblock}
	
	où
	
	* Notification_File est le fichier JSON qui définit votre notification.
	
	* *X-Auth-User-Token* est un paramètre qui transmet le jeton {{site.data.keyword.Bluemix_notm}} UAA, le jeton IAM ou la clé d'API.
	
	* *Auth_Type* est le préfixe qui définit le type de jeton ou de clé d'API. La liste ci-dessous répertorie les valeurs valides : *apikey*, *iam* et *uaa*, où

        * *apikey* indique que le jeton est une clé d'API.
		* *iam* indique que le jeton spécifié est un jeton généré par IAM.
		* *uaa* indique que le jeton spécifié est un jeton généré par UAA.
	
	* *X-Auth-Scope-Id* est un paramètre qui transmet l'identificateur global unique de l'espace. Un identificateur global unique identifiant un espace doit avoir le préfixe *s-*. Cet en-tête est requis si vous utilisez un jeton d'authentification UAA. Si vous utilisez un jeton d'authentification IAM, cet en-tête est facultatif et les informations de domaine qui sont associées au jeton sont utilisées.
	
	* Token est le jeton UAA, le jeton IAM ou la clé d'API.
	
	* Space est l'identificateur global unique de l'espace. Il n'est requis que si vous utilisez un jeton UAA.
	
    Par exemple 	
	
	```
	curl -XPOST -d @s-email-dep-A.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.{DomainName}/v1/alert/notification
	```
	{: screen}

## Suppression d'une notification
{:#delete}

Pour supprimer une notification, procédez comme suit :

1. Connectez-vous à une région, une organisation et un espace {{site.data.keyword.Bluemix_notm}}. Exécutez la commande :

    Par exemple, pour vous connecter à la région du sud des États-Unis, exécutez la commande suivante :

```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Suivez les instructions. Entrez vos données d'identification {{site.data.keyword.Bluemix_notm}} et sélectionnez une organisation et un espace.

2. Obtenez le jeton d'authentification ou la clé d'API.

    * Pour l'authentification IAM, voir [Obtention du jeton IAM via l'interface de ligne de commande Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) ou [Génération d'une clé d'API IAM via l'interface de ligne de commande Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Pour l'authentification UAA, voir [Obtention du jeton UAA via l'interface de ligne de commande Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) ou [Obtention du jeton UAA via l'API REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).
	Par exemple, pour utiliser le jeton IAM, exécutez la commande suivante :

    ```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	Le résultat de cette commande est le suivant :
	
	```
	IAM token:  Bearer djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ
    UAA token:  Bearer eyJhbGciOiJIUz..Ky6vagp3k_QcIcKJ-td83qXhO5Uze43KcplG6PzcGs8
	```
	{: screen}
	
	Exportez ensuite la variable *Token* :
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Remarque :** le jeton exclut *Bearer*.
	
3. Obtenez l'identificateur global unique de l'espace. Un identificateur global unique identifiant un espace doit avoir le préfixe *s-*.

    Exécutez la commande suivante :
	
	```
	bx iam space SpaceName --guid
	```
	{: codeblock}
	
	Où *SpaceName* est le nom de l'espace dans lequel vous allez définir une notification.
	
	Par exemple, pour obtenir l'identificateur global unique d'un espace dont le nom est *dev*, exécutez la commande suivante :
	
	```
	bx iam space dev --guid
	```
	{: screen}
	
	Le résultat de cette commande est le suivant :
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	Exportez ensuite la variable *Space* :
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. Exécutez la commande cURL suivante pour supprimer une notification :

    ```
	curl -XDELETE --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification/Notification_Name
	```
	{: codeblock}
	
	où
	
	* *Auth_Type* est le préfixe qui définit le type de jeton ou de clé d'API. La liste ci-dessous répertorie les valeurs valides : *apikey*, *iam* et *uaa*, où

        * *apikey* indique que le jeton est une clé d'API.
		* *iam* indique que le jeton spécifié est un jeton généré par IAM.
		* *uaa* indique que le jeton spécifié est un jeton généré par UAA.
	
	* *X-Auth-User-Token* est un paramètre qui transmet le jeton {{site.data.keyword.Bluemix_notm}} UAA, le jeton IAM ou la clé d'API. Le jeton ou la clé d'API doit avoir pour préfixe l'une des valeurs suivantes : *apikey*, *iam* ou *uaa*, où

        * *apikey* indique que le jeton est une clé d'API.
		* *iam* indique que le jeton spécifié est un jeton généré par IAM.
		* *uaa* indique que le jeton spécifié est un jeton généré par UAA.
		
	* Token est le jeton UAA, le jeton IAM ou la clé d'API.
	
	* Space est l'identificateur global unique de l'espace. Il n'est requis que si vous utilisez un jeton UAA.
	
	* Notification_Name est le nom de la notification, c'est-à-dire la valeur de la zone *name* lorsque vous répertoriez les propriétés d'une notification.
	


## Affichage de la liste de toutes les notifications
{: #list}

Pour répertorier toutes les notifications, procédez comme suit :

1. Connectez-vous à une région, une organisation et un espace {{site.data.keyword.Bluemix_notm}}. Exécutez la commande :

    Par exemple, pour vous connecter à la région du sud des États-Unis, exécutez la commande suivante :

```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Suivez les instructions. Entrez vos données d'identification {{site.data.keyword.Bluemix_notm}} et sélectionnez une organisation et un espace.

2. Obtenez le jeton d'authentification ou la clé d'API.

    * Pour l'authentification IAM, voir [Obtention du jeton IAM via l'interface de ligne de commande Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) ou [Génération d'une clé d'API IAM via l'interface de ligne de commande Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Pour l'authentification UAA, voir [Obtention du jeton UAA via l'interface de ligne de commande Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) ou [Obtention du jeton UAA via l'API REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).
	Par exemple, pour utiliser le jeton IAM, exécutez la commande suivante :

    ```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	Le résultat de cette commande est le suivant :
	
	```
	IAM token:  Bearer djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ
    UAA token:  Bearer eyJhbGciOiJIUz..Ky6vagp3k_QcIcKJ-td83qXhO5Uze43KcplG6PzcGs8
	```
	{: screen}
	
	Exportez ensuite la variable *Token* :
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Remarque :** le jeton exclut *Bearer*.
	
3. Obtenez l'identificateur global unique de l'espace. Un identificateur global unique identifiant un espace doit avoir le préfixe *s-*.

    Exécutez la commande suivante :
	
	```
	bx iam space SpaceName --guid
	```
	{: codeblock}
	
	Où *SpaceName* est le nom de l'espace dans lequel vous allez définir une notification.
	
	Par exemple, pour obtenir l'identificateur global unique d'un espace dont le nom est *dev*, exécutez la commande suivante :
	
	```
	bx iam space dev --guid
	```
	{: screen}
	
	Le résultat de cette commande est le suivant :
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	Exportez ensuite la variable *Space* :
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. Exécutez la commande cURL suivante pour répertorier toutes les notifications :

    ```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notifications
	```
	{: codeblock}
	
	où
	
	* *Auth_Type* est le préfixe qui définit le type de jeton ou de clé d'API. La liste ci-dessous répertorie les valeurs valides : *apikey*, *iam* et *uaa*, où

        * *apikey* indique que le jeton est une clé d'API.
		* *iam* indique que le jeton spécifié est un jeton généré par IAM.
		* *uaa* indique que le jeton spécifié est un jeton généré par UAA.
		
	* *X-Auth-User-Token* est un paramètre qui transmet le jeton {{site.data.keyword.Bluemix_notm}} UAA, le jeton IAM ou la clé d'API. Le jeton ou la clé d'API doit avoir pour préfixe l'une des valeurs suivantes : *apikey*, *iam* ou *uaa*, où

        * *apikey* indique que le jeton est une clé d'API.
		* *iam* indique que le jeton spécifié est un jeton généré par IAM.
		* *uaa* indique que le jeton spécifié est un jeton généré par UAA.
		
	* Token est le jeton UAA, le jeton IAM ou la clé d'API.
	
	* Space est l'identificateur global unique de l'espace. Il n'est requis que si vous utilisez un jeton UAA.


			

## Affichage des détails d'une notification
{: #show}


Pour afficher les informations sur une notification, procédez comme suit :

1. Connectez-vous à une région, une organisation et un espace {{site.data.keyword.Bluemix_notm}}. Exécutez la commande :

    Par exemple, pour vous connecter à la région du sud des États-Unis, exécutez la commande suivante :

```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Suivez les instructions. Entrez vos données d'identification {{site.data.keyword.Bluemix_notm}} et sélectionnez une organisation et un espace.

2. Obtenez le jeton d'authentification ou la clé d'API.

    * Pour l'authentification IAM, voir [Obtention du jeton IAM via l'interface de ligne de commande Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) ou [Génération d'une clé d'API IAM via l'interface de ligne de commande Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Pour l'authentification UAA, voir [Obtention du jeton UAA via l'interface de ligne de commande Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) ou [Obtention du jeton UAA via l'API REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).
	Par exemple, pour utiliser le jeton IAM, exécutez la commande suivante :

    ```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	Le résultat de cette commande est le suivant :
	
	```
	IAM token:  Bearer djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ
    UAA token:  Bearer eyJhbGciOiJIUz..Ky6vagp3k_QcIcKJ-td83qXhO5Uze43KcplG6PzcGs8
	```
	{: screen}
	
	Exportez ensuite la variable *Token* :
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Remarque :** le jeton exclut *Bearer*.
	
3. Obtenez l'identificateur global unique de l'espace. Un identificateur global unique identifiant un espace doit avoir le préfixe *s-*.

    Exécutez la commande suivante :
	
	```
	bx iam space SpaceName --guid
	```
	{: codeblock}
	
	Où *SpaceName* est le nom de l'espace dans lequel vous allez définir une notification.
	
	Par exemple, pour obtenir l'identificateur global unique d'un espace dont le nom est *dev*, exécutez la commande suivante :
	
	```
	bx iam space dev --guid
	```
	{: screen}
	
	Le résultat de cette commande est le suivant :
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	Exportez ensuite la variable *Space* :
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. Exécutez la commande cURL suivante pour afficher les détails d'une notification :

    ```
	curl -XGET --header "X-Auth-User-Token:uaa ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification/Notification_Name
	```
	{: codeblock}
	
	où
	
	* *X-Auth-User-Token* est un paramètre qui transmet le jeton {{site.data.keyword.Bluemix_notm}} UAA, le jeton IAM ou la clé d'API. Le jeton ou la clé d'API doit avoir pour préfixe l'une des valeurs suivantes : *apikey*, *iam* ou *uaa*, où

        * *apikey* indique que le jeton est une clé d'API.
		* *iam* indique que le jeton spécifié est un jeton généré par IAM.
		* *uaa* indique que le jeton spécifié est un jeton généré par UAA.
		
	* Token est le jeton UAA, le jeton IAM ou la clé d'API.
	
	* Space est l'identificateur global unique de l'espace. Il n'est requis que si vous utilisez un jeton UAA.
	
	* Notification_Name est le nom de la notification, c'est-à-dire la valeur de la zone *name* lorsque vous répertoriez les propriétés d'une notification.
	
     
		

			
## Test d'une notification
{: #test}	
			
Pour tester une notification, procédez comme suit :

1. Connectez-vous à une région, une organisation et un espace {{site.data.keyword.Bluemix_notm}}. Exécutez la commande :

    Par exemple, pour vous connecter à la région du sud des États-Unis, exécutez la commande suivante :

```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Suivez les instructions. Entrez vos données d'identification {{site.data.keyword.Bluemix_notm}} et sélectionnez une organisation et un espace.

2. Obtenez le jeton d'authentification ou la clé d'API.

    * Pour l'authentification IAM, voir [Obtention du jeton IAM via l'interface de ligne de commande Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) ou [Génération d'une clé d'API IAM via l'interface de ligne de commande Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Pour l'authentification UAA, voir [Obtention du jeton UAA via l'interface de ligne de commande Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) ou [Obtention du jeton UAA via l'API REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).
	Par exemple, pour utiliser le jeton IAM, exécutez la commande suivante :

    ```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	Le résultat de cette commande est le suivant :
	
	```
	IAM token:  Bearer djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ
    UAA token:  Bearer eyJhbGciOiJIUz..Ky6vagp3k_QcIcKJ-td83qXhO5Uze43KcplG6PzcGs8
	```
	{: screen}
	
	Exportez ensuite la variable *Token* :
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Remarque :** le jeton exclut *Bearer*.

3. Obtenez l'identificateur global unique de l'espace. Un identificateur global unique identifiant un espace doit avoir le préfixe *s-*.

    Exécutez la commande suivante :
	
	```
	bx iam space SpaceName --guid
	```
	{: codeblock}
	
	Où *SpaceName* est le nom de l'espace dans lequel vous allez définir une notification.
	
	Par exemple, pour obtenir l'identificateur global unique d'un espace dont le nom est *dev*, exécutez la commande suivante :
	
	```
	bx iam space dev --guid
	```
	{: screen}
	
	Le résultat de cette commande est le suivant :
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	Exportez ensuite la variable *Space* :
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. Exécutez la commande cURL suivante pour tester une notification :

    ```
	curl -XPOST --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification/test/Notification_Name
	```
	{: codeblock}
	
	où
	
	* *Auth_Type* est le préfixe qui définit le type de jeton ou de clé d'API. La liste ci-dessous répertorie les valeurs valides : *apikey*, *iam* et *uaa*, où

        * *apikey* indique que le jeton est une clé d'API.
		* *iam* indique que le jeton spécifié est un jeton généré par IAM.
		* *uaa* indique que le jeton spécifié est un jeton généré par UAA.
	
	* *X-Auth-User-Token* est un paramètre qui transmet le jeton {{site.data.keyword.Bluemix_notm}} UAA, le jeton IAM ou la clé d'API. Le jeton ou la clé d'API doit avoir pour préfixe l'une des valeurs suivantes : *apikey*, *iam* ou *uaa*, où

        * *apikey* indique que le jeton est une clé d'API.
		* *iam* indique que le jeton spécifié est un jeton généré par IAM.
		* *uaa* indique que le jeton spécifié est un jeton généré par UAA.
		
	* Token est le jeton UAA, le jeton IAM ou la clé d'API.
	
	* Space est l'identificateur global unique de l'espace. Il n'est requis que si vous utilisez un jeton UAA.
	
	* Notification_Name est le nom de la notification, c'est-à-dire la valeur de la zone *name* lorsque vous répertoriez les propriétés d'une notification.
	


## Mise à jour d'une notification
{: #update}

Pour mettre à jour une notification, procédez comme suit :

1. Connectez-vous à une région, une organisation et un espace {{site.data.keyword.Bluemix_notm}}. Exécutez la commande :

    Par exemple, pour vous connecter à la région du sud des États-Unis, exécutez la commande suivante :

```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Suivez les instructions. Entrez vos données d'identification {{site.data.keyword.Bluemix_notm}} et sélectionnez une organisation et un espace.

2. Obtenez le jeton d'authentification ou la clé d'API.

    * Pour l'authentification IAM, voir [Obtention du jeton IAM via l'interface de ligne de commande Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) ou [Génération d'une clé d'API IAM via l'interface de ligne de commande Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Pour l'authentification UAA, voir [Obtention du jeton UAA via l'interface de ligne de commande Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) ou [Obtention du jeton UAA via l'API REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).
	Par exemple, pour utiliser le jeton IAM, exécutez la commande suivante :

    ```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	Le résultat de cette commande est le suivant :
	
	```
	IAM token:  Bearer djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ
    UAA token:  Bearer eyJhbGciOiJIUz..Ky6vagp3k_QcIcKJ-td83qXhO5Uze43KcplG6PzcGs8
	```
	{: screen}
	
	Exportez ensuite la variable *Token* :
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Remarque :** le jeton exclut *Bearer*.
	
3. Obtenez l'identificateur global unique de l'espace. Un identificateur global unique identifiant un espace doit avoir le préfixe *s-*.

    Exécutez la commande suivante :
	
	```
	bx iam space SpaceName --guid
	```
	{: codeblock}
	
	Où *SpaceName* est le nom de l'espace dans lequel vous allez définir une notification.
	
	Par exemple, pour obtenir l'identificateur global unique d'un espace dont le nom est *dev*, exécutez la commande suivante :
	
	```
	bx iam space dev --guid
	```
	{: screen}
	
	Le résultat de cette commande est le suivant :
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	Exportez ensuite la variable *Space* :
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. Exécutez la commande cURL suivante pour mettre à jour une notification :

    ```
	curl -XPUT -d @Notification_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: codeblock}
	
	où
	
	* Notification_File est le fichier JSON qui définit votre notification.
	
	* *Auth_Type* est le préfixe qui définit le type de jeton ou de clé d'API. La liste ci-dessous répertorie les valeurs valides : *apikey*, *iam* et *uaa*, où

        * *apikey* indique que le jeton est une clé d'API.
		* *iam* indique que le jeton spécifié est un jeton généré par IAM.
		* *uaa* indique que le jeton spécifié est un jeton généré par UAA.
	
	* *X-Auth-User-Token* est un paramètre qui transmet le jeton Bluemix UAA, le jeton IAM ou la clé d'API. Le jeton ou la clé d'API doit avoir pour préfixe l'une des valeurs suivantes : *apikey*, *iam* ou *uaa*, où

        * *apikey* indique que le jeton est une clé d'API.
		* *iam* indique que le jeton spécifié est un jeton généré par IAM.
		* *uaa* indique que le jeton spécifié est un jeton généré par UAA.
		
	* Token est le jeton UAA, le jeton IAM ou la clé d'API.
	
	* Space est l'identificateur global unique de l'espace. Il n'est requis que si vous utilisez un jeton UAA.

	
        

