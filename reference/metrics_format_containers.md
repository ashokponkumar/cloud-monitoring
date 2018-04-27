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


# {{site.data.keyword.containershort_notm}}
{: #metrics_format_containers}

Queries that you define in Grafana to monitor the {{site.data.keyword.containershort_notm}} service follow the following pattern: 
{:shortdesc}



## Query format for CPU metrics collected for containers
{: #cpu_containers}

The format of a query that you define in Grafana to monitor a CPU metric for a container that runs in the {{site.data.keyword.containershort_notm}} service is described as follows: 

```
{Source}.{Cloud Type}.{Service Name}.{Region}.{Cluster Name}.{Namespace}.{Metric Source}.{Pod Name}.{Container Name}.{Metric Type}.{Metric Subtype}.[Functions]
```
{: codeblock}  

where

<table>
  <caption>Fields required to define a CPU metric for a container </caption>
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
	  <td>Defines the region where the container is running.</td>
	  <td>For the US South region, the value is: *us-south* <br>For the United Kingdom region, the value is: *united-kingdom*  <br>For Germany, the value is: *frankfurt* <br>For Sydney, the value is: *sydney* </td>
  </tr>
  <tr>
    <td>Cluster Name</td>
	  <td>Name of the cluster to monitor.</td>
	  <td></td>
  </tr>
  <tr>
    <td>Metric Source</td>
	  <td>Source of the data.</td>
	  <td>*container* </td>
  </tr>
  <tr>
    <td>Namespace</td>
	<td>Namespace where the container is deployed in a Kubernetes cluster.</td>
	<td></td>
  </tr>
   <tr>
    <td>Pod Name</td>
	<td>Name of the pod.</td>
	<td></td>
  </tr>
  <tr>
    <td>Container Name</td>
	<td>Name of the container.</td>
	<td></td>
  </tr>
  <tr>
    <td>Metric Type</td>
	  <td>Type of metric.</td>
	  <td>*CPU*</td>
  </tr>
  <tr>
    <td>Metric Subtype</td>
	  <td>Metric to be displayed.</td>
	  <td>*usage*, *num-cores*, *usage-pct*, *usage-pct-container-requested*</td>
  </tr>
  <tr>
    <td>Functions</td>
    <td>Query functions that you can select to visualize a container metric in the panel. </td>
    <td>[Functions ![(External link icon)](../../../icons/launch-glyph.svg "External link icon")](http://graphite.readthedocs.io/en/latest/functions.html){: new_window}</td>
   </tr>
</table>

For example, a query to monitor the CPU usage for a container that is running in a Kubernetes cluster in the US South region is defined as follows:

```
ibmcloud.public.containers-kubernetes.us-south.myCluster.container.default.myPod.myContainer-1775968925-kfwfk.cpu.usage
```
{: screen}



## Query format for Load metrics collected for workers
{: #load_workers}

The format of a query that you define in Grafana to monitor a load metric for a worker that runs in the {{site.data.keyword.containershort_notm}} service is described as follows: 

```
{Source}.{Cloud Type}.{Service Name}.{Region}.{Cluster Name}.{Metric Source}.{Worker Name}.{Metric Type}.{Metric Subtype}.[Functions]
```
{: codeblock}  

where

<table>
  <caption>Fields required to define a load metric for a worker </caption>
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
	  <td>Defines the region where the container is running.</td>
	  <td>For the US South region, the value is: *us-south* <br>For the United Kingdom region, the value is: *united-kingdom*  <br>For Germany, the value is: *frankfurt* <br>For Sydney, the value is: *sydney* </td>
  </tr>
  <tr>
    <td>Cluster Name</td>
	<td>Name of the cluster to monitor.</td>
	<td></td>
  </tr>
  <tr>
    <td>Metric Source</td>
	<td>Source of the data.</td>
	<td>*worker* </td>
  </tr>
   <tr>
    <td>Worker Name</td>
	<td>Name of the worker.</td>
	<td></td>
  </tr>
  <tr>
    <td>Metric Type</td>
	<td>Type of metric.</td>
	<td>*load*</td>
  </tr>
  <tr>
    <td>Metric Subtype</td>
	  <td>Metric to be displayed.</td>
	  <td>*avg-1*, *avg-5*, *avg-15*</td>
  </tr>
  <tr>
    <td>Functions</td>
    <td>Query functions that you can select to visualize a container metric in the panel. </td>
    <td>[Functions ![(External link icon)](../../../icons/launch-glyph.svg "External link icon")](http://graphite.readthedocs.io/en/latest/functions.html){: new_window}</td>
   </tr>
</table>

For example, a query to monitor the CPU usage for a worker that is running in a Kubernetes cluster in the US South region is defined as follows:

```
ibmcloud.public.containers-kubernetes.us-south.myCluster.worker.MyWorker.load.avg-1
```
{: screen}


## Query format for memory metrics collected for containers
{: #mem_containers}

The format of a query that you define in Grafana to monitor a memory metric for a container that runs in the {{site.data.keyword.containershort_notm}} service is described as follows: 

```
{Source}.{Cloud Type}.{Service Name}.{Region}.{Cluster Name}.{Namespace}.{Metric Source}.{Pod Name}.{Container Name}.{Metric Type}.{Metric Subtype}.[Functions]
```
{: codeblock}  

where

<table>
  <caption>Fields required to define a memory metric for a container</caption>
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
	  <td>Defines the region where the container is running.</td>
	  <td>For the US South region, the value is: *us-south* <br>For the United Kingdom region, the value is: *united-kingdom*  <br>For Germany, the value is: *frankfurt* <br>For Sydney, the value is: *sydney* </td>
  </tr>
  <tr>
    <td>Cluster Name</td>
	<td>Name of the cluster to monitor.</td>
	<td></td>
  </tr>
  <tr>
    <td>Metric Source</td>
	<td>Source of the data.</td>
	<td>*container* </td>
  </tr>
  <tr>
    <td>Namespace</td>
	<td>Namespace where the container is deployed in a Kubernetes cluster.</td>
	<td></td>
  </tr>
   <tr>
    <td>Pod Name</td>
	<td>Name of the pod.</td>
	<td></td>
  </tr>
  <tr>
    <td>Container Name</td>
	  <td>Name of the container.</td>
	  <td></td>
  </tr>
  <tr>
    <td>Metric Type</td>
  	<td>Type of metric.</td>
  	<td>*memory*</td>
  </tr>
  <tr>
    <td>Metric Subtype</td>
	  <td>Metric to be displayed.</td>
	  <td>*limit*, *current*, *usage-pct*</td>
  </tr>
  <tr>
    <td>Functions</td>
    <td>Query functions that you can select to visualize a container metric in the panel. </td>
    <td>[Functions ![(External link icon)](../../../icons/launch-glyph.svg "External link icon")](http://graphite.readthedocs.io/en/latest/functions.html){: new_window}</td>
   </tr>
</table>

For example, a query to monitor the memory usage for a container that is running in a Kubernetes cluster in the US South region is defined as follows:

```
ibmcloud.public.containers-kubernetes.us-south.myCluster.container.default.myPod.myContainer-1775968925-kfwfk.memory.usage
```
{: screen}