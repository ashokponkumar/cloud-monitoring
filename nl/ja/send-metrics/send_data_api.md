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


# Monitoring サービスへのデータの送信
{: #send_data_api}

Metrics API を使用して、{{site.data.keyword.monitoringshort}} サービスにメトリックを送信することができます。メトリックは、{{site.data.keyword.Bluemix_notm}} のスペースに送信できます。
{:shortdesc}


{{site.data.keyword.Bluemix_notm}} Docker コンテナーの場合、基本のシステム・メトリックは自動的に収集されます。Cloud Foundry アプリケーション、および仮想計算機 (VM) で実行されているアプリの場合、メトリックは、[Metrics API](https://console.bluemix.net/apidocs/927-ibm-cloud-monitoring-rest-api?&language=node#introduction){: new_window} を使用してアプリから直接送信する必要があります。 



## UAA を使用したスペースへのメトリックの送信
{: #uaa}

メトリックを {{site.data.keyword.Bluemix_notm}} スペースに送信するには、以下の手順を実行します。

1. {{site.data.keyword.Bluemix_notm}} の地域、組織、およびスペースにログインします。以下のコマンドを実行します。

    例えば、米国南部地域にログインするには、以下のコマンドを実行します。
	
	```
    cf login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    指示に従います。{{site.data.keyword.Bluemix_notm}} の資格情報を入力し、組織とスペースを選択します。

2. UAA 認証トークンを取得します。

    UAA 認証について詳しくは、[Bluemix CLI を使用した UAA トークンの取得](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli)または [REST API を使用した UAA トークンの取得](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api)を参照してください。

	例えば、UAA トークンを使用するには、以下のコマンドを実行します。

    ```
	cf oauth-token
	```
	{: codeblock}
	
	このコマンドの結果は以下のようになります。
	
	```
	bearer eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8
	```
	{: screen}
	
	次に、変数 *Token* をエクスポートします。以下に例を示します。```
	export Token="eyJhbGciOiJI....cGlxryWS8"
	```
	{: screen}
	
	**注:** このトークンは *Bearer* を除外します。
		
3. スペース GUID を取得します。この GUID には、スペースを識別するための *s-* の接頭部が付いている必要があります。

    以下のコマンドを実行します。
	
	```
	cf space SpaceName --guid
	```
	{: codeblock}
	
	ここで、*SpaceName* は、通知を定義するスペースの名前です。
	
	例えば、*dev* という名前のスペースの GUID を取得するには、以下のコマンドを実行します。
	
	```
	cf space dev --guid
	```
	{: screen}
	
	このコマンドの結果は以下のようになります。
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	次に、変数 *Space* をエクスポートします。以下に例を示します。```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. 以下の cURL コマンドを実行して、メトリックを送信します。

    ```
	curl -XPOST -d @Metric_File --header "X-Auth-User-Token:uaa ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics
	```
	{: codeblock}
	
	ここで、
	
	* Metrics_File は、{{site.data.keyword.monitoringshort}} サービスに送信されるメトリックの定義を含む JSON ファイルを表しています。
	
	* *X-Auth-User-Token* は、{{site.data.keyword.Bluemix_notm}} の UAA トークンを渡すパラメーターです。
	
	* *Auth_Type* は、トークンのタイプを定義する接頭部です。*uaa* は、指定されたトークンが UAA 生成トークンであることを識別します。
	
	* *X-Auth-Scope-Id* は、スペースの GUID を渡すパラメーターです。この GUID には、スペースを識別するための *s-* の接頭部が付いている必要があります。

 * Token は、UAA トークンを表します。
	
	* Space は、スペースの GUID を表します。例えば、以下のコマンドを実行して、2 つのメトリック `myhost.cpu.idle` と `myapp.login.attempts` を {{site.data.keyword.monitoringshort}} サービスに送信することができます。
	
	```
	curl -XPOST @metric.json --header "X-Auth-User-Token:uaa ${Token}" https://metrics.ng.bluemix.net/v1/metrics
	```
	{: screen}
	
	サンプル・ファイル *metric.json* は以下のように定義されています。

    ```
    [
      {
        "name" : "myhost.cpu.idle",
        "value" : 22.37
      },
      {
        "name": "myapp.login.retries",
        "value": 4
      }
    ]
	```
	{: screen}


## IAM または API キーを使用したスペースへのメトリックの送信
{: #iam}

メトリックを {{site.data.keyword.Bluemix_notm}} スペースに送信するには、以下の手順を実行します。

1. {{site.data.keyword.Bluemix_notm}} の地域、組織、およびスペースにログインします。以下のコマンドを実行します。

    例えば、米国南部地域にログインするには、以下のコマンドを実行します。
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    指示に従います。{{site.data.keyword.Bluemix_notm}} の資格情報を入力し、組織とスペースを選択します。

2. 認証トークンまたは API キーを取得します。

    IAM 認証について詳しくは、[Bluemix CLI を使用した IAM トークンの取得](/docs/services/cloud-monitoring/security/auth_iam.html#iam_cli) または [Bluemix CLI を使用した IAM API キーの生成](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli) を参照してください。
	例えば、IAM トークンを使用するには、以下のコマンドを実行します。

    ```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	このコマンドの結果は以下のようになります。
	
	```
	IAM token:  Bearer djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ
    UAA token:  Bearer eyJhbGciOiJIUz..Ky6vagp3k_QcIcKJ-td83qXhO5Uze43KcplG6PzcGs8
	```
	{: screen}
	
	次に、変数 *Token* をエクスポートします。以下に例を示します。```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**注:** このトークンは *Bearer* を除外します。
		
3. スペース GUID を取得します。スペース GUID を取得するには、[スペースの GUID を取得する方法を教えてください](/docs/service/cloud-monitoring/qa/cli_qa.html#space_guid) を参照してください。この GUID には、スペースを識別するための *s-* の接頭部が付いている必要があります。例えば、以下のコマンドを実行します。
	
	```
	    bx iam space NAME --guid
    ```
	{: codeblock}
	
	ここで、*SpaceName* は、通知を定義するスペースの名前です。
	
	例えば、*dev* という名前のスペースの GUID を取得するには、以下のコマンドを実行します。
	
	```
	bx iam space dev --guid
	```
	{: screen}
	
	このコマンドの結果は以下のようになります。
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	次に、変数 *Space* をエクスポートします。以下に例を示します。```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. cURL コマンドを実行してメトリックを送信します。

    ```
	curl -XPOST -d @Metric_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics
	```
	{: codeblock}
	
	ここで、
	
	* Metrics_File は、{{site.data.keyword.monitoringshort}} サービスに送信されるメトリックの定義を含む JSON ファイルを表しています。
	
	* *X-Auth-User-Token* は、{{site.data.keyword.Bluemix_notm}} の IAM トークン、または API キーを渡すパラメーターです。
	
	* *Auth_Type* は、トークンまたは API キーのタイプを定義する接頭部です。以下のリストに、有効値 *apikey*、*iam*、または *uaa* の概要を示します。詳細は次のとおりです。

  * *apikey* は、トークンが API キーであることを識別します。
		* *iam* は、指定されたトークンが IAM 生成トークンであることを識別します。
		* *X-Auth-Scope-Id* は、スペースの GUID を渡すパラメーターです。この GUID には、スペースを識別するための *s-* の接頭部が付いている必要があります。

 * Token は、IAM トークン、または API キーを表します。
	
	* Space は、スペースの GUID を表します。例えば、以下のコマンドを実行して、2 つのメトリック `myhost.cpu.idle` と `myapp.login.retries` を、スペースを宛先にして、{{site.data.keyword.monitoringshort}} サービスに送信することができます。
	
	```
curl -XPOST @metric.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/metrics
```
{: screen}
	
サンプル・ファイル *metric.json* は以下のように定義されています。

    ```
[
  {
    "name" : "myhost.cpu.idle",
        "value" : 22.37
      },
  {
    "name": "myapp.login.retries",
    "value": 4
  }
]
```
{: screen}















