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


# Trabalhando com notificações usando a API de Alertas
{: #notifications}

Use a API de Alertas para criar, excluir e atualizar uma notificação, para mostrar os detalhes para uma notificação e para listar as notificações que são definidas no espaço do {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

## Criando um modelo de notificação
{: #template}

Uma notificação é um arquivo JSON. 

É possível criar qualquer número de modelos de notificação e, em seguida, reutilizá-los para criar notificações desse tipo em sua organização. 

É possível definir qualquer um dos tipos de notificações a seguir:

* E-mail: defina uma notificação do tipo *Email* para enviar um e-mail para um endereço de e-mail válido. 
* Webhook: defina uma notificação do tipo *Webhook* para terminais https apenas. Inclua um parâmetro no terminal para ajudar a reduzir a chance de mais alguém tentar chamar o seu terminal.
* Pagerduty: defina uma notificação do tipo *PagerDuty* para enviar os dados de alerta de uma métrica para seu sistema de gerenciamento de incidente PagerDuty. 

Por exemplo, a tabela a seguir lista exemplos de modelos de notificação:

<table>
  <caption></caption>
  <tr>
    <th>Tipo</th>
	<th>Modelo</th>
	<th>Amostra</th>
  </tr>
  <tr>
    <td>Email</td>
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
	"description" : "Send email notification when there is an infrastructure problem.", "detalhe": "xxx@yyy.com " }
	```
	{: screen}
	</td>
  </tr>
  <tr>
    <td>Webhook</td>
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

Em que

* O *Template_Name* define o nome do modelo de notificação.
* A *Description* explica quando esse tipo de notificação é usado.
* O *EmailAddress* define o endereço de e-mail do destinatário da notificação.
* O *Endpoint* define a URL na qual o POST deve ser feito. Essa é uma URL que pode levar solicitação POST. 
* A *Pagerduty_APIkey* define uma chave API exclusiva. Essa chave API é gerada por um administrador ou um proprietário de conta do PagerDuty.


Conclua as etapas a seguir para criar um modelo de notificação:

1. Crie um diretório para armazenar os recursos de serviço {{site.data.keyword.monitoringshort}}, por exemplo, *cloud-monitoring*.

    Por exemplo, em um sistema Ubuntu, execute o comando a seguir:
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
2. Crie um diretório para armazenar seus modelos de notificação, por exemplo, *notification-templates*.

    Por exemplo, em um sistema Ubuntu, execute o comando a seguir:
	
	```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	Altere para este diretório:
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
3. Crie um modelo de notificação em um diretório local de seu sistema.

    Por exemplo, crie o arquivo **email-template.json** para um modelo de notificação por e-mail, por exemplo, usando o editor de vi: 
	
	```
	{
    "name": "email_template",
    "type": "Email",
    "description" : "Send email to manager of department X when ....",
    "detail": "xxx@yyy"
    }
	```
	{: codeblock}
	


## Criando uma Notificação
{: #create}

Para criar uma notificação, conclua as etapas a seguir:

1. Crie um arquivo de notificação.

    1. Use um modelo de notificação para criar o arquivo. Para obter mais informações, consulte [Criando um modelo de notificação](#template).
	
	2. Atualize o arquivo:
	
	    * Insira um nome exclusivo no campo *name*. O valor desse campo é usado posteriormente para atualizar, excluir e mostrar os detalhes da notificação.
		* Digite uma descrição.
		* Insira um endereço de e-mail válido, uma URL de terminal ou a chave PagerDuty.
		
	3. Salve o arquivo. Por exemplo, use um prefixo como *s-* para notificações que você define para as consultas em execução em um espaço do {{site.data.keyword.Bluemix_notm}}.
	
	    **Dica:** crie seu arquivo de notificação no seguinte diretório: *~/cloud-monitoring/notifications/* para agrupar os recursos do serviço {{site.data.keyword.monitoringshort_notm}}. 
	
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
	
5. Execute o comando cURL a seguir para criar uma notificação:

    ```
	curl -XPOST -d @Notification_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: codeblock}
	
	Em que
	
	* Notification_File é o arquivo JSON que define sua notificação.
	
	* O *X-Auth-User-Token* é um parâmetro que transmite o token de UAA, o token do IAM ou a chave API do {{site.data.keyword.Bluemix_notm}}.
	
	* *Auth_Type* é o prefixo que define o tipo de token ou a chave API. A lista a seguir descreve os valores válidos: *apikey*, *iam* ou *uaa*, em que

        * *apikey* identifica que o token é uma chave API.
		* *iam* identifica que o token especificado é um token IAM gerado.
		* *uaa* identifica que o token especificado é um token UAA gerado.
	
	* *X-Auth-Scope-Id* é um parâmetro que transmite o GUID de espaço. O GUID deve ser prefixado com *s-* para identificar um espaço. Esse cabeçalho será necessário se você usar uma autenticação do token UAA. Se você usar um token de autenticação IAM, esse cabeçalho será opcional e as informações de domínio ligadas ao token serão usadas.
	
	* Token é o token UAA, o token IAM ou a chave API.
	
	* Espaço é o GUID do espaço. Ele será necessário apenas quando você usar um token UAA.
	
    Por exemplo, 	
	
	```
	curl -XPOST -d @s-email-dep-A.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.{DomainName}/v1/alert/notification
	```
	{: screen}

## Excluindo uma notificação
{:#delete}

Para excluir uma notificação, conclua as etapas a seguir:

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
	
4. Execute o comando cURL a seguir para excluir uma notificação:

    ```
	curl -XDELETE --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification/Notification_Name
	```
	{: codeblock}
	
	Em que
	
	* *Auth_Type* é o prefixo que define o tipo de token ou a chave API. A lista a seguir descreve os valores válidos: *apikey*, *iam* ou *uaa*, em que

        * *apikey* identifica que o token é uma chave API.
		* *iam* identifica que o token especificado é um token IAM gerado.
		* *uaa* identifica que o token especificado é um token UAA gerado.
	
	* O *X-Auth-User-Token* é um parâmetro que transmite o token de UAA, o token do IAM ou a chave API do {{site.data.keyword.Bluemix_notm}}. O token ou a chave API deve ser prefixado com um dos valores a seguir: *apikey*, *iam* ou *uaa*, em que

        * *apikey* identifica que o token é uma chave API.
		* *iam* identifica que o token especificado é um token IAM gerado.
		* *uaa* identifica que o token especificado é um token UAA gerado.
		
	* Token é o token UAA, o token IAM ou a chave API.
	
	* Espaço é o GUID do espaço. Ele será necessário apenas quando você usar um token UAA.
	
	* Notification_Name é o nome da notificação, ou seja, o valor do campo *name* quando você lista as propriedades de uma notificação.
	


## Listando todas as notificações
{: #list}

Para listar todas as notificações, conclua as etapas a seguir:

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
	
4. Execute o comando cURL para listar todas as notificações:

    ```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notifications
	```
	{: codeblock}
	
	Em que
	
	* *Auth_Type* é o prefixo que define o tipo de token ou a chave API. A lista a seguir descreve os valores válidos: *apikey*, *iam* ou *uaa*, em que

        * *apikey* identifica que o token é uma chave API.
		* *iam* identifica que o token especificado é um token IAM gerado.
		* *uaa* identifica que o token especificado é um token UAA gerado.
		
	* O *X-Auth-User-Token* é um parâmetro que transmite o token de UAA, o token do IAM ou a chave API do {{site.data.keyword.Bluemix_notm}}. O token ou a chave API deve ser prefixado com um dos valores a seguir: *apikey*, *iam* ou *uaa*, em que

        * *apikey* identifica que o token é uma chave API.
		* *iam* identifica que o token especificado é um token IAM gerado.
		* *uaa* identifica que o token especificado é um token UAA gerado.
		
	* Token é o token UAA, o token IAM ou a chave API.
	
	* Espaço é o GUID do espaço. Ele será necessário apenas quando você usar um token UAA.


			

## Mostrando os detalhes de uma notificação
{: #show}


Para mostrar as informações sobre uma notificação conclua as etapas a seguir:

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
	
4. Execute o comando cURL a seguir para mostrar os detalhes de uma notificação:

    ```
	curl -XGET --header "X-Auth-User-Token:uaa ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification/Notification_Name
	```
	{: codeblock}
	
	Em que
	
	* O *X-Auth-User-Token* é um parâmetro que transmite o token de UAA, o token do IAM ou a chave API do {{site.data.keyword.Bluemix_notm}}. O token ou a chave API deve ser prefixado com um dos valores a seguir: *apikey*, *iam* ou *uaa*, em que

        * *apikey* identifica que o token é uma chave API.
		* *iam* identifica que o token especificado é um token IAM gerado.
		* *uaa* identifica que o token especificado é um token UAA gerado.
		
	* Token é o token UAA, o token IAM ou a chave API.
	
	* Espaço é o GUID do espaço. Ele será necessário apenas quando você usar um token UAA.
	
	* Notification_Name é o nome da notificação, ou seja, o valor do campo *name* quando você lista as propriedades de uma notificação.
	
     
		

			
## Testando uma notificação
{: #test}	
			
Para testar uma notificação, conclua as etapas a seguir:

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
	
4. Execute o comando cURL a seguir para testar uma notificação:

    ```
	curl -XPOST --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification/test/Notification_Name
	```
	{: codeblock}
	
	Em que
	
	* *Auth_Type* é o prefixo que define o tipo de token ou a chave API. A lista a seguir descreve os valores válidos: *apikey*, *iam* ou *uaa*, em que

        * *apikey* identifica que o token é uma chave API.
		* *iam* identifica que o token especificado é um token IAM gerado.
		* *uaa* identifica que o token especificado é um token UAA gerado.
	
	* O *X-Auth-User-Token* é um parâmetro que transmite o token de UAA, o token do IAM ou a chave API do {{site.data.keyword.Bluemix_notm}}. O token ou a chave API deve ser prefixado com um dos valores a seguir: *apikey*, *iam* ou *uaa*, em que

        * *apikey* identifica que o token é uma chave API.
		* *iam* identifica que o token especificado é um token IAM gerado.
		* *uaa* identifica que o token especificado é um token UAA gerado.
		
	* Token é o token UAA, o token IAM ou a chave API.
	
	* Espaço é o GUID do espaço. Ele será necessário apenas quando você usar um token UAA.
	
	* Notification_Name é o nome da notificação, ou seja, o valor do campo *name* quando você lista as propriedades de uma notificação.
	


## Atualizando uma notificação
{: #update}

Para atualizar uma notificação, conclua as etapas a seguir:

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
	
4. Execute o comando cURL a seguir para atualizar uma notificação:

    ```
	curl -XPUT -d @Notification_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: codeblock}
	
	Em que
	
	* Notification_File é o arquivo JSON que define sua notificação.
	
	* *Auth_Type* é o prefixo que define o tipo de token ou a chave API. A lista a seguir descreve os valores válidos: *apikey*, *iam* ou *uaa*, em que

        * *apikey* identifica que o token é uma chave API.
		* *iam* identifica que o token especificado é um token IAM gerado.
		* *uaa* identifica que o token especificado é um token UAA gerado.
	
	* O *X-Auth-User-Token * é um parâmetro que transmite o token UAA bluemix, o token IAM ou a chave API. O token ou a chave API deve ser prefixado com um dos valores a seguir: *apikey*, *iam* ou *uaa*, em que

        * *apikey* identifica que o token é uma chave API.
		* *iam* identifica que o token especificado é um token IAM gerado.
		* *uaa* identifica que o token especificado é um token UAA gerado.
		
	* Token é o token UAA, o token IAM ou a chave API.
	
	* Espaço é o GUID do espaço. Ele será necessário apenas quando você usar um token UAA.

	
        

