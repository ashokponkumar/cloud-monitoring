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


# Segurança
{: #security_ov}

É possível usar métodos de autenticação diferentes para enviar métricas para o serviço {{site.data.keyword.monitoringshort}}. As permissões para acessar, modificar e excluir métricas são controladas usando funções.
{:shortdesc}

   
## Modelos de Autenticação
{: #auth}

Para acessar as métricas que são armazenadas no serviço do {{site.data.keyword.monitoringshort}} para um espaço do {{site.data.keyword.Bluemix_notm}}, você requer um token de autenticação ou uma chave API. 

O serviço {{site.data.keyword.monitoringshort}} suporta os modelos de autenticação a seguir:

* [{{site.data.keyword.Bluemix_notm}} UAA autenticação](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_uaa)
* [{{site.data.keyword.Bluemix_notm}} IAM Autenticação](/docs/services/cloud-monitoring/security/auth_iam.html#auth_iam)

Um token do UAA e um token do IAM expiram após um período de tempo. A chave API não expira. 

Se a chave API estiver comprometida, será possível revogá-la excluindo-a. Em seguida, será possível será possível uma nova. Para obter mais informações, veja [Revogando uma chave API usando a UI do Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#revoke_ui). 

O modelo de autenticação do IAM oferece recursos de gerenciamento de UI, CLI ou API. É possível usar a CLI apenas para gerenciar tokens do UAA.

A tabela a seguir lista os modelos de autenticação que são suportados para cada tipo de domínio:

<table>
  <caption>Tabela 1. Modelos de autenticação suportadas para cada domínio</caption>
  <tr>
    <th></th>
	<th align="right">Conta</th>
    <th align="right">Organização</th>
    <th align="right">Espaço</th>	
  </tr>
  <tr>
    <th align="left">UAA</th>
	<td align="center">Não</td>
	<td align="center">Sim</td>
	<td align="center">Sim</td>
  </tr>
  <tr>
    <th align="left">IAM</th>
	<td align="center">Sim</td>
	<td align="center">Sim</td>
	<td align="center">Sim</td>
  </tr>
</table>

O serviço {{site.data.keyword.monitoringshort}} usa o recurso de controle de acesso do IAM para controlar quais ações ou operações um usuário ou serviço pode executar em relação ao domínio. Por exemplo, é possível definir uma política do IAM para permitir que um usuário envie e crie painéis em um domínio, mas não para recuperar as métricas do domínio.



## Funções do UAA do Bluemix
{: #bmx_roles}

A tabela a seguir lista os privilégios de cada função do {{site.data.keyword.Bluemix_notm}} para trabalhar com o serviço do {{site.data.keyword.monitoringshort}}:

<table>
  <caption>Tabela 2. As funções e privilégios do {{site.data.keyword.Bluemix_notm}} para trabalhar com o serviço do {{site.data.keyword.monitoringshort}}.</caption>
  <tr>
    <th>Atribuição</th>
	<th>Domain</th>
	<th>Privilégios de acesso</th>
  </tr>
  <tr>
    <td>Manager</td>
	<td>Organização <br>Espaço</td>
	<td>Todas as APIs RESTful</td>
  </tr>
  <tr>
    <td>Developer</td>
	<td>Espaço</td>
	<td>Todas as APIs RESTful</td>
  </tr>
  <tr>
    <td>Auditor</td>
	<td>Organização <br>Espaço</td>
	<td>Apenas APIs RESTful que executam operações somente leitura, como as métricas de consulta.</td>
  </tr>
</table>


## Funções do IAM do Bluemix
{: #iam_roles}

A tabela a seguir lista o relacionamento entre a API, uma ação de serviço e uma função do IAM que é usada pelo serviço {{site.data.keyword.monitoringshort}}.

<table>
  <caption>Tabela 3. Relacionamento entre a API, uma ação de serviço e uma função do IAM. </caption>
  <tr>
    <th>API</th>
	<th>Ação</th>
	<th>Função IAM</th>
	<th>Descrição</th>
  </tr>
  <tr>
    <td>POST /v1/metrics</td>
    <td>Domain.write</td>
	<td>Administrador, Editor, Operador</td>
	<td>Enviar métricas para o domínio</td>
  </tr>
  <tr>
    <td>GET /v1/metrics</td>
    <td>Domain.render</td>
	<td>Administrador, Editor, Visualizador</td>
	<td>Recuperar / consulta de métricas</td>
  </tr>
  <tr>
    <td>GET /v1/metrics/list</td>
    <td>Domain.find</td>
	<td>Administrador, Editor</td>
	<td>Procure métricas no domínio</td>
  </tr>
</table>

## Obtendo o token de segurança ou a chave API
{: #get_token}

Use o modelo do UAA do {{site.data.keyword.Bluemix_notm}} para obter um token de autenticação que possa ser usado para acessar os dados que estiverem armazenados no serviço do {{site.data.keyword.monitoringshort}} para um espaço no {{site.data.keyword.Bluemix_notm}}. É possível obter o token de autenticação usando a CLI do {{site.data.keyword.Bluemix_notm}} ou usando a API de REST de `Login`. Para obter mais informações, veja [Obtendo o token do UAA usando a CLI do {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_cli) e [Obtendo o token do UAA usando a API](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_api).

Use o modelo do IAM do {{site.data.keyword.Bluemix_notm}} para obter um token de autenticação que possa ser usado para acessar os dados que estiverem armazenados no serviço do {{site.data.keyword.monitoringshort}} ou para obter uma chave API. O token tem um tempo de expiração. A chave API não expira. Para obter mais informações, veja [Obtendo o token do IAM usando a CLI do {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli), [Gerando uma chave API do IAM usando a CLI do {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli) ou [Gerando uma chave API do IAM usando a UI do {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_ui).



