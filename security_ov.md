---

copyright:
  years: 2017

lastupdated: "2017-07-12"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Security
{: #security_ov}

You can use different authentication methods to send metrics to the {{site.data.keyword.monitoringshort}} service. Permissions to access, modify, and delete metrics are controlled by using roles.
{:shortdesc}

   
## Authentication models
{: #auth}

To access the metrics that are stored in the {{site.data.keyword.monitoringshort}} service for a {{site.data.keyword.Bluemix_notm}} space, you require an authentication token or API key. 

The {{site.data.keyword.monitoringshort}} service supports the following authentication models:

* [{{site.data.keyword.Bluemix_notm}} UAA authentication](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_uaa)
* [{{site.data.keyword.Bluemix_notm}} IAM Authentication](/docs/services/cloud-monitoring/security/auth_iam.html#auth_iam)

A UAA token and an IAM token expire after a period of time. The API key does not expire. 

If the API key is compromised, you can revoke it by deleting it. Then, you can recreate a new one. For more information, see [Revoking an API key by using the Bluemix UI](/docs/services/cloud-monitoring/security/auth_iam.html#revoke_ui). 

The IAM authentication model offers UI, CLI, or API management capabilities. You can only use the CLI to manage UAA tokens.

The following table lists the authentication models that are supported for each type of domain:

<table>
  <caption>Table 1. Auth models supported for each domain</caption>
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



## Bluemix UAA roles
{: #bmx_roles}

The following table lists the privileges of each {{site.data.keyword.Bluemix_notm}} role to work with the {{site.data.keyword.monitoringshort}} service:

<table>
  <caption>Table 2. {{site.data.keyword.Bluemix_notm}} roles and privileges to work with the {{site.data.keyword.monitoringshort}} service.</caption>
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


## Bluemix IAM roles
{: #iam_roles}

The following table lists the relationship between the API, a service action, and an IAM role that is used by the {{site.data.keyword.monitoringshort}} Service.

<table>
  <caption>Table 3. Relationship between the API, a service action, and an IAM role. </caption>
  <tr>
    <th>API</th>
	<th>Action</th>
	<th>IAM role</th>
	<th>Description</th>
  </tr>
  <tr>
    <td>POST /v1/metrics</td>
    <td>domain.write</td>
	<td>Administrator, Editor, Operator</td>
	<td>Send metrics to the domain</td>
  </tr>
  <tr>
    <td>GET /v1/metrics</td>
    <td>domain.render</td>
	<td>Administrator, Editor, Viewer</td>
	<td>Retrieve/query metrics</td>
  </tr>
  <tr>
    <td>GET /v1/metrics/list</td>
    <td>domain.find</td>
	<td>Administrator, Editor</td>
	<td>Search for metrics in the domain</td>
  </tr>
</table>

## Getting the security token or API key
{: #get_token}

Use the {{site.data.keyword.Bluemix_notm}} UAA model to get an authentication token that you can use to access data that is stored in the {{site.data.keyword.monitoringshort}} service for a space in {{site.data.keyword.Bluemix_notm}}. You can obtain the authentication token by using the {{site.data.keyword.Bluemix_notm}} CLI or by using the `Login` REST API. For more information, see [Getting the UAA token by using the {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_cli) and [Getting the UAA token by using the API](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_api).

Use the {{site.data.keyword.Bluemix_notm}} IAM model to get an authentication token that you can use to access data that is stored in the {{site.data.keyword.monitoringshort}} service or to get an API key. The token has an expiration time. The API key does not expire. For more information, see [Getting the IAM token by using the {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli), [Generating an IAM API key by using the {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli), or [Generating an IAM API key by using the {{site.data.keyword.Bluemix_notm}} UI](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_ui).



