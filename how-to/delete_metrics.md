---

copyright:
  years: 2017, 2018

lastupdated: "2018-04-27"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}



# Deleting metrics
{: #delete_metrics}

You can delete metrics from the {{site.data.keyword.monitoringshort}} service by using [the Metrics API](https://console.bluemix.net/apidocs/927-ibm-cloud-monitoring-rest-api?&language=node#introduction){: new_window}.
{:shortdesc}

After you [log in to a region in the {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#login), complete the following steps to delete a notification:


## Step 1: Get the security token
{: step1}

You can use a UAA token, an IAM token, or an API key. 

Choose one of the following methods to obtain the security token:
	
* To get a UAA token, see [Getting the UAA token by using the {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli).
* To get an IAM token, see [Getting the IAM token by using the {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/security/auth_iam.html#auth_iam).
* To get an API key, see [getting an API key](/docs/services/cloud-monitoring/security/auth_api_key.html#auth_api_key).
	
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
	
Then, export the variable *Token*:
	
```
export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
```
{: screen}
	
**Note:** The token excludes *Bearer*.
	
## Step 2: Verify the permissions to work with metrics 
{: step2}

Depending on the domain where the metrics are available, consider the following information:

* To work with metrics that are available in a space domain, the user needs *developer* role in the Cloud Foundry space associated with the domain. 
* To work with metrics that are available in the account domain, the user needs an IAM policy with *administrator* role, *operator* role, or *editor* role.

To verify your user permissions, complete the following steps:

1. Log in to the {{site.data.keyword.Bluemix_notm}} console.

    Open a web browser and launch the {{site.data.keyword.Bluemix_notm}} dashboard: [http://console.bluemix.net ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://bluemix.net){:new_window}
	
	After you log in with your user ID and password, the {{site.data.keyword.Bluemix_notm}} UI opens.

2. From the menu bar, click **Manage > Account > Users**. 

    The *Users* window displays a list of users with their email addresses for the currently selected account.
	
3. Verify the access permissions to work with the {{site.data.keyword.monitoringshort}} service.


## Step 3: Get the space ID 
{: step3}

This step only applies when you want to delete metrics that are available in a space domain.

To get the space GUID, run the following command:
	
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
	
Then, export the variable *Space*. The GUID must be prefixed with *s-* to identify a space.
	
```
export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
```
{: screen}

	

## Step 4: List the metrics
{: step4}


Run the following cURL command to list the metrics that are available in a space domain:

```
curl -H "X-Auth-Scope-Id: ${Space}" -H “X-Auth-User-Token: Auth_Type ${TOKEN}" -XGET METRICS_ENDPOINT/v1/metrics/list?query=*

```
{: screen}

Run the following cURL command to list the metrics that are available in the account domain:

```
curl -H "X-Auth-User-Token: apikey ${TOKEN}" -XGET METRICS_ENDPOINT/v1/metrics/list?query=*
```
{: screen}

where
	
* The *X-Auth-User-Token* is a parameter that passes the {{site.data.keyword.Bluemix_notm}} UAA token, IAM token, or API key.
	
* *Auth_Type* is the prefix that defines the type of token or API key. The following list outlines the valid values: *apikey*, *iam* or *uaa*, where

  * *apikey* identifies that the token is an API key.
	* *iam* identifies that the token specified is an IAM generated token.
	* *uaa* identifies that the token specified is an UAA generated token.
	
* *X-Auth-Scope-Id* is a parameter that passes the space GUID. The GUID must be prefixed with *s-* to identify a space. 
	
* Token is the UAA or IAM authentication token, or the API key.
	
* Space is the GUID of the space. 
	
* METRICS_ENDPOINT represents the entry point to the service. Each region has a different URL. To get the list of endpoints per region, see [Endpoints](/docs/services/cloud-monitoring/send_retrieve_metrics_ov.html#endpoints).

* *query* defines the filter that is applied. For example, `query=metric-service.*` lists all metrics that exist under the hierarchy `metric-service.*`, and `query=*` lists all metrics in the domain.


## Step 5 - Option 1: Delete all metrics
{: step5}
  

Run the following cURL command to delete all metrics in a space domain:

```
curl -XDELETE --header "X-Auth-User-Token: Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" METRICS_ENDPOINT/v1/metrics/list?query=*
```
{: screen}

Run the following cURL command to delete all metrics from the account domain:

```
curl -H "X-Auth-User-Token: Auth_Type ${Token}" -XDELETE  METRICS_ENDPOINT/v1/metrics/list?query=*
```
{: screen}

where
	
* The *X-Auth-User-Token* is a parameter that passes the {{site.data.keyword.Bluemix_notm}} UAA token.
	
* Auth_Type is the prefix that defines the type of token. Valid values are: *uaa* for a UAA token, *iam* for an IAM token, and *api* for an API key.
	
* *X-Auth-Scope-Id* is a parameter that passes the space GUID. The GUID must be prefixed with *s-* to identify a space.
	
* *Token* represents the security token.
	
* *Space* represents the GUID of the space. 
	
* *METRICS_ENDPOINT* represents the entry point to the service. Each region has a different URL. To get the list of endpoints per region, see [Endpoints](/docs/services/cloud-monitoring/send_retrieve_metrics_ov.html#endpoints).

* *query* defines the filter that is applied. `query=*` indicates all metrics in the domain.


## Step 5 - Option 2: Delete all metrics for a service
{: step6}
  

Run the following cURL command to delete all metrics for a service in a space domain:

```
curl -XDELETE --header "X-Auth-User-Token: Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" METRICS_ENDPOINT/v1/metrics/list?query=metric-service.*
```
{: screen}

Run the following cURL command to delete all metrics from the account domain:

```
curl -H "X-Auth-User-Token: Auth_Type ${Token}" -XDELETE  METRICS_ENDPOINT/v1/metrics/list?query=metric-service.*
```
{: screen}

where
	
* The *X-Auth-User-Token* is a parameter that passes the {{site.data.keyword.Bluemix_notm}} UAA token.
	
* Auth_Type is the prefix that defines the type of token. Valid values are: *uaa* for a UAA token, *iam* for an IAM token, and *api* for an API key.
	
* *X-Auth-Scope-Id* is a parameter that passes the space GUID. The GUID must be prefixed with *s-* to identify a space.
	
* *Token* represents the security token.
	
* *Space* represents the GUID of the space. 
	
* *METRICS_ENDPOINT* represents the entry point to the service. Each region has a different URL. To get the list of endpoints per region, see [Endpoints](/docs/services/cloud-monitoring/send_retrieve_metrics_ov.html#endpoints).

* *query* defines the filter that is applied. `query=*` indicates all metrics in the domain.

* *metric-service.** is the name of a service. For example, `containers-kubernetes`.