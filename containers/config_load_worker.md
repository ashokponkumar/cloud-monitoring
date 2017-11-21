---

copyright:
  years: 2017

lastupdated: "2017-11-21"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


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



## Step 2: Launch Grafana
{: #step2}

Launch Grafana from a browser. For more information, see [Navigating to the Grafana dashboard from a web browser](/docs/services/cloud-monitoring/grafana/navigating_grafana.html#launch_grafana_from_browser).

Ensure that in Grafana you are logged in to the account where the cluster is running. 

Check that the organization and space correspond to the space that is bound to the Kubernetes cluster. For information on how to get the space name and ID, see [How do I find the space ID and space name of a space that is associated with a cluster?](/docs/services/cloud-monitoring/qa/qa_containers.html#qa2).



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

