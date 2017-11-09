---

copyright:
  years: 2017
lastupdated: "2017-11-09"

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

Install the following CLIs:

* {{site.data.keyword.Bluemix_notm}} CLI: You use this CLI to get and manage {{site.data.keyword.Bluemix_notm}} information, such as account details. For more information, see [Installing the {{site.data.keyword.Bluemix_notm}} CLI](/docs/cli/reference/bluemix_cli/download_cli.html#download_install).
* {{site.data.keyword.containershort}} CLI: You use this CLI to manage your Kubernetes cluster. For more information, see [Installing the {{site.data.keyword.containershort}} CLI](/docs/cli/reference/bluemix_cli/download_cli.html#download_install).

Open a terminal session from where you can manage the Kubernetes cluster and deploy apps from the command line. 

## Step 1: Deploying an app in a container
{: #step1}

Complete the following steps to deploy a container in a Kubernetes cluster:

1. [Create a Kubernetes cluster](/docs/containers/cs_tutorials.html#cs_cluster_tutorial).

    **Note:** Create the cluster in a region where the {{site.data.keyword.monitoringlong}} service is available. For more information about the regions where the {{site.data.keyword.monitoringshort}} service is available, see [Regions](/docs/services/cloud-monitoring/monitoring_ov.html#regions).

2. Set up the cluster context in a Linux terminal. After the context is set, you can manage the Kubernetes cluster and deploy the application in the Kubernetes cluster.

    Log in to the region, organization, and space in the {{site.data.keyword.Bluemix_notm}} that is associated with the cluster that you created. For more information, see [How do I log in to the {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#login).

	Initialize the {{site.data.keyword.loganalysisshort}} service plug-in.

	```
	bx cs init
	```
	{: codeblock}

    Set your terminal context to your cluster.
    
	```
	bx cs cluster-config MyCluster
	```
	{: codeblock}

    The output of running this command provides the command that you must run in your terminal to set the path to your configuration file. For example:

	```
	export KUBECONFIG=/Users/ibm/.bluemix/plugins/container-service/clusters/MyCluster/kube-config-hou02-MyCluster.yml
	```
	{: codeblock}

    Copy and paste the command to set the environment variable in your terminal, and then press **Enter**.

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

To analyze metrics for a cluster, you must access Grafana in the cloud Public region where the cluster is created.

To analyze metrics for a cluster, you must access Grafana in the cloud Public region where the cluster is created. 
    
Then, from a browser, launch Grafana. Enter the {{site.data.keyword.monitoringshort}} service URL for the region where you created the cluster. To get the URLs per region, see [Regions](/docs/services/cloud-monitoring/monitoring_ov.html#regions).

For example, for the US South region, launch: `https://metrics.ng.bluemix.net/`
    
    

## Step 3: Analyzing metrics in Grafana
{: #step3}

Complete the following steps to create a Grafana dashboard:

1. Create a new dashboard.

    * Select the side menubar toggle ![Grafana sidemenu bar](images/grafana_settings.gif "Grafana sidemenu bar").
    * Select **Dashboards**.
    * Click **New**

    A dashboard opens. The dashboard includes an empty row that is ready for configuration.

    ![Grafana dashboard](images/grafana4_f1.gif "Grafana dashboard")

     In Grafana, you add rows to divide the dashboard into sections. A row groups 1 or more panels. Within a row, a panel is the smallest visualization unit that you can configure to display data for a metric, for example, you can choose a graph panel or a table panel. You can drag and drop panels to rearrange panels in a dashboard. The data that a panel displays is configured through queries. You can define one or more queries in a panel. Each query represents a different set of data. You can also set the time range for a panel. Normally, the time range is set by the *Dashboard* time picker.

2. Add a *Graph* panel to monitor the nanoseconds of cpu time across all cores for a container.

    1. Select **Graph**.

    2. Click on the graph title, and then select **edit**.

        The *Metrics* tab opens. You can see here the default data source.

        ![Graph panel including configuration tabs](images/grafana4_f2.gif "Graph panel including configuration tabs")

3. Define the query that filters the data that is displayed in the graph.

    The following table outlines the different fields that are required to configure a query that filters data for a container metric:

    <table>
      <caption>Table 1. Grafana query fields for containers</caption>
      <tr>
        <th align="center">Field</th>
        <th align="center">Description</th>
        <th align="center">Valid values</th>
      </tr>
      <tr>
        <td>Prefix</td>
        <td>Prefix that is used to indicates that it is an {{site.data.keyword.IBM_notm}} Cloud resource.</td>
        <td>`crn`</td>
      </tr>
      <tr>
        <td>Version</td>
        <td>Version that indicates the format and fields of the metric data collected. </td>
        <td>`v1`</td>
      </tr>
      <tr>
        <td>Provider</td>
        <td>Cloud provider where the data is collected. This field is an alphanumeric identifier.</td>
        <td>For the public {{site.data.keyword.IBM_notm}} Cloud, the value is: `bluemix`</td>
      </tr>
      <tr>
        <td>Type</td>
        <td>Cloud environment where the data is collected.</td>
        <td>For the public {{site.data.keyword.IBM_notm}} Cloud, the value is: `public`</td>
      </tr>
      <tr>
        <td>Service name</td>
        <td>Cloud infrastructure where metrics are collected.</td>
        <td>For the {{site.data.keyword.monitoringshort}} service, the value is: `containers-kubernetes`</td>
      </tr>
      <tr>
        <td>Region</td>
        <td>Cloud region where metrics are collected.</td>
        <td>For the US South region, the value is: `ng` <br>For the United Kingdom region, the value is: `eu-gb` <br>For Germany, the value is: `eu-de` <br>For Sydney, the value is: `au-syd` </td>
      </tr>
      <tr>
        <td>Account</td>
        <td>GUID of the account where metrics are collected. <br>The format of this field is the following: `a_ID` where ID is the GUID of the account. <br>To get the GUID of the account, see [How to get the GUID of an account](/docs/services/cloud-monitoring/qa/cli_qa.html#account_guid).</td>
        <td>a_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx</td>
      </tr>
      <tr>
        <td>Cluster</td>
        <td>GUID of the cluster where metrics are collected.</td>
        <td>xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx</td>
      </tr>
      <tr>
        <td>Metric</td>
        <td>Metric that is collected automatically for a container.</td>
        <td>For a list of CPU metrics, see [CPU metrics for containers](/docs/services/cloud-monitoring/containers/monitoring_containers_ov.html#cpu_metrics_containers) <br>For a list of memory metrics, see [Memory metrics](/docs/services/cloud-monitoring/containers/monitoring_containers_ov.html#memory_metrics) </td>
      </tr>
      <tr>
        <td>Container in a pod</td>
        <td>Combination of Kubernetes resource names and GUIDs that are required to identify uniquely a container that runs in a pod. <br>**Note:** When you look at the list of available options for this entry in the query, notice that there is also an entry with the following format: {namespace}{pod_name}{deploymentname_name}_POD{container_ID}. This entries correspond to internal container IDs that are created by Kubernetes.</td>
		<td>{namespace}_{pod_name}_{deployment_name}_{container_id}</td>
      </tr>
      <tr>
        <td>Functions</td>
        <td>Query functions that you can select to visualize a container metric in the panel. <br>For more information, see [Functions ![(External link icon)](../../icons/launch-glyph.svg "External link icon")](http://graphite.readthedocs.io/en/latest/functions.html){: new_window}</td>
        <td></td>
      </tr>
    </table>

    The following image shows how the query is displayed in Grafana when you configure it:

    ![Sample query](images/grafana4_query_f3.gif "Sample query")

    Complete the following steps to define the query:

    In the *Metrics* tab, select **Add query**. <br>A query entry is added. Each query is labeled with a letter.

    ![New query entry](images/grafana4_query_f1.gif "New query entry")
        
    1. Click **Select metric** to specify the prefix, and then choose `crn`.
    
    2. Click **Select metric** to specify the version, and then choose `v1`.
    
    3. Click **Select metric** to specify the provider, and then choose `bluemix`.
    
    4. Click **Select metric** to specify the type, and then choose `public`.
    
    5. Click **Select metric** to specify the service name, and then choose `containers-kubernetes`.
    
    6. Click **Select metric** to specify the region, and then choose the region from where you are working, for example, `us-south`.
    
    7. Click **Select metric** to specify the scope, and then choose the account ID for which you want to display data, for example, `a_91d1d1exxxxxxx4df920bbd06461b066`
	
	    To get the GUID of the account, see [How to get the GUID of an account](/docs/services/cloud-monitoring/qa/cli_qa.html#account_guid).
    
    8. Click **Select metric** to specify the cluster ID, and then choose the Cluster ID.
	
	    To get the ID of a cluster, run the following commands from a terminal:

        1. [Setup the cluster context](/docs/containers/cs_cli_install.html#cs_cli_configure) in a Linux terminal.

        2. Run the following command: `bx cs clusters` The output of this command lists all the clusters in the account with their corresponding IDs.
    
    9. Click **Select metric** to specify the metric, and then choose a container metric. For example, to monitor the *CPU usage* of a container, choose `container-metric-cpu_usage`.
	
	    For a list of CPU metrics, see [CPU metrics collected by default](/docs/services/cloud-monitoring/containers/monitoring_containers_ov.html#cpu_metrics) 
		
		For a list of memory metrics, see [Memory metrics collected by default](/docs/services/cloud-monitoring/containers/monitoring_containers_ov.html#memory_metrics)
    
    10. Click **Select metric** to specify the container details, and then choose any entry that applies to the container that you want to monitor. 
	
	    An entry has the following format: `{namespace}_{pod_name}_{deployment_name}_{container_id}` 
		
		Look for the namespace where your container is running, and the deployment name of that container. 
		
		Next, select **Toggel Edit Mode** from the list of actions that you can choose when editing a metric, and mofidy the value by replacing the container ID with an asterisk (*). Container IDs change frequently. By using the asterisk, you monitor the active containers that are running.
		
		For example, `default_hello-world-deployment-3355293961-0fwkg_hello-world-deployment_*`.
		
		To get a list of all the namespaces in your cluster, run the following command in a terminal where the cluster context is set: `kubectl get namespaces`
		
		To get the list of pods in a namespace, run the following command in a terminal where the cluster context is set: `kubectl --namespace=NamespaceName get pods` where Namespacename is the name of the namespace, for example, *default*.
		
		To get the list of deployments, run the following command in a terminal where the cluster context is set: `kubectl get deployments`
    
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
