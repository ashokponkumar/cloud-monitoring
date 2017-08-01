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


# Bluemix CLI の使用に関するよくある質問と回答
{: #cli_qa}

{{site.data.keyword.monitoringshort}} サービスでの {{site.data.keyword.Bluemix}} CLI の使用に関する一般的な質問の回答を以下に示します。
{:shortdesc}

* [{{site.data.keyword.Bluemix_notm}} CLI のインストール方法を教えてください](#install_bmx_cli)
* [アカウントの GUID の取得方法を教えてください](#account_guid)
* [組織の GUID の取得方法を教えてください](#org_guid)
* [スペースの GUID の取得方法を教えてください](#space_guid)


## Bluemix CLI のインストール方法を教えてください
{: #install_bmx_cli}

{{site.data.keyword.Bluemix_notm}} CLI をインストールするには、以下の手順を実行します。

1. CLI をダウンロードします。

    例えば、{{site.data.keyword.Bluemix_notm}} CLI パッケージを Ubuntu システムにインストールするには、[{{site.data.keyword.Bluemix_notm}} CLI パッケージ ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](http://clis.ng.bluemix.net/ui/home.html "外部リンク・アイコン"){: new_window} をダウンロードします。 

2. 以下のコマンドを実行して、{{site.data.keyword.Bluemix_notm}} CLI パッケージを解凍します。
    
    ```
    tar -xvf Bluemix_CLI_0.5.4_amd64.tar.gz
    ```
    {: codeblock}
    
3. *Bluemix_CLI* ディレクトリーに移動し、root 権限で `./install_bluemix_cli` コマンドを実行して CF プラグインをインストールします。このコマンドを root ユーザーとして実行するか、sudo コマンドを使用して root 権限を取得できます。以下に例を示します。
    
    ```
    cd Bluemix_CLI
    ```
    {: codeblock}
    
    ```
    sudo ./install_bluemix_cli
    ```
    {: codeblock}
    
4. CF プラグインのインストールを検証します。例えば、CF プラグインのバージョンを確認します。以下のコマンドを実行します。
    
    ```
    cf -v
    ```
    {: codeblock}
    
    出力は以下のようになります。
    
    ```
    cf version 6.25.0+787326d.2017-02-28
    ```
    {: screen}
    
5. {{site.data.keyword.Bluemix_notm}} CLI のインストールを検証します。例えば、プラグインのバージョンを確認します。以下のコマンドを実行します。
    
    ```
    bx -version
    ```
    {: codeblock}
    
    出力は以下のようになります。
    
    ```
    bx version 0.5.4+ae22935-2017-05-18T03:55:55+00:00
    ```
    {: screen}
	
## アカウントの GUID の取得方法を教えてください
{: #account_guid}
	
アカウントの GUID を取得するには、以下の手順を実行します。
	
1. {{site.data.keyword.Bluemix_notm}} の地域、組織、およびスペースにログインします。以下のコマンドを実行します。

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    指示に従います。{{site.data.keyword.Bluemix_notm}} の資格情報を入力し、組織とスペースを選択します。
	
2. `bx iam accounts` コマンドを実行して、アカウントの GUID を取得します。

    ```
	bx iam accounts
	```
	{: codeblock}
	
	例えば、ユーザー xxx@yyy.com のすべてのアカウントを、対応する GUID と共に取得するには、以下のコマンドを実行します。
	
	```
	bx iam accounts
	Retrieving all accounts of xxx@yyy.com...
    OK
    Account GUID                       Name                               Type    State    Owner User ID
    12345123451234512345123451234512   A Account                          TRIAL   ACTIVE   xxx@yyy.com
    23456234562345622456234561234561   B Account                          TRIAL   ACTIVE   zzz@yyy.com
	```
	{: screen}

	
## 組織の GUID の取得方法を教えてください
{: #org_guid}

組織の GUID を取得するには、以下の手順を実行します。
	
1. {{site.data.keyword.Bluemix_notm}} の地域、組織、およびスペースにログインします。以下のコマンドを実行します。

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    指示に従います。{{site.data.keyword.Bluemix_notm}} の資格情報を入力し、組織とスペースを選択します。

2. `bx iam org` コマンドを実行して、組織の GUID を取得します。

    ```
    bx iam org NAME --guid
    ```
    {: codeblock}
	
    ここで、NAME は、{{site.data.keyword.Bluemix_notm}} 組織の名前です。
		
## スペースの GUID の取得方法を教えてください
{: #space_guid}
	
スペースの GUID を取得するには、以下の手順を実行します。
	
1. {{site.data.keyword.Bluemix_notm}} の地域、組織、およびスペースにログインします。以下のコマンドを実行します。

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    指示に従います。{{site.data.keyword.Bluemix_notm}} の資格情報を入力し、組織とスペースを選択します。

2. `bx iam space` コマンドを実行して、スペースの GUID を取得します。

    ```
    bx iam space NAME --guid
    ```
    {: codeblock}
	
    ここで、NAME は、{{site.data.keyword.Bluemix_notm}} スペースの名前です。
	
    例えば、スペース *dev* の GUID を取得するには、以下のコマンドを実行します。
	
    ```
    bx iam space dev --guid
    e03afff1-3456-4af6-1234-59treg1b0abc
    ```
{: screen}

	
