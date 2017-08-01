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


# Bluemix IAM モデルを使用した認証
{: #auth_iam}

{{site.data.keyword.Bluemix}} IAM モデルを使用して、{{site.data.keyword.monitoringshort}} サービスに保管されているメトリックへのアクセスに使用できる認証トークンを取得したり、API キーを取得したりします。トークンには有効期限があります。API キーには有効期限切れはありません。
{:shortdesc}


## Bluemix CLI を使用した IAM トークンの取得 
{: #iam_token_cli}

{{site.data.keyword.Bluemix_notm}} CLI を使用して許可トークンを取得するには、以下の手順を実行します。

1. {{site.data.keyword.Bluemix_notm}} CLI をインストールします。

   詳しくは、[{{site.data.keyword.Bluemix_notm}} CLI のインストール](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa)を参照してください。
   
   CLI がインストールされたら、次の手順に進んでください。
    
2. {{site.data.keyword.Bluemix_notm}} の地域、組織、およびスペースにログインします。以下のコマンドを実行します。

    例えば、米国南部地域の場合は、以下のコマンドを実行します。
	
    ```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    指示に従います。{{site.data.keyword.Bluemix_notm}} の資格情報を入力し、組織とスペースを選択します。
	
3. `bx iam oauth-tokens` コマンドを実行して、IAM トークンを取得します。

    ```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	この出力は、そのスペースおよび組織でユーザー ID を認証するために使用する必要がある IAM トークンを返します。

**注:** トークンを使用する場合、API 呼び出しで渡すトークンの値から *Bearer* を削除してください。 		
		
## Bluemix CLI を使用した IAM API キーの生成
{: #iam_apikey_cli}

{{site.data.keyword.Bluemix_notm}} CLI を使用して IAM API キーを生成するには、以下の手順を実行します。

1. {{site.data.keyword.Bluemix_notm}} CLI をインストールします。

   詳しくは、[{{site.data.keyword.Bluemix_notm}} CLI のインストール](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa) を参照してください。

   CLI がインストールされたら、次の手順に進んでください。
	
2. 以下のように `bx login` {{site.data.keyword.Bluemix_notm}} CLI コマンドを使用して、{{site.data.keyword.Bluemix_notm}} にログインします。

    例えば、米国南部地域の場合は、以下のコマンドを実行します。
	
    ```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    指示に従います。{{site.data.keyword.Bluemix_notm}} の資格情報を入力し、組織とスペースを選択します。

3. `bx iam api-key-create` コマンドを実行して、IAM API キーを作成します。

    ```
    bx iam api-key-create NAME [-d DESCRIPTION][-f, --file FILE]
	```
	{: codeblock}
	
	ここで、
	
	* NAME は、作成する API キーの名前です。
	* DESCRIPTION は、API キーを説明するために使用するテキストです。
	* FILE は、キーを保存するファイルの名前です。
	
    例えば、API キー *MyKey* を作成するには、以下のコマンドを実行します。
	
	```
	bx iam api-key-create MyKey -d "This is my API key for service X" 
	```
	{: screen}
	
## Bluemix UI を使用した IAM API キーの生成
{: #iam_apikey_ui}

{{site.data.keyword.Bluemix_notm}} UI を介して IAM API キーを生成するには、以下の手順を実行します。

1. {{site.data.keyword.Bluemix_notm}} コンソールにログインします。

2. コンソールのメニュー・バーから、**「管理」>「セキュリティー」>「Bluemix API キー」**をクリックします。

3. **「API キーの作成」**をクリックします。

4. API キーの名前と説明を入力します。次に、**「API キーの作成」**をクリックします。

5. API キーを保存します。**「API キーのダウンロード」**をクリックします。

    **「表示」**をクリックして、API キーを表示します。

**注:** API キーは、作成時にのみ表示またはダウンロードできます。API キーが消失した場合は、新しい API キーを作成する必要があります。


	

	
## Bluemix UI を使用した API キーの取り消し
{: #revoke_ui}
	
{{site.data.keyword.Bluemix_notm}} UI を介して IAM API キーを取り消すには、以下の手順を実行します。

1. {{site.data.keyword.Bluemix_notm}} コンソールにログインします。

2. コンソールのメニュー・バーから、**「管理」>「セキュリティー」>「Bluemix API キー」**をクリックします。

3. **「アクション」**をクリックしてから**「キーの削除」**をクリックします。





	

	
	
	
	
	
	
