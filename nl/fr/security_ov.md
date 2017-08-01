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


# Sécurité
{: #security_ov}

Vous pouvez utiliser d'autres méthodes d'authentification pour envoyer des métriques au service {{site.data.keyword.monitoringshort}}. Les droits d'accès, de modification et de suppression des métriques sont contrôlés à l'aide de rôles.
{:shortdesc}

   
## Modèles d'authentification
{: #auth}

Une jeton d'authentification ou une clé d'API est nécessaire pour accéder aux métriques qui sont stockées dans le service {{site.data.keyword.monitoringshort}} pour un espace {{site.data.keyword.Bluemix_notm}}.  

Le service {{site.data.keyword.monitoringshort}} prend en charge les modèles d'authentification suivants :

* [L'authentification {{site.data.keyword.Bluemix_notm}} UAA](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_uaa)
* [L'authentification {{site.data.keyword.Bluemix_notm}} IAM](/docs/services/cloud-monitoring/security/auth_iam.html#auth_iam)

Un jeton UAA et un jeton IAM arrivent à expiration au bout d'un certain temps. La clé d'API n'arrive jamais à expiration. 

Si la clé d'API est compromise, vous pouvez la révoquer en la supprimant. Ensuite, vous pouvez en créer une nouvelle. Pour plus d'informations, voir [Révocation d'une clé d'API à l'aide de l'interface utilisateur Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#revoke_ui). 

Le modèle d'authentification IAM offre des fonctions d'interface utilisateur, d'interface de ligne de commande ou de gestion d'API. Vous pouvez utiliser l'interface de ligne de commande pour gérer des jetons UAA. 

Le tableau suivant répertorie les modèles d'authentification pris en charge pour chaque type de domaine :

<table>
  <caption>Tableau 1. Modèles d'authentification pris en charge pour chaque domaine</caption>
  <tr>
    <th></th>
	<th align="right">Compte</th>
    <th align="right">Organisation</th>
    <th align="right">Espace</th>	
  </tr>
  <tr>
    <th align="left">UAA</th>
	<td align="center">Non</td>
	<td align="center">Oui</td>
	<td align="center">Oui</td>
  </tr>
  <tr>
    <th align="left">IAM</th>
	<td align="center">Oui</td>
	<td align="center">Oui</td>
	<td align="center">Oui</td>
  </tr>
</table>

Le service {{site.data.keyword.monitoringshort}} utilise la fonction de contrôle d'accès IAM pour définir les actions et les opérations qu'un utilisateur ou un service peut effectuer pour le domaine. Par exemple, vous pouvez définir une règle IAM qui autorise un utilisateur à envoyer et à créer des tableaux de bord dans un domaine, mais qui ne lui permet pas d'extraire les métriques depuis le domaine.



## Rôles Bluemix UAA
{: #bmx_roles}

Le tableau suivant répertorie les privilèges de chaque rôle {{site.data.keyword.Bluemix_notm}} pouvant être utilisé avec le service {{site.data.keyword.monitoringshort}} : 

<table>
  <caption>Tableau 2. Rôles et privilèges {{site.data.keyword.Bluemix_notm}} pouvant être utilisés avec le service {{site.data.keyword.monitoringshort}}. </caption>
  <tr>
    <th>Rôle</th>
	<th>Domaine</th>
	<th>Privilèges d'accès</th>
  </tr>
  <tr>
    <td>Responsable</td>
	<td>Organisation <br>Espace</td>
	<td>Toutes les API RESTful</td>
  </tr>
  <tr>
    <td>Développeur</td>
	<td>Espace</td>
	<td>Toutes les API RESTful</td>
  </tr>
  <tr>
    <td>Auditeur</td>
	<td>Organisation <br>Espace</td>
	<td>Uniquement les API RESTful qui effectuent des opérations en lecture seule, comme interroger des métriques.</td>
  </tr>
</table>


## Rôles Bluemix IAM
{: #iam_roles}

Le tableau ci-dessous répertorie la relation entre l'API, une action de service et un rôle IAM qui est utilisée par le service {{site.data.keyword.monitoringshort}}.

<table>
  <caption>Tableau 3. Relation entre l'API, une action de service et un rôle IAM </caption>
  <tr>
    <th>API</th>
	<th>Action</th>
	<th>Rôle IAM</th>
	<th>Description</th>
  </tr>
  <tr>
    <td>POST /v1/metrics</td>
    <td>domain.write</td>
	<td>Administrateur, Editeur, Opérateur</td>
	<td>Envoyer des métriques au domaine</td>
  </tr>
  <tr>
    <td>GET /v1/metrics</td>
    <td>domain.render</td>
	<td>Administrateur, Editeur, Afficheur</td>
	<td>Extraire/interroger des métriques</td>
  </tr>
  <tr>
    <td>GET /v1/metrics/list</td>
    <td>domain.find</td>
	<td>Administrateur, Editeur</td>
	<td>Rechercher des métriques dans le domaine</td>
  </tr>
</table>

## Obtention du jeton de sécurité ou de la clé d'API
{: #get_token}

Utilisez le modèle {{site.data.keyword.Bluemix_notm}} UAA afin d'obtenir un jeton d'authentification que vous pouvez utiliser pour accéder aux données stockées dans le service {{site.data.keyword.monitoringshort}} pour un espace dans {{site.data.keyword.Bluemix_notm}}. Vous pouvez vous procurer le jeton d'authentification via l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}} ou l'API REST `Login`. Pour plus d'informations, voir [Obtention du jeton UAA via l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_cli) et  [Obtention du jeton UAA via l'API](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_api).

Utilisez le modèle {{site.data.keyword.Bluemix_notm}} IAM afin d'obtenir un jeton d'authentification que vous pouvez utiliser pour accéder aux données qui sont stockées dans le service {{site.data.keyword.monitoringshort}} ou afin d'obtenir une clé d'API. Le jeton possède un délai d'expiration. La clé d'API n'arrive jamais à expiration. Pour plus d'informations, voir [Obtention du jeton IAM via l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}} ](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli), [Génération d'une clé d'API IAM via l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli) ou [Génération d'une clé d'API IAM via l'interface utilisateur {{site.data.keyword.Bluemix_notm}} ](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_ui).



