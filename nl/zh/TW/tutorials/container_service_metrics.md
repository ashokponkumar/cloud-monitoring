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


# 在 Grafana 中分析 Kubernetes 叢集中所部署應用程式的度量值
{: #container_service_metrics}

使用此指導教學，以學習如何使用 {{site.data.keyword.monitoringlong}} 服務監視容器效能。
{:shortdesc}


## 目標
{: #objectives}

學習如何搜尋及分析 Kubernetes 叢集中所部署應用程式的容器度量值：

1. 識別在何處將叢集中所收集的度量值轉遞至 {{site.data.keyword.monitoringshort}} 服務。 
2. 啟動 Grafana，並設定您可檢視叢集度量值的 {{site.data.keyword.monitoringshort} 網域。
3. 搜尋及分析 {{site.data.keyword.Bluemix_notm}} 的 Kubernetes 叢集中所部署應用程式的容器度量值。

本指導教學逐步引導您取得在 {{site.data.keyword.Bluemix_notm}} 中運作之下列完整情境所需的步驟：佈建叢集、識別在何處叢集將度量值傳送至 {{site.data.keyword.Bluemix_notm}} 中的 {{site.data.keyword.monitoringshort}} 服務、在叢集中部署應用程式，以及使用 Grafana 來檢視及過濾該叢集的容器度量值。


**附註：**若要完成此指導教學，您必須完成必要條件以及不同步驟中所鏈結的指導教學。


## 必要條件
{: #prereqs}

1. 是 {{site.data.keyword.Bluemix_notm}} 帳戶的成員或擁有者，並具有許可權可以建立 Kubernetes 標準叢集、將應用程式部署至叢集，以及查詢 {{site.data.keyword.Bluemix_notm}} 中 Grafana 監視的度量值。

    {{site.data.keyword.Bluemix_notm}} 的使用者 ID 必須已獲指派下列原則：
    
    * 具有 *operator* 或 *administrator* 許可權之 {{site.data.keyword.containershort}} 的 IAM 原則。
    * {{site.data.keyword.monitoringshort}} 服務已佈建 *developer* 許可權之空間的 CF 角色。
    
    如需相關資訊，請參閱[透過 IBM Cloud 使用者介面將 IAM 原則指派給使用者](/docs/services/cloud-monitoring/security/grant_permissions.html#grant_permissions_ui_account)及[使用 IBM Cloud 使用者介面將 CF 角色授與使用者](/docs/services/cloud-monitoring/security/grant_permissions.html#grant_permissions_ui_space)。

2. 具有終端機階段作業，您可以從中管理 Kubernetes 叢集，並從指令行部署應用程式。本指導教學中的範例是針對 Ubuntu Linux 系統。

3. 安裝 CLI，以在 Ubuntu 系統中使用 {{site.data.keyword.containershort}}。

    * 安裝 {{site.data.keyword.Bluemix_notm}} CLI。如需相關資訊，請參閱[安裝 {{site.data.keyword.Bluemix_notm}} CLI](/docs/cli/reference/bluemix_cli/download_cli.html#download_install)。
    
    * 安裝 {{site.data.keyword.containershort}} CLI，以在 {{site.data.keyword.containershort}} 中建立及管理 Kubernetes 叢集，以及將容器化應用程式部署至叢集。如需相關資訊，請參閱[安裝 CS 外掛程式](/docs/containers/cs_cli_install.html#cs_cli_install_steps)。
    

    
 

## 步驟 1：佈建 Kubernetes 叢集
{: #step1}

請完成下列步驟：

1. 建立標準 Kubernetes 叢集。

   * [透過使用者介面建立 Kubernetes 標準叢集](/docs/containers/cs_cluster.html#cs_cluster_ui)。
   * [使用 CLI 建立 Kubernetes 標準叢集](/docs/containers/cs_cluster.html#cs_cluster_cli)。

2. 在終端機中設定叢集環境定義。設定環境定義之後，您可以管理 Kubernetes 叢集，並在 Kubernetes 叢集中部署應用程式。

    登入 {{site.data.keyword.Bluemix_notm}} 中與所建立叢集相關聯的地區、組織及空間。如需相關資訊，請參閱[如何登入 {{site.data.keyword.Bluemix_notm}}](/docs/services/CloudLogAnalysis/qa/cli_qa.html#login)。

	起始設定 {{site.data.keyword.containershort}} 服務外掛程式。

	```
	bx cs init
	```
	{: codeblock}

    將終端機環境定義設為叢集。
    
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



## 步驟 2：識別叢集將度量值轉遞至 {{site.data.keyword.monitoringshort}} 服務的位置
{: #step2}

叢集是帳戶層次資源。當您在 {{site.data.keyword.containershort}} 上佈建叢集時，可以在帳戶層次建立叢集，或使用與其相關聯的 Cloud Foundry (CF) 空間來建立它們。只要叢集已佈建並備妥，就會自動收集度量值，並將其轉遞至 {{site.data.keyword.monitoringshort}} 服務。

* 具有相關聯 CF 空間的叢集會將度量值轉遞至空間度量值網域。
* 已在帳戶層次建立的叢集會將度量值轉遞至帳戶度量值網域。

若要識別叢集轉遞度量值的位置，請執行下列指令：

```
$ bx cs cluster-get ClusterName --json
```
{: codeblock}

其中 *ClusterName* 是您叢集的名稱。

在輸出中，下列欄位會提供度量值轉遞位置的相關資訊：

* **logOrg** 定義 CF 組織的 ID。
* **logOrgName** 定義 CF 組織的名稱。
* **logSpace** 定義 CF 空間的 ID。
* **logSpaceName** 定義 CF 空間的名稱。

如果欄位是空的，則會將度量值轉遞至帳戶網域。
如果欄位已設定 CF 組織及 CF 空間，則會將度量值轉遞至與此空間相關聯的空間網域。

例如，將度量值轉遞至帳戶網域之叢集的輸出如下：

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





## 步驟 3：將查看度量值網域中度量值的許可權授與使用者
{: #step3}

若要將檢視空間網域中度量值的許可權授與使用者，您必須將 CF 角色指派給該使用者，而這個角色說明此使用者可以使用空間中 {{site.data.keyword.monitoringshort}} 服務的動作。

若要將檢視帳戶網域中度量值的許可權授與使用者，您必須將 IAM 原則指派給該使用者，而這個原則說明此使用者可以使用 {{site.data.keyword.monitoringshort}} 服務的動作。

### 將查看空間網域中度量值的許可權授與使用者
{: #space}

請完成下列步驟，以將使用 {{site.data.keyword.monitoringshort}} 服務的許可權授與使用者：

1. 登入 {{site.data.keyword.Bluemix_notm}} 主控台。

    開啟 Web 瀏覽器，並啟動 {{site.data.keyword.Bluemix_notm}} 儀表板：[http://bluemix.net ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://bluemix.net){:new_window}
	
	使用您的使用者 ID 及密碼登入之後，即會開啟 {{site.data.keyword.Bluemix_notm}} 使用者介面。

2. 從功能表列中，按一下**管理 > 帳戶 > 使用者**。

    *使用者*視窗會顯示目前已選取帳戶的使用者及其電子郵件位址清單。
	
3. 如果使用者是帳戶成員，請從清單中選取使用者名稱，或按一下*動作*功能表中的**管理使用者**。

    如果使用者不是帳戶的成員，請參閱[邀請使用者](/docs/iam/iamuserinv.html#iamuserinv)。

4. 選取 **Cloud Foundry 存取**，然後選取**指派組織**。

5. 輸入下列值：

    <table>
      <caption></caption>
      <tr>
        <th>欄位</th>
        <th>值</th>
      </tr>
      <tr>
        <td>組織</td>
        <td>MyOrg</td>
      </tr>
      <tr>
        <td>組織角色</td>
        <td>無組織角色</td>
      </tr>
      <tr>
        <td>地區</td>
        <td>美國南部</td>
      </tr>
      <tr>
        <td>空間</td>
        <td>dev</td>
      </tr>
      <tr>
        <td>空間角色</td>
        <td>審核員</td>
      </tr>
	
6. 按一下**儲存角色**。

### 將查看帳戶網域中度量值的許可權授與使用者
{: #acc}

請完成下列步驟，以將使用 {{site.data.keyword.monitoringshort}} 服務的許可權授與使用者：

1. 登入 {{site.data.keyword.Bluemix_notm}} 主控台。

    開啟 Web 瀏覽器，並啟動 {{site.data.keyword.Bluemix_notm}} 儀表板：[http://bluemix.net ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://bluemix.net){:new_window}
	
	使用您的使用者 ID 及密碼登入之後，即會開啟 {{site.data.keyword.Bluemix_notm}} 使用者介面。

2. 從功能表列中，按一下**管理 > 帳戶 > 使用者**。

    *使用者*視窗會顯示目前已選取帳戶的使用者及其電子郵件位址清單。
	
3. 如果使用者是帳戶成員，請從清單中選取使用者名稱，或按一下*動作*功能表中的**管理使用者**。

    如果使用者不是帳戶的成員，請參閱[邀請使用者](/docs/iam/iamuserinv.html#iamuserinv)。

4. 選取**存取原則 > 指派存取權 > 指派資源的存取權**。

5. 選擇服務 **{{site.data.keyword.monitoringlong}}**、選取叢集可用地區（在此指導教學中是**美國南部**），然後選取角色 **viewer**。



## 步驟 4：授與 {{site.data.keyword.containershort_notm}} 金鑰擁有者許可權
{: #step4}

叢集將度量值轉遞至空間網域時，您也必須將 Cloud Foundry (CF) 許可權授與組織及空間中的 {{site.data.keyword.containershort}} 金鑰擁有者。金鑰擁有者需要有組織的 *orgManager* 角色，以及空間的 *SpaceManager* 及 *Developer*。

叢集將度量值轉遞至帳戶網域時，{{site.data.keyword.containershort}} 金鑰擁有者必須要有具有 {{site.data.keyword.monitoringshort}} 服務之管理者許可權的 IAM 原則。

### 授與查看空間網域中度量值的許可權
{: #space_1}

將下列許可權授與 {{site.data.keyword.containershort}} 金鑰擁有者使用者 ID：組織的 *orgManager* 角色，以及空間的 *SpaceManager* 及 *Developer*。請完成下列步驟：
    
1. 登入 {{site.data.keyword.Bluemix_notm}} 主控台。

    開啟 Web 瀏覽器，並啟動 {{site.data.keyword.Bluemix_notm}} 儀表板：[http://bluemix.net ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://bluemix.net){:new_window}
	
	使用您的使用者 ID 及密碼登入之後，即會開啟 {{site.data.keyword.Bluemix_notm}} 使用者介面。

2. 從功能表列中，按一下**管理 > 帳戶 > 使用者**。

    *使用者*視窗會顯示目前已選取帳戶的使用者及其電子郵件位址清單。
	
3. 尋找 {{site.data.keyword.containershort}} 金鑰擁有者使用者 ID。

    執行指令 `bx cs api-key-info ClusterName`，以取得 {{site.data.keyword.containershort}} 金鑰擁有者使用者 ID。

4. 選取 **Cloud Foundry 存取**，然後選取**指派組織**。

5. 輸入下列值： 

    <table>
      <caption></caption>
      <tr>
        <th>欄位</th>
        <th>值</th>
      </tr>
      <tr>
        <td>組織</td>
        <td>MyOrg</td>
      </tr>
      <tr>
        <td>組織角色</td>
        <td>管理員</td>
      </tr>
      <tr>
        <td>地區</td>
        <td>美國南部</td>
      </tr>
      <tr>
        <td>空間</td>
        <td>dev</td>
      </tr>
      <tr>
        <td>空間角色</td>
        <td>開發人員</td>
      </tr>
	
6. 按一下**儲存角色**。


### 授與查看帳戶網域中度量值的許可權
{: #acc_1}

請完成下列步驟：

1. 登入 {{site.data.keyword.Bluemix_notm}} 主控台。

    開啟 Web 瀏覽器，並啟動 {{site.data.keyword.Bluemix_notm}} 儀表板：[http://bluemix.net ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://bluemix.net){:new_window}
	
	使用您的使用者 ID 和密碼登入之後，{{site.data.keyword.Bluemix_notm}} 使用者介面隨即開啟。

2. 從功能表列中，按一下**管理 > 帳戶 > 使用者**。 

    *使用者*視窗會顯示目前已選取帳戶的使用者及其電子郵件位址清單。
	
3. 尋找 {{site.data.keyword.containershort}} 金鑰擁有者使用者 ID。

    執行指令 `bx cs api-key-info ClusterName`，以取得 {{site.data.keyword.containershort}} 金鑰擁有者使用者 ID。

4. 選取**存取原則 > 指派存取權 > 指派資源的存取權**。

5. 選擇服務 **{{site.data.keyword.monitoringlong}}**、選取叢集可用地區（在此指導教學中是**美國南部**），然後選取角色 **administrator**。	

## 步驟 5：在 Kubernetes 叢集中部署範例應用程式
{: #step5}

在 Kubernetes 叢集中部署及執行範例應用程式。完成下列指導教學中的步驟，以部署範例應用程式：[課程 1：將單一實例應用程式部署至 Kubernetes 叢集](/docs/containers/cs_tutorials_apps.html#cs_apps_tutorial_lesson1)。

應用程式是 Hello World Node.js 應用程式：

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

在此範例應用程式中，當您在瀏覽器中測試應用程式時，應用程式會將下列訊息寫入至 stdout：`Sample app is listening on port 8080`。


## 步驟 6：啟動 Grafana 並設定度量值網域
{: #step6}

從瀏覽器啟動 Grafana，並設定您可檢視叢集度量值的 {{site.data.keyword.monitoringshort}} 網域。

若要分析叢集的度量值，您必須在叢集建立所在的「雲端公用」地區中存取 Grafana。如需相關資訊，請參閱[從 Web 瀏覽器導覽至 Grafana 儀表板](/docs/services/cloud-monitoring/grafana/navigating_grafana.html#launch_grafana_from_browser)。

1. 從瀏覽器啟動 Grafana。 

    輸入您已建立叢集之地區的 {{site.data.keyword.monitoringshort}} 服務 URL。 
    
    若要取得每個地區的 URL，請參閱[監視服務的 URL](/docs/services/cloud-monitoring/monitoring_ov.html#region)。

    例如，對於「美國南部」地區，啟動：[https://metrics.ng.bluemix.net/](https://metrics.ng.bluemix.net/)。

2. 設定您可檢視叢集度量值的 {{site.data.keyword.monitoringshort} 網域。

    在 Grafana 中，選取 ID。然後，確認您使用正確的帳戶，並選擇網域。

    具有相關聯 CF 空間的叢集會將度量值轉遞至空間度量值網域。選取 `Domain = space`，以及與您叢集相關聯的組織及空間。

    已在帳戶層次建立的叢集會將度量值轉遞至帳戶度量值網域。選取 `Domain = account`

## 步驟 7：在 Grafana 中監視叢集
{: #step7}

{{site.data.keyword.containershort}} 提供 Grafana 儀表板，以用來監視叢集度量值。 

請完成下列步驟，以開啟範例儀表板：

1. 選取側邊功能表列切換 ![Grafana 側邊功能表列](images/grafana_settings.gif "Grafana 側邊功能表列")。
2. 選取**儀表板**。
3. 按一下**開啟**。
4. 選取 **ClusterMonitoringDashboard_v1**。

即會開啟範例儀表板。 

![Grafana 範例儀表板](images/cluster_grafana_sample_dashboard.png "Grafana 範例儀表板")



## 後續步驟
{: #next_steps}

定義度量值的警示。如需相關資訊，請參閱[配置警示](/docs/services/cloud-monitoring/config_alerts_ov.html#config_alerts_ov)。
