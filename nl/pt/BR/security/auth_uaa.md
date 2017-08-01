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


# Autenticando usando o modelo do UAA do Bluemix
{: #auth_uaa}

Use o modelo do UAA do {{site.data.keyword.Bluemix}} para obter um token de autenticação que pode ser usado para acessar métricas que são armazenadas no
serviço {{site.data.keyword.monitoringshort}} para um espaço em um {{site.data.keyword.Bluemix_notm}}. É possível obter o token de autenticação usando a CLI do {{site.data.keyword.Bluemix_notm}} ou usando a API de REST de `Login`.
{:shortdesc}

Para acessar as métricas usando um token de autenticação do UAA, são necessárias as informações a seguir:

* Um token do UAA para acessar o serviço {{site.data.keyword.monitoringshort}} usando as APIs RESTful.
* O GUID do espaço.

		
## Obtendo um token do UAA usando a CLI do Bluemix
{: #uaa_cli}


Para obter o token de autorização, conclua as etapas a seguir:

1. Instale a CLI do {{site.data.keyword.Bluemix_notm}}.

   Para obter mais informações, veja [Instalando a CLI do {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa).
   
   Se a CLI estiver instalada, continue com a próxima etapa.
    
2. Efetue login em uma região, uma organização e um espaço do {{site.data.keyword.Bluemix_notm}}. Execute o
comando:

    ```
    cf login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    Siga as instruções. Insira suas credenciais do {{site.data.keyword.Bluemix_notm}}, selecione uma organização e um espaço.
	
3. Execute o comando `cf oauth-token` para obter o token UAA do {{site.data.keyword.Bluemix_notm}}.

    ```
	cf oauth-token
	```
	{: codeblock}
	
	A saída retorna o token UAA que se deve usar para autenticar seu ID do usuário nesse espaço e nessa organização.

4. Obtenha o GUID para o espaço da organização para o qual você obteve um token de autenticação.

   Para obter mais informações, veja [Como eu obtenho o GUID de um espaço](/docs/services/cloud-monitoring/qa/cli_qa.html#space_guid).
	
5. Exporte as variáveis a seguir: TOKEN e SPACE.

    * *TOKEN* é o token do oauth que você obteve na etapa anterior, excluindo o Bearer.
	
	* *SPACE* é o GUID do espaço que você obteve na etapa anterior.
		
	Por
exemplo,
	
	```
	Export TOKEN="eyJhbGciOiJI .... cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8 "export SPACE="667fb895-abcd-defg-aaaa-cf4587341095"
	```
	{: screen}
	
6. Execute o comando a seguir para obter o token UAA para trabalhar com o serviço {{site.data.keyword.monitoringshort}}:
 
    ```
	curl -k -X GET  --header "X-Auth-Token: ${TOKEN}"  --header "X-Auth-Project-Id: ${SPACE}"  METRICS_ENDPOINT
    ```
    {: codeblock}	
	
	em que
	* SPACE é o GUID do espaço em que o serviço está em execução.
	* TOKEN é o token UAA do {{site.data.keyword.Bluemix_notm}} que é obtido em uma etapa anterior sem o prefixo do portador.
	* METRICS_ENDPOINT é o terminal de métricas (https://metrics.ng.bluemix.net/token) para a região do {{site.data.keyword.Bluemix_notm}} na qual a organização e o espaço estão disponíveis.

	
## Obtendo o token do UAA usando a API
{: #uaa_api}

Para obter o token de autorização, execute o comando cURL a seguir:

```
Curl -XPOST -d 'user=USERNAME & passwd= PASSWORD e espaço = SPACE_NAME & organization= ORG_NAME " METRICS_ENDPOINT
```
{: codeblock}

em que

* USERNAME é um ID do {{site.data.keyword.Bluemix_notm}} para o qual você deseja obter o token de autenticação para trabalhar com o serviço {{site.data.keyword.monitoringshort}}.
* PASSWORD é a senha do ID do usuário usado para efetuar login no {{site.data.keyword.Bluemix_notm}}.
* SPACE_NAME é o nome do espaço em que as métricas são coletadas.
* ORG_NAME é o nome da organização no {{site.data.keyword.Bluemix_notm}} em que o espaço está hospedado.
* METRICS_ENDPOINT é o terminal de métricas (https://metrics.ng.bluemix.net/login) para a região do {{site.data.keyword.Bluemix_notm}} na qual a organização e o espaço estão disponíveis.

A saída é um documento JSON que contém o token UAA. Obtenha o valor para o token no campo **access-token**.

Por exemplo, um documento JSON de amostra é semelhante ao seguinte:

```
{
    "Access_token": "eyJhbGc ...",
    "logging_token": "xxxxxxxxxxxx",
    "org_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "space_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```
{: screen}

O valor do *logging_token* corresponde ao token UAA que é necessário para trabalhar com o serviço {{site.data.keyword.monitoringshort}}.
 
**Observação:**

* Se você não está usando o cURL, deve-se configurar o cabeçalho **Content-Type: application/x-www-form-urlencoded**.
* Se aparecer um código de erro *BXNMS0122E: as credenciais do usuário são inválidas*, verifique se você está usando um ID válido do {{site.data.keyword.IBM_notm}}.


