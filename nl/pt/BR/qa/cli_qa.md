---

copyright:
  years: 2017

lastupdated: "2017-06-19"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Perguntas e respostas frequentes usando a CLI do Bluemix
{: #cli_qa}

Veja as respostas para as perguntas comuns sobre como usar a CLI do {{site.data.keyword.Bluemix}} com o serviço {{site.data.keyword.monitoringshort}}. 
{:shortdesc}

* [Como instalar o {{site.data.keyword.Bluemix_notm}} CLI](#install_bmx_cli)
* [Como obter o GUID de uma conta](#account_guid)
* [Como obter o GUID de uma organização](#org_guid)
* [Como obter o GUID de um espaço](#space_guid)


## Como instalar a CLI do Bluemix?
{: #install_bmx_cli}

Conclua as etapas a seguir para instalar a CLI do {{site.data.keyword.Bluemix_notm}}:

1. Faça download da CLI.

    Por exemplo, para instalar o pacote da CLI do {{site.data.keyword.Bluemix_notm}} em um sistema Ubuntu, faça download do pacote da CLI do [{{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](http://clis.ng.bluemix.net/ui/home.html "Ícone de link externo"){: new_window}. 

2. Execute o comando a seguir para extrair o pacote da CLI do {{site.data.keyword.Bluemix_notm}}:
    
    ```
    Tar -xvf Bluemix_CLI_0.5.4_amd64.tar.gz
    ```
    {: codeblock}
    
3. Acesse o diretório *Bluemix_CLI* e execute o comando `./install_bluemix_cli` com a permissão raiz para instalar o plug-in do CF. É possível executar o comando como um usuário raiz ou usar o comando sudo para obter a permissão raiz. Por exemplo:
    
    ```
    cd Bluemix_CLI
    ```
    {: codeblock}
    
    ```
    sudo ./install_bluemix_cli
    ```
    {: codeblock}
    
4. Verifique a instalação do plug-in do CF. Por exemplo, verifique a versão do plug-in do CF. Execute o seguinte comando:
    
    ```
    cf -v
    ```
    {: codeblock}
    
    A saída é semelhante ao seguinte:
    
    ```
    cf version 6.25.0+787326d.2017-02-28
    ```
    {: screen}
    
5. Verifique o installition do {{site.data.keyword.Bluemix_notm}} CLI. Por exemplo, verifique a versão do plug-in. Execute o seguinte comando:
    
    ```
    bx -version
    ```
    {: codeblock}
    
    A saída é semelhante ao seguinte:
    
    ```
    bx version 0.5.4+ae22935-2017-05-18T03:55:55+00:00
    ```
    {: screen}
	
## Como obter o GUID de uma conta
{: #account_guid}
	
Conclua as etapas a seguir para obter o GUID de uma conta:
	
1. Efetue login em uma região, uma organização e um espaço do {{site.data.keyword.Bluemix_notm}}. Execute o comando:

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    Siga as instruções. Insira suas credenciais do {{site.data.keyword.Bluemix_notm}}, selecione uma organização e um espaço.
	
2. Execute o comando `bx iam accounts` para obter o GUID de uma conta.

    ```
	bx iam accounts
	```
	{: codeblock} 
	
	Por exemplo, para recuperar todas as contas com seus GUIDs correspondentes para o usuário xxx@yyy.com, execute o comando:
	
	```
	bx iam accounts
	Retrieving all accounts of xxx@yyy.com...
    OK
    Account GUID                       Name                               Type    State    Owner User ID
    12345123451234512345123451234512   A Account                          TRIAL   ACTIVE   xxx@yyy.com
    23456234562345622456234561234561   B Account                          TRIAL   ACTIVE   zzz@yyy.com   
	```
	{: screen}

	
## Como obter o GUID de uma organização
{: #org_guid}

Conclua as etapas a seguir para obter o GUID de uma organização:
	
1. Efetue login em uma região, uma organização e um espaço do {{site.data.keyword.Bluemix_notm}}. Execute o comando:

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    Siga as instruções. Insira suas credenciais do {{site.data.keyword.Bluemix_notm}}, selecione uma organização e um espaço.

2. Execute o comando `bx iam org` para obter o GUID da organização. 

    ```
    bx iam org NAME --guid
    ```
    {: codeblock}
	
    em que NAME é o nome da organização do {{site.data.keyword.Bluemix_notm}}.
		
## Como obter o GUID de um espaço
{: #space_guid}
	
Conclua as etapas a seguir para obter o GUID de um espaço:
	
1. Efetue login em uma região, uma organização e um espaço do {{site.data.keyword.Bluemix_notm}}. Execute o comando:

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    Siga as instruções. Insira suas credenciais do {{site.data.keyword.Bluemix_notm}}, selecione uma organização e um espaço.
	
2. Execute o comando `bx iam space` para obter o GUID do espaço. 

    ```
    bx iam space NAME --guid
    ```
    {: codeblock}
	
    em que NAME é o nome de um espaço do {{site.data.keyword.Bluemix_notm}}. 
	
    Por exemplo, para obter o GUID para o espaço *dev*, execute o comando a seguir:
	
    ```
    bx iam space dev --guid
    e03afff1-3456-4af6-1234-59treg1b0abc
    ```
    {: screen}




		
		
