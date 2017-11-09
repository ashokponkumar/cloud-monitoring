---

copyright:
  years: 2017

lastupdated: "2017-11-09"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Frequent questions and answers
{: #qa}

Here are the answers to common questions about the {{site.data.keyword.monitoringshort}} service. 
{:shortdesc}

* [Why can I not see my old data for a metric that I haven not sent metrics values for recently?](#qa3)


## Why can I not see my old data for a metric that I have not sent metrics values for recently?
{: #qa3}

{{site.data.keyword.monitoringshort}} deletes all data for a metric path that appears to be transient in nature by identifying metrics that have not been written to in the past 7 days. 

For example:

* When a container is deleted, the metrics associated with that container exist for 7 days after which time the metrics are deleted.
* If you have a statsd gauge called `<space_id>.test.statsd.gauge-hello` and you do not write to it for a week, the metric will be identified as transient, and that metric will be deleted along with all of its historical information. 

