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


# Getting a UAA token
{: #auth_uaa}

Use the {{site.data.keyword.Bluemix}} UAA model to get an authentication token that you can use to work with the {{site.data.keyword.monitoringshort}} service. You can obtain the authentication token by using the {{site.data.keyword.Bluemix_notm}} CLI, or by using the `Login` REST API.
{:shortdesc}

The token has an expiration time. 
		
## Getting the UAA token by using the IBM Cloud CLI
{: #uaa_cli}


To get the UAA token, complete the following steps:

1. (Pre-req) Install the {{site.data.keyword.Bluemix_notm}} CLI.

   For more information, see [Installing the {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa).
   
   If the CLI is installed, continue with the next step.
    
2. Log in to a region, organization, and space in the {{site.data.keyword.Bluemix_notm}}. 

    For more information, see [How do I log in to the {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#login).
	
3. Run the `bx iam oauth-tokens` command to get the UAA token.

    ```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	The output returns the UAA token.

**Note:** When you use the token, remove *Bearer* from the value of the token that you pass in API calls.
	


	