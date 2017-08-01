---

copyright:
  years: 2017

lastupdated: "2017-07-10"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Monitoring サービスのプロビジョニング
{: #provision}

{{site.data.keyword.Bluemix}} UI またはコマンド・ラインから {{site.data.keyword.monitoringshort}} サービスをプロビジョンできます。
{:shortdesc}


## UI からのプロビジョニング
{: #ui}

{{site.data.keyword.Bluemix_notm}} で {{site.data.keyword.monitoringshort}} サービスのインスタンスをプロビジョンするには、以下の手順を実行します。

1. {{site.data.keyword.Bluemix_notm}} アカウントにログインします。

    {{site.data.keyword.Bluemix_notm}} ダッシュボードは、[http://bluemix.net ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](http://bluemix.net "外部リンク・アイコン"){:new_window} にあります。
    
	ユーザー ID およびパスワードを使用してログインすると、{{site.data.keyword.Bluemix_notm}} UI が開きます。

2. **「カタログ」**をクリックします。{{site.data.keyword.Bluemix_notm}} で使用可能なサービスのリストが開きます。

3. **「DevOps」**カテゴリーを選択して、表示されるサービスのリストをフィルターに掛けます。

4. **「モニタリング」**タイルをクリックします。

5. サービス・プランを選択します。最大 45 日間のメトリックの収集およびアクセス、およびワイルドカードを使用するルールを含むアラート・ルールの定義を行うには、**「プレミアム」**プランを選択します。 デフォルトでは、**「ライト」**プランが設定されます。このプランでは、サービスをプロビジョンしているスペース内のプラットフォーム・メトリックを収集し、それらのメトリックを 15 日間保持することが可能です。 

    サービス・プランについて詳しくは、[サービス・プラン](/docs/services/cloud-monitoring/monitoring_ov.html#plans)を参照してください。
	
6. **「作成」**をクリックして、ログインした {{site.data.keyword.Bluemix_notm}} スペースで {{site.data.keyword.monitoringshort}} サービスをプロビジョンします。
  
 

## CLI からのプロビジョニング
{: #cli}

コマンド・ラインを通じて {{site.data.keyword.Bluemix_notm}} で {{site.data.keyword.monitoringshort}} サービスのインスタンスをプロビジョンするには、以下の手順を実行します。

1. [前提条件] {{site.data.keyword.Bluemix_notm}} CLI をインストールします。

   詳しくは、[{{site.data.keyword.Bluemix_notm}} CLI のインストール](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa)を参照してください。
   
   CLI がインストールされたら、次の手順に進んでください。
    
2. {{site.data.keyword.Bluemix_notm}} の地域、組織、およびスペースにログインします。以下のコマンドを実行します。

    ```
    cf login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    指示に従います。{{site.data.keyword.Bluemix_notm}} の資格情報を入力し、組織とスペースを選択します。
	
3. `cf create-service` コマンドを実行して、インスタンスをプロビジョンします。

    ```
	cf create-service service_name service_plan service_instance_name
	```
	{: codeblock}
	
	詳細は次のとおりです。
	
	* service_name は、サービスの名前 (すなわち **Monitoring**) です。
	* service_plan は、サービス・プラン名です。最大 45 日間のメトリックの収集およびアクセス、およびワイルドカードを使用するルールを含むアラート・ルールの定義を行うには、**「プレミアム」**プランを選択します。 デフォルトでは、**「ライト」**プランが設定されます。このプランでは、サービスをプロビジョンしているスペース内のプラットフォーム・メトリックを収集し、それらのメトリックを 15 日間保持することが可能です。プランのリストについては、[{{site.data.keyword.monitoringshort}} サービス・プラン](/docs/services/cloud-monitoring/monitoring_ov.html#plan) を参照してください。
	* service_instance_name は、作成する新規サービス・インスタンスに使用する名前です。
	
	cf コマンドについて詳しくは、[cf create-service](/docs/cli/reference/cfcommands/index.html#cf_create-service) を参照してください。
	例えば、プレミアム・プランで {{site.data.keyword.monitoringshort}} サービスのインスタンスを作成するには、以下のコマンドを実行します。
	```
	cf create-service Monitoring premium my_monitoring_svc
	```
	    {: codeblock}
	
    4. サービスが正常に作成されたことを確認します。以下のコマンドを実行します。
	
	```	
	cf services
	```
	{: codeblock}
	
	コマンド実行の出力は、以下のようになります。
	```
    Getting services in org MyOrg / space MySpace as xxx@yyy.com...
    OK
    
    name                           service                  plan                   bound apps              last operation
    my_monitoring_svc              Monitoring               premium                                        create succeeded
	```
	{: screen}
	
    



