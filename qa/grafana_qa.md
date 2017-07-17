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


# Frequent questions and answers when using Grafana
{: #grafana_qa}

Here are the answers to common questions about using Grafana with the {{site.data.keyword.monitoringshort}} service. 
{:shortdesc}

* [I get a 404 when I log in to the Monitoring service web UI](/docs/services/cloud-monitoring/qa/grafana_qa.html#404)
* [I just uploaded json data for a Grafana dashboard, why did I lose my scrollbar?](/docs/services/cloud-monitoring/qa/grafana_qa.html#2)


## I get a 404 when I log in to the Monitoring service web UI by using the UUA auth model
{: #404}

If you get a 404 when you try to log in to {{site.data.keyword.monitoringshort}} service web UI (Grafana), check that the space exists, and that you have access to it.

When you use the [UAA authentication model](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_uaa) to access the {{site.data.keyword.monitoringshort}} service, you must use the same user ID and password that you use to log in to the {{site.data.keyword.Bluemix_notm}} console. 

To verify that you have access to the account, organization, and space where you want to log in, log in to the {{site.data.keyword.Bluemix_notm}} console and switch to the space. 

You can also use the command line to verify that you have access to that space. Run the following command to log in to a {{site.data.keyword.Bluemix_notm}} region, organization, and space:

```
bx login -a https://api.ng.bluemix.net
```
{: codeblock}

Follow the instructions. Enter your {{site.data.keyword.Bluemix_notm}} credentials, select an organization and a space.


## I just uploaded JSON data for a Grafana dashboard, why did I lose my scrollbar?
{: #2}

There is a bug in Grafana that hides the scrollbar after you import a dashboard. 

To see the scrollbar, save the dashboard after importing it. 






