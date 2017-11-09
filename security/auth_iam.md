---

copyright:
  years: 2017

lastupdated: "2017-11-09"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Getting an IAM token
{: #auth_iam}

Use the {{site.data.keyword.Bluemix}} IAM model to get an authentication token that you can use to work with the {{site.data.keyword.monitoringshort}} service. You can obtain the authentication token by using the {{site.data.keyword.Bluemix_notm}} CLI.
{:shortdesc}

The token has an expiration time. 

## Getting the IAM token by using the IBM Cloud CLI 
{: #iam_token_cli}

To get the authorization token by using the {{site.data.keyword.Bluemix_notm}} CLI, complete the following steps:

1. (Pre-req) Install the {{site.data.keyword.Bluemix_notm}} CLI.

   For more information, see [Installing the {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa).
   
   If the CLI is installed, continue with the next step.
    
2. Log in to a region, organization, and space in the {{site.data.keyword.Bluemix_notm}}. 

    For more information, see [How do I log in to the {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#login).
	
3. Run the `bx iam oauth-tokens` command to get the IAM token.

    ```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	The output returns the IAM token.

**Note:** When you use the token, remove *Bearer* from the value of the token that you pass in API calls.
		



	

	
	
	
	
	
	