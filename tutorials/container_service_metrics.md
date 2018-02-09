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


# Analyze metrics in Grafana for an app that is deployed in a Kubernetes cluster
{: #container_service_metrics}

Use this tutorial to learn how to use the {{site.data.keyword.monitoringlong}} service to monitor the performance of your container. 
{:shortdesc}


## Objectives
{: #objectives}

Learn how to search and analyze container metrics for an app that is deployed in a Kubernetes cluster:

1. Identify where metrics that are collected in a cluster are forwarded to the {{site.data.keyword.monitoringshort}} service. 
2. Launch Grafana and set the {{site.data.keyword.monitoringshort} domain where you can view the cluster metrics.
3. Search and analyze container metrics for an app that is deployed in a Kubernetes cluster in the {{site.data.keyword.Bluemix_notm}}.

This tutorial walks through the steps that are required to get the following end to end scenario working in the {{site.data.keyword.Bluemix_notm}}: Provisioning a cluster, identifying where the cluster sends metrics to the {{site.data.keyword.monitoringshort}} service in the {{site.data.keyword.Bluemix_notm}}, deploying an app in the cluster, and using Grafana to view and filter container metrics for that cluster.


**Note:** To complete this tutorial, you must complete the prerequisites and the tutorials that are linked from the different steps.


## Prerequisites
{: #prereqs}

1. Be a member or an owner of an {{site.data.keyword.Bluemix_notm}} account with permissions to create Kubernetes standard clusters, deploy apps into clusters, and query the metrics in {{site.data.keyword.Bluemix_notm}} for monitoring in Grafana.

    Your user ID for the {{site.data.keyword.Bluemix_notm}} must have the following policies assigned:
    
    * An IAM policy for the {{site.data.keyword.containershort}} with *operator* or *administrator* permissions.
    * A CF role for the space where the {{site.data.keyword.monitoringshort}} service is provisioned with *developer* permissions.
    
    For more information, see [Assign an IAM policy to a user through the IBM Cloud UI](/docs/services/cloud-monitoring/security/grant_permissions.html#grant_permissions_ui_account) and [Granting a user a CF role by using the IBM Cloud UI](/docs/services/cloud-monitoring/security/grant_permissions.html#grant_permissions_ui_space).

2. Have a terminal session from where you can manage the Kubernetes cluster and deploy apps from the command line. The examples in this tutorial are given for an Ubuntu Linux system.

3. Install the CLIs to work with the {{site.data.keyword.containershort}} in your Ubuntu system.

    * Install the {{site.data.keyword.Bluemix_notm}} CLI. For more information, see [Installing the {{site.data.keyword.Bluemix_notm}} CLI](/docs/cli/reference/bluemix_cli/download_cli.html#download_install).
    
    * Install the {{site.data.keyword.containershort}} CLI to create and manage your Kubernetes clusters in {{site.data.keyword.containershort}}, and to deploy containerized apps to your cluster. For more information, see [Install the CS plugin](/docs/containers/cs_cli_install.html#cs_cli_install_steps).
    

    
 

## Step 1: Provision a Kubernetes cluster
{: #step1}

Complete the following steps:

1. Create a standard Kubernetes cluster.

   * [Create a Kubernetes standard cluster through the UI](/docs/containers/cs_cluster.html#cs_cluster_ui).
   * [Create a Kubernetes standard cluster by using the CLI](/docs/containers/cs_cluster.html#cs_cluster_cli).

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



## Step 2: Identify where the cluster forwards metrics into the {{site.data.keyword.monitoringshort}} service
{: #step2}

A cluster is an account level resource. When you provision a cluster on the {{site.data.keyword.containershort}}, clusters can be created at the account level or they can be created with a Cloud Foundry (CF) space associated to it. As soon as the cluster is provisioned and ready, metrics are collected automatically and forwarded to the {{site.data.keyword.monitoringshort}} service.

* Clusters that have a CF space associated forward metrics to the space metrics domain.
* CLusters that are created at account level forward metrics to the account metrics domain.

To identify where your cluster forward metrics, run the following command:

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

For example, the output for a cluster that forwards metrics to the account domain looks as follows:

```
$ bx cs cluster-get MyDemoCluster --json
{
    "id": "f9adabcjhefg745746hgfjbnkdnfsks",
    "name": "MyDemoCluster",
    "region": "eu-gb",
    "dataCenter": "lon02",
    "location": "eu-gb-lon02",
    "serverURL": "https://xxx.xxx.xxx.x:xxxxx",
    "state": "normal",
    "createdDate": "2018-01-30T17:41:14+0000",
    "modifiedDate": "2018-01-30T17:41:14+0000",
    "workerCount": 2,
    "isPaid": true,
    "masterKubeVersion": "1.8.6_1505",
    "targetVersion": "1.8.6_1505",
    "ingressHostname": "mydemocluster.uk-south.containers.mybluemix.net",
    "ingressSecretName": "mydemocluster",
    "ownerEmail": "xxxx@uibm.com",
    "logOrg": "",
    "logOrgName": "",
    "logSpace": "",
    "logSpaceName": "",
    "monitoringURL": "https://metrics.eu-gb.bluemix.net/app/#/grafana4/dashboard/db/a-siuhfieuhf7346586hfrhf_ClusterMonitoringDashboard_v1?scopeId=a-siuhfieuhf7346586hfrhf\u0026?var-Account_ID=a_siuhfieuhf7346586hfrhf\u0026var-Cluster=MyDemoCluster\u0026var-Namespace=default\u0026var-Pod_ID=All",
    "addons": [
        {
            "name": "customer-storage-pod",
            "enabled": true
        },
        {
            "name": "basic-ingress-v2",
            "enabled": true
        },
        {
            "name": "storage-watcher-pod",
            "enabled": true
        }
    ],
    "vlans": null
}
```
{: screen}





## Step 3: Grant your user permissions to see metrics in the metrics domain
{: #step3}

To grant a user permissions to view metrics in a space domain, you must assign that user a CF role that describes the actions that this user can do with the {{site.data.keyword.monitoringshort}} service in the space. 

To grant a user permissions to view metrics in an account domain, you must assign that user an IAM policy that describes the actions that this user can do with the {{site.data.keyword.monitoringshort}} service. 

### Grant your user permissions to see metrics in a space domain
{: #space}

Complete the following steps to grant a user permissions to work with the {{site.data.keyword.monitoringshort}} service:

1. Log in to the {{site.data.keyword.Bluemix_notm}} console.

    Open a web browser and launch the {{site.data.keyword.Bluemix_notm}} dashboard: [http://bluemix.net ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://bluemix.net){:new_window}
	
	After you log in with your user ID and password, the {{site.data.keyword.Bluemix_notm}} UI opens.

2. From the menu bar, click **Manage > Account > Users**. 

    The *Users* window displays a list of users with their email addresses for the currently selected account.
	
3. If the user is a member of the account, select the user name from the list, or click **Manage user** from the *Actions* menu.

    If the user is not a member of the account, see [Inviting users](/docs/iam/iamuserinv.html#iamuserinv).

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
        <td>Auditor</td>
      </tr>
	
6. Click **Save role**.

### Grant your user permissions to see metrics in the account domain
{: #acc}

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



## Step 4: Grant the {{site.data.keyword.containershort_notm}} key owner permissions
{: #step4}

When the cluster forwards metrics to a space domain, you must also grant Cloud Foundry (CF) permissions to the {{site.data.keyword.containershort}} key owner in the organization and space. The key owner needs *orgManager* role for the organization, and *SpaceManager* and *Developer* for the space. 

When the cluster forwards metrics to the account domain, the {{site.data.keyword.containershort}} key owner must have an IAM policy with administrator permissions for the {{site.data.keyword.monitoringshort}} service.

### Grant permissions to see metrics in a space domain
{: #space_1}

Grant the {{site.data.keyword.containershort}} key owner user ID the following permissions: *orgManager* role for the organization, and *SpaceManager* and *Developer* for the space. Complete the following steps:
    
1. Log in to the {{site.data.keyword.Bluemix_notm}} console.

    Open a web browser and launch the {{site.data.keyword.Bluemix_notm}} dashboard: [http://bluemix.net ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://bluemix.net){:new_window}
	
	After you log in with your user ID and password, the {{site.data.keyword.Bluemix_notm}} UI opens.

2. From the menu bar, click **Manage > Account > Users**. 

    The *Users* window displays a list of users with their email addresses for the currently selected account.
	
3. Find the {{site.data.keyword.containershort}} key owner user ID.

    Run the command `bx cs api-key-info ClusterName` to get the {{site.data.keyword.containershort}} key owner user ID.

4. Select **Cloud Foundry access**, then select **Assign an organization**.

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
        <td>Manager</td>
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
        <td>Developer</td>
      </tr>
	
6. Click **Save role**.


### Grant permissions to see metrics in the account domain
{: #acc_1}

Complete the following steps:

1. Log in to the {{site.data.keyword.Bluemix_notm}} console.

    Open a web browser and launch the {{site.data.keyword.Bluemix_notm}} dashboard: [http://bluemix.net ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://bluemix.net){:new_window}
	
	After you log in with your user ID and password, the {{site.data.keyword.Bluemix_notm}} UI opens.

2. From the menu bar, click **Manage > Account > Users**. 

    The *Users* window displays a list of users with their email addresses for the currently selected account.
	
3. Find the {{site.data.keyword.containershort}} key owner user ID.

    Run the command `bx cs api-key-info ClusterName` to get the {{site.data.keyword.containershort}} key owner user ID.

4. Select **Access policies > Assign Access > Assign access to resources**.

5. Choose the service **{{site.data.keyword.monitoringlong}}**, select the region where the cluster is available, **US-South** for this tutorial, and select a role, **administrator**.	

## Step 5: Deploy a sample app in the Kubernetes cluster
{: #step5}

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


## Step 6: Launch Grafana and set the metrics domain
{: #step6}

Launch Grafana from a browser and set the {{site.data.keyword.monitoringshort}} domain where you can view the cluster metrics.

To analyze metrics for a cluster, you must access Grafana in the cloud Public region where the cluster is created. For more information, see [Navigating to the Grafana dashboard from a web browser](/docs/services/cloud-monitoring/grafana/navigating_grafana.html#launch_grafana_from_browser).

1. From a browser, launch Grafana. 

    Enter the {{site.data.keyword.monitoringshort}} service URL for the region where you created the cluster. 
    
    To get the URLs per region, see [URLs for the Monitoring service](/docs/services/cloud-monitoring/monitoring_ov.html#region).

    For example, for the US South region, launch: [https://metrics.ng.bluemix.net/](https://metrics.ng.bluemix.net/).

2. Set the {{site.data.keyword.monitoringshort} domain where you can view the cluster metrics.

    In Grafana, select your ID. Then, check that you are in the correct account, and choose a domain.

    Clusters that have a CF space associated forward metrics to the space metrics domain. Select `Domain = space`, and the organization and space that is associated with your cluster.

    CLusters that are created at account level forward metrics to the account metrics domain. Select `Domain = account`

## Step 7: Monitor the cluster in Grafana
{: #step7}

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
