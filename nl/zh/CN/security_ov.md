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


# 安全性
{: #security_ov}

可以使用不同的认证方法将度量值发送到 {{site.data.keyword.monitoringshort}} 服务。访问、修改和删除度量值的许可权通过使用角色进行控制。
{:shortdesc}

   
## 认证模型
{: #auth}

要访问在 {{site.data.keyword.monitoringshort}} 服务中存储的针对 {{site.data.keyword.Bluemix_notm}} 空间的度量值，您需要认证令牌或 API 密钥。 

{{site.data.keyword.monitoringshort}} 服务支持以下认证模型：

* [{{site.data.keyword.Bluemix_notm}} UAA 认证](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_uaa)
* [{{site.data.keyword.Bluemix_notm}} IAM 认证](/docs/services/cloud-monitoring/security/auth_iam.html#auth_iam)

UAA 令牌和 IAM 令牌在一段时间后到期。API 密钥不会到期。
 

如果 API 密钥已损坏，可以通过删除 API 密钥来将其撤销。然后，您可以重新创建一个新的 API 密钥。有关更多信息，请参阅[使用 Bluemix UI 撤销 API 密钥](/docs/services/cloud-monitoring/security/auth_iam.html#revoke_ui)。 

IAM 认证模型提供 UI、CLI 或 API 管理功能。您仅可以使用 CLI 来管理 UAA 令牌。

下表列出了每种类型的域支持的认证模型：

<table>
  <caption>表 1. 每种域支持的认证模型</caption>
  <tr>
    <th></th>
	<th align="right">帐户</th>
    <th align="right">组织</th>
    <th align="right">空间</th>	
  </tr>
  <tr>
    <th align="left">UAA</th>
	<td align="center">否</td>
	<td align="center">是</td>
	<td align="center">是</td>
  </tr>
  <tr>
    <th align="left">IAM</th>
	<td align="center">是</td>
	<td align="center">是</td>
	<td align="center">是</td>
  </tr>
</table>

{{site.data.keyword.monitoringshort}} 服务使用 IAM 访问控制功能来管理用户或服务可以对域执行的操作。例如，可以定义 IAM 策略以允许用户针对域发送和创建仪表板，但不允许用户检索域中的度量值。



## Bluemix UAA 角色
{: #bmx_roles}

下表列出了每个 {{site.data.keyword.Bluemix_notm}} 角色使用 {{site.data.keyword.monitoringshort}} 服务的特权：

<table>
  <caption>表 2. 使用 {{site.data.keyword.monitoringshort}} 服务的 {{site.data.keyword.Bluemix_notm}} 角色和特权。</caption>
  <tr>
    <th>角色</th>
	<th>域</th>
	<th>访问特权</th>
  </tr>
  <tr>
    <td>管理员</td>
	<td>组织<br>空间</td>
	<td>所有 RESTful API</td>
  </tr>
  <tr>
    <td>开发人员</td>
	<td>空间</td>
	<td>所有 RESTful API</td>
  </tr>
  <tr>
    <td>审计员</td>
	<td>组织<br>空间</td>
	<td>仅限执行只读操作（如查询度量值）的 RESTful API。</td>
  </tr>
</table>


## Bluemix IAM 角色
{: #iam_roles}

下表列出了 {{site.data.keyword.monitoringshort}} 服务使用的 API、服务操作和 IAM 角色之间的关系。

<table>
  <caption>表 3. API、服务操作和 IAM 角色之间的关系。</caption>
  <tr>
    <th>API</th>
	<th>操作</th>
	<th>IAM 角色</th>
	<th>描述</th>
  </tr>
  <tr>
    <td>POST /v1/metrics</td>
    <td>domain.write</td>
	<td>管理员、编辑者和操作员</td>
	<td>向域发送度量值</td>
  </tr>
  <tr>
    <td>GET /v1/metrics</td>
    <td>domain.render</td>
	<td>管理员、编辑者和查看者</td>
	<td>检索/查询度量值</td>
  </tr>
  <tr>
    <td>GET /v1/metrics/list</td>
    <td>domain.find</td>
	<td>管理员和编辑者</td>
	<td>在域中搜索度量值</td>
  </tr>
</table>

## 获取安全性令牌或 API 密钥
{: #get_token}

使用 {{site.data.keyword.Bluemix_notm}} UAA 模型可获取认证令牌，可以使用该认证令牌访问 {{site.data.keyword.monitoringshort}} 服务中存储的针对 {{site.data.keyword.Bluemix_notm}} 中空间的数据。可以使用 {{site.data.keyword.Bluemix_notm}} CLI 或使用 `Login` REST API 来获取认证令牌。
有关更多信息，请参阅[使用 {{site.data.keyword.Bluemix_notm}} CLI 获取 UAA 令牌](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_cli)和[使用 API 获取 UAA 令牌](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_api)。

使用 {{site.data.keyword.Bluemix_notm}} IAM 模型可获取认证令牌，可以使用该认证令牌访问在 {{site.data.keyword.monitoringshort}} 服务中存储的数据或者获取 API 密钥。该令牌有到期时间。API 密钥不会到期。
有关更多信息，请参阅[使用 {{site.data.keyword.Bluemix_notm}} CLI 获取 IAM 令牌](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli)、[使用 {{site.data.keyword.Bluemix_notm}} CLI 生成 IAM API 密钥](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli)或[使用 {{site.data.keyword.Bluemix_notm}} UI 生成 IAM API 密钥](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_ui)。



