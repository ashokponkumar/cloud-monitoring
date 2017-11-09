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


# Configuring an alert that sends an email
{: #configure_email_alert}

To configure an email alert on a metric, define a rule and an email notification. Then, register the rule and the notification method with the {{site.data.keyword.monitoringshort}} service.
{:shortdesc}

Complete the following steps:

## Step 1: Create a rule
{: #step1}

Create a rule for a metric query that you have defined in Grafana. For more information, see [Creating a rule](/docs/services/cloud-monitoring/alerts/rules.html#create).

For example, create the rule file `s-rule-1.json` into the `~/cloud-monitoring/rules/` directory. 

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
"NOTIFICATION_NAME"
]
}
```
{: screen}

Then, export the variable *RULE_FILE*:
	
```
export RULE_FILE="s-rule-1.json"
```
{: screen}

## Step 2: Register the rule in the Monitoring service
{: #step2}
	
From a terminal, complete the following steps:

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
	
4. Run the following cURL command to register a rule:
	
    ```
	curl -XPOST -d @$RULE_FILE --header "X-Auth-User-Token: Auth_Type ${Token}" METRICS_ENDPOINT/v1/alert/rule
	```
	{: screen}
	
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
	
	To verify that the rule is created successfully, run the following command:
	
	```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" METRICS_ENDPOINT/v1/alert/rule/checkbytesin
	```
	{: codeblock}
	
	For example,

    ```
	curl -XGET --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule/checkbytesin
	```
	{: screen}

## Step 3: Create an email notification
{: #step3}


To create a notification file that sends an email, consider the following information:

* Use the notification template for sending emails. For more information, see [Notification templates](/docs/services/cloud-monitoring/config_alerts_ov.html#notification_template).
* Create the file in a local directory, for example, `~/cloud-monitoring/notifications`.

For example, create the file **email.json** by using the vi editor: 
	
```
{
"name": "NOTIFICATION_NAME",
"type": "Email",
"description" : "Send email to manager of department X when ....",
"detail": "Enter email address, for example, xxx@yyy"
}
```
{: codeblock}	
	
For more information, see [Creating a notification](/docs/services/cloud-monitoring/alerts/notifications.html#create).
	
## Step 4: Register the notification method in the Monitoring service
{: #step4}
	
From a terminal, complete the following steps:

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
	
4. Run the following cURL command to register a notification:

    ```
	curl -XPOST -d @$NOTIFICATION_FILE --header "X-Auth-User-Token: Auth_Type ${Token}" METRICS_ENDPOINT/v1/alert/notification
	```
	{: screen}
	
	where
	
	* NOTIFICATION_FILE is the JSON file that defines your notification.
	
	* The *X-Auth-User-Token* is a parameter that passes the {{site.data.keyword.Bluemix_notm}} UAA token, IAM token, or API key.
	
	* *Auth_Type* is the prefix that defines the type of token or API key. The following list outlines the valid values: *apikey*, *iam* or *uaa*, where

        * *apikey* identifies that the token is an API key.
		* *iam* identifies that the token specified is an IAM generated token.
		* *uaa* identifies that the token specified is an UAA generated token.
	
	* *X-Auth-Scope-Id* is a parameter that passes the space GUID. The GUID must be prefixed with *s-* to identify a space. This header is required if you use an UAA token authentication. If you use an IAM authentication token, then this header is optional, and the domain information that is bound to the token is used.
	
	* Token is the UAA or IAM authentication token, or the API key.
	
	* Space is the GUID of the space. 
	
	* METRICS_ENDPOINT represents the entry point to the service. Each region has a different URL. To get the list of endpoints per region, see [Endpoints](/docs/services/cloud-monitoring/send_retrieve_metrics_ov.html#endpoints).
	
    To verify that the notification is created successfully, run the following command:

	```
	curl -XGET --header "X-Auth-User-Token: Auth_Type ${Token}" METRICS_ENDPOINT/v1/alert/notification/NOTIFICATION_NAME
	```
	{: codeblock}
	
	For example:
	
    ```
	curl -XGET --header "X-Auth-User-Token: Auth_Type ${Token}" https://metrics.ng.bluemix.net/v1/alert/notification/NOTIFICATION_NAME
	```
	{: screen}
	


    
   
  


 
	  