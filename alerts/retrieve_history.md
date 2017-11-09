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



# Retrieving the history of an alert
{: #retrieve_history}


To retrieve the history of an alert, complete the following steps:

1. Log in to a region, organization, and space in the {{site.data.keyword.Bluemix_notm}}. 

    For more information, see [How do I log in to the {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#login).

2. Get the security token. You can use a UAA token, an IAM token, or an API key. Choose one of the following methods to obtain the security token:
	
	* To get a UAA token, see [Getting the UAA token by using the {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli).
	
	* To get an IAM token, see [Getting the IAM token by using the {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/security/auth_iam.html#auth_iam).
	
	* To get an API key, see [getting an API key](/docs/services/cloud-monitoring/security/auth_api_key.html#auth_api_key).
	
	From the same terminal where you logged in to {{site.data.keyword.Bluemix_notm}}, set the Token variable for the token.

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
	
4. Retrieve the history of a rule

    * To retrieve the history of a rule by its name, see [Retrieving the history of a rule by using its name](/docs/services/cloud-monitoring/alerts/retrieve_history.html#by_name).
	* To retrieve the history of a rule for a period of time, see [Retrieving the history of a rule for the last 48 hours](/docs/services/cloud-monitoring/alerts/retrieve_history.html#by_time).
	* To retrieve a number of entries from the history of a rule, see [Retrieving the last 3 entries in the history of a rulee](/docs/services/cloud-monitoring/alerts/retrieve_history.html#number).
	
	
## Retrieving the history of a rule by rule name
{: #by_name}
	
Run the following cURL command to get the history of an alert:

```
curl -XGET --header "X-Auth-User-Token: Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" METRICS_ENDPOINT/v1/alert/history?rule_name=RULE_NAME
```
{: codeblock}
	
where
	
* The *X-Auth-User-Token* is a parameter that passes the UAA token, IAM token, or API key.
	
* *Auth_Type* is the prefix that defines the type of token or API key. The following list outlines the valid values: *apikey*, *iam* or *uaa*, where

    * *apikey* identifies that the token is an API key.
	* *iam* identifies that the token specified is an IAM generated token.
	* *uaa* identifies that the token specified is an UAA generated token.
	
* *X-Auth-Scope-Id* is a parameter that passes the space GUID. The GUID must be prefixed with *s-* to identify a space. This header is required if you use an UAA token authentication. If you use an IAM authentication token, then this header is optional, and the domain information that is bound to the token is used.
	
* *Token* is the UAA or IAM authentication token, or the API key.
	
* *Space* is the GUID of the space. 
	
* *RULE_NAME* is the name of the rule that is used to trigger the alert. The value is the one that is specified in the field *name*.
	
* *METRICS_ENDPOINT* represents the entry point to the service. Each region has a different URL. To get the list of endpoints per region, see [Endpoints](/docs/services/cloud-monitoring/send_retrieve_metrics_ov.html#endpoints).
	
    
For example, the history of rule `highNginxCPU` is the following:

```
curl -XGET --header "X-Auth-User-Token: iam ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/history?rule_name=highNginxCPU
```
{: screen}

```
[
 {
 "rule": "highNginxCPU",
 "timestamp": "2017-04-17T22.23.21.000",
 "value": 99.5,
 "from_level": "OK",
 "to_level": "ERROR"
 },
 {
 "rule": "highNginxCPU",
 "timestamp": "2017-04-17T10.11.12.123",
 "value": 98.5,
 "from_level" : "OK",
 "to_level": "WARN"
 },
 {
 "rule": "highNginxCPU",
 "timestamp": "2017-04-17T10.12.14.000",
 "value": 97.0,
 "from_level" : "WARN",
 "to_level": "OK"
 }
]
```
{: screen}


## Retrieving the history of a rule for the last 48 hours
{: #by_time}

Run the following cURL command to get the history of an alert for the last 48 hours:

```
curl -H "X-Auth-User-Token: iam $Token" -H "X-Auth-Scope-Id:$Space" METRICS_ENDPOINT/v1/alert/history?start=-48h
```
{: codeblock}

where
	
* The *X-Auth-User-Token* is a parameter that passes the UAA token, IAM token, or API key.
	
* *Auth_Type* is the prefix that defines the type of token or API key. The following list outlines the valid values: *apikey*, *iam* or *uaa*, where

    * *apikey* identifies that the token is an API key.
	* *iam* identifies that the token specified is an IAM generated token.
	* *uaa* identifies that the token specified is an UAA generated token.
	
* *X-Auth-Scope-Id* is a parameter that passes the space GUID. The GUID must be prefixed with *s-* to identify a space. This header is required if you use an UAA token authentication. If you use an IAM authentication token, then this header is optional, and the domain information that is bound to the token is used.
	
* *Token* is the UAA or IAM authentication token, or the API key.
	
* *Space* is the GUID of the space. 
	
* *RULE_NAME* is the name of the rule that is used to trigger the alert. The value is the one that is specified in the field *name*.
	
* *METRICS_ENDPOINT* represents the entry point to the service. Each region has a different URL. To get the list of endpoints per region, see [Endpoints](/docs/services/cloud-monitoring/send_retrieve_metrics_ov.html#endpoints).
	

## Retrieving the last 3 entries in the history of a rule
{: #number}

Run the following cURL command to get the last 3 entries in the history of an alert:

```
curl -H "X-Auth-User-Token: iam $Token" -H "X-Auth-Scope-Id:$Space" METRICS_ENDPOINT/v1/alert/history?max_results=3
```
{: codeblock}

where
	
* The *X-Auth-User-Token* is a parameter that passes the UAA token, IAM token, or API key.
	
* *Auth_Type* is the prefix that defines the type of token or API key. The following list outlines the valid values: *apikey*, *iam* or *uaa*, where

    * *apikey* identifies that the token is an API key.
	* *iam* identifies that the token specified is an IAM generated token.
	* *uaa* identifies that the token specified is an UAA generated token.
	
* *X-Auth-Scope-Id* is a parameter that passes the space GUID. The GUID must be prefixed with *s-* to identify a space. This header is required if you use an UAA token authentication. If you use an IAM authentication token, then this header is optional, and the domain information that is bound to the token is used.
	
* *Token* is the UAA or IAM authentication token, or the API key.
	
* *Space* is the GUID of the space. 
	
* *RULE_NAME* is the name of the rule that is used to trigger the alert. The value is the one that is specified in the field *name*.
	
* *METRICS_ENDPOINT* represents the entry point to the service. Each region has a different URL. To get the list of endpoints per region, see [Endpoints](/docs/services/cloud-monitoring/send_retrieve_metrics_ov.html#endpoints).
	