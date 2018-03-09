---

copyright:
  years: 2017, 2018

lastupdated: "2018-02-01"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Enviando dados usando o plug-in de Monitoramento (collectd)
{: #conf_monitoring_plugin}

É possível configurar o collectd para coletar métricas em um ambiente. Use o plug-in do {{site.data.keyword.monitoringshort}} para enviar essas métricas para um domínio do espaço de seu ambiente. 
{: shortdesc}

A figura a seguir mostra uma visualização de alto nível de como usar o plug-in do {{site.data.keyword.monitoringshort}} para enviar métricas para o serviço do {{site.data.keyword.monitoringshort}}:

![High level view on how to use the {{site.data.keyword.monitoringshort}} plugin](images/monitoring_plugin_ov.png "High level view on how to use the {{site.data.keyword.monitoringshort}} plugin")



## Instalar o plug-in de Monitoramento
{: #install}

Em um terminal, conclua as etapas a seguir: 

1. (Pré-requisito) Instale a CLI do {{site.data.keyword.Bluemix_notm}}.

   Para obter mais informações, veja [Instalando a CLI do {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa).
   
   Se a CLI estiver instalada, continue com a próxima etapa.
	
2. Efetue login em uma região, uma organização e um espaço no {{site.data.keyword.Bluemix_notm}}. 

    Para obter mais informações, veja [Como efetuar login no {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#login).

    Por exemplo, para efetuar login na região sul dos EUA, execute o comando a seguir:
	
	```
    bx login -a https://api.ng.bluemix.net
	bx target -o MyOrg -s MySpace
    ```
    {: codeblock}

    Siga as instruções. Insira suas credenciais do {{site.data.keyword.Bluemix_notm}}, selecione uma organização e um espaço.

### Etapa 1: Instalar o collectd
{: #collectd}

Como administrador, conclua as etapas a seguir para instalar o collectd:

1.	Efetue login como usuário raiz. 

    ```
    sudo -s
    ```
    {: codeblock}

2.	Instale o pacote do Network Time Protocol (NTP) para sincronizar o horário dos logs. 

    Por exemplo, para um sistema Ubunutu, veja se `timedatectl status` mostra *Network time on: yes*. Em caso positivo, o sistema Ubuntu já está configurado para usar ntp e será possível ignorar esta etapa.
    
    ```
    # timedatectl status
    Local time: Mon 2017-06-12 03:01:22 PDT
    Universal time: Mon 2017-06-12 10:01:22 UTC
    RTC time: Mon 2017-06-12 10:01:22
    Time zone: America/Los_Angeles (PDT, -0700)
    Network time on: yes
    NTP synchronized: yes
    RTC in local TZ: no
    ```
    {: screen}
    
    Conclua as etapas a seguir para instalar ntp em um sistema Ubuntu:

    1.	Execute o comando a seguir para atualizar os pacotes. 

        ```
        apt-get update
        ```
        {: codeblock}
        
    2.	Execute o comando a seguir para instalar o pacote ntp. 

        ```
        apt-get install ntp
        ```
        {: codeblock}
        
    3.	Execute o comando a seguir para instalar o pacote ntpdate. 
    
        ```
        apt-get install ntpdate
        ```
        {: codeblock}
        
    4.	Execute o comando a seguir para parar o serviço 
        
        ```
        service ntp stop
        ```
        {: codeblock}
        
    5.	Execute o comando a seguir para sincronizar o relógio do sistema. 
    
        ```
        ntpdate -u 0.ubuntu.pool.ntp.org
        ```
        {: codeblock}
        
        O resultado confirma que o horário está ajustado, por exemplo:
        
        ```
        4 May 19:02:17 ntpdate[5732]: adjust time server 50.116.55.65 offset 0.000685 sec
        ```
        {: screen}
        
    6.	Execute o comando a seguir para iniciar o ntpd novamente. 
    
        ```
        service ntp start
        ```
        {: codeblock}
    
        O resultado confirma que o serviço está sendo iniciado.

3. Instale o collectd. Execute o seguinte comando:

    ```
	apt-get install collectd 
	```
	{: codeblock}


### Etapa 2: instale o plug-in do Monitoramento
{: #plugin}

Conclua as etapas a seguir para instalar o plug-in do {{site.data.keyword.monitoringshort}}:

1. 	Efetue login como usuário raiz. 

    ```
    sudo -s
    ```
    {: codeblock}
	
2. Inclua o repositório de serviço do {{site.data.keyword.monitoringshort}}. Execute o comando a seguir:

    ```
    wget -O - https://downloads.opvis.bluemix.net/client/IBM_Logmet_repo_install.sh | bash
    ```
   {: codeblock}
   
3. Instale o plug-in. Execute o comando a seguir:

    ```
	apt-get install ibmcloud-monitoring
	```
	{: codeblock}


## Configure o plug-in de Monitoramento
{: #configure}

Para configurar o plug-in do {{site.data.keyword.monitoringshort}}, conclua as etapas a seguir:
	
### Etapa 1: designe uma política do IAM ao usuário
{: #iam_policy}

Para enviar métricas para qualquer domínio, deve-se conceder a seu ID do usuário uma função do IAM com permissões para enviar métricas para o serviço de Monitoramento.
 
* As permissões são configuradas designando uma política do IAM para esse usuário no IBM Cloud. 
* As funções do IAM a seguir permitem que um usuário envie métricas: *Administrador*, *Editor*, *Operador*. 

Para designar um usuário a uma política do IAM, escolha um dos métodos a seguir:

* Through the {{site.data.keyword.Bluemix_notm}} UI: For more information, see [Assigning a user an IAM policy through the {{site.data.keyword.Bluemix_notm}} UI ](/docs/services/cloud-monitoring/security/assign_policy.html#assign_policy_ui).
* Usando a linha de comandos: para obter mais informações, veja [Designando a um usuário uma política do IAM usando a linha de comandos](/docs/services/cloud-monitoring/security/assign_policy.html#assign_policy_commandline).
 
### Etapa 2: obtenha uma chave API
{: #api_key}
 
Para enviar métricas para um espaço, deve-se obter uma chave API para autenticar com o serviço {{site.data.keyword.monitoringshort}}.

Para obter mais informações sobre como obter uma chave API, veja [Obtendo uma chave API](/docs/services/cloud-monitoring/security/auth_api_key.html#auth_api_key).
	
From the same terminal where you logged in to the {{site.data.keyword.Bluemix_notm}}, set the APIKEY variable for the token.

Por exemplo,

```
export APIKEY="kjshdgf.....ldkdjdj"
```
{: screen}
	
### Etapa 3: obtenha informações sobre o terminal
{: #endpoint}

Para determinar o terminal do {{site.data.keyword.monitoringshort}} em que você enviará as métricas, veja a lista de terminais por região, veja [Terminais](/docs/services/cloud-monitoring/send_retrieve_metrics_ov.html#endpoints) e identifique aquele para a região em que deseja enviar métricas.

From the same terminal where you logged in to the {{site.data.keyword.Bluemix_notm}}, set the **METRIC_ENDPOINT** variable. Por exemplo,

```
export METRIC_ENDPOINT="metrics.ng.bluemix.net"
```
{: screen}



### Etapa 4: obtenha informações sobre o ID do domínio do espaço
{: #domain}

Para obter o ID do domínio para um espaço, [obtenha o GUID do espaço](/docs/services/cloud-monitoring/qa/cli_qa.html#space_guid). Em seguida, configure o ID do domínio conforme a seguir: `s-SpaceID` em que SpaceID é o GUID do espaço.

From the same terminal where you logged in to the {{site.data.keyword.Bluemix_notm}}, set the SpaceID variable:

```
export SpaceID="kjshdgf.....ldkdjdj"
```
{: screen}



### Etapa 5: execute o script de configuração
{: #script}

Execute o script a seguir para configurar o plug-in do {{site.data.keyword.monitoringshort}} para enviar métricas para um espaço:

```
/opt/ibmcloud_monitoring/configure -e $METRIC_ENDPOINT -a $APIKEY -s s-$SpaceID
```
{: codeblock}


O script inclui automaticamente a sub-rotina **Include** no arquivo collectd.conf principal:

```
<LoadPlugin IBMCloudMonitoring>
   FlushInterval 60
</LoadPlugin>
<Plugin IBMCloudMonitoring>
  <Endpoint "ng">
     Host "metrics.ng.bluemix.net"
     Port 9095
     ApiKey "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
     SkipInternalPrefixForStatsd false
     RateCounter false
     ScopeId "s-cedc73c5-6d55-4193-a9de-378620d6fab5"
  </Endpoint>
</Plugin>
```
{: screen}


### Etapa 6: reinicie o daemon collectd
{: #restart}

Conclua as etapas a seguir:

1. 	Efetue login como usuário raiz. 

    ```
    sudo -s
    ```
    {: codeblock}
	
2.  Reinicie o collectd.

   Se o daemon collectd foi incluído como um serviço, execute o comando a seguir:
	
	```
	service collectd restart
	```
	{: codeblock}

As métricas que são ativadas em collectd são aquelas que estão disponíveis para análise no Grafana.


