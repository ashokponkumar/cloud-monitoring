---

copyright:
  years: 2017, 2018

lastupdated: "2018-02-07"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Recuperando o histórico de um alerta usando a API Alerts
{: #retrieve_history}

Use a API Alerts para recuperar o histórico de um alerta.
{:shortdesc}


Para recuperar o histórico de um alerta usando a API Alerts, conclua as etapas a seguir:

1. Efetue login em uma região, uma organização e um espaço no {{site.data.keyword.Bluemix_notm}}. 

    Para obter mais informações, veja [Como efetuar login no {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#login).

2. Obtenha o token de segurança. É possível usar um token UAA, um token IAM ou uma chave API. Escolha um dos métodos a seguir para obter o token de segurança:
	
	* Para obter um token do UAA, consulte
[Obtendo o token do UAA usando a
CLI do {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli).
	
	* Para obter um token do IAM, consulte
[Obtendo o token do IAM usando a
CLI do {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/security/auth_iam.html#auth_iam).
	
	* Para obter uma chave API, consulte
[Recebendo uma chave
API](/docs/services/cloud-monitoring/security/auth_api_key.html#auth_api_key).
	
	Por meio do mesmo terminal em que efetuou login no {{site.data.keyword.Bluemix_notm}}, configure a variável do Token para o token.

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
	
	Em seguida, exporte a *Token* variável:
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Nota:** O token exclui *Bearer*.
	
3. Obtenha o GUID do espaço. O GUID deve ser prefixado com *s-* para identificar um espaço.

    Execute o comando a seguir:
	
	```
	bx iam space SpaceName --guid
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
	
	Em seguida, exporte a variável *Space*. **Nota:** o GUID deve ser prefixado com *s-* para identificar um espaço.
	
	Por exemplo:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. Recupere o histórico de uma regra

    * Para recuperar o histórico de uma regra por seu nome, veja [Recuperando o histórico de uma regra usando seu nome](/docs/services/cloud-monitoring/alerts/retrieve_history.html#by_name).
	* Para recuperar o histórico de uma regra para um período de tempo, veja [Recuperando o histórico de uma regra para as últimas 48 horas](/docs/services/cloud-monitoring/alerts/retrieve_history.html#by_time).
	* Para recuperar um número de entradas do histórico de uma regra, veja [Recuperando as últimas 3 entradas no histórico de uma regra](/docs/services/cloud-monitoring/alerts/retrieve_history.html#number).
	
	
## Recuperando o histórico de uma regra por nome da regra
{: #by_name}
	
Execute o comando cURL a seguir para obter o histórico de um alerta:

```
curl -XGET --header "X-Auth-User-Token: Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" METRICS_ENDPOINT/v1/alert/history?rule_name=RULE_NAME
```
{: codeblock}
	
Em que
	
* O *X-Auth-User-Token * é um parâmetro que transmite o token UAA, o token do IAM ou a chave API.
	
* *Auth_Type* é o prefixo que define o tipo de token ou a chave API. A lista a seguir descreve os valores válidos: *apikey*, *iam* ou *uaa*, em que

        * *apikey* identifica que o token é uma chave API.
	* *iam* identifica que o token especificado é um token IAM gerado.
	* *uaa* identifica que o token especificado é um token UAA gerado.
	
* *X-Auth-Scope-Id* é um parâmetro que transmite o GUID de espaço. O GUID deve ser prefixado com *s-* para identificar um espaço. Esse cabeçalho será necessário se você usar uma autenticação do token UAA. Se você usar um token de autenticação IAM, esse cabeçalho será opcional e as informações de domínio ligadas ao token serão usadas.
	
* *Token* é o token de autenticação do UAA ou IAM ou a chave API.
	
* *Space* é o GUID do espaço. 
	
* *RULE_NAME* é o nome da regra que é usada para acionar o alerta. O valor é aquele que é especificado no campo *name*.
	
* *METRICS_ENDPOINT* representa o ponto de entrada para o serviço. Cada região possui uma URL diferente. Para obter a lista de terminais por região, veja [Terminais](/docs/services/cloud-monitoring/send_retrieve_metrics_ov.html#endpoints).
	
    
Por exemplo, o histórico de regra `highNginxCPU` é o seguinte:

```
curl -XGET --header "X-Auth-User-Token: iam ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/history?rule_name=highNginxCPU
```
{: screen}

```
[
 {
 "rule": "highNginxCPU",
 "timestamp": "2017-04-17T22.23.21.000",
 "value": 99.5,
 "from_level": "OK",
 "to_level": "ERROR"
 },
 {
 "rule": "highNginxCPU",
 "timestamp": "2017-04-17T10.11.12.123",
 "value": 98.5,
 "from_level" : "OK",
 "to_level": "WARN"
 },
 {
 "rule": "highNginxCPU",
 "timestamp": "2017-04-17T10.12.14.000",
 "value": 97.0,
 "from_level" : "WARN",
 "to_level": "OK"
 }
]
```
{: screen}


## Recuperando o histórico de uma regra para as últimas 48 horas
{: #by_time}

Execute o comando cURL a seguir para obter o histórico de um alerta para as últimas 48 horas:

```
curl -H "X-Auth-User-Token: iam $Token" -H "X-Auth-Scope-Id:$Space" METRICS_ENDPOINT/v1/alert/history?start=-48h
```
{: codeblock}

	em que
	
* O *X-Auth-User-Token * é um parâmetro que transmite o token UAA, o token do IAM ou a chave API.
	
* *Auth_Type* é o prefixo que define o tipo de token ou a chave API. A lista a seguir descreve os valores válidos: *apikey*, *iam* ou *uaa*, em que

        * *apikey* identifica que o token é uma chave API.
	* *iam* identifica que o token especificado é um token IAM gerado.
	* *uaa* identifica que o token especificado é um token UAA gerado.
	
* *X-Auth-Scope-Id* é um parâmetro que transmite o GUID de espaço. O GUID deve ser prefixado com *s-* para identificar um espaço. Esse cabeçalho será necessário se você usar uma autenticação do token UAA. Se você usar um token de autenticação IAM, esse cabeçalho será opcional e as informações de domínio ligadas ao token serão usadas.
	
* *Token* é o token de autenticação do UAA ou IAM ou a chave API.
	
* *Space* é o GUID do espaço. 
	
* *RULE_NAME* é o nome da regra que é usada para acionar o alerta. O valor é aquele que é especificado no campo *name*.
	
* *METRICS_ENDPOINT* representa o ponto de entrada para o serviço. Cada região possui uma URL diferente. Para obter a lista de terminais por região, veja [Terminais](/docs/services/cloud-monitoring/send_retrieve_metrics_ov.html#endpoints).
	

## Recuperando as últimas 3 entradas no histórico de uma regra
{: #number}

Execute o comando cURL a seguir para obter as últimas 3 entradas no histórico de um alerta:

```
curl -H "X-Auth-User-Token: iam $Token" -H "X-Auth-Scope-Id:$Space" METRICS_ENDPOINT/v1/alert/history?max_results=3
```
{: codeblock}

	em que
	
* O *X-Auth-User-Token * é um parâmetro que transmite o token UAA, o token do IAM ou a chave API.
	
* *Auth_Type* é o prefixo que define o tipo de token ou a chave API. A lista a seguir descreve os valores válidos: *apikey*, *iam* ou *uaa*, em que

        * *apikey* identifica que o token é uma chave API.
	* *iam* identifica que o token especificado é um token IAM gerado.
	* *uaa* identifica que o token especificado é um token UAA gerado.
	
* *X-Auth-Scope-Id* é um parâmetro que transmite o GUID de espaço. O GUID deve ser prefixado com *s-* para identificar um espaço. Esse cabeçalho será necessário se você usar uma autenticação do token UAA. Se você usar um token de autenticação IAM, esse cabeçalho será opcional e as informações de domínio ligadas ao token serão usadas.
	
* *Token* é o token de autenticação do UAA ou IAM ou a chave API.
	
* *Space* é o GUID do espaço. 
	
* *RULE_NAME* é o nome da regra que é usada para acionar o alerta. O valor é aquele que é especificado no campo *name*.
	
* *METRICS_ENDPOINT* representa o ponto de entrada para o serviço. Cada região possui uma URL diferente. Para obter a lista de terminais por região, veja [Terminais](/docs/services/cloud-monitoring/send_retrieve_metrics_ov.html#endpoints).
	
