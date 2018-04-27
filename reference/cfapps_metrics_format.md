---

copyright:
  years: 2017, 2018

lastupdated: "2018-03-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# CF apps
{: #cfapps_metrics_format}

Queries that you define in Grafana to monitor a Cloud Foundry application that runs in {{site.data.keyword.Bluemix}} Public must comply with the following format: 
{:shortdesc}

```
{Source}.{Cloud Type}.{Service Name}.{Region}.{CFapp Name}.{CFapp Index}.{CFapp container}.{Metric Type}.{Metric Subtype}.[Functions]
```
{: codeblock}

where

<table>
  <caption>Common fields in a query</caption>
  <tr>
    <th>Field</th>
	<th>Description</th>
	<th>Value</th>
  </tr>
  <tr>
    <td>Source</td>
	<td>This is a reserved name. Metrics under this hierarchy come from {{site.data.keyword.Bluemix_notm}} applications and services.</td>
	<td>*ibmcloud*</td>
  </tr>
  <tr>
    <td>Cloud Type</td>
	<td>Indicates the type of Cloud. </td>
	<td>For the {{site.data.keyword.Bluemix_notm}} public cloud, the value is: *public*</td>
  </tr>
  <tr>
    <td>Service Name</td>
	<td>Defines the name of the service in the {{site.data.keyword.Bluemix_notm}}.</td>
	<td>For CF apps, the value is: *cloud-foundry*</td>
  </tr>
  <tr>
    <td>Region</td>
	<td>Defines the region where the CF app instance is running.</td>
	<td>For the US South region, the value is: *us-south* <br>For the United Kingdom region, the value is: *eu-gb*  <br>For Germany, the value is: *eu-de* <br>For Sydney, the value is: *au-syd* </td>
  </tr>
  <tr>
    <td>CFapp Name</td>
	<td>Name of the CF application.</td>
	<td></td>
  </tr>
  <tr>
    <td>CFapp Index</td>
	  <td>Index of the CF application instance.</td>
	  <td>For example: *0* </br>If you have a CF app with one instance, there is only index 0. If you scale the CF app, for example to 10 instances, you have 0 to 9 as index values.</td>
  </tr>
  <tr>
    <td>CFapp container</td>
	  <td>Fixed value.</td>
	  <td>*container*</td>
  </tr>
  <tr>
    <td>Metric Type</td>
	  <td>Type of metric. Disk, memory, and CPU are metric series collected automatically.</td>
	  <td>*CPU*</td>
  </tr>
  <tr>
    <td>Metric Subtype</td>
	  <td>Metric to be dispayed. </br>[CPU metrics](/docs/services/cloud-monitoring/cf/monitoring_cf_apps_ov.html#cpu_metrics) </br>[Disk metrics](/docs/services/cloud-monitoring/cf/monitoring_cf_apps_ov.html#disk_metrics) </br>[Memory metrics](/docs/services/cloud-monitoring/cf/monitoring_cf_apps_ov.html#mem_metrics)</td>
	  <td></td>
  </tr>
  <tr>
    <td>Functions</td>
    <td>Query functions that you can select to visualize a container metric in the panel. </td>
    <td>[Functions ![(External link icon)](../../../icons/launch-glyph.svg "External link icon")](http://graphite.readthedocs.io/en/latest/functions.html){: new_window}</td>
   </tr>
</table>




