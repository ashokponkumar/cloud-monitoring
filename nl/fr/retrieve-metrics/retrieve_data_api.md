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


# Extraction de données à partir du service Monitoring
{: #retrieve_data_api}

Vous pouvez extraire des métriques à partir du service {{site.data.keyword.monitoringshort}} à l'aide de l'[API Metrics](https://console.bluemix.net/apidocs/927-ibm-cloud-monitoring-rest-api?&language=node#introduction){: new_window}. Vous pouvez extraire des métriques à partir d'un espace {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}


Pour définir l'ensemble de données que vous prévoyez d'extraire, prenez en compte les informations suivantes : 

* Vous devez définir l'espace {{site.data.keyword.Bluemix_notm}} à partir duquel vous souhaitez extraire les données. 

* Vous devez fournir un jeton ou une clé d'API pour gérer le service {{site.data.keyword.monitoringshort}}.  

* Vous devez spécifier un chemin d'accès à 1 ou plusieurs métriques. Pour plus d'informations, voir [Définition des métriques](#metrics).

* Vous pouvez éventuellement spécifier une période personnalisée. Par défaut, si vous ne spécifiez pas de période, les données que vous extrayez sont celles qui correspondent aux 24 dernières heures. Pour plus d'informations, voir [Configuration d'une période](#time).

**Remarque :** vous pouvez extraire un maximum de 5 cibles par demande. 


## Définition des métriques
{: #metrics}

Afin d'extraire des données pour une métrique, vous devez spécifier le chemin d'accès à cette dernière à partir de la racine de l'arborescence de métriques et vous devez séparer chaque étape par un point (.).

Le chemin est spécifié dans le paramètre **Cible**. Un maximum de 5 cibles peut être spécifié pour chaque demande.  

Vous pouvez éventuellement appliquer des fonctions à la métrique. Pour plus d'informations sur les fonctions prises en charge, voir [Fonctions Graphite 0.9.15![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://graphite.readthedocs.io/en/latest/functions.html).

Vous pouvez utiliser des caractères génériques pour définir un chemin. L'utilisation de caractères génériques vous permet d'identifier plusieurs métriques dans un seul chemin.  

* **Astérisque** : vous pouvez utiliser l'astérisque (*) pour zéro ou plusieurs caractères.  

    Un élément de chemin peut contenir plusieurs astérisques, par exemple : `servers.sp*aeg*r.cpu.total.*` Cet exemple renvoie des données sur le montant total d'unité centrale pour tous les serveurs qui correspondent à ce modèle. 

* **Liste ou plage de caractères** : vous pouvez définir des caractères entre crochets ([...]) pour spécifier un seul emplacement de caractère dans la chaîne qui compose le chemin. 

    Vous pouvez définir une plage inclusive de caractères en spécifiant deux caractères séparés par un trait d'union (-). Une correspondance sera trouvée pour tout caractère placé entre ces deux caractères.  
	
	Vous pouvez définir une ou plusieurs plages de caractères entre crochets.  
	
	Lorsque les caractères inclus entre crochets ne peuvent pas être lus en tant que plage, ils sont traités comme une liste de valeurs.  
	
	Vous pouvez inclure un trait d'union (-) comme caractère dans une liste de valeurs en le plaçant au début ou à la fin des crochets. 

* **Liste de valeurs** : vous pouvez définir des valeurs séparées par une virgule entre accolades pour définir des listes de valeurs.  

    Par exemple, pour télécharger des métriques pour les chemins `servers.myserver.cpu.total.user` et `servers.myserver.cpu.total.system}`, vous pouvez définir un seul et même chemin pour les deux types : `servers.myserver.cpu.total.{user,system}`



## Définition d'une période
{: #time}

Vous pouvez éventuellement spécifier une période en utilisant un temps relatif ou un temps absolu. Vous devez définir les paramètres de requête **De** et**Jusqu'à**.  

* Lorsque l'heure de début de la période est omise, la valeur par défaut correspond à 24 heures en arrière.  

* Lorsque l'heure de fin de la période est omise, la valeur par défaut correspond à l'heure en cours (*maintenant*). 

Le paramètre de **temps relatif** est toujours précédé du signe moins (-) et suivi d'une unité de temps.  

Le tableau suivant répertorie les différentes abréviations de temps relatives que vous pouvez utiliser :


| Abréviation  | Description |
|--------------|-------------|
| s            | Secondes    |
| min          | Minutes     |
| h            | Heures      |
| j            | Jours       |
| s            | Semaines    |
| moi          | Mois        |
| maintenant   | Heure en cours |


Vous pouvez utiliser n'importe quel des formats suivants pour définir un **temps absolu**: `HH:MM_YYMMDD`, `YYYYMMDD`, `MM/DD/YY`

| Abréviation  | Description |
|--------------|-------------|
| AAAA         | Année à quatre chiffres|
| MM           | Chiffre à deux mois (01=Jan etc) |
| JJ           | Jour du mois à deux chiffres (01 à 31)|
| hh           | Heure à deux chiffres (00 à 23)|
| mm           | Minute à deux chiffres (00 à 59) |
| ss           | Seconde à deux chiffres (00 à 59)|

**Exemple**

Le tableau suivant répertorie les différents exemples illustrant comment définir différentes périodes à l'aide des paramètres *De* et *Jusqu'à*.

<table>
  <caption>Tableau 1. Différents exemples illustrant comment définir différentes périodes à l'aide des paramètres *De* et *Jusqu'à* </caption>
  <tr>
    <th>Exemple</th>
	<th>Description</th>
  </tr>
  <tr>
    <td>&from=monday</td>
	<td>Période de temps qui inclut des données comprises entre le dernier lundi jusqu'à maintenant.</td>
  </tr>
  <tr>
    <td>&from=20171101&until=20171130</td>
	<td>Période de temps définie sur un mois, en l'occurrence, sur novembre. </td>
  </tr>
  <tr>
    <td>&from=02:00_20170701&until=14:00_20170701</td>
	<td>Permet d'afficher des données sur un cycle de 24 heures, par exemple, entre 02h00 et 14h00, le 1er juillet 2017. </td>
  </tr>
  <tr>
    <td>&from=-8d&until=-7d</td>
	<td>Permet d'afficher les données correspondant au même jour de la semaine, en l'occurrence, la semaine dernière. </td>
  </tr>
  
</table>


## Définition de l'espace
{: #domain}

Lorsque vous utilisez le modèle d'authentification {{site.data.keyword.Bluemix_notm}} UAA ou le modèle d'authentification {{site.data.keyword.Bluemix_notm}} IAM pour vous authentifier auprès du service {{site.data.keyword.monitoringshort}}, la zone d'en-tête **X-Auth-Scope-Id** est requise et doit prendre la valeur de l'identificateur global unique de l'espace. Un identificateur global unique identifiant un espace doit avoir le préfixe *s-*.

    



## Définition du jeton d'autorisation ou de la clé d'API
{: #security}
  
Vous devez configurer la zone **X-Auth-User-Token** pour définir le jeton UAA, le jeton IAM ou la clé d'API utilisée à des fins d'authentification.  

Un jeton ou une clé d'API doit avoir pour préfixe l'une des valeurs suivantes : `apikey`, `iam` ou `uaa` 

où 

* *apikey* identifie le jeton comme étant une clé d'API. 
* *iam* identifie le jeton spécifié comme étant un jeton généré par IAM. 
* *uaa* identifie le jeton spécifié comme étant un jeton généré par UAA. 

Par exemple, vous pouvez configurer l'en-tête suivant pour une clé d'API : `X-Auth-User-Token: apikey SomeIAMGeneratedKey`


## Extraction de métriques depuis un espace à l'aide d'UAA
{: #uaa}

Pour extraire des métriques à partir d'un espace {{site.data.keyword.Bluemix_notm}}, procédez comme suit :

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
	export Token="eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8"
	```
	{: screen}
	
	**Remarque :** le jeton exclut *Bearer*.
		
3. Obtenez l'identificateur global unique de l'espace. Un identificateur global unique identifiant un espace doit avoir le préfixe *s-*.

    Exécutez la commande suivante :
	
	```
	cf space SpaceName --guid
	```
	{: codeblock}
	
	Où *SpaceName* est le nom de l'espace comportant le préfixe *s-* dans lequel vous allez définir une notification.
	
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
	curl -XGET --header "X-Auth-User-Token:uaa ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics?from=Start_Time&until=End_Time&target=string
	```
	{: codeblock}

	où
	
	* *X-Auth-User-Token* est un paramètre qui transmet le jeton {{site.data.keyword.Bluemix_notm}} UAA.
	
	* *uaa* est le préfixe indiquant que le jeton spécifié est un jeton généré par UAA.
	
	* *X-Auth-Scope-Id* est un paramètre qui transmet l'identificateur global unique de l'espace. Un identificateur global unique identifiant un espace doit avoir le préfixe *s-*. Cet en-tête est requis uniquement si vous utilisez l'authentification UAA. Si vous utilisez un jeton d'authentification IAM, cet en-tête est facultatif et les informations de domaine qui sont associées au jeton sont utilisées.
	
	* *Token* représente le jeton UAA.
	
	* *Space* représente l'identificateur global unique de l'espace. Il n'est requis que si vous utilisez un jeton UAA.
	
	* *Start_Time* définit la période relative ou absolue qui marque le début de la demande. La valeur par défaut correspond à 24 heures en arrière : '-24h'. *End_Time* définit la période relative ou absolue qui marque la fin de la demande. La valeur par défaut correspond à l'heure en cours : 'maintenant'. Pour plus d'informations, voir [Définition d'une période de temps](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#time). 	
	* *Path*  identifie une ou plusieurs métriques. Vous pouvez éventuellement appliquer des fonctions à chaque métrique. Les chemins prennent également en charge l'utilisation des caractères génériques, ce qui vous permet d'identifier une ou plusieurs métriques dans un seul chemin. Pour plus d'informations, voir [Définition des métriques](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#metrics). 	
	
	
## Extraction de métriques à partir d'un espace à l'aide d'IAM ou d'une clé d'API
{: #iam}

Pour extraire des métriques à partir d'un espace {{site.data.keyword.Bluemix_notm}} à l'aide d'IAM ou d'une clé d'API, procédez comme suit :

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
	
	Où *SpaceName* est le nom de l'espace comportant le préfixe *s-* dans lequel vous allez définir une notification.
	
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
	
5. Exécutez une commande cURL pour extraire des métriques.

   	```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics?from=Start_Time&until=End_Time&target=string
	```
	{: codeblock}

	où
	
	* *X-Auth-User-Token* est un paramètre qui transmet le jeton {{site.data.keyword.Bluemix_notm}} AIM ou la clé d'API.
	
	* *Auth_Type* est le préfixe qui définit le type de jeton ou de clé d'API. 

        * *apikey* identifie le jeton comme étant une clé d'API.
		* *iam* indique que le jeton spécifié est un jeton généré par IAM.
	
	* *X-Auth-Scope-Id* est un paramètre qui transmet l'identificateur global unique de l'espace. Un identificateur global unique identifiant un espace doit avoir le préfixe *s-*. Cet en-tête est requis uniquement si vous utilisez l'authentification UAA. Si vous utilisez un jeton d'authentification IAM, cet en-tête est facultatif et les informations de domaine qui sont associées au jeton sont utilisées.
	
	* *Token* représente le jeton UAA.
	
	* *Space* représente l'identificateur global unique de l'espace. Il n'est requis que si vous utilisez un jeton UAA.
	
	* *Start_Time* définit la période relative ou absolue qui marque le début de la demande. La valeur par défaut correspond à 24 heures en arrière : '-24h'. *End_Time* définit la période relative ou absolue qui marque la fin de la demande. La valeur par défaut correspond à l'heure en cours : 'maintenant'. Pour plus d'informations, voir [Définition d'une période de temps](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#time). 	
	* *Path*  identifie une ou plusieurs métriques. Vous pouvez éventuellement appliquer des fonctions à chaque métrique. Les chemins prennent également en charge l'utilisation des caractères génériques, ce qui vous permet d'identifier une ou plusieurs métriques dans un seul chemin. Pour plus d'informations, voir [Définition des métriques](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#metrics). 	
	
	





