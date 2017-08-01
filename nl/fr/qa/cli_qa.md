---

copyright:
  years: 2017

lastupdated: "2017-06-19"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Questions fréquentes sur l'utilisation de l'interface de ligne de commande Bluemix et réponses
{: #cli_qa}

Vous trouverez ci-après les réponses aux questions fréquentes concernant l'utilisation de l'interface de ligne de commande {{site.data.keyword.Bluemix}} avec le service {{site.data.keyword.monitoringshort}}. 
{:shortdesc}

* [Comment installer l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}} ?](#install_bmx_cli)
* [Comment obtenir l'identificateur global unique d'un compte ?](#account_guid)
* [Comment obtenir l'identificateur global unique d'une organisation ? ](#org_guid)
* [Comment obtenir l'identificateur global unique d'un espace ?](#space_guid)


## Comment installer l'interface de ligne de commande Bluemix ?
{: #install_bmx_cli}

Procédez comme suit pour installer l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}} :

1. Téléchargez l'interface de ligne de commande.

    Par exemple, pour installer le package d'interface de ligne de commande {{site.data.keyword.Bluemix_notm}} sur un système Ubuntu, téléchargez le [package d'interface de ligne de commande {{site.data.keyword.Bluemix_notm}} ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://clis.ng.bluemix.net/ui/home.html){: new_window}. 

2. Exécutez la commande suivante pour extraire le package d'interface de ligne de commande {{site.data.keyword.Bluemix_notm}} :
    
    ```
    tar -xvf Bluemix_CLI_0.5.4_amd64.tar.gz
    ```
    {: codeblock}
    
3. Accédez au répertoire *Bluemix_CLI* et exécutez la commande `./install_bluemix_cli` avec les droits root afin d'installer le plug-in CF. Vous pouvez exécuter la commande en tant qu'utilisateur root ou utiliser la commande sudo pour obtenir les droits root. Par exemple :
    
    ```
    cd Bluemix_CLI
    ```
    {: codeblock}
    
    ```
    sudo ./install_bluemix_cli
    ```
    {: codeblock}
    
4. Vérifiez l'installation du plug-in CF. Par exemple, vérifiez la version du plug-in CF. Exécutez la commande suivante :
    
    ```
    cf -v
    ```
    {: codeblock}
    
    La sortie est similaire à :
    
    ```
    cf version 6.25.0+787326d.2017-02-28
    ```
    {: screen}
    
5. Vérifiez l'installation de l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}. Par exemple, vérifiez la version du plug-in. Exécutez la commande suivante :
    
    ```
    bx -version
    ```
    {: codeblock}
    
    La sortie est similaire à :
    
    ```
    bx version 0.5.4+ae22935-2017-05-18T03:55:55+00:00
    ```
    {: screen}
	
## Comment obtenir l'identificateur global unique d'un compte ?
{: #account_guid}
	
Procédez comme suit pour obtenir l'identificateur global unique d'un compte :
	
1. Connectez-vous à une région, une organisation et un espace {{site.data.keyword.Bluemix_notm}}. Exécutez la commande :

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    Suivez les instructions. Entrez vos données d'identification {{site.data.keyword.Bluemix_notm}} et sélectionnez une organisation et un espace.
	
2. Exécutez la commande `bx iam accounts` pour obtenir l'identificateur global unique d'un compte.

    ```
	bx iam accounts
	```
	{: codeblock} 
	
	Par exemple, afin d'extraire tous les comptes et les identificateurs globaux uniques correspondants pour l'utilisateur xxx@yyy.com, exécutez la commande :
	
	```
	bx iam accounts
	Extraction de tous les comptes de xxx@yyy.com...
    OK
    Identificateur global unique du compte   Nom                                Type    Etat     ID utilisateur du propriétaire
    12345123451234512345123451234512         A Account                          TRIAL   ACTIVE   xxx@yyy.com
    23456234562345622456234561234561         B Account                          TRIAL   ACTIVE   zzz@yyy.com   
	```
	{: screen}

	
## Comment obtenir l'identificateur global unique d'une organisation ?
{: #org_guid}

Procédez comme suit pour obtenir l'identificateur global unique d'une organisation :
	
1. Connectez-vous à une région, une organisation et un espace {{site.data.keyword.Bluemix_notm}}. Exécutez la commande :

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    Suivez les instructions. Entrez vos données d'identification {{site.data.keyword.Bluemix_notm}} et sélectionnez une organisation et un espace.

2. Exécutez la commande `bx iam org` pour obtenir l'identificateur global unique d'une organisation. 

    ```
    bx iam org NAME --guid
    ```
    {: codeblock}
	
    où NAME est le nom de l'organisation {{site.data.keyword.Bluemix_notm}}.
		
## Comment obtenir l'identificateur global unique d'un espace ?
{: #space_guid}
	
Procédez comme suit pour obtenir l'identificateur global unique d'un espace :
	
1. Connectez-vous à une région, une organisation et un espace {{site.data.keyword.Bluemix_notm}}. Exécutez la commande :

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    Suivez les instructions. Entrez vos données d'identification {{site.data.keyword.Bluemix_notm}} et sélectionnez une organisation et un espace.
	
2. Exécutez la commande `bx iam space` pour obtenir l'identificateur global unique d'un espace. 

    ```
    bx iam space NAME --guid
    ```
    {: codeblock}
	
    où NAME est le nom d'un espace {{site.data.keyword.Bluemix_notm}}. 
	
    Par exemple, pour obtenir l'identificateur global unique de l'espace *dev*, exécutez la commande suivante :
	
    ```
    bx iam space dev --guid
    e03afff1-3456-4af6-1234-59treg1b0abc
    ```
    {: screen}




		
		
