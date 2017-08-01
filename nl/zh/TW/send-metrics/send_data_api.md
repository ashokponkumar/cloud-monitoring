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


# 將資料傳送至監視服務
{: #send_data_api}

您可以使用「度量值 API」，將度量值傳送至 {{site.data.keyword.monitoringshort}} 服務。也可以將度量值傳送至 {{site.data.keyword.Bluemix_notm}} 空間。
{:shortdesc}


若為 {{site.data.keyword.Bluemix_notm}} Docker 容器，會自動收集基本系統度量值。若為 Cloud Foundry 應用程式及「虛擬機器 (VM)」中執行的應用程式，則必須使用[度量值 API](https://console.bluemix.net/apidocs/927-ibm-cloud-monitoring-rest-api?&language=node#introduction){: new_window}，直接從應用程式傳送度量值。 



## 使用 UAA 將度量值傳送至空間
{: #uaa}

若要將度量值傳送至 {{site.data.keyword.Bluemix_notm}} 空間，請完成下列步驟：

1. 登入 {{site.data.keyword.Bluemix_notm}} 地區、組織及空間。執行下列指令：

    例如，若要登入「美國南部」地區，請執行下列指令：
	
    ```
    cf login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    遵循指示。輸入您的 {{site.data.keyword.Bluemix_notm}} 認證，選取組織及空間。

2. 取得 UAA 鑑別記號。

	如需 UAA 鑑別的相關資訊，請參閱[使用 Bluemix CLI 取得 UAA 記號](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli)或[使用 REST API 取得 UAA 記號](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api)。

	例如，若要使用 UAA 記號，請執行下列指令：

	```
	cf oauth-token
	```
	{: codeblock}
	
	這個指令的結果如下：
	
	```
	bearer eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8
	```
	{: screen}
	
	然後，匯出變數 *Token*。例如：
	
	```
	export Token="eyJhbGciOiJI....cGlxryWS8"
	```
	{: screen}
	
	**附註：**記號會排除 *Bearer*。
		
3. 取得空間 GUID。GUID 必須加上字首 *s-*，以識別空間。

    執行下列指令：
	
	```
	cf space SpaceName --guid
	```
	{: codeblock}
	
	其中 *SpaceName* 是空間名稱，您將在該空間裡定義通知。
	
	例如，若要取得名為 *dev* 的空間 GUID，請執行下列指令：
	
	```
	cf space dev --guid
	```
	{: screen}
	
	這個指令的結果如下：
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	然後，匯出變數 *Space*。例如：
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. 執行下列 cURL 指令，以傳送度量值：

	```
	curl -XPOST -d @Metric_File --header "X-Auth-User-Token:uaa ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics
	```
	{: codeblock}
	
	其中
	
	* Metrics_File 代表 JSON 檔案，其中包括傳送至 {{site.data.keyword.monitoringshort}} 服務的度量值定義。
	
	* *X-Auth-User-Token* 是傳遞 {{site.data.keyword.Bluemix_notm}} UAA 記號的參數。
	
	* *Auth_Type* 是定義記號類型的字首。*uaa* 識別所指定的記號是 UAA 產生的記號。
	
	* *X-Auth-Scope-Id* 參數會傳遞空間 GUID。GUID 必須加上字首 *s-*，以識別空間。
	
	* Token 代表 UAA 記號。
	
	* Space 代表空間的 GUID。 
	
	例如，您可以執行下列指令，將 2 個度量值（`myhost.cpu.idle` 及 `myapp.login.attempts`）傳送至 {{site.data.keyword.monitoringshort}} 服務：
	
	```
	curl -XPOST @metric.json --header "X-Auth-User-Token:uaa ${Token}" https://metrics.ng.bluemix.net/v1/metrics
	```
	{: screen}
	
	範例檔 *metric.json* 定義如下：

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

 
## 使用 IAM 或 API 金鑰將度量值傳送至空間
{: #iam}

若要將度量值傳送至 {{site.data.keyword.Bluemix_notm}} 空間，請完成下列步驟：

1. 登入 {{site.data.keyword.Bluemix_notm}} 地區、組織及空間。執行下列指令：

    例如，若要登入「美國南部」地區，請執行下列指令：
	
    ```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    遵循指示。輸入您的 {{site.data.keyword.Bluemix_notm}} 認證，選取組織及空間。

2. 取得鑑別記號或 API 金鑰。

	如需 IAM 鑑別的相關資訊，請參閱[使用 Bluemix CLI 取得 IAM 記號](/docs/services/cloud-monitoring/security/auth_iam.html#iam_cli)或[使用 Bluemix CLI 產生 IAM API 金鑰](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli)。

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
	
	然後，匯出變數 *Token*。例如：
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**附註：**記號會排除 *Bearer*。
		
3. 取得空間 GUID。

    若要取得空間 GUID，請參閱[如何取得空間的 GUID](/docs/service/cloud-monitoring/qa/cli_qa.html#space_guid)。GUID 必須加上字首 *s-*，以識別空間。
    
    例如，執行下列指令：
	
	```
	    bx iam space NAME --guid
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
	
	然後，匯出變數 *Space*。例如：
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. 執行 cURL 指令來傳送度量值。

	```
	curl -XPOST -d @Metric_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics
	```
	{: codeblock}
	
	其中
	
	* Metrics_File 代表 JSON 檔案，其中包括傳送至 {{site.data.keyword.monitoringshort}} 服務的度量值定義。
	
	* *X-Auth-User-Token* 是傳遞 {{site.data.keyword.Bluemix_notm}} IAM 記號或 API 金鑰的參數。
	
	* *Auth_Type* 是定義記號或 API 金鑰類型的字首。下列清單概述有效值：*apikey*、*iam* 或 *uaa*，其中

        * *apikey* 識別該記號是 API 金鑰。
		* *iam* 識別所指定的記號是 IAM 產生的記號。
	
	* *X-Auth-Scope-Id* 參數會傳遞空間 GUID。GUID 必須加上字首 *s-*，以識別空間。 
	
	* Token 代表 IAM 記號或 API 金鑰。
	
	* Space 代表空間的 GUID。 

	
例如，您可以執行下列指令，將 two 個度量值（`myhost.cpu.idle` 及 `myapp.login.retries`）傳送至 {{site.data.keyword.monitoringshort}} 服務中的空間：
	
```
curl -XPOST @metric.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/metrics
```
{: screen}
	
範例檔 *metric.json* 定義如下：

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


















 
