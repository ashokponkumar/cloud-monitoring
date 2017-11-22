---

copyright:
  years: 2017
lastupdated: "2017-11-21"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Getting started with IBM Cloud Monitoring
{: #getting-started-with-ibm-cloud-monitoring}

In this getting started tutorial, we'll take you through the steps to analyze a container by using the {{site.data.keyword.monitoringlong}} service. Learn how to search and analyze container metrics for an app that is deployed in a Kubernetes cluster. 
{:shortdesc}

## Before you begin
{: #prereqs}

Sign up for an [{{site.data.keyword.IBM_notm}} Cloud account](https://console.bluemix.net/registration/). Your user ID must be a member or an owner of an {{site.data.keyword.IBM_notm}} Cloud account with permissions to create Kubernetes clusters, deploy apps into clusters, and query metrics in Grafana.

* To create and manage a standard cluster, your user ID must have IAM **Administrator** role permissions for the {{site.data.keyword.containershort}}. For more info, see [Assigning a user an IAM policy through the IBM Cloud UI](/docs/services/cloud-monitoring/security/assign_policy.html#grant_permissions).
* To define queries and view metric data, your user ID must have a **developer** Cloud Foundry role in the space where the cluster is created or running. 

Install the following CLIs:

* {{site.data.keyword.Bluemix_notm}} CLI: You use this CLI to get and manage {{site.data.keyword.Bluemix_notm}} information, such as account details. For more information, see [Installing the {{site.data.keyword.Bluemix_notm}} CLI](/docs/cli/reference/bluemix_cli/download_cli.html#download_install).
* {{site.data.keyword.containershort}} CLI: You use this CLI to manage your Kubernetes cluster. For more information, see [Installing the {{site.data.keyword.containershort}} CLI](/docs/cli/reference/bluemix_cli/download_cli.html#download_install).


## Step 1: Deploying an app in a container
{: #step1}

Open a terminal session from where you can manage the Kubernetes cluster and deploy apps from the command line. 

Complete the following steps to deploy a container in a Kubernetes cluster:

1. [Create a Kubernetes standard cluster](/docs/containers/cs_tutorials.html#cs_cluster_tutorial).

    **Note:** Create the cluster in a region where the {{site.data.keyword.monitoringlong}} service is available. For more information about the regions where the {{site.data.keyword.monitoringshort}} service is available, see [Regions](/docs/services/cloud-monitoring/monitoring_ov.html#regions).

2. Set up the cluster context in a Linux terminal. After the context is set, you can manage the Kubernetes cluster and deploy the application in the Kubernetes cluster.

    For more information, see [How do I set up a cluster environment in my terminal session?](/docs/services/cloud-monitoring/qa/qa_containers.html#qa1).

3. Deploy and run a sample app in the Kubernetes cluster. [Complete the steps for lesson 1](/docs/containers/cs_tutorials_apps.html#cs_apps_tutorial_lesson1).

    The app is a Hello World Node.js app:

    ```
    var express = require('express')
    var app = express()

    app.get('/', function(req, res) {
      res.send('Hello world! Your app is up and running in a cluster!\n')
    })
    app.listen(8080, function() {
      console.log('Sample app is listening on port 8080.')
    })
    ```
	{: screen}

    When the app is deployed, metrics collection is automatically enabled.



## Step 2: Navigating to the Grafana dashboard
{: #step2}

Launch Grafana from a browser.

To analyze metrics for a cluster, you must access Grafana in the cloud Public region where the cluster is created. For more information, see [Navigating to the Grafana dashboard from a web browser](/docs/services/cloud-monitoring/grafana/navigating_grafana.html#launch_grafana_from_browser).

Then, from a browser, launch Grafana. Enter the {{site.data.keyword.monitoringshort}} service URL for the region where you created the cluster. To get the URLs per region, see [URLs for the Monitoring service](/docs/services/cloud-monitoring/monitoring_ov.html#region).

For example, for the US South region, launch: `https://metrics.ng.bluemix.net/`
    
    

## Step 3: Analyzing metrics in Grafana
{: #step3}

Complete the following steps to create a Grafana dashboard and define a metric that monitors the CPU usage of a container:

1. Create a new dashboard.

    * Select the side menubar toggle ![Grafana sidemenu bar](images/grafana_settings.gif "Grafana sidemenu bar").
    * Select **Dashboards**.
    * Click **New**

    A dashboard opens. The dashboard includes an empty row that is ready for configuration.

    ![Grafana dashboard](images/grafana4_f1.gif "Grafana dashboard")

     In Grafana, you add rows to divide the dashboard into sections. A row groups 1 or more panels. Within a row, a panel is the smallest visualization unit that you can configure to display data for a metric, for example, you can choose a graph panel or a table panel. You can drag and drop panels to rearrange panels in a dashboard. The data that a panel displays is configured through queries. You can define one or more queries in a panel. Each query represents a different set of data. You can also set the time range for a panel. Normally, the time range is set by the *Dashboard* time picker.

2. Add a *Graph* panel.

    1. Select **Graph**.

    2. Click on the graph title, and then select **edit**.

        The *Metrics* tab opens. You can see here the default data source.

        ![Graph panel including configuration tabs](images/grafana4_f2.gif "Graph panel including configuration tabs")

3. Define the query that filters the data that is displayed in the graph. This query monitors the nanoseconds of cpu time across all cores for a container.

    For information about the format of the query, see [Query format for CPU metrics collected for containers](/docs/services/cloud-monitoring/reference/metrics_format.html#cpu_containers).
 
    In the *Metrics* tab, select **Add query**. <br>A query entry is added. Each query is labeled with a letter. 

    ![New query entry](images/grafana4_query_f1.gif "New query entry") 
	
	<br>Complete the following steps to define the query:
        
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

4. Save the dashboard for later reuse.

    1. Click the save dashboard image ![Save dashboard image](images/grafana_save_image.gif "Save dashboard image").

        ![Save dashboard](images/grafana_save_dashboard.gif "Save dashboard")

    2. Enter the name of the dashboard.
    3. Click **Save**.


## Next steps
{: #next_steps}

Define an alert for a metric. For more information, see [Configuring alerts](/docs/services/cloud-monitoring/config_alerts_ov.html#config_alerts_ov).
