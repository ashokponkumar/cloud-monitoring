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


# Enviando dados para o serviço Monitoring
{: #send_data_api}

É possível enviar métricas para o serviço do {{site.data.keyword.monitoringshort}} usando a API de Métricas. É possível enviar métricas para um espaço do {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}


Para os contêineres do Docker do {{site.data.keyword.Bluemix_notm}}, são coletadas métricas do sistema básico automaticamente. Para os aplicativos Cloud Foundry e os apps em execução em uma Máquina virtual (VM), as métricas devem ser enviadas diretamente do app usando [a API de Métricas](https://console.bluemix.net/apidocs/927-ibm-cloud-monitoring-rest-api?&language=node#introduction){: new_window}. 



## Enviando métricas para um espaço usando o UAA
{: #uaa}

Para enviar métricas para um espaço do {{site.data.keyword.Bluemix_notm}}, conclua as etapas a seguir:

1. Efetue login em uma região, uma organização e um espaço do {{site.data.keyword.Bluemix_notm}}. Execute o comando:

    Por exemplo, para efetuar login na região sul dos EUA, execute o comando a seguir:
	
	```
    cf login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Siga as instruções. Insira suas credenciais do {{site.data.keyword.Bluemix_notm}}, selecione uma organização e um espaço.

2. Obtenha o token de autenticação UAA.

    Para obter mais informações sobre autenticação do UAA, consulte [Obtendo o token UAA usando a CLI do Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) ou [Obtendo o token do UAA usando a API de REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

	Por exemplo, para usar o token do UAA, execute o comando a seguir:

    ```
	cf oauth-token
	```
	{: codeblock}
	
	O resultado desse comando é o seguinte:
	
	```
	bearer eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8
	```
	{: screen}
	
	Em seguida, exporte a variável *Token*. Por exemplo:
	
	```
	export Token="eyJhbGciOiJI....cGlxryWS8"
	```
	{: screen}
	
	**Nota:** O token exclui *Bearer*.
		
3. Obtenha o GUID do espaço. O GUID deve ser prefixado com *s-* para identificar um espaço.

    Execute o comando a seguir:
	
	```
	cf space SpaceName --guid
	```
	{: codeblock}
	
	Em que *SpaceName* é o nome do espaço em que você definirá uma notificação.
	
	Por exemplo, para obter o GUID de um espaço com o nome *dev*, execute o comando a seguir:
	
	```
	cf space dev --guid
	```
	{: screen}
	
	O resultado desse comando é o seguinte:
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	Em seguida, exporte a variável *Space*. Por exemplo:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. Execute o comando cURL a seguir para enviar métricas:

    ```
	curl -XPOST -d @Metric_File --header "X-Auth-User-Token:uaa ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics
	```
	{: codeblock}
	
	Em que
	
	* O Metrics_File representa o arquivo JSON que inclui a definição de métricas que são enviadas para o serviço {{site.data.keyword.monitoringshort}}.
	
	* O *X-Auth-User-Token* é um parâmetro que passa o token do UAA do {{site.data.keyword.Bluemix_notm}}.
	
	* *Auth_Type* é o prefixo que define o tipo de token. *uaa* identifica que o token especificado é um token UAA gerado.
	
	* *X-Auth-Scope-Id* é um parâmetro que transmite o GUID de espaço. O GUID deve ser prefixado com *s-* para identificar um espaço.
	
	* Token representa o token UAA.
	
	* O espaço representa o GUID do espaço. 
	
	Por exemplo, é possível executar o comando a seguir para enviar duas métricas, `myhost.cpu.idle` e `myapp.login.attempts`, para o serviço {{site.data.keyword.monitoringshort}}:
	
	```
	curl -XPOST @metric.json --header "X-Auth-User-Token:uaa ${Token}" https://metrics.ng.bluemix.net/v1/metrics
	```
	{: screen}
	
	O arquivo de amostra *metric.json* é definido conforme a seguir:

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

 
## Enviando métricas para um espaço usando o IAM ou uma chave API
{: #iam}

Para enviar métricas para um espaço do {{site.data.keyword.Bluemix_notm}}, conclua as etapas a seguir:

1. Efetue login em uma região, uma organização e um espaço do {{site.data.keyword.Bluemix_notm}}. Execute o comando:

    Por exemplo, para efetuar login na região sul dos EUA, execute o comando a seguir:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Siga as instruções. Insira suas credenciais do {{site.data.keyword.Bluemix_notm}}, selecione uma organização e um espaço.

2. Obtenha o token de autenticação ou a chave API.

    Para obter informações sobre a autenticação do IAM, consulte [Obtendo o token do IAM usando a CLI do Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_cli) ou [Gerando uma chave API do IAM usando a CLI do Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).

	Por exemplo, para usar o token IAM, execute o comando a seguir:

    ```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	O resultado desse comando é o seguinte:
	
	```
	IAM token:  Bearer djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ
    UAA token:  Bearer eyJhbGciOiJIUz..Ky6vagp3k_QcIcKJ-td83qXhO5Uze43KcplG6PzcGs8
	```
	{: screen}
	
	Em seguida, exporte a variável *Token*. Por exemplo:
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Nota:** O token exclui *Bearer*.
		
3. Obtenha o GUID do espaço.

    Para obter o GUID do espaço, consulte [Como obtenho o GUID de um espaço](/docs/service/cloud-monitoring/qa/cli_qa.html#space_guid). O GUID deve ser prefixado com *s-* para identificar um espaço.
    
    Por exemplo, execute o comando a seguir:
	
	```
	bx iam space NAME --guid
	```
	{: codeblock}
	
	Em que *SpaceName* é o nome do espaço em que você definirá uma notificação.
	
	Por exemplo, para obter o GUID de um espaço com o nome *dev*, execute o comando a seguir:
	
	```
	bx iam space dev --guid
	```
	{: screen}
	
	O resultado desse comando é o seguinte:
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	Em seguida, exporte a variável *Space*. Por exemplo:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. Execute um comando cURL para enviar métricas.

    ```
	curl -XPOST -d @Metric_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics
	```
	{: codeblock}
	
	Em que
	
	* O Metrics_File representa o arquivo JSON que inclui a definição de métricas que são enviadas para o serviço {{site.data.keyword.monitoringshort}}.
	
	* O *X-Auth-User-Token* é um parâmetro que transmite o token do IAM ou a chave API do {{site.data.keyword.Bluemix_notm}}.
	
	* *Auth_Type* é o prefixo que define o tipo de token ou a chave API. A lista a seguir descreve os valores válidos: *apikey*, *iam* ou *uaa*, em que

        * *apikey* identifica que o token é uma chave API.
		* *iam* identifica que o token especificado é um token IAM gerado.
	
	* *X-Auth-Scope-Id* é um parâmetro que transmite o GUID de espaço. O GUID deve ser prefixado com *s-* para identificar um espaço. 
	
	* Token representa o token do IAM ou a chave API.
	
	* O espaço representa o GUID do espaço. 

	
Por exemplo, é possível executar o comando a seguir para enviar duas métricas, `myhost.cpu.idle` e `myapp.login.retries`, para um espaço no serviço do {{site.data.keyword.monitoringshort}}:
	
```
curl -XPOST @metric.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/metrics
```
{: screen}
	
O arquivo de amostra *metric.json* é definido conforme a seguir:

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


















 
