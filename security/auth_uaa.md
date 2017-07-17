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


# Authenticating by using the Bluemix UAA model
{: #auth_uaa}

Use the {{site.data.keyword.Bluemix}} UAA model to get an authentication token that you can use to access metrics that are stored in the {{site.data.keyword.monitoringshort}} service for a space in a {{site.data.keyword.Bluemix_notm}}. You can obtain the authentication token by using the {{site.data.keyword.Bluemix_notm}} CLI or by using the `Login` REST API.
{:shortdesc}

To access metrics by using a UAA authentication token, you need the following information:

* A UAA token to access the {{site.data.keyword.monitoringshort}} service by using the RESTful APIs.
* The GUID of the space.

		
## Getting the UAA token by using the Bluemix CLI
{: #uaa_cli}


To get the authorization token, complete the following steps:

1. Install the {{site.data.keyword.Bluemix_notm}} CLI.

   For more information, see [Installing the {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa).
   
   If the CLI is installed, continue with the next step.
    
2. Log in to a {{site.data.keyword.Bluemix_notm}} region, organization, and space. Run the command:

    ```
    cf login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    Follow the instructions. Enter your {{site.data.keyword.Bluemix_notm}} credentials, select an organization and a space.
	
3. Run the `cf oauth-token` command to get the {{site.data.keyword.Bluemix_notm}} UAA token.

    ```
	cf oauth-token
	```
	{: codeblock}
	
	The output returns the UAA token that you must use to authenticate your user ID in that space and organization.

4. Get the GUID for the space of the organization for which you have obtained an authentication token.

   For more information, see [How do I get the GUID of a space](/docs/services/cloud-monitoring/qa/cli_qa.html#space_guid).  
	
5. Export the following variables: TOKEN and SPACE.

    * *TOKEN* is the oauth token that you got in the previous step excluding Bearer.
	
	* *SPACE* is the GUID of the space that you got in the previous step.
		
	For example,
	
	```
	export TOKEN="eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8"
	export SPACE="667fb895-abcd-defg-aaaa-cf4587341095"
	```
	{: screen}
	
6. Run the following command to get the UAA token to work with the {{site.data.keyword.monitoringshort}} service:
 
    ```
	curl -k -X GET  --header "X-Auth-Token: ${TOKEN}"  --header "X-Auth-Project-Id: ${SPACE}"  METRICS_ENDPOINT
    ```
    {: codeblock}	
	
	where
	* SPACE is the GUID of the space where the service is running.
	* TOKEN is the {{site.data.keyword.Bluemix_notm}} UAA token that you get in a previous step without the bearer prefix.
	* METRICS_ENDPOINT is the metrics endpoint (https://metrics.ng.bluemix.net/token) for the {{site.data.keyword.Bluemix_notm}} region where the organization and space are available.

	
## Getting the UAA token by using the API
{: #uaa_api}

To get the authorization token, run the following cURL command:

```
curl -XPOST -d 'user=USERNAME&passwd=PASSWORD&space=SPACE_NAME&organization=ORG_NAME' METRICS_ENDPOINT
```
{: codeblock}

where

* USERNAME is a {{site.data.keyword.Bluemix_notm}} ID for which you want to get the authentication token to work with the {{site.data.keyword.monitoringshort}} service.
* PASSWORD is the password of the user ID used to log in to {{site.data.keyword.Bluemix_notm}}.
* SPACE_NAME is the name of the space where metrics are collected.
* ORG_NAME is the organization name in {{site.data.keyword.Bluemix_notm}} where the space is hosted.
* METRICS_ENDPOINT is the metrics endpoint (https://metrics.ng.bluemix.net/login) for the {{site.data.keyword.Bluemix_notm}} region where the organization and space are available.

The output is a JSON document that contains the UAA token. Get the value for the token from the field **access-token**. 

For example, a sample JSON document looks as follows:

```
{
    "access_token": "eyJhbGc...",
    "logging_token": "xxxxxxxxxxxx",
    "org_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "space_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```
{: screen}

The value of the *logging_token* corresponds to the UAA token that you need to work with the {{site.data.keyword.monitoringshort}} service.
 
**Note:** 

* If you are not using cURL, you must set the header **Content-Type: application/x-www-form-urlencoded**.
* If you get the error code *BXNMS0122E: User credentials are invalid*, check that you are using a valid {{site.data.keyword.IBM_notm}} ID.


