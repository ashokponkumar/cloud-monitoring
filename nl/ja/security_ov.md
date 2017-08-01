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


# セキュリティー
{: #security_ov}

さまざまな認証方式を使用して、{{site.data.keyword.monitoringshort}} サービスにメトリックを送信することができます。メトリックをアクセス、変更、および削除する権限は、役割を使用して制御されます。
{:shortdesc}

   
## 認証モデル
{: #auth}

{{site.data.keyword.Bluemix_notm}} スペースの {{site.data.keyword.monitoringshort}} サービスに保管されているメトリックにアクセスするには、認証トークンまたは API キーが必要です。 

{{site.data.keyword.monitoringshort}} サービスでは、以下の認証モデルがサポートされます。

* [{{site.data.keyword.Bluemix_notm}} UAA 認証](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_uaa)
* [{{site.data.keyword.Bluemix_notm}} IAM 認証](/docs/services/cloud-monitoring/security/auth_iam.html#auth_iam)

UAA トークンおよび IAM トークンは、一定期間後に期限切れになります。API キーには有効期限切れはありません。
 

API キーが漏えいした場合は、それを削除することによって取り消すことができます。その後、新しい API キーを再作成できます。詳しくは、[Bluemix UI を使用した API キーの取り消し](/docs/services/cloud-monitoring/security/auth_iam.html#revoke_ui)を参照してください。 

IAM 認証モデルは、UI、CLI、または API の管理機能を提供します。UAA トークンを管理するには、CLI のみを使用できます。

以下の表に、各ドメイン・タイプでサポートされている認証モデルをリストします。

<table>
  <caption>表 1. 各ドメインでサポートされる認証モデル</caption>
  <tr>
    <th></th>
	<th align="right">アカウント</th>
    <th align="right">組織</th>
    <th align="right">スペース</th>	
  </tr>
  <tr>
    <th align="left">UAA</th>
	<td align="center">いいえ</td>
	<td align="center">はい</td>
	<td align="center">はい</td>
  </tr>
  <tr>
    <th align="left">IAM</th>
	<td align="center">はい</td>
	<td align="center">はい</td>
	<td align="center">はい</td>
  </tr>
</table>

{{site.data.keyword.monitoringshort}} サービスは、IAM アクセス制御機能を使用して、ユーザーまたはサービスがドメインに対して実行できるアクションまたは操作を管理します。例えば、ユーザーがドメインに対してダッシュボードを送信および作成することは許可するが、ドメインからメトリックを取り出すことは許可しないように IAM ポリシーを定義することができます。



## Bluemix UAA 役割
{: #bmx_roles}

以下の表に、{{site.data.keyword.monitoringshort}} サービスを処理するための {{site.data.keyword.Bluemix_notm}} の各役割をリストします。

<table>
  <caption>表 2. {{site.data.keyword.monitoringshort}} サービスを処理するための {{site.data.keyword.Bluemix_notm}} の役割および特権</caption>
  <tr>
    <th>役割</th>
	<th>ドメイン</th>
	<th>アクセス権</th>
  </tr>
  <tr>
    <td>管理者</td>
	<td>組織の<br>スペース</td>
	<td>すべての RESTful API</td>
  </tr>
  <tr>
    <td>開発者</td>
	<td>スペース</td>
	<td>すべての RESTful API</td>
  </tr>
  <tr>
    <td>監査員</td>
	<td>組織の<br>スペース</td>
	<td>メトリックの照会などの読み取り専用操作を実行する RESTful API のみ。</td>
  </tr>
</table>


## Bluemix IAM 役割
{: #iam_roles}

以下の表に、API、サービス・アクション、および {{site.data.keyword.monitoringshort}} サービスによって使用される IAM 役割の間の関係をリストします。

<table>
  <caption>表 3. API、サービス・アクション、および IAM 役割の間の関係。</caption>
  <tr>
    <th>API</th>
	<th>アクション</th>
	<th>IAM 役割</th>
	<th>説明</th>
  </tr>
  <tr>
    <td>POST /v1/metrics</td>
    <td>domain.write</td>
	<td>管理者、エディター、オペレーター</td>
	<td>ドメインにメトリックを送信する</td>
  </tr>
  <tr>
    <td>GET /v1/metrics</td>
    <td>domain.render</td>
	<td>管理者、エディター、ビューアー</td>
	<td>メトリックを取得/照会する</td>
  </tr>
  <tr>
    <td>GET /v1/metrics/list</td>
    <td>domain.find</td>
	<td>管理者、エディター</td>
	<td>ドメイン内でメトリックを検索する</td>
  </tr>
</table>

## セキュリティー・トークンまたは API キーの取得
{: #get_token}

{{site.data.keyword.Bluemix_notm}} UAA モデルを使用して、{{site.data.keyword.Bluemix_notm}} 内のスペース用に、{{site.data.keyword.monitoringshort}} サービスに保管されているデータにアクセスするために使用できる認証トークンを取得します。この認証トークンは、{{site.data.keyword.Bluemix_notm}} CLI を使用するか、`Login` REST API を使用して取得できます。
詳しくは、[{{site.data.keyword.Bluemix_notm}} CLI を使用した UAA トークンの取得](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_cli)および [API を使用した UAA トークンの取得](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_api)を参照してください。

{{site.data.keyword.Bluemix_notm}} IAM モデルを使用して、{{site.data.keyword.monitoringshort}} サービスに保管されているデータへのアクセスに使用できる認証トークンを取得したり、API キーを取得したりします。トークンには有効期限があります。API キーには有効期限切れはありません。
詳しくは、[{{site.data.keyword.Bluemix_notm}} CLI を使用した IAM トークンの取得](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli)、[{{site.data.keyword.Bluemix_notm}} CLI を使用した IAM API キーの生成](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli)、または[{{site.data.keyword.Bluemix_notm}} UI を使用した IAM API キーの生成](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_ui)を参照してください。



