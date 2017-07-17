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



# Retrieving the history of a rule
{: #retrieve_history}


To retrieve the history of an alert, complete the following steps:

1. Log in to a {{site.data.keyword.Bluemix_notm}} region, organization, and space. Run the command:

    For example, to log in to the US South region, run the following command:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Follow the instructions. Enter your {{site.data.keyword.Bluemix_notm}} credentials, select an organization and a space.

2. Get the authentication token or API key.

    * For IAM authentication, see [Getting the IAM token by using the Bluemix CLI](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) or [Generating an IAM API key by using the Bluemix CLI](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	                                          
	* For UAA authentication, see [Getting the UAA token by using the Bluemix CLI](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) or [Getting the UAA token by using the REST API](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

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
	
	Then, export the variable *Space*:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. Run the following cURL command to get the history of an alert:

    ```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/history?rule=Rule_Name
	```
	{: codeblock}
	
	where
	
	* The *X-Auth-User-Token* is a parameter that passes the {{site.data.keyword.Bluemix_notm}} UAA token, IAM token, or API key.
	
	* *Auth_Type* is the prefix that defines the type of token or API key. The following list outlines the valid values: *apikey*, *iam* or *uaa*, where

        * *apikey* identifies that the token is an API key.
		* *iam* identifies that the token specified is an IAM generated token.
		* *uaa* identifies that the token specified is an UAA generated token.
	
	* *X-Auth-Scope-Id* is a parameter that passes the space GUID. The GUID must be prefixed with *s-* to identify a space. This header is required if you use an UAA token authentication. If you use an IAM authentication token, then this header is optional, and the domain information that is bound to the token is used.
	
	* Token is the UAA or IAM authentication token, or the API key.
	
	* Space is the GUID of the space. It is only required when you use a UAA token.
	
	* Rule_Name is the name of the rule that is used to trigger the alert. The value is the one that is specified in the field *name*.
	
    
For example, the history of rule `highNginxCPU` is the following:

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


