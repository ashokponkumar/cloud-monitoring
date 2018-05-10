---

copyright:
  years: 2017, 2018

lastupdated: "2018-04-20"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Analyze metrics in Grafana for an app that is deployed in a Kubernetes cluster
{: #container_service_metrics}

Use this tutorial to learn how to use the {{site.data.keyword.monitoringlong}} service to monitor the performance of your container. 
{:shortdesc}


## Objectives
{: #objectives}

Learn how to search and analyze container metrics for an app that is deployed in a Kubernetes cluster:

1. Identify where metrics that are collected in a cluster are forwarded to the {{site.data.keyword.monitoringshort}} service. 
2. Launch Grafana and set the {{site.data.keyword.monitoringshort}} domain where you can view the cluster metrics.
3. Search and analyze container metrics for an app that is deployed in a Kubernetes cluster in the {{site.data.keyword.Bluemix_notm}}.

This tutorial walks through the steps that are required to get the following end to end scenario working in the {{site.data.keyword.Bluemix_notm}}: Provisioning a cluster, identifying where the cluster sends metrics to the {{site.data.keyword.monitoringshort}} service in the {{site.data.keyword.Bluemix_notm}}, deploying an app in the cluster, and using Grafana to view and filter container metrics for that cluster.


**Note:** To complete this tutorial, you must complete the prerequisites and the tutorials that are linked from the different steps.


## Prerequisites
{: #prereqs}

1. Be a member or an owner of an {{site.data.keyword.Bluemix_notm}} account with permissions to create Kubernetes standard clusters, deploy apps into clusters, and query the metrics in {{site.data.keyword.Bluemix_notm}} for monitoring in Grafana.

    Your user ID for the {{site.data.keyword.Bluemix_notm}} must have the following policies assigned:
    
    * An IAM policy for the {{site.data.keyword.containershort}} with *operator* or *administrator* permissions.
    
    For more information, see [Assign an IAM policy to a user through the IBM Cloud UI](/docs/services/cloud-monitoring/security/assign_policy.html#assign_policy_ui).

2. Have a terminal session from where you can manage the Kubernetes cluster and deploy apps from the command line. The examples in this tutorial are given for an Ubuntu Linux system.

3. Install the CLIs to work with the {{site.data.keyword.containershort}} in your Ubuntu system.

    * Install the {{site.data.keyword.Bluemix_notm}} CLI. For more information, see [Installing the {{site.data.keyword.Bluemix_notm}} CLI](/docs/cli/reference/bluemix_cli/download_cli.html#download_install).
    
    * Install the {{site.data.keyword.containershort}} CLI to create and manage your Kubernetes clusters in {{site.data.keyword.containershort}}, and to deploy containerized apps to your cluster. For more information, see [Install the CS plugin](/docs/containers/cs_cli_install.html#cs_cli_install_steps).
    

    
 

## Step 1: Provision a Kubernetes cluster
{: #step1}

Complete the following steps:

1. Create a standard Kubernetes cluster. For more information, see [Create a Kubernetes standard cluster](/docs/containers/cs_tutorials.html#cs_cluster_tutorial).

2. Set up the cluster context in a terminal. After the context is set, you can manage the Kubernetes cluster and deploy the application in the Kubernetes cluster.

    Log in to the region, organization, and space in the {{site.data.keyword.Bluemix_notm}} that is associated with the cluster that you created. For more information, see [How do I log in to the {{site.data.keyword.Bluemix_notm}}](/docs/services/CloudLogAnalysis/qa/cli_qa.html#login).

	Initialize the {{site.data.keyword.containershort}} service plug-in.

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



## Step 2: Grant your user permissions to see metrics in the account domain
{: #step2}

To grant a user permissions to view metrics in an account domain, you must assign the user an IAM policy for the {{site.data.keyword.monitoringshort}} service with **Viewer** role.

Complete the following steps to grant a user permissions to work with the {{site.data.keyword.monitoringshort}} service:

1. Log in to the {{site.data.keyword.Bluemix_notm}} console.

    Open a web browser and launch the {{site.data.keyword.Bluemix_notm}} dashboard: [http://bluemix.net ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://bluemix.net){:new_window}
	
	After you log in with your user ID and password, the {{site.data.keyword.Bluemix_notm}} UI opens.

2. From the menu bar, click **Manage > Account > Users**. 

    The *Users* window displays a list of users with their email addresses for the currently selected account.
	
3. If the user is a member of the account, select the user name from the list, or click **Manage user** from the *Actions* menu.

    If the user is not a member of the account, see [Inviting users](/docs/iam/iamuserinv.html#iamuserinv).

4. Select **Access policies > Assign Access > Assign access to resources**.

5. Choose the service **{{site.data.keyword.monitoringlong}}**, select the region where the cluster is available, **US-South** for this tutorial, and select a role, **viewer**.



## Step 3: Grant the {{site.data.keyword.containershort_notm}} key owner permissions
{: #step3}

For the cluster to forward metrics to the account domain, the {{site.data.keyword.containershort}} key owner must have the following IAM policies:

* IAM policy with **editor** permisisons for the {{site.data.keyword.monitoringshort}} service.
* IAM policy with **administrator** permisisons for the {{site.data.keyword.containershort}}.


Complete the following steps to grant the {{site.data.keyword.containershort}} key owner permissions:

1. Log in to the {{site.data.keyword.Bluemix_notm}} console.

    Open a web browser and launch the {{site.data.keyword.Bluemix_notm}} dashboard: [http://bluemix.net ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://bluemix.net){:new_window}
	
	After you log in with your user ID and password, the {{site.data.keyword.Bluemix_notm}} UI opens.

2. From the menu bar, click **Manage > Account > Users**. 

    The *Users* window displays a list of users with their email addresses for the currently selected account.
	
3. Find the {{site.data.keyword.containershort}} key owner user ID.

    Run the command `bx cs api-key-info ClusterName` to get the {{site.data.keyword.containershort}} key owner user ID.

4. Select **Access policies > Assign Access > Assign access to resources**.

5. Choose the service **{{site.data.keyword.monitoringlong}}**, select the region where the cluster is available, **US-South** for this tutorial, and select a role, **editor**.	

6. Repeat the steps 2 to 4, and choose the service {{site.data.keyword.containershort}}. Select **All regions**, and role **administrator**.	




## Step 4: Deploy a sample app in the Kubernetes cluster
{: #step4}

Deploy and run a sample app in the Kubernetes cluster. Complete the steps in the following tutorial to deploy the sample app: [Lesson 1: Deploying single instance apps to Kubernetes clusters](/docs/containers/cs_tutorials_apps.html#cs_apps_tutorial_lesson1).

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

In this sample app, when you test your app in a browser, the app writes to stdout the following message: `Sample app is listening on port 8080.`


## Step 5: Launch Grafana and set the metrics domain
{: #step5}

Launch Grafana from a browser and set the {{site.data.keyword.monitoringshort}} domain where you can view the cluster metrics.

To analyze metrics for a cluster, you must access Grafana in the cloud Public region where the cluster is created. For more information, see [Navigating to the Grafana dashboard from a web browser](/docs/services/cloud-monitoring/grafana/navigating_grafana.html#launch_grafana_from_browser).

1. From a browser, launch Grafana. 

    Enter the {{site.data.keyword.monitoringshort}} service URL for the region where you created the cluster. 
    
    To get the URLs per region, see [URLs for the Monitoring service](/docs/services/cloud-monitoring/monitoring_ov.html#region).

    For example, for the US South region, launch: [https://metrics.ng.bluemix.net/](https://metrics.ng.bluemix.net/).

2. Set the {{site.data.keyword.monitoringshort}} domain to **Account**.

    In Grafana, select your ID. Then, check that you are in the correct account, and choose a domain. Select `Domain = account`.

    Clusters forward metrics to the account metrics domain. 

## Step 6: Monitor the cluster in Grafana
{: #step6}

The {{site.data.keyword.containershort}} provides a Grafana dashboard that you can use to monitor your cluster metrics. 

Complete the following steps to open the sample dashboard:

1. Select the side menubar toggle ![Grafana sidemenu bar](images/grafana_settings.gif "Grafana sidemenu bar").
2. Select **Dashboards**.
3. Click **Open**.
4. Select **ClusterMonitoringDashboard_v1**.

The sample dashboard opens. 

![Grafana sample dashboard](images/cluster_grafana_sample_dashboard.png "Grafana sample dashboard")



## Next steps
{: #next_steps}

Define an alert for a metric. For more information, see [Configuring alerts](/docs/services/cloud-monitoring/config_alerts_ov.html#config_alerts_ov).
