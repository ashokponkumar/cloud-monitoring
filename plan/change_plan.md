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


# Changing the plan
{: #change_plan}

You can change your {{site.data.keyword.monitoringshort}} service plan in {{site.data.keyword.Bluemix}} in the service Dashboard or by running the `cf update-service` command. You can upgrade or reduce your plan at any time.
{:shortdesc}

## Changing the service plan through the UI
{: #change_plan_ui}

To change your service plan in {{site.data.keyword.Bluemix_notm}} in the service Dashboard, complete the following steps:

1. Log in to {{site.data.keyword.Bluemix_notm}}, and then click the {{site.data.keyword.monitoringshort}} service from the {{site.data.keyword.Bluemix_notm}} dashboard. 
    
2. Select the **Plan** tab in the {{site.data.keyword.Bluemix_notm}} UI.

    Information about your current plan is displayed.
	
3. Select a new plan to either upgrade or reduce your plan. 

4. Click **Save**.



## Changing the service plan through the CLI
{: #change_plan_cli}

To change your service plan in {{site.data.keyword.Bluemix_notm}} through the CLI, complete the following steps:

1. 1. Log in to a {{site.data.keyword.Bluemix_notm}} region, organization, and space. Run the command:

    ```
    cf login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}
	
2. Run the `cf services` command to check your current plan, and to get the {{site.data.keyword.loganalysisshort}} service name from the list of services that is available in the space. 

    The value of the field **name** is the one that you must use to change the plan. 

    For example,
	
	```
	$ cf services
	Getting services in org MyOrg / space dev as xxx@yyy.com...
	OK
	name            service      plan   bound apps   last operation
	Monitoring-0c   Monitoring   premium             create succeeded
    ```
	{: screen}
    
3. Upgrade or reduce your plan. Run the `cf update-service` command.
    
	```
	cf update-service service_name [-p new_plan]
	```
	{: codeblock}
	
	where 
	
	* *service_name* is the name of your service. You can run the `cf services` command to get the value.
	* *new_plan* is the name of the plan.
	
	The following table lists the different plans and their supported values:
	
	<table>
	  <caption>Table 1. List of plans.</caption>
	  <tr>
	    <th>Plan</th>
		<th>Features</th>
	    <th>Name</th>
	  </tr>
	  <tr>
	    <td>Lite</td>
	    <td>Complimentary monitoring.</td>
		<td>lite</td>
	  </tr>
	  <tr>
	    <td>Premium</td>
	    <td>Premium monitoring.</td>
		<td>premium</td>
	  </tr>
	</table>
	
	For example, to reduce your plan to the *Lite* plan, run the following command:
	
	```
	cf update-service "Monitoring-0c" -p lite
    Updating service instance Monitoring-0c as xxx@yyy.com...
    OK
	```
	{: screen}

4. Verify the new plan is changed. Run the `cf services` command.

    ```
	cf services
    Getting services in org MyOrg / space dev as xxx@yyy.com...
    OK

    name              service       plan   bound apps   last operation
    Monitoring-0c     Monitoring    lite                create succeeded
	```
	{: screen}





