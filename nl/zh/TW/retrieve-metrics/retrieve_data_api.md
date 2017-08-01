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


# 從監視服務擷取資料
{: #retrieve_data_api}

您可以使用[度量值 API](https://console.bluemix.net/apidocs/927-ibm-cloud-monitoring-rest-api?&language=node#introduction){: new_window}，從 {{site.data.keyword.monitoringshort}} 服務擷取度量值。您可以從 {{site.data.keyword.Bluemix_notm}} 空間擷取度量值。
{:shortdesc}


若要定義您計劃擷取的資料集，請考量下列資訊： 

* 您必須設定您要從中擷取資料的 {{site.data.keyword.Bluemix_notm}} 空間。

* 您必須提供記號或 API 金鑰，以便使用 {{site.data.keyword.monitoringshort}} 服務。 

* 您必須指定一個以上度量值的路徑。如需相關資訊，請參閱[定義度量值](#metrics)。

* 您可以選擇性地指定自訂的時段。依預設，如果沒有指定時段，則您擷取的資料是對應於過去 24 小時的資料。如需相關資訊，請參閱[配置時段](#time)。

**附註：**每個要求最多可以擷取 5 個目標。


## 定義度量值
{: #metrics}

若要擷取度量值的資料，您必須指定度量值的路徑（從度量值樹狀結構根目錄到度量值），而且必須以句號 (.) 區隔每個步驟。

路徑指定在 **Target** 參數中。每個要求最多可以指定 5 個目標。  

您可以選擇性地將函數套用至度量值。如需所支援函數的相關資訊，請參閱 [Graphite 0.9.15 函數 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://graphite.readthedocs.io/en/latest/functions.html "外部鏈結圖示")。

您可以使用萬用字元來定義路徑。藉由使用萬用字元，您可以用單一路徑識別多個度量值。 

* **星號**：您可以使用星號 (*) 以符合零個以上的字元。 

    在單一路徑元素內可以有多個星號，例如：`servers.sp*aeg*r.cpu.total.*` 這個範例會傳回符合型樣之所有伺服器的 CPU 總計相關資料。

* **字元清單或範圍**：您可以在方括弧 ([...]) 中定義字元，以指定路徑字串中的單一字元位置。 

    您可以指定以連字號 (-) 區隔的兩個字元，來設定字元內含範圍。這兩個字元之間的任何字元都會符合。 
	
	您可以在方括弧內定義一個以上的字元範圍。 
	
	當一組方括弧內的字元無法當作範圍來讀取時，會將字元視為值的清單。 
	
	您可以包含連字號 (-) 作為值清單中的字元，方法是將它放在方括弧內的開始或結束處。

* **值清單**：您可以在大括弧內設定以逗點區隔的值，以定義值清單。 

    例如，若要下載下列路徑的度量值：`servers.myserver.cpu.total.user` 和 `servers.myserver.cpu.total.system`，您可以為這兩種類型設定單一路徑：`servers.myserver.cpu.total.{user,system}`



## 設定時段
{: #time}

您可以使用相對時間或絕對時間，選擇性地指定時段。您必須定義查詢參數 **From** 和 **Until**。  

* 如果省略時段的開始時間，預設值是 24 小時前。 

* 如果省略時段的結束時間，預設值是現行時間 *now*。

**相對時間**一律會在前面加上減號 (-)，後面接著一個時間單位。 

下表列出您可以使用的不同相對時間縮寫：


| 縮寫| 說明|
|--------------|-------------|
| s            | 秒|
| min          | 分鐘|
| h            | 小時|
| d            | 天|
| w            | 週|
| mon          | 月|
| now          | 現行時間|


您可以使用下列任何格式來定義**絕對時間**：`HH:MM_YYMMDD`、`YYYYMMDD`、`MM/DD/YY`

| 縮寫         | 說明        |
|--------------|-------------|
| YYYY         | 四位數年份|
| MM           | 二位數月份（01=一月，依此類推）|
| DD           | 二位數日期（01 到 31）|
| hh           | 二位數的小時（00 到 23）|
| mm           | 二位數的分鐘（00 到 59）|
| ss           | 二位數的秒鐘（00 到 59）|

**範例**

下表顯示關於如何設定 *from* 和 *until* 參數以設定不同時段的不同範例：

<table>
  <caption>表 1. 顯示如何設定 *from* 和 *until* 參數以設定不同時段的不同範例</caption>
  <tr>
    <th>範例</th>
	<th>說明</th>
  </tr>
  <tr>
    <td>&from=monday</td>
	<td>包括從上星期一到目前為止之資料的時段。</td>
  </tr>
  <tr>
    <td>&from=20171101&until=20171130</td>
	<td>設為月份的時段，例如 11 月</td>
  </tr>
  <tr>
    <td>&from=02:00_20170701&until=14:00_20170701</td>
	<td>以 24 小時週期顯示資料，例如 2017 年 7 月 1 日從 02:00 至 14:00</td>
  </tr>
  <tr>
    <td>&from=-8d&until=-7d</td>
	<td>顯示當週的同一天，例如上週</td>
  </tr>
  
</table>


## 設定空間
{: #domain}

當您使用 {{site.data.keyword.Bluemix_notm}} UAA 鑑別模型或 {{site.data.keyword.Bluemix_notm}} IAM 鑑別模型來向 {{site.data.keyword.monitoringshort}} 服務進行鑑別時，**X-Auth-Scope-Id** 標頭欄位是必要項目，而且必須設為空間 GUID。GUID 必須加上字首 *s-*，以識別空間。



## 設定授權記號或 API 金鑰
{: #security}
  
您必須設定 **X-Auth-User-Token** 欄位以定義用於鑑別的 UAA 記號、IAM 記號或 API 金鑰。 

記號或 API 金鑰必須加上下列其中一個值作為字首：`apikey`、`iam` 或 `uaa` 

其中 

* *apikey* 識別該記號是 API 金鑰。
* *iam* 識別所指定的記號是 IAM 產生的記號。
* *uaa* 識別所指定的記號是 UAA 產生的記號。

例如，您可以為 API 金鑰設定下列標頭：`X-Auth-User-Token: apikey SomeIAMGeneratedKey`


## 使用 UAA 從空間擷取度量值
{: #uaa}

若要從 {{site.data.keyword.Bluemix_notm}} 空間擷取度量值，請完成下列步驟：

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
	export Token="eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8"
	```
	{: screen}
	
	**附註：**記號會排除 *Bearer*。
		
3. 取得空間 GUID。GUID 必須加上字首 *s-*，以識別空間。

    執行下列指令：
	
	```
	cf space SpaceName --guid
	```
	{: codeblock}
	
	其中 *SpaceName* 是以 *s-* 為字首的空間名稱，您將在該空間中定義通知。
	
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
	curl -XGET --header "X-Auth-User-Token:uaa ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics?from=Start_Time&until=End_Time&target=string
	```
	{: codeblock}

	其中
	
	* *X-Auth-User-Token* 是傳遞 {{site.data.keyword.Bluemix_notm}} UAA 記號的參數。
	
	* *uaa* 是所指定的記號是 UAA 產生的記號。
	
	* *X-Auth-Scope-Id* 參數會傳遞空間 GUID。GUID 必須加上字首 *s-*，以識別空間。只有在使用 UAA 鑑別時，才需要此標頭。如果您使用 IAM 鑑別記號，則此標頭是選用項目，並且會使用連結至該記號的網域資訊。
	
	* *Token* 代表 UAA 記號。
	
	* *Space* 代表空間的 GUID。只有在您使用 UAA 記號時才需要它。
	
	* *Start_Time* 定義設定要求開始的相對或絕對時段。預設值為 24 小時前：`-24h`。*End_Time* 定義設定要求結束的相對或絕對時段。預設值為目前的現行時間：`now`。如需相關資訊，請參閱[設定時段](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#time)。
	
	* *Path* 識別一個或數個度量值。您可以選擇性地將函數套用至每個度量值。路徑也支援萬用字元，這樣可讓您以單一路徑識別多個度量值。如需相關資訊，請參閱[定義度量值](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#metrics)。
	
	
	
## 使用 IAM 或 API 金鑰從空間擷取度量值
{: #iam}

若要使用 IAM 或 API 金鑰，從 {{site.data.keyword.Bluemix_notm}} 空間擷取度量值，請完成下列步驟：

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
	
	其中 *SpaceName* 是以 *s-* 為字首的空間名稱，您將在該空間中定義通知。
	
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
	
5. 執行 cURL 指令來擷取度量值。

	```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics?from=Start_Time&until=End_Time&target=string
	```
	{: codeblock}

	其中
	
	* *X-Auth-User-Token* 是傳遞 {{site.data.keyword.Bluemix_notm}} IAM 記號或 API 金鑰的參數。
	
	* *Auth_Type* 是定義記號或 API 金鑰類型的字首。 

        * *apikey* 識別該記號是 API 金鑰。
		* *iam* 識別所指定的記號是 IAM 產生的記號。
	
	* *X-Auth-Scope-Id* 參數會傳遞空間 GUID。GUID 必須加上字首 *s-*，以識別空間。只有在使用 UAA 鑑別時，才需要此標頭。如果您使用 IAM 鑑別記號，則此標頭是選用項目，並且會使用連結至該記號的網域資訊。
	
	* *Token* 代表 UAA 記號。
	
	* *Space* 代表空間的 GUID。只有在您使用 UAA 記號時才需要它。
	
	* *Start_Time* 定義設定要求開始的相對或絕對時段。預設值為 24 小時前：`-24h`。*End_Time* 定義設定要求結束的相對或絕對時段。預設值為目前的現行時間：`now`。如需相關資訊，請參閱[設定時段](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#time)。
	
	* *Path* 識別一個或數個度量值。您可以選擇性地將函數套用至每個度量值。路徑也支援萬用字元，這樣可讓您以單一路徑識別多個度量值。如需相關資訊，請參閱[定義度量值](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#metrics)。






