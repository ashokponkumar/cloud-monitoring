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


# アラート API を使用した通知の処理
{: #notifications}

アラート API を使用して、通知の作成、削除、および更新、通知の詳細表示、および {{site.data.keyword.Bluemix_notm}} スペース内に定義されている通知のリストを行うことができます。{:shortdesc}

## 通知テンプレートの作成
{: #template}

通知は JSON ファイルです。 

通知テンプレートはいくつでも作成でき、それらを再使用して組織内にそのタイプの通知を作成することができます。 

以下のいずれかのタイプの通知を定義できます。

* Email: *Email* タイプの通知を定義すると、有効な E メール・アドレスに E メールが送信されます。 
* Webhook: *Webhook* タイプの通知は、https　エンドポイントのみを対象に定義します。他のユーザーによって自分のエンドポイントが呼び出されないようにするには、エンドポイントにパラメーターを追加します。
* Pagerduty: *PagerDuty* タイプの通知を定義すると、メトリックのアラート・データが PagerDuty インシデント管理システムに送信されます。 

例として、以下の表に通知テンプレートのサンプルをリストします。

<table>
  <caption></caption>
  <tr>
    <th>タイプ</th>
	<th>テンプレート</th>
	<th>サンプル</th>
  </tr>
  <tr>
    <td>E メール</td>
	<td>
	```
	{
	"name": "Template_Name",
	"type": "Email",
	"description" : "Description",
	"detail": "EmailAddress"
	}
	```
	{: codeblock}
	</td>
	<td>
	```
	{
	"name": "my-email",
	"type": "Email",
	"description" : "Send email notification when there is an infrastructure problem.",
	"detail": "xxx@yyy.com"
	}
	```
	{: screen}
	</td>
  </tr>
  <tr>
    <td>Web フック</td>
	<td>
	```
	{
	"name": "Template_Name",
	"type": "Webhook",
	"description" : "Description",
	"detail": "Endpoint"
	}
	```
	{: codeblock}
	</td>
	<td>
	```
	{
	"name": "my-webhook",
	"type": "Webhook",
	"description" : "Fire a webhook when there is an infrastructure problem..",
	"detail": "https://myendpoint.bluemix.net?key=abcd1234"
	}
	```
	{: screen}
	</td>
  </tr>
  <tr>
    <td>Pagerduty</td>
	<td>
	```
	{
	"name": "Template_Name",
	"type": "PagerDuty",
	"description" : "Description",
	"detail": "Pagerduty_APIkey"
	}
	```
	{: codeblock}
	</td>
	<td>
	```
	{
	"name": "my-pagerduty",
	"type": "PagerDuty",
	"description" : "Fire a PagerDuty alert when there is an infrastructure problem..",
	"detail": "abcd1234"
	}
	```
	{: screen}
	</td>
  </tr>
</table>

各部の意味は、次のとおりです。

* *Template_Name* は、通知テンプレートの名前を定義します。
* *Description* は、このタイプの通知がどのような時に使用されるかを説明します。
* *EmailAddress* は、通知の受信者の E メール・アドレスを定義します。
* *Endpoint* は、POST を実行する URL を定義します。これは、POST 要求を受け取ることのできる URL です。 
* *Pagerduty_APIkey* は、固有 API キーを定義します。この API キーは、PagerDuty アカウントの管理者または所有者によって生成されます。


通知テンプレートを作成するには、以下の手順を実行します。

1. {{site.data.keyword.monitoringshort}} サービス・リソースを保管するディレクトリーを作成します。例えば、*cloud-monitoring* など。

    例えば、Ubuntu システムでは以下のコマンドを実行します。
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
2. 通知テンプレートを保管するディレクトリーを作成します。例えば、*notification-templates* など。

    例えば、Ubuntu システムでは以下のコマンドを実行します。
	
	```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	以下を実行して、このディレクトリーに移動します。
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
3. システムのローカル・ディレクトリーに通知テンプレートを作成します。

    例えば、以下のように vi エディターを使用して、E メール通知テンプレート用のファイル **email-template.json** を作成します。 
	
	```
	{
    "name": "email_template",
    "type": "Email",
    "description" : "Send email to manager of department X when ....",
    "detail": "xxx@yyy"
    }
	```
	{: codeblock}
	


## 通知の作成
{: #create}

通知を作成するには、以下の手順を実行します。

1. 通知ファイルを作成します。

    1. 通知テンプレートを使用してファイルを作成します。詳細情報については、[通知テンプレートの作成](#template)を参照してください。
	
	2. 以下を実行して、ファイルを更新します。
	
	    * *name* フィールドに固有の名前を入力します。このフィールドの値は、後で通知を更新および削除したり、通知の詳細を表示したりするために使用されます。
		* 説明を入力します。
		* 有効な E メール・アドレス、URL エンドポイント、または PagerDuty キーを入力します。
		
	3. ファイルを保存します。例えば、{{site.data.keyword.Bluemix_notm}} スペース内で実行されている照会に対して定義する通知には、*s-* のような接頭部を使用します。
	
	    **ヒント:** *~/cloud-monitoring/notifications/* ディレクトリーに通知ファイルを作成して、{{site.data.keyword.monitoringshort_notm}} サービスのリソースをグループ化します。 
	
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
	
5. 以下の cURL コマンドを実行して、通知を作成します。

    ```
	curl -XPOST -d @Notification_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: codeblock}
	
	詳細は次のとおりです。
	
	* Notification_File は、通知を定義する JSON ファイルです。
	
	* *X-Auth-User-Token* は、{{site.data.keyword.Bluemix_notm}} UAA トークン、IAM トークン、または API キーを渡すパラメーターです。
	
	* *Auth_Type* は、トークンまたは API キーのタイプを定義する接頭部です。以下のリストに、有効値 *apikey*、*iam*、または *uaa* の概要を示します。詳細は次のとおりです。

  * *apikey* は、トークンが API キーであることを識別します。
		* *iam* は、指定されたトークンが IAM 生成トークンであることを識別します。
		* *uaa* は、指定されたトークンが UAA 生成トークンであることを識別します。
	
	* *X-Auth-Scope-Id* は、スペースの GUID を渡すパラメーターです。この GUID には、スペースを識別するための *s-* の接頭部が付いている必要があります。UAA トークン認証を使用する場合、このヘッダーは必須です。IAM 認証トークンを使用する場合、このヘッダーはオプションで、このトークンにバインドされているドメイン情報が使用されます。
	
	* Token は、UAA トークン、IAM トークン、または API キーです。
	
	* Space は、スペースの GUID です。これは、UAA トークンを使用する場合にのみ必要です。
	
	以下に例を示します。 	
	
	```
	curl -XPOST -d @s-email-dep-A.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.{DomainName}/v1/alert/notification
	```
	{: screen}

## 通知の削除
{:#delete}

通知を削除するには、以下の手順を実行します。

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
	
4. 以下の cURL コマンドを実行して、通知を削除します。

    ```
	curl -XDELETE --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification/Notification_Name
	```
	{: codeblock}
	
	詳細は次のとおりです。
	
	* *Auth_Type* は、トークンまたは API キーのタイプを定義する接頭部です。以下のリストに、有効値 *apikey*、*iam*、または *uaa* の概要を示します。詳細は次のとおりです。

  * *apikey* は、トークンが API キーであることを識別します。
		* *iam* は、指定されたトークンが IAM 生成トークンであることを識別します。
		* *uaa* は、指定されたトークンが UAA 生成トークンであることを識別します。
	
	* *X-Auth-User-Token* は、{{site.data.keyword.Bluemix_notm}} UAA トークン、IAM トークン、または API キーを渡すパラメーターです。トークンまたは API キーには、*apikey*、*iam*、または *uaa* のいずれかの値が接頭部として付いている必要があります。詳細は次のとおりです。

  * *apikey* は、トークンが API キーであることを識別します。
		* *iam* は、指定されたトークンが IAM 生成トークンであることを識別します。
		* *uaa* は、指定されたトークンが UAA 生成トークンであることを識別します。
	
	* Token は、UAA トークン、IAM トークン、または API キーです。
	
	* Space は、スペースの GUID です。これは、UAA トークンを使用する場合にのみ必要です。
	
	* Notification_Name は、通知の名前です。つまり、通知のプロパティーをリストした時の *name* フィールドの値です。
	


## すべての通知のリスト
{: #list}

すべての通知をリストするには、以下の手順を実行します。

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
	
4. 以下の cURL コマンドを実行して、すべての通知をリストします。

    ```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notifications
	```
	{: codeblock}
	
	詳細は次のとおりです。
	
	* *Auth_Type* は、トークンまたは API キーのタイプを定義する接頭部です。以下のリストに、有効値 *apikey*、*iam*、または *uaa* の概要を示します。詳細は次のとおりです。

  * *apikey* は、トークンが API キーであることを識別します。
		* *iam* は、指定されたトークンが IAM 生成トークンであることを識別します。
		* *uaa* は、指定されたトークンが UAA 生成トークンであることを識別します。
	
	* *X-Auth-User-Token* は、{{site.data.keyword.Bluemix_notm}} UAA トークン、IAM トークン、または API キーを渡すパラメーターです。トークンまたは API キーには、*apikey*、*iam*、または *uaa* のいずれかの値が接頭部として付いている必要があります。詳細は次のとおりです。

  * *apikey* は、トークンが API キーであることを識別します。
		* *iam* は、指定されたトークンが IAM 生成トークンであることを識別します。
		* *uaa* は、指定されたトークンが UAA 生成トークンであることを識別します。
	
	* Token は、UAA トークン、IAM トークン、または API キーです。
	
	* Space は、スペースの GUID です。これは、UAA トークンを使用する場合にのみ必要です。


			

## 通知の詳細の表示
{: #show}


通知に関する情報を表示するには、以下の手順を実行します。

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
	
4. 以下の cURL コマンドを実行して、通知の詳細を表示します。

    ```
	curl -XGET --header "X-Auth-User-Token:uaa ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification/Notification_Name
	```
	{: codeblock}
	
	詳細は次のとおりです。
	
	* *X-Auth-User-Token* は、{{site.data.keyword.Bluemix_notm}} UAA トークン、IAM トークン、または API キーを渡すパラメーターです。トークンまたは API キーには、*apikey*、*iam*、または *uaa* のいずれかの値が接頭部として付いている必要があります。詳細は次のとおりです。

  * *apikey* は、トークンが API キーであることを識別します。
		* *iam* は、指定されたトークンが IAM 生成トークンであることを識別します。
		* *uaa* は、指定されたトークンが UAA 生成トークンであることを識別します。
	
	* Token は、UAA トークン、IAM トークン、または API キーです。
	
	* Space は、スペースの GUID です。これは、UAA トークンを使用する場合にのみ必要です。
	
	* Notification_Name は、通知の名前です。つまり、通知のプロパティーをリストした時の *name* フィールドの値です。
	


## 通知のテスト
{: #test}	
			
通知をテストするには、以下の手順を実行します。

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
	
4. 以下の cURL コマンドを実行して、通知をテストします。

    ```
	curl -XPOST --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification/test/Notification_Name
	```
	{: codeblock}
	
	詳細は次のとおりです。
	
	* *Auth_Type* は、トークンまたは API キーのタイプを定義する接頭部です。以下のリストに、有効値 *apikey*、*iam*、または *uaa* の概要を示します。詳細は次のとおりです。

  * *apikey* は、トークンが API キーであることを識別します。
		* *iam* は、指定されたトークンが IAM 生成トークンであることを識別します。
		* *uaa* は、指定されたトークンが UAA 生成トークンであることを識別します。
	
	* *X-Auth-User-Token* は、{{site.data.keyword.Bluemix_notm}} UAA トークン、IAM トークン、または API キーを渡すパラメーターです。トークンまたは API キーには、*apikey*、*iam*、または *uaa* のいずれかの値が接頭部として付いている必要があります。詳細は次のとおりです。

  * *apikey* は、トークンが API キーであることを識別します。
		* *iam* は、指定されたトークンが IAM 生成トークンであることを識別します。
		* *uaa* は、指定されたトークンが UAA 生成トークンであることを識別します。
	
	* Token は、UAA トークン、IAM トークン、または API キーです。
	
	* Space は、スペースの GUID です。これは、UAA トークンを使用する場合にのみ必要です。
	
	* Notification_Name は、通知の名前です。つまり、通知のプロパティーをリストした時の *name* フィールドの値です。
	


## 通知の更新
{: #update}

通知を更新するには、以下の手順を実行します。

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
	
4. 以下の cURL コマンドを実行して、通知を更新します。

    ```
	curl -XPUT -d @Notification_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: codeblock}
	
	詳細は次のとおりです。
	
	* Notification_File は、通知を定義する JSON ファイルです。
	
	* *Auth_Type* は、トークンまたは API キーのタイプを定義する接頭部です。以下のリストに、有効値 *apikey*、*iam*、または *uaa* の概要を示します。詳細は次のとおりです。

  * *apikey* は、トークンが API キーであることを識別します。
		* *iam* は、指定されたトークンが IAM 生成トークンであることを識別します。
		* *uaa* は、指定されたトークンが UAA 生成トークンであることを識別します。
	
	* *X-Auth-User-Token* は、bluemix UAA トークン、IAM トークン、API キーを渡すパラメーターです。トークンまたは API キーには、*apikey*、*iam*、または *uaa* のいずれかの値が接頭部として付いている必要があります。詳細は次のとおりです。

  * *apikey* は、トークンが API キーであることを識別します。
		* *iam* は、指定されたトークンが IAM 生成トークンであることを識別します。
		* *uaa* は、指定されたトークンが UAA 生成トークンであることを識別します。
	
	* Token は、UAA トークン、IAM トークン、または API キーです。
	
	* Space は、スペースの GUID です。これは、UAA トークンを使用する場合にのみ必要です。


			

