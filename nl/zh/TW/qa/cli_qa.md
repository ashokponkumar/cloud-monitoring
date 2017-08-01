---

copyright:
  years: 2017

lastupdated: "2017-06-19"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# 使用 Bluemix CLI 時常見的問題與解答
{: #cli_qa}

以下是關於搭配使用 {{site.data.keyword.Bluemix}} CLI 與 {{site.data.keyword.monitoringshort}} 服務的常見問題與解答。
{:shortdesc}

* [如何安裝 {{site.data.keyword.Bluemix_notm}} CLI](#install_bmx_cli)
* [如何取得帳戶的 GUID](#account_guid)
* [如何取得組織的 GUID](#org_guid)
* [如何取得空間的 GUID](#space_guid)


## 如何安裝 Bluemix CLI？
{: #install_bmx_cli}

請完成下列步驟來安裝 {{site.data.keyword.Bluemix_notm}} CLI：

1. 下載 CLI。

    例如，若要在 Ubuntu 系統中安裝 {{site.data.keyword.Bluemix_notm}} CLI 套件，請下載 [{{site.data.keyword.Bluemix_notm}} CLI 套件 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://clis.ng.bluemix.net/ui/home.html "外部鏈結圖示"){: new_window}。 

2. 執行下列指令，以解壓縮 {{site.data.keyword.Bluemix_notm}} CLI 套件：
    
    ```
    tar -xvf Bluemix_CLI_0.5.4_amd64.tar.gz
    ```
    {: codeblock}
    
3. 移至 *Bluemix_CLI* 目錄，然後使用 root 許可權來執行 `./install_bluemix_cli` 指令，以安裝 CF 外掛程式。您可以使用 root 使用者身分執行此指令，或使用 sudo 指令來取得 root 許可權。例如：
    
    ```
    cd Bluemix_CLI
    ```
    {: codeblock}
    
    ```
    sudo ./install_bluemix_cli
    ```
    {: codeblock}
    
4. 驗證 CF 外掛程式的安裝。例如，檢查 CF 外掛程式的版本。執行下列指令：
    
    ```
    cf -v
    ```
    {: codeblock}
    
    輸出如下所示：
    
    ```
    cf version 6.25.0+787326d.2017-02-28
    ```
    {: screen}
    
5. 驗證 {{site.data.keyword.Bluemix_notm}} CLI 的安裝。例如，檢查外掛程式的版本。執行下列指令：
    
    ```
    bx -version
    ```
    {: codeblock}
    
    輸出如下所示：
    
    ```
    bx version 0.5.4+ae22935-2017-05-18T03:55:55+00:00
    ```
    {: screen}
	
## 如何取得帳戶的 GUID
{: #account_guid}
	
請完成下列步驟，以取得帳戶的 GUID：
	
1. 登入 {{site.data.keyword.Bluemix_notm}} 地區、組織及空間。執行下列指令：

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    遵循指示。輸入您的 {{site.data.keyword.Bluemix_notm}} 認證，選取組織及空間。
	
2. 執行 `bx iam accounts` 指令，以取得帳戶的 GUID。

	```
	bx iam accounts
	```
	{: codeblock} 
	
	例如，若要擷取使用者 xxx@yyy.com 的所有帳戶及其對應 GUID，請執行下列指令：
	
	```
	bx iam accounts
	Retrieving all accounts of xxx@yyy.com...
    OK
    Account GUID                       Name                               Type    State    Owner User ID   
    12345123451234512345123451234512   A Account                          TRIAL   ACTIVE   xxx@yyy.com   
    23456234562345622456234561234561   B Account                          TRIAL   ACTIVE   zzz@yyy.com   
	```
	{: screen}

	
## 如何取得組織的 GUID
{: #org_guid}

請完成下列步驟，以取得組織的 GUID：
	
1. 登入 {{site.data.keyword.Bluemix_notm}} 地區、組織及空間。執行下列指令：

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    遵循指示。輸入您的 {{site.data.keyword.Bluemix_notm}} 認證，選取組織及空間。

2. 執行 `bx iam org` 指令，以取得組織 GUID。 

    ```
    bx iam org NAME --guid
    ```
    {: codeblock}
	
    其中，NAME 是 {{site.data.keyword.Bluemix_notm}} 組織的名稱。        
		
## 如何取得空間的 GUID
{: #space_guid}
	
請完成下列步驟，以取得空間的 GUID：
	
1. 登入 {{site.data.keyword.Bluemix_notm}} 地區、組織及空間。執行下列指令：

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    遵循指示。輸入您的 {{site.data.keyword.Bluemix_notm}} 認證，選取組織及空間。
	
2. 執行 `bx iam space` 指令，以取得空間 GUID。 

    ```
    bx iam space NAME --guid
    ```
    {: codeblock}
	
    其中，NAME 是 {{site.data.keyword.Bluemix_notm}} 空間的名稱。 
	
    例如，若要取得空間 *dev* 的 GUID，請執行下列指令：
	
    ```
    bx iam space dev --guid
    e03afff1-3456-4af6-1234-59treg1b0abc
    ```
    {: screen}




		
		
