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


# 从 Monitoring 服务检索数据
{: #retrieve_data_api}

可以使用[度量值 API](https://console.bluemix.net/apidocs/927-ibm-cloud-monitoring-rest-api?&language=node#introduction){: new_window} 从 {{site.data.keyword.monitoringshort}} 服务检索度量值。您可以从 {{site.data.keyword.Bluemix_notm}} 空间检索度量值。
{:shortdesc}


要定义计划检索的数据集，请考虑以下信息： 

* 必须设置要从中检索数据的 {{site.data.keyword.Bluemix_notm}} 空间。

* 您必须提供令牌或 API 密钥才能使用 {{site.data.keyword.monitoringshort}} 服务。 

* 必须指定 1 个或多个度量值的路径。有关更多信息，请参阅[定义度量值](#metrics)。

* （可选）您可以指定定制时间段。缺省情况下，如果未指定时间段，那么检索的数据是与过去 24 小时相对应的数据。有关更多信息，请参阅[配置时间段](#time)。

**注：**对于每个请求，最多可以检索 5 个目标。


## 定义度量值
{: #metrics}

要检索度量值的数据，必须将度量值树的根目录中的度量值路径指定为度量值，并且必须使用句点 (.) 来分隔每个步骤。

路径在**目标**参数中指定。每个请求最多可指定 5 个目标。  

可选择将函数应用于度量值。有关受支持函数的更多信息，请参阅[Graphite 0.9.15 函数![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](http://graphite.readthedocs.io/en/latest/functions.html "外部链接图标")。

您可以使用通配符定义路径。通过使用通配符，可以识别单个路径中的多个度量值。 

* **星号**：可以使用星号 (*) 来匹配零个或多个字符。 

    单个路径元素中可以有多个星号，例如：`servers.sp*aeg*r.cpu.total.*`。此示例将返回与该模式匹配的所有服务器的 CPU 总数的相关数据。

* **字符列表或范围**：可以在方括号 ([...]) 中定义字符，以在路径字符串中指定单个字符位置。 

    可以通过指定以连字符 (-) 分隔的两个字符来设置字符包含范围。这两个字符之间的任何字符都将匹配。 
	
	您可以在方括号中定义一个或多个字符范围。 
	
	如果不能将一组方括号中的字符作为范围来读取，那么这些字符将被视为值列表。 
	
	您可以在值列表中包含连字符 (-) 作为字符，方法是将其放在方括号的开头或结尾。

* **值列表**：您可以在花括号中设置以逗号分隔的值以定义值列表。 

    例如，要下载路径 `servers.myserver.cpu.total.user` 和 `servers.myserver.cpu.total.system}` 的度量值，您可以为这两个类型设置单个路径：`servers.myserver.cpu.total.{user,system}`



## 设置时间段
{: #time}

您可以选择使用相对时间或绝对时间来指定时间段。您必须定义查询参数 **From** 和 **Until**。  

* 当省略时间段的开始时间时，缺省值为 24 小时前。 

* 当省略时间段的结束时间时，缺省值为当前时间 *now*。

**相对时间**始终以减号 (-) 开头，后跟一个时间单位。 

下表列出了可以使用的不同相对时间缩写：


| 缩写| 描述|
|--------------|-------------|
| s            | 秒          |
| min          | 分钟        |
| h            | 小时        |
| d            | 天          |
| w            | 周          |
| mon          | 月          |
| now          | 当前时间    |


您可以使用以下任意格式来定义**绝对时间**：`HH:MM_YYMMDD`、`YYYYMMDD` 和 `MM/DD/YY`

| 缩写| 描述|
|--------------|-------------|
| YYYY         | 四位年 |
| MM           | 两位月（01=1 月等）|
| DD           | 两位日期（01 到 31）|
| hh           | 两位小时（00 到 23）|
| mm           | 两位分钟（00 到 59）|
| ss           | 两位秒（00 到 59）|

**示例**

下表显示了有关如何通过设置 *from* 和 *until* 参数来设置不同时间段的不同示例：

<table>
  <caption>表 1. 显示有关如何通过设置 *from* 和 *until* 参数来设置不同时间段的不同示例</caption>
  <tr>
    <th>示例</th>
	<th>描述</th>
  </tr>
  <tr>
    <td>&from=monday</td>
	<td>包含上星期一到现在的数据的时间段。</td>
  </tr>
  <tr>
    <td>&from=20171101&until=20171130</td>
	<td>设置为月份的时间段，例如 11 月</td>
  </tr>
  <tr>
    <td>&from=02:00_20170701&until=14:00_20170701</td>
	<td>以 24 小时为周期显示数据，例如，2017 年 7 月 1 日 02:00 到 14:00</td>
  </tr>
  <tr>
    <td>&from=-8d&until=-7d</td>
	<td>显示一周的同一天，例如上周</td>
  </tr>
  
</table>


## 设置空间
{: #domain}

使用 {{site.data.keyword.Bluemix_notm}} UAA 认证模型或 {{site.data.keyword.Bluemix_notm}} IAM 认证模型向 {{site.data.keyword.monitoringshort}} 服务进行认证时，头字段 **X-Auth-Scope-Id** 是必需的，并且必须设置为空间 GUID。GUID 必须以 *s-* 为前缀以标识空间。



## 设置授权令牌或 API 密钥
{: #security}
  
必须设置字段 **X-Auth-User-Token** 才能定义用于认证的 UAA 令牌、IAM 令牌或 API 密钥。 

令牌或 API 密钥必须以下列其中一个值作为前缀：`apikey`、`iam` 或 `uaa` 

其中 

* *apikey* 表明令牌是 API 密钥。
* *iam* 表明指定的令牌是 IAM 生成的令牌。
* *uaa* 表明指定的令牌是 UAA 生成的令牌。

例如，您可以设置 API 密钥的以下头：`X-Auth-User-Token: apikey SomeIAMGeneratedKey`


## 使用 UAA 从空间检索度量值
{: #uaa}

要从 {{site.data.keyword.Bluemix_notm}} 空间检索度量值，请完成以下步骤：

1. 登录到 {{site.data.keyword.Bluemix_notm}} 区域、组织和空间。运行以下命令：

    例如，要登录到美国南部区域，请运行以下命令：
	
	```
    cf login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    遵循指示信息进行操作。输入您的 {{site.data.keyword.Bluemix_notm}} 凭证，然后选择组织和空间。

2. 获取 UAA 认证令牌。

    有关 UAA 认证的更多信息，请参阅[使用 Bluemix CLI 获取 UAA 令牌](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli)或[使用 REST API 获取 UAA 令牌](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api)。

	例如，要使用 UAA 令牌，请运行以下命令：

    ```
	cf oauth-token
	```
	{: codeblock}
	
	此命令的结果如下所示：
	
	```
	bearer eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8
	```
	{: screen}
	
	然后，导出变量 *Token*。例如：
	
	```
	export Token="eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8"
	```
	{: screen}
	
	**注：**令牌不包含 *Bearer*。
		
3. 获取空间 GUID。该 GUID 必须以 *s-* 为前缀以标识空间。

    运行以下命令：
	
	```
	cf space SpaceName --guid
	```
	{: codeblock}
	
	其中，*SpaceName* 是要定义通知的空间的名称，以 *s-* 作为前缀。
	
	例如，要获取名称为 *dev* 的空间的 GUID，请运行以下命令：
	
	```
	cf space dev --guid
	```
	{: screen}
	
	此命令的结果如下所示：
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	然后，导出变量 *Space*。例如：
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. 运行以下 cURL 命令来发送度量值：

    ```
	curl -XGET --header "X-Auth-User-Token:uaa ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics?from=Start_Time&until=End_Time&target=string
	```
	{: codeblock}

	其中
	
	* *X-Auth-User-Token* 是用于传递 {{site.data.keyword.Bluemix_notm}} UAA 令牌的参数。
	
	* *uaa* 是表明指定的令牌是 UAA 生成的令牌的前缀。
	
	* *X-Auth-Scope-Id* 是用于传递空间 GUID 的参数。该 GUID 必须以 *s-* 为前缀以标识空间。仅当使用 UAA 认证时，此头才是必需的。如果使用 IAM 认证令牌，那么此头是可选的，并且将使用绑定到该令牌的域信息。
	
	* *Token* 表示 UAA 令牌。
	
	* *Space* 表示空间的 GUID。仅当使用 UAA 令牌时才是必需的。
	
	* *Start_Time* 定义用于设置请求开始的相对或绝对时间段。缺省值为 24h 前：“-24h”。*End_Time* 定义用于设置请求结束的相对或绝对时间段。缺省值为当前时间 now：“now”。有关更多信息，请参阅[设置时间段](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#time)。 	
	* *Path* 表明一个或多个度量值。您可以选择对每个度量值应用函数。路径还支持通配符，通过通配符，可以识别单个路径中的多个度量值。有关更多信息，请参阅[定义度量值](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#metrics)。
	
## 使用 IAM 或 API 密钥从空间检索度量值
{: #iam}

要使用 IAM 或 API 密钥从 {{site.data.keyword.Bluemix_notm}} 空间检索度量值，请完成以下步骤：
1. 登录到 {{site.data.keyword.Bluemix_notm}} 区域、组织和空间。运行以下命令：

    例如，要登录到美国南部区域，请运行以下命令：
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    遵循指示信息进行操作。输入您的 {{site.data.keyword.Bluemix_notm}} 凭证，然后选择组织和空间。

2. 获取认证令牌或 API 密钥。

    有关 IAM 认证的更多信息，请参阅[使用 Bluemix CLI 获取 IAM 令牌](/docs/services/cloud-monitoring/security/auth_iam.html#iam_cli)或[使用 Bluemix CLI 生成 IAM API 密钥](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli)。
	例如，要使用 IAM 令牌，请运行以下命令：

    ```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	此命令的结果如下所示：
	
	```
	IAM token:  Bearer djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ
    UAA token:  Bearer eyJhbGciOiJIUz..Ky6vagp3k_QcIcKJ-td83qXhO5Uze43KcplG6PzcGs8
	```
	{: screen}
	
	然后，导出变量 *Token*。例如：
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**注：**令牌不包含 *Bearer*。
		
3. 获取空间 GUID。

    要获取空间 GUID，请参阅[如何获取空间的 GUID](/docs/service/cloud-monitoring/qa/cli_qa.html#space_guid)。该 GUID 必须以 *s-* 为前缀以标识空间。

    例如，运行以下命令：
	
	```
	    bx iam space NAME --guid
    ```
	{: codeblock}
	
	其中，*SpaceName* 是要定义通知的空间的名称，以 *s-* 作为前缀。
	
	例如，要获取名称为 *dev* 的空间的 GUID，请运行以下命令：
	
	```
	bx iam space dev --guid
	```
	{: screen}
	
	此命令的结果如下所示：
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	然后，导出变量 *Space*。For example:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. 运行 cURL 命令来检索度量值。

    ```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics?from=Start_Time&until=End_Time&target=string
	```
	{: codeblock}

	其中
	
	* *X-Auth-User-Token* 是用于传递 {{site.data.keyword.Bluemix_notm}} AIM 令牌或 API 密钥的参数。
	
	* *Auth_Type* 是用于定义令牌或 API 密钥的类型的前缀。

        * *apikey* 表明令牌是 API 密钥。
		* *iam* 表明指定的令牌是 IAM 生成的令牌。
	
	* *X-Auth-Scope-Id* 是用于传递空间 GUID 的参数。该 GUID 必须以 *s-* 为前缀以标识空间。仅当使用 UAA 认证时，此头才是必需的。如果使用 IAM 认证令牌，那么此头是可选的，并且将使用绑定到该令牌的域信息。
	
	* *Token* 表示 UAA 令牌。
	
	* *Space* 表示空间的 GUID。仅当使用 UAA 令牌时才是必需的。
	
	* *Start_Time* 定义用于设置请求开始的相对或绝对时间段。缺省值为 24h 前：“-24h”。*End_Time* 定义用于设置请求结束的相对或绝对时间段。缺省值为当前时间 now：“now”。有关更多信息，请参阅[设置时间段](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#time)。
	
	* *Path* 表明一个或多个度量值。您可以选择对每个度量值应用函数。路径还支持通配符，通过通配符，可以识别单个路径中的多个度量值。有关更多信息，请参阅[定义度量值](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#metrics)。





