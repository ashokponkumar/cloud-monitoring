---

Copyright: years: 2017

lastupdated: "2017-07-10"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Fornecendo o serviço Monitoring
{: #provision}

É possível provisionar o serviço do {{site.data.keyword.monitoringshort}} por meio da UI do {{site.data.keyword.Bluemix}} ou por meio da linha de comandos.
{:shortdesc}


## Fornecimento por meio da UI
{: #ui}

Conclua as etapas a seguir para provisionar uma instância do serviço do {{site.data.keyword.monitoringshort}} no {{site.data.keyword.Bluemix_notm}}:

1. Efetue login em sua conta do {{site.data.keyword.Bluemix_notm}}.

    O painel do {{site.data.keyword.Bluemix_notm}} pode ser localizado em: [http://bluemix.net ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](http://bluemix.net "Ícone de link externo"){:new_window}.
    
	Após você efetuar login com o seu ID do usuário e senha, a UI do {{site.data.keyword.Bluemix_notm}} será aberta.

2. Clique em **Catálogo**. A lista de serviços disponíveis no {{site.data.keyword.Bluemix_notm}} será aberta.

3. Selecione a categoria do **DevOps** para filtrar a lista de serviços exibida.

4. Clique no azulejo **Monitoramento**.

5. Selecione um plano de serviço. Para coletar e acessar as métricas por até 45 dias e para definir as regras de alerta, incluindo as regras com curingas, selecione o plano **Premium**. Por padrão, o plano **Lite** é configurado, o que autoriza você para a coleta de métricas de plataforma no espaço em que estiver provisionando o serviço e um período de retenção de 15 dias para essas métricas. 

    Para obter mais informações sobre os planos de serviços, veja [Planos de serviços](/docs/services/cloud-monitoring/monitoring_ov.html#plans).
	
6. Clique em **Criar** para provisionar o serviço do {{site.data.keyword.monitoringshort}} no espaço do {{site.data.keyword.Bluemix_notm}} no qual você está com login efetuado.
  
 

## Fornecimento por meio da CLI
{: #cli}

Conclua as etapas a seguir para provisionar uma instância do serviço do {{site.data.keyword.monitoringshort}} no {{site.data.keyword.Bluemix_notm}} por meio da linha de comandos:

1. [Pré-requisito] Instale a CLI do {{site.data.keyword.Bluemix_notm}}.

   Para obter mais informações, veja [Instalando a CLI do {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa).
   
   Se a CLI estiver instalada, continue com a próxima etapa.
    
2. Efetue login em uma região, uma organização e um espaço do {{site.data.keyword.Bluemix_notm}}. Execute o
comando:

    ```
    cf login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    Siga as instruções. Insira suas credenciais do {{site.data.keyword.Bluemix_notm}}, selecione uma organização e um espaço.
	
3. Execute o comando `cf create-service` para provisionar uma instância.

    ```
	cf create-service service_name service_plan service_instance_name
	```
	{: codeblock}
	
	Em que
	
	* service_name é o nome do serviço, ou seja, **Monitoramento**.
	* service_plan é o nome do plano de serviço. Para coletar e acessar as métricas por até 45 dias e para definir as regras de alerta, incluindo as regras com curingas, selecione o plano **Premium**. Por padrão, o plano **Lite** é configurado, o que autoriza você para a coleta de métricas de plataforma no espaço em que estiver provisionando o serviço e um período de retenção de 15 dias para essas métricas. Para obter uma lista de planos, veja [planos de serviços do {{site.data.keyword.monitoringshort}}](/docs/services/cloud-monitoring/monitoring_ov.html#plan).
	* service_instance_name é o nome que você quer usar para a nova instância de serviço que criar.
	
	Para obter mais informações sobre o comando cf, veja [cf create-service](/docs/cli/reference/cfcommands/index.html#cf_create-service).

	Por exemplo, para criar uma instância do serviço do {{site.data.keyword.monitoringshort}} com um plano premium, execute o comando a seguir:
	
	```
	cf create-service Monitoring premium my_monitoring_svc
	```
	{
	
4. Verifique se o serviço foi criado com êxito. Execute o comando a seguir:

    ```	
	cf services
	```
	{: codeblock}
	
	A saída da execução do comando é semelhante a:
	
	```
    Getting services in org MyOrg / space MySpace as xxx@yyy.com...
    OK
    
    name                           service                  plan                   bound apps              last operation
    my_monitoring_svc              Monitoring               premium                                        create succeeded
	```
	{: screen}

	



