---

copyright:
  years: 2017, 2018

lastupdated: "2018-04-24"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Security
{: #security_ov}

To control the {{site.data.keyword.monitoringshort}} service actions that a user is allowed to perform, you can assign one or more roles to a user. To authenticate a user to work with metrics and alerts, you can use a UAA token, an IAM token, or an API Key. 
{:shortdesc}

   
## Authentication models
{: #auth}

To work with metrics that are stored in the {{site.data.keyword.monitoringshort}} service for a space, you require an authentication token or an API key. 

To get a security token, see:

* [Getting a UAA token](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_uaa)
* [Getting an IAM token](/docs/services/cloud-monitoring/security/auth_iam.html#auth_iam)

To get an API Key, see [Generating an API Key](/docs/services/cloud-monitoring/security/auth_api_key.html#auth_api_key). If the API key is compromised, you can revoke it by deleting it. Then, you can recreate a new one. For more information, see [Revoking an API key by using the {{site.data.keyword.Bluemix_notm}} UI](/docs/services/cloud-monitoring/security/auth_api_key.html#revoke_ui). 

A UAA token and an IAM token expire after a period of time. The API key does not expire. 

The following table lists the security models that are supported for each type of domain:

<table>
  <caption>Table 1. Security models supported for each domain</caption>
  <tr>
    <th></th>
	<th align="right">Account</th>
    <th align="right">Organization</th>
    <th align="right">Space</th>	
  </tr>
  <tr>
    <th align="left">UAA</th>
	<td align="center">No</td>
	<td align="center">Yes</td>
	<td align="center">Yes</td>
  </tr>
  <tr>
    <th align="left">IAM</th>
	<td align="center">Yes</td>
	<td align="center">Yes</td>
	<td align="center">Yes</td>
  </tr>
</table>

The {{site.data.keyword.monitoringshort}} service uses the IAM access control feature to govern what actions or operations a user or service can perform against the domain. For example, you can define an IAM policy to allow a user to send and create dashboards against a domain, but not to retrieve the metrics from the domain.



## Cloud Foundry roles
{: #bmx_roles}

The following table lists the privileges of each Cloud Foundry role to work with the {{site.data.keyword.monitoringshort}} service:

<table>
  <caption>Table 2. Cloud Foundry roles and privileges to work with the {{site.data.keyword.monitoringshort}} service.</caption>
  <tr>
    <th>Role</th>
	<th>Domain</th>
	<th>Access privileges</th>
  </tr>
  <tr>
    <td>Manager</td>
	<td>Organization <br>Space</td>
	<td>All RESTful APIs</td>
  </tr>
  <tr>
    <td>Developer</td>
	<td>Space</td>
	<td>All RESTful APIs</td>
  </tr>
  <tr>
    <td>Auditor</td>
	<td>Organization <br>Space</td>
	<td>Only RESTful APIs that perform read only operations, like query metrics.</td>
  </tr>
</table>


## IAM roles
{: #iam_roles}

The following table lists the {{site.data.keyword.monitoringshort}} service actions when you work with metrics and the IAM roles that grant permissions to a user to execute these tasks:

<table>
  <caption>Table 3. Working with metrics </caption>
  <tr>
	<th>Action</th>
	<th>IAM role</th>
  </tr>
  <tr>
    <td>Send metrics to the domain</td>
	<td>Administrator, Editor, Operator</td>
  </tr>
  <tr>
    <td>Retrieve/query metrics</td>
	<td>Administrator, Editor, Viewer</td>
  </tr>
  <tr>
    <td>Search for metrics in the domain</td>
	<td>Administrator, Editor</td>
  </tr>
</table>

The following table lists the {{site.data.keyword.monitoringshort}} service actions when you work with alerts and the IAM roles that grant permissions to a user to execute these tasks:

<table>
  <caption>Table 4. Working with alerts. </caption>
  <tr>
	<th>Action</th>
	<th>IAM role</th>
  </tr>
  <tr>
    <td>Create, edit and delete alert rules</td>
	<td>Administrator, Editor</td>
  </tr>
  <tr>
    <td>View alerts</td>
	<td>Administrator, Editor, Viewer</td>
  </tr>
  <tr>
    <td>Create, edit and delete alert notifications</td>
	<td>Administrator, Editor</td>
  </tr>
  <tr>
    <td>View notifications</td>
	<td>Administrator, Editor, Viewer</td>
  </tr>
  <tr>
    <td>View triggered alert history records</td>
	<td>Administrator, Editor, Viewer</td>
  </tr>
</table>



