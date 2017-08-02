---

copyright:
  years: 2017

lastupdated: "2017-08-01"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Sending data to the Monitoring service
{: #send_data_api}

You can send metrics to the {{site.data.keyword.monitoringshort}} service by using the Metrics API. You can send metrics to a {{site.data.keyword.Bluemix_notm}} space.
{:shortdesc}


For {{site.data.keyword.Bluemix_notm}} Docker containers, basic system metrics are automatically collected. For Cloud Foundry applications, and apps running in a Virtual Machine (VM), metrics must be sent directly from the app by using [the Metrics API](https://console.bluemix.net/apidocs/927-ibm-cloud-monitoring-rest-api?&language=node#introduction){: new_window}. 



## Sending metrics to a space by using UAA
{: #uaa}

To send metrics to a {{site.data.keyword.Bluemix_notm}} space, complete the following steps:

1. Log in to a {{site.data.keyword.Bluemix_notm}} region, organization, and space. Run the command:

    ```
	cf login -a Endpoint
	```
	{: codeblock}
	
	Where *Endpoint* is the URL to log in to {{site.data.keyword.Bluemix_notm}}. This URL is different per region.
	
	<table>
	    <caption>List of endpoints to access {{site.data.keyword.Bluemix_notm}}</caption>
		<tr>
		  <th>Region</th>
		  <th>URL</th>
		</tr>
		<tr>
		  <td>US South</td>
		  <td>api.ng.bluemix.net</td>
		</tr>
		<tr>
		  <td>United Kingdom</td>
		  <td>api.eu-gb.bluemix.net</td>
		</tr>
	</table>

    For example, to log in to the US South region, run the following command:
	
	```
    cf login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Follow the instructions. Enter your {{site.data.keyword.Bluemix_notm}} credentials, select an organization and a space.

2. Get the UAA authentication token.

    For more information about UAA authentication, see [Getting the UAA token by using the Bluemix CLI](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) or [Getting the UAA token by using the REST API](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

	For example, to use the UAA token, run the following command:

    ```
	cf oauth-token
	```
	{: codeblock}
	
	The result of this command is the following:
	
	```
	bearer eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8
	```
	{: screen}
	
	Then, export the variable *Token*. For example:
	
	```
	export Token="eyJhbGciOiJI....cGlxryWS8"
	```
	{: screen}
	
	**Note:** The token excludes *Bearer*.
		
3. Get the space GUID. The GUID must be prefixed with *s-* to identify a space.

    Run the following command:
	
	```
	cf space SpaceName --guid
	```
	{: codeblock}
	
	Where *SpaceName* is the name of the space where you are going to define a notification.
	
	For example, to get the GUID of a space with name *dev*, run the following command:
	
	```
	cf space dev --guid
	```
	{: screen}
	
	The result of this command is the following:
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	Then, export the variable *Space*. For example:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. Run the following cURL command to send metrics:

    ```
	curl -XPOST -d @Metric_File --header "X-Auth-User-Token:uaa ${Token}" --header "X-Auth-Scope-Id: ${Space}" Endpoint/v1/metrics
	```
	{: codeblock}
	
	where
	
	* The Metrics_File represents the JSON file that includes the definition of metrics that are sent to the {{site.data.keyword.monitoringshort}} service.
	
	* The *X-Auth-User-Token* is a parameter that passes the {{site.data.keyword.Bluemix_notm}} UAA token.
	
	* *Auth_Type* is the prefix that defines the type of token. *uaa* identifies that the token specified is an UAA generated token.
	
	* *X-Auth-Scope-Id* is a parameter that passes the space GUID. The GUID must be prefixed with *s-* to identify a space.
	
	* Token represents the UAA token.
	
	* Space represents the GUID of the space. 
	
	* Endpoint represents the entry point to the service. Each region has a different URL. The following table lists the endpoints by region:
	
		<table>
	    <caption>List of endpoints</caption>
		<tr>
		  <th>Region</th>
		  <th>URL</th>
		</tr>
		<tr>
		  <td>US South</td>
		  <td>[https://metrics.ng.bluemix.net](https://metrics.ng.bluemix.net)</td>
		</tr>
		<tr>
		  <td>United Kingdom</td>
		  <td>[https://metrics.eu-gb.bluemix.net](https://metrics.eu-gb.bluemix.net)</td>
		</tr>
	    </table>
	
	For example, you can run the following command to send 2 metrics, `myhost.cpu.idle` and `myapp.login.attempts`, to the {{site.data.keyword.monitoringshort}} service:
	
	```
	curl -XPOST @metric.json --header "X-Auth-User-Token:uaa ${Token}" https://metrics.ng.bluemix.net/v1/metrics
	```
	{: screen}
	
	The sample file *metric.json* is defined as follows:

    ```
    [
      {
        "name" : "myhost.cpu.idle",
        "value" : 22.37
      },
      {
        "name": "myapp.login.retries",
        "value": 4
      }
    ]
	```
	{: screen}

 
## Sending metrics to a space by using IAM or an API key
{: #iam}

To send metrics to a {{site.data.keyword.Bluemix_notm}} space, complete the following steps:

1. Log in to a {{site.data.keyword.Bluemix_notm}} region, organization, and space. Run the command:

    ```
	bx login -a Endpoint
	```
	{: codeblock}
	
	Where *Endpoint* is the URL to log in to {{site.data.keyword.Bluemix_notm}}. This URL is different per region.
	
	<table>
	    <caption>List of endpoints to access {{site.data.keyword.Bluemix_notm}}</caption>
		<tr>
		  <th>Region</th>
		  <th>URL</th>
		</tr>
		<tr>
		  <td>US South</td>
		  <td>api.ng.bluemix.net</td>
		</tr>
		<tr>
		  <td>United Kingdom</td>
		  <td>api.eu-gb.bluemix.net</td>
		</tr>
	</table>
	
	For example, to log in to the US South region, run the following command:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Follow the instructions. Enter your {{site.data.keyword.Bluemix_notm}} credentials, select an organization and a space.

2. Get the authentication token or API key.

    For more information about IAM authentication, see [Getting the IAM token by using the Bluemix CLI](/docs/services/cloud-monitoring/security/auth_iam.html#iam_cli) or [Generating an IAM API key by using the Bluemix CLI](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).

	For example, to use the IAM token, run the following command:

    ```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	The result of this command is the following:
	
	```
	IAM token:  Bearer djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ
    UAA token:  Bearer eyJhbGciOiJIUz..Ky6vagp3k_QcIcKJ-td83qXhO5Uze43KcplG6PzcGs8
	```
	{: screen}
	
	Then, export the variable *Token*. For example:
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Note:** The token excludes *Bearer*.
		
3. Get the space GUID.

    To get the space GUID, see [How do I get the GUID of a space](/docs/service/cloud-monitoring/qa/cli_qa.html#space_guid). The GUID must be prefixed with *s-* to identify a space.
    
    For example, run the following command:
	
	```
	bx iam space NAME --guid
	```
	{: codeblock}
	
	Where *SpaceName* is the name of the space where you are going to define a notification.
	
	For example, to get the GUID of a space with name *dev*, run the following command:
	
	```
	bx iam space dev --guid
	```
	{: screen}
	
	The result of this command is the following:
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	Then, export the variable *Space*. For example:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. Run a cURL command to send metrics.

    ```
	curl -XPOST -d @Metric_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" Endpoint/v1/metrics
	```
	{: codeblock}
	
	where
	
	* The Metrics_File represents the JSON file that includes the definition of metrics that are sent to the {{site.data.keyword.monitoringshort}} service.
	
	* The *X-Auth-User-Token* is a parameter that passes the {{site.data.keyword.Bluemix_notm}} IAM token, or API key.
	
	* *Auth_Type* is the prefix that defines the type of token or API key. The following list outlines the valid values: *apikey*, *iam* or *uaa*, where

        * *apikey* identifies that the token is an API key.
		* *iam* identifies that the token specified is an IAM generated token.
	
	* *X-Auth-Scope-Id* is a parameter that passes the space GUID. The GUID must be prefixed with *s-* to identify a space. 
	
	* Token represents the IAM token, or API key.
	
	* Space represents the GUID of the space. 
	
	* Endpoint represents the entry point to the service. Each region has a different URL. The following table lists the endpoints by region:
	
		<table>
	    <caption>List of endpoints</caption>
		<tr>
		  <th>Region</th>
		  <th>URL</th>
		</tr>
		<tr>
		  <td>US South</td>
		  <td>[https://metrics.ng.bluemix.net](https://metrics.ng.bluemix.net)</td>
		</tr>
		<tr>
		  <td>United Kingdom</td>
		  <td>[https://metrics.eu-gb.bluemix.net](https://metrics.eu-gb.bluemix.net)</td>
		</tr>
	    </table>

	
For example, in the US South region, you can run the following command to send two metrics, `myhost.cpu.idle` and `myapp.login.retries`, to a space into the {{site.data.keyword.monitoringshort}} service:
	
```
curl -XPOST @metric.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/metrics
```
{: screen}
	
The sample file *metric.json* is defined as follows:

```
[
  {
    "name" : "myhost.cpu.idle",
    "value" : 22.37
  },
  {
    "name": "myapp.login.retries",
    "value": 4
  }
]
```
{: screen}


















 