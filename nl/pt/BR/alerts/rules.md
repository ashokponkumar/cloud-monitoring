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


# Trabalhando com regras usando a API de Alertas
{: #rules}

Use a API de Alertas para criar, excluir e atualizar uma regra, para mostrar os detalhes para uma regra e para listar as regras que são definidas em seu espaço do {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}


## Criando uma regra
{: #create}

Para criar uma regra, conclua as etapas a seguir:

1. Crie um arquivo de regras que contenha JSON válido. Salve o arquivo. 

    Por exemplo, para salvar o arquivo, use um prefixo como *s-* para as regras que você define para as consultas em execução em um espaço do {{site.data.keyword.Bluemix_notm}}.
	
	**Dicas:** 
	
	* Defina regras diferentes para uma consulta a fim de alertar sobre erros ou avisos usando um método de notificação diferente. É possível configurar somente um método de notificação por regra. 
	* Crie seu arquivo de regras no diretório a seguir: *~/cloud-monitoring/rules/* para agrupar os recursos do serviço {{site.data.keyword.monitoringshort_notm}}. 

    Insira as informações a seguir no arquivo de regras:
	
	* *Nome*: Insira um nome exclusivo. O valor desse campo é usado posteriormente para atualizar, excluir e mostrar os detalhes da regra.
	* *Descrição*: Insira uma descrição.
	* *expression*: insira a consulta que você deseja monitorar.
	* *enabled*: configure para *true* para ativar a regra.
	* *from*: momento inicial que é usado para analisar os dados com base nos valores de limite.
	* *until*: momento final que é usado para analisar os dados com base nos valores de limite.
	* *comparison*: a operação de comparação que é usada para identificar o tipo de verificação a ser feita, por exemplo, *below*.
	* *error_level*: define o limite que você configura para acionar um alerta de erro.
	* *warning_level*: define o limite que você configura para acionar um alerta de aviso.
	* *frequency*: define a frequência com que você verifica os dados.
	* *dashboard_url*: define uma URL que mostra um painel com a consulta no Grafana.
	* *notifications*: o método de notificação em caso de alerta descrito com essa regra é acionado.
	
	Por exemplo, 
	
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
	
2. Efetue login em uma região, uma organização e um espaço do {{site.data.keyword.Bluemix_notm}}. Execute o comando:

    Por exemplo, para efetuar login na região sul dos EUA, execute o comando a seguir:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Siga as instruções. Insira suas credenciais do {{site.data.keyword.Bluemix_notm}}, selecione uma organização e um espaço.

3. Obtenha o token de autenticação ou a chave API.

    * Para a autenticação do IAM, veja [Obtendo o token do IAM usando a CLI do Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) ou [Gerando uma chave API do IAM usando a CLI do Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Para a autenticação do UAA, veja [Obtendo o token do UAA usando a CLI do Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) ou [Obtendo o token do UAA usando a API de REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

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
	
4. Obtenha o GUID de espaço. O GUID deve ser prefixado com *s-* para identificar um espaço.

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
	
	Em seguida, exporte a *Space* variável:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. Execute o comando cURL a seguir para criar uma regra:

    ```
	curl -XPOST -d @Rule_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: codeblock}
	
	Em que
	
	* Rule_File é o arquivo JSON que define sua regra de alerta.
	
	* O *X-Auth-User-Token* é um parâmetro que transmite o token do UAA, o token do IAM ou a chave API do {{site.data.keyword.Bluemix_notm}}.
	
	* *Auth_Type* é o prefixo que define o tipo de token ou a chave API. A lista a seguir descreve os valores válidos: *apikey*, *iam* ou *uaa*, em que

        * *apikey* identifica que o token é uma chave API.
		* *iam* identifica que o token especificado é um token IAM gerado.
		* *uaa* identifica que o token especificado é um token UAA gerado.
	
	* *X-Auth-Scope-Id* é um parâmetro que transmite o GUID de espaço. O GUID deve ser prefixado com *s-* para identificar um espaço. Esse cabeçalho será necessário se você usar uma autenticação do token UAA. Se você usar um token de autenticação IAM, esse cabeçalho será opcional e as informações de domínio ligadas ao token serão usadas.
	
	* Token é o token de autenticação UAA ou IAM ou a chave API.
	
	* Espaço é o GUID do espaço. Ele será necessário apenas quando você usar um token UAA.
	
    Por exemplo, 	
	
	```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}

## Excluindo uma regra
{: #delete}

Para excluir uma regra, conclua as etapas a seguir:

1. Efetue login em uma região, uma organização e um espaço do {{site.data.keyword.Bluemix_notm}}. Execute o comando:

    Por exemplo, para efetuar login na região sul dos EUA, execute o comando a seguir:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Siga as instruções. Insira suas credenciais do {{site.data.keyword.Bluemix_notm}}, selecione uma organização e um espaço.

2. Obtenha o token de autenticação ou a chave API.

    * Para a autenticação do IAM, veja [Obtendo o token do IAM usando a CLI do Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) ou [Gerando uma chave API do IAM usando a CLI do Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Para a autenticação do UAA, veja [Obtendo o token do UAA usando a CLI do Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) ou [Obtendo o token do UAA usando a API de REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

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
	
	Em seguida, exporte a *Space* variável:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. Execute o comando cURL a seguir para excluir uma regra:

    ```
	curl -XDELETE --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/rule/Rule_Name
	```
	{: codeblock}
	
	Em que
	
    * O *X-Auth-User-Token* é um parâmetro que transmite o token do UAA, o token do IAM ou a chave API do {{site.data.keyword.Bluemix_notm}}.
	
	* *Auth_Type* é o prefixo que define o tipo de token ou a chave API. A lista a seguir descreve os valores válidos: *apikey*, *iam* ou *uaa*, em que

        * *apikey* identifica que o token é uma chave API.
		* *iam* identifica que o token especificado é um token IAM gerado.
		* *uaa* identifica que o token especificado é um token UAA gerado.
	
	* *X-Auth-Scope-Id* é um parâmetro que transmite o GUID de espaço. O GUID deve ser prefixado com *s-* para identificar um espaço. Esse cabeçalho será necessário se você usar uma autenticação do token UAA. Se você usar um token de autenticação IAM, esse cabeçalho será opcional e as informações de domínio ligadas ao token serão usadas.
	
	* Token é o token de autenticação UAA ou IAM ou a chave API.
	
	* Espaço é o GUID do espaço. Ele será necessário apenas quando você usar um token UAA.
	
	* Rule_Name é o nome da regra conforme especificado no campo *name*.
	
    
	
## Listando todas as regras
{: #list}

Para listar todas as regras, conclua as etapas a seguir:

1. Efetue login em uma região, uma organização e um espaço do {{site.data.keyword.Bluemix_notm}}. Execute o comando:

    Por exemplo, para efetuar login na região sul dos EUA, execute o comando a seguir:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Siga as instruções. Insira suas credenciais do {{site.data.keyword.Bluemix_notm}}, selecione uma organização e um espaço.

2. Obtenha o token de autenticação ou a chave API.

    * Para a autenticação do IAM, veja [Obtendo o token do IAM usando a CLI do Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) ou [Gerando uma chave API do IAM usando a CLI do Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Para a autenticação do UAA, veja [Obtendo o token do UAA usando a CLI do Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) ou [Obtendo o token do UAA usando a API de REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

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
	
	Em seguida, exporte a *Space* variável:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. Execute o comando cURL a seguir para listar todas as regras em um espaço:

    ```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/rules
	```
	{: codeblock}
	
	Em que
	
	* O *X-Auth-User-Token* é um parâmetro que transmite o token do UAA, o token do IAM ou a chave API do {{site.data.keyword.Bluemix_notm}}.
	
	* *Auth_Type* é o prefixo que define o tipo de token ou a chave API. A lista a seguir descreve os valores válidos: *apikey*, *iam* ou *uaa*, em que

        * *apikey* identifica que o token é uma chave API.
		* *iam* identifica que o token especificado é um token IAM gerado.
		* *uaa* identifica que o token especificado é um token UAA gerado.
	
	* *X-Auth-Scope-Id* é um parâmetro que transmite o GUID de espaço. O GUID deve ser prefixado com *s-* para identificar um espaço. Esse cabeçalho será necessário se você usar uma autenticação do token UAA. Se você usar um token de autenticação IAM, esse cabeçalho será opcional e as informações de domínio ligadas ao token serão usadas.
	
	* Token é o token de autenticação UAA ou IAM ou a chave API.
	
	* Espaço é o GUID do espaço. Ele será necessário apenas quando você usar um token UAA.
	

	
	

## Mostrando os detalhes de uma regra
{: show}

Para mostrar os detalhes de uma regra, conclua as etapas a seguir:

1. Efetue login em uma região, uma organização e um espaço do {{site.data.keyword.Bluemix_notm}}. Execute o comando:

    Por exemplo, para efetuar login na região sul dos EUA, execute o comando a seguir:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Siga as instruções. Insira suas credenciais do {{site.data.keyword.Bluemix_notm}}, selecione uma organização e um espaço.

2. Obtenha o token de autenticação ou a chave API.

    * Para a autenticação do IAM, veja [Obtendo o token do IAM usando a CLI do Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) ou [Gerando uma chave API do IAM usando a CLI do Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Para a autenticação do UAA, veja [Obtendo o token do UAA usando a CLI do Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) ou [Obtendo o token do UAA usando a API de REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

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
	
	Em seguida, exporte a *Space* variável:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. Execute o comando cURL a seguir para mostrar os detalhes de uma regra:

    ```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/rule/Rule_Name
	```
	{: codeblock}
	
	Em que
	
	* O *X-Auth-User-Token* é um parâmetro que transmite o token do UAA, o token do IAM ou a chave API do {{site.data.keyword.Bluemix_notm}}.
	
	* *Auth_Type* é o prefixo que define o tipo de token ou a chave API. A lista a seguir descreve os valores válidos: *apikey*, *iam* ou *uaa*, em que

        * *apikey* identifica que o token é uma chave API.
		* *iam* identifica que o token especificado é um token IAM gerado.
		* *uaa* identifica que o token especificado é um token UAA gerado.
	
	* *X-Auth-Scope-Id* é um parâmetro que transmite o GUID de espaço. O GUID deve ser prefixado com *s-* para identificar um espaço. Esse cabeçalho será necessário se você usar uma autenticação do token UAA. Se você usar um token de autenticação IAM, esse cabeçalho será opcional e as informações de domínio ligadas ao token serão usadas.
	
	* Token é o token de autenticação UAA ou IAM ou a chave API.
	
	* Espaço é o GUID do espaço. Ele será necessário apenas quando você usar um token UAA.
	
	* Rule_Name é o nome da regra conforme especificado no campo *name*.
	

## Atualizando uma regra
{: #update}

Para atualizar uma regra, conclua as etapas a seguir:

1. Efetue login em uma região, uma organização e um espaço do {{site.data.keyword.Bluemix_notm}}. Execute o comando:

    Por exemplo, para efetuar login na região sul dos EUA, execute o comando a seguir:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Siga as instruções. Insira suas credenciais do {{site.data.keyword.Bluemix_notm}}, selecione uma organização e um espaço.

2. Obtenha o token de autenticação ou a chave API.

    * Para a autenticação do IAM, veja [Obtendo o token do IAM usando a CLI do Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) ou [Gerando uma chave API do IAM usando a CLI do Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Para a autenticação do UAA, veja [Obtendo o token do UAA usando a CLI do Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) ou [Obtendo o token do UAA usando a API de REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

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
	
	Em seguida, exporte a *Space* variável:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. Execute o comando cURL a seguir para atualizar uma regra:

    ```
	curl -XPUT `-d @Rule_File` --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: codeblock}
	
	Em que
	
	* Rule_File é o arquivo JSON que define sua regra de alerta.
	
	* O *X-Auth-User-Token* é um parâmetro que transmite o token do UAA, o token do IAM ou a chave API do {{site.data.keyword.Bluemix_notm}}.
	
	* *Auth_Type* é o prefixo que define o tipo de token ou a chave API. A lista a seguir descreve os valores válidos: *apikey*, *iam* ou *uaa*, em que

        * *apikey* identifica que o token é uma chave API.
		* *iam* identifica que o token especificado é um token IAM gerado.
		* *uaa* identifica que o token especificado é um token UAA gerado.
	
	* *X-Auth-Scope-Id* é um parâmetro que transmite o GUID de espaço. O GUID deve ser prefixado com *s-* para identificar um espaço. Esse cabeçalho será necessário se você usar uma autenticação do token UAA. Se você usar um token de autenticação IAM, esse cabeçalho será opcional e as informações de domínio ligadas ao token serão usadas.
	
	* Token é o token de autenticação UAA ou IAM ou a chave API.
	
	* Espaço é o GUID do espaço. Ele será necessário apenas quando você usar um token UAA.

	
	
