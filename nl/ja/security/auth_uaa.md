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


# Bluemix UAA モデルを使用した認証
{: #auth_uaa}

{{site.data.keyword.Bluemix}} UAA モデルを使用して、{{site.data.keyword.Bluemix_notm}} 内のスペース用に、{{site.data.keyword.monitoringshort}} サービスに保管されているメトリックにアクセスするために使用できる認証トークンを取得します。この認証トークンは、{{site.data.keyword.Bluemix_notm}} CLI を使用するか、`Login` REST API を使用して取得できます。
{:shortdesc}

UAA 認証トークンを使用してメトリックにアクセスするには、以下の情報が必要です。

* RESTful API を使用して {{site.data.keyword.monitoringshort}} サービスにアクセスするための UAA トークン。
* スペースの GUID。

		
## Bluemix CLI を使用した UAA トークンの取得
{: #uaa_cli}


許可トークンを取得するには、以下の手順を実行します。

1. {{site.data.keyword.Bluemix_notm}} CLI をインストールします。

   詳しくは、[{{site.data.keyword.Bluemix_notm}} CLI のインストール](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa)を参照してください。
   
   CLI がインストールされたら、次の手順に進んでください。
    
2. {{site.data.keyword.Bluemix_notm}} の地域、組織、およびスペースにログインします。以下のコマンドを実行します。

    ```
    cf login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    指示に従います。{{site.data.keyword.Bluemix_notm}} の資格情報を入力し、組織とスペースを選択します。
	
3. `cf oauth-token` コマンドを実行して、{{site.data.keyword.Bluemix_notm}} UAA トークンを取得します。

    ```
	cf oauth-token
	```
	{: codeblock}
	
	この出力は、そのスペースおよび組織でユーザー ID を認証するために使用する必要がある UAA トークンを返します。

4. 認証トークンを取得した組織のスペースの GUID を取得します。

   詳細情報については、[スペースの GUID の取得方法を教えてください](/docs/services/cloud-monitoring/qa/cli_qa.html#space_guid) を参照してください。
	
5. 変数 TOKEN および SPACE をエクスポートします。

    * *TOKEN* は、直前のステップで取得した OAuth トークン (Bearer を除く) です。
	
	* *SPACE* は、直前のステップで取得したスペースの GUID です。
		
	以下に例を示します。 	
	
	```
	export TOKEN="eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8"
	export SPACE="667fb895-abcd-defg-aaaa-cf4587341095"
	```
	{: screen}
	
6. 以下のコマンドを実行して、{{site.data.keyword.monitoringshort}} サービスを処理するための UAA トークンを取得します。

    ```
	curl -k -X GET  --header "X-Auth-Token: ${TOKEN}"  --header "X-Auth-Project-Id: ${SPACE}"  METRICS_ENDPOINT
    ```
    {: codeblock}	
	
	ここで、
	* SPACE は、サービスが実行されているスペースの GUID です。
	* TOKEN は、前の手順で取得した {{site.data.keyword.Bluemix_notm}} UAA トークンで、bearer 接頭部は付いていません。
	* METRICS_ENDPOINT は、組織およびスペースが使用可能な {{site.data.keyword.Bluemix_notm}} 地域のメトリック・エンドポイント (https://metrics.ng.bluemix.net/token) です。

	
## API を使用した UAA トークンの取得
{: #uaa_api}

許可トークンを取得するには、以下の cURL コマンドを実行します。

```
curl -XPOST -d 'user=USERNAME&passwd=PASSWORD&space=SPACE_NAME&organization=ORG_NAME' METRICS_ENDPOINT
```
{: codeblock}

ここで、

* USERNAME は、{{site.data.keyword.monitoringshort}} サービスを処理するための認証トークンの取得対象 {{site.data.keyword.Bluemix_notm}} ID です。
* PASSWORD は、{{site.data.keyword.Bluemix_notm}} にログインするために使用されるユーザー ID のパスワードです。
* SPACE_NAME は、メトリックを収集するスペースの名前です。
* ORG_NAME は、スペースがホストされている {{site.data.keyword.Bluemix_notm}} 内の組織名です。
* METRICS_ENDPOINT は、組織およびスペースが使用可能な {{site.data.keyword.Bluemix_notm}} 地域のメトリック・エンドポイント (https://metrics.ng.bluemix.net/login) です。

	
この出力は、UAA トークンを含む JSON 文書です。トークンの値は、**access-token** フィールドから取得します。

例えば、サンプル JSON 文書は以下のようになります。

```
{
    "access_token": "eyJhbGc...",
    "logging_token": "xxxxxxxxxxxx",
    "org_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "space_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```
{: screen}

*logging_token* の値は、{{site.data.keyword.monitoringshort}} サービスを処理するために必要な UAA トークンに対応しています。
**注:**

* cURL を使用していない場合は、ヘッダー **Content-Type: application/x-www-form-urlencoded** を設定する必要があります。
* エラー・コード *BXNMS0122E: User credentials are invalid* を受け取る場合は、有効な {{site.data.keyword.IBM_notm}} ID を使用していることを確認してください。


