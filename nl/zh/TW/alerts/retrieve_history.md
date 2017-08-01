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



# 擷取規則的歷程
{: #retrieve_history}


若要擷取警示的歷程，請完成下列步驟：

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
	
4. 執行下列 cURL 指令，以取得警示的歷程：

	```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/history?rule=Rule_Name
	```
	{: codeblock}
	
	其中
	
	* *X-Auth-User-Token* 參數會傳遞 {{site.data.keyword.Bluemix_notm}} UAA 記號、IAM 記號或 API 金鑰。
	
	* *Auth_Type* 是定義記號或 API 金鑰類型的字首。下列清單概述有效值：*apikey*、*iam* 或 *uaa*，其中

        * *apikey* 識別該記號是 API 金鑰。
		* *iam* 識別所指定的記號是 IAM 產生的記號。
		* *uaa* 識別所指定的記號是 UAA 產生的記號。
	
	* *X-Auth-Scope-Id* 參數會傳遞空間 GUID。GUID 必須加上字首 *s-*，以識別空間。如果您使用 UAA 記號鑑別，此標頭為必要項目。如果您使用 IAM 鑑別記號，則此標頭是選用項目，並且會使用連結至該記號的網域資訊。
	
	* Token 是 UAA 或 IAM 鑑別記號，或是 API 金鑰。
	
	* Space 是空間的 GUID。只有在您使用 UAA 記號時才需要它。
	
	* Rule_Name 是用來觸發警示的規則名稱。該值是指定在 *name* 欄位中的值。
	
    
例如，`highNginxCPU` 規則的歷程如下：

```
[
 {
 "rule": "highNginxCPU",
 "timestamp": "2017-04-17T22.23.21.000",
 "value": 99.5,
 "from_level": "OK",
 "to_level": "ERROR"
 },
 {
 "rule": "highNginxCPU",
 "timestamp": "2017-04-17T10.11.12.123",
 "value": 98.5,
 "from_level" : "OK",
 "to_level": "WARN"
 },
 {
 "rule": "highNginxCPU",
 "timestamp": "2017-04-17T10.12.14.000",
 "value": 97.0,
 "from_level" : "WARN",
 "to_level": "OK"
 }
]
```
{: screen}


