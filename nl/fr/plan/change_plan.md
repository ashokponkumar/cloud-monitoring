---

copyright:
  years: 2017
lastupdated: "2017-07-10"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Changement de plan
{: #change_plan}

Votre pouvez changer de plan de service {{site.data.keyword.monitoringshort}} dans {{site.data.keyword.Bluemix}} dans le tableau de bord du service ou en exécutant la commande `cf update-service`. Vous pouvez mettre à niveau ou rétrograder votre plan à tout moment.
{:shortdesc}

## Changement de plan de service dans l'interface utilisateur
{: #change_plan_ui}

Pour changer de plan de service dans {{site.data.keyword.Bluemix_notm}} depuis le tableau de bord du service, procédez comme suit :

1. Connectez-vous à {{site.data.keyword.Bluemix_notm}}, puis cliquez sur le service {{site.data.keyword.monitoringshort}} dans le tableau de bord {{site.data.keyword.Bluemix_notm}}. 
    
2. Sélectionnez l'onglet **Plan** dans l'interface utilisateur {{site.data.keyword.Bluemix_notm}}.

    Les informations relatives au plan en cours sont affichées.
	
3. Sélectionnez un nouveau plan pour mettre à niveau votre plan ou le rétrograder. 

4. Cliquez sur **Sauvegarder**.



## Changement de plan de service via l'interface de ligne de commande
{: #change_plan_cli}

Pour changer de plan de service dans {{site.data.keyword.Bluemix_notm}} via l'interface de ligne de commande, procédez comme suit :

1. 1. Connectez-vous à une région, une organisation et un espace {{site.data.keyword.Bluemix_notm}}. Exécutez la commande :

    ```
    cf login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}
	
2. Exécutez la commande `cf services` pour identifier le plan en cours et pour obtenir le nom du service {{site.data.keyword.loganalysisshort}} depuis la liste des services disponible dans l'espace.  

    La valeur de la zone **nom** est celle que vous devez utiliser pour changer de plan. 

    Par exemple,
	
	```
	$ cf services
	Obtention des services dans l'organisation MyOrg / l'espace dev en tant que xxx@yyy.com...
	OK
	nom             service      plan   applications liées dernière opération
	Monitoring-0c   Monitoring   premium                   create succeeded
    ```
	{: screen}
    
3. Mettez à jour votre plan ou rétrogradez-le. Exécutez la commande `cf update-service`.
    
	```
	cf update-service service_name [-p new_plan]
	```
	{: codeblock}
	
	où 
	
	* *service_name* est le nom de votre service. Vous pouvez exécuter la commande `cf services` pour obtenir cette valeur.
	* *new_plan* est le nom du plan.
	
	Le tableau suivant répertorie les différents plans et les valeurs prises en charge :
	
	<table>
	  <caption>Tableau 1. Liste des plans</caption>
	  <tr>
	    <th>Plan</th>
		<th>Fonctions</th>
	    <th>Nom</th>
	  </tr>
	  <tr>
	    <td>Lite</td>
	    <td>Surveillance gratuite.</td>
		<td>lite</td>
	  </tr>
	  <tr>
	    <td>Premium</td>
	    <td>Surveillance premium.</td>
		<td>premium</td>
	  </tr>
	</table>
	
	Par exemple, pour rétrograder votre plan et passer au plan *Lite*, exécutez la commande suivante :
	
	```
	cf update-service "Monitoring-0c" -p lite
    Mise à jour de l'instance de service Monitoring-0c en tant que xxx@yyy.com...
    OK
	```
	{: screen}

4. Vérifiez que le plan a été changé. Exécutez la commande `cf services`.

    ```
	cf services
    Obtention des services dans l'organisation MyOrg / l'espace dev en tant que xxx@yyy.com...
    OK

    nom               service       plan   applications liées  dernière opération
    Monitoring-0c     Monitoring    lite                       create succeeded
	```
	{: screen}






