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



# 監視 Kubernetes 叢集的常見問題
{: #qa_containers}

以下是關於 {{site.data.keyword.monitoringshort}} 服務及 {{site.data.keyword.containershort_notm}} 服務的常見問題與解答。
{:shortdesc}

* [我容器中的 Grafana 查詢未顯示資料，或發生錯誤](/docs/services/cloud-monitoring/qa/qa_containers.html#metric_format_change)
* [如何在終端機階段作業中設定叢集環境？](/docs/services/cloud-monitoring/qa/qa_containers.html#qa1)
* [如何找到與叢集相關聯之空間的空間 ID 及空間名稱？](/docs/services/cloud-monitoring/qa/qa_containers.html#qa2)
* [我可以在何處取得叢集名稱及叢集 ID？](/docs/services/cloud-monitoring/qa/qa_containers.html#qa3)
* [如何取得名稱空間清單？](/docs/services/cloud-monitoring/qa/qa_containers.html#qa7)
* [如何取得 Kubernetes 叢集之名稱空間中的 Pod 清單？](/docs/services/cloud-monitoring/qa/qa_containers.html#qa8)
* [如何依名稱空間取得叢集中的所有 Pod？](/docs/services/cloud-monitoring/qa/qa_containers.html#qa9)

## 我容器中的 Grafana 查詢未顯示資料，或發生錯誤
{: #metric_format_change}

您可為容器定義的查詢格式已變更。

下列格式已遭淘汰： 

```
{Prefix}.{Version}.{Provider}.{Type}.{ServiceName}.{Region}.{Account}.{Cluster}.{Metric}.{Container in a pod}.{Functions}
```
{: screen}

有效的新格式如下：

* [容器的 CPU 度量值查詢格式](/docs/services/cloud-monitoring/reference/metrics_format.html#cpu_containers)
* [工作者的負載度量值查詢格式](/docs/services/cloud-monitoring/reference/metrics_format.html#load_workers)
* [容器的記憶體度量值查詢格式](/docs/services/cloud-monitoring/reference/metrics_format.html#mem_containers)

將舊查詢移轉為新格式，以在 Grafana 中視覺化資料。

例如，如果您的查詢開頭為 `crn.v1.bluemix.public.containers-kubernetes.us-south.`，則必須將查詢移轉為新格式，亦即，`ibmcloud.public.containers-kubernetes.us-south`。

下表列出具有已淘汰格式的欄位：

 <table>
      <caption>表 1. 容器的 Grafana 查詢欄位</caption>
      <tr>
        <th align="center">欄位</th>
        <th align="center">說明</th>
        <th align="center">有效值</th>
      </tr>
      <tr>
        <td>字首</td>
        <td>字首用來表示它是 {{site.data.keyword.IBM_notm}} Cloud 資源。</td>
        <td>`crn`</td>
      </tr>
      <tr>
        <td>版本</td>
        <td>版本用於指出所收集度量值資料的格式及欄位。</td>
        <td>`v1`</td>
      </tr>
      <tr>
        <td>提供者</td>
        <td>收集其中資料的雲端提供者。此欄位是英數 ID。</td>
        <td>對於公用 {{site.data.keyword.IBM_notm}} Cloud，值為：`bluemix`</td>
      </tr>
      <tr>
        <td>類型</td>
        <td>收集其中資料的雲端環境。</td>
        <td>對於公用 {{site.data.keyword.IBM_notm}} Cloud，值為：`public`</td>
      </tr>
      <tr>
        <td>服務名稱</td>
        <td>在其中收集度量值的雲端基礎架構。</td>
        <td>對於 {{site.data.keyword.monitoringshort}} 服務，值為：`containers-kubernetes`</td>
      </tr>
      <tr>
        <td>地區</td>
        <td>在其中收集度量值的「雲端」地區。</td>
        <td>對於「美國南部」地區，值為：`ng` <br>對於「英國」地區，值為：`eu-gb` <br>對於「德國」，值為：`eu-de` <br>對於「雪梨」，值為：`au-syd` </td>
      </tr>
      <tr>
        <td>帳戶</td>
        <td>在其中收集度量值之帳戶的 GUID。<br>此欄位的格式如下：`a_ID`，其中 ID 是帳戶的 GUID。<br>若要取得帳戶的 GUID，請參閱[如何取得帳戶的 GUID](/docs/services/cloud-monitoring/qa/cli_qa.html#account_guid)。</td>
        <td>a_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx</td>
      </tr>
      <tr>
        <td>叢集</td>
        <td>在其中收集度量值之叢集的 GUID。</td>
        <td>xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx</td>
      </tr>
      <tr>
        <td>度量值</td>
        <td>自動收集的容器度量值。</td>
        <td>如需 CPU 度量值的清單，請參閱[容器的 CPU 度量值](/docs/services/cloud-monitoring/containers/monitoring_containers_ov.html#cpu_metrics_containers) <br>如需記憶體度量值的清單，請參閱[記憶體度量值](/docs/services/cloud-monitoring/containers/monitoring_containers_ov.html#memory_metrics) </td>
      </tr>
      <tr>
        <td>POD 中的容器</td>
        <td>Kubernetes 資源名稱及 GUID 的組合，需有此組合，才能專門識別 POD 中執行的容器。<br>**附註：**當您查看查詢中此項目的可用選項清單時，請注意，還有下列格式的項目：{namespace}{pod_name}{deploymentname_name}_POD{container_ID}。此項目對應於 Kubernetes 所建立的內部容器 ID。</td>
		<td>{namespace}_{pod_name}_{deployment_name}_{container_id}</td>
      </tr>
      <tr>
        <td>函數</td>
        <td>您可以選取以視覺化畫面中之容器度量值的查詢函數。<br>如需相關資訊，請參閱 [Functions ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://graphite.readthedocs.io/en/latest/functions.html){: new_window}</td>
        <td></td>
      </tr>
    </table>


## 如何在終端機階段作業中設定叢集環境？
{: #qa1}

您必須使用指令，以設定 Kubernetes 叢集環境定義以進行管理。

**附註：**若要管理叢集，您需要 {{site.data.keyword.containershort_notm}} 服務的 IAM 原則，以將完成作業所需的許可權指派給使用者。

請完成下列步驟，以設定叢集的環境定義：

1. 登入 {{site.data.keyword.Bluemix_notm}} 中與所建立叢集相關聯的地區、組織及空間。如需相關資訊，請參閱[如何登入 {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#login)。

2. 起始設定 {{site.data.keyword.containershort_notm}} 服務外掛程式。執行下列指令：

	```
	bx cs init
	```
	{: codeblock}

3. 在終端機階段作業中，設定叢集的環境定義。請執行下列指令：
    
	```
	bx cs cluster-config MyCluster
	```
	{: codeblock}

    此指令的執行輸出提供您必須在終端機中執行的指令，以設定配置檔的路徑。例如：

	```
	export KUBECONFIG=/Users/ibm/.bluemix/plugins/container-service/clusters/MyCluster/kube-config-hou02-MyCluster.yml
	```
	{: codeblock}

    複製並貼上指令，以在終端機中設定環境變數，然後按 **Enter**。

 
 
## 如何找到與叢集相關聯之空間的空間 ID 及空間名稱？
{: #qa2}

在 {{site.data.keyword.Bluemix_notm}} 帳戶中建立叢集時，度量值會與該帳戶內的空間相關聯。當您建立查詢以檢視叢集度量值時，需要空間 ID。
{:shortdesc}

**附註：**若要管理叢集，您需要 {{site.data.keyword.containershort_notm}} 服務的 IAM 原則，以將完成作業所需的許可權指派給使用者。

若要尋找叢集的空間 ID，請完成下列步驟：

1. 登入 {{site.data.keyword.Bluemix_notm}} 中與所建立叢集相關聯的地區、組織及空間。如需相關資訊，請參閱[如何登入 {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#login)。

2. 起始設定 {{site.data.keyword.containershort_notm}} 服務外掛程式。執行下列指令：

	```
	bx cs init
	```
	{: codeblock}

3. 取得叢集詳細資料。執行下列指令：

    ```
    bx cs cluster-get cluster-name
    ```
    {: codeblock}

    其中 **cluster-name** 是叢集的名稱。

    空間 ID 是針對**日誌空間**欄位所指出的值。

    例如，執行指令的輸出如下：

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

4. 取得空間名稱。

    列出所有空間名稱：
	
	```
	bx account spaces
	```
	{: codeblock}
	
	會針對每一個空間執行下列指令，直到您找到相符 ID 的名稱：
	
	```
	bx iam space Spacename --guid
	```
	{: codeblock}
	
	


## 我可以在何處取得叢集名稱及叢集 ID？
{: #qa3}

請完成下列步驟，透過使用者介面取得叢集名稱及 ID：

1. 登入 {{site.data.keyword.Bluemix_notm}} 帳戶。

    {{site.data.keyword.Bluemix_notm}} 儀表板可以在下列網址找到：[http://bluemix.net ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://bluemix.net){:new_window}。
    
	使用您的使用者 ID 和密碼登入之後，{{site.data.keyword.Bluemix_notm}} 使用者介面隨即開啟。

2. 從 {{site.data.keyword.Bluemix_notm}} *儀表板*中，選取**功能表 > 容器**。

    即會顯示帳戶中的可用叢集清單。

3. 若要取得叢集 ID，請選取叢集項目。 

    即會開啟概觀頁面。在此頁面中，您可以取得叢集 ID。



請完成下列步驟，透過指令行取得叢集名稱及 ID：

1. 登入 {{site.data.keyword.Bluemix_notm}} 中與所建立叢集相關聯的地區、組織及空間。如需相關資訊，請參閱[如何登入 {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#login)。

2. 列出帳戶中的可用叢集。執行下列指令： 

	```
	bx cs clusters
	``` 
	{: codeblock}
	
	此指令的輸出會列出帳戶中具有其對應 ID 的所有叢集。
	
3. 若要取得叢集 ID，請執行下列指令：

	```
	bx cs cluster-get my_cluster
	```
	{: screen}	
 


## 如何取得名稱空間清單？
{: #qa7}

若要取得叢集中的所有名稱空間清單，請完成下列步驟：

請完成下列步驟：

1. 設定叢集環境定義。如需相關資訊，請參閱[如何在終端機階段作業中設定叢集環境？](/docs/services/cloud-monitoring/qa/qa_containers.html#qa1)。

2. 列出所有名稱空間。請執行下列 kubectl 指令：

	```
	kubectl get namespaces
	```
	{: codeblock}

## 如何取得 Kubernetes 叢集中每個名稱空間的 Pod 清單？
{: #qa8}
		
若要取得名稱空間中的 Pod 清單，請完成下列步驟：

1. 設定叢集環境定義。如需相關資訊，請參閱[如何在終端機階段作業中設定叢集環境？](/docs/services/cloud-monitoring/qa/qa_containers.html#qa1)。

2. 若要取得 Kubernetes 叢集中每個名稱空間的 Pod 清單，請執行下列指令：

	```
	kubectl --namespace=NamespaceName get pods 
	```
	{: codeblock}
	
	其中 Namespacename 是名稱空間名稱（例如，*default*）。

## 如何依名稱空間取得叢集中的所有 Pod？
{: #qa9}
		
請完成下列步驟：

1. 設定叢集環境定義。如需相關資訊，請參閱[如何在終端機階段作業中設定叢集環境？](/docs/services/cloud-monitoring/qa/qa_containers.html#qa1)。
	
2. 若要依名稱空間取得叢集中的所有 Pod，請執行下列指令：

	```
	kubectl get pods --all-namespaces
	```
	{: codeblock}
		


