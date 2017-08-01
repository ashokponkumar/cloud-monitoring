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


# 向 Monitoring 服务发送数据
{: #send_data_api}

您可以使用“度量值 API”将度量值发送到 {{site.data.keyword.monitoringshort}} 服务。您可以将度量值发送到 {{site.data.keyword.Bluemix_notm}} 空间。
{:shortdesc}


对于 {{site.data.keyword.Bluemix_notm}} Docker 容器，会自动收集基本系统度量值。对于 Cloud Foundry 应用程序以及在虚拟机 (VM) 中运行的应用程序，必须使用[度量值 API](https://console.bluemix.net/apidocs/927-ibm-cloud-monitoring-rest-api?&language=node#introduction){: new_window} 从应用程序直接发送度量值。 



## 使用 UAA 将度量值发送到空间
{: #uaa}

要将度量值发送到 {{site.data.keyword.Bluemix_notm}} 空间，请完成以下步骤：

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
	export Token="eyJhbGciOiJI....cGlxryWS8"
	```
	{: screen}
	
	**注：**令牌不包含 *Bearer*。
		
3. 获取空间 GUID。该 GUID 必须以 *s-* 为前缀以标识空间。

    运行以下命令：
```
	cf space SpaceName --guid
	```
	{: codeblock}
	
	其中，*SpaceName* 是要定义通知的空间的名称。
	
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
	curl -XPOST -d @Metric_File --header "X-Auth-User-Token:uaa ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics
	```
	{: codeblock}
	
	其中
	
	* Metrics_File 表示 JSON 文件，其中包含发送到 {{site.data.keyword.monitoringshort}} 服务的度量值的定义。
	
	* *X-Auth-User-Token* 是用于传递 {{site.data.keyword.Bluemix_notm}} UAA 令牌的参数。
	
	* *Auth_Type* 是用于定义令牌类型的前缀。*uaa* 表明指定的令牌是 UAA 生成的令牌。
	
	* *X-Auth-Scope-Id* 是用于传递空间 GUID 的参数。该 GUID 必须以 *s-* 为前缀以标识空间。

    * Token 表示 UAA 令牌。
	
	* Space 表示空间的 GUID。
	
	例如，可以运行以下命令将“myhost.cpu.idle”和“myapp.login.attempts”这两个度量值发送到 {{site.data.keyword.monitoringshort}} 服务： 	
	```
	curl -XPOST @metric.json --header "X-Auth-User-Token:uaa ${Token}" https://metrics.ng.bluemix.net/v1/metrics
	```
	{: screen}
	
	样本文件 *metric.json* 按如下所示进行定义：

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

 
## 使用 IAM 或 API 密钥将度量值发送到空间
{: #iam}

要将度量值发送到 {{site.data.keyword.Bluemix_notm}} 空间，请完成以下步骤：

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
	
	其中，*SpaceName* 是要定义通知的空间的名称。
	
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
	
	然后，导出变量 *Space*。例如：
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. 运行 cURL 命令来发送度量值。

    ```
	curl -XPOST -d @Metric_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics
	```
	{: codeblock}
	
	其中
	
	* Metrics_File 表示 JSON 文件，其中包含发送到 {{site.data.keyword.monitoringshort}} 服务的度量值的定义。
	
	* *X-Auth-User-Token* 是用于传递 {{site.data.keyword.Bluemix_notm}} IAM 令牌或 API 密钥的参数。
	
	* *Auth_Type* 是用于定义令牌或 API 密钥的类型的前缀。以下列表概括了有效值：*apikey*、*iam* 或 *uaa*，其中：

  * *apikey* 表明令牌是 API 密钥。
		* *iam* 表明指定的令牌是 IAM 生成的令牌。
		* *X-Auth-Scope-Id* 是用于传递空间 GUID 的参数。该 GUID 必须以 *s-* 为前缀以标识空间。

    * Token 表示 IAM 令牌或 API 密钥。
	
	* Space 表示空间的 GUID。
	
	例如，可以运行以下命令将“myhost.cpu.idle”和“myapp.login.retries”这两个度量值发送到 {{site.data.keyword.monitoringshort}} 服务中的空间：
	
	```
curl -XPOST @metric.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/metrics
```
{: screen}
	
样本文件 *metric.json* 按如下所示进行定义：

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


















 
