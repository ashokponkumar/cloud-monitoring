---

Copyright: years: 2017

lastupdated: "2017-07-12"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Recuperando dados do serviço de Monitoramento
{: #retrieve_data_api}

É possível recuperar métricas por meio do serviço do {{site.data.keyword.monitoringshort}} usando [a API de Métricas](https://console.bluemix.net/apidocs/927-ibm-cloud-monitoring-rest-api?&language=node#introduction){: new_window}. É possível recuperar métricas de um espaço do {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}


Para definir o conjunto de dados que você planeja recuperar, considere as informações a seguir: 

* Deve-se configurar o espaço do {{site.data.keyword.Bluemix_notm}} por meio do qual você deseja recuperar os dados.

* Deve-se fornecer um token ou uma chave API para trabalhar com o serviço do {{site.data.keyword.monitoringshort}}. 

* Deve-se especificar um caminho para 1 ou mais métricas. Para obter mais informações, veja [Definindo as métricas](#metrics).

* Como opção, é possível especificar um período customizado. Por padrão, se você não especificar um período, os dados recuperados serão aqueles que corresponderem às últimas 24 horas. Para obter mais informações, veja [Configurando um período de tempo](#time).

**Nota:** é possível recuperar um máximo de 5 destinos por solicitação.


## Definindo métricas
{: #metrics}

Para recuperar dados para uma métrica, deve-se especificar o caminho da métrica da raiz da árvore de métricas até a métrica e deve-se separar cada etapa com um ponto (.).

O caminho é especificado no parâmetro **Target**. Um máximo de 5 destinos pode ser especificado por solicitação.  

É possível, opcionalmente, aplicar funções na métrica. Para obter mais informações sobre funções suportadas, veja [Funções do Graphite 0.9.15![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](http://graphite.readthedocs.io/en/latest/functions.html "Ícone de link externo").

É possível usar curingas para definir um caminho. Usando curingas, é possível identificar mais de uma métrica em um único caminho. 

* **Asterisco**: é possível usar um asterisco (*) para corresponder zero ou mais caracteres. 

    É possível ter mais de um asterisco dentro de um único elemento de caminho, por exemplo: `servers.sp*aeg*r.cpu.total.*` Esse exemplo retorna dados sobre o total de CPU para todos os servidores que correspondem ao padrão.

* **Lista ou intervalo de caracteres**: é possível definir os caracteres entre colchetes ([...]) para especificar um único caractere na posição da sequência de caminho. 

    É possível definir um intervalo inclusivo de caracteres especificando dois caracteres separados por um hífen (-). Qualquer caractere entre esses dois caracteres corresponderá. 
	
	É possível definir um ou mais intervalos de caracteres entre colchetes. 
	
	Quando não for possível ler os caracteres entre um conjunto de colchetes como um intervalo, eles serão tratados como uma lista de valores. 
	
	É possível incluir um hífen (-) como um caractere em uma lista de valores colocando-o no início ou término entre colchetes.

* **Lista de valores**: é possível configurar valores separados por vírgula dentro de chaves para definir listas de valores. 

    Por exemplo, para fazer download de métricas para os caminhos a seguir: `servers.myserver.cpu.total.user` e `servers.myserver.cpu.total.system}`, é possível configurar um único caminho para ambos os tipos: `servers.myserver.cpu.total.{user,system}`



## Configurando um período de tempo
{: #time}

É possível, opcionalmente, especificar um período de tempo usando um tempo relativo ou um tempo absoluto. Deve-se definir os parâmetros de consulta **From** e **Until**.  

* Quando o horário de início do período de tempo for omitido, o valor padrão será 24 horas atrás. 

* Quando o horário de encerramento do período de tempo for omitido, o valor padrão será o horário atual *agora*.

O **Tempo relativo** é sempre precedido por um sinal de menos (-) e seguido por uma unidade de tempo. 

A tabela a seguir lista as diferentes abreviações de tempo relativo que podem ser usadas:


| Abreviação | Descrição |
|--------------|-------------|
| s            | Segundos     |
| mínimo          | Minutos     |
| h            | Horas       |
| D            | Dias        |
| w            | Semanas       |
| Seg          | Meses      |
| agora          | Hora Atual |


É possível usar qualquer um dos formatos a seguir para definir um **Tempo absoluto**: `HH:MM_YYMMDD`, `YYYYMMDD`, `MM/DD/YY`

| Abreviação | Descrição |
|--------------|-------------|
| YYYY         | Ano com quatro dígitos |
| MM           | Dois dígitos de mês (01=Jan etc) |
| DD           | Dois dígitos de dia do mês (01 até 31) |
| hh           | Dois dígitos de hora (00 até 23) |
| mm           | Dois dígitos de minuto (00 até 59) |
| ss           | Dois dígitos de segundo (00 até 59) |

**Exemplo**

A tabela a seguir mostra exemplos diferentes sobre como configurar períodos de tempo diferentes definindo os parâmetros *from* e *until*:

<table>
  <caption>Tabela 1. Exemplos diferentes que mostram como configurar períodos de tempo diferentes definindo os parâmetros *from* e *until*</caption>
  <tr>
    <th>Exemplo</th>
	<th>Descrição</th>
  </tr>
  <tr>
    <td>&from=monday</td>
	<td>Período de tempo que inclui dados de segunda-feira passada até agora.</td>
  </tr>
  <tr>
    <td>&from=20171101&until=20171130</td>
	<td>Período de tempo configurado para um mês, por exemplo, novembro </td>
  </tr>
  <tr>
    <td>&from=02:00_20170701&until=14:00_20170701</td>
	<td>Para mostrar dados em um ciclo de 24 horas, por exemplo, das 02h às 14h no dia 1 de julho de 2017</td>
  </tr>
  <tr>
    <td>&from=-8d&until=-7d</td>
	<td>Para mostrar o mesmo dia da semana, por exemplo, na semana passada</td>
  </tr>
  
</table>


## Configurando o espaço
{: #domain}

Quando você usar o modelo de autenticação do UAA do {{site.data.keyword.Bluemix_notm}} ou o modelo de autenticação do IAM do {{site.data.keyword.Bluemix_notm}} para autenticar com o serviço do {{site.data.keyword.monitoringshort}}, o campo de cabeçalho **X-Auth-Scope-Id** será necessário e deverá ser configurado para o GUID de espaço. O GUID deve ser prefixado com *s-* para identificar um espaço.



## Configurando o token de autorização ou a chave API
{: #security}
  
Deve-se configurar o campo **X-Auth-User-Token** para definir o token do UAA, o token do IAM ou a chave API que é usada para autenticação. 

Um token ou uma chave API deve ser prefixada com um dos valores a seguir: `apikey`, `iam` ou `uaa` 

Em que 

* *apikey* identifica que o token é uma chave API.
* *iam* identifica que o token especificado é um token gerado pelo IAM.
* *uaa* identifica que o token especificado é um token gerado pelo UAA

Por exemplo, é possível configurar o cabeçalho a seguir para uma chave API: `X-Auth-User-Token: apikey SomeIAMGeneratedKey`


## Recuperando métricas de um espaço usando o UAA
{: #uaa}

Para enviar métricas de um espaço do {{site.data.keyword.Bluemix_notm}}, conclua as etapas a seguir:

1. Efetue login em uma região, uma organização e um espaço do {{site.data.keyword.Bluemix_notm}}. Execute o
comando:

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
	export Token="eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8"
	```
	{: screen}
	
	**Nota:** O token exclui *Bearer*.
		
3. Obtenha o GUID do espaço. O GUID deve ser prefixado com *s-* para identificar um espaço.

    Execute o comando a seguir:
	
	```
	cf space SpaceName --guid
	```
	{: codeblock}
	
	Em que *SpaceName* é o nome do espaço prefixado com *s-* no qual você definirá uma notificação.
	
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
	curl -XGET --header "X-Auth-User-Token:uaa ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics?from=Start_Time&until=End_Time&target=string
	```
	{: codeblock}

	em que
	
	* O *X-Auth-User-Token* é um parâmetro que passa o token do UAA do {{site.data.keyword.Bluemix_notm}}.
	
	* *uaa* é o prefixo que indica que o token especificado é um token gerado pelo UAA.
	
	* *X-Auth-Scope-Id* é um parâmetro que transmite o GUID de espaço. O GUID deve ser prefixado com *s-* para identificar um espaço. Esse cabeçalho será necessário apenas se você usar a autenticação de UAA. Se você usar um token de autenticação IAM, esse cabeçalho será opcional e as informações de domínio ligadas ao token serão usadas.
	
	* *Token* representa o token do UAA.
	
	* *Space* representa o GUID do espaço. Ele será necessário apenas quando você usar um token UAA.
	
	* *Start_Time* define o período de tempo relativo ou absoluto que define o início da solicitação. Assume o padrão para 24h atrás: `-24h`. *End_Time* define o período de tempo relativo ou absoluto para configurar o término da solicitação. Assume o padrão para tempo atual agora: `agora`. Para obter mais informações, veja [Configurando um período de tempo](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#time).
	
	* *Path* identifica uma ou várias métricas. É possível, opcionalmente, aplicar funções em cada métrica. Caminhos também suportam curingas, o que permite que você identifique mais de uma métrica em um único caminho. Para obter mais informações, veja [Definindo as métricas](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#metrics).
	
	
	
## Recuperando métricas de um espaço usando o IAM ou uma chave API
{: #iam}

Para recuperar métricas de um espaço do {{site.data.keyword.Bluemix_notm}} usando o IAM ou uma chave API, conclua as etapas a seguir:

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
	
	Em que *SpaceName* é o nome do espaço prefixado com *s-* no qual você definirá uma notificação.
	
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
	
5. Execute um comando cURL para recuperar métricas.

   	```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics?from=Start_Time&until=End_Time&target=string
	```
	{: codeblock}

	em que
	
	* O *X-Auth-User-Token* é um parâmetro que passa o token do AIM {{site.data.keyword.Bluemix_notm}} ou a chave API.
	
	* *Auth_Type* é o prefixo que define o tipo de token ou a chave API. 

        * *apikey* identifica que o token é uma chave API.
		* *iam* identifica que o token especificado é um token IAM gerado.
	
	* *X-Auth-Scope-Id* é um parâmetro que transmite o GUID de espaço. O GUID deve ser prefixado com *s-* para identificar um espaço. Esse cabeçalho será necessário apenas se você usar a autenticação de UAA. Se você usar um token de autenticação IAM, esse cabeçalho será opcional e as informações de domínio ligadas ao token serão usadas.
	
	* *Token* representa o token do UAA.
	
	* *Space* representa o GUID do espaço. Ele será necessário apenas quando você usar um token UAA.
	
	* *Start_Time* define o período de tempo relativo ou absoluto que define o início da solicitação. Assume o padrão para 24h atrás: `-24h`. *End_Time* define o período de tempo relativo ou absoluto para configurar o término da solicitação. Assume o padrão para tempo atual agora: `agora`. Para obter mais informações, veja [Configurando um período de tempo](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#time).
	
	* *Path* identifica uma ou várias métricas. É possível, opcionalmente, aplicar funções em cada métrica. Caminhos também suportam curingas, o que permite que você identifique mais de uma métrica em um único caminho. Para obter mais informações, veja [Definindo as métricas](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#metrics).
	
	
	





