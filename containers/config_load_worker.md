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



# Configuring CPU metrics for a worker in Grafana
{: #config_load_worker}

In the {{site.data.keyword.Bluemix}}, selected load metrics for a worker are collected automatically. To monitor them through the {{site.data.keyword.monitoringlong}}, you must define a Grafana query. 
{:shortdesc}

For a list of the load metrics that are collected automatically, see [Load metrics for workers](/docs/services/cloud-monitoring/containers/monitoring_containers_ov.html#load_metrics_workers).


## Step 1: Gather the data for the worker that you want to monitor
{: #step1}

Obtain the following information for the worker that you want to monitor:

* **Region** where the cluster is running.
* **Cluster name** where the container is running. 
* **Worker name** that you want to monitor. 

Find out if metrics are forwarded to a space domain or to the account domain.

To identify where your cluster forwards metrics, run the following command:

```
$ bx cs cluster-get ClusterName --json
```
{: codeblock}

where *ClusterName* is the name of your cluster.

In the output, the following fields provide the information about where metrics are forwarded:

* **logOrg** defines the ID of a CF organization.
* **logOrgName** defines the name of a CF organization.
* **logSpace** defines the ID of a CF space.
* **logSpaceName** defines the name of a CF space.

If the fields are empty, then metrics are forwarded to the account domain.
If the fields have a CF organization and a CF space set, then metrics are forwarded to the space domain that is associated with this space.

## Step 2: Launch Grafana
{: #step2}

Launch Grafana from a browser. For more information, see [Navigating to the Grafana dashboard from a web browser](/docs/services/cloud-monitoring/grafana/navigating_grafana.html#launch_grafana_from_browser).

Ensure that in Grafana you are logged in to the account where the cluster is running. 

1. From a browser, launch Grafana. 

    Enter the {{site.data.keyword.monitoringshort}} service URL for the region where you created the cluster. 
    
    To get the URLs per region, see [URLs for the Monitoring service](/docs/services/cloud-monitoring/monitoring_ov.html#region).

    For example, for the US South region, launch: [https://metrics.ng.bluemix.net/](https://metrics.ng.bluemix.net/).

2. Set the {{site.data.keyword.monitoringshort} domain where you can view the cluster metrics.

    In Grafana, select your ID. Then, check that you are in the correct account, and choose a domain.

    Clusters that have a CF space associated forward metrics to the space metrics domain. Select `Domain = space`, and the organization and space that is associated with your cluster.

    CLusters that are created at account level forward metrics to the account metrics domain. Select `Domain = account`



## Step 3: Define a query in Grafana for a CPU metric
{: #step3}

Complete the following steps to create a Grafana dashboard and define a query to monitor the CPU usage of a worker:

1. Create a new dashboard.

    * Select the side menubar toggle ![Grafana sidemenu bar](images/grafana_settings.gif "Grafana sidemenu bar").
    * Select **Dashboards**.
    * Click **New**

    A dashboard opens. The dashboard includes an empty row that is ready for configuration.

2. Add a *Graph* panel to monitor the a CPU metric for a pod.

    1. Select **Graph**.

    2. Click on the graph title, and then select **edit**.

        The *Metrics* tab opens. You can see here the default data source.

3. Define the query that filters the data that is displayed in the graph. 

    For information about the format of the query, see [Query format for load metrics collected for workers](/docs/services/cloud-monitoring/reference/metrics_format.html#load_workers).

    In the *Metrics* tab, select **Add query**. <br>A query entry is added. Each query is labeled with a letter.
	
	Complete the following steps to define the query:

    1. Click **Select metric** to specify the source, and then choose `ibmcloud`.
    
    2. Click **Select metric** to specify the cloud type, and then choose `public`.
    
    3. Click **Select metric** to specify the service name, and then choose `containers-kubernetes`.
	
    4. Click **Select metric** to specify the region, and then choose the region where your cluster is running. For example, `us-south`.
    
    5. Click **Select metric** to specify the cluster name, and then choose the name of the cluster where your container is running.
		
	6. Click **Select metric** to specify the worker name of the worker that you want to monitor.
	
	7. Click **Select metric** to specify the metric type, and then click **Select metric** to specify the metric subtype.
	
	    For a list of CPU metrics, see [CPU metrics for workers](/docs/services/cloud-monitoring/containers/monitoring_containers_ov.html#load_metrics_workers).
	
	10. Click the plus image ![Add icons](images/grafana_plus_image.gif "Plus image") and choose a function. You can add a function to transform, combine, and perform computations on the data that is available for a metric.

        For example, you can add the **alias(newName)** function to add an alias for a metric. This alias is used to print a string instead of the metric name in the legend that is displayed in the graph.

        To add an alias for your metric, complete the following steps:

        1. Click the plus symbol.
        2. Select **Special**.
        3 Select **alias**.
        4. Enter a string, for example, `My sample metric`.

4. Save the dashboard for later reuse.

    1. Click the save dashboard image ![Save dashboard image](images/grafana_save_image.gif "Save dashboard image").
    2. Enter the name of the dashboard.
    3. Click **Save**.

