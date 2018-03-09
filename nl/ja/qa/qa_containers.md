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



# Kubernetes クラスターのモニターに関する FAQ
{: #qa_containers}

{{site.data.keyword.monitoringshort}} サービスおよび {{site.data.keyword.containershort_notm}} サービスに関する一般的な質問の回答を以下に示します。
{:shortdesc}

* [コンテナーの Grafana 照会がデータを表示しないか、エラーになる](/docs/services/cloud-monitoring/qa/qa_containers.html#metric_format_change)
* [端末セッションでクラスター環境をセットアップするにはどうすればよいですか](/docs/services/cloud-monitoring/qa/qa_containers.html#qa1)
* [クラスターに関連付けられているスペースのスペース ID およびスペース名を検出するにはどうすればよいですか](/docs/services/cloud-monitoring/qa/qa_containers.html#qa2)
* [クラスター名とクラスター ID をどこで取得できますか](/docs/services/cloud-monitoring/qa/qa_containers.html#qa3)
* [名前空間のリストを取得するにはどうすればよいですか](/docs/services/cloud-monitoring/qa/qa_containers.html#qa7)
* [Kubernetes クラスター内の名前空間にあるポッドのリストを取得するにはどうすればよいですか](/docs/services/cloud-monitoring/qa/qa_containers.html#qa8)
* [クラスター内のすべてのポッドを名前空間別に取得するにはどうすればよいですか](/docs/services/cloud-monitoring/qa/qa_containers.html#qa9)

## コンテナーの Grafana 照会がデータを表示しないか、エラーになる
{: #metric_format_change}

コンテナーに対して定義できる照会のフォーマットが変更されました。

以下のフォーマットは非推奨です。 

```
{Prefix}.{Version}.{Provider}.{Type}.{ServiceName}.{Region}.{Account}.{Cluster}.{Metric}.{Container in a pod}.{Functions}
```
{: screen}

有効な新しいフォーマットは、以下のとおりです。

* [コンテナーの CPU メトリック照会フォーマット](/docs/services/cloud-monitoring/reference/metrics_format.html#cpu_containers)
* [ワーカーのロード・メトリック照会フォーマット](/docs/services/cloud-monitoring/reference/metrics_format.html#load_workers)
* [コンテナーのメモリー・メトリック照会フォーマット](/docs/services/cloud-monitoring/reference/metrics_format.html#mem_containers)

Grafana でデータを視覚化できるように、古い照会を新しいフォーマットに移行してください。

例えば、照会が `crn.v1.bluemix.public.containers-kubernetes.us-south.` で始まる場合、新フォーマット `ibmcloud.public.containers-kubernetes.us-south` に移行する必要があります。

以下の表は、非推奨になったフォーマットのフィールドをリストしたものです。

 <table>
      <caption>表 1. コンテナー用の Grafana 照会フィールド</caption>
      <tr>
        <th align="center">フィールド</th>
        <th align="center">説明</th>
        <th align="center">有効値</th>
      </tr>
      <tr>
        <td>Prefix</td>
        <td>{{site.data.keyword.IBM_notm}} Cloud リソースであることを示すために使用される接頭部。</td>
        <td>`crn`</td>
      </tr>
      <tr>
        <td>Version</td>
        <td>収集されるメトリック・データのフォーマットとフィールドを示すバージョン。 </td>
        <td>`v1`</td>
      </tr>
      <tr>
        <td>Provider</td>
        <td>データの収集が行われるクラウド・プロバイダー。 このフィールドは英数字の識別子。</td>
        <td>パブリック {{site.data.keyword.IBM_notm}} Cloud の場合、この値は `bluemix`</td>
      </tr>
      <tr>
        <td>Type</td>
        <td>データの収集が行われるクラウド環境。</td>
        <td>パブリック {{site.data.keyword.IBM_notm}} Cloud の場合、この値は `public`</td>
      </tr>
      <tr>
        <td>Service name</td>
        <td>メトリックの収集が行われるクラウド・インフラストラクチャー。</td>
        <td>{{site.data.keyword.monitoringshort}} サービスの場合、この値は `containers-kubernetes`</td>
      </tr>
      <tr>
        <td>Region</td>
        <td>メトリックの収集が行われるクラウド地域。</td>
        <td>米国南部地域の場合、この値は `ng` <br>英国地域の場合、この値は `eu-gb` <br>ドイツの場合、この値は `eu-de` <br>シドニーの場合、この値は`au-syd` </td>
      </tr>
      <tr>
        <td>Account</td>
        <td>メトリックの収集が行われるアカウントの GUID。 <br>このフィールドのフォーマットは、`a_ID` です。ここで、ID はアカウントの GUID です。 <br>アカウントの GUID を取得するには、[アカウントの GUID の取得方法](/docs/services/cloud-monitoring/qa/cli_qa.html#account_guid)を参照してください。</td>
        <td>a_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx</td>
      </tr>
      <tr>
        <td>Cluster</td>
        <td>メトリックの収集が行われるクラスターの GUID。</td>
        <td>xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx</td>
      </tr>
      <tr>
        <td>Metric</td>
        <td>コンテナーのために自動的に収集されるメトリック。</td>
        <td>CPU メトリックのリストについては、
[コンテナー用の CPU メトリック](/docs/services/cloud-monitoring/containers/monitoring_containers_ov.html#cpu_metrics_containers)を参照してください。 <br>メモリーのメトリックのリストについては、[メモリーのメトリック](/docs/services/cloud-monitoring/containers/monitoring_containers_ov.html#memory_metrics)を参照してください。 </td>
      </tr>
      <tr>
        <td>Container in a pod</td>
        <td>ポッド内で実行されているコンテナーを一意的に識別するために必要な Kubernetes リソース名と GUID の組み合わせ。 <br>**注:** 照会内でこの項目に使用可能なオプションのリストを表示する際、次のフォーマットの項目もあることに注意してください: {namespace}{pod_name}{deploymentname_name}_POD{container_ID}。 この項目は、Kubernetes によって作成される内部コンテナー ID に対応しています。</td>
		<td>{namespace}_{pod_name}_{deployment_name}_{container_id}</td>
      </tr>
      <tr>
        <td>Functions</td>
        <td>パネル内でコンテナー・メトリックを視覚化するために選択できる照会関数。 <br>詳しくは、[Functions ![(外部リンク・アイコン)](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://graphite.readthedocs.io/en/latest/functions.html){: new_window} を参照してください。</td>
        <td></td>
      </tr>
    </table>


## 端末セッションでクラスター環境をセットアップするにはどうすればよいですか
{: #qa1}

コマンドを使用して Kubernetes クラスターを管理するには、そのクラスターのコンテキストを設定する必要があります。

**注:** クラスターを管理するには、タスクを実行する権限を持つユーザーに割り当てられた {{site.data.keyword.containershort_notm}} サービスに対する IAM ポリシーが必要です。

クラスターのコンテキストをセットアップするには、以下の手順を実行します。

1. {{site.data.keyword.Bluemix_notm}} で、作成したクラスターに関連付けられた、地域、組織、およびスペースにログインします。詳しくは、[{{site.data.keyword.Bluemix_notm}} にログインするにはどうすればよいですか](/docs/services/cloud-monitoring/qa/cli_qa.html#login)を参照してください。

2. {{site.data.keyword.containershort_notm}} サービス・プラグインを初期化します。以下のコマンドを実行します。

	```
	bx cs init
	```
	{: codeblock}

3. 端末セッションでクラスターのコンテキストを設定します。 以下のコマンドを実行します。
    
	```
	bx cs cluster-config MyCluster
	```
	{: codeblock}

    このコマンド実行の出力では、構成ファイルへのパスを設定するためにご使用の端末で実行する必要のあるコマンドが示されます。以下に例を示します。

	```
	export KUBECONFIG=/Users/ibm/.bluemix/plugins/container-service/clusters/MyCluster/kube-config-hou02-MyCluster.yml
	```
	{: codeblock}

    ご使用の端末で環境変数を設定するコマンドをコピーして貼り付け、**Enter** を押します。

 
 
## クラスターに関連付けられているスペースのスペース ID およびスペース名を検出するにはどうすればよいですか
{: #qa2}

ある {{site.data.keyword.Bluemix_notm}} アカウントでクラスターが作成されると、メトリックはそのアカウント内のスペースと関連付けられます。クラスター・メトリックを表示するための照会を作成するときには、スペース ID が必要です。
{:shortdesc}

**注:** クラスターを管理するには、タスクを実行する権限を持つユーザーに割り当てられた {{site.data.keyword.containershort_notm}} サービスに対する IAM ポリシーが必要です。

クラスターのスペース ID を調べるには、以下の手順を実行します。

1. {{site.data.keyword.Bluemix_notm}} で、作成したクラスターに関連付けられた、地域、組織、およびスペースにログインします。詳しくは、[{{site.data.keyword.Bluemix_notm}} にログインするにはどうすればよいですか](/docs/services/cloud-monitoring/qa/cli_qa.html#login)を参照してください。

2. {{site.data.keyword.containershort_notm}} サービス・プラグインを初期化します。以下のコマンドを実行します。

	```
	bx cs init
	```
	{: codeblock}

3. クラスター詳細を取得します。以下のコマンドを実行します。

    ```
    bx cs cluster-get cluster-name
    ```
    {: codeblock}

    ここで、**cluster-name** はクラスターの名前です。

    スペース ID は、**Log Space** フィールドに示される値です。

    このコマンド実行の出力は、以下の例のようになります。

    ```
    Name:			    cluster-name
    ID:			        c213f81296db4c68b84e212c19135a99
    State:			    normal
    Created:		    2017-08-22T18:18:59+0000
    Datacenter:		    dal10
    Master URL:		    https://169.46.7.242:21210
    Ingress subdomain:  cluster-name.us-south.containers.mybluemix.net
    Ingress secret:     cluster-name
    Workers:		    5
    Log Space:		    fa277ff8-8a73-324b-9b75-0f11d54a3ae2
    ```
    {: screen}

4. スペース名を取得します。

    すべてのスペース名をリストします。
	
    ```
	bx account spaces
	```
    {: codeblock}
	
	一致する ID の名前が見つかるまで、各スペースに対して以下のコマンドを実行します。
	
	```
	bx iam space Spacename --guid
	```
	{: codeblock}
	
	


## クラスター名とクラスター ID をどこで取得できますか
{: #qa3}

UI を使用してクラスター名と ID を取得するには、以下の手順を実行します。

1. {{site.data.keyword.Bluemix_notm}} アカウントにログインします。

    {{site.data.keyword.Bluemix_notm}} ダッシュボードは、[http://bluemix.net ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")] (http://bluemix.net){:new_window} にあります。
    
	ユーザー ID およびパスワードを使用してログインすると、{{site.data.keyword.Bluemix_notm}} UI が開きます。

2. {{site.data.keyword.Bluemix_notm}} *ダッシュボード*で、**「メニュー」>「コンテナー」**を選択します。

    アカウントで使用可能なクラスターのリストが表示されます。

3. クラスター ID を取得するため、1 つのクラスター項目を選択します。

    概要ページが開きます。このページで、クラスター ID を取得できます。



コマンド・ラインを使用してクラスター名と ID を取得するには、以下の手順を実行します。

1. {{site.data.keyword.Bluemix_notm}} で、作成したクラスターに関連付けられた、地域、組織、およびスペースにログインします。詳しくは、[{{site.data.keyword.Bluemix_notm}} にログインするにはどうすればよいですか] (/docs/services/cloud-monitoring/qa/cli_qa.html#login) を参照してください。

2. アカウントで使用可能なクラスターをリストします。以下のコマンドを実行します。 

    ```
	bx cs clusters
	``` 
	{: codeblock}
	
	このコマンドの出力には、アカウント内のすべてのクラスターとそれに対応する ID が表示されます。
	
3. クラスター ID を取得するため、以下のコマンドを実行します。

    ```
	bx cs cluster-get my_cluster
	```
    {: screen}	
 


## 名前空間のリストを取得するにはどうすればよいですか
{: #qa7}

クラスター内のすべての名前空間のリストを取得するには、以下の手順を実行します。

以下のステップを実行します。

1. クラスター・コンテキストをセットアップします。詳しくは、[端末セッションでクラスター環境をセットアップするにはどうすればよいですか] (/docs/services/cloud-monitoring/qa/qa_containers.html#qa1) を参照してください。

2. すべての名前空間をリストします。以下の kubectl コマンドを実行します。

    ```
    kubectl get namespaces
	```
	{: codeblock}

## Kubernetes クラスター内の名前空間ごとのポッドのリストを取得するにはどうすればよいですか
{: #qa8}
		
名前空間内のポッドのリストを取得するには、以下の手順を実行します。

1. クラスター・コンテキストをセットアップします。詳しくは、[端末セッションでクラスター環境をセットアップするにはどうすればよいですか] (/docs/services/cloud-monitoring/qa/qa_containers.html#qa1) を参照してください。

2. Kubernetes クラスター内の名前空間ごとのポッドのリストを取得するには、以下のコマンドを実行します。

    ```
	kubectl --namespace=NamespaceName get pods 
	```
	{: codeblock}
	
	ここで、Namespacename は名前空間の名前です (例: *default*)。

## クラスター内のすべてのポッドを名前空間別に取得するにはどうすればよいですか
{: #qa9}
		
以下のステップを実行します。

1. クラスター・コンテキストをセットアップします。詳しくは、[端末セッションでクラスター環境をセットアップするにはどうすればよいですか] (/docs/services/cloud-monitoring/qa/qa_containers.html#qa1) を参照してください。

2. クラスター内のすべてのポッドを名前空間別に取得するには、以下のコマンドを実行します。

	```
	kubectl get pods --all-namespaces
	```
	{: codeblock}
		


