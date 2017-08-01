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



# Extraction de l'historique d'une règle
{: #retrieve_history}


Pour extraire l'historique d'une alerte, procédez comme suit :

1. Connectez-vous à une région, une organisation et un espace {{site.data.keyword.Bluemix_notm}}. Exécutez la commande :

    Par exemple, pour vous connecter à la région du sud des États-Unis, exécutez la commande suivante :
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Suivez les instructions. Entrez vos données d'identification {{site.data.keyword.Bluemix_notm}} et sélectionnez une organisation et un espace.

2. Obtenez le jeton d'authentification ou la clé d'API.

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
	
4. Exécutez la commande cURL suivante pour obtenir l'historique d'une alerte :

    ```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/history?rule=Rule_Name
	```
	{: codeblock}
	
	où
	
	* *X-Auth-User-Token* est un paramètre qui transmet le jeton {{site.data.keyword.Bluemix_notm}} UAA, le jeton IAM ou la clé d'API.
	
	* *Auth_Type* est le préfixe qui définit le type de jeton ou de clé d'API. La liste ci-dessous répertorie les valeurs valides : *apikey*, *iam* et *uaa*, où

        * *apikey* indique que le jeton est une clé d'API.
		* *iam* indique que le jeton spécifié est un jeton généré par IAM.
		* *uaa* indique que le jeton spécifié est un jeton généré par UAA.
	
	* *X-Auth-Scope-Id* est un paramètre qui transmet l'identificateur global unique de l'espace. Un identificateur global unique identifiant un espace doit avoir le préfixe *s-*. Cet en-tête est requis si vous utilisez un jeton d'authentification UAA. Si vous utilisez un jeton d'authentification IAM, cet en-tête est facultatif et les informations de domaine qui sont associées au jeton sont utilisées.
	
	* Token est le jeton d'authentification UAA ou IAM, ou la clé d'API.
	
	* Space est l'identificateur global unique de l'espace. Il n'est requis que si vous utilisez un jeton UAA.
	
	* Rule_Name est le nom de la règle utilisée pour déclencher l'alerte. La valeur est celle spécifiée dans la zone *name*.
	
    
Par exemple, l'historique de la règle `highNginxCPU` est le suivant :

```
[
 {
 "rule": "highNginxCPU",
 "timestamp": "2017-04-17T22.23.21.000",
 "value": 99.5,
 "from_level": "OK",
 "to_level": "ERROR"
 },
 {
 "rule": "highNginxCPU",
 "timestamp": "2017-04-17T10.11.12.123",
 "value": 98.5,
 "from_level" : "OK",
 "to_level": "WARN"
 },
 {
 "rule": "highNginxCPU",
 "timestamp": "2017-04-17T10.12.14.000",
 "value": 97.0,
 "from_level" : "WARN",
 "to_level": "OK"
 }
]
```
{: screen}


