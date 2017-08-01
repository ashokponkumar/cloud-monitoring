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


# プランの変更
{: #change_plan}

{{site.data.keyword.monitoringshort}} サービス・プランは、{{site.data.keyword.Bluemix}} のサービス・ダッシュボードで変更するか、`cf update-service` コマンドを実行して変更できます。プランのアップグレード、またはライト・プランへの切り替えは、いつでも実行できます。{:shortdesc}

## UI を介したサービス・プランの変更
{: #change_plan_ui}

サービス・ダッシュボードで {{site.data.keyword.Bluemix_notm}} のサービス・プランを変更するには、以下の手順を実行します。

1. {{site.data.keyword.Bluemix_notm}} にログインし、{{site.data.keyword.Bluemix_notm}} ダッシュボードから {{site.data.keyword.monitoringshort}} サービスをクリックします。 
    
2. {{site.data.keyword.Bluemix_notm}} UI で**「プラン」**タブを選択します。

    現在のプランに関する情報が表示されます。
	
3. 新しいプランを選択して、プランをアップグレードするか、ライト・プランに切り替えます。 

4. **「保存」**をクリックします。



## CLI を介したサービス・プランの変更
{: #change_plan_cli}

CLI を介して {{site.data.keyword.Bluemix_notm}} のサービス・プランを変更するには、以下の手順を実行します。

1. 1. {{site.data.keyword.Bluemix_notm}} の地域、組織、およびスペースにログインします。以下のコマンドを実行します。

    ```
    cf login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}
	
2. `cf services` コマンドを実行して現在のプランを確認し、スペース内で使用可能なサービスのリストから {{site.data.keyword.loganalysisshort}} サービス名を取得します。 

    **name** フィールドの値は、プランを変更するために使用する必要がある値です。 

    以下に例を示します。
	
	```
	$ cf services
	Getting services in org MyOrg / space dev as xxx@yyy.com...
	OK
	name            service      plan   bound apps   last operation
	Monitoring-0c   Monitoring   premium             create succeeded
    ```
	{: screen}
    
3. プランをアップグレードするか、ライト・プランに切り替えます。`cf update-service` コマンドを実行します。
    
	```
	cf update-service service_name [-p new_plan]
	```
	{: codeblock}
	
	ここで、 
	
	* *service_name* はサービスの名前です。`cf services` コマンドを実行するとこの値を取得できます。
	* *new_plan* はプランの名前です。
	
	以下の表に、各プランと、サポートされる値をリストします。
	
	<table>
	  <caption>表 1. プランのリスト</caption>
	  <tr>
	    <th>プラン</th>
		<th>特徴</th>
	    <th>名前</th>
	  </tr>
	  <tr>
	    <td>ライト</td>
	    <td>無料モニタリング。</td>
		<td>lite</td>
	  </tr>
	  <tr>
	    <td>プレミアム</td>
	    <td>プレミアム・モニタリング。</td>
		<td>premium</td>
	  </tr>
	</table>
	
	例えば、プランを*ライト*・プランに切り替えるには、以下のコマンドを実行します。
	
	```
	cf update-service "Monitoring-0c" -p lite
    Updating service instance Monitoring-0c as xxx@yyy.com...
    OK
	```
	{: screen}

4. 新規プランが変更されていることを確認します。`cf services` コマンドを実行します。

    ```
	cf services
    Getting services in org MyOrg / space dev as xxx@yyy.com...
    OK

    name              service       plan   bound apps   last operation
    Monitoring-0c     Monitoring    lite                create succeeded
	```
	{: screen}






