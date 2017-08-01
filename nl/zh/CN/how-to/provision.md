---

copyright:
  years: 2017

lastupdated: "2017-07-10"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# 供应 Monitoring 服务
{: #provision}

您可以从 {{site.data.keyword.Bluemix}} UI 或从命令行供应 {{site.data.keyword.monitoringshort}} 服务。
{:shortdesc}


## 从 UI 供应
{: #ui}

完成以下步骤以在 {{site.data.keyword.Bluemix_notm}} 中供应 {{site.data.keyword.monitoringshort}} 服务的实例：

1. 登录到 {{site.data.keyword.Bluemix_notm}} 帐户。

    可以在以下网址处找到 {{site.data.keyword.Bluemix_notm}} 仪表板：[http://bluemix.net ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](http://bluemix.net "外部链接图标"){:new_window}。
    
	使用用户标识和密码登录后，{{site.data.keyword.Bluemix_notm}} UI 即会打开。

2. 单击**目录**。{{site.data.keyword.Bluemix_notm}} 上可用的服务列表即会打开。

3. 选择 **DevOps** 类别以过滤显示的服务列表。

4. 单击**监视**磁贴。

5. 选择服务套餐。要收集并访问最多 45 天的度量值，同时定义警报规则（包括带有通配符的规则），请选择**高级**套餐。缺省情况下，会设置 **Lite** 套餐，通过该套餐，您可以收集供应服务所在空间中的平台度量值，并保留这些度量值 15 天。 

    有关服务套餐的更多信息，请参阅[服务套餐](/docs/services/cloud-monitoring/monitoring_ov.html#plans)。
	
6. 单击**创建**以在您登录的 {{site.data.keyword.Bluemix_notm}} 空间中供应 {{site.data.keyword.monitoringshort}} 服务。
  
 

## 从 CLI 供应
{: #cli}

完成以下步骤，以便通过命令行在 {{site.data.keyword.Bluemix_notm}} 中供应 {{site.data.keyword.monitoringshort}} 服务的实例：

1. [先决条件] 安装 {{site.data.keyword.Bluemix_notm}} CLI。

   有关更多信息，请参阅[安装 {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa)。
   
   如果 CLI 已安装，请继续执行下一步。
    
2. 登录到 {{site.data.keyword.Bluemix_notm}} 区域、组织和空间。运行以下命令：

    ```
    cf login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    遵循指示信息进行操作。输入您的 {{site.data.keyword.Bluemix_notm}} 凭证，然后选择组织和空间。
	
3. 运行 `cf create-service` 命令以供应实例。

    ```
	cf create-service service_name service_plan service_instance_name
	```
	{: codeblock}
	
	其中
	
	* service_name 是服务的名称，即 **Monitoring**。
	* service_plan 是服务套餐名称。要收集并访问最多 45 天的度量值，同时定义警报规则（包括带有通配符的规则），请选择 **高级** 套餐。
缺省情况下，会设置 **Lite** 套餐，通过该套餐，您可以收集供应服务所在空间中的平台度量值，并保留这些度量值 15 天。要获取套餐列表，请参阅 [{{site.data.keyword.monitoringshort}} 服务套餐](/docs/services/cloud-monitoring/monitoring_ov.html#plan)。
 * service_instance_name 是要用于创建的新服务实例的名称。
	
	有关 cf 命令的更多信息，请参阅 [cf create-service](/docs/cli/reference/cfcommands/index.html#cf_create-service)。

	例如，要使用高级套餐创建 {{site.data.keyword.monitoringshort}} 服务的实例，请运行以下命令：
	
	```
	cf create-service Monitoring premium my_monitoring_svc
	```
	    {: codeblock}
	
    4. 验证是否已成功创建服务。运行以下命令：

    ```	
	cf services
	```
	{: codeblock}
	
	运行命令的输出如下所示：
	
	```
    在组织 MyOrg / 空间 MySpace 中获取服务作为 xxx@yyy.com...
    正常
    
    名称                           服务                  套餐                   界限应用程序              上次操作
    my_monitoring_svc              Monitoring               高级                                        成功创建
	```
	{: screen}



 
	  



