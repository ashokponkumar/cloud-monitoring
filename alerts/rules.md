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


# CRUD rules
{: #rules}

Use the Alerts API to create, delete, or update a rule, to show the details for a rule, and to list the rules that are defined in a space.
{:shortdesc}


## Creating a rule
{: #create}

To configure a rule in the {{site.data.keyword.monitoringshort}} service, create a rule file that includes the rule details, and register the rule with the {{site.data.keyword.monitoringshort}} service.

Complete the following steps:

1. Create a rule file that contains valid JSON. Save the file. 

    For example, to save the file, use a prefix like *s-* for rules that you define for the queries running in a space.
	
	**Tips:** 
	
	* Define different rules for a query to alert about errors or warnings by using a different notification method. 
	* Create your rule file in the following directory: *~/cloud-monitoring/rules/* to group resources of the {{site.data.keyword.monitoringshort_notm}} service. 

    Enter the following information in the rule's file:
	
	* *name*: Enter a unique name. The value of this field is used later to update, delete, and show details of the rule.
	* *description*: Enter a description.
	* *expression*: Enter the query that you want to monitor.
	* *enabled*: Set to *true* to enable the rule.
	* *from*: Initial point in time that is used to analyze the data based on the threshold values.
	* *until*: Final point in time that is used to analyze the data based on the threshold values.
	* *comparison*: Comparison operation that is used to identify what type of check to make, for example, *below*.
	* *error_level*: Defines the threshold that you set to trigger an error alert.
	* *warning_level*: Defines the threshold that you set to trigger a warning alert.
	* *frequency*: Defines how often you check the data.
	* *dashboard_url*: Defines a URL that shows in Grafana a dashboard with the query.
	* *notifications*: The method of notification in case the alert described with this rule is triggered.
	
	For example, the rule file myrulefile.json looks as follows:
	
	```
	{
    "name": "checkbytesin",
    "description": "MH check Bytes In per second",
    "expression": "movingAverage(messagehub.65qser11-8034-1234-5678-c82fb42wdfgh.1.kafka-java-console-sample-topic.BytesInPerSec.15MinuteRate,\"5min\")",
    "enabled": true,
    "from": "-5min",
    "until": "now",
    "comparison": "above",
    "comparison_scope": "last",
    "error_level" : 27,
    "warning_level" : 25,
    "frequency": "1min",
    "dashboard_url": "https://metrics.ng.bluemix.net",
    "notifications": [
      "emailXXX"
    ]
    }
    ```
	{: screen}
	
	Then, export the variable *RULE_FILE*:
	
	```
	export RULE_FILE="myrulefile.json"
	```
	{: screen}
	
2. Log in to a region, organization, and space in the {{site.data.keyword.Bluemix_notm}}.

    For more information, see [How do I log in to the {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#login).

3. Get the security token. You can use a UAA token, an IAM token, or an API key. Choose one of the following methods to obtain the security token:
	
	* To get a UAA token, see [Getting the UAA token by using the {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli.
	
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
	
4. Get the space GUID. The GUID must be prefixed with *s-* to identify a space.

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
	
5. Run the following cURL command to register the rule in the {{site.data.keyword.monitoringshort}} service:

    ```
	curl -XPOST -d @$RULE_FILE --header "X-Auth-User-Token: Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" METRICS_ENDPOINT/v1/alert/rule
	```
	{: codeblock}
	
	where
	
	* RULE_FILE is the JSON file that defines your alert rule.
	
	* The *X-Auth-User-Token* is a parameter that passes the {{site.data.keyword.Bluemix_notm}} UAA token, IAM token, or API key.
	
	* *Auth_Type* is the prefix that defines the type of token or API key. The following list outlines the valid values: *apikey*, *iam* or *uaa*, where

        * *apikey* identifies that the token is an API key.
		* *iam* identifies that the token specified is an IAM generated token.
		* *uaa* identifies that the token specified is an UAA generated token.
	
	* *X-Auth-Scope-Id* is a parameter that passes the space GUID. The GUID must be prefixed with *s-* to identify a space. This header is required if you use an UAA token authentication. If you use an IAM authentication token, then this header is optional, and the domain information that is bound to the token is used.
	
	* Token is the UAA or IAM authentication token, or the API key.
	
	* Space is the GUID of the space. 
	
	* METRICS_ENDPOINT represents the entry point to the service. Each region has a different URL. To get the list of endpoints per region, see [Endpoints](/docs/services/cloud-monitoring/send_retrieve_metrics_ov.html#endpoints).
	
    For example, 	
	
	```
	curl -XPOST -d @$RULE_FILE --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}

## Deleting a rule
{: #delete}

To delete a rule, complete the following steps:

1. Log in to a region, organization, and space in the {{site.data.keyword.Bluemix_notm}}. 

    For more information, see [How do I log in to the {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#login).

2. Get the security token. You can use a UAA token, an IAM token, or an API key. Choose one of the following methods to obtain the security token:
	
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
		
4. Run the following cURL command to delete a rule:

    ```
	curl -XDELETE --header "X-Auth-User-Token: Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" METRICS_ENDPOINT/v1/alert/rule/Rule_Name
	```
	{: codeblock}
	
	where
	
    * The *X-Auth-User-Token* is a parameter that passes the UAA token, IAM token, or API key.
	
	* *Auth_Type* is the prefix that defines the type of token or API key. The following list outlines the valid values: *apikey*, *iam* or *uaa*, where

        * *apikey* identifies that the token is an API key.
		* *iam* identifies that the token specified is an IAM generated token.
		* *uaa* identifies that the token specified is an UAA generated token.
	
	* *X-Auth-Scope-Id* is a parameter that passes the space GUID. The GUID must be prefixed with *s-* to identify a space. This header is required if you use an UAA token authentication. If you use an IAM authentication token, then this header is optional, and the domain information that is bound to the token is used.
	
	* Token is the UAA or IAM authentication token, or the API key.
	
	* Space is the GUID of the space. 
	
	* Rule_Name is the name of the rule as specified in the field *name*.
	
	* METRICS_ENDPOINT represents the entry point to the service. Each region has a different URL. To get the list of endpoints per region, see [Endpoints](/docs/services/cloud-monitoring/send_retrieve_metrics_ov.html#endpoints).
	
    
	
## Listing all rules
{: #list}

To list all rules, complete the following steps:

1. Log in to a region, organization, and space in the {{site.data.keyword.Bluemix_notm}}. 

    For more information, see [How do I log in to the {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#login).

2. Get the security token. You can use a UAA token, an IAM token, or an API key. Choose one of the following methods to obtain the security token:
	
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
	
4. Run the following cURL command to list all the rules in a space:

    ```
	curl -XGET --header "X-Auth-User-Token: Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" METRICS_ENDPOINT/v1/alert/rules
	```
	{: codeblock}
	
	where
	
	* The *X-Auth-User-Token* is a parameter that passes the UAA token, IAM token, or API key.
	
	* *Auth_Type* is the prefix that defines the type of token or API key. The following list outlines the valid values: *apikey*, *iam* or *uaa*, where

        * *apikey* identifies that the token is an API key.
		* *iam* identifies that the token specified is an IAM generated token.
		* *uaa* identifies that the token specified is an UAA generated token.
	
	* *X-Auth-Scope-Id* is a parameter that passes the space GUID. The GUID must be prefixed with *s-* to identify a space. This header is required if you use an UAA token authentication. If you use an IAM authentication token, then this header is optional, and the domain information that is bound to the token is used.
	
	* Token is the UAA or IAM authentication token, or the API key.
	
	* Space is the GUID of the space. 
	
	* METRICS_ENDPOINT represents the entry point to the service. Each region has a different URL. To get the list of endpoints per region, see [Endpoints](/docs/services/cloud-monitoring/send_retrieve_metrics_ov.html#endpoints).
	

	
	

## Showing the details of a rule
{: show}

To show the details of a rule, complete the following steps:

1. Log in to a region, organization, and space in the {{site.data.keyword.Bluemix_notm}}. 

    For more information, see [How do I log in to the {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#login).

2. Get the security token. You can use a UAA token, an IAM token, or an API key. Choose one of the following methods to obtain the security token:
	
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
	
4. Run the following cURL command to show the details of a rule:

    ```
	curl -XGET --header "X-Auth-User-Token: Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" METRICS_ENDPOINT/v1/alert/rule/Rule_Name
	```
	{: codeblock}
	
	where
	
	* The *X-Auth-User-Token* is a parameter that passes the UAA token, IAM token, or API key.
	
	* *Auth_Type* is the prefix that defines the type of token or API key. The following list outlines the valid values: *apikey*, *iam* or *uaa*, where

        * *apikey* identifies that the token is an API key.
		* *iam* identifies that the token specified is an IAM generated token.
		* *uaa* identifies that the token specified is an UAA generated token.
	
	* *X-Auth-Scope-Id* is a parameter that passes the space GUID. The GUID must be prefixed with *s-* to identify a space. This header is required if you use an UAA token authentication. If you use an IAM authentication token, then this header is optional, and the domain information that is bound to the token is used.
	
	* Token is the UAA or IAM authentication token, or the API key.
	
	* Space is the GUID of the space. 
	
	* Rule_Name is the name of the rule as specified in the field *name*.
	
	* METRICS_ENDPOINT represents the entry point to the service. Each region has a different URL. To get the list of endpoints per region, see [Endpoints](/docs/services/cloud-monitoring/send_retrieve_metrics_ov.html#endpoints).
	

## Updating a rule
{: #update}

To update a rule, modify the rule by updating the rule file, then complete the following steps:

1. Log in to a region, organization, and space in the {{site.data.keyword.Bluemix_notm}}. 

    For more information, see [How do I log in to the {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#login).

2. Get the security token. You can use a UAA token, an IAM token, or an API key. Choose one of the following methods to obtain the security token:
	
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
	
4. Run the following cURL command to update a rule:

    ```
	curl -XPUT `-d @$RULE_FILE` --header "X-Auth-User-Token: Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" METRICS_ENDPOINT/v1/alert/rule
	```
	{: codeblock}
	
	where
	
	* RULE_FILE is the JSON file that defines your alert rule.
	
	* The *X-Auth-User-Token* is a parameter that passes the UAA token, IAM token, or API key.
	
	* *Auth_Type* is the prefix that defines the type of token or API key. The following list outlines the valid values: *apikey*, *iam* or *uaa*, where

        * *apikey* identifies that the token is an API key.
		* *iam* identifies that the token specified is an IAM generated token.
		* *uaa* identifies that the token specified is an UAA generated token.
	
	* *X-Auth-Scope-Id* is a parameter that passes the space GUID. The GUID must be prefixed with *s-* to identify a space. This header is required if you use an UAA token authentication. If you use an IAM authentication token, then this header is optional, and the domain information that is bound to the token is used.
	
	* Token is the UAA or IAM authentication token, or the API key.
	
	* Space is the GUID of the space. 
	
	* METRICS_ENDPOINT represents the entry point to the service. Each region has a different URL. To get the list of endpoints per region, see [Endpoints](/docs/services/cloud-monitoring/send_retrieve_metrics_ov.html#endpoints).

	
	
