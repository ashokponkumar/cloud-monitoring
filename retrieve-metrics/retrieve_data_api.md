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


# Retrieving metrics
{: #retrieve_data_api}

You can retrieve metrics from the {{site.data.keyword.monitoringshort}} service by using [the Metrics API](https://console.bluemix.net/apidocs/927-ibm-cloud-monitoring-rest-api?&language=node#introduction){: new_window}.
{:shortdesc}


## Retrieving metrics from a space
{: #uaa}

To retrieve metrics from a space, complete the following steps:

1. Log in to a region, organization, and space in the {{site.data.keyword.Bluemix_notm}}. 

    For more information, see [How do I log in to the {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#login).

2. Set the security token or API key
  
    You must set the field **X-Auth-User-Token** with a security token, or API key. You can use a UAA token, an IAM token, or an API key.

    First, choose one of the following methods to obtain the security token that you need to send metrics:
	
    * To get a UAA token, see [Getting the UAA token by using the {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli).
    
	* To get an IAM token, see [Getting the IAM token by using the {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/security/auth_iam.html#auth_iam).
    
	* To get an API key, see [Getting an API key](/docs/services/cloud-monitoring/security/auth_api_key.html#auth_api_key). 

    A token or API key must be prefixed with one of the following values: `apikey`, `iam`, or `uaa` 

    where 

    * *apikey* identifies that the token is an API key.
    * *iam*  identifies that the token specified is an IAM generated token.
    * *uaa* identifies that the token specified is a UAA generated token.

    For example, you can set the following header for an API key: `X-Auth-User-Token: apikey SomeIAMGeneratedKey`
	
	Then, from the same terminal where you logged in to the {{site.data.keyword.Bluemix_notm}}, set the Token variable for the token.

    For example, set a UAA token, and remove the *Bearer* part from the token value:

    ```
    export Token="kjshdgf.....ldkdjdj"
    ```
    {: screen}
		
3. Get the Space ID.

    To retrieve metrics, the header field **X-Auth-Scope-Id** is required, and must be set to the space GUID. 

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
	curl -XGET --header "X-Auth-User-Token: Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" METRICS_ENDPOINT/v1/metrics?from=Start_Time&until=End_Time&target=string
	```
	{: codeblock}

	where
	
	* The *X-Auth-User-Token* is a parameter that passes the {{site.data.keyword.Bluemix_notm}} UAA token.
	
	* Auth_Type is the prefix that defines the type of token. Valid values are: *uaa* for a UAA token, *iam* for an IAM token, and *api* for an API key.
	
	* *X-Auth-Scope-Id* is a parameter that passes the space GUID. The GUID must be prefixed with *s-* to identify a space. This header is only required if you use UAA authentication. If you use an IAM authentication token, then this header is optional, and the domain information that is bound to the token is used.
	
	* *Token* represents the security token.
	
	* *Space* represents the GUID of the space. 
	
	* *Start_Time* defines the relative or absolute time period that sets the start of the request. Defaults to 24h ago: `-24h`. *End_Time* defines the elative or absolute time period to set the end of the request. Defaults to current time now: `now`. For more information, see [Setting a period of time](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#time).
	
	* *Path*  identifies one or several metrics. You can optionally apply functions to each metric. Paths also support wildcards, which allows you to identify more than one metric in a single path. For more information, see [Defining the metrics](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#metrics).
	
	* *METRICS_ENDPOINT* represents the entry point to the service. Each region has a different URL. To get the list of endpoints per region, see [Endpoints](/docs/services/cloud-monitoring/send_retrieve_metrics_ov.html#endpoints).
	

	
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


