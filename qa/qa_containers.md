---

copyright:
  years: 2017

lastupdated: "2017-11-15"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# FAQ for monitoring Kubernetes clusters
{: #qa_containers}

Here are the answers to common questions about the {{site.data.keyword.monitoringshort}} service and the {{site.data.keyword.containershort_notm}} service. 
{:shortdesc}

* [The Grafana query for my containers is not showing data or is in error](/docs/services/cloud-monitoring/qa/qa_containers.html#metric_format_change)
* [How do I set up a cluster environment in my terminal session?](/docs/services/cloud-monitoring/qa/qa_containers.html#qa1)
* [How do I find the space ID and space name of a space that is associated with a cluster?](/docs/services/cloud-monitoring/qa/qa_containers.html#qa2)
* [Where can I get the cluster name and the cluster ID?](/docs/services/cloud-monitoring/qa/qa_containers.html#qa3)
* [How to get the list of namespaces?](/docs/services/cloud-monitoring/qa/qa_containers.html#qa7)
* [How to get the list of pods in a namespace in a Kubernetes cluster?](/docs/services/cloud-monitoring/qa/qa_containers.html#qa8)
* [How to get all the pods in a cluster by namespace?](/docs/services/cloud-monitoring/qa/qa_containers.html#qa9)

## The Grafana query for my containers is not showing data or is in error
{: #metric_format_change}

The format of the query that you can define for a container has changed.

The following format is deprecated: 

```
{Prefix}.{Version}.{Provider}.{Type}.{ServiceName}.{Region}.{Account}.{Cluster}.{Metric}.{Container in a pod}.{Functions}
```
{: screen}

The new formats that are valid are the following:

* [CPU metric query format for a container](/docs/services/cloud-monitoring/reference/metrics_format.html#cpu_containers)
* [Load metric query format for a worker](/docs/services/cloud-monitoring/reference/metrics_format.html#load_workers)
* [Memory metric query format for a container](/docs/services/cloud-monitoring/reference/metrics_format.html#mem_containers)

Migrate your old queries to the new format to visualize your data in Grafana.

For example, if your query starts as `crn.v1.bluemix.public.containers-kubernetes.us-south.`, you must migrate your query to the new format, that is, to `ibmcloud.public.containers-kubernetes.us-south`.

The following table lists the fields in the format that has been deprecated:

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


## How do I set up a cluster environment in my terminal session?
{: #qa1}

You must set the context of a Kubernetes cluster to manage it by using commands.

**Note:** To manage a cluster, you need an IAM policy for the {{site.data.keyword.containershort_notm}} service assigned to your user with permissions to complete the task.

Complete the following steps to setup the context of a cluster:

1. Log in to the region, organization, and space in the {{site.data.keyword.Bluemix_notm}} that is associated with the cluster that you created. For more information, see [How do I log in to the {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#login).

2. Initialize the {{site.data.keyword.containershort_notm}} service plug-in. Run the following command:

	```
	bx cs init
	```
	{: codeblock}

3. Set the context of the cluster in your terminal session. Run the following commands:
    
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

 
 
## How do I find the space ID and space name of a space that is associated with a cluster?
{: #qa2}

When a cluster is created in an {{site.data.keyword.Bluemix_notm}} account, the metrics are associated with a space within that account. When you create queries to view cluster metrics, you need the space ID.
{:shortdesc}

**Note:** To manage a cluster, you need an IAM policy for the {{site.data.keyword.containershort_notm}} service assigned to your user with permissions to complete the task.

To find the space ID for a cluster, complete the following steps:

1. Log in to the region, organization, and space in the {{site.data.keyword.Bluemix_notm}} that is associated with the cluster that you created. For more information, see [How do I log in to the {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#login).

2. Initialize the {{site.data.keyword.containershort_notm}} service plug-in. Run the following command:

	```
	bx cs init
	```
	{: codeblock}

3. Get the cluster details. Run the following command:

    ```
    bx cs cluster-get cluster-name
    ```
    {: codeblock}

    where **cluster-name** is the name of the cluster.

    The space ID is the value indicated for the **Log Space** field.

    For example, the output of running the command is the following:

    ```
    Name:			    cluster-name
    ID:			        c213f81296db4c68b84e212c19135a99
    State:			    normal
    Created:		    2017-08-22T18:18:59+0000
    Datacenter:		    dal10
    Master URL:		    https://169.46.7.242:21210
    Ingress subdomain:  cluster-name.us-south.containers.mybluemix.net
    Ingress secret:     cluster-name
    Workers:		    5
    Log Space:		    fa277ff8-8a73-324b-9b75-0f11d54a3ae2
    ```
    {: screen}

4. Get the space name.

    List all space names:
	
    ```
	bx account spaces
	```
    {: codeblock}
	
	Run the following command for each space until you find the name of the matching ID:
	
	```
	bx iam space Spacename --guid
	```
	{: codeblock}
	
	


## Where can I get the cluster name and the cluster ID?
{: #qa3}

Complete the following steps to get the cluster name and ID through the UI:

1. Log in to your {{site.data.keyword.Bluemix_notm}} account.

    The {{site.data.keyword.Bluemix_notm}} dashboard can be found at: [http://bluemix.net ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://bluemix.net){:new_window}.
    
	After you log in with your user ID and password, the {{site.data.keyword.Bluemix_notm}} UI opens.

2. From the {{site.data.keyword.Bluemix_notm}} *Dashboard*, select **Menu > Containers**.

    The list of clusters that are available in the account are displayed.

3. To get the cluster ID, select a cluster entry. 

    The overview page opens. In this page, you can get the cluster ID.



Complete the following steps to get the cluster name and ID thorugh the command line:

1. Log in to the region, organization, and space in the {{site.data.keyword.Bluemix_notm}} that is associated with the cluster that you created. For more information, see [How do I log in to the {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#login).

2. List the clusters that are available in the account. Run the following command: 

    ```
	bx cs clusters
	``` 
	{: codeblock}
	
	The output of this command lists all the clusters in the account with their corresponding IDs.
	
3. To get the the custer ID, run the following command:

    ```
	bx cs cluster-get my_cluster
	```
    {: screen}	
 


## How to get the list of namespaces?
{: #qa7}

To get a list of all the namespaces in your cluster, complete the following steps:

Complete the following steps:

1. Set up the cluster context. For more information, see [How do I set up a cluster environment in my terminal session?](/docs/services/cloud-monitoring/qa/qa_containers.html#qa1).

2. List all the namespaces. Run the following kubectl command:

    ```
    kubectl get namespaces
	```
	{: codeblock}

## How to get the list of pods per namespace in a Kubernetes cluster?
{: #qa8}
		
To get the list of pods in a namespace, complete the following steps:

1. Set up the cluster context. For more information, see [How do I set up a cluster environment in my terminal session?](/docs/services/cloud-monitoring/qa/qa_containers.html#qa1).

2. To get the list of pods per namespace in a Kubernetes cluster, run the following command:

    ```
	kubectl --namespace=NamespaceName get pods 
	```
	{: codeblock}
	
	where Namespacename is the name of the namespace, for example, *default*.

## How to get all the pods in a cluster by namespace?
{: #qa9}
		
Complete the following steps:

1. Set up the cluster context. For more information, see [How do I set up a cluster environment in my terminal session?](/docs/services/cloud-monitoring/qa/qa_containers.html#qa1).
	
2. To get all the pods in a cluster by namespace, run the following command:

	```
	kubectl get pods --all-namespaces
	```
	{: codeblock}
		


