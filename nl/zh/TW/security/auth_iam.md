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


# 使用 Bluemix IAM 模型進行鑑別
{: #auth_iam}

使用 {{site.data.keyword.Bluemix}} IAM 模型來取得鑑別記號，您可以用來存取 {{site.data.keyword.monitoringshort}} 服務中儲存的度量值，或用來取得 API 金鑰。記號具有有效期限。API 金鑰不會到期。
{:shortdesc}


## 使用 Bluemix CLI 取得 IAM 記號 
{: #iam_token_cli}

若要使用 {{site.data.keyword.Bluemix_notm}} CLI 取得授權記號，請完成下列步驟：

1. 安裝 {{site.data.keyword.Bluemix_notm}} CLI。

   如需相關資訊，請參閱[安裝 {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa)。
   
   如果已安裝 CLI，請繼續進行下一步。
    
2. 登入 {{site.data.keyword.Bluemix_notm}} 地區、組織及空間。執行下列指令：

    例如，針對「美國南部」地區，請執行下列指令：
	
    ```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    遵循指示。輸入您的 {{site.data.keyword.Bluemix_notm}} 認證，選取組織及空間。
	
3. 執行 `bx iam oauth-tokens` 指令，以取得 IAM 記號。

	```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	輸出會傳回 IAM 記號，您必須用來在該空間及組織中鑑別您的使用者 ID。

**附註：**當您使用記號時，請從您在 API 呼叫中傳遞的記號值移除 *Bearer*。
		
		
## 使用 Bluemix CLI 產生 IAM API 金鑰
{: #iam_apikey_cli}

請完成下列步驟，使用 {{site.data.keyword.Bluemix_notm}} CLI 產生 IAM API 金鑰：

1. 安裝 {{site.data.keyword.Bluemix_notm}} CLI。

   如需相關資訊，請參閱[安裝 {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa)。
   
   如果已安裝 CLI，請繼續進行下一步。
	
2. 使用 `bx login` {{site.data.keyword.Bluemix_notm}} CLI 指令，登入 {{site.data.keyword.Bluemix_notm}}：

    例如，針對「美國南部」地區，請執行下列指令：
	
    ```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    遵循指示。輸入您的 {{site.data.keyword.Bluemix_notm}} 認證，選取組織及空間。
 
3. 執行 `bx iam api-key-create` 指令，以建立 IAM API 金鑰。

	```
	    bx iam api-key-create NAME [-d DESCRIPTION] [-f, --file FILE]
	```
	{: codeblock} 
	
	其中
	
	* NAME 是要建立之 API 金鑰的名稱。
	* DESCRIPTION 是用來說明 API 金鑰的文字。
	* FILE 是儲存金鑰的檔案名稱。
	
    例如，若要建立 API 金鑰 *MyKey*，請執行下列指令：
	
	```
	bx iam api-key-create MyKey -d "This is my API key for service X" 
	```
	{: screen}
	
	
	
	
## 使用 Bluemix 使用者介面產生 IAM API 金鑰
{: #iam_apikey_ui}

請完成下列步驟，透過 {{site.data.keyword.Bluemix_notm}} 使用者介面產生 IAM API 金鑰：

1. 登入 {{site.data.keyword.Bluemix_notm}} 主控台。 

2. 從主控台功能表列中，按一下**管理 > 安全 > Bluemix API 金鑰**。

3. 按一下**建立 API 金鑰**。

4. 輸入 API 金鑰的名稱及說明。然後，按一下**建立 API 金鑰**。

5. 儲存 API 金鑰。按一下**下載 API 金鑰**。

    按一下**顯示**，以顯示 API 金鑰。  

**附註：**只能在建立時顯示或下載 API 金鑰。如果 API 金鑰遺失，您必須建立新的 API 金鑰。  


	
## 使用 Bluemix 使用者介面撤銷 API 金鑰
{: #revoke_ui}
	
請完成下列步驟，透過 {{site.data.keyword.Bluemix_notm}} 使用者介面撤銷 IAM API 金鑰：

1. 登入 {{site.data.keyword.Bluemix_notm}} 主控台。

2. 從主控台功能表列中，按一下**管理 > 安全 > Bluemix API 金鑰**。

3. 按一下**動作**，然後按一下**刪除金鑰**。





	

	
	
	
	
	
	
