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


# Configuring a metric query in Grafana
{: #define_query}

In the {{site.data.keyword.Bluemix}}, metrics from selected Cloud services are collected automatically. To monitor them through the {{site.data.keyword.monitoringlong}}, you must define a Grafana query. 
{:shortdesc}

## Step 1: Gather the data for the CF app or service that you want to monitor
{: #step1}

Obtain the following information for the CF app or service that you want to monitor:

* **Region** where the CF app is running.
* **Organization** where the CF app is running. 	
* **Space** where the CF app is running. 
* **CF app name** of the app that you want to monitor or **Service Name**. 


## Step 2: Launch Grafana
{: #step2}

Launch Grafana from a browser. For more information, see [Navigating to the Grafana dashboard from a web browser](/docs/services/cloud-monitoring/grafana/navigating_grafana.html#launch_grafana_from_browser).

Ensure that in Grafana you are logged in to the account, organization, and region where the CF app or service is running. 


## Step 3: Define a query in Grafana
{: #step3}

Complete the following steps to create a Grafana dashboard and define a query:

1. Create a new dashboard.

    * Select the side menubar toggle ![Grafana side menu bar](images/grafana_settings.gif "Grafana side menu bar").
    * Select **Dashboards**.
    * Click **New**

    A dashboard opens. The dashboard includes an empty row that is ready for configuration.

2. Add a *Graph* panel.

    1. Select **Graph**.

    2. Click on the graph title, and then select **edit**.

        The *Metrics* tab opens. You can see here the default data source.

3. Define the query that filters the data that is displayed in the graph. 

    In the *Metrics* tab, select **Add query**. <br>A query entry is added. Each query is labeled with a letter.
    
    ![New query entry](images/grafana4_query_f1.gif "New query entry")
        
    1. Click **Select metric**, and then choose the source: `ibmcloud`.
    
    2. Click **Select metric**, and then choose the Cloud type: `public`.
    
    3. Click **Select metric**, and then choose the service name, for example, `cloud-foundry` for CF app metrics or `message hub` for {{site.data.keyword.messagehub}} metrics.
    
    4. Click **Select metric**, and then choose the region from where you are working, for example, `ng` for US South region.
    
    5. Select the specific service fields that apply to the service or to the CF app that you want to monitor.

        <table>
          <caption>Metric query formats per service or CF app</caption>
          <tr>
            <th>Service name</th>
            <th>Link to metric query format</th> 
          </tr>
          <tr>
            <td>{{site.data.keyword.containershort_notm}}</td>
            <td>[Query format for CPU metrics collected for containers](/docs/services/cloud-monitoring/reference/metrics_format_containers.html#cpu_containers) </br>[Query format for Load metrics collected for workers](/docs/services/cloud-monitoring/reference/metrics_format_containers.html#load_workers) </br>[Query format for memory metrics collected for containers](/docs/services/cloud-monitoring/reference/metrics_format_containers.html#mem_containers)</td> 
          </tr>
          <tr>
            <td>CF apps</td>
            <td>[Query format for CF apps](/docs/services/cloud-monitoring/reference/cfapps_metrics_format.html#cfapps_metrics_format)</td> 
          </tr>
          <tr>
            <td>{{site.data.keyword.messagehub}}</td>
            <td>[Query format for {{site.data.keyword.messagehub}}](/docs/services/cloud-monitoring/reference/mh_metrics_format.html#mh_metrics_format)</td> 
          </tr>
        </table>

        For example, for a CF app, choose:
    
        Click **Select metric**, and then choose the CF app name, for example, `logtester`.
    
        Click **Select metric**, and then choose the CF app instance index, for example, `0`.

        Click **Select metric**, and then choose `container`.
    
    9. Click **Select metric**, and then choose a type of metric. For example, **cpu** for CPU metrics, **memory** for memory metrics, and **disk** for disk metrics. 

        **Note:** Skip this step for CF apps. 

    10. Click **Select metric**, and then choose a metric. 

        <table>
          <caption>List of metrics per service or CF app</caption>
          <tr>
            <th>Service name</th>
            <th>Link to metrics</th> 
          </tr>
          <tr>
            <td>{{site.data.keyword.containershort_notm}}</td>
            <td>[CPU metrics](/docs/services/cloud-monitoring/cf/monitoring_cf_apps_ov.html#cpu_metrics </br>[Disk metrics](/docs/services/cloud-monitoring/cf/monitoring_cf_apps_ov.html#disk_metrics) </br>[Memory metrics](/docs/services/cloud-monitoring/cf/monitoring_cf_apps_ov.html#mem_metrics)</td> 
          </tr>
          <tr>
            <td>CF apps</td>
            <td>[CPU metrics](/docs/services/cloud-monitoring/cf/monitoring_cf_apps_ov.html#cpu_metrics)  </br>[Disk metrics](/docs/services/cloud-monitoring/cf/monitoring_cf_apps_ov.html#disk_metrics)   </br>[Memory metrics](/docs/services/cloud-monitoring/cf/monitoring_cf_apps_ov.html#mem_metrics)</td> 
          </tr>
          <tr>
            <td>{{site.data.keyword.messagehub}}</td>
            <td>[Metrics for a Kafka topic](/docs/services/cloud-monitoring/mh/monitoring_mh_ov.html#kafka_topic_metrics) </br>[Metrics for a Kafka partition](/docs/services/cloud-monitoring/mh/monitoring_mh_ov.html#kafka_partition_metrics)</td> 
          </tr>
        </table>

    10. Click the plus image ![Add icons](images/grafana_plus_image.gif "Plus image") and choose a function. You can add a function to transform, combine, and perform computations on the data that is available for a metric.
        
        For example, you can add the **alias(newName)** function to add an alias for a metric. This alias is used to print a string instead of the metric name in the legend that is displayed in the graph.
        
        To add an alias for your metric, complete the following steps:
        
        1. Click the plus symbol.
        2. Select **Special**. 
        3 Select **alias**.
        4. Enter a string, for example, `My sample metric`.


## Step 4: Save the dashboard for later reuse
{: #step4}

Complete the following steps:

1. Click the save dashboard image ![Save dashboard image](images/grafana_save_image.gif "Save dashboard image").
2. Enter the name of the dashboard.
3. Click **Save**.
