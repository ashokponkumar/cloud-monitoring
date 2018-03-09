---

copyright:
  years: 2017, 2018

lastupdated: "2018-02-07"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Kubernetes クラスターにデプロイされたアプリに関する Grafana でのメトリックの分析
{: #container_service_metrics}

このチュートリアルでは、{{site.data.keyword.monitoringlong}} サービスを使用して、コンテナーのパフォーマンスをモニターする方法を説明します。
{:shortdesc}


## 達成目標
{: #objectives}

以下のように Kubernetes クラスターにデプロイされたアプリのコンテナー・メトリックの検索および分析方法について説明します。

1. クラスターで収集されたメトリックが、{{site.data.keyword.monitoringshort}} サービスに転送される際の場所を識別します。 
2. Grafana を起動し、クラスター・メトリックを表示できる {{site.data.keyword.monitoringshort}} ドメインを設定します。
3. {{site.data.keyword.Bluemix_notm}} の Kubernetes クラスターにデプロイされたアプリに関するコンテナー・メトリックを検索および分析します。

このチュートリアルでは、{{site.data.keyword.Bluemix_notm}} で以下のエンドツーエンドのシナリオを機能させるために必要なステップを順に説明します。クラスターのプロビジョン、{{site.data.keyword.Bluemix_notm}} でクラスターがメトリックを {{site.data.keyword.monitoringshort}} サービスに送信する際の場所の識別、クラスターへのアプリのデプロイ、Grafana によるそのクラスターのコンテナー・メトリックの表示とフィルタリングの各ステップです。


**注:** このチュートリアルを完了するには、前提条件と、それぞれのステップからリンクされたチュートリアルを完了する必要があります。


## 前提条件
{: #prereqs}

1. Kubernetes 標準クラスターの作成、クラスターへのアプリのデプロイ、および Grafana でモニターするための {{site.data.keyword.Bluemix_notm}} 内のメトリック照会を実行できる権限を持つ、{{site.data.keyword.Bluemix_notm}} アカウントのメンバーまたは所有者になります。

    {{site.data.keyword.Bluemix_notm}} のユーザー ID には、以下のポリシーが割り当てられている必要があります。
    
    * *オペレーター* または*管理者* 権限が設定された、{{site.data.keyword.containershort}} の IAM ポリシー。
    * *開発者* 権限が設定された、{{site.data.keyword.monitoringshort}} サービスがプロビジョンされているスペースの CF 役割。
    
    詳しくは、[IBM Cloud UI によるユーザーへの IAM ポリシーの割り当て (Assign an IAM policy to a user through the IBM Cloud UI)](/docs/services/cloud-monitoring/security/grant_permissions.html#grant_permissions_ui_account) と[IBM Cloud UI の使用によるユーザーへの CF 役割の付与 (Granting a user a CF role by using the IBM Cloud UI)](/docs/services/cloud-monitoring/security/grant_permissions.html#grant_permissions_ui_space) を参照してください。

2. コマンド・ラインから Kubernetes クラスターの管理およびアプリのデプロイを実行できる端末セッションを用意します。このチュートリアルの例は、Ubuntu Linux システム用です。

3. {{site.data.keyword.containershort}} の作業を行うための CLI を Ubuntu システムにインストールします。

    * {{site.data.keyword.Bluemix_notm}} CLI をインストールします。 詳しくは、[{{site.data.keyword.Bluemix_notm}} CLI のインストール](/docs/cli/reference/bluemix_cli/download_cli.html#download_install)を参照してください。
    
    * {{site.data.keyword.containershort}} での Kubernetes クラスターの作成と管理、およびクラスターへのコンテナー化アプリのデプロイを行うための {{site.data.keyword.containershort}} CLI をインストールします。詳しくは、[CS プラグインのインストール](/docs/containers/cs_cli_install.html#cs_cli_install_steps)を参照してください。
    

    
 

##  手順 1 : Kubernetes クラスターのプロビジョン
{: #step1}

以下のステップを実行します。

1. 標準の Kubernetes クラスターを作成します。

   * [UI により Kubernetes 標準クラスターを作成します](/docs/containers/cs_cluster.html#cs_cluster_ui)。
   * [CLI を使用して Kubernetes 標準クラスターを作成します](/docs/containers/cs_cluster.html#cs_cluster_cli)。

2. 端末にクラスター・コンテキストをセットアップします。 コンテキストを設定すると、Kubernetes クラスターを管理し、Kubernetes クラスターにアプリケーションをデプロイできるようになります。

    {{site.data.keyword.Bluemix_notm}} で、作成したクラスターに関連付けられた地域、組織、およびスペースにログインします。詳しくは、[{{site.data.keyword.Bluemix_notm}} にログインするにはどうすればよいですか](/docs/services/CloudLogAnalysis/qa/cli_qa.html#login)を参照してください。

	{{site.data.keyword.containershort}} サービス・プラグインを初期化します。

	```
	bx cs init
	```
	{: codeblock}

    端末コンテキストをクラスターに設定します。
    
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



## 手順 2: クラスターがメトリックを {{site.data.keyword.monitoringshort}} サービスに転送する際の場所の識別
{: #step2}

クラスターはアカウント・レベルのリソースです。{{site.data.keyword.containershort}} でクラスターをプロビジョンする際に、クラスターはアカウント・レベルで作成することも、Cloud Foundry (CF) スペースを関連付けて作成することもできます。クラスターがプロビジョンされて作動可能になるとすぐに、メトリックが自動的に収集され、{{site.data.keyword.monitoringshort}} サービスに転送されます。

* CF スペースが関連付けられたクラスターは、スペース・メトリック・ドメインにメトリックを転送します。
* アカウント・レベルで作成されたクラスターは、アカウント・メトリック・ドメインにメトリックを転送します。

クラスターがメトリックをどこに転送するかを調べるには、以下のコマンドを実行します。

```
$ bx cs cluster-get ClusterName --json
```
{: codeblock}

ここで、*ClusterName* はクラスターの名前です。

出力の以下のフィールドで、メトリックがどこに転送されるかに関する情報が示されます。

* **logOrg** は、CF 組織の ID を定義します。
* **logOrgName** は、CF 組織の名前を定義します。
* **logSpace** は、CF スペースの ID を定義します。
* **logSpaceName** は、CF スペースの名前を定義します。

これらのフィールドが空の場合、メトリックはアカウント・ドメインに転送されます。
フィールドに CF 組織と CF スペースが設定されている場合、メトリックはこのスペースに関連付けられているスペース・ドメインに転送されます。

例えば、メトリックをアカウント・ドメインに転送するクラスターの出力は、以下のようになります。

```
$ bx cs cluster-get MyDemoCluster --json
{
    "id": "f9adabcjhefg745746hgfjbnkdnfsks",
    "name": "MyDemoCluster",
    "region": "eu-gb",
    "dataCenter": "lon02",
    "location": "eu-gb-lon02",
    "serverURL": "https://xxx.xxx.xxx.x:xxxxx",
    "state": "normal",
    "createdDate": "2018-01-30T17:41:14+0000",
    "modifiedDate": "2018-01-30T17:41:14+0000",
    "workerCount": 2,
    "isPaid": true,
    "masterKubeVersion": "1.8.6_1505",
    "targetVersion": "1.8.6_1505",
    "ingressHostname": "mydemocluster.uk-south.containers.mybluemix.net",
    "ingressSecretName": "mydemocluster",
    "ownerEmail": "xxxx@uibm.com",
    "logOrg": "",
    "logOrgName": "",
    "logSpace": "",
    "logSpaceName": "",
    "monitoringURL": "https://metrics.eu-gb.bluemix.net/app/#/grafana4/dashboard/db/a-siuhfieuhf7346586hfrhf_ClusterMonitoringDashboard_v1?scopeId=a-siuhfieuhf7346586hfrhf\u0026?var-Account_ID=a_siuhfieuhf7346586hfrhf\u0026var-Cluster=MyDemoCluster\u0026var-Namespace=default\u0026var-Pod_ID=All",
    "addons": [
        {
            "name": "customer-storage-pod",
            "enabled": true
        },
        {
            "name": "basic-ingress-v2",
            "enabled": true
        },
        {
            "name": "storage-watcher-pod",
            "enabled": true
        }
    ],
    "vlans": null
}
```
{: screen}





## 手順 3: メトリック・ドメイン内のメトリックを表示する権限をユーザーに付与
{: #step3}

スペース・ドメイン内のメトリックを表示する権限をユーザーに付与するには、スペース内でユーザーが {{site.data.keyword.monitoringshort}} サービスを使用して実行できるアクションを記述した CF 役割を、当該ユーザーに割り当てる必要があります。

アカウント・ドメイン内のメトリックを表示する権限をユーザーに付与するには、ユーザーが {{site.data.keyword.monitoringshort}} サービスを使用して実行できるアクションを記述した IAM ポリシーを、当該ユーザーに割り当てる必要があります。

### スペース・ドメイン内のメトリックを表示する権限をユーザーに付与
{: #space}

以下の手順を実行し、{{site.data.keyword.monitoringshort}} サービスを処理する権限をユーザーに付与します。

1. {{site.data.keyword.Bluemix_notm}} コンソールにログインします。

    Web ブラウザーを開き、{{site.data.keyword.Bluemix_notm}} ダッシュボードを起動
します。[http://bluemix.net ![外部リンクのアイコン](../../../icons/launch-glyph.svg "外部リンクのアイコン")](http://bluemix.net){:new_window}
	
	ユーザー ID およびパスワードを使用してログインすると、{{site.data.keyword.Bluemix_notm}} UI が開きます。

2. メニュー・バーから、**「管理」>「アカウント」>「ユーザー」**をクリックします。 

    「*ユーザー*」ウィンドウに、現在選択されているアカウントにおけるユーザーのリストが、E メール・アドレスと共に表示されます。
	
3. ユーザーがアカウントのメンバーの場合、リストからユーザー名を選択するか、*「アクション」*メニューから**「ユーザーの管理 (Manage user)」**をクリックします。

    ユーザーがアカウントのメンバーではない場合、[ユーザーの招待](/docs/iam/iamuserinv.html#iamuserinv)を参照してください。

4. **「Cloud Foundry アクセス権限」**を選択し、次に**「組織の割り当て」** を選択します。

5. 以下の値を入力します。

    <table>
      <caption></caption>
      <tr>
        <th>フィールド</th>
        <th>値</th>
      </tr>
      <tr>
        <td>組織</td>
        <td>MyOrg</td>
      </tr>
      <tr>
        <td>組織の役割</td>
        <td>組織の役割なし</td>
      </tr>
      <tr>
        <td>地域</td>
        <td>米国南部</td>
      </tr>
      <tr>
        <td>スペース</td>
        <td>dev</td>
      </tr>
      <tr>
        <td>スペースの役割</td>
        <td>監査員</td>
      </tr>
	
6. **「役割の保存」**をクリックします。

### アカウント・ドメイン内のメトリックを表示する権限をユーザーに付与
{: #acc}

以下の手順を実行し、{{site.data.keyword.monitoringshort}} サービスを処理する権限をユーザーに付与します。

1. {{site.data.keyword.Bluemix_notm}} コンソールにログインします。

    Web ブラウザーを開き、{{site.data.keyword.Bluemix_notm}} ダッシュボードを起動
します。[http://bluemix.net ![外部リンクのアイコン](../../../icons/launch-glyph.svg "外部リンクのアイコン")](http://bluemix.net){:new_window}
	
	ユーザー ID およびパスワードを使用してログインすると、{{site.data.keyword.Bluemix_notm}} UI が開きます。

2. メニュー・バーから、**「管理」>「アカウント」>「ユーザー」**をクリックします。 

    「*ユーザー*」ウィンドウに、現在選択されているアカウントにおけるユーザーのリストが、E メール・アドレスと共に表示されます。
	
3. ユーザーがアカウントのメンバーの場合、リストからユーザー名を選択するか、*「アクション」*メニューから**「ユーザーの管理 (Manage user)」**をクリックします。

    ユーザーがアカウントのメンバーではない場合、[ユーザーの招待](/docs/iam/iamuserinv.html#iamuserinv)を参照してください。

4. **「アクセス・ポリシー」>「アクセス権限の割り当て」>「リソースへのアクセス権限の割り当て」**と選択します。

5. サービス**「{{site.data.keyword.monitoringlong}}」**を選択し、クラスターが使用可能な地域 (このチュートリアルの場合には**「米国南部」**) を選択し、役割**「ビューアー」**を選択します。



## 手順 4: {{site.data.keyword.containershort_notm}} キー所有者に権限を付与
{: #step4}

クラスターがスペース・ドメインにメトリックを転送する場合、組織およびスペース内の {{site.data.keyword.containershort}} キー所有者に Cloud Foundry (CF) 権限を付与することも必要です。キー所有者には、組織の*組織管理者* 役割と、スペースの*スペース管理者* および*開発者* の役割が必要です。

クラスターがアカウント・ドメインにメトリックを転送する場合、{{site.data.keyword.containershort}} キー所有者には、{{site.data.keyword.monitoringshort}} サービスの管理者権限を含む IAM ポリシーが必要です。

### スペース・ドメイン内のメトリックを表示する権限を付与
{: #space_1}

{{site.data.keyword.containershort}} キー所有者のユーザー ID に、組織の*組織管理者* 役割と、スペースの*スペース管理者* および*開発者* の役割の権限を付与します。以下のステップを実行します。
    
1. {{site.data.keyword.Bluemix_notm}} コンソールにログインします。

    Web ブラウザーを開き、{{site.data.keyword.Bluemix_notm}} ダッシュボードを起動
します。[http://bluemix.net ![外部リンクのアイコン](../../../icons/launch-glyph.svg "外部リンクのアイコン")](http://bluemix.net){:new_window}
	
	ユーザー ID およびパスワードを使用してログインすると、{{site.data.keyword.Bluemix_notm}} UI が開きます。

2. メニュー・バーから、**「管理」>「アカウント」>「ユーザー」**をクリックします。 

    「*ユーザー*」ウィンドウに、現在選択されているアカウントにおけるユーザーのリストが、E メール・アドレスと共に表示されます。
	
3. {{site.data.keyword.containershort}} キー所有者のユーザー ID を調べます。

    コマンド `bx cs api-key-info ClusterName` を実行して、{{site.data.keyword.containershort}} キー所有者のユーザー ID を取得します。

4. **「Cloud Foundry アクセス権限」**を選択し、次に**「組織の割り当て」**を選択します。

5. 以下の値を入力します。 

    <table>
      <caption></caption>
      <tr>
        <th>フィールド</th>
        <th>値</th>
      </tr>
      <tr>
        <td>組織</td>
        <td>MyOrg</td>
      </tr>
      <tr>
        <td>組織の役割</td>
        <td>管理者</td>
      </tr>
      <tr>
        <td>地域</td>
        <td>米国南部</td>
      </tr>
      <tr>
        <td>スペース</td>
        <td>dev</td>
      </tr>
      <tr>
        <td>スペースの役割</td>
        <td>開発者</td>
      </tr>
	
6. **「役割の保存」**をクリックします。


### アカウント・ドメイン内のメトリックを表示する権限を付与
{: #acc_1}

以下のステップを実行します。

1. {{site.data.keyword.Bluemix_notm}} コンソールにログインします。

    Web ブラウザーを開き、{{site.data.keyword.Bluemix_notm}} ダッシュボードを起動
します。[http://bluemix.net ![外部リンクのアイコン](../../../icons/launch-glyph.svg "外部リンクのアイコン")](http://bluemix.net){:new_window}
	
	ユーザー ID およびパスワードを使用してログインすると、{{site.data.keyword.Bluemix_notm}} UI が開きます。

2. メニュー・バーから、**「管理」>「アカウント」>「ユーザー」**をクリックします。 

    *「ユーザー」*ウィンドウに、現在選択されているアカウントにおけるユーザーのリストが、E メール・アドレスと共に表示されます。
	
3. {{site.data.keyword.containershort}} キー所有者のユーザー ID を調べます。

    コマンド `bx cs api-key-info ClusterName` を実行して、{{site.data.keyword.containershort}} キー所有者のユーザー ID を取得します。

4. **「アクセス・ポリシー」>「アクセス権限の割り当て」>「リソースへのアクセス権限の割り当て」**と選択します。

5. サービス**「{{site.data.keyword.monitoringlong}}」**を選択し、クラスターが使用可能な地域 (このチュートリアルの場合には**「米国南部」**) を選択し、役割**「管理者」**を選択します。	

## 手順 5: Kubernetes クラスターへのサンプル・アプリのデプロイ
{: #step5}

Kubernetes クラスターにサンプル・アプリをデプロイし、実行します。 チュートリアル[『演習 1: Kubernetes クラスターへの単一インスタンス・アプリのデプロイ (Lesson 1: Deploying single instance apps to Kubernetes clusters)』](/docs/containers/cs_tutorials_apps.html#cs_apps_tutorial_lesson1)の手順を実行して、サンプル・アプリをデプロイします。

このアプリは、以下のような Hello World Node.js アプリです。

```
var express = require('express')
var app = express()

app.get('/', function(req, res) {
  res.send('Hello world! Your app is up and running in a cluster!\n')
})
app.listen(8080, function() {
  console.log('Sample app is listening on port 8080.')
})
```
{: screen}

このサンプル・アプリでは、ブラウザーでアプリをテストすると、「`Sample app is listening on port 8080.`」というメッセージを標準出力に書き込みます。


## 手順 6: Grafana の起動とメトリック・ドメインの設定
{: #step6}

ブラウザーから Grafana を起動し、クラスター・メトリックを表示できる {{site.data.keyword.monitoringshort}} ドメインを設定します。

クラスターのメトリックを分析するには、そのクラスターが作成されているクラウド Public 地域で Grafana にアクセスする必要があります。 詳しくは、[Web ブラウザーから Grafana ダッシュボードへのナビゲート](/docs/services/cloud-monitoring/grafana/navigating_grafana.html#launch_grafana_from_browser)を参照してください。

1. ブラウザーから Grafana を起動します。 

    クラスターを作成した地域の {{site.data.keyword.monitoringshort}} サービス URL を入力します。 
    
    地域ごとの URL を取得するには、[Monitoring サービスの URL](/docs/services/cloud-monitoring/monitoring_ov.html#region) を参照してください。

    例えば、米国南部地域の場合は、[https://metrics.ng.bluemix.net/](https://metrics.ng.bluemix.net/)を起動します。

2. クラスター・メトリックを表示できる {{site.data.keyword.monitoringshort}} ドメインを設定します。

    Grafana で、ID を選択します。 次に、正しいアカウントにログインしていることを確認して、ドメインを選択します。

    CF スペースが関連付けられたクラスターは、スペース・メトリック・ドメインにメトリックを転送します。`Domain = space` を選択し、クラスターに関連付けられている組織とスペースを選択します。

    アカウント・レベルで作成されたクラスターは、アカウント・メトリック・ドメインにメトリックを転送します。`Domain = account` を選択します。

## 手順 7: Grafana でのクラスターのモニター
{: #step7}

{{site.data.keyword.containershort}} では、クラスター・メトリックのモニターに使用できる Grafana ダッシュボードが提供されます。 

サンプル・ダッシュボードを開くには、以下のステップを実行します。

1. サイド・メニュー・バーのトグル を選択します。![Grafana サイド・メニュー・バー](images/grafana_settings.gif "Grafana サイド・メニュー・バー")
2. **「Dashboards」**を選択します。
3. **「Open」**をクリックします。
4. **「ClusterMonitoringDashboard_v1」**を選択します。

サンプル・ダッシュボードが開きます。 

![Grafana サンプル・ダッシュボード](images/cluster_grafana_sample_dashboard.png "Grafana サンプル・ダッシュボード")



## 次の手順
{: #next_steps}

メトリックのアラートを定義します。 詳しくは、[アラートの構成](/docs/services/cloud-monitoring/config_alerts_ov.html#config_alerts_ov)を参照してください。
