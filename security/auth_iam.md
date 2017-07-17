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


# Authenticating by using the Bluemix IAM model
{: #auth_iam}

Use the {{site.data.keyword.Bluemix}} IAM model to get an authentication token that you can use to access metrics that are stored in the {{site.data.keyword.monitoringshort}} service or to get an API key. The token has an expiration time. The API key does not expire.
{:shortdesc}


## Getting the IAM token by using the Bluemix CLI 
{: #iam_token_cli}

To get the authorization token by using the {{site.data.keyword.Bluemix_notm}} CLI, complete the following steps:

1. Install the {{site.data.keyword.Bluemix_notm}} CLI.

   For more information, see [Installing the {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa).
   
   If the CLI is installed, continue with the next step.
    
2. Log in to a {{site.data.keyword.Bluemix_notm}} region, organization, and space. Run the command:

    For example, for the US South region, run the following command:
	
    ```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Follow the instructions. Enter your {{site.data.keyword.Bluemix_notm}} credentials, select an organization and a space.
	
3. Run the `bx iam oauth-tokens` command to get the IAM token.

    ```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	The output returns the IAM token that you must use to authenticate your user ID in that space and organization.

**Note:** When you use the token, remove *Bearer* from the value of the token that you pass in an API call.
		
		
## Generating an IAM API key by using the Bluemix CLI
{: #iam_apikey_cli}

Complete the following steps to generate an IAM API key by using the {{site.data.keyword.Bluemix_notm}} CLI:

1. Install the {{site.data.keyword.Bluemix_notm}} CLI.

   For more information, see [Installing the {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa).
   
   If the CLI is installed, continue with the next step.
	
2. Log in to {{site.data.keyword.Bluemix_notm}} by using the `bx login` {{site.data.keyword.Bluemix_notm}} CLI command:

    For example, for the US South region, run the following command:
	
    ```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Follow the instructions. Enter your {{site.data.keyword.Bluemix_notm}} credentials, select an organization and a space.
 
3. Run the `bx iam api-key-create` command to create an IAM API key.

    ```
    bx iam api-key-create NAME [-d DESCRIPTION] [-f, --file FILE]
	```
	{: codeblock} 
	
	where
	
	* NAME is the name of the API key to be created.
	* DESCRIPTION is text that you use to describe the API key.
	* FILE is the name of the file where the key is saved.
	
    For example, to create an API key *MyKey*, run the following command:
	
	```
	bx iam api-key-create MyKey -d "This is my API key for service X" 
	```
	{: screen}
	
	
	
	
## Generating an IAM API key by using the Bluemix UI
{: #iam_apikey_ui}

Complete the following steps to generate an IAM API key through the {{site.data.keyword.Bluemix_notm}} UI:

1. Log in to the {{site.data.keyword.Bluemix_notm}} console. 

2. From the console menu bar, click **Manage > Security > Bluemix API keys**.

3. Click **Create API key**.

4. Enter a name and description for your API key. Then, click **Create API key**.

5. Save the API key. Click **Download API key**.

    Click **Show** to display the API key.  

**Note:** The API key is only available to show or download at the time of creation. If the API key is lost, you must create a new API key.  


	
## Revoking an API key by using the Bluemix UI
{: #revoke_ui}
	
Complete the following steps to revoke an IAM API key through the {{site.data.keyword.Bluemix_notm}} UI:

1. Log in to the {{site.data.keyword.Bluemix_notm}} console.

2. From the console menu bar, click **Manage > Security > Bluemix API keys**.

3. Click **Actions**, and then **Delete key**.





	

	
	
	
	
	
	