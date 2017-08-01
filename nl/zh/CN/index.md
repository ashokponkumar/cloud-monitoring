---

copyright:
  years: 2017
lastupdated: "2017-06-26"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# IBM Cloud Monitoring in Bluemix 入门
{: #getting-started-with-ibm-cloud-monitoring}

本入门教程将引导您逐步完成使用 {{site.data.keyword.monitoringlong}} 服务分析容器的步骤。了解如何针对在 Kubernetes 集群中部署的应用程序搜索并分析容器度量值。
{:shortdesc}

## 开始之前
{: #prereqs}

创建 [Bluemix 帐户](https://console.bluemix.net/registration/)。您的用户标识必须是 Bluemix 帐户的成员或所有者，并具有创建 Kubernetes 集群、将应用程序部署至集群，以及查询 Bluemix 中的日志以在 Grafana 中进行高级分析的许可权。

打开终端会话，您可以从中管理 Kubernetes 集群并通过命令行部署应用程序。此教程的示例供 Ubuntu Linux 系统使用。

在本地环境中[安装 CLI 插件](/docs/containers/cs_cli_install.html#cs_cli_install_steps)，以通过命令行管理 IBM Bluemix Container Service。 



## 步骤 1：在容器中部署应用程序
{: #step1}

要在 Kubernetes 集群中部署容器，请完成以下步骤：

1. [创建 Kubernetes 集群](/docs/containers/cs_cluster.html#cs_cluster_ui)。

2. 在 Linux 终端中[设置集群上下文](/docs/containers/cs_cli_install.html#cs_cli_configure)。设置上下文后，您可以管理 Kubernetes 集群并在 Kubernetes 集群中部署应用程序。

3. 在 Kubernetes 集群中部署并运行样本应用程序。[完成第 1 课的步骤](/docs/containers/cs_tutorials.html#cs_apps_tutorial)。

    该应用程序是 Hello World Node.js 应用程序：

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

    部署应用程序时，会自动启用度量值收集。



## 步骤 2：导航至 Grafana 仪表板
{: #step2}

通过浏览器启动 Grafana。 

要分析集群的度量值，您必须在创建集群的云公共区域中访问 Grafana。 
    
然后，通过浏览器启动以下 URL 以打开 Grafana：`https://metrics.ng.bluemix.net/`
    
    
## 步骤 3：在 Grafana 中分析度量值
{: #step3}

要创建 Grafana 仪表板，请完成以下步骤：
    
1. 创建新的仪表板。

    * 选择侧菜单栏切换 ![Grafana 侧菜单栏](images/grafana_settings.gif "Grafana 侧菜单栏")。 
    * 选择**仪表板**。 
    * 单击**新建**。
    
    这将打开仪表板。仪表板包含一个空行，可随时用于配置。 
    
    ![Grafana 仪表板](images/grafana4_f1.gif "Grafana 仪表板")
    
     在 Grafana 中，通过添加行可将仪表板分成不同部分。一行可将一个或多个面板分组在一起。在一行内，面板是最小的可视化单元，可以配置为显示度量值的数据，例如可以选择图形面板或表面板。您可以通过拖放面板在仪表板中重新排列面板。面板显示的数据可通过查询进行配置。可以在一个面板中定义一个或多个查询。每个查询表示一组不同的数据。还可以设置面板的时间范围。通常，时间范围由*仪表板*时间选取器进行设置。
    
2. 添加*图形*面板以针对容器监视所有核心中 CPU 时间的纳秒数。
    
    1. 选择**图形**。
    
    2. 单击图形标题，然后选择**编辑**。
    
        这将打开*度量值*选项卡。在此可以看到缺省数据源。
    
        ![包含配置选项卡的图形面板](images/grafana4_f2.gif "包含配置选项卡的图形面板")
    
3. 定义用于过滤图形中所显示数据的查询。 

    下表概述了在配置用于过滤容器度量值的数据的查询时，所必需的不同字段：

    <table>
      <caption>表 1. 容器的 Grafana 查询字段</caption>
      <tr>
        <th align="center">字段</th>
        <th align="center">描述</th>
        <th align="center">有效值</th>
      </tr>
      <tr>
        <td>前缀</td>
        <td>容器度量值的前缀。<br><br>此前缀应用于为 Kubernetes 集群中部署的容器收集的数据。</td>
        <td>`crn`</td>
      </tr>
      <tr>
        <td>版本</td>
        <td>收集的度量值数据的版本。</td>
        <td>`v1`</td>
      </tr>
      <tr>
        <td>提供者</td>
        <td>在其中收集数据的云提供者。</td>
        <td>`bluemix`</td>
      </tr>
      <tr>
        <td>类型</td>
        <td>在其中收集数据的云环境。</td>
        <td>`public`</td>
      </tr>
      <tr>
        <td>源</td>
        <td>在其中收集度量值的云基础架构。</td>
        <td>`containers-kubernetes`</td>
      </tr>
      <tr>
        <td>区域</td>
        <td>在其中收集度量值的云区域。</td>
        <td>* `ng` <br>* `eu-gb` <br>* `eu-de` </td>
      </tr>
      <tr>
        <td>帐户</td>
        <td>收集度量值的帐户的 GUID。<br>此字段的格式如下：`a_*ID*`，其中 ID 是帐户的 GUID。</td>
        <td></td>
      </tr>
      <tr>
        <td>集群</td>
        <td>收集度量值的集群的 GUID。</td>
        <td></td>
      </tr>
      <tr>
        <td>容器度量值</td>
        <td>为容器收集的度量值。</td>
        <td>* `memory_current` <br>* `memory_limit` <br>* `cpu_usage` <br>* `cpu_usage_pct` <br>* `cpu_num_cores`</td>
      </tr>
      <tr>
        <td>pod 中的容器</td>
        <td>Kubernetes 资源名称和 GUID 的组合，此组合对于唯一标识在 pod 中运行的容器是必需的。<br> 此字段的格式如下：*{namespace}_#{pod_name}_#{container_name}_#{container_id}* <br><br>**注**：查看查询中此条目的可用选项列表时，请注意还有一个以下格式的条目：*{namespace}_#{pod_name}_#{container_name}_POD_#{container_id}*。此条目对应于 Kubernetes 创建的内部容器标识。</td>
        <td></td>
      </tr>
      <tr>
        <td>函数</td>
        <td>可以选择用于在面板中可视化容器度量值的查询函数。<br>有关更多信息，请参阅[函数 ![（外部链接图标）](../icons/launch-glyph.svg "外部链接图标")](http://graphite.readthedocs.io/en/latest/functions.html "外部链接图标"){: new_window}</td>
        <td></td>
      </tr>
    </table>
    
    下图显示了配置查询时，该查询如何在 Grafana 中显示：
    
    ![样本查询](images/grafana4_query_f3.gif "样本查询")
    
    要定义查询，请完成以下步骤：
    
    在*度量值*选项卡中，选择**添加查询**。<br>这将添加一个查询条目。每个查询都会使用一个字母进行标记。
    
    ![新建查询条目](images/grafana4_query_f1.gif "新建查询条目")
        
    1. 单击**选择度量值**，然后选择 `crn`。
    
    2. 单击**选择度量值**，然后选择 `v1`。
    
    3. 单击**选择度量值**，然后选择 `bluemix`。
    
    4. 单击**选择度量值**，然后选择 `public`。
    
    5. 单击**选择度量值**，然后选择 `containers-kubernetes`。
    
    6. 单击**选择度量值**，然后选择您在其中工作的区域，例如 `us-south`。
    
    7. 单击**选择度量值**，然后选择要显示其数据的帐户标识，例如 `a_91d1d1exxxxxxx4df920bbd06461b066`。
    
    8. 单击**选择度量值**，然后选择集群标识。
    
    9. 单击**选择度量值**，然后选择容器度量值。要监视容器的 *CPU 使用率*，请选择 `container-metric-cpu_usage`。
    
    10. 单击**选择度量值**，然后选择对应于要监视其 CPU 使用率的容器的标识，例如 `default_hello-world-deployment-3355293961-0fwkg_hello-world-deployment_ad5eb446a493db31f1d9eb158f5de915fc063d6c136823488b694e63bb00aa57`。
    
    11. 单击加号图像 ![“添加”图标](images/grafana_plus_image.gif "加号图像")，然后选择函数。可以添加函数来对可用于度量值的数据执行变换、组合和计算。
        
        例如，可以添加 **alias(newName)** 函数来为度量值添加别名。此别名用于在图形中所显示的图注中显示字符串，而不显示度量值名称。
        
        要为度量值添加别名，请完成以下步骤：
        
        1. 单击加号。
        2. 选择**特殊**。3 选择**别名**。
        4. 输入字符串，例如 `My sample metric`。
        
4. 保存仪表板以供将来复用。 

    1. 单击保存仪表板图像 ![保存仪表板图像](images/grafana_save_image.gif "保存仪表板图像")。 
    
        ![保存仪表板](images/grafana_save_dashboard.gif "保存仪表板")
    
    2. 输入仪表板的名称。
    3. 单击**保存**。


## 后续步骤
{: #next_steps}

为度量值定义警报。有关更多信息，请参阅[配置警报](/docs/services/cloud-monitoring/config_alerts_ov.html#config_alerts_ov)。



