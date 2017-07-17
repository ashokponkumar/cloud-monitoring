---

copyright:
  years: 2017

lastupdated: "2017-06-19"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Frequent questions and answers using the Bluemix CLI
{: #cli_qa}

Here are the answers to common questions about using {{site.data.keyword.Bluemix}} CLI with the {{site.data.keyword.monitoringshort}} service. 
{:shortdesc}

* [How do I install the {{site.data.keyword.Bluemix_notm}} CLI](#install_bmx_cli)
* [How do I get the GUID of an account](#account_guid)
* [How do I get the GUID of an organization](#org_guid)
* [How do I get the GUID of a space](#space_guid)


## How do I install the Bluemix CLI?
{: #install_bmx_cli}

Complete the following steps to install the {{site.data.keyword.Bluemix_notm}} CLI:

1. Download the CLI.

    For example, to install the {{site.data.keyword.Bluemix_notm}} CLI package in a Ubuntu system, download the [{{site.data.keyword.Bluemix_notm}} CLI package ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://clis.ng.bluemix.net/ui/home.html){: new_window}. 

2. Run the following command to extract the {{site.data.keyword.Bluemix_notm}} CLI package:
    
    ```
    tar -xvf Bluemix_CLI_0.5.4_amd64.tar.gz
    ```
    {: codeblock}
    
3. Go to the *Bluemix_CLI* directory, and run the `./install_bluemix_cli` command with root permission to install the CF plugin. You can run the command as a root user or use the sudo command to get the root permission. For example:
    
    ```
    cd Bluemix_CLI
    ```
    {: codeblock}
    
    ```
    sudo ./install_bluemix_cli
    ```
    {: codeblock}
    
4. Verify the installition of the CF plugin. For example, check the version of the CF plugin. Run the following command:
    
    ```
    cf -v
    ```
    {: codeblock}
    
    The output looks as follows:
    
    ```
    cf version 6.25.0+787326d.2017-02-28
    ```
    {: screen}
    
5. Verify the installition of the {{site.data.keyword.Bluemix_notm}} CLI. For example, check the version of the plugin. Run the following command:
    
    ```
    bx -version
    ```
    {: codeblock}
    
    The output looks as follows:
    
    ```
    bx version 0.5.4+ae22935-2017-05-18T03:55:55+00:00
    ```
    {: screen}
	
## How do I get the GUID of an account
{: #account_guid}
	
Complete the following steps to get the GUID of an account:
	
1. Log in to a {{site.data.keyword.Bluemix_notm}} region, organization, and space. Run the command:

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    Follow the instructions. Enter your {{site.data.keyword.Bluemix_notm}} credentials, select an organization and a space.
	
2. Run the `bx iam accounts` command to get the GUID of an account.

    ```
	bx iam accounts
	```
	{: codeblock} 
	
	For example, to retrieve all the accounts with their corresponding GUIDs for user xxx@yyy.com, run the command:
	
	```
	bx iam accounts
	Retrieving all accounts of xxx@yyy.com...
    OK
    Account GUID                       Name                               Type    State    Owner User ID   
    12345123451234512345123451234512   A Account                          TRIAL   ACTIVE   xxx@yyy.com   
    23456234562345622456234561234561   B Account                          TRIAL   ACTIVE   zzz@yyy.com   
	```
	{: screen}

	
## How do I get the GUID of an organization
{: #org_guid}

Complete the following steps to get the GUID of an organization:
	
1. Log in to a {{site.data.keyword.Bluemix_notm}} region, organization, and space. Run the command:

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    Follow the instructions. Enter your {{site.data.keyword.Bluemix_notm}} credentials, select an organization and a space.

2. Run the `bx iam org` command to get the organization GUID. 

    ```
    bx iam org NAME --guid
    ```
    {: codeblock}
	
    where NAME is the name of the {{site.data.keyword.Bluemix_notm}} organization.        
		
## How do I get the GUID of a space
{: #space_guid}
	
Complete the following steps to get the GUID of a space:
	
1. Log in to a {{site.data.keyword.Bluemix_notm}} region, organization, and space. Run the command:

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    Follow the instructions. Enter your {{site.data.keyword.Bluemix_notm}} credentials, select an organization and a space.
	
2. Run the `bx iam space` command to get the space GUID. 

    ```
    bx iam space NAME --guid
    ```
    {: codeblock}
	
    where NAME is the name of a {{site.data.keyword.Bluemix_notm}} space. 
	
    For example, to get the GUID for the space *dev*, run the following command:
	
    ```
    bx iam space dev --guid
    e03afff1-3456-4af6-1234-59treg1b0abc
    ```
    {: screen}




		
		