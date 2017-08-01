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


# 使用 Grafana 的常见问题及解答
{: #grafana_qa}

下面是对将 Grafana 与 {{site.data.keyword.monitoringshort}} 服务一起使用时的常见问题的解答。
{:shortdesc}

* [登录到 Monitoring 服务 Web UI 时收到 404](/docs/services/cloud-monitoring/qa/grafana_qa.html#404)
* [我刚上传了 Grafana 仪表板的 JSON 数据，为什么滚动条不见了？](/docs/services/cloud-monitoring/qa/grafana_qa.html#2)


## 使用 UAA 认证模型登录到 Monitoring 服务 Web UI 时收到 404
{: #404}

如果尝试登录到 {{site.data.keyword.monitoringshort}} 服务 Web UI (Grafana) 时收到 404，请检查空间是否存在，以及您是否有权访问该空间。

使用 [UAA 认证模型](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_uaa)来访问 {{site.data.keyword.monitoringshort}} 服务时，必须使用登录到 {{site.data.keyword.Bluemix_notm}} 控制台时所用的相同用户标识和密码。 

要验证您是否有权访问要登录到的帐户、组织和空间，请登录到 {{site.data.keyword.Bluemix_notm}} 控制台并切换到该空间。 

还可以使用命令行来验证您是否有权访问该空间。运行以下命令来登录到 {{site.data.keyword.Bluemix_notm}} 区域、组织和空间：

```
bx login -a https://api.ng.bluemix.net
```
{: codeblock}

遵循指示信息进行操作。输入您的 {{site.data.keyword.Bluemix_notm}} 凭证，然后选择组织和空间。


## 我刚上传了 Grafana 仪表板的 JSON 数据，为什么滚动条不见了？
{: #2}

这是 Grafana 中的一个错误，它会在导入仪表板后隐藏滚动条。 

要显示滚动条，请在导入仪表板后保存该仪表板。 








