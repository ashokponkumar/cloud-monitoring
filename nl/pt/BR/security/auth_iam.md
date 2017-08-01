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


# Autenticando usando o modelo IAM do Bluemix
{: #auth_iam}

Use o modelo IAM do {{site.data.keyword.Bluemix}} para obter um token de autenticação que pode ser usado para acessar métricas que são armazenadas no serviço {{site.data.keyword.monitoringshort}} ou para obter uma chave API. O token tem um tempo de expiração. A chave API não expira.
{:shortdesc}


## Obtendo o token IAM usando a CLI do Bluemix 
{: #iam_token_cli}

Para obter o token de autorização usando a CLI do {{site.data.keyword.Bluemix_notm}}, conclua as etapas a seguir:

1. Instale a CLI do {{site.data.keyword.Bluemix_notm}}.

   Para obter mais informações, veja [Instalando a CLI do {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa).
   
   Se a CLI estiver instalada, continue com a próxima etapa.
    
2. Efetue login em uma região, uma organização e um espaço do {{site.data.keyword.Bluemix_notm}}. Execute o comando:

    Por exemplo, para a região sul dos EUA, execute o comando a seguir:
	
    ```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Siga as instruções. Insira suas credenciais do {{site.data.keyword.Bluemix_notm}}, selecione uma organização e um espaço.
	
3. Execute o comando `bx iam oauth-tokens` para obter o token IAM.

    ```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	A saída retorna o token IAM que se deve usar para autenticar seu ID do usuário nesse espaço e nessa organização.

**Nota:** Ao usar o token, remova *Bearer* do valor do token que você passa em uma chamada API.
		
		
## Gerando uma chave API do IAM usando a CLI do Bluemix
{: #iam_apikey_cli}

Conclua as etapas a seguir para gerar uma chave API do IAM usando a CLI do {{site.data.keyword.Bluemix_notm}}:

1. Instale a CLI do {{site.data.keyword.Bluemix_notm}}.

   Para obter mais informações, veja [Instalando a CLI do {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa).

   Se a CLI estiver instalada, continue com a próxima etapa.
	
2. Efetue login no {{site.data.keyword.Bluemix_notm}} usando o comando da CLI `bx login` do {{site.data.keyword.Bluemix_notm}}:

    Por exemplo, para a região sul dos EUA, execute o comando a seguir:
	
    ```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Siga as instruções. Insira suas credenciais do {{site.data.keyword.Bluemix_notm}}, selecione uma organização e um espaço.
 
3. Execute o comando `bx iam api-key-create` para criar uma chave API do IAM.

    ```
    bx iam api-key-create NAME [-d DESCRIPTION][-f, --file FILE]
	```
	{: codeblock} 
	
	em que
	
	* NAME é o nome da chave API a ser criada.
	* DESCRIPTION é o texto que é usado para descrever a chave API.
	* FILE é o nome do arquivo no qual a chave é salva.
	
    Por exemplo, para criar uma chave API *MyKey*, execute o comando a seguir:
	
	```
	bx iam api-key-create MyKey -d "This is my API key for service X" 
	```
	{: screen}
	
	
	
	
## Gerando uma chave API do IAM usando a UI do Bluemix
{: #iam_apikey_ui}

Conclua as etapas a seguir para gerar uma chave API do IAM usando a UI do {{site.data.keyword.Bluemix_notm}}:

1. Efetue login no console {{site.data.keyword.Bluemix_notm}}. 

2. Na barra de menus do console, clique em **Gerenciar > Segurança > Chaves API do Bluemix**.

3. Clique em ** Criar API key * *.

4. Insira um nome e uma descrição para sua chave API. Em seguida, clique em ** Criar API key * *.

5. Salve a chave API. Clique em ** Download chave API * *.

    Clique em **Show** para exibir a chave API.  

**Observação:** a chave API está disponível para ser exibida ou para ser transferida por download somente no momento da criação. Se a chave API for perdida, uma nova chave API deverá ser criada.  


	
## Revogando uma chave API usando a UI do Bluemix
{: #revoke_ui}
	
Conclua as etapas a seguir para revogar uma chave API do IAM por meio da UI do {{site.data.keyword.Bluemix_notm}}

1. Efetue login no console {{site.data.keyword.Bluemix_notm}}.

2. Na barra de menus do console, clique em **Gerenciar > Segurança > Chaves API do Bluemix**.

3. Clique em **Ações** e, em seguida, em **Excluir Chave**.





	

	
	
	
	
	
	
