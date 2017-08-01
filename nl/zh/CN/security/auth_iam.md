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


# 使用 Bluemix IAM 模型进行认证
{: #auth_iam}

使用 {{site.data.keyword.Bluemix}} IAM 模型可获取认证令牌，可以使用该认证令牌访问在 {{site.data.keyword.monitoringshort}} 服务中存储的度量值或者获取 API 密钥。该令牌有到期时间。API 密钥不会到期。
{:shortdesc}


## 使用 Bluemix CLI 获取 IAM 令牌 
{: #iam_token_cli}

要使用 {{site.data.keyword.Bluemix_notm}} CLI 获取授权令牌，请完成以下步骤：

1. 安装 {{site.data.keyword.Bluemix_notm}} CLI。

   有关更多信息，请参阅[安装 {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa)。
   
   如果 CLI 已安装，请继续执行下一步。
    
2. 登录到 {{site.data.keyword.Bluemix_notm}} 区域、组织和空间。运行以下命令：

    例如，对于美国南部区域，请运行以下命令：
	
    ```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    遵循指示信息进行操作。输入您的 {{site.data.keyword.Bluemix_notm}} 凭证，然后选择组织和空间。
	
3. 运行 `bx iam oauth-tokens` 命令来获取 IAM 令牌。

    ```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	输出会返回 IAM 令牌，必须使用该令牌在该空间和组织中认证您的用户标识。
**注：** 使用令牌时，请从 API 调用中传递的令牌的值中除去 *Bearer*。
		
		
## 使用 Bluemix CLI 生成 IAM API 密钥
{: #iam_apikey_cli}

完成以下步骤，以使用 {{site.data.keyword.Bluemix_notm}} CLI 生成 IAM API 密钥：

1. 安装 {{site.data.keyword.Bluemix_notm}} CLI。

   有关更多信息，请参阅[安装 {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa)。
   如果 CLI 已安装，请继续执行下一步。

2. 使用“bx login”{{site.data.keyword.Bluemix_notm}} CLI 命令登录到 {{site.data.keyword.Bluemix_notm}}：

    例如，对于美国南部区域，请运行以下命令：
	
    ```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    遵循指示信息进行操作。输入您的 {{site.data.keyword.Bluemix_notm}} 凭证，然后选择组织和空间。

3. 运行“bx iam api-key-create”命令来创建 IAM API 密钥。

    ```
    bx iam api-key-create NAME [-d DESCRIPTION][-f, --file FILE]
	```
	{: codeblock} 
	
	其中
	
	* NAME 是要创建的 API 密钥的名称。
 * DESCRIPTION 是用于描述 API 密钥的文本。
	* FILE 是保存密钥的文件的名称。
	
    例如，要创建 API key *MyKey*，请运行以下命令：
	
	```
	bx iam api-key-create MyKey -d "This is my API key for service X" 
	```
	{: screen}
	
## 使用 Bluemix UI 生成 IAM API 密钥
{: #iam_apikey_ui}

完成以下步骤，以通过 {{site.data.keyword.Bluemix_notm}} UI 生成 IAM API 密钥：
1. 登录到 {{site.data.keyword.Bluemix_notm}} 控制台。

2. 在控制台菜单栏中，单击 **管理 > 安全性 > Bluemix API 密钥**。

3. 单击 **创建 API 密钥**。

4. 为 API 密钥输入名称和描述。然后，单击 **创建 API 密钥**。

5. 保存 API 密钥。单击 **下载 API 密钥**。

    单击 **显示** 以显示 API 密钥。

**注：** API 密钥仅在创建时才可显示或下载。如果 API 密钥丢失，必须创建新的 API 密钥。
## 使用 Bluemix UI 撤销 API 密钥
{: #revoke_ui}
	
完成以下步骤，以通过 {{site.data.keyword.Bluemix_notm}} UI 撤销 IAM API 密钥：

1. 登录到 {{site.data.keyword.Bluemix_notm}} 控制台。

2. 在控制台菜单栏中，单击 **管理 > 安全性 > Bluemix API 密钥**。

3. 单击 **操作**，然后 **删除密钥**。





	

	
	
	
	
	
	
