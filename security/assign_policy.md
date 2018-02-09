---

copyright:
  years: 2017, 2018

lastupdated: "2018-02-01"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Granting a user permissions
{: #grant_permissions}

In the {{site.data.keyword.Bluemix}}, you can assign one or more roles to users. These roles define what tasks are enabled for that user to work with the {{site.data.keyword.monitoringshort}} service. 
{:shortdesc}

To grant a user permissions to work with metrics, you must add a policy for that user that describes the actions that this user can do with the {{site.data.keyword.monitoringshort}} service in the account. Only account owners can assign individual policies to users.


## Assigning a user an IAM policy through the IBM Cloud UI 
{: #assign_policy_ui}

Complete the following steps to grant a user permissions to work with the {{site.data.keyword.monitoringshort}} service:

1. Log in to the {{site.data.keyword.Bluemix_notm}} console.

    Open a web browser and launch the {{site.data.keyword.Bluemix_notm}} dashboard: [http://console.bluemix.net ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://bluemix.net){:new_window}
	
	After you log in with your user ID and password, the {{site.data.keyword.Bluemix_notm}} UI opens.

2. From the menu bar, click **Manage > Account > Users**. 

    The *Users* window displays a list of users with their email addresses for the currently selected account.
	
3. If the user is a member of the account, select the user name from the list, or click **Manage user** from the *Actions* menu.

    If the user is not a member of the account, see [Inviting users](/docs/iam/iamuserinv.html#iamuserinv).

4. Click **Assign service policies**.

5. Enter information about the policy. The following table lists the fields that are required or optional to define a policy: 

    <table>
	  <caption>List of fields to configure an IAM policy.</caption>
	  <tr>
	    <th>Field</th>
		<th>Value</th>
		<th>Status</th>
	  </tr>
	  <tr>
	    <td>Service</td>
		<td>{{site.data.keyword.monitoringlong}}</td>
		<td>Required</td>
	  </tr>
	  <tr>
	    <td>Roles</td>
		<td>Select one or more IAM roles. <br>Valid roles are: *administrator*, *operator*, *editor*, and *viewer*. <br>For more information about the actions that are allowed per role, see [IAM roles](/docs/services/cloud-monitoring/security_ov.html#iam_roles).</td>
		<td>Required</td>
	  </tr>
	  <tr>
	    <td>Regions</td>
		<td>You can specify the regions where the user is going to be granted access to work with metrics. Select **Specify optional service context**. Then, add one or more regions individually, or select **All current regions** to grant access to all regions.</td>
		<td>Optional</td>
	  </tr>
	</table>
	
6. Click **Assign policy**.
	
The policy that you configure is applicable to the selected regions. 

## Assigning a user an IAM policy by using the command line
{: #assign_policy_commandline}

Complete the following steps to grant a user access to view metrics by using the command line:

1. (Pre-req) Install the {{site.data.keyword.Bluemix_notm}} CLI.

   For more information, see [Installing the {{site.data.keyword.Bluemix_notm}} CLI](/docs/cli/reference/bluemix_cli/download_cli.html#download_install).
   
   If the CLI is installed, continue with the next step.
	
2. Log in to a region, organization, and space in the {{site.data.keyword.Bluemix_notm}}. 

    For more information, see [How do I log in to the {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#login).
	
3. Get the account ID. 

    Run the following command to get the account ID:

    ```
	bx iam accounts
	```
    {: codeblock}	

	A list of accounts with their GUIDs is displayed.
	
	Export the ID of the account where you plan to grant permissions to a user. Configure a shell variable such as `$acct_id`, for example:
	
	```
	export acct_id="1234567891234567812341234123412"
	```
	{: screen}

4. Get the {{site.data.keyword.IBM_notm}} ID of the user to whom you want to grant permissions.

    1. Display the users that are associated with the account. Run the following command:
	
	    ```
		bx iam account-users
		```
		{: codeblock}
		
	2. Get the GUID of the user. **Note: This step must be completed by the user to whom you plan to grant permissions.**
	
	    For example, request the user to run the following commands to get his user ID:
		
		Get the IAM token. For more information, see [Getting the IAM token by using the {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli).

        Get the data from the IAM token that is between the first 2 dots in the IAM token. Export the data to a shell variable such as `$user_data`. 
		
		```
	    export user_data="xxxxxxxxxxxxxxxxxxxxxxxxxxx"
	    ```
	    {: screen}
		
		Run the following command, for example, to get the user ID. This command uses jq in this sample to decode the information into JSON formatted text:
		
		```
		echo $user_data | base64 -d | jq
		```
		{: codeblock}
		
		The output of running this command is the following:
		
		```
		$ echo $user_data | base64 -d | jq
        {
        "iam_id": "IBMid-xxxxxxxxxx",
        "id": "IBMid-xxxxxxxxxx",
        "realmid": "IBMid",
        ......
		}
        ```
	    {: screen}
		
		Send the user ID, that is the value of the field *id* to the account owner. 
		
	3. Export the user ID to a shell variable such as `$user_ibm_id`.
	
		```
		export user_ibm_id="IBMid-xxxxxxxxxx"
		```
		{: codeblock}

3. Invite the user to the account if it is not already a member. For more information, see [Inviting users](/docs/iam/iamuserinv.html#iamuserinv).

    For example, run the following command to invite user xxx@yyy.com to the account zzz@ggg.com:
	
	```
	bx iam account-user-invite xxx@yyy.com zzz@ggg.com OrgAuditor dev SpaceDeveloper
	```
	{: codeblock}
		
4. Create a policy file name. 

    For example, use the following template to grant view permissions in the US South region:
	
	```
	{
		"roles" : [
			{
				"id": "crn:v1:bluemix:public:iam::::role:Editor" 
			}
		],
		"resources": [
			{
				"serviceName": "ibmcloud-monitoring",
				"region": "us-south"
			}
		]
	}
	```
	{: codeblock}
	
	Name the policy file: `policy.json`
	
5. Get the IAM token for your user ID.

    For more information, see [Getting the IAM token by using the {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli).

    Export the IAM token to a shell variable such as `$iam_token`, for example:
	
	```
	export iam_token="xxxxxxxxxxxxxxxxxxxxx"
	```
	{: screen}
	
6. Grant the user permissions to work with the {{site.data.keyword.monitoringshort}} service. 

   Run the following cURL command to grant permissions in the US South region:
	
    ```
	curl -X POST --header "Authorization: $iam_token" --header "Content-Type: application/json" https://iampap.ng.bluemix.net/acms/v1/scopes/a%2F$acct_id/users/$user_ibm_id/policies -d @policy.json
	```
	{: codeblock}
	
	Run the following cURL command to grant permissions in the United Kingdom region:
	
    ```
	curl -X POST --header "Authorization: $iam_token" --header "Content-Type: application/json" https://iampap.eu-gb.bluemix.net/acms/v1/scopes/a%2F$acct_id/users/$user_ibm_id/policies -d @policy.json
	```
	{: codeblock}

	





## Granting a user a CF role through the IBM Cloud UI 
{: #grant_permissions_ui_space}


To work with metrics in the space domain or in the organization domain, a user must have a Cloud Foundry (CF) role assigned in the {{site.data.keyword.Bluemix_notm}}. A CF role describes the actions that a user can do with the {{site.data.keyword.monitoringshort}} service in a space or in an organization. 

Complete the following steps to grant a user permissions to work with the {{site.data.keyword.monitoringshort}} service:

1. Log in to the {{site.data.keyword.Bluemix_notm}} console.

    Open a web browser and launch the {{site.data.keyword.Bluemix_notm}} dashboard: [http://bluemix.net ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://bluemix.net){:new_window}
	
	After you log in with your user ID and password, the {{site.data.keyword.Bluemix_notm}} UI opens.

2. From the menu bar, click **Manage > Account > Users**. 

    The *Users* window displays a list of users with their email addresses for the currently selected account.
	
3. If the user is a member of the account, select the user name from the list, or click **Manage user** from the *Actions* menu.

    If the user is not a member of the account, see [Inviting users](/docs/iam/iamuserinv.html#iamuserinv).

4. Click **Assign organization**.

5. Enter information about the policy. The following table lists the fields that are required or optional to define a policy: 

    <table>
	  <caption>List of fields to configure a CF policy.</caption>
	  <tr>
	    <th>Field</th>
		<th>Value</th>
	  </tr>
	  <tr>
	    <td>Organization</td>
		<td>Choose an organization from the list.</td>
	  </tr>
	  <tr>
	    <td>Organization Roles</td>
		<td>Select **No organization role**. However, if the user has an organizational role, choose an organization role from the list. <br>Valid values are: **Billing manager**, **Auditor**, **Manager**</td>
	  </tr>
	  <tr>
	    <td>Region</td>
		<td>Choose a region from the list. <br>Valid values are: **All current regions**, **US South**, **United Kingdom**</td>
	  </tr>
	  <tr>
	    <td>Space</td>
		<td>By default, **All current spaces** is predefined when the *Region* field is set to **All current regions**. To grant permission to an individual space, choose a specific region, and then choose a space from the list.</td>
	  </tr>
	  <tr>
	    <td>Space Roles</td>
		<td>Choose a space role from the list. <br>Valid values are: **Manager**, **Auditor**, **Developer**, and **No space role**. For more information, see [Cloud Foundry roles](/docs/services/cloud-monitoring/security_ov.html#bmx_roles).</td>
	  </tr>
	</table>
	
6. Click **Assign policy**.
	
The policy that you configure is applicable to the selected regions. 


