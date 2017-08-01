---

copyright:
  years: 2017

lastupdated: "2017-07-18"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# 通过警报 API 使用通知
{: #notifications}

使用警报 API 可创建、删除和更新通知，显示通知详细信息以及列出在 {{site.data.keyword.Bluemix_notm}} 空间中定义的通知。
{:shortdesc}

## 创建通知模板
{: #template}

通知是一个 JSON 文件。 

可以创建任意数量的通知模板，然后复用这些模板以在您的组织中创建该类型的通知。 

可以定义以下任意类型的通知：

* Email：定义类型为 *Email* 的通知可向有效电子邮件地址发送电子邮件。 
* Webhook：仅对于 HTTPS 端点，定义类型为 *Webhook* 的通知。向端点添加参数以帮助减少其他人尝试调用您的端点的机会。
* Pagerduty：定义类型为 *PagerDuty* 的通知，以将度量值的警报数据发送到 PagerDuty 事件管理系统。 

例如，下表列出了通知模板的示例：

<table>
  <caption></caption>
  <tr>
    <th>类型</th>
	<th>模板</th>
	<th>样本</th>
  </tr>
  <tr>
    <td>Email</td>
	<td>
	```
	{
	"name": "Template_Name",
	"type": "Email",
	"description" : "Description",
	"detail": "EmailAddress"
	}
	```
	{: codeblock}
	</td>
	<td>
	```
	{
	"name": "my-email",
	"type": "Email",
	"description" : "Send email notification when there is an infrastructure problem.",
	"detail": "xxx@yyy.com"
	}
	```
	{: screen}
	</td>
  </tr>
  <tr>
    <td>Webhook</td>
	<td>
	```
	{
	"name": "Template_Name",
	"type": "Webhook",
	"description" : "Description",
	"detail": "Endpoint"
	}
	```
	{: codeblock}
	</td>
	<td>
	```
	{
	"name": "my-webhook",
	"type": "Webhook",
	"description" : "Fire a webhook when there is an infrastructure problem..",
	"detail": "https://myendpoint.bluemix.net?key=abcd1234"
	}
	```
	{: screen}
	</td>
  </tr>
  <tr>
    <td>Pagerduty</td>
	<td>
	```
	{
	"name": "Template_Name",
	"type": "PagerDuty",
	"description" : "Description",
	"detail": "Pagerduty_APIkey"
	}
	```
	{: codeblock}
	</td>
	<td>
	```
	{
	"name": "my-pagerduty",
	"type": "PagerDuty",
	"description" : "Fire a PagerDuty alert when there is an infrastructure problem..",
	"detail": "abcd1234"
	}
	```
	{: screen}
	</td>
  </tr>
</table>

其中

* *Template_Name* 定义通知模板的名称。
* *Description* 说明何时使用此类型的通知。
* *EmailAddress* 定义通知接收方的电子邮件地址。
* *Endpoint* 定义应该执行 POST 操作的 URL。这是可以接受 POST 请求的 URL。 
* *Pagerduty_APIkey* 定义唯一 API 密钥。此 API 密钥由 PagerDuty 帐户管理员或所有者生成。


要创建通知模板，请完成以下步骤：

1. 创建一个目录以用于存储 {{site.data.keyword.monitoringshort}} 服务资源，例如 *cloud-monitoring*。

    例如，在 Ubuntu 系统中，运行以下命令：
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
2. 创建一个目录以用于存储通知模板，例如 *notification-templates*。

    例如，在 Ubuntu 系统中，运行以下命令：
	
	```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	切换到此目录：
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
3. 在系统的本地目录中创建通知模板。

    例如，使用 vi 编辑器为电子邮件通知模板创建 **email-template.json** 文件： 
	
	```
	{
    "name": "email_template",
    "type": "Email",
    "description" : "Send email to manager of department X when ....",
    "detail": "xxx@yyy"
    }
	```
	{: codeblock}
	


## 创建通知
{: #create}

要创建通知，请完成以下步骤：

1. 创建通知文件。

    1. 使用通知模板来创建该文件。有关更多信息，请参阅[创建通知模板](#template)。
	
	2. 更新文件：
	
	    * 在 *name* 字段中输入唯一名称。此字段的值后续将用于更新、删除和显示通知的详细信息。
		* 输入描述。
		* 输入有效电子邮件地址、URL 端点或 PagerDuty 密钥。
		
	3. 保存文件。例如，针对您为 {{site.data.keyword.Bluemix_notm}} 空间中运行的查询所定义的通知，使用 *s-* 之类的前缀。
	
	    **提示**：在 *~/cloud-monitoring/notifications/* 目录中创建通知文件，以用于对 {{site.data.keyword.monitoringshort_notm}} 服务的资源分组。 
	
2. 登录到 {{site.data.keyword.Bluemix_notm}} 区域、组织和空间。运行以下命令：

    例如，要登录到美国南部区域，请运行以下命令：
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    遵循指示信息进行操作。输入您的 {{site.data.keyword.Bluemix_notm}} 凭证，然后选择组织和空间。

3. 获取认证令牌或 API 密钥。

    * 有关 IAM 认证的信息，请参阅[使用 Bluemix CLI 获取 IAM 令牌](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli)或[使用 Bluemix CLI 生成 IAM API 密钥](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli)。
	
	* 对于 UAA 认证，请参阅[使用 Bluemix CLI 获取 UAA 令牌](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli)或[使用 REST API 获取 UAA 令牌](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api)。

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
	
	然后，导出变量 *Token*：
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**注：**令牌不包含 *Bearer*。
		
4. 获取空间 GUID。该 GUID 必须以 *s-* 为前缀以标识空间。

    运行以下命令：
```
	bx iam space SpaceName --guid
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
	
	然后，导出变量 *Space*：
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. 运行以下 cURL 命令来创建通知：

    ```
	curl -XPOST -d @Notification_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: codeblock}
	
	其中
	
	* Notification_File 是用于定义通知的 JSON 文件。
	
	* *X-Auth-User-Token* 是用于传递 {{site.data.keyword.Bluemix_notm}} UAA 令牌、IAM 令牌或 API 密钥的参数。
	
	* *Auth_Type* 是用于定义令牌或 API 密钥的类型的前缀。以下列表概括了有效值：*apikey*、*iam* 或 *uaa*，其中：

  * *apikey* 表明令牌是 API 密钥。
		* *iam* 表明指定的令牌是 IAM 生成的令牌。
		* *uaa* 表明指定的令牌是 UAA 生成的令牌。
	
	* *X-Auth-Scope-Id* 是用于传递空间 GUID 的参数。该 GUID 必须以 *s-* 为前缀以标识空间。如果使用 UAA 认证令牌，此头是必需的。如果使用 IAM 认证令牌，那么此头是可选的，并且将使用绑定到该令牌的域信息。
	
	* Token 是 UAA 令牌、IAM 令牌或 API 密钥。
	
	* Space 是空间的 GUID。仅当使用 UAA 令牌时才是必需的。
	
	例如：
```
	curl -XPOST -d @s-email-dep-A.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.{DomainName}/v1/alert/notification
	```
	{: screen}

## 删除通知
{:#delete}

要删除通知，请完成以下步骤：

1. 登录到 {{site.data.keyword.Bluemix_notm}} 区域、组织和空间。运行以下命令：

    例如，要登录到美国南部区域，请运行以下命令：
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    遵循指示信息进行操作。输入您的 {{site.data.keyword.Bluemix_notm}} 凭证，然后选择组织和空间。

2. 获取认证令牌或 API 密钥。

    * 对于 IAM 认证，请参阅[使用 Bluemix CLI 获取 IAM 令牌](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) 或[使用 Bluemix CLI 生成 IAM API 密钥](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli)。 	
	* 对于 UAA 认证，请参阅[使用 Bluemix CLI 获取 UAA 令牌 ](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) 或[使用 REST API 获取 UAA 令牌](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api)。例如，要使用 IAM 令牌，请运行以下命令：

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
	
	然后，导出变量 *Token*：
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**注：**令牌不包含 *Bearer*。
		
3. 获取空间 GUID。该 GUID 必须以 *s-* 为前缀以标识空间。

    运行以下命令：
```
	bx iam space SpaceName --guid
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
	
	然后，导出变量 *Space*：
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. 运行以下 cURL 命令来删除通知：

    ```
	curl -XDELETE --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification/Notification_Name
	```
	{: codeblock}
	
	其中
	
	* *Auth_Type* 是用于定义令牌或 API 密钥的类型的前缀。以下列表概括了有效值：*apikey*、*iam* 或 *uaa*，其中：

  * *apikey* 表明令牌是 API 密钥。
		* *iam* 表明指定的令牌是 IAM 生成的令牌。
		* *uaa* 表明指定的令牌是 UAA 生成的令牌。
	
	* *X-Auth-User-Token* 是用于传递 {{site.data.keyword.Bluemix_notm}} UAA 令牌、IAM 令牌或 API 密钥的参数。令牌或 API 密钥必须使用以下某个值作为前缀：*apikey*、*iam* 或 *uaa*，其中：

  * *apikey* 表明令牌是 API 密钥。
		* *iam* 表明指定的令牌是 IAM 生成的令牌。
		* *uaa* 表明指定的令牌是 UAA 生成的令牌。
	
	* Token 是 UAA 令牌、IAM 令牌或 API 密钥。
	
	* Space 是空间的 GUID。仅当使用 UAA 令牌时才是必需的。
	
	* Notification_Name 是通知的名称，即列出通知属性时 *name* 字段的值。
	


## 列出所有通知
{: #list}

要列出所有通知，请完成以下步骤：

1. 登录到 {{site.data.keyword.Bluemix_notm}} 区域、组织和空间。运行以下命令：

    例如，要登录到美国南部区域，请运行以下命令：
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    遵循指示信息进行操作。输入您的 {{site.data.keyword.Bluemix_notm}} 凭证，然后选择组织和空间。

2. 获取认证令牌或 API 密钥。

    * 对于 IAM 认证，请参阅[使用 Bluemix CLI 获取 IAM 令牌](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) 或[使用 Bluemix CLI 生成 IAM API 密钥](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli)。
	
	* 对于 UAA 认证，请参阅[使用 Bluemix CLI 获取 UAA 令牌 ](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) 或[使用 REST API 获取 UAA 令牌](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api)。

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
	
	然后，导出变量 *Token*：
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**注：**令牌不包含 *Bearer*。
		
3. 获取空间 GUID。该 GUID 必须以 *s-* 为前缀以标识空间。

    运行以下命令：
```
	bx iam space SpaceName --guid
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
	
	然后，导出变量 *Space*：
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. 运行以下 cURL 命令来列出所有通知：

    ```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notifications
	```
	{: codeblock}
	
	其中
	
	* *Auth_Type* 是用于定义令牌或 API 密钥的类型的前缀。以下列表概括了有效值：*apikey*、*iam* 或 *uaa*，其中：

  * *apikey* 表明令牌是 API 密钥。
		* *iam* 表明指定的令牌是 IAM 生成的令牌。
		* *uaa* 表明指定的令牌是 UAA 生成的令牌。
	
	* *X-Auth-User-Token* 是用于传递 {{site.data.keyword.Bluemix_notm}} UAA 令牌、IAM 令牌或 API 密钥的参数。令牌或 API 密钥必须使用以下某个值作为前缀：*apikey*、*iam* 或 *uaa*，其中：

  * *apikey* 表明令牌是 API 密钥。
		* *iam* 表明指定的令牌是 IAM 生成的令牌。
		* *uaa* 表明指定的令牌是 UAA 生成的令牌。
	
	* Token 是 UAA 令牌、IAM 令牌或 API 密钥。
	
	* Space 是空间的 GUID。仅当使用 UAA 令牌时才是必需的。
	
	## 显示通知的详细信息
{: #show}

要显示有关通知的信息，请完成以下步骤：

1. 登录到 {{site.data.keyword.Bluemix_notm}} 区域、组织和空间。运行以下命令：

    例如，要登录到美国南部区域，请运行以下命令：
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    遵循指示信息进行操作。输入您的 {{site.data.keyword.Bluemix_notm}} 凭证，然后选择组织和空间。

2. 获取认证令牌或 API 密钥。

    * 对于 IAM 认证，请参阅[使用 Bluemix CLI 获取 IAM 令牌](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) 或[使用 Bluemix CLI 生成 IAM API 密钥](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli)。
	
	* 对于 UAA 认证，请参阅[使用 Bluemix CLI 获取 UAA 令牌 ](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) 或[使用 REST API 获取 UAA 令牌](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api)。

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
	
	然后，导出变量 *Token*：
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**注：**令牌不包含 *Bearer*。
		
3. 获取空间 GUID。该 GUID 必须以 *s-* 为前缀以标识空间。

    运行以下命令：
```
	bx iam space SpaceName --guid
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
	
	然后，导出变量 *Space*：
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. 运行以下 cURL 命令来显示通知的详细信息：

    ```
	curl -XGET --header "X-Auth-User-Token:uaa ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification/Notification_Name
	```
	{: codeblock}
	
	其中
	
	* *X-Auth-User-Token* 是用于传递 {{site.data.keyword.Bluemix_notm}} UAA 令牌、IAM 令牌或 API 密钥的参数。令牌或 API 密钥必须使用以下某个值作为前缀：*apikey*、*iam* 或 *uaa*，其中：

  * *apikey* 表明令牌是 API 密钥。
		* *iam* 表明指定的令牌是 IAM 生成的令牌。
		* *uaa* 表明指定的令牌是 UAA 生成的令牌。
	
	* Token 是 UAA 令牌、IAM 令牌或 API 密钥。
	
	* Space 是空间的 GUID。仅当使用 UAA 令牌时才是必需的。
	
	* Notification_Name 是通知的名称，即列出通知属性时 *name* 字段的值。
	


## 测试通知
{: #test}	
			
要测试通知，请完成以下步骤：

1. 登录到 {{site.data.keyword.Bluemix_notm}} 区域、组织和空间。运行以下命令：

    例如，要登录到美国南部区域，请运行以下命令：
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    遵循指示信息进行操作。输入您的 {{site.data.keyword.Bluemix_notm}} 凭证，然后选择组织和空间。

2. 获取认证令牌或 API 密钥。

    * 对于 IAM 认证，请参阅[使用 Bluemix CLI 获取 IAM 令牌](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) 或[使用 Bluemix CLI 生成 IAM API 密钥](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli)。
	
	* 对于 UAA 认证，请参阅[使用 Bluemix CLI 获取 UAA 令牌 ](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) 或[使用 REST API 获取 UAA 令牌](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api)。

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
	
	然后，导出变量 *Token*：
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**注：**令牌不包含 *Bearer*。
		
3. 获取空间 GUID。该 GUID 必须以 *s-* 为前缀以标识空间。

    运行以下命令：
```
	bx iam space SpaceName --guid
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
	
	然后，导出变量 *Space*：
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. 运行以下 cURL 命令来测试通知：

    ```
	curl -XPOST --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification/test/Notification_Name
	```
	{: codeblock}
	
	其中
	
	* *Auth_Type* 是用于定义令牌或 API 密钥的类型的前缀。以下列表概括了有效值：*apikey*、*iam* 或 *uaa*，其中：

  * *apikey* 表明令牌是 API 密钥。
		* *iam* 表明指定的令牌是 IAM 生成的令牌。
		* *uaa* 表明指定的令牌是 UAA 生成的令牌。
	
	* *X-Auth-User-Token* 是用于传递 {{site.data.keyword.Bluemix_notm}} UAA 令牌、IAM 令牌或 API 密钥的参数。令牌或 API 密钥必须使用以下某个值作为前缀：*apikey*、*iam* 或 *uaa*，其中：

  * *apikey* 表明令牌是 API 密钥。
		* *iam* 表明指定的令牌是 IAM 生成的令牌。
		* *uaa* 表明指定的令牌是 UAA 生成的令牌。
	
	* Token 是 UAA 令牌、IAM 令牌或 API 密钥。
	
	* Space 是空间的 GUID。仅当使用 UAA 令牌时才是必需的。
	
	* Notification_Name 是通知的名称，即列出通知属性时 *name* 字段的值。
	


## 更新通知
{: #update}

要更新通知，请完成以下步骤：

1. 登录到 {{site.data.keyword.Bluemix_notm}} 区域、组织和空间。运行以下命令：

    例如，要登录到美国南部区域，请运行以下命令：
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    遵循指示信息进行操作。输入您的 {{site.data.keyword.Bluemix_notm}} 凭证，然后选择组织和空间。

2. 获取认证令牌或 API 密钥。

    * 对于 IAM 认证，请参阅[使用 Bluemix CLI 获取 IAM 令牌](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) 或[使用 Bluemix CLI 生成 IAM API 密钥](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli)。
	
	* 对于 UAA 认证，请参阅[使用 Bluemix CLI 获取 UAA 令牌 ](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) 或[使用 REST API 获取 UAA 令牌](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api)。

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
	
	然后，导出变量 *Token*：
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**注：**令牌不包含 *Bearer*。
		
3. 获取空间 GUID。该 GUID 必须以 *s-* 为前缀以标识空间。

    运行以下命令：
```
	bx iam space SpaceName --guid
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
	
	然后，导出变量 *Space*：
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. 运行以下 cURL 命令来更新通知：

    ```
	curl -XPUT -d @Notification_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: codeblock}
	
	其中
	
	* Notification_File 是用于定义通知的 JSON 文件。
	
	* *Auth_Type* 是用于定义令牌或 API 密钥的类型的前缀。以下列表概括了有效值：*apikey*、*iam* 或 *uaa*，其中：

  * *apikey* 表明令牌是 API 密钥。
		* *iam* 表明指定的令牌是 IAM 生成的令牌。
		* *uaa* 表明指定的令牌是 UAA 生成的令牌。
	
	* *X-Auth-User-Token* 是用于传递 Bluemix UAA 令牌、IAM 令牌或 API 密钥的参数。令牌或 API 密钥必须使用以下某个值作为前缀：*apikey*、*iam* 或 *uaa*，其中：

  * *apikey* 表明令牌是 API 密钥。
		* *iam* 表明指定的令牌是 IAM 生成的令牌。
		* *uaa* 表明指定的令牌是 UAA 生成的令牌。
	
	* Token 是 UAA 令牌、IAM 令牌或 API 密钥。
	
	* Space 是空间的 GUID。仅当使用 UAA 令牌时才是必需的。
	
	

