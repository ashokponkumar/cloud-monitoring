---

copyright:
  years: 2017, 2018

lastupdated: "2018-02-01"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Obtention d'un jeton IAM
{: #auth_iam}

Utilisez le modèle {{site.data.keyword.Bluemix}} IAM pour obtenir un jeton d'authentification que vous pouvez utiliser pour travailler avec le service {{site.data.keyword.monitoringshort}}. Vous pouvez vous procurer le jeton d'authentification via l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

Le jeton possède un délai d'expiration. 

## Obtention du jeton IAM à l'aide de l'interface de ligne de commande IBM Cloud 
{: #iam_token_cli}

Pour obtenir le jeton d'autorisation via l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}, procédez comme suit :

1. (Prérequis) Installez l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}.

   Pour plus d'informations, voir [Installation de l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}.](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa)
   
   Si l'interface de ligne de commande est installée, passez à l'étape suivante.
    
2. Connectez-vous à une région, une organisation et un espace dans {{site.data.keyword.Bluemix_notm}}. 

    Pour plus d'informations, voir [Comment se connecter à {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#login).
	
3. Exécutez la commande `bx iam oauth-tokens` pour obtenir le jeton IAM. 

    ```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	La sortie de cette commande renvoie le jeton IAM.

**Remarque :** Lorsque vous utilisez le jeton, supprimez *Bearer* de la valeur du jeton que vous transmettez dans des appels API.
		



	

	
	
	
	
	
	
