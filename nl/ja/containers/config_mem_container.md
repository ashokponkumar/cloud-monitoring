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



# Grafana でのコンテナーのメモリー・メトリックの構成
{: #config_mem_container}

{{site.data.keyword.Bluemix}} では、コンテナーについて選択されたメモリー・メトリックが自動的に収集されます。{{site.data.keyword.monitoringlong}} を介してそれらをモニターするには、Grafana 照会を定義する必要があります。
{:shortdesc}

メモリーのメトリックのリストについては、[メモリーのメトリック](/docs/services/cloud-monitoring/containers/monitoring_containers_ov.html#memory_metrics)を参照してください。


## 手順 1: モニターするコンテナーのデータの収集
{: #step1}

モニターするコンテナーについて以下の情報を取得します。

* クラスターが稼働している**地域**。
* コンテナーが稼働している**クラスター名**。 
* コンテナーがデプロイされている**名前空間**。 

    名前空間は、クラスター・リソースをグループ化するために使用されます。
	
* モニターするコンテナーに関連付けられている**ポッド名**。 

    ポッドは、1 つ以上のコンテナーのグループを、共有ストレージとネットワーク、およびクラスター内のコンテナーの実行方法に関する詳細情報と共に定義します。
	
* モニターするコンテナーの**コンテナー名**。

メトリックがスペース・ドメインに転送されるか、またはアカウント・ドメインに転送されるかを確認します。

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

これらのフィールドが空の場合、メトリックはアカウント・ドメインに転送されます。フィールドに CF 組織と CF スペースが設定されている場合、メトリックはこのスペースに関連付けられたスペース・ドメインに転送されます。

## 手順 2: Grafana の起動
{: #step2}

ブラウザーから Grafana を起動します。 詳しくは、[Web ブラウザーから Grafana ダッシュボードへのナビゲート](/docs/services/cloud-monitoring/grafana/navigating_grafana.html#launch_grafana_from_browser)を参照してください。

Grafana では、クラスターが実行されているアカウントに必ずログインしてください。 

1. ブラウザーから Grafana を起動します。 

    クラスターを作成した地域の {{site.data.keyword.monitoringshort}} サービス URL を入力します。 
    
    地域ごとの URL を取得するには、[Monitoring サービスの URL](/docs/services/cloud-monitoring/monitoring_ov.html#region) を参照してください。

    例えば、米国南部地域の場合は、[https://metrics.ng.bluemix.net/](https://metrics.ng.bluemix.net/)を起動します。

2. クラスター・メトリックを表示できる {{site.data.keyword.monitoringshort}} ドメインを設定します。

    Grafana で、ID を選択します。 次に、正しいアカウントにログインしていることを確認してドメインを選択します。

    CF スペースが関連付けられたクラスターは、スペース・メトリック・ドメインにメトリックを転送します。`Domain = space` を選択し、クラスターに関連付けられている組織とスペースを選択します。

    アカウント・レベルで作成されたクラスターは、アカウント・メトリック・ドメインにメトリックを転送します。`Domain = account` を選択します。




## 手順 3: Grafana で CPU メトリックの照会を定義
{: #step3}

Grafana ダッシュボードを作成し、ワーカーの CPU 使用量をモニターするための照会を定義するには、以下の手順を実行します。

1. 新規ダッシュボードを作成します。

    * サイド・メニュー・バーのトグル を選択します。![Grafana サイド・メニュー・バー](images/grafana_settings.gif "Grafana サイド・メニュー・バー")
    * **「Dashboards」**を選択します。
    * **「New」**をクリックします

    ダッシュボードが開きます。 ダッシュボードには、すぐに構成できる空の行が含まれています。

2. ポッドの CPU メトリックをモニターするために*「Graph」*パネルを追加します。

    1. **「Graph」**をクリックします。

    2. グラフのタイトルをクリックし、次に**「edit」**を選択します。

        *「Metrics」*タブが開きます。 デフォルト・データ・ソースが表示されています。

3. グラフに表示されるデータをフィルターに掛ける照会を定義します。 

    照会のフォーマットについて詳しくは、[コンテナーに関して収集されるメモリー・メトリックの照会フォーマット](/docs/services/cloud-monitoring/reference/metrics_format.html#mem_containers)を参照してください。

    *「Metrics」*タブで、**「Add query」**を選択します。 <br>照会項目が追加されます。 各照会には、1 文字のラベルが付いています。
	
	以下の手順を実行して照会を定義します。

    1. ソースを指定するために**「Select metric」**をクリックし、次に `ibmcloud` を選択します。
    
    2. クラウド・タイプを指定するために**「Select metric」**をクリックし、次に `public` を選択します。
    
    3. サービス名を指定するために**「Select metric」**をクリックし、次に `containers-kubernetes` を選択します。
	
    4. **「Select metric」**をクリックして地域を指定し、次に、クラスターが実行されている地域を選択します。例えば、米国南部地域で使用可能なクラスターでは `us-south` を選択します。
    
    5. **「Select metric」**をクリックしてクラスター名を指定し、次に、コンテナーが実行されているクラスターの名前を選択します。
		
	6. **「Select metric」**をクリックしてメトリック・ソースを指定します。**「container」**を選択します。
		
	7. **「Select metric」**をクリックして名前空間を指定します。次に、コンテナーに関連付けられている、クラスター内の名前空間の名前を入力します。
		
	8. **「Select metric」**をクリックしてポッド名を指定します。
	
	9. **「Select metric」**をクリックして、モニターするコンテナーのコンテナー名を指定します。
	
	10. **「Select metric」**をクリックしてメトリック・タイプを指定し、次に**「Select metric」**をクリックしてメトリックのサブタイプを指定します。
	
	    例えば、コンテナーが現在使用しているメモリーのバイト数をモニターするには、タイプには**「memory」**を選択し、サブタイプには**「current」**を選択します。
	
	    メモリーのメトリックのリストについては、[コンテナー用のメモリーのメトリック](/docs/services/cloud-monitoring/containers/monitoring_containers_ov.html#memory_metrics)を参照してください。 
	
	11. 正符号イメージ ![追加アイコン](images/grafana_plus_image.gif "正符号イメージ") をクリックし、関数を選択します。 関数を追加すると、メトリックに使用可能なデータを変換したり、結合したり、それらのデータに対して計算を実行したりすることができます。

        例えば、**alias(newName)** 関数を追加して、メトリックの別名を追加することができます。 この別名は、グラフに表示される凡例にメトリック名の代わりにストリングを表示するために使用されます。

        メトリックの別名を追加するには、以下の手順を実行します。

        1. 正符号をクリックします。
        2. **「Special」**を選択します。
        3. **「alias」**を選択します。
        4. ストリング (例えば、`My sample metric`) を入力します。

4. 後で再利用するためにダッシュボードを保存します。

    1. ダッシュボードの保存イメージ ![ダッシュボードの保存イメージ](images/grafana_save_image.gif "ダッシュボードの保存イメージ") をクリックします。.
    2. ダッシュボードの名前を入力します。
    3. **「保存」**をクリックします。

	
