---

copyright:
  years: 2017, 2018

lastupdated: "2018-04-27"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}



# {{site.data.keyword.messagehub}}
{: #monitoring_mh_ov}

In the {{site.data.keyword.Bluemix}}, {{site.data.keyword.messagehub}} metrics are collected automatically. The number of bytes in and out of a topic are collected. A checkpoint is taken every 15 minutes. You can use Grafana to visualize these metrics. 
{:shortdesc}


**Note:** {{site.data.keyword.messagehub}} metrics are not available in {{site.data.keyword.IBM_notm}} Dedicated environments.



## Viewing and analyzing metrics
{: #view}

To monitor the performance of {{site.data.keyword.messagehub}} in the {{site.data.keyword.Bluemix_notm}}, use Grafana. {{site.data.keyword.messagehub}} provides a dashboard that you can use to view and monitor the performance of your topics.

The {{site.data.keyword.monitoringlong}} service uses Grafana, an open source analytics and visualization platform, that you can use to monitor, search, analyze, and visualize your metrics in a variety of graphs, for example charts and tables. 

You can launch Grafana in any of the following ways:

* You can click the **Grafana** button on the {{site.data.keyword.messagehub}} dashboard in the {{site.data.keyword.Bluemix_notm}} console.

    Grafana opens within the context of the space in the {{site.data.keyword.Bluemix_notm}} where {{site.data.keyword.messagehub}} is running.
    
* You can launch Grafana directly from a web browser.

    Check that you are in the correct organization and space where the {{site.data.keyword.messagehub}} instance is running.
    
    For more information, see [Navigating to the Grafana dashboard from a web browser](/docs/services/cloud-monitoring/grafana/navigating_grafana.html#launch_grafana_from_browser).
    

Consider the following information:

* You must launch Grafana in the same {{site.data.keyword.Bluemix_notm}} region where the {{site.data.keyword.messagehub}} instance is running.
* Use the Grafana dashboard provided by default to start monitoring your {{site.data.keyword.messagehub}} instance.
* Create custom Grafana dashboards to build ad-hoc dashboards. You can define one or more metric queries in a Grafana dashboard to monitor a {{site.data.keyword.messagehub}} instance. For more information, see [Configuring a metric query in Grafana](/docs/services/cloud-monitoring/grafana/define_query.html#define_query).
* You can also define alerts on queries. For more information, see [Configuring alerts](/docs/services/cloud-monitoring/config_alerts_ov.html#config_alerts_ov).


## Metrics for a Kafka topic
{: #kafka_topic_metrics}

For each Kafka topic that is defined, the following metrics are collected:


<table>
  <caption>Metrics per Kafka topic</caption>
  <tr>
    <th>Metric</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>BytesInPerSec</td>
    <td>Rate of bytes sent to topic by Kafka client applications.</td>
  </tr>
  <tr>
    <td>BytesOutPerSec</td>
    <td>Rate of bytes received from a topic by Kafka client applications.</td>
  </tr>
</table>



## Metrics for a Kafka partition
{: #kafka_partition_metrics}

For each Kafka partition that has a Cloud Storage bridge that consumes messages, the following metrics are collected:


<table>
  <caption>Kafka partition metrics</caption>
  <tr>
    <th>Metric</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>lastSeen</td>
    <td>The offset for the last Kafka record that is processed by the Cloud Storage bridge.</td>
  </tr>
  <tr>
    <td>lastCommitted</td>
    <td>The committed offset for Kafka records that are processed by the Cloud Storage bridge.</td>
  </tr>
  <tr>
    <td>notParsedButProcessedMessages</td>
    <td>Number of messages that are received by a Cloud Storage bridge instance from a Kafka topic. Counts only the messages that contain valid JSON that cannot be processed by the bridge.</td>
  </tr>
  <tr>
    <td>notParsedIgnoredMessages</td>
    <td>Number of messages that are received by a Cloud Storage bridge instance from a Kafka topic. Counts only messages that contain data which could not be parsed because the data is not a valid JSON document.</td>
  </tr>
</table>




## References
{: #mhlinks}

* [Getting started with Message Hub](/docs/services/MessageHub/index.html#messagehub)
* [Monitoring and logging](/docs/services/MessageHub/messagehub072.html#monitoring)