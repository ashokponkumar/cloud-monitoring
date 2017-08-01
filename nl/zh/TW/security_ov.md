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


# 安全
{: #security_ov}

您可以使用不同的鑑別方法，將度量值傳送至 {{site.data.keyword.monitoringshort}} 服務。存取、修改及刪除度量值的許可權是藉由使用角色來控制。
{:shortdesc}

   
## 鑑別模型
{: #auth}

若要存取 {{site.data.keyword.Bluemix_notm}} 空間的 {{site.data.keyword.monitoringshort}} 服務所儲存的度量值，您需要鑑別記號或 API 金鑰。 

{{site.data.keyword.monitoringshort}} 服務支援下列鑑別模型：

* [{{site.data.keyword.Bluemix_notm}} UAA 鑑別](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_uaa)
* [{{site.data.keyword.Bluemix_notm}} IAM 鑑別](/docs/services/cloud-monitoring/security/auth_iam.html#auth_iam)

UAA 記號和 IAM 記號會在一段時間後到期。API 金鑰不會到期。
 

如果 API 金鑰外洩，您可以藉由刪除它來加以撤銷。然後，您可以重建新的金鑰。如需相關資訊，請參閱[使用 Bluemix 使用者介面撤銷 API 金鑰](/docs/services/cloud-monitoring/security/auth_iam.html#revoke_ui)。 

IAM 鑑別模型提供使用者介面、CLI 或 API 管理功能。您只能使用 CLI 來管理 UAA 記號。

下表列出每一種網域類型支援的鑑別模型：

<table>
  <caption>表 1. 每一個網域支援的鑑別模型</caption>
  <tr>
    <th></th>
	<th align="right">帳戶</th>
    <th align="right">組織</th>
    <th align="right">空間</th>	
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

{{site.data.keyword.monitoringshort}} 服務會使用 IAM 存取控制特性，控管使用者或服務可對網域執行哪些動作或作業。例如，您可以定義一個 IAM 原則，容許使用者針對網域傳送及建立儀表板，但不容許從網域擷取度量值。



## Bluemix UAA 角色
{: #bmx_roles}

下表列出每一個 {{site.data.keyword.Bluemix_notm}} 角色使用 {{site.data.keyword.monitoringshort}} 服務的專用權：

<table>
  <caption>表 2. 使用 {{site.data.keyword.monitoringshort}} 服務的 {{site.data.keyword.Bluemix_notm}} 角色及專用權。</caption>
  <tr>
    <th>角色</th>
	<th>網域</th>
	<th>存取權</th>
  </tr>
  <tr>
    <td>管理員</td>
	<td>組織<br>空間</td>
	<td>所有 RESTful API</td>
  </tr>
  <tr>
    <td>開發人員</td>
	<td>空間</td>
	<td>所有 RESTful API</td>
  </tr>
  <tr>
    <td>審核員</td>
	<td>組織<br>空間</td>
	<td>僅執行唯讀作業的 RESTful API，如查詢度量值。</td>
  </tr>
</table>


## Bluemix IAM 角色
{: #iam_roles}

下表列出 API、服務動作及「{{site.data.keyword.monitoringshort}} 服務」所使用的 IAM 角色之間的關係。

<table>
  <caption>表 3. API、服務動作與 IAM 角色之間的關係。</caption>
  <tr>
    <th>API</th>
	<th>動作</th>
	<th>IAM 角色</th>
	<th>說明</th>
  </tr>
  <tr>
    <td>POST /v1/metrics</td>
    <td>domain.write</td>
	<td>管理者、編輯者、操作員</td>
	<td>將度量值傳送至網域</td>
  </tr>
  <tr>
    <td>GET /v1/metrics</td>
    <td>domain.render</td>
	<td>管理者、編輯者、檢視者</td>
	<td>擷取/查詢度量值</td>
  </tr>
  <tr>
    <td>GET /v1/metrics/list</td>
    <td>domain.find</td>
	<td>管理者、編輯者</td>
	<td>搜尋網域中的度量值</td>
  </tr>
</table>

## 取得安全記號或 API 金鑰
{: #get_token}

請使用 {{site.data.keyword.Bluemix_notm}} UAA 模型來取得鑑別記號，您可以用此鑑別記號來存取 {{site.data.keyword.Bluemix_notm}} 中之空間的 {{site.data.keyword.monitoringshort}} 服務所儲存的資料。您可以使用 {{site.data.keyword.Bluemix_notm}} CLI 或使用 `Login` REST API，取得鑑別記號。
如需相關資訊，請參閱[使用 {{site.data.keyword.Bluemix_notm}} CLI 取得 UAA 記號](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_cli)及[使用 API 取得 UAA 記號](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_api)。

請使用 {{site.data.keyword.Bluemix_notm}} IAM 模型來取得鑑別記號，您可以用此鑑別記號來存取 {{site.data.keyword.monitoringshort}} 服務中儲存的資料，或是用來取得 API 金鑰。記號具有有效期限。API 金鑰不會到期。
如需相關資訊，請參閱[使用 {{site.data.keyword.Bluemix_notm}} CLI 取得 IAM 記號](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli)、[使用 {{site.data.keyword.Bluemix_notm}} CLI 產生 IAM API 金鑰](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli)或[使用 {{site.data.keyword.Bluemix_notm}} 使用者介面產生 IAM API 金鑰](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_ui)。



