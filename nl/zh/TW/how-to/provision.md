---

copyright:
  years: 2017, 2018

lastupdated: "2018-02-01"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}



# 佈建監視服務
{: #provision}

您可以從 {{site.data.keyword.Bluemix}} 使用者介面或從指令行，佈建 {{site.data.keyword.monitoringshort}} 服務。
{:shortdesc}


## 從使用者介面佈建
{: #ui}

完成下列步驟，以在 {{site.data.keyword.Bluemix_notm}} 中佈建 {{site.data.keyword.monitoringshort}} 服務的實例：

1. 登入 {{site.data.keyword.Bluemix_notm}} 帳戶。

    {{site.data.keyword.Bluemix_notm}} 儀表板可以在下列網址找到：[http://bluemix.net ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://bluemix.net){:new_window}。
    
	使用您的使用者 ID 和密碼登入之後，{{site.data.keyword.Bluemix_notm}} 使用者介面隨即開啟。

2. 按一下**型錄**。{{site.data.keyword.Bluemix_notm}} 上可用的服務清單隨即開啟。

3. 選取 **DevOps** 種類，以過濾所顯示的服務清單。

4. 按一下 **Monitoring** 磚。

5. 選取服務方案。 

    * 若要收集和存取最多 45 天的度量值，以及定義警示規則（包括具有萬用字元的規則），請選取**超值**方案。 
	
	* 依預設，會設定**精簡**方案，它讓您能夠在您要佈建服務的空間中收集平台度量值，並且對於那些度量值有 15 天的保留期間。 

    如需服務方案的相關資訊，請參閱[服務方案](/docs/services/cloud-monitoring/monitoring_ov.html#plan)。
	
6. 按一下**建立**以在您登入的 {{site.data.keyword.Bluemix_notm}} 空間中佈建 {{site.data.keyword.monitoringshort}} 服務。
  
 

## 從 CLI 佈建
{: #cli}

完成下列步驟，以透過指令行在 {{site.data.keyword.Bluemix_notm}} 中佈建 {{site.data.keyword.monitoringshort}} 服務的實例：

1. [必要條件] 安裝 {{site.data.keyword.Bluemix_notm}} CLI。

   如需相關資訊，請參閱[安裝 {{site.data.keyword.Bluemix_notm}} CLI](/docs/cli/reference/bluemix_cli/download_cli.html#download_install)。
   
   如果已安裝 CLI，請繼續進行下一步。
    
2. 登入 {{site.data.keyword.Bluemix_notm}} 中的地區、組織及空間。 

    如需相關資訊，請參閱[如何登入 {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#login)。
	
3. 執行 `bx cf create-service` 指令以佈建實例。

	```
	bx cf create-service service_name service_plan service_instance_name
	```
	{: codeblock}
	
	其中
	
	* service_name 是 **Monitoring**。
	* service_plan 是服務方案名稱。若要收集和存取最多 45 天的度量值，以及定義警示規則（包括具有萬用字元的規則），請選取**超值**方案。依預設，會設定**精簡**方案，它讓您能夠在您要佈建服務的空間中收集平台度量值，並且對於那些度量值有 15 天的保留期間。如需方案清單，請參閱 [{{site.data.keyword.monitoringshort}} 服務方案](/docs/services/cloud-monitoring/monitoring_ov.html#plan)。
	* service_instance_name 是您要用於所建立之新服務實例的名稱。
	
	如需 bx cf 指令的相關資訊，請參閱 [cf create-service](/docs/cli/reference/cfcommands/index.html#cf_create-service)。

	例如，若要建立具有超值方案的 {{site.data.keyword.monitoringshort}} 服務實例，請執行下列指令：
	
	```
	cf create-service Monitoring premium my_monitoring_svc
	```
	{: codeblock}
	
4. 驗證已順利建立服務。執行下列指令：

	```	
	cf services
	```
	{: codeblock}
	
	執行指令的輸出如下所示：
	
	```
    Getting services in org MyOrg / space MySpace as xxx@yyy.com...
    OK
    
    name                           service                  plan                   bound apps              last operation
    my_monitoring_svc              Monitoring               premium                                        create succeeded
	```
	{: screen}

	



