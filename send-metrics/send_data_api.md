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


# Sending data by using the Metrics API
{: #send_data_api}

You can send metrics to the {{site.data.keyword.monitoringshort}} service by using the Metrics API. 
{:shortdesc}


For the {{site.data.keyword.Bluemix_notm}} Docker containers, basic system metrics are automatically collected. For Cloud Foundry applications, and apps running in a Virtual Machine (VM), metrics must be sent directly from the app by using [the Metrics API](https://console.bluemix.net/apidocs/927-ibm-cloud-monitoring-rest-api?&language=node#introduction){: new_window}. 



## Sending metrics to a space domain
{: #space_uaa}

To send metrics to a space by using cURL, complete the following steps:

1. Log in to a region, organization, and space in the {{site.data.keyword.Bluemix_notm}}. 

    For more information, see [How do I log in to the {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#login).

2. Get the security token. You can use a UAA token, an IAM token, or an API key.

    Choose one of the following methods to obtain the security token that you need to send metrics:
	
	* To get a UAA token, see [Getting the UAA token by using the {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli);
	
	* To get an IAM token, see [Getting the IAM token by using the {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/security/auth_iam.html#auth_iam).
	
	* To get an API key, see [Getting an API key](/docs/services/cloud-monitoring/security/auth_api_key.html#auth_api_key).
	
	From the same terminal where you logged in to {{site.data.keyword.Bluemix_notm}}, set the *Token* variable for the token.

    For example, set a UAA token, and remove the *Bearer* part from the token value:

    ```
    export Token="kjshdgf.....ldkdjdj"
    ```
    {: screen}
		
3. Get the space GUID. The GUID must be prefixed with *s-* to identify a space.

    Run the following command:
	
	```
	bx iam space SpaceName --guid
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
	
	Then, export the variable *Space*. **Note:** The GUID must be prefixed with *s-* to identify a space.
	
	For example:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. Run the following cURL command to send metrics:

    ```
	curl -XPOST -d @Metric_File --header "X-Auth-User-Token: Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" Endpoint/v1/metrics
	```
	{: codeblock}
	
	where
	
	* The Metrics_File represents the JSON file that includes the definition of metrics that are sent to the {{site.data.keyword.monitoringshort}} service.
	
	* The *X-Auth-User-Token* is a parameter that passes the security token.
	
	* *Auth_Type* is the prefix that defines the type of token. Valid values are: *uaa* for a UAA token, *iam* for an IAM token, and *api* for an API key.
	
	* *X-Auth-Scope-Id* is a parameter that passes the space GUID. The GUID must be prefixed with *s-* to identify a space.
	
	* Token represents the security token or API key.
	
	* Space represents the GUID of the space. 
	
	* Endpoint represents the entry point to the service. Each region has a different URL. To get the list of endpoints per region, see [Endpoints](/docs/services/cloud-monitoring/send_retrieve_metrics_ov.html#endpoints).
	
	For example, you can run the following command to send 2 metrics, `myhost.cpu.idle` and `myapp.login.attempts`, to the {{site.data.keyword.monitoringshort}} service:
	
	```
	curl -XPOST @metric.json --header "X-Auth-User-Token: uaa ${Token}" https://metrics.ng.bluemix.net/v1/metrics
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

 











 