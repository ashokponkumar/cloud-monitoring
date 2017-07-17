---

copyright:
  years: 2017

lastupdated: "2017-07-10"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Provisioning the Monitoring service
{: #provision}

You can provision the {{site.data.keyword.monitoringshort}} service from {{site.data.keyword.Bluemix}} UI, or from the command line.
{:shortdesc}


## Provisioning from the UI
{: #ui}

Complete the following steps to provision an instance of the {{site.data.keyword.monitoringshort}} service in {{site.data.keyword.Bluemix_notm}}:

1. Log in to your {{site.data.keyword.Bluemix_notm}} account.

    The {{site.data.keyword.Bluemix_notm}} dashboard can be found at: [http://bluemix.net ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://bluemix.net){:new_window}.
    
	After you log in with your user ID and password, the {{site.data.keyword.Bluemix_notm}} UI opens.

2. Click **Catalog**. The list of the services that are available on {{site.data.keyword.Bluemix_notm}} opens.

3. Select the **DevOps** category to filter the list of services that is displayed.

4. Click the **Monitoring** tile.

5. Select a service plan. To collect and access metrics for up to 45 days, and to define alerting rules, including rules with wildcards, select the **Premium** plan. By default, the **Lite** plan is set, which entitles you to the collection of platform metrics in the space where you are provisioning the service, and a 15 days retention period for those metrics. 

    For more information about the service plans, see [Service plans](/docs/services/cloud-monitoring/monitoring_ov.html#plans).
	
6. Click **Create** to provision the {{site.data.keyword.monitoringshort}} service in the {{site.data.keyword.Bluemix_notm}} space where you are logged in.
  
 

## Provisioning from the CLI
{: #cli}

Complete the following steps to provision an instance of the {{site.data.keyword.monitoringshort}} service in {{site.data.keyword.Bluemix_notm}} through the command line:

1. [Pre-requisite] Install the {{site.data.keyword.Bluemix_notm}} CLI.

   For more information, see [Installing the {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa).
   
   If the CLI is installed, continue with the next step.
    
2. Log in to a {{site.data.keyword.Bluemix_notm}} region, organization, and space. Run the command:

    ```
    cf login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    Follow the instructions. Enter your {{site.data.keyword.Bluemix_notm}} credentials, select an organization and a space.
	
3. Run the `cf create-service` command to provision an instance.

    ```
	cf create-service service_name service_plan service_instance_name
	```
	{: codeblock}
	
	Where
	
	* service_name is the name of the service, that is, **Monitoring**.
	* service_plan is the service plan name. To collect and access metrics for up to 45 days, and to define alerting rules, including rules with wildcards, select the **Premium** plan. By default, the **Lite** plan is set, which entitles you to the collection of platform metrics in the space where you are provisioning the service, and a 15 days retention period for those metrics. For a list of plans, see [{{site.data.keyword.monitoringshort}} service plans](/docs/services/cloud-monitoring/monitoring_ov.html#plan).
	* service_instance_name is the name that you want to use for the new service instance that you create.
	
	For more information about the cf command, see [cf create-service](/docs/cli/reference/cfcommands/index.html#cf_create-service).

	For example, to create an instance of the {{site.data.keyword.monitoringshort}} service with a premium plan, run the following command:
	
	```
	cf create-service Monitoring premium my_monitoring_svc
	```
	{: codeblock}
	
4. Verify that the service is created successfully. Run the following command:

    ```	
	cf services
	```
	{: codeblock}
	
	The output of running the command looks as follows:
	
	```
    Getting services in org MyOrg / space MySpace as xxx@yyy.com...
    OK
    
    name                           service                  plan                   bound apps              last operation
    my_monitoring_svc              Monitoring               premium                                        create succeeded
	```
	{: screen}

	



