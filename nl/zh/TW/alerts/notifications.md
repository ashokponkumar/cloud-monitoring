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


# 使用警示 API 來使用通知
{: #notifications}

使用「警示 API」來建立、刪除及更新通知，以顯示通知的詳細資料，以及列出 {{site.data.keyword.Bluemix_notm}} 空間中所定義的通知。
{:shortdesc}

## 建立通知範本
{: #template}

通知是一個 JSON 檔案。 

您可以建立任意數目的通知範本，然後重複使用它們，以在組織中建立該類型的通知。 

您可以定義下列任何類型的通知：

* Email：定義 *Email* 類型的通知，以將電子郵件傳送至有效的電子郵件位址。 
* Webhook：僅針對 HTTPS 端點定義 *Webhook* 類型的通知。將參數新增至端點，以協助減少其他人嘗試呼叫您的端點的機會。
* Pagerduty：定義 *PagerDuty* 類型的通知，以將度量值的警示資料傳送至 PagerDuty 突發事件管理系統。 

例如，下表列出通知範本的範例：

<table>
  <caption></caption>
  <tr>
    <th>類型</th>
	<th>範本</th>
	<th>範例</th>
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

* *Template_Name* 定義通知範本的名稱。
* *Description* 說明此通知類型的使用時機。
* *EmailAddress* 定義通知收件者的電子郵件位址。
* *Endpoint* 定義應該進行 POST 的 URL。這是可以接受 POST 要求的 URL。 
* *Pagerduty_APIkey* 定義一個唯一的 API 金鑰。此 API 金鑰是由 PagerDuty 帳戶管理者或擁有者所產生。


請完成下列步驟來建立通知範本：

1. 建立目錄（例如 *cloud-monitoring*）來儲存 {{site.data.keyword.monitoringshort}} 服務資源。

    例如，在 Ubuntu 系統中，執行下列指令：
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
2. 建立目錄（例如 *notification-templates*）來儲存通知範本。

    例如，在 Ubuntu 系統中，執行下列指令：
	
	```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	切換至此目錄：
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
3. 在系統的本端目錄中建立通知範本。

    例如，使用 vi 編輯器建立電子郵件通知範本的 **email-template.json** 檔案： 
	
	```
	{
    "name": "email_template",
    "type": "Email",
    "description" : "Send email to manager of department X when ....",
    "detail": "xxx@yyy"
    }
	```
	{: codeblock}
	


## 建立通知
{: #create}

若要建立通知，請完成下列步驟：

1. 建立通知檔案。

    1. 使用通知範本來建立檔案。如需相關資訊，請參閱[建立通知範本](#template)。
	
	2. 更新檔案：
	
	    * 在 *name* 欄位中輸入唯一名稱。此欄位的值稍後會用於更新、刪除及顯示通知的詳細資料。
		* 輸入說明。
		* 輸入有效的電子郵件位址、URL 端點或 PagerDuty 金鑰。
		
	3. 儲存檔案。例如，請對您為 {{site.data.keyword.Bluemix_notm}} 空間中所執行查詢定義的通知，使用 *s-* 之類的字首。
	
	    **提示：**請在下列目錄中建立通知檔案：*~/cloud-monitoring/notifications/*，以將 {{site.data.keyword.monitoringshort_notm}} 服務的資源分組。 
	
2. 登入 {{site.data.keyword.Bluemix_notm}} 地區、組織及空間。執行下列指令：

    例如，若要登入「美國南部」地區，請執行下列指令：
	
    ```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    遵循指示。輸入您的 {{site.data.keyword.Bluemix_notm}} 認證，選取組織及空間。

3. 取得鑑別記號或 API 金鑰。

	* 若為 IAM 鑑別，請參閱[使用 Bluemix CLI 取得 IAM 記號](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli)或[使用 Bluemix CLI 產生 IAM API 金鑰](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli)。
	
	* 若為 UAA 鑑別，請參閱[使用 Bluemix CLI 取得 UAA 記號](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli)或[使用 REST API 取得 UAA 記號](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api)。

	例如，若要使用 IAM 記號，請執行下列指令：

	```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	這個指令的結果如下：
	
	```
	IAM token:  Bearer djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ
    UAA token:  Bearer eyJhbGciOiJIUz..Ky6vagp3k_QcIcKJ-td83qXhO5Uze43KcplG6PzcGs8
	```
	{: screen}
	
	然後，匯出變數 *Token*：
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**附註：**記號會排除 *Bearer*。
		
4. 取得空間 GUID。GUID 必須加上字首 *s-*，以識別空間。

    執行下列指令：
	
	```
	bx iam space SpaceName --guid
	```
	{: codeblock}
	
	其中 *SpaceName* 是空間名稱，您將在該空間裡定義通知。
	
	例如，若要取得名為 *dev* 的空間 GUID，請執行下列指令：
	
	```
	bx iam space dev --guid
	```
	{: screen}
	
	這個指令的結果如下：
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	然後，匯出變數 *Space*：
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. 執行下列 cURL 指令，以建立通知：

	```
	curl -XPOST -d @Notification_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: codeblock}
	
	其中
	
	* Notification_File 是定義通知的 JSON 檔案。
	
	* *X-Auth-User-Token* 參數會傳遞 {{site.data.keyword.Bluemix_notm}} UAA 記號、IAM 記號或 API 金鑰。
	
	* *Auth_Type* 是定義記號或 API 金鑰類型的字首。下列清單概述有效值：*apikey*、*iam* 或 *uaa*，其中

        * *apikey* 識別該記號是 API 金鑰。
		* *iam* 識別所指定的記號是 IAM 產生的記號。
		* *uaa* 識別所指定的記號是 UAA 產生的記號。
	
	* *X-Auth-Scope-Id* 參數會傳遞空間 GUID。GUID 必須加上字首 *s-*，以識別空間。如果您使用 UAA 記號鑑別，此標頭為必要項目。如果您使用 IAM 鑑別記號，則此標頭是選用項目，並且會使用連結至該記號的網域資訊。
	
	* Token 是 UAA 記號、IAM 記號或 API 金鑰。
	
	* Space 是空間的 GUID。只有在您使用 UAA 記號時才需要它。
	
    例如， 	
	
	```
	curl -XPOST -d @s-email-dep-A.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.{DomainName}/v1/alert/notification
	```
	{: screen}

## 刪除通知
{:#delete}

若要刪除通知，請完成下列步驟：

1. 登入 {{site.data.keyword.Bluemix_notm}} 地區、組織及空間。執行下列指令：

    例如，若要登入「美國南部」地區，請執行下列指令：
	
    ```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    遵循指示。輸入您的 {{site.data.keyword.Bluemix_notm}} 認證，選取組織及空間。

2. 取得鑑別記號或 API 金鑰。

	* 若為 IAM 鑑別，請參閱[使用 Bluemix CLI 取得 IAM 記號](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli)或[使用 Bluemix CLI 產生 IAM API 金鑰](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli)。
	
	* 若為 UAA 鑑別，請參閱[使用 Bluemix CLI 取得 UAA 記號](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli)或[使用 REST API 取得 UAA 記號](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api)。

	例如，若要使用 IAM 記號，請執行下列指令：

	```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	這個指令的結果如下：
	
	```
	IAM token:  Bearer djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ
    UAA token:  Bearer eyJhbGciOiJIUz..Ky6vagp3k_QcIcKJ-td83qXhO5Uze43KcplG6PzcGs8
	```
	{: screen}
	
	然後，匯出變數 *Token*：
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**附註：**記號會排除 *Bearer*。
	
3. 取得空間 GUID。GUID 必須加上字首 *s-*，以識別空間。

    執行下列指令：
	
	```
	bx iam space SpaceName --guid
	```
	{: codeblock}
	
	其中 *SpaceName* 是空間名稱，您將在該空間裡定義通知。
	
	例如，若要取得名為 *dev* 的空間 GUID，請執行下列指令：
	
	```
	bx iam space dev --guid
	```
	{: screen}
	
	這個指令的結果如下：
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	然後，匯出變數 *Space*：
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. 執行下列 cURL 指令，以刪除通知：

	```
	curl -XDELETE --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification/Notification_Name
	```
	{: codeblock}
	
	其中
	
	* *Auth_Type* 是定義記號或 API 金鑰類型的字首。下列清單概述有效值：*apikey*、*iam* 或 *uaa*，其中

        * *apikey* 識別該記號是 API 金鑰。
		* *iam* 識別所指定的記號是 IAM 產生的記號。
		* *uaa* 識別所指定的記號是 UAA 產生的記號。
	
	* *X-Auth-User-Token* 參數會傳遞 {{site.data.keyword.Bluemix_notm}} UAA 記號、IAM 記號或 API 金鑰。記號或 API 金鑰必須加上下列其中一個值作為字首：*apikey*、*iam* 或 *uaa*，其中

        * *apikey* 識別該記號是 API 金鑰。
		* *iam* 識別所指定的記號是 IAM 產生的記號。
		* *uaa* 識別所指定的記號是 UAA 產生的記號。
		
	* Token 是 UAA 記號、IAM 記號或 API 金鑰。
	
	* Space 是空間的 GUID。只有在您使用 UAA 記號時才需要它。
	
	* Notification_Name 是通知的名稱，亦即當您列出通知內容時的 *name* 欄位值。
	


## 列出所有通知
{: #list}

若要列出所有通知，請完成下列步驟：

1. 登入 {{site.data.keyword.Bluemix_notm}} 地區、組織及空間。執行下列指令：

    例如，若要登入「美國南部」地區，請執行下列指令：
	
    ```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    遵循指示。輸入您的 {{site.data.keyword.Bluemix_notm}} 認證，選取組織及空間。

2. 取得鑑別記號或 API 金鑰。

	* 若為 IAM 鑑別，請參閱[使用 Bluemix CLI 取得 IAM 記號](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli)或[使用 Bluemix CLI 產生 IAM API 金鑰](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli)。
	
	* 若為 UAA 鑑別，請參閱[使用 Bluemix CLI 取得 UAA 記號](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli)或[使用 REST API 取得 UAA 記號](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api)。

	例如，若要使用 IAM 記號，請執行下列指令：

	```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	這個指令的結果如下：
	
	```
	IAM token:  Bearer djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ
    UAA token:  Bearer eyJhbGciOiJIUz..Ky6vagp3k_QcIcKJ-td83qXhO5Uze43KcplG6PzcGs8
	```
	{: screen}
	
	然後，匯出變數 *Token*：
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**附註：**記號會排除 *Bearer*。
	
3. 取得空間 GUID。GUID 必須加上字首 *s-*，以識別空間。

    執行下列指令：
	
	```
	bx iam space SpaceName --guid
	```
	{: codeblock}
	
	其中 *SpaceName* 是空間名稱，您將在該空間裡定義通知。
	
	例如，若要取得名為 *dev* 的空間 GUID，請執行下列指令：
	
	```
	bx iam space dev --guid
	```
	{: screen}
	
	這個指令的結果如下：
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	然後，匯出變數 *Space*：
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. 執行下列 cURL 指令，以列出所有通知：

	```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notifications
	```
	{: codeblock}
	
	其中
	
	* *Auth_Type* 是定義記號或 API 金鑰類型的字首。下列清單概述有效值：*apikey*、*iam* 或 *uaa*，其中

        * *apikey* 識別該記號是 API 金鑰。
		* *iam* 識別所指定的記號是 IAM 產生的記號。
		* *uaa* 識別所指定的記號是 UAA 產生的記號。
		
	* *X-Auth-User-Token* 參數會傳遞 {{site.data.keyword.Bluemix_notm}} UAA 記號、IAM 記號或 API 金鑰。記號或 API 金鑰必須加上下列其中一個值作為字首：*apikey*、*iam* 或 *uaa*，其中

        * *apikey* 識別該記號是 API 金鑰。
		* *iam* 識別所指定的記號是 IAM 產生的記號。
		* *uaa* 識別所指定的記號是 UAA 產生的記號。
		
	* Token 是 UAA 記號、IAM 記號或 API 金鑰。
	
	* Space 是空間的 GUID。只有在您使用 UAA 記號時才需要它。


			

## 顯示通知的詳細資料
{: #show}


若要顯示通知的相關資訊，請完成下列步驟：

1. 登入 {{site.data.keyword.Bluemix_notm}} 地區、組織及空間。執行下列指令：

    例如，若要登入「美國南部」地區，請執行下列指令：
	
    ```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    遵循指示。輸入您的 {{site.data.keyword.Bluemix_notm}} 認證，選取組織及空間。

2. 取得鑑別記號或 API 金鑰。

	* 若為 IAM 鑑別，請參閱[使用 Bluemix CLI 取得 IAM 記號](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli)或[使用 Bluemix CLI 產生 IAM API 金鑰](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli)。
	
	* 若為 UAA 鑑別，請參閱[使用 Bluemix CLI 取得 UAA 記號](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli)或[使用 REST API 取得 UAA 記號](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api)。

	例如，若要使用 IAM 記號，請執行下列指令：

	```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	這個指令的結果如下：
	
	```
	IAM token:  Bearer djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ
    UAA token:  Bearer eyJhbGciOiJIUz..Ky6vagp3k_QcIcKJ-td83qXhO5Uze43KcplG6PzcGs8
	```
	{: screen}
	
	然後，匯出變數 *Token*：
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**附註：**記號會排除 *Bearer*。
	
3. 取得空間 GUID。GUID 必須加上字首 *s-*，以識別空間。

    執行下列指令：
	
	```
	bx iam space SpaceName --guid
	```
	{: codeblock}
	
	其中 *SpaceName* 是空間名稱，您將在該空間裡定義通知。
	
	例如，若要取得名為 *dev* 的空間 GUID，請執行下列指令：
	
	```
	bx iam space dev --guid
	```
	{: screen}
	
	這個指令的結果如下：
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	然後，匯出變數 *Space*：
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. 執行下列 cURL 指令，以顯示通知的詳細資料：

	```
	curl -XGET --header "X-Auth-User-Token:uaa ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification/Notification_Name
	```
	{: codeblock}
	
	其中
	
	* *X-Auth-User-Token* 參數會傳遞 {{site.data.keyword.Bluemix_notm}} UAA 記號、IAM 記號或 API 金鑰。記號或 API 金鑰必須加上下列其中一個值作為字首：*apikey*、*iam* 或 *uaa*，其中

        * *apikey* 識別該記號是 API 金鑰。
		* *iam* 識別所指定的記號是 IAM 產生的記號。
		* *uaa* 識別所指定的記號是 UAA 產生的記號。
		
	* Token 是 UAA 記號、IAM 記號或 API 金鑰。
	
	* Space 是空間的 GUID。只有在您使用 UAA 記號時才需要它。
	
	* Notification_Name 是通知的名稱，亦即當您列出通知內容時的 *name* 欄位值。
	
     
		

			
## 測試通知
{: #test}	
			
若要測試通知，請完成下列步驟：

1. 登入 {{site.data.keyword.Bluemix_notm}} 地區、組織及空間。執行下列指令：

    例如，若要登入「美國南部」地區，請執行下列指令：
	
    ```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    遵循指示。輸入您的 {{site.data.keyword.Bluemix_notm}} 認證，選取組織及空間。

2. 取得鑑別記號或 API 金鑰。

	* 若為 IAM 鑑別，請參閱[使用 Bluemix CLI 取得 IAM 記號](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli)或[使用 Bluemix CLI 產生 IAM API 金鑰](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli)。
	
	* 若為 UAA 鑑別，請參閱[使用 Bluemix CLI 取得 UAA 記號](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli)或[使用 REST API 取得 UAA 記號](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api)。

	例如，若要使用 IAM 記號，請執行下列指令：

	```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	這個指令的結果如下：
	
	```
	IAM token:  Bearer djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ
    UAA token:  Bearer eyJhbGciOiJIUz..Ky6vagp3k_QcIcKJ-td83qXhO5Uze43KcplG6PzcGs8
	```
	{: screen}
	
	然後，匯出變數 *Token*：
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**附註：**記號會排除 *Bearer*。

3. 取得空間 GUID。GUID 必須加上字首 *s-*，以識別空間。

    執行下列指令：
	
	```
	bx iam space SpaceName --guid
	```
	{: codeblock}
	
	其中 *SpaceName* 是空間名稱，您將在該空間裡定義通知。
	
	例如，若要取得名為 *dev* 的空間 GUID，請執行下列指令：
	
	```
	bx iam space dev --guid
	```
	{: screen}
	
	這個指令的結果如下：
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	然後，匯出變數 *Space*：
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. 執行下列 cURL 指令，以測試通知：

	```
	curl -XPOST --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification/test/Notification_Name
	```
	{: codeblock}
	
	其中
	
	* *Auth_Type* 是定義記號或 API 金鑰類型的字首。下列清單概述有效值：*apikey*、*iam* 或 *uaa*，其中

        * *apikey* 識別該記號是 API 金鑰。
		* *iam* 識別所指定的記號是 IAM 產生的記號。
		* *uaa* 識別所指定的記號是 UAA 產生的記號。
	
	* *X-Auth-User-Token* 參數會傳遞 {{site.data.keyword.Bluemix_notm}} UAA 記號、IAM 記號或 API 金鑰。記號或 API 金鑰必須加上下列其中一個值作為字首：*apikey*、*iam* 或 *uaa*，其中

        * *apikey* 識別該記號是 API 金鑰。
		* *iam* 識別所指定的記號是 IAM 產生的記號。
		* *uaa* 識別所指定的記號是 UAA 產生的記號。
		
	* Token 是 UAA 記號、IAM 記號或 API 金鑰。
	
	* Space 是空間的 GUID。只有在您使用 UAA 記號時才需要它。
	
	* Notification_Name 是通知的名稱，亦即當您列出通知內容時的 *name* 欄位值。
	


## 更新通知
{: #update}

若要更新通知，請完成下列步驟：

1. 登入 {{site.data.keyword.Bluemix_notm}} 地區、組織及空間。執行下列指令：

    例如，若要登入「美國南部」地區，請執行下列指令：
	
    ```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    遵循指示。輸入您的 {{site.data.keyword.Bluemix_notm}} 認證，選取組織及空間。

2. 取得鑑別記號或 API 金鑰。

	* 若為 IAM 鑑別，請參閱[使用 Bluemix CLI 取得 IAM 記號](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli)或[使用 Bluemix CLI 產生 IAM API 金鑰](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli)。
	
	* 若為 UAA 鑑別，請參閱[使用 Bluemix CLI 取得 UAA 記號](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli)或[使用 REST API 取得 UAA 記號](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api)。

	例如，若要使用 IAM 記號，請執行下列指令：

	```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	這個指令的結果如下：
	
	```
	IAM token:  Bearer djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ
    UAA token:  Bearer eyJhbGciOiJIUz..Ky6vagp3k_QcIcKJ-td83qXhO5Uze43KcplG6PzcGs8
	```
	{: screen}
	
	然後，匯出變數 *Token*：
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**附註：**記號會排除 *Bearer*。
	
3. 取得空間 GUID。GUID 必須加上字首 *s-*，以識別空間。

    執行下列指令：
	
	```
	bx iam space SpaceName --guid
	```
	{: codeblock}
	
	其中 *SpaceName* 是空間名稱，您將在該空間裡定義通知。
	
	例如，若要取得名為 *dev* 的空間 GUID，請執行下列指令：
	
	```
	bx iam space dev --guid
	```
	{: screen}
	
	這個指令的結果如下：
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	然後，匯出變數 *Space*：
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. 執行下列 cURL 指令，以更新通知：

	```
	curl -XPUT -d @Notification_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: codeblock}
	
	其中
	
	* Notification_File 是定義通知的 JSON 檔案。
	
	* *Auth_Type* 是定義記號或 API 金鑰類型的字首。下列清單概述有效值：*apikey*、*iam* 或 *uaa*，其中

        * *apikey* 識別該記號是 API 金鑰。
		* *iam* 識別所指定的記號是 IAM 產生的記號。
		* *uaa* 識別所指定的記號是 UAA 產生的記號。
	
	* *X-Auth-User-Token* 參數會傳遞 Bluemix UAA 記號、IAM 記號或 API 金鑰。記號或 API 金鑰必須加上下列其中一個值作為字首：*apikey*、*iam* 或 *uaa*，其中

        * *apikey* 識別該記號是 API 金鑰。
		* *iam* 識別所指定的記號是 IAM 產生的記號。
		* *uaa* 識別所指定的記號是 UAA 產生的記號。
		
	* Token 是 UAA 記號、IAM 記號或 API 金鑰。
	
	* Space 是空間的 GUID。只有在您使用 UAA 記號時才需要它。

	
        

