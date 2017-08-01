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


# 使用 Bluemix CLI 的常见问题及解答
{: #cli_qa}

下面是对将 {{site.data.keyword.Bluemix}} CLI 与 {{site.data.keyword.monitoringshort}} 服务一起使用时的常见问题的解答。
{:shortdesc}

* [如何安装 {{site.data.keyword.Bluemix_notm}} CLI](#install_bmx_cli)
* [如何获取帐户的 GUID](#account_guid)
* [如何获取组织的 GUID](#org_guid)
* [如何获取空间的 GUID](#space_guid)


## 如何安装 Bluemix CLI？
{: #install_bmx_cli}

要安装 {{site.data.keyword.Bluemix_notm}} CLI，请完成以下步骤：

1. 下载 CLI。

    例如，要在 Ubuntu 系统中安装 {{site.data.keyword.Bluemix_notm}} CLI 包，请下载 [{{site.data.keyword.Bluemix_notm}} CLI 包 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](http://clis.ng.bluemix.net/ui/home.html "外部链接图标"){: new_window}。 

2. 运行以下命令来解压缩 {{site.data.keyword.Bluemix_notm}} CLI 包：
    
    ```
    tar -xvf Bluemix_CLI_0.5.4_amd64.tar.gz
    ```
    {: codeblock}
    
3. 转至 *Bluemix_CLI* 目录，然后使用 root 用户权限运行 `./install_bluemix_cli` 命令来安装 CF 插件。可以通过 root 用户身份运行此命令，也可以使用 sudo 命令来获取 root 用户权限。例如：
    
    ```
    cd Bluemix_CLI
    ```
    {: codeblock}
    
    ```
    sudo ./install_bluemix_cli
    ```
    {: codeblock}
    
4. 验证 CF 插件的安装情况。例如，检查 CF 插件的版本。运行以下命令：
    
    ```
cf -v
```
    {: codeblock}
    
    输出类似以下内容：
    
    ```
    cf version 6.25.0+787326d.2017-02-28
    ```
    {: screen}
    
5. 验证 {{site.data.keyword.Bluemix_notm}} CLI 的安装情况。例如，检查该插件的版本。运行以下命令：
    
    ```
    bx -version
    ```
    {: codeblock}
    
    输出类似以下内容：
    
    ```
    bx version 0.5.4+ae22935-2017-05-18T03:55:55+00:00
    ```
    {: screen}
	
## 如何获取帐户的 GUID
{: #account_guid}
	
要获取帐户的 GUID，请完成以下步骤：
	
1. 登录到 {{site.data.keyword.Bluemix_notm}} 区域、组织和空间。运行以下命令：

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    遵循指示信息进行操作。输入您的 {{site.data.keyword.Bluemix_notm}} 凭证，然后选择组织和空间。
	
2. 运行 `bx iam accounts` 命令来获取帐户的 GUID。

    ```
	bx iam accounts
	```
	{: codeblock} 
	
	例如，要检索用户 xxx@yyy.com 的所有帐户及其对应的 GUID，请运行以下命令：
	```
	bx iam accounts
	Retrieving all accounts of xxx@yyy.com...
    OK
    Account GUID                       Name                               Type    State    Owner User ID   
    12345123451234512345123451234512   A Account                          TRIAL   ACTIVE   xxx@yyy.com   
    23456234562345622456234561234561   B Account                          TRIAL   ACTIVE   zzz@yyy.com   
	```
	{: screen}

	
## 如何获取组织的 GUID
{: #org_guid}

要获取组织的 GUID，请完成以下步骤：
	
1. 登录到 {{site.data.keyword.Bluemix_notm}} 区域、组织和空间。运行以下命令：

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    遵循指示信息进行操作。输入您的 {{site.data.keyword.Bluemix_notm}} 凭证，然后选择组织和空间。

2. 运行“bx iam org”命令来获取组织 GUID。

    ```
    bx iam org NAME --guid
    ```
    {: codeblock}
	
    其中，NAME 是 {{site.data.keyword.Bluemix_notm}} 组织的 GUID。
		
## 如何获取空间的 GUID
{: #space_guid}
	
要获取空间的 GUID，请完成以下步骤：

1. 登录到 {{site.data.keyword.Bluemix_notm}} 区域、组织和空间。运行以下命令：

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    遵循指示信息进行操作。输入您的 {{site.data.keyword.Bluemix_notm}} 凭证，然后选择组织和空间。

2. 运行“bx iam space”命令来获取空间 GUID。

    ```
    bx iam space NAME --guid
    ```
    {: codeblock}
	
    其中，NAME 是 {{site.data.keyword.Bluemix_notm}} 空间的名称。
	
    例如，要获取空间 *dev* 的 GUID，请运行以下命令：
	
    ```
    bx iam space dev --guid
    e03afff1-3456-4af6-1234-59treg1b0abc
    ```
    {: screen}




		
		
