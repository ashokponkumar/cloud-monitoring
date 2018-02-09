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


# General format
{: #metrics_format}

Queries that you define in Grafana to monitor an {{site.data.keyword.Bluemix}} service must comply with the following format: 
{:shortdesc}

```
{Source}.{Cloud Type}.{Service Name}.{Region}.[service fields].{Metric type}.{Metric Subtype}.[Functions]
```
{: codeblock}

where [service fields] represent the specific fields of the metric query of a service or CF app. 

<table>
  <caption>Common fields in a query</caption>
  <tr>
    <th>Field</th>
	<th>Description</th>
	<th>Value</th>
  </tr>
  <tr>
    <td>Source</td>
	<td>This is a reserved name. Metrics under this hierarchy come from {{site.data.keyword.Bluemix_notm}} services.</td>
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
	  <td>For the {{site.data.keyword.containershort}}, the value is: *containers-kubernetes*</td>
  </tr>
  <tr>
    <td>Region</td>
	  <td>Defines the region where the container is running.</td>
	  <td>For the US South region, the value is: *us-south* <br>For the United Kingdom region, the value is: *united-kingdom*  <br>For Germany, the value is: *frankfurt* <br>For Sydney, the value is: *sydney* </td>
  </tr>
  <tr>
    <td>Metric Type</td>
	<td>Type of metric.</td>
	<td>*cpu* <br>*load* <br>*memory*</td>
  </tr>
  <tr>
    <td>Metric Subtype</td>
	<td>Subtype of the metric.</td>
	<td>For example: *usage*, *num-cores*, *usage-pct*, *usage-pct-container-requested*</td>
  </tr>
  <tr>
    <td>Functions</td>
    <td>Query functions that you can select to visualize a container metric in the panel. </td>
    <td>[Functions ![(External link icon)](../../../icons/launch-glyph.svg "External link icon")](http://graphite.readthedocs.io/en/latest/functions.html){: new_window}</td>
   </tr>
</table>




