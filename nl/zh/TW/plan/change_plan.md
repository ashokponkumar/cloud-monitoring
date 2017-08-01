---

copyright:
  years: 2017
lastupdated: "2017-07-10"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 變更方案
{: #change_plan}

您可以在 {{site.data.keyword.Bluemix}} 的服務「儀表板」中或透過執行 `cf update-service` 指令，來變更 {{site.data.keyword.monitoringshort}} 服務方案。您隨時可以升級或降低方案。
{:shortdesc}

## 透過使用者介面變更服務方案
{: #change_plan_ui}

若要在 {{site.data.keyword.Bluemix_notm}} 的服務儀表板中變更服務方案，請完成下列步驟：

1. 登入 {{site.data.keyword.Bluemix_notm}}，然後從 {{site.data.keyword.Bluemix_notm}} 儀表板按一下 {{site.data.keyword.monitoringshort}} 服務。 
    
2. 在 {{site.data.keyword.Bluemix_notm}} 使用者介面中，選取**方案**標籤。

    即會顯示現行方案的相關資訊。
	
3. 選取新方案以升級或降低您的方案。 

4. 按一下**儲存**。



## 透過 CLI 變更服務方案
{: #change_plan_cli}

若要在 {{site.data.keyword.Bluemix_notm}} 中透過 CLI 變更服務方案，請完成下列步驟：

1. 1. 登入 {{site.data.keyword.Bluemix_notm}} 地區、組織及空間。執行下列指令：

    ```
    cf login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}
	
2. 執行 `cf services` 指令來檢查現行方案，以及從空間中的可用服務清單取得 {{site.data.keyword.loganalysisshort}} 服務名稱。 

    **name** 欄位的值就是您必須用來變更方案的值。 

    例如，
	
	```
	$ cf services
	Getting services in org MyOrg / space dev as xxx@yyy.com...
	OK
	name            service      plan   bound apps   last operation
	Monitoring-0c   Monitoring   premium             create succeeded
	```
	{: screen}
    
3. 升級或降低您的方案。執行 `cf update-service` 指令。
    
	```
	cf update-service service_name [-p new_plan]
	```
	{: codeblock}
	
	其中 
	
	* *service_name* 是服務的名稱。您可以執行 `cf services` 指令來取得值。
	* *new_plan* 是方案的名稱。
	
	下表列出不同的方案及其支援的值：
	
	<table>
	  <caption>表 1. 方案清單。</caption>
	  <tr>
	    <th>方案</th>
		<th>特性</th>
	    <th>名稱</th>
	  </tr>
	  <tr>
	    <td>精簡</td>
	    <td>免費監視。</td>
		<td>lite</td>
	  </tr>
	  <tr>
	    <td>進階</td>
	    <td>進階監視。</td>
		<td>premium</td>
	  </tr>
	</table>
	
	例如，若要將您的方案降低為*精簡* 方案，請執行下列指令：
	
	```
	cf update-service "Monitoring-0c" -p lite
    Updating service instance Monitoring-0c as xxx@yyy.com...
    OK
	```
	{: screen}

4. 驗證已變更新方案。執行 `cf services` 指令。

	```
	cf services
    Getting services in org MyOrg / space dev as xxx@yyy.com...
    OK

    name              service       plan   bound apps   last operation
    Monitoring-0c     Monitoring    lite                create succeeded
	```
	{: screen}






