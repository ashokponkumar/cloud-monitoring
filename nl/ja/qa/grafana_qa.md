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


# Grafana を使用する際のよくある質問と回答
{: #grafana_qa}

{{site.data.keyword.monitoringshort}} サービスでの Grafana の使用に関する一般的な質問の回答を以下に示します。
{:shortdesc}

* [Monitoring サービスの Web UI にログインする際に 404 を受け取る](/docs/services/cloud-monitoring/qa/grafana_qa.html#404)
* [Grafana ダッシュボード用の JSON データをアップロードしたところ、スクロール・バーが消えた](/docs/services/cloud-monitoring/qa/grafana_qa.html#2)


## UUA 認証モデルを使用して Monitoring サービスの Web UI にログインする際に 404 を受け取る
{: #404}

{{site.data.keyword.monitoringshort}} サービスの Web UI (Grafana) にログインしようとした時に 404 を受け取る場合は、スペースが存在していること、およびそのスペースへのアクセス権を持っていることを確認してください。

[UAA 認証モデル](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_uaa)を使用して {{site.data.keyword.monitoringshort}} サービスにアクセスする際は、{{site.data.keyword.Bluemix_notm}} コンソールへのログインに使用するユーザー ID およびパスワードと同じものを使用する必要があります。 

ログインしようとしているアカウント、組織、およびスペースへのアクセス権を持っているかどうか確認するには、{{site.data.keyword.Bluemix_notm}} コンソールにログインし、スペースに切り替えます。 

また、コマンド・ラインを使用して、そのスペースへのアクセス権があるかどうかを確認することもできます。以下のコマンドを実行して、{{site.data.keyword.Bluemix_notm}} 地域、組織、およびスペースにログインします。

```
    bx login -a https://api.ng.bluemix.net
    ```
{: codeblock}

指示に従います。{{site.data.keyword.Bluemix_notm}} の資格情報を入力し、組織とスペースを選択します。


## Grafana ダッシュボード用の JSON データをアップロードしたところ、スクロール・バーが消えた
{: #2}

Grafana には、ダッシュボードをインポートした後にスクロール・バーを非表示にするバグがあります。 

スクロール・バーを表示するには、インポートした後にダッシュボードを保存してください。 








