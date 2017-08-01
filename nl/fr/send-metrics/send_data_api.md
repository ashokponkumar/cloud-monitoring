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


# Envoi de données au service Monitoring
{: #send_data_api}

Vous pouvez envoyer des métriques au service {{site.data.keyword.monitoringshort}} à l'aide de l'API Metrics. Vous pouvez envoyer des métriques à un espace {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}


Pour les conteneurs Docker {{site.data.keyword.Bluemix_notm}}, les métriques système de base sont collectées automatiquement. Pour les applications Cloud Foundry et les applications s'exécutant sur une machine virtuelle, les métriques doivent être envoyées directement depuis l'application à l'aide de l'[API Metrics](https://console.bluemix.net/apidocs/927-ibm-cloud-monitoring-rest-api?&language=node#introduction){: new_window}. 



## Envoi de métriques à un espace à l'aide d'UAA
{: #uaa}

Pour envoyer des métriques à un espace {{site.data.keyword.Bluemix_notm}}, procédez comme suit :

1. Connectez-vous à une région, une organisation et un espace {{site.data.keyword.Bluemix_notm}}. Exécutez la commande :

    Par exemple, pour vous connecter à la région du sud des États-Unis, exécutez la commande suivante :

	
	```
    cf login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Suivez les instructions. Entrez vos données d'identification {{site.data.keyword.Bluemix_notm}} et sélectionnez une organisation et un espace.

2. Obtenez le jeton d'authentification UAA.

    Pour plus d'informations sur l'authentification UAA, voir [Obtention du jeton UAA via l'interface de ligne de commande Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) ou [Obtention du jeton UAA via l'API REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

	Par exemple, pour utiliser le jeton UAA, exécutez la commande suivante :

    ```
	cf oauth-token
	```
	{: codeblock}
	
	Le résultat de cette commande est le suivant :
	
	```
	bearer eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8
	```
	{: screen}
	
	Ensuite, exportez la variable *Token*. Exemple :
	
	```
	export Token="eyJhbGciOiJI....cGlxryWS8"
	```
	{: screen}
	
	**Remarque :** le jeton exclut *Bearer*.
		
3. Obtenez l'identificateur global unique de l'espace. Un identificateur global unique identifiant un espace doit avoir le préfixe *s-*.

    Exécutez la commande suivante :
	
	```
	cf space SpaceName --guid
	```
	{: codeblock}
	
	Où *SpaceName* est le nom de l'espace dans lequel vous allez définir une notification.
	
	Par exemple, pour obtenir l'identificateur global unique d'un espace dont le nom est *dev*, exécutez la commande suivante :
	
	```
	cf space dev --guid
	```
	{: screen}
	
	Le résultat de cette commande est le suivant :
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	Ensuite, exportez la variable *Space*. Exemple :
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. Exécutez la commande cURL suivante pour envoyer des métriques :

    ```
	curl -XPOST -d @Metric_File --header "X-Auth-User-Token:uaa ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics
	```
	{: codeblock}
	
	où
	
	* Metrics_File représente le fichier JSON qui comporte la définition des métriques envoyées au service {{site.data.keyword.monitoringshort}}.
	
	* *X-Auth-User-Token* est un paramètre qui transmet le jeton UAA {{site.data.keyword.Bluemix_notm}}.
	
	* *Auth_Type* est le préfixe qui définir le type de jeton. *uaa* indique que le jeton spécifié est un jeton généré par UAA.
	
	* *X-Auth-Scope-Id* est un paramètre qui transmet l'identificateur global unique de l'espace. Un identificateur global unique identifiant un espace doit avoir le préfixe *s-*.
	
	* Token représente le jeton UAA.
	
	* Space représente l'identificateur global unique de l'espace. 
	
	Par exemple, vous pouvez exécuter la commande suivante pour envoyer deux métriques, `myhost.cpu.idle` et `myapp.login.attempts`, au service {{site.data.keyword.monitoringshort}} :
	
	```
	curl -XPOST @metric.json --header "X-Auth-User-Token:uaa ${Token}" https://metrics.ng.bluemix.net/v1/metrics
	```
	{: screen}
	
	Exemple de fichier *metric.json* :

    ```
    [
      {
        "name" : "myhost.cpu.idle",
        "value" : 22.37
      },
      {
        "name": "myapp.login.retries",
        "value": 4
      }
    ]
	```
	{: screen}

 
## Envoi de métriques à un espace à l'aide d'IAM ou d'une clé d'API
{: #iam}

Pour envoyer des métriques à un espace {{site.data.keyword.Bluemix_notm}}, procédez comme suit :

1. Connectez-vous à une région, une organisation et un espace {{site.data.keyword.Bluemix_notm}}. Exécutez la commande :

    Par exemple, pour vous connecter à la région du sud des États-Unis, exécutez la commande suivante :
```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Suivez les instructions. Entrez vos données d'identification {{site.data.keyword.Bluemix_notm}} et sélectionnez une organisation et un espace.

2. Obtenez le jeton d'authentification ou la clé d'API.

    Pour plus d'informations sur l'authentification IAM, voir [Obtention du jeton IAM via l'interface de ligne de commande Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_cli) ou [Génération d'une clé d'API IAM via l'interface de ligne de commande Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).

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
	
	Ensuite, exportez la variable *Token*. Exemple :
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Remarque :** le jeton exclut *Bearer*.
		
3. Obtenez l'identificateur global unique de l'espace.

    Pour obtenir l'identificateur global unique de l'espace, voir [Comment obtenir l'identificateur global unique d'un espace ?](/docs/service/cloud-monitoring/qa/cli_qa.html#space_guid). Un identificateur global unique identifiant un espace doit avoir le préfixe *s-*.
    
    Par exemple, exécutez la commande suivante :
	
	```
	bx iam space NAME --guid
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
	
	Ensuite, exportez la variable *Space*. Exemple :
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. Exécutez une commande cURL pour envoyer des métriques.

    ```
	curl -XPOST -d @Metric_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics
	```
	{: codeblock}
	
	où
	
	* Metrics_File représente le fichier JSON qui comporte la définition des métriques envoyées au service {{site.data.keyword.monitoringshort}}.
	
	* *X-Auth-User-Token* est un paramètre qui transmet le jeton {{site.data.keyword.Bluemix_notm}} IAM ou la clé d'API.
	
	* *Auth_Type* est le préfixe qui définit le type de jeton ou de clé d'API. La liste ci-dessous répertorie les valeurs valides : *apikey*, *iam* et *uaa*, où

        * *apikey* indique que le jeton est une clé d'API.
		* *iam* indique que le jeton spécifié est un jeton généré par IAM.
	
	* *X-Auth-Scope-Id* est un paramètre qui transmet l'identificateur global unique de l'espace. Un identificateur global unique identifiant un espace doit avoir le préfixe *s-*. 
	
	* Token représente le jeton IAM ou la clé d'API.
	
	* Space représente l'identificateur global unique de l'espace. 

	
Par exemple, vous pouvez exécuter la commande suivante pour envoyer deux métriques, `myhost.cpu.idle` et `myapp.login.retries`, à un espace dans le service {{site.data.keyword.monitoringshort}} :
```
curl -XPOST @metric.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/metrics
```
{: screen}
	
Exemple de fichier *metric.json* :

```
[
  {
    "name" : "myhost.cpu.idle",
    "value" : 22.37
  },
  {
    "name": "myapp.login.retries",
    "value": 4
  }
]
```
{: screen}


















 
