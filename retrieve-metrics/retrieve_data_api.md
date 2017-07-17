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


# Retrieving data from the Monitoring service
{: #retrieve_data_api}

You can retrieve metrics from the {{site.data.keyword.monitoringshort}} service by using [the Metrics API](https://console.bluemix.net/apidocs/927-ibm-cloud-monitoring-rest-api?&language=node#introduction){: new_window}. You can retrieve metrics from a {{site.data.keyword.Bluemix_notm}} space.
{:shortdesc}


To define the set of data that you plan to retrieve, consider the following information: 

* You must set the {{site.data.keyword.Bluemix_notm}} space from where you want to retrieve the data.

* You must provide a token or API key to work with the {{site.data.keyword.monitoringshort}} service. 

* You must specify a path to 1 or more metrics. For more information, see [Defining the metrics](#metrics).

* Optionally, you can specify a custom time period. By default, if you do not specify a time period, the data that you retrieve is the data that corresponds to the last 24 hours. For more information, see [Configuring a period of time](#time).

**Note:** You can retrieve a maximum of 5 targets per request.


## Defining the metrics
{: #metrics}

To retrieve data for a metric, you must specify the path of the metric from the root of the metrics tree to the metric, and you must separate each step with a period (.).

The path is specified in the **Target** parameter. A maximum of 5 targets can be specified per request.  

You can optionally apply functions to the metric. For more information about supported functions, see [Graphite 0.9.15 functions ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://graphite.readthedocs.io/en/latest/functions.html).

You can use wildcards to define a path. By using wildcards, you can identify more than one metric in a single path. 

* **Asterisk**: You can use an asterisk (*) to match zero or more characters. 

    You can have more than one asterisk within a single path element, for example: `servers.sp*aeg*r.cpu.total.*` This example returns data about the total CPU for all servers that match the pattern.

* **Character list or range**: You can define characters in brackets ([...]) to specify a single character position in the path string. 

    You can set a character inclusive range by specifying two characters that are separated by a hyphen (-). Any character between those two characters will match. 
	
	You can define one or more character ranges within the brackets. 
	
	When the characters inside a set of brackets cannot be read as a range, the characters are treated as a list of values. 
	
	You can include a hyphen (-) as a character in a list of values by putting it at the beginning or end inside the brackets.

* **Value list**: You can set comma-separated values within braces to define value lists. 

    For example, to download metrics for the following paths: `servers.myserver.cpu.total.user` and `servers.myserver.cpu.total.system}`, you can set a single path for both types: `servers.myserver.cpu.total.{user,system}`



## Setting a period of time
{: #time}

You can optionally specify a period of time by using a relative time or an absolute time. You must define the query parameters **From** and **Until**.  

* When the start time of the period of time is omitted, the default value is 24 hours ago. 

* When the end time of the period of time is omitted, the default value is the current time *now*.

The **Relative time** is always preceded by a minus sign (-), and followed by a unit of time. 

The following table lists the different relative time abbreviations that you can use:


| Abbreviation | Description |
|--------------|-------------|
| s            | Seconds     |
| min          | Minutes     |
| h            | Hours       |
| d            | Days        |
| w            | Weeks       |
| mon          | Months      |
| now          | Current Time |


You can use any of the following formats to define an **Absolute time**: `HH:MM_YYMMDD`, `YYYYMMDD`, `MM/DD/YY`

| Abbreviation | Description |
|--------------|-------------|
| YYYY         | Four digit year |
| MM           | Two month digit (01=Jan etc) |
| DD           | Two-digit day of month (01 through 31) |
| hh           | Two digits of hour (00 through 23) |
| mm           | Two digits of minute (00 through 59) |
| ss           | Two digits of second (00 through 59) |

**Example**

The following table shows different examples on how to set different time periods by seting the *from* and *until* parameters:

<table>
  <caption>Table 1. Different examples that show how to set different time periods by seting the *from* and *until* parameters</caption>
  <tr>
    <th>Example</th>
	<th>Description</th>
  </tr>
  <tr>
    <td>&from=monday</td>
	<td>Period of time that includes data from last monday until now.</td>
  </tr>
  <tr>
    <td>&from=20171101&until=20171130</td>
	<td>Period of time set to a month, for example November </td>
  </tr>
  <tr>
    <td>&from=02:00_20170701&until=14:00_20170701</td>
	<td>To show data in a 24 hour cycle, for example, from 02:00 to 14:00 on July 1st, 2017</td>
  </tr>
  <tr>
    <td>&from=-8d&until=-7d</td>
	<td>To show the same day of the week, for example, last week</td>
  </tr>
  
</table>


## Setting the space
{: #domain}

When you use the {{site.data.keyword.Bluemix_notm}} UAA authentication model or the {{site.data.keyword.Bluemix_notm}} IAM authentication model to authenticate with the {{site.data.keyword.monitoringshort}} service, the header field **X-Auth-Scope-Id** is required and must be set to the space GUID. The GUID must be prefixed with *s-* to identify a space.



## Setting the authorization token or API key
{: #security}
  
You must set the field **X-Auth-User-Token** to define the UAA token, IAM token, or API key that is used for authentication. 

A token or API key must be prefixed with one of the following values: `apikey`, `iam`, or `uaa` 

where 

* *apikey* identifies that the token is an API key.
* *iam*  identifies that the token specified is an IAM generated token.
* *uaa* identifies that the token specified is an UAA generated token.

For example, you can set the following header for an API key: `X-Auth-User-Token: apikey SomeIAMGeneratedKey`


## Retrieving metrics from a space by using UAA
{: #uaa}

To retrieve metrics from a {{site.data.keyword.Bluemix_notm}} space, complete the following steps:

1. Log in to a {{site.data.keyword.Bluemix_notm}} region, organization, and space. Run the command:

    For example, to log in to the US South region, run the following comnmand:
	
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
	export Token="eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8"
	```
	{: screen}
	
	**Note:** The token excludes *Bearer*.
		
3. Get the space GUID. The GUID must be prefixed with *s-* to identify a space.

    Run the following command:
	
	```
	cf space SpaceName --guid
	```
	{: codeblock}
	
	Where *SpaceName* is the name of the space prefixed with *s-* where you are going to define a notification.
	
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
	curl -XGET --header "X-Auth-User-Token:uaa ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics?from=Start_Time&until=End_Time&target=string
	```
	{: codeblock}

	where
	
	* The *X-Auth-User-Token* is a parameter that passes the {{site.data.keyword.Bluemix_notm}} UAA token.
	
	* *uaa* is the prefix that indicates that the token specified is a UAA generated token.
	
	* *X-Auth-Scope-Id* is a parameter that passes the space GUID. The GUID must be prefixed with *s-* to identify a space. This header is only required if you use UAA authentication. If you use an IAM authentication token, then this header is optional, and the domain information that is bound to the token is used.
	
	* *Token* represents the UAA token.
	
	* *Space* represents the GUID of the space. It is only required when you use a UAA token.
	
	* *Start_Time* defines the relative or absolute time period that sets the start of the request. Defaults to 24h ago: `-24h`. *End_Time* defines the elative or absolute time period to set the end of the request. Defaults to current time now: `now`. For more information, see [Setting a period of time](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#time).
	
	* *Path*  identifies one or several metrics. You can optionally apply functions to each metric. Paths also support wildcards, which allows you to identify more than one metric in a single path. For more information, see [Defining the metrics](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#metrics).
	
	
	
## Retrieving metrics from a space by using IAM or an API key
{: #iam}

To retriece metrics from a {{site.data.keyword.Bluemix_notm}} space by using IAM or an API key, complete the following steps:

1. Log in to a {{site.data.keyword.Bluemix_notm}} region, organization, and space. Run the command:

    For example, to log in to the US South region, run the following comnmand:
	
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
	
	Where *SpaceName* is the name of the space prefixed with *s-* where you are going to define a notification.
	
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
	
5. Run a cURL command to retrive metrics.

   	```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics?from=Start_Time&until=End_Time&target=string
	```
	{: codeblock}

	where
	
	* The *X-Auth-User-Token* is a parameter that passes the {{site.data.keyword.Bluemix_notm}} AIM token or API key.
	
	* *Auth_Type* is the prefix that defines the type of token or API key. 

        * *apikey* identifies that the token is an API key.
		* *iam* identifies that the token specified is an IAM generated token.
	
	* *X-Auth-Scope-Id* is a parameter that passes the space GUID. The GUID must be prefixed with *s-* to identify a space. This header is only required if you use UAA authentication. If you use an IAM authentication token, then this header is optional, and the domain information that is bound to the token is used.
	
	* *Token* represents the UAA token.
	
	* *Space* represents the GUID of the space. It is only required when you use a UAA token.
	
	* *Start_Time* defines the relative or absolute time period that sets the start of the request. Defaults to 24h ago: `-24h`. *End_Time* defines the elative or absolute time period to set the end of the request. Defaults to current time now: `now`. For more information, see [Setting a period of time](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#time).
	
	* *Path*  identifies one or several metrics. You can optionally apply functions to each metric. Paths also support wildcards, which allows you to identify more than one metric in a single path. For more information, see [Defining the metrics](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#metrics).





