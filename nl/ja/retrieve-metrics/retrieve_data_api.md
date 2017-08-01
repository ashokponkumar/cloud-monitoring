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


# Monitoring サービスからのデータの取得
{: #retrieve_data_api}

[Metrics API](https://console.bluemix.net/apidocs/927-ibm-cloud-monitoring-rest-api?&language=node#introduction){: new_window} を使用して、{{site.data.keyword.monitoringshort}} サービスからメトリックを取得することができます。メトリックは、{{site.data.keyword.Bluemix_notm}} のスペースから取得できます。
{:shortdesc}


取得する予定のデータ・セットを定義する場合、以下の情報を考慮してください。 

* データの取得元の {{site.data.keyword.Bluemix_notm}} スペースを設定する必要があります。

* {{site.data.keyword.monitoringshort}} サービスで作業するためのトークンまたは API キーを入力する必要があります。 

* 1 つ以上のメトリックのパスを指定する必要があります。詳しくは、[メトリックの定義](#metrics)を参照してください。

* オプションで、カスタム期間を指定できます。デフォルトでは、期間を指定しない場合、取得するデータは直近の 24 時間に対応するデータです。詳しくは、[期間の構成](#time)を参照してください。

**注:** 要求ごとに最大 5 個のターゲットを取得できます。


## メトリックの定義
{: #metrics}

メトリックのデータを取得するには、メトリック・ツリーのルートからメトリックまでのメトリックのパスを指定する必要があり、各ステップをピリオド (.) で区切る必要があります。

パスは、**Target** パラメーターで指定されます。要求ごとに最大 5 個のターゲットを指定できます。  

オプションで、関数をメトリックに適用できます。サポートされる関数について詳しくは、[Graphite 0.9.15 functions ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](http://graphite.readthedocs.io/en/latest/functions.html "外部リンク・アイコン") を参照してください。

ワイルドカードを使用してパスを定義できます。ワイルドカードを使用して、単一のパスで複数のメトリックを識別できます。 

* **アスタリスク**: ゼロ個以上の文字と一致させるには、アスタリスク (*) を使用できます。 

    単一パス・エレメント内に複数のアスタリスクを含めることができます。例えば `servers.sp*aeg*r.cpu.total.*` と指定できます。この例では、パターンに一致するすべてのサーバーの合計 CPU に関するデータが返されます。

* **文字のリストまたは範囲**: 大括弧内で文字を定義して、パス・ストリング内の単一の文字位置を指定できます。 

    2 つの文字をハイフン (-) で区切って指定することにより、文字の範囲 (両端を含む) を設定できます。それらの 2 つの文字の間の任意の文字が一致します。 
	
	大括弧内に 1 つ以上の文字範囲を定義できます。 
	
	1 組の大括弧内の文字を、範囲として読み取ることができない場合、それらの文字は値のリストとして扱われます。 
	
	ハイフン (-) は、それを大括弧内の先頭または末尾に置くことにより、値のリスト内の 1 文字として含めることができます。

* **値リスト**: 中括弧内にコンマで区切った値を設定して、値リストを定義できます。 

    例えば、`servers.myserver.cpu.total.user` および `servers.myserver.cpu.total.system}` というパスのメトリックをダウンロードする場合、`servers.myserver.cpu.total.{user,system}` という単一パスを設定することで、両方のタイプに対応できます。



## 期間の設定
{: #time}

オプションで、相対時間または絶対時間を使用して、期間を指定できます。照会パラメーター **From** および **Until** を定義する必要があります。  

* 期間の開始時刻が省略されている場合、デフォルト値は 24 時間前です。 

* 期間の終了時刻が省略されている場合、デフォルト値は現在時刻 *now* です。

**相対時間**の場合は常に、先頭に負符号 (-) があり、末尾に時間単位が付加されます。 

以下の表に、使用できるさまざまな相対時間の省略形を示します。


| 省略形       | 説明|
|--------------|-------------|
| s            | 秒          |
| min          | 分          |
| h            | 時間        |
| d            | 日          |
| w            | 週          |
| mon          | 月          |
| now          | 現在時刻    |


**絶対時間** を定義する場合、`HH:MM_YYMMDD`、`YYYYMMDD`、`MM/DD/YY` のうちの任意の形式を使用できます。

| 省略形       | 説明|
|--------------|-------------|
| YYYY         | 4 桁の西暦年|
| MM           | 2 桁の月 (01=1 月など) |
| DD           | 2 桁の日 (01 から 31) |
| hh           | 2 桁の時 (00 から 23) |
| mm           | 2 桁の分 (00 から 59) |
| ss           | 2 桁の秒 (00 から 59) |

**例**

以下の表に、*from* および *until* パラメーターを設定することによってさまざまな期間を設定する方法の例を示します。

<table>
  <caption>表 1. *from* および *until* パラメーターを設定することによってさまざまな期間を設定する方法の例</caption>
  <tr>
    <th>例</th>
	<th>説明</th>
  </tr>
  <tr>
    <td>&from=monday</td>
	<td>最後の月曜から現在までのデータを含む期間</td>
  </tr>
  <tr>
    <td>&from=20171101&until=20171130</td>
	<td>1 カ月 (例えば 11 月) に設定された期間</td>
  </tr>
  <tr>
    <td>&from=02:00_20170701&until=14:00_20170701</td>
	<td>24 時制で、例えば 2017 年 7 月 1 日の 02:00 から 14:00 までのデータを表示</td>
  </tr>
  <tr>
    <td>&from=-8d&until=-7d</td>
	<td>例えば先週の同じ曜日を表示</td>
  </tr>
  
</table>


## スペースの設定
{: #domain}

{{site.data.keyword.Bluemix_notm}} UAA 認証モデルまたは {{site.data.keyword.Bluemix_notm}} IAM 認証モデルを使用して {{site.data.keyword.monitoringshort}} サービスで認証する場合、ヘッダー・フィールド **X-Auth-Scope-Id** が必須であり、スペース GUID に設定する必要があります。この GUID には、スペースを識別するための *s-* の接頭部が付いている必要があります。



## 許可トークンまたは API キーの設定
{: #security}
  
認証に使用される UAA トークン、IAM トークン、または API キーを定義するように **X-Auth-User-Token** フィールドを設定する必要があります。 

トークンまたは API キーには、`apikey`、`iam`、または `uaa` のいずれかの値が接頭部として付いている必要があります。 

ここで、 

* *apikey* は、トークンが API キーであることを識別します。
* *iam* は、指定されたトークンが IAM 生成トークンであることを識別します。
* *uaa* は、指定されたトークンが UAA 生成トークンであることを識別します。

例えば、API キーの場合、`X-Auth-User-Token: apikey SomeIAMGeneratedKey` というヘッダーを設定できます。


## UAA を使用したスペースからのメトリックの取得
{: #uaa}

メトリックを {{site.data.keyword.Bluemix_notm}} スペースから取得するには、以下の手順を実行します。

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
	export Token="eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8"
	```
	{: screen}
	
	**注:** このトークンは *Bearer* を除外します。
		
3. スペース GUID を取得します。この GUID には、スペースを識別するための *s-* の接頭部が付いている必要があります。

    以下のコマンドを実行します。
	
	```
	cf space SpaceName --guid
	```
	{: codeblock}
	
	ここで、*SpaceName* は、通知を定義するスペースの名前であり、*s-* という接頭部を含んでいます。
	
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
	curl -XGET --header "X-Auth-User-Token:uaa ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics?from=Start_Time&until=End_Time&target=string
	```
	{: codeblock}

	詳細は次のとおりです。
	
	* *X-Auth-User-Token* は、{{site.data.keyword.Bluemix_notm}} の UAA トークンを渡すパラメーターです。
	
	* *uaa* は、指定されたトークンが UAA 生成トークンであることを示す接頭部です。
	
	* *X-Auth-Scope-Id* は、スペースの GUID を渡すパラメーターです。この GUID には、スペースを識別するための *s-* の接頭部が付いている必要があります。UAA 認証を使用する場合、このヘッダーは必須です。IAM 認証トークンを使用する場合、このヘッダーはオプションで、このトークンにバインドされているドメイン情報が使用されます。
	
	* *Token* は、UAA トークンを表します。
	
	* *Space* は、スペースの GUID を表します。これは、UAA トークンを使用する場合にのみ必要です。
	
	* *Start_Time* は、要求の開始を設定する相対時間または絶対時間を定義します。デフォルトは 24 時間前 (「-24h」) です。*End_Time* は、要求の終了を設定する相対時間または絶対時間を定義します。デフォルトは現在時刻 (「now」) です。詳しくは、 [期間の設定](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#time) を参照してください。
	
	* *Path*  は、1 つまたは複数のメトリックを識別します。オプションで、関数を各メトリックに適用できます。パスではワイルドカードもサポートします。これにより、単一のパスで複数のメトリックを識別できます。詳しくは、[メトリックの定義](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#metrics) を参照してください。
	
	
	
## IAM または API キーを使用したスペースからのメトリックの取得
{: #iam}

IAM または API キーを使用してメトリックを {{site.data.keyword.Bluemix_notm}} スペースから取得するには、以下の手順を実行します。

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
	
	ここで、*SpaceName* は、通知を定義するスペースの名前であり、*s-* という接頭部を含んでいます。
	
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
	
5. cURL コマンドを実行してメトリックを取得します。

    ```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics?from=Start_Time&until=End_Time&target=string
	```
	{: codeblock}

	詳細は次のとおりです。
	
	* *X-Auth-User-Token* は、{{site.data.keyword.Bluemix_notm}} の AIM トークンまたは API キーを渡すパラメーターです。
	
	* *Auth_Type* は、トークンまたは API キーのタイプを定義する接頭部です。* *apikey* は、トークンが API キーであることを識別します。
		* *iam* は、指定されたトークンが IAM 生成トークンであることを識別します。
		* *X-Auth-Scope-Id* は、スペースの GUID を渡すパラメーターです。この GUID には、スペースを識別するための *s-* の接頭部が付いている必要があります。UAA 認証を使用する場合、このヘッダーは必須です。IAM 認証トークンを使用する場合、このヘッダーはオプションで、このトークンにバインドされているドメイン情報が使用されます。
	
	* *Token* は、UAA トークンを表します。
	
	* *Space* は、スペースの GUID を表します。これは、UAA トークンを使用する場合にのみ必要です。
	
	* *Start_Time* は、要求の開始を設定する相対時間または絶対時間を定義します。デフォルトは 24 時間前 (「-24h」) です。*End_Time* は、要求の終了を設定する相対時間または絶対時間を定義します。デフォルトは現在時刻 (「now」) です。詳しくは、 [期間の設定](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#time) を参照してください。
	
	* *Path*  は、1 つまたは複数のメトリックを識別します。オプションで、関数を各メトリックに適用できます。パスではワイルドカードもサポートします。これにより、単一のパスで複数のメトリックを識別できます。詳しくは、[メトリックの定義](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#metrics) を参照してください。
	
	
	





