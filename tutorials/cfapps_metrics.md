---

copyright:
  years: 2017, 2018

lastupdated: "2018-02-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Analyze metrics in Grafana for a CF app
{: #cfapps_metrics}

Use this tutorial to learn how to use the {{site.data.keyword.monitoringlong}} service to monitor the performance of a Cloud Foundry (CF) app that runs in {{site.data.keyword.Bluemix_notm}} Public. 
{:shortdesc}


## Objectives
{: #objectives}

Learn how to search and analyze metrics for a CF app:

1. Deploy a CF app.
2. Launch Grafana and set the {{site.data.keyword.monitoringshort}} domain where you can view the CF app metrics.
3. Search and analyze metrics for a CF app that runs in a space of the {{site.data.keyword.Bluemix_notm}}.

This tutorial assumes that you are working in the US South region.


## Prerequisites
{: #prereqs}

1. Be a member or an owner of an {{site.data.keyword.Bluemix_notm}} account with permissions to provision services in a space, deploy CF apps, and to query metrics in {{site.data.keyword.Bluemix_notm}} through the {{site.data.keyword.monitoringshort}} service.

    Your user ID for the {{site.data.keyword.Bluemix_notm}} must have a CF role for the space where the {{site.data.keyword.monitoringshort}} service and the CF app are provisioned. The role required is *developer*.
    
    For more information, see [Granting a user a CF role by using the IBM Cloud UI](/docs/services/cloud-monitoring/security/grant_permissions.html#grant_permissions_ui_space).

2. Provision the {{site.data.keyword.monitoringshort}} service in a space where you have permisisons to provision services in the US South region.

    For more information, see [Provisioning the {{site.data.keyword.monitoringshort}} service](/docs/services/cloud-monitoring/how-to/provision.html#provision).

## Step 1: Grant your user permissions to work with CF apps and the {{site.data.keyword.monitoringshort}} service
{: #step1}

To grant a user permissions to deploy CF apps in a space or to view metrics in a space domain, you must assign that user a CF role that describes the actions that this user can do with the {{site.data.keyword.monitoringshort}} service in the space and in the {{site.data.keyword.Bluemix_notm}}. 

**Note:** This tutorial assumes that you are the account owner or have permissions to add roles to your user ID. If you do not have permisisons, request the owner to complete this step.

Complete the following steps to grant a user permissions to complete the tutorial:

1. Log in to the {{site.data.keyword.Bluemix_notm}} console.

    Open a web browser and launch the {{site.data.keyword.Bluemix_notm}} dashboard: [http://bluemix.net ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://bluemix.net){:new_window}
	
	After you log in with your user ID and password, the {{site.data.keyword.Bluemix_notm}} UI opens.

2. From the menu bar, click **Manage > Account > Users**. 

    The *Users* window displays a list of users with their email addresses for the currently selected account.
	
3. Find your user name from the list, and click **Manage user** from the *Actions* menu.

4. Select **Cloud Foundry access**, then select **Assign organization**.

5. Enter the following values: 

    <table>
      <caption></caption>
      <tr>
        <th>Field</th>
        <th>Value</th>
      </tr>
      <tr>
        <td>Organization</td>
        <td>MyOrg</td>
      </tr>
      <tr>
        <td>Organization Role</td>
        <td>No organization role</td>
      </tr>
      <tr>
        <td>Region</td>
        <td>US South</td>
      </tr>
      <tr>
        <td>Space</td>
        <td>dev</td>
      </tr>
      <tr>
        <td>Space role</td>
        <td>developer</td>
      </tr>
    </table>
	
6. Click **Save role**.
 

## Step 2: Deploy a CF app
{: #step2}

Complete the following steps from the {{site.data.keyword.Bluemix_notm}} console:

1. Click **Catalog** in the {{site.data.keyword.Bluemix_notm}} toolbar.

2. Click **Cloud Foundry Apps > Liberty for Java**. 

3. Enter the following information:

    * **App name**: Name of the application. Must be unique.
    * **Region**: Choose US South.
    * **Organization**: Choose the organization where you provisioned the {{site.data.keyword.monitoringshort}} service.
    * **Space**: Choose the space where you provisioned the {{site.data.keyword.monitoringshort}} service.

3. Click **Create**.

As soon as the CF app is running, metrics are collected and forwarded to the {{site.data.keyword.monitoringshort}} service.

## Step 3: Launch Grafana and set the metrics domain
{: #step3}

Launch Grafana from a browser and set the {{site.data.keyword.monitoringshort}} domain where you can view the CF app metrics.

1. From a browser, launch Grafana. 

    Enter the {{site.data.keyword.monitoringshort}} service URL for the region where the {{site.data.keyword.monitoringshort}} service is provisioned.
    
    To get the URLs per region, see [URLs for the Monitoring service](/docs/services/cloud-monitoring/monitoring_ov.html#region).

    For example, for the US South region, launch: [https://metrics.ng.bluemix.net/](https://metrics.ng.bluemix.net/).

2. Set the {{site.data.keyword.monitoringshort}} domain where you can view the cluster metrics.

    In Grafana, select your ID. Then, check that you are in the correct account, and choose `Domain = space`.

    Verify that the organization name and the space name correspond to the ones where you deployed the CF app and provisioned the {{site.data.keyword.monitoringshort}} service.


## Step 4: Create a Grafana dashboard to monitor a metric
{: #step4}


Complete the following steps to create a new dashboard in Grafana:

1. Select the side menubar toggle ![Grafana sidemenu bar](images/grafana_settings.gif "Grafana sidemenu bar").
2. Select **Dashboards**.
3. Click **New**

A dashboard opens. The dashboard includes an empty row that is ready for configuration.

![Grafana dashboard](images/grafana4_f1.gif "Grafana dashboard")

In Grafana, you add rows to divide the dashboard into sections. A row groups 1 or more panels. Within a row, a panel is the smallest visualization unit that you can configure to display data for a metric, for example, you can choose a graph panel or a table panel. You can drag and drop panels to rearrange panels in a dashboard. The data that a panel displays is configured through queries. You can define one or more queries in a panel. Each query represents a different set of data. You can also set the time range for a panel. Normally, the time range is set by the *Dashboard* time picker.

Define the query that filters the data that is displayed in the graph. This query monitors the percentage of CPU utilization toward the limit of the container.

For information about the format of the query, see [Grafana query format for CF apps](/docs/services/cloud-monitoring/reference/cfapps_metrics_format.html#cfapps_metrics_format).
    
1. Add a *Graph* panel to monitor the nanoseconds of cpu time across all cores for a container.
    
    1. Select **Graph**.
    
    2. Click on the graph title, and then select **edit**.
    
        The *Metrics* tab opens. You can see here the default data source.
    
        ![Graph panel including configuration tabs](images/grafana4_f2.gif "Graph panel including configuration tabs")
    
2. Define the query that filters the data that is displayed in the graph. 
    
    In the *Metrics* tab, select **Add query**. <br>A query entry is added. Each query is labeled with a letter.
    
    ![New query entry](images/grafana4_query_f1.gif "New query entry")
        
    1. Click **Select metric**, and then choose the source: `ibmcloud`.
    
    2. Click **Select metric**, and then choose the Cloud type: `public`.
    
    3. Click **Select metric**, and then choose `cloud-foundry`.
    
    4. Click **Select metric**, and then choose the region from where you are working, for example, `us-south` for US South region.
    
    5. Click **Select metric**, and then choose the CF app name, for example, `logtester`.
    
    6. Click **Select metric**, and then choose the CF app instance index, for example, `0`.

    7. Click **Select metric**, and then choose `container`.
    
    9. Click **Select metric**, and then choose a metric. To monitor the *Percentegae of CPU usage* of a container, choose `cpu-utilization`.

    10. Click the plus image ![Add icons](images/grafana_plus_image.gif "Plus image") and choose a function. You can add a function to transform, combine, and perform computations on the data that is available for a metric.
        
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
