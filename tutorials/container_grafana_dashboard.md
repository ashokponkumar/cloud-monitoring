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


# Create a Grafana dashboard to monitor a Kubernetes cluster
{: #container_grafana_dashboard}


Use this tutorial to learn how to create a Grafana dashboard in the {{site.data.keyword.monitoringlong}} service to monitor the performance of your cluster. 
{:shortdesc}


## Objectives
{: #objectives}

Learn how to search and analyze container metrics for an app that is deployed in a Kubernetes cluster:

1. Launch Grafana and set the {{site.data.keyword.monitoringshort} domain where you can view the cluster metrics.
2. Create a Grafana dashboard and define a metric that monitors the CPU usage of a container.


## Assumptions
{: #assumptions}

The tutorial assumes:

* A cluster is available in the US South region. 
* The cluster forwards metrics to the account domain, that is, the cluster does not have a CF organization and space associated to it.

To complete this tutorial, you must complete the tutorial [Analyze metrics in Grafana for an app that is deployed in a Kubernetes cluster](/docs/services/cloud-monitoring/container_service_metrics.html#container_service_metrics) or have a cluster provisoned with at least 1 application deployed.



## Step1: Launch Grafana
{: #step1}

Launch Grafana from a browser and set the {{site.data.keyword.monitoringshort}} domain where you can view the cluster metrics.

To analyze metrics for a cluster, you must access Grafana in the cloud Public region where the cluster is created. For more information, see [Navigating to the Grafana dashboard from a web browser](/docs/services/cloud-monitoring/grafana/navigating_grafana.html#launch_grafana_from_browser).

1. From a browser, launch Grafana. 

    Enter the {{site.data.keyword.monitoringshort}} service URL for the region where you created the cluster. 
    
    To get the URLs per region, see [URLs for the Monitoring service](/docs/services/cloud-monitoring/monitoring_ov.html#region).

    For example, for the US South region, launch: [https://metrics.ng.bluemix.net/](https://metrics.ng.bluemix.net/).

2. Set the {{site.data.keyword.monitoringshort}} domain to **account**.

    In Grafana, select your ID. Then, check that you are in the correct account, and choose `Domain = account`.


## Step2: Create a Grafana dashboard
{: #step2}

Complete the following steps to create a new dashboard:

1. Select the side menubar toggle ![Grafana sidemenu bar](images/grafana_settings.gif "Grafana sidemenu bar").
2. Select **Dashboards**.
3. Click **New**

A dashboard opens. The dashboard includes an empty row that is ready for configuration.

![Grafana dashboard](images/grafana4_f1.gif "Grafana dashboard")

In Grafana, you add rows to divide the dashboard into sections. A row groups 1 or more panels. Within a row, a panel is the smallest visualization unit that you can configure to display data for a metric, for example, you can choose a graph panel or a table panel. You can drag and drop panels to rearrange panels in a dashboard. The data that a panel displays is configured through queries. You can define one or more queries in a panel. Each query represents a different set of data. You can also set the time range for a panel. Normally, the time range is set by the *Dashboard* time picker.

## Step3: Add a Graph to the dashboard to monitor a metric
{: #step3}

Complete the following steps:

1. Select **Graph**.

2. Click on the graph title, and then select **edit**.

    The *Metrics* tab opens. You can see here the default data source.


![Graph panel including configuration tabs](images/grafana4_f2.gif "Graph panel including configuration tabs")


## Step4: Define a metric query
{: #step4}

Define the query that filters the data that is displayed in the graph. This query monitors the nanoseconds of cpu time across all cores for a container.

For information about the format of the query, see [Query format for CPU metrics collected for containers](/docs/services/cloud-monitoring/reference/metrics_format.html#cpu_containers).
 
In the *Metrics* tab, select **Add query**. <br>A query entry is added. Each query is labeled with a letter. 

![New query entry](images/grafana4_query_f1.gif "New query entry") 
	
Complete the following steps to define the query:
        
1. Click **Select metric** to specify the source, and then choose `ibmcloud`.
    
2. Click **Select metric** to specify the cloud type, and then choose `public`.
    
3. Click **Select metric** to specify the service name, and then choose `containers-kubernetes`.
	
4. Click **Select metric** to specify the region, and then choose the region where your cluster is running. For example, `us-south`.
    
5. Click **Select metric** to specify the cluster name, and then choose the name of the cluster where your container is running.
		
6. Click **Select metric** to specify the metric source. Select **container**.
		
7. Click **Select metric** to specify the namespace. Then enter the name of the namespace in your cluster that is associated with your container.
		
8. Click **Select metric** to specify the pod name.
	
9. Click **Select metric** to specify the container name of the container that you want to monitor.
	
10. Click **Select metric** to specify the metric type, and then click **Select metric** to specify the metric subtype.
	
    For example, to monitor the nanoseconds of cpu time across all cores for a container, select **cpu** for the type, and **usage** for the subtype.
		
	For a list of CPU metrics, see [CPU metrics for containers](/docs/services/cloud-monitoring/containers/monitoring_containers_ov.html#cpu_metrics_containers).
    
11. Click the plus image ![Add icons](images/grafana_plus_image.gif "Plus image") and choose a function. You can add a function to transform, combine, and perform computations on the data that is available for a metric.

    For example, you can add the **alias(newName)** function to add an alias for a metric. This alias is used to print a string instead of the metric name in the legend that is displayed in the graph.

    To add an alias for your metric, complete the following steps:

    1. Click the plus symbol.
    2. Select **Special**.
    3 Select **alias**.
    4. Enter a string, for example, `My sample metric`.

## Step5: Save the dashboard
{: #step5}

Save the dashboard for later reuse.

1. Click the save dashboard image ![Save dashboard image](images/grafana_save_image.gif "Save dashboard image").

    ![Save dashboard](images/grafana_save_dashboard.gif "Save dashboard")

2. Enter the name of the dashboard.
3. Click **Save**.



## Next steps
{: #next_steps}

Define an alert for a metric. For more information, see [Configuring alerts](/docs/services/cloud-monitoring/config_alerts_ov.html#config_alerts_ov).