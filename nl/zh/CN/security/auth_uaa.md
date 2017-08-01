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


# 使用 Bluemix UAA 模型进行认证
{: #auth_uaa}

使用 {{site.data.keyword.Bluemix}} UAA 模型可获取认证令牌，可以使用该认证令牌访问 {{site.data.keyword.monitoringshort}} 服务中存储的针对 {{site.data.keyword.Bluemix_notm}} 中空间的度量值。可以使用 {{site.data.keyword.Bluemix_notm}} CLI 或使用 `Login` REST API 来获取认证令牌。
{:shortdesc}

要使用 UAA 认证令牌访问度量值，您需要以下信息：

* 用于通过 RESTful API 访问 {{site.data.keyword.monitoringshort}} 服务的 UAA 令牌。
* 空间的 GUID。

		
## 使用 Bluemix CLI 获取 UAA 令牌
{: #uaa_cli}


要获取授权令牌，请完成以下步骤：

1. 安装 {{site.data.keyword.Bluemix_notm}} CLI。

   有关更多信息，请参阅[安装 {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa)。
   
   如果 CLI 已安装，请继续执行下一步。
    
2. 登录到 {{site.data.keyword.Bluemix_notm}} 区域、组织和空间。运行以下命令：

    ```
    cf login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    遵循指示信息进行操作。输入您的 {{site.data.keyword.Bluemix_notm}} 凭证，然后选择组织和空间。
	
3. 运行 `cf oauth-token` 命令来获取 {{site.data.keyword.Bluemix_notm}} UAA 令牌。

    ```
	cf oauth-token
	```
	{: codeblock}
	
	输出会返回 UAA 令牌，必须使用该令牌在该空间和组织中认证您的用户标识。

4. 获取已为其获取认证令牌的组织空间的 GUID。

   有关更多信息，请参阅[如何获取空间的 GUID](/docs/services/cloud-monitoring/qa/cli_qa.html#space_guid)。
	
5. 导出以下变量：TOKEN 和 SPACE。

    * *TOKEN* 是您在上一步中获取的 oauth 令牌（不包括 Bearer）。
	
	* *SPACE* 是在上一步中获取的空间 GUID。
		
	例如：
	
	```
	export TOKEN="eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8"
	export SPACE="667fb895-abcd-defg-aaaa-cf4587341095"
	```
	{: screen}
	
6. 运行以下命令来获取 UAA 令牌以使用 {{site.data.keyword.monitoringshort}} 服务：

    ```
	curl -k -X GET  --header "X-Auth-Token: ${TOKEN}"  --header "X-Auth-Project-Id: ${SPACE}"  METRICS_ENDPOINT
    ```
    {: codeblock}	
	
	其中
	* SPACE 是运行服务的空间的 GUID。
	* TOKEN 是在上一步中获取的 {{site.data.keyword.Bluemix_notm}} UAA 令牌，不包含 bearer 前缀。
	* METRICS_ENDPOINT 是组织和空间在其中可用的 {{site.data.keyword.Bluemix_notm}} 区域的度量值端点 (https://metrics.ng.bluemix.net/token)。

	
## 使用 API 获取 UAA 令牌
{: #uaa_api}

要获取授权令牌，请运行以下 cURL 命令：

```
curl -XPOST -d 'user=USERNAME&passwd=PASSWORD&space=SPACE_NAME&organization=ORG_NAME' METRICS_ENDPOINT
```
{: codeblock}

其中

* USERNAME 是要为其获取认证令牌以使用 {{site.data.keyword.monitoringshort}} 服务的 {{site.data.keyword.Bluemix_notm}} 标识。
* PASSWORD 是用于登录 {{site.data.keyword.Bluemix_notm}} 的用户标识的密码。
* SPACE_NAME 是要收集其度量值的空间的名称。
* ORG_NAME 是 {{site.data.keyword.Bluemix_notm}} 中托管空间的组织的名称。
* METRICS_ENDPOINT 是组织和空间在其中可用的 {{site.data.keyword.Bluemix_notm}} 区域的度量值端点 (https://metrics.ng.bluemix.net/login)。
	
输出是包含 UAA 令牌的 JSON 文档。从 **access-token** 字段获取该令牌的值。

例如，样本 JSON 文档类似以下内容：

```
{
    "access_token": "eyJhbGc...",
    "logging_token": "xxxxxxxxxxxx",
    "org_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "space_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```
{: screen}

*logging_token* 的值对应于使用 {{site.data.keyword.monitoringshort}} 服务时所需的 UAA 令牌。

**注：**

* 如果使用的不是 cURL，那么必须设置头 **Content-Type: application/x-www-form-urlencoded**。
* 如果收到错误代码 *BXNMS0122E: 用户凭证无效*，请检查您是否使用了有效的 {{site.data.keyword.IBM_notm}} 标识。


