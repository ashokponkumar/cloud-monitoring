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



# よくある質問と回答
{: #qa}

{{site.data.keyword.monitoringshort}} サービスに関する一般的な質問の回答を以下に示します。 
{:shortdesc}

* [最近メトリック値を送信していないメトリックの古いデータを表示できないのはなぜですか?](#qa3)


## 最近メトリック値を送信していないメトリックの古いデータを表示できないのはなぜですか?
{: #qa3}

{{site.data.keyword.monitoringshort}} は、過去 7 日間書き込まれていないメトリックを識別することにより、事実上一時的と思われるメトリック・パスの全データを削除します。 

以下に例を示します。

* コンテナーが削除されると、そのコンテナーに関連付けられたメトリックは 7 日間存在し、その後、それらのメトリックは削除されます。
* `<space_id>.test.statsd.gauge-hello` という名前の statsd ゲージがあり、それに 1 週間書き込まないと、メトリックは一時的と見なされ、そのメトリックはすべての履歴情報と共に削除されます。 

