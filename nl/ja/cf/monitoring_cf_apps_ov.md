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

# Cloud Foundry で実行されているアプリのモニター
 {:#monitoring_bluemix_apps}

{{site.data.keyword.monitoringshort}} サービスにメトリックを送信するために、Cloud Foundry (CF) アプリケーションは、{{site.data.keyword.monitoringshort}} REST API を使用してアプリから直接メトリックを送信できます。
{:shortdesc}

Cloud Foundry インフラストラクチャーを使用して {{site.data.keyword.Bluemix_notm}} でアプリを実行している時に、アプリの正常性状況、リソース使用量、およびトラフィック・メトリックをモニターしたい場合があります。このパフォーマンス情報を使用すると、適宜、意思決定やアクション実行を行うことができます。


Cloud Foundry で実行されているアプリは collectd を実行できません。代わりに、lumberjack クライアントまたは RESTful API を使用して、アプリ自体からデータを送信する必要があります。 

##CF アプリからデータを送信する他の方法 [@lindj created a collectd and statsd buildpack] (https://github.com/jimlindeman/python-buildpack-collectd)

これは python-buildpack リリースのクローンです。その python-buildpack リリースには、statsd 入力プラグインと Bluemix のマルチテナント graphite 出力プラグインを使用して localhost:8125 で listen するために collectd がインストールされています。 

ビルドパックを使用して、CF アプリを Bluemix にデプロイすることができます。ビルドパックでは、フレームワークと、アプリのランタイム・サポートが提供されます。collectd をインストールし、statsd 入力プラグインと Bluemix のマルチテナント Graphite 出力プラグインを使用して localhost:8125 で listen するよう構成することができます。 
