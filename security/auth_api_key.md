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


# Working with API Keys
{: #auth_api_key}

You can get an API by using the {{site.data.keyword.Bluemix}} CLI or the {{site.data.keyword.Bluemix_notm}} UI. The API key does not expire. If an API is compromissed, you can revoke it and generate a new one.
{:shortdesc}

## Generating an IAM API key by using the IBM Cloud CLI
{: #iam_apikey_cli}

Complete the following steps to generate an API key by using the {{site.data.keyword.Bluemix_notm}} CLI:

1. (Pre-req) Install the {{site.data.keyword.Bluemix_notm}} CLI.

   For more information, see [Installing the {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa).
   
   If the CLI is installed, continue with the next step.
	
2. Log in to a region, organization, and space in the {{site.data.keyword.Bluemix_notm}}. 

    For more information, see [How do I log in to the {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#login).
 
3. Run the `bx iam api-key-create` command to create an API key.

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
	
	
	
	
## Generating an IAM API key by using the IBM Cloud UI
{: #iam_apikey_ui}

Complete the following steps to generate an API key through the {{site.data.keyword.Bluemix_notm}} UI:

1. Log in to the {{site.data.keyword.Bluemix_notm}} console.

    Open a web browser and launch the {{site.data.keyword.Bluemix_notm}} dashboard: [http://console.bluemix.net ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://bluemix.net){:new_window}
	
	After you log in with your user ID and password, the {{site.data.keyword.Bluemix_notm}} UI opens.

2. From the console menu bar, click **Manage > Security > IBM Cloud API keys**.

3. Click **Create API key**.

4. Enter a name and description for your API key. Then, click **Create API key**.

5. Save the API key. Click **Download API key**.

    Click **Show** to display the API key.  

**Note:** The API key is only available to show or download at the time of creation. If the API key is lost, you must create a new API key.  


	
## Revoking an API key by using the IBM Cloud UI
{: #revoke_ui}
	
Complete the following steps to revoke an IAM API key through the {{site.data.keyword.Bluemix_notm}} UI:

1. Log in to the {{site.data.keyword.Bluemix_notm}} console.

    Open a web browser and launch the {{site.data.keyword.Bluemix_notm}} dashboard: [http://console.bluemix.net ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://bluemix.net){:new_window}
	
	After you log in with your user ID and password, the {{site.data.keyword.Bluemix_notm}} UI opens.

2. From the console menu bar, click **Manage > Security > IBM Cloud API keys**.

3. Click **Actions**, and then **Delete key**.





	

	
	
	
	
	
	