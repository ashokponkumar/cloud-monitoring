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



# Deleting alerts
{: #delete_alerts}

You can delete alerts from the {{site.data.keyword.monitoringshort}} service by using [the Alerts API](https://console.bluemix.net/apidocs/940-ibm-cloud-monitoring-alerts-api?&language=node#introduction){: new_window}.
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
	

## Step 2: Get the space ID 
{: step2}

This step only applies when you want to delete alerts that are available in a space domain.

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

	

## Step 3: List the rules
{: step3}


Run the following cURL command to list the rules that are defined in a space domain:

```
curl -H "X-Auth-Scope-Id: ${Space}" -H “X-Auth-User-Token: Auth_Type ${TOKEN}" -XGET METRICS_ENDPOINT/v1/alert/rules

```
{: screen}

Run the following cURL command to list the notifications in the account domain:

```
curl -H "X-Auth-User-Token: apikey ${TOKEN}" -XGET METRICS_ENDPOINT/v1/alert/rules
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


## Step 4: Delete an alert rule
{: step4}
  

Run the following cURL command to delete a rule in a space domain:

```
curl -H "X-Auth-Scope-Id: ${Space}" -H “X-Auth-User-Token: Auth_Type ${TOKEN}" -XDELETE METRICS_ENDPOINT/v1/alert/rule/{name} 
```
{: screen}

Run the following cURL command to delete a rule from the account domain:

```
curl -H "X-Auth-User-Token: apikey ${TOKEN}" -XDELETE METRICS_ENDPOINT/v1/alert/rule/{name} 
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

* *name* is the name of the rule that you want to delete.
	
