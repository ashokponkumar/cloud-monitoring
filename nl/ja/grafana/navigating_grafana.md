---

copyright:
  years: 2017

lastupdated: "2017-05-25"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

# Grafana ダッシュボードへのナビゲート
{:#navigating_grafana}

{{site.data.keyword.Bluemix}} では、分析および視覚化のためのオープン・ソース・プラットフォームである Grafana を使用して、さまざまなグラフ (図表や表など) でメトリックのモニター、検索、分析、および視覚化を行うことができます。高機能な分析タスクを実行するには、Grafana を使用してください。
{:shortdesc}

Grafana は以下のいずれかの方法で起動できます。

* {{site.data.keyword.Bluemix_notm}} から

    Grafana で特定の Docker コンテナーのメトリックを指定し、その特定のコンテナーに応じて起動することができます。この機能は、{{site.data.keyword.Bluemix_notm}} が管理するクラウド・インフラストラクチャーにデプロイされたコンテナーにのみ適用されます。 
    
    詳しくは、[{{site.data.keyword.Bluemix_notm}} ダッシュボードから Grafana ダッシュボードへのナビゲート] (navigating_grafana.html#launch_grafana_from_bluemix) を参照してください。

* ブラウザーの直接リンクから

    Grafana を起動し、表示されているデータが、提供されている {{site.data.keyword.Bluemix_notm}} スペース内のサービスのログを集約するようにすることができます。
    
    詳しくは、[Web ブラウザーから Grafana ダッシュボードへのナビゲート](navigating_grafana.html#launch_grafana_from_browser)を参照してください。
    
Grafana について詳しくは、[Grafana User Guide ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](http://docs.grafana.org/guides/getting_started/ "外部リンク・アイコン"){: new_window} を参照してください。


##  Bluemix ダッシュボードから Grafana ダッシュボードへのナビゲート
{: #launch_grafana_from_bluemix}

**注:** この機能は、{{site.data.keyword.Bluemix_notm}} が管理するクラウド・インフラストラクチャーにデプロイされたコンテナーにのみ適用されます。 

Grafana に表示されるデータをフィルター操作するために使用される照会は、Kibana が起動された {{site.data.keyword.Bluemix_notm}} コンテナーのデータを取り出します。 

Docker コンテナーのメトリックを Grafana に表示するには、以下の手順を実行します。

1. {{site.data.keyword.Bluemix_notm}} にログインし、{{site.data.keyword.Bluemix_notm}} ダッシュボードでコンテナーをクリックします。 
    
2. ナビゲーション・バーで、**「モニターおよびログ」**をクリックします。モニター・タブが開きます。 
    
3. **「詳細ビュー」**をクリックします。**「Grafana」**ダッシュボードが開きます。


##  Web ブラウザーから Grafana ダッシュボードへのナビゲート
{: #launch_grafana_from_browser}

Grafana に表示されるデータをフィルター操作するために使用される照会によって、{{site.data.keyword.Bluemix_notm}} 組織内のスペースのデータが取り出されます。Grafana に表示されるメトリック情報には、ログインしている {{site.data.keyword.Bluemix_notm}} 組織のスペース内にデプロイされているすべてのリソースのレコードが含まれます。

ブラウザーから Grafana を起動するには、以下の手順を実行します。

1. Grafana ユーザー・インターフェースにログインするため [https://metrics.ng.bluemix.net](https://metrics.ng.bluemix.net) を開きます。
2. **「Grafana」**を選択します。
     

