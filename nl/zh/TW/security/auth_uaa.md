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


# 使用 Bluemix UAA 模型進行鑑別
{: #auth_uaa}

使用 {{site.data.keyword.Bluemix}} UAA 模型來取得鑑別記號，您可以用來存取 {{site.data.keyword.monitoringshort}} 服務中儲存的度量值，以取得 {{site.data.keyword.Bluemix_notm}} 中的空間。您可以使用 {{site.data.keyword.Bluemix_notm}} CLI 或使用 `Login` REST API，取得鑑別記號。
{:shortdesc}

若要使用 UAA 鑑別記號存取度量值，您需要下列資訊：

* 使用 RESTful API 存取 {{site.data.keyword.monitoringshort}} 服務的 UAA 記號。
* 空間的 GUID。

		
## 使用 Bluemix CLI 取得 UAA 記號
{: #uaa_cli}


若要取得授權記號，請完成下列步驟：

1. 安裝 {{site.data.keyword.Bluemix_notm}} CLI。

   如需相關資訊，請參閱[安裝 {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa)。
   
   如果已安裝 CLI，請繼續進行下一步。
    
2. 登入 {{site.data.keyword.Bluemix_notm}} 地區、組織及空間。執行下列指令：

    ```
    cf login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    遵循指示。輸入您的 {{site.data.keyword.Bluemix_notm}} 認證，選取組織及空間。
	
3. 執行 `cf oauth-token` 指令，以取得 {{site.data.keyword.Bluemix_notm}} UAA 記號。

	```
	cf oauth-token
	```
	{: codeblock}
	
	輸出會傳回 UAA 記號，您必須用來在該空間及組織中鑑別您的使用者 ID。

4. 針對您已取得其鑑別記號之組織的空間取得 GUID。

   如需相關資訊，請參閱[如何取得空間的 GUID](/docs/services/cloud-monitoring/qa/cli_qa.html#space_guid)。  
	
5. 匯出下列變數：TOKEN 及 SPACE。

	* *TOKEN* 是您在前一個步驟中取得的 oauth 記號，不包含 Bearer。
	
	* *SPACE* 是您在前一個步驟中取得的空間 GUID。
		
	例如，
	
	```
	export TOKEN="eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8"
	export SPACE="667fb895-abcd-defg-aaaa-cf4587341095"
	```
	{: screen}
	
6. 執行下列指令來取得 UAA 記號，以使用 {{site.data.keyword.monitoringshort}} 服務：
 
    ```
	curl -k -X GET  --header "X-Auth-Token: ${TOKEN}"  --header "X-Auth-Project-Id: ${SPACE}"  METRICS_ENDPOINT
    ```
    {: codeblock}	
	
	其中
	* SPACE 是服務執行所在空間的 GUID。
	* TOKEN 是您在前一個步驟中取得的 {{site.data.keyword.Bluemix_notm}} UAA 記號，不含 bearer 字首。
	* METRICS_ENDPOINT 是提供組織及空間的 {{site.data.keyword.Bluemix_notm}} 地區的度量值端點 (https://metrics.ng.bluemix.net/token)。

	
## 使用 API 取得 UAA 記號
{: #uaa_api}

若要取得授權記號，請執行下列 cURL 指令：

```
curl -XPOST -d 'user=USERNAME&passwd=PASSWORD&space=SPACE_NAME&organization=ORG_NAME' METRICS_ENDPOINT
```
{: codeblock}

其中

* USERNAME 是您要取得其鑑別記號以使用 {{site.data.keyword.monitoringshort}} 服務的 {{site.data.keyword.Bluemix_notm}} ID。
* PASSWORD 是用來登入 {{site.data.keyword.Bluemix_notm}} 的使用者 ID 的密碼。
* SPACE_NAME 是在其中收集度量值的空間名稱。
* ORG_NAME 是 {{site.data.keyword.Bluemix_notm}} 中管理空間的組織名稱。
* METRICS_ENDPOINT 是提供組織及空間的 {{site.data.keyword.Bluemix_notm}} 地區的度量值端點 (https://metrics.ng.bluemix.net/login)。

輸出是包含 UAA 記號的 JSON 文件。請從 **access-token** 欄位取得記號的值。 

例如，範例 JSON 文件如下所示：

```
{
    "access_token": "eyJhbGc...",
    "logging_token": "xxxxxxxxxxxx",
    "org_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "space_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```
{: screen}

*logging_token* 的值對應於您使用 {{site.data.keyword.monitoringshort}} 服務所需的 UAA 記號。
 
**附註：** 

* 如果您不是使用 cURL，則必須設定標頭 **Content-Type: application/x-www-form-urlencoded**。
* 如果收到錯誤碼 *BXNMS0122E: User credentials are invalid*，請確認您使用有效的 {{site.data.keyword.IBM_notm}} ID。


