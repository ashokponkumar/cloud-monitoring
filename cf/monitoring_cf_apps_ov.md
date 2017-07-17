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

# Monitoring apps running on Cloud Foundry
 {:#monitoring_bluemix_apps}

To send metrics into the {{site.data.keyword.monitoringshort}} service, a Cloud Foundry (CF) application can sent the metrics directly from the app by using the {{site.data.keyword.monitoringshort}} REST API.
{:shortdesc}

When you are using the Cloud Foundry infrastructure to run your apps on {{site.data.keyword.Bluemix_notm}}, you might want to monitor the health status of the app, the resource usage, and traffic metrics. With this performance information, you can then make decisions or take actions accordingly.


An app running on cloud foundry cannot run collectd. Instead, data must be sent from the app itself, using a lumberjack client or RESTful api. 

##Additional Ways to Send Data From a CF App [@lindj created a collectd and statsd buildpack] (https://github.com/jimlindeman/python-buildpack-collectd)

This is a cloneof the python-buildpack release that has collectd installed to listen on localhost:8125 with statsd input plugin and Bluemix's multi-tenant graphite output plugin 

You can use a buildpack to deploy a CF app in Bluemix. The buildpack provides the framework and the runtime support for the app. You can install and configure collectd to listen on localhost:8125 with statsd input plugin and Bluemix's multi-tenant graphite output plugin 