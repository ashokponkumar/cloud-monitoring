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

# Configuring alerts in Grafana
{: #config_alerts_grafana}

The {{site.data.keyword.monitoringshort}} service provides a query-based alerting system. You can configure alerts in Grafana on dashboards that do not have template variables. 
{:shortdesc}

To configure an alert on a metric query through the Grafana UI, complete the following steps:

1. Launch Grafana.
2. Define one or more notification channels.
3. Create a Grafana dashboard that includes a Graph and at least one query metric. 
4. Configure the alert through the **Alerts** tab on a Grafana graph.

## Step 1: Launch Grafana
{: #step1}

Launch Grafana from a browser. For example, enter the following URL to open Grafana in the US South region: [https://metrics.ng.bluemix.net/](https://metrics.ng.bluemix.net/).

For a list of Grafana URLs per region, see [URLs for the {{site.data.keyword.monitoringshort}} service](/docs/services/cloud-monitoring/monitoring_ov.html#region).

## Step 2: Define one or more notification channels
{: #step2}

Complete the following steps:

1. In Grafana, select the side menubar.

2. Select **Alerting > Notification channels > New channel**.

3. Enter the data for the new channel. Add an email notificatiuon channel, for example.

<table>
  <caption>Email notification method and data that you must enter to create a new channel</caption>
  <tr>
     <th>Field</th>
     <th>Description</th>
  </tr>
  <tr>
    <td>Name</td>
    <td>Name of the notification channel. This value must be unique.</td>
  </tr>
  <tr>
    <td>Description</td>
    <td>Additional information you might want to include for documentation purposes.</td>
  </tr>
  <tr>
    <td>Type</td>
    <td>Select: **Email**</td>
  </tr>
  <tr>
    <td>Email ID</td>
    <td>Enter the email address of the recipient. </br>You can enter multiple email addresses separated by commas.</td>
  </tr>
</table>

<table>
  <caption>Webhook notification method and data that you must enter to create a new channel</caption>
  <tr>
     <th>Field</th>
     <th>Description</th>
  </tr>
  <tr>
    <td>Name</td>
    <td>Name of the notification channel. This value must be unique.</td>
  </tr>
  <tr>
    <td>Description</td>
    <td>Additional information you might want to include for documentation purposes.</td>
  </tr>
  <tr>
    <td>Type</td>
    <td>Select: **Webhook**</td>
  </tr>
  <tr>
    <td>Webhook POST URL</td>
    <td>Enter the URL where the POST should be made.</td>
  </tr>
  <tr>
    <td>Webhook headers</td>
    <td>Enter any headers.</td>
  </tr>
  <tr>
    <td>Webhook query parameters</td>
    <td>Enter any query parameters.</td>
  </tr>
</table>

<table>
  <caption>PagerDuty notification method and data that you must enter to create a new channel</caption>
  <tr>
     <th>Field</th>
     <th>Description</th>
  </tr>
  <tr>
    <td>Name</td>
    <td>Name of the notification channel. This value must be unique.</td>
  </tr>
  <tr>
    <td>Description</td>
    <td>Additional information you might want to include for documentation purposes.</td>
  </tr>
  <tr>
    <td>Type</td>
    <td>Select: **PagerDuty**</td>
  </tr>
  <tr>
    <td>PagerDuty API key</td>
    <td>Enter the PagerDuty key.</td>
  </tr>
</table>

## Step 3: Define a metric
{: #step3}

Complete the following steps to create a new dashboard:

1. Select the side menubar toggle ![Grafana sidemenu bar](images/grafana_settings.gif "Grafana sidemenu bar").
2. Select **Dashboards**.
3. Click **New**

A dashboard opens. The dashboard includes an empty row that is ready for configuration. 

Next, add a metric query:

1. Add a *Graph* panel.
2. Select **Graph**.
3. Click on the graph title, and then select **edit**.
    
    The *Metrics* tab opens. You can see here the default data source.
    
4. Define the metric query that filters the data that is displayed in the graph. 


## Step 4: Define an alert
{: #step4}

Complete the following steps to define an alert in the Grafana UI:

1. Select the **Alert** tab.
2. In the **Alert Config** section, define the rule that defines the conditions under which an alert notification is generated.

    Configure the **Evaluate every** field, and the **Conditions** section.

3. In the **Notifications** section, select one or more notification channels.

**Note:** 

* When a condition is set, you can see a red line on the graph to define the threshold set. You can drag it on the graph to change it.
* Once the alert is created, if you want to modify it, you must do it by using the Alerts API.
* If you select **Delete**, the alert is deleted.

## Next: Verify that an alert is generated
{: #next}

For example, if you created an email notification channel, check your email.

Look for emails with sender **IBM Cloud Monitoriing**.

An email includes information on the alert raised, and a link to the Grafana dashboard where you can view the situation.

For example:

```
Rule : grafana4_alerting-dashboard_1
Description : Alert created via Grafana 4 dashboard: alerting-dashboard on expression: ibmcloud.public.containers-kubernetes.eu-de.MyDemoCluster.worker.*.load.avg-15
Expression : ibmcloud.public.containers-kubernetes.eu-de.MyDemoCluster.worker.*.load.avg-15
Error Level : 0.8
Warn Level : 
Notification : email-channel
Dashboard URL : https://urldefense.proofpoint.com/v2/url?u=https-3A__metrics.eu-2Dde.bluemix.n......&s=Csta29jgJ8BsUZuJOeyX9G_6NoEnGi2RBbtIDFhjtfw&e=

Alerts:
Target: ibmcloud.public.containers-kubernetes.eu-de.MyDemoCluster.worker.kube-fra02-cr39f48d743e90451fa5a57d636796a489-w2.load.avg-15    From: WARN    To: OK    CurrentValue: 0.66    Comparison: above    Timestamp: 2018-02-06T17:34:14Z
```
{: screen}


