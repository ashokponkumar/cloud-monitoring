---

copyright:
  years: 2017, 2018

lastupdated: "2018-02-09"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}



# FAQ using Grafana
{: #grafana_qa}

Here are the answers to common questions about using Grafana with the {{site.data.keyword.monitoringshort}} service. 
{:shortdesc}

* [I cannot see alerts that I have defined by using the Alerts API in my Grafana dashboard](/docs/services/cloud-monitoring/qa/grafana_qa.html#alerts1)
* [I get a BXNMSAL41E when I try to save a change that I have made to my Grafana dashboard](/docs/services/cloud-monitoring/qa/grafana_qa.html#BXNMSAL41E)
* [I get a BXNMSAL36E when I try to save a change after I add an alert to my Grafana dashboard](/docs/services/cloud-monitoring/qa/grafana_qa.html#BXNMSAL36E)
* [I get a 404 when I log in to the Monitoring service web UI](/docs/services/cloud-monitoring/qa/grafana_qa.html#404)
* [I just uploaded json data for a Grafana dashboard, why did I lose my scrollbar?](/docs/services/cloud-monitoring/qa/grafana_qa.html#2)


## I cannot see alerts that I have defined by using the Alerts API in my Grafana dashboard
{: #alerts1}

Alerts that you define by using the Alert API do not show in the alerts tab in Grafana. To see alerts in Grafana, you must define them directly in a Grafana dashboard.

For more information, see [Configuring alerts in Grafana](/docs/services/cloud-monitoring/alerts/config_alerts_grafana.html#config_alerts_grafana).

## I get a BXNMSAL41E when I try to save a change that I have made to my Grafana dashboard
{: #BXNMSAL41E}

You can define notification channels and alerts in Grafana. If you delete a notification channel in Grafana, and you do not update the rule to remove that notification channel, you get a **BXNMSAL41E** error when you try to save your Grafana dashboard.

To fix the problem, update the rule by using the Alerts API and retry saving the dashboard. When you update the rule, remove the notification channel that has been deleted.

For more information, see [Alerts API](https://console.bluemix.net/apidocs/940-ibm-cloud-monitoring-alerts-api?&language=node#introduction).

## I get a BXNMSAL36E when I try to save a change after I add an alert to my Grafana dashboard
{: #BXNMSAL36E}

If you reach the quota for the domain where you are monitoring metrics in the {{site.data.keyword.monitoringshort}} service, you get the following error: **BXNMSAL36E**

Upgrade your plan, and retry again.

For more information on how to upgrade your plan, see [Changing the plan](/docs/services/cloud-monitoring/plan/change_plan.html#change_plan).


## I get a 404 when I log in to the Monitoring service web UI by using the UUA auth model
{: #404}

If you get a 404 when you try to log in to {{site.data.keyword.monitoringshort}} service web UI (Grafana), check that the space exists, and that you have access to it.

When you use the [UAA authentication model](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_uaa) to access the {{site.data.keyword.monitoringshort}} service, you must use the same user ID and password that you use to log in to the {{site.data.keyword.Bluemix_notm}} console. 

To verify that you have access to the account, organization, and space where you want to log in, log in to the {{site.data.keyword.Bluemix_notm}} console and switch to the space. 

You can also use the command line to verify that you have access to that space. Run the following command to log in to a region, organization, and space in the {{site.data.keyword.Bluemix_notm}}:

```
bx login -a https://api.ng.bluemix.net
```
{: codeblock}

Follow the instructions. Enter your {{site.data.keyword.Bluemix_notm}} credentials, select an organization and a space.


## I just uploaded JSON data for a Grafana dashboard, why did I lose my scrollbar?
{: #2}

There is a bug in Grafana that hides the scrollbar after you import a dashboard. 

To see the scrollbar, save the dashboard after importing it. 






