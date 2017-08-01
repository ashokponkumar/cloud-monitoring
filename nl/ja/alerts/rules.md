---

copyright:
  years: 2017

lastupdated: "2017-07-18"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# アラート API を使用したルールの処理
{: #rules}

アラート API を使用して、ルールを作成、削除、および更新し、ルールの詳細を表示し、{{site.data.keyword.Bluemix_notm}} スペース内に定義されているルールをリストします。
{:shortdesc}


## ルールの作成
{: #create}

ルールを作成するには、以下の手順を実行します。

1. 有効な JSON を含むルール・ファイルを作成します。ファイルを保存します。 

    例えば、ファイルを保存するには、{{site.data.keyword.Bluemix_notm}} スペース内で実行されている照会に対して定義するルールには、*s-* のような接頭部を使用します。
	
	**ヒント:** 
	
	* 異なる通知方法を使用してエラーまたは警告についてのアラートを出すための照会には、別のルールを定義します。設定できる通知方法は、1 つのルールにつき 1 つのみです。 
	* *~/cloud-monitoring/rules/* ディレクトリーにルール・ファイルを作成して、{{site.data.keyword.monitoringshort_notm}} サービスのリソースをグループ化します。 

    ルールのファイルに以下の情報を入力します。
	
	* *name*: 固有の名前を入力します。このフィールドの値は、後でルールを更新および削除したり、ルールの詳細を表示したりするために使用されます。
	* *description*: 説明を入力します。
	* *expression*: モニターする照会を入力します。
	* *enabled*: ルールを有効にするには、*true* に設定します。
	* *from*: しきい値に基づいてデータを分析するために使用される初期ポイント・イン・タイム。
	* *until*: しきい値に基づいてデータを分析するために使用される最終ポイント・イン・タイム。
	* *comparison*: 実行するチェックのタイプを識別するために使用される比較演算。例えば、*below*。
	* *error_level*: エラー・アラートをトリガーするために設定するしきい値を定義します。
	* *warning_level*: 警告アラートをトリガーするために設定するしきい値を定義します。
	* *frequency*: データをチェックする頻度を定義します。
	* *dashboard_url*: 照会を含むダッシュボードを Grafana 内に表示する URL を定義します。
	* *notifications*: このルールによって記述されているアラートがトリガーされた場合の通知方法。
	
	以下に例を示します。 
	
	```
	{
    "name": "checkbytesin",
    "description": "MH check Bytes In per second",
    "expression": "movingAverage(messagehub.65qser11-8034-1234-5678-c82fb42wdfgh.1.kafka-java-console-sample-topic.BytesInPerSec.15MinuteRate,\"5min\")",
    "enabled": true,
    "from": "-5min",
    "until": "now",
    "comparison": "above",
    "comparison_scope": "last",
    "error_level" : 27,
    "warning_level" : 25,
    "frequency": "1min",
    "dashboard_url": "https://metrics.ng.bluemix.net",
    "notifications": [
      "emailXXX"
    ]
    }
    ```
	{: screen}
	
2. {{site.data.keyword.Bluemix_notm}} の地域、組織、およびスペースにログインします。以下のコマンドを実行します。

    例えば、米国南部地域にログインするには、以下のコマンドを実行します。
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    指示に従います。{{site.data.keyword.Bluemix_notm}} の資格情報を入力し、組織とスペースを選択します。

3. 認証トークンまたは API キーを取得します。

    * IAM 認証については、[Bluemix CLI を使用した IAM トークンの取得](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli)または [Bluemix CLI を使用した IAM API キーの生成](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli)を参照してください。
	
	* UAA 認証については、[Bluemix CLI を使用した UAA トークンの取得](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli)または [REST API を使用した UAA トークンの取得](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api)を参照してください。

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
	
	次に、以下のようにして変数 *Token* をエクスポートします。
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**注:** このトークンは *Bearer* を除外します。
		
4. スペース GUID を取得します。この GUID には、スペースを識別するための *s-* の接頭部が付いている必要があります。

    以下のコマンドを実行します。
	
	```
	bx iam space SpaceName --guid
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
	
	次に、以下のようにして変数 *Space* をエクスポートします。
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. 以下の cURL コマンドを実行して、ルールを作成します。

    ```
	curl -XPOST -d @Rule_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: codeblock}
	
	詳細は次のとおりです。
	
	* Rule_File は、アラート・ルールを定義する JSON ファイルです。
	
	* * *X-Auth-User-Token* は、{{site.data.keyword.Bluemix_notm}} の UAA トークン、IAM トークン、または API キーを渡すパラメーターです。
	
	* *Auth_Type* は、トークンまたは API キーのタイプを定義する接頭部です。以下のリストに、有効値 *apikey*、*iam*、または *uaa* の概要を示します。詳細は次のとおりです。

  * *apikey* は、トークンが API キーであることを識別します。
		* *iam* は、指定されたトークンが IAM 生成トークンであることを識別します。
		* *uaa* は、指定されたトークンが UAA 生成トークンであることを識別します。
	
	* *X-Auth-Scope-Id* は、スペースの GUID を渡すパラメーターです。この GUID には、スペースを識別するための *s-* の接頭部が付いている必要があります。UAA トークン認証を使用する場合、このヘッダーは必須です。IAM 認証トークンを使用する場合、このヘッダーはオプションで、このトークンにバインドされているドメイン情報が使用されます。
	
	* Token は、UAA 認証トークン、IAM 認証トークン、または API キーです。
	
	* Space は、スペースの GUID です。これは、UAA トークンを使用する場合にのみ必要です。
	
	以下に例を示します。 	
	
	```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}

## ルールの削除
{: #delete}

ルールを削除するには、以下の手順を実行します。

1. {{site.data.keyword.Bluemix_notm}} の地域、組織、およびスペースにログインします。以下のコマンドを実行します。

    例えば、米国南部地域にログインするには、以下のコマンドを実行します。
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    指示に従います。{{site.data.keyword.Bluemix_notm}} の資格情報を入力し、組織とスペースを選択します。

2. 認証トークンまたは API キーを取得します。

    * IAM 認証については、[[Bluemix CLI を使用した IAM トークンの取得](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) または [Bluemix CLI を使用した IAM API キーの生成](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli) を参照してください。
	
	* UAA 認証については、[Bluemix CLI を使用した UAA トークンの取得](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) または [REST API を使用した UAA トークンの取得](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api) を参照してください。

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
	
	次に、以下のようにして変数 *Token* をエクスポートします。
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**注:** このトークンは *Bearer* を除外します。
		
3. スペース GUID を取得します。この GUID には、スペースを識別するための *s-* の接頭部が付いている必要があります。

    以下のコマンドを実行します。
	
	```
	bx iam space SpaceName --guid
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
	
	次に、以下のようにして変数 *Space* をエクスポートします。
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. 以下の cURL コマンドを実行して、ルールを削除します。

    ```
	curl -XDELETE --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/rule/Rule_Name
	```
	{: codeblock}
	
	詳細は次のとおりです。
	
    * * *X-Auth-User-Token* は、{{site.data.keyword.Bluemix_notm}} の UAA トークン、IAM トークン、または API キーを渡すパラメーターです。
	
	* *Auth_Type* は、トークンまたは API キーのタイプを定義する接頭部です。以下のリストに、有効値 *apikey*、*iam*、または *uaa* の概要を示します。詳細は次のとおりです。

  * *apikey* は、トークンが API キーであることを識別します。
		* *iam* は、指定されたトークンが IAM 生成トークンであることを識別します。
		* *uaa* は、指定されたトークンが UAA 生成トークンであることを識別します。
	
	* *X-Auth-Scope-Id* は、スペースの GUID を渡すパラメーターです。この GUID には、スペースを識別するための *s-* の接頭部が付いている必要があります。UAA トークン認証を使用する場合、このヘッダーは必須です。IAM 認証トークンを使用する場合、このヘッダーはオプションで、このトークンにバインドされているドメイン情報が使用されます。
	
	* Token は、UAA 認証トークン、IAM 認証トークン、または API キーです。
	
	* Space は、スペースの GUID です。これは、UAA トークンを使用する場合にのみ必要です。
	
	* Rule_Name は、*name* フィールドに指定されているルールの名前です。
	

	
## すべてのルールのリスト
{: #list}

すべてのルールをリストするには、以下の手順を実行します。

1. {{site.data.keyword.Bluemix_notm}} の地域、組織、およびスペースにログインします。以下のコマンドを実行します。

    例えば、米国南部地域にログインするには、以下のコマンドを実行します。
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    指示に従います。{{site.data.keyword.Bluemix_notm}} の資格情報を入力し、組織とスペースを選択します。

2. 認証トークンまたは API キーを取得します。

    * IAM 認証については、[[Bluemix CLI を使用した IAM トークンの取得](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) または [Bluemix CLI を使用した IAM API キーの生成](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli) を参照してください。
	
	* UAA 認証については、[Bluemix CLI を使用した UAA トークンの取得](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) または [REST API を使用した UAA トークンの取得](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api) を参照してください。

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
	
	次に、以下のようにして変数 *Token* をエクスポートします。
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**注:** このトークンは *Bearer* を除外します。
		
3. スペース GUID を取得します。この GUID には、スペースを識別するための *s-* の接頭部が付いている必要があります。

    以下のコマンドを実行します。
	
	```
	bx iam space SpaceName --guid
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
	
	次に、以下のようにして変数 *Space* をエクスポートします。
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. 以下の cURL コマンドを実行して、スペース内のすべてのルールをリストします。

    ```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/rules
	```
	{: codeblock}
	
	詳細は次のとおりです。
	
	* * *X-Auth-User-Token* は、{{site.data.keyword.Bluemix_notm}} の UAA トークン、IAM トークン、または API キーを渡すパラメーターです。
	
	* *Auth_Type* は、トークンまたは API キーのタイプを定義する接頭部です。以下のリストに、有効値 *apikey*、*iam*、または *uaa* の概要を示します。詳細は次のとおりです。

  * *apikey* は、トークンが API キーであることを識別します。
		* *iam* は、指定されたトークンが IAM 生成トークンであることを識別します。
		* *uaa* は、指定されたトークンが UAA 生成トークンであることを識別します。
	
	* *X-Auth-Scope-Id* は、スペースの GUID を渡すパラメーターです。この GUID には、スペースを識別するための *s-* の接頭部が付いている必要があります。UAA トークン認証を使用する場合、このヘッダーは必須です。IAM 認証トークンを使用する場合、このヘッダーはオプションで、このトークンにバインドされているドメイン情報が使用されます。
	
	* Token は、UAA 認証トークン、IAM 認証トークン、または API キーです。
	
	* Space は、スペースの GUID です。これは、UAA トークンを使用する場合にのみ必要です。
	
	## ルールの詳細の表示
{: show}

ルールの詳細を表示するには、以下の手順を実行します。

1. {{site.data.keyword.Bluemix_notm}} の地域、組織、およびスペースにログインします。以下のコマンドを実行します。

    例えば、米国南部地域にログインするには、以下のコマンドを実行します。
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    指示に従います。{{site.data.keyword.Bluemix_notm}} の資格情報を入力し、組織とスペースを選択します。

2. 認証トークンまたは API キーを取得します。

    * IAM 認証については、[[Bluemix CLI を使用した IAM トークンの取得](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) または [Bluemix CLI を使用した IAM API キーの生成](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli) を参照してください。
	
	* UAA 認証については、[Bluemix CLI を使用した UAA トークンの取得](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) または [REST API を使用した UAA トークンの取得](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api) を参照してください。

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
	
	次に、以下のようにして変数 *Token* をエクスポートします。
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**注:** このトークンは *Bearer* を除外します。
		
3. スペース GUID を取得します。この GUID には、スペースを識別するための *s-* の接頭部が付いている必要があります。

    以下のコマンドを実行します。
	
	```
	bx iam space SpaceName --guid
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
	
	次に、以下のようにして変数 *Space* をエクスポートします。
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. 以下の cURL コマンドを実行して、ルールの詳細を表示します。

    ```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/rule/Rule_Name
	```
	{: codeblock}
	
	詳細は次のとおりです。
	
	* * *X-Auth-User-Token* は、{{site.data.keyword.Bluemix_notm}} の UAA トークン、IAM トークン、または API キーを渡すパラメーターです。
	
	* *Auth_Type* は、トークンまたは API キーのタイプを定義する接頭部です。以下のリストに、有効値 *apikey*、*iam*、または *uaa* の概要を示します。詳細は次のとおりです。

  * *apikey* は、トークンが API キーであることを識別します。
		* *iam* は、指定されたトークンが IAM 生成トークンであることを識別します。
		* *uaa* は、指定されたトークンが UAA 生成トークンであることを識別します。
	
	* *X-Auth-Scope-Id* は、スペースの GUID を渡すパラメーターです。この GUID には、スペースを識別するための *s-* の接頭部が付いている必要があります。UAA トークン認証を使用する場合、このヘッダーは必須です。IAM 認証トークンを使用する場合、このヘッダーはオプションで、このトークンにバインドされているドメイン情報が使用されます。
	
	* Token は、UAA 認証トークン、IAM 認証トークン、または API キーです。
	
	* Space は、スペースの GUID です。これは、UAA トークンを使用する場合にのみ必要です。
	
	* Rule_Name は、*name* フィールドに指定されているルールの名前です。
	

## ルールの更新
{: #update}

ルールを更新するには、以下の手順を実行します。

1. {{site.data.keyword.Bluemix_notm}} の地域、組織、およびスペースにログインします。以下のコマンドを実行します。

    例えば、米国南部地域にログインするには、以下のコマンドを実行します。
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    指示に従います。{{site.data.keyword.Bluemix_notm}} の資格情報を入力し、組織とスペースを選択します。

2. 認証トークンまたは API キーを取得します。

    * IAM 認証については、[[Bluemix CLI を使用した IAM トークンの取得](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) または [Bluemix CLI を使用した IAM API キーの生成](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli) を参照してください。
	
	* UAA 認証については、[Bluemix CLI を使用した UAA トークンの取得](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) または [REST API を使用した UAA トークンの取得](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api) を参照してください。

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
	
	次に、以下のようにして変数 *Token* をエクスポートします。
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**注:** このトークンは *Bearer* を除外します。
		
3. スペース GUID を取得します。この GUID には、スペースを識別するための *s-* の接頭部が付いている必要があります。

    以下のコマンドを実行します。
	
	```
	bx iam space SpaceName --guid
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
	
	次に、以下のようにして変数 *Space* をエクスポートします。
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. 以下の cURL コマンドを実行して、ルールを更新します。

    ```
	curl -XPUT `-d @Rule_File` --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: codeblock}
	
	詳細は次のとおりです。
	
	* Rule_File は、アラート・ルールを定義する JSON ファイルです。
	
	* * *X-Auth-User-Token* は、{{site.data.keyword.Bluemix_notm}} の UAA トークン、IAM トークン、または API キーを渡すパラメーターです。
	
	* *Auth_Type* は、トークンまたは API キーのタイプを定義する接頭部です。以下のリストに、有効値 *apikey*、*iam*、または *uaa* の概要を示します。詳細は次のとおりです。

  * *apikey* は、トークンが API キーであることを識別します。
		* *iam* は、指定されたトークンが IAM 生成トークンであることを識別します。
		* *uaa* は、指定されたトークンが UAA 生成トークンであることを識別します。
	
	* *X-Auth-Scope-Id* は、スペースの GUID を渡すパラメーターです。この GUID には、スペースを識別するための *s-* の接頭部が付いている必要があります。UAA トークン認証を使用する場合、このヘッダーは必須です。IAM 認証トークンを使用する場合、このヘッダーはオプションで、このトークンにバインドされているドメイン情報が使用されます。
	
	* Token は、UAA 認証トークン、IAM 認証トークン、または API キーです。
	
	* Space は、スペースの GUID です。これは、UAA トークンを使用する場合にのみ必要です。
	
	
