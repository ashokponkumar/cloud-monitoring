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


# Analisar métricas no Grafana para um app que é implementado em um cluster do Kubernetes
{: #container_service_metrics}

Use este tutorial para aprender como usar o serviço do {{site.data.keyword.monitoringlong}} para monitorar o desempenho de seu contêiner.
{:shortdesc}


## Objetivos
{: #objectives}

Aprenda como procurar e analisar métricas do contêiner para um app que é implementado em um cluster do Kubernetes:

1. Identifique o local em que as métricas que são coletadas em um cluster são encaminhadas para o serviço do {{site.data.keyword.monitoringshort}}. 
2. Ative o Grafana e configure o domínio {{site.data.keyword.monitoringshort} no qual é possível visualizar as métricas do cluster.
3. Procure por e analise métricas do contêiner para um app que é implementado em um cluster do Kubernetes no {{site.data.keyword.Bluemix_notm}}.

Este tutorial percorre as etapas que são necessárias para que o cenário de ponta a ponta a seguir funcione no {{site.data.keyword.Bluemix_notm}}: Fornecendo um cluster, identificando o local para o qual o cluster envia métricas para o serviço do {{site.data.keyword.monitoringshort}} no {{site.data.keyword.Bluemix_notm}}, implementando um app no cluster e usando o Grafana para visualizar e filtrar métricas do contêiner para esse cluster.


**Nota:** para concluir este tutorial, deve-se concluir os pré-requisitos e os tutoriais que são vinculados nas diferentes etapas.


## Pré-requisitos
{: #prereqs}

1. Ser membro ou proprietário de uma conta do {{site.data.keyword.Bluemix_notm}} com permissões para criar clusters padrão do Kubernetes, implementar apps nos clusters e consultar as métricas no {{site.data.keyword.Bluemix_notm}} para monitoramento no Grafana.

    Seu ID de usuário do {{site.data.keyword.Bluemix_notm}} deve ter as políticas a seguir designadas:
    
    * Uma política do IAM para o {{site.data.keyword.containershort}} com as permissões *operator* ou *administrator*.
    * Uma função do CF para o espaço no qual o serviço do {{site.data.keyword.monitoringshort}} é fornecido com permissões *developer*.
    
    Para obter mais informações, consulte [Designar uma política do IAM a um usuário por meio da UI do IBM Cloud](/docs/services/cloud-monitoring/security/grant_permissions.html#grant_permissions_ui_account) e [Concedendo a um usuário uma função do CF usando a UI do IBM Cloud](/docs/services/cloud-monitoring/security/grant_permissions.html#grant_permissions_ui_space).

2. Ter uma sessão de terminal por meio da qual é possível gerenciar o cluster do Kubernetes e implementar apps na linha de comandos. Os exemplos neste tutorial são fornecidos para um sistema Ubuntu Linux.

3. Instale as CLIs para que funcionem com o {{site.data.keyword.containershort}} no seu sistema Ubuntu.

    * Instale a CLI do {{site.data.keyword.Bluemix_notm}}. Para obter mais informações, consulte [Instalando a CLI do {{site.data.keyword.Bluemix_notm}}](/docs/cli/reference/bluemix_cli/download_cli.html#download_install).
    
    * Instale a CLI do {{site.data.keyword.containershort}} para criar e gerenciar seus clusters do Kubernetes no {{site.data.keyword.containershort}} e para implementar apps conteinerizados em seu cluster. Para obter mais informações, consulte [Instalar o plug-in CS](/docs/containers/cs_cli_install.html#cs_cli_install_steps).
    

    
 

## Etapa 1: Fornecer um cluster do Kubernetes
{: #step1}

Conclua as etapas a seguir:
1. Crie um cluster padrão do Kubernetes.

   * [Crie um cluster padrão do Kubernetes por meio da UI](/docs/containers/cs_cluster.html#cs_cluster_ui).
   * [Crie um cluster padrão do Kubernetes usando a CLI](/docs/containers/cs_cluster.html#cs_cluster_cli).

2. Configure o contexto do cluster em um terminal. Após a configuração do contexto, é possível gerenciar o cluster do Kubernetes e implementar o aplicativo no cluster do Kubernetes.

    Efetue login na região, organização e espaço no {{site.data.keyword.Bluemix_notm}} que está associado ao cluster que você criou. Para obter mais informações, consulte [Como efetuar login no {{site.data.keyword.Bluemix_notm}}](/docs/services/CloudLogAnalysis/qa/cli_qa.html#login).

	Inicialize o plug-in de serviço do {{site.data.keyword.containershort}}.

	```
	bx cs init
	```
	{: codeblock}

    Configure seu contexto de terminal para o cluster.
    
	```
	bx cs cluster-config MyCluster
	```
	{: codeblock}

    A saída da execução desse comando fornece o comando que deve ser executado em seu terminal para configurar o caminho para seu arquivo de configuração. Por exemplo:

	```
	export KUBECONFIG=/Users/ibm/.bluemix/plugins/container-service/clusters/MyCluster/kube-config-hou02-MyCluster.yml
	```
	{: codeblock}

    Copie e cole o comando para configurar a variável de ambiente em seu terminal e, em seguida, pressione **Enter**.



## Etapa 2: Identificar o local para o qual o cluster encaminha métricas para o serviço do {{site.data.keyword.monitoringshort}}
{: #step2}

Um cluster é um recurso de nível de conta. Ao fornecer um cluster no {{site.data.keyword.containershort}}, os clusters podem ser criados no nível de conta ou podem ser criados com um espaço do Cloud Foundry (CF) associado a ele. Assim que o cluster é fornecido e está pronto, as métricas são coletadas e encaminhadas automaticamente para o serviço do {{site.data.keyword.monitoringshort}}.

* Os clusters que possuem um espaço do CF associado encaminham métricas para o domínio de métricas de espaço.
* Os clusters que são criados no nível de conta encaminham métricas para o domínio de métricas de contas.

Para identificar o local para o qual seu cluster encaminha métricas, execute o comando a seguir:

```
$ bx cs cluster-get ClusterName --json
```
{: codeblock}

em que *ClusterName* é o nome do cluster.

Na saída, os campos a seguir fornecem as informações sobre o local para o qual as métricas são encaminhadas:

* **logOrg** define o ID de uma organização do CF.
* **logOrgName** define o nome de uma organização do CF.
* **logSpace** define o ID de um espaço do CF.
* **logSpaceName** define o nome de um espaço do CF.

Se os campos estiverem vazios, as métricas serão encaminhadas para o domínio de contas.
Se os campos tiverem uma organização do CF e um conjunto de espaços do CF, as métricas serão encaminhadas para o domínio de espaço associado a esse espaço.

Por exemplo, a saída para um cluster que encaminha métricas para o domínio de contas é semelhante ao seguinte:

```
$ bx cs cluster-get MyDemoCluster --json
{
    "id": "f9adabcjhefg745746hgfjbnkdnfsks",
    "name": "MyDemoCluster",
    "region": "eu-gb",
    "dataCenter": "lon02",
    "location": "eu-gb-lon02",
    "serverURL": "https://xxx.xxx.xxx.x:xxxxx",
    "state": "normal",
    "createdDate": "2018-01-30T17:41:14+0000",
    "modifiedDate": "2018-01-30T17:41:14+0000",
    "workerCount": 2,
    "isPaid": true,
    "masterKubeVersion": "1.8.6_1505",
    "targetVersion": "1.8.6_1505",
    "ingressHostname": "mydemocluster.uk-south.containers.mybluemix.net",
    "ingressSecretName": "mydemocluster",
    "ownerEmail": "xxxx@uibm.com",
    "logOrg": "",
    "logOrgName": "",
    "logSpace": "",
    "logSpaceName": "",
    "monitoringURL": "https://metrics.eu-gb.bluemix.net/app/#/grafana4/dashboard/db/a-siuhfieuhf7346586hfrhf_ClusterMonitoringDashboard_v1?scopeId=a-siuhfieuhf7346586hfrhf\u0026?var-Account_ID=a_siuhfieuhf7346586hfrhf\u0026var-Cluster=MyDemoCluster\u0026var-Namespace=default\u0026var-Pod_ID=All",
    "addons": [
        {
            "name": "customer-storage-pod",
            "enabled": true
        },
        {
            "name": "basic-ingress-v2",
            "enabled": true
        },
        {
            "name": "storage-watcher-pod",
            "enabled": true
        }
    ],
    "vlans": null
}
```
{: screen}





## Etapa 3: Conceder permissões de usuário para ver métricas no domínio de métricas
{: #step3}

Para conceder a um usuário permissões para visualizar métricas em um domínio de espaço, deve-se designar a esse usuário uma função do CF que descreva as ações que esse usuário pode executar com o serviço do {{site.data.keyword.monitoringshort}} no espaço.

Para conceder permissões a um usuário para visualizar métricas em um domínio de contas, deve-se designar a esse usuário uma política do IAM que descreva as ações que esse usuário pode executar com o serviço {{site.data.keyword.monitoringshort}}.

### Conceder ao usuário permissões para ver métricas em um domínio de espaço
{: #space}

Conclua as etapas a seguir para conceder a um usuário permissões para trabalhar com o serviço do {{site.data.keyword.monitoringshort}}:

1. Efetue login no console do {{site.data.keyword.Bluemix_notm}}.

    Abra um navegador da web e ative o painel do {{site.data.keyword.Bluemix_notm}}: [http://bluemix.net ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](http://bluemix.net){:new_window} 	

	Depois que você efetua login com seu ID do usuário e senha, a UI do {{site.data.keyword.Bluemix_notm}} é aberta.

2. Na barra de menus, clique em **Gerenciar > Conta > Usuários**.

    A janela *Usuários* exibe uma lista de usuários com seus endereços de e-mail para a conta selecionada atualmente. 	

3. Se o usuário for membro da conta, selecione o nome do usuário na lista ou clique em **Gerenciar usuário** no menu *Ações*.

    Se o usuário não for membro da conta, consulte [Convidando usuários](/docs/iam/iamuserinv.html#iamuserinv).

4. Selecione **Acesso ao Cloud Foundry** e, em seguida, selecione **Designar organização**.

5. Insira os valores a seguir:

    <table>
      <caption></caption>
      <tr>
        <th>Campo</th>
        <th>Valor</th>
      </tr>
      <tr>
        <td>Organização</td>
        <td>MyOrg</td>
      </tr>
      <tr>
        <td>Função de organização</td>
        <td>Nenhuma função de organização</td>
      </tr>
      <tr>
        <td>Região</td>
        <td>US South</td>
      </tr>
      <tr>
        <td>Espaço</td>
        <td>dev</td>
      </tr>
      <tr>
        <td>Função de espaço</td>
        <td>Auditor</td>
      </tr>
	
6. Clique em **Salvar função**.

### Conceder ao usuário permissões para ver métricas no domínio de contas
{: #acc}

Conclua as etapas a seguir para conceder a um usuário permissões para trabalhar com o serviço do {{site.data.keyword.monitoringshort}}:

1. Efetue login no console do {{site.data.keyword.Bluemix_notm}}.

    Abra um navegador da web e ative o painel do {{site.data.keyword.Bluemix_notm}}: [http://bluemix.net ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](http://bluemix.net){:new_window} 	

	Depois que você efetua login com seu ID do usuário e senha, a UI do {{site.data.keyword.Bluemix_notm}} é aberta.

2. Na barra de menus, clique em **Gerenciar > Conta > Usuários**.

    A janela *Usuários* exibe uma lista de usuários com seus endereços de e-mail para a conta selecionada atualmente. 	

3. Se o usuário for membro da conta, selecione o nome do usuário na lista ou clique em **Gerenciar usuário** no menu *Ações*.

    Se o usuário não for membro da conta, consulte [Convidando usuários](/docs/iam/iamuserinv.html#iamuserinv).

4. Selecione **Políticas de acesso > Designar acesso > Designar acesso a recursos**.

5. Escolha o serviço do **{{site.data.keyword.monitoringlong}}**, selecione a região em que o cluster está disponível, **Sul dos EUA** para este tutorial e selecione uma função, **visualizador**.



## Etapa 4: Conceder permissões de proprietário da chave ao {{site.data.keyword.containershort_notm}}
{: #step4}

Quando o cluster encaminha métricas para um domínio de espaço, deve-se também conceder permissões do Cloud Foundry (CF) para o proprietário da chave do {{site.data.keyword.containershort}} na organização e no espaço. O proprietário da chave precisa da função *orgManager* para a organização e *SpaceManager* e *Developer* para o espaço.

Quando o cluster encaminha métricas para o domínio de contas, o proprietário da chave do {{site.data.keyword.containershort}} deve ter uma política do IAM com permissões de administrador para o serviço do {{site.data.keyword.monitoringshort}}.

### Conceder permissões para ver métricas em um domínio de espaço
{: #space_1}

Conceda ao ID do usuário proprietário da chave do {{site.data.keyword.containershort}} as permissões a seguir: função *orgManager* para a organização e *SpaceManager* e *Developer* para o espaço. Conclua as etapas a seguir:
    
1. Efetue login no console do {{site.data.keyword.Bluemix_notm}}.
    Abra um navegador da web e ative o painel do {{site.data.keyword.Bluemix_notm}}: [http://bluemix.net ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](http://bluemix.net){:new_window} 	

	Depois que você efetua login com seu ID do usuário e senha, a UI do {{site.data.keyword.Bluemix_notm}} é aberta.

2. Na barra de menus, clique em **Gerenciar > Conta > Usuários**.

    A janela *Usuários* exibe uma lista de usuários com seus endereços de e-mail para a conta selecionada atualmente. 	

3. Localize o ID do usuário do proprietário da chave do {{site.data.keyword.containershort}}.

    Execute o comando `bx cs api-key-info ClusterName` para obter o ID do usuário do proprietário da chave do {{site.data.keyword.containershort}}.

4. Selecione **Acesso ao Cloud Foundry** e, em seguida, selecione **Designar uma organização**.

5. Digite os seguintes valores: 

    <table>
      <caption></caption>
      <tr>
        <th>Campo</th>
        <th>Valor</th>
      </tr>
      <tr>
        <td>Organização</td>
        <td>MyOrg</td>
      </tr>
      <tr>
        <td>Função de organização</td>
        <td>Manager</td>
      </tr>
      <tr>
        <td>Região</td>
        <td>Sul dos Estados Unidos</td>
      </tr>
      <tr>
        <td>Espaço</td>
        <td>dev</td>
      </tr>
      <tr>
        <td>Função de espaço</td>
        <td>Developer</td>
      </tr>
	
6. Clique em **Salvar função**.


### Conceder permissões para ver métricas no domínio de contas
{: #acc_1}

Conclua as etapas a seguir:

1. Efetue login no console do {{site.data.keyword.Bluemix_notm}}.

    Abra um navegador da web e ative o painel do {{site.data.keyword.Bluemix_notm}}: [http://bluemix.net ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](http://bluemix.net){:new_window}
	
	Depois de efetuar login com seu ID de usuário e senha, a UI do {{site.data.keyword.Bluemix_notm}} é aberta.

2. Na barra de menus, clique em **Gerenciar > Conta > Usuários**. 

    A janela *Usuários* exibe uma lista de usuários com seus endereços de e-mail para a conta selecionada atualmente.
	
3. Localize o ID do proprietário da chave do {{site.data.keyword.containershort}}.

    Execute o comando `bx cs api-key-info ClusterName` para obter o ID do proprietário da chave do {{site.data.keyword.containershort}}.

4. Selecione **Políticas de acesso > Designar acesso > Designar acesso a recursos**.

5. Escolha o serviço do **{{site.data.keyword.monitoringlong}}**, selecione a região em que o cluster está disponível, **Sul dos EUA** para este tutorial e selecione uma função, **administrador**.	

## Etapa 5: Implementar um app de amostra no cluster do Kubernetes
{: #step5}

Implemente e execute um aplicativo de amostra no cluster do Kubernetes. Conclua as etapas no tutorial a seguir para implementar o app de amostra: [Lição 1: Implementando apps de instância única em clusters do Kubernetes](/docs/containers/cs_tutorials_apps.html#cs_apps_tutorial_lesson1).

O app é um app Hello World Node.js:

```
var express = require('express')
    var app = express()

app.get('/', function(req, res) {
  res.send('Hello world! Your app is up and running in a cluster!\n')
})
app.listen(8080, function() {
  console.log('Sample app is listening on port 8080.')
})
```
{: screen}

Nesse aplicativo de amostra, quando você testa seu app em um navegador, o app grava no stdout a mensagem a seguir: `Sample app is listening on port 8080.`


## Etapa 6: Ativar o Grafana e configurar o domínio de métricas
{: #step6}

Ative o Grafana em um navegador e configure o domínio do {{site.data.keyword.monitoringshort}} no qual é possível visualizar as métricas do cluster.

Para analisar métricas para um cluster, deve-se acessar o Grafana na região Pública de nuvem na qual o cluster estiver criado. Para obter mais informações, veja [Navegando para o painel do Grafana por meio de um navegador da web](/docs/services/cloud-monitoring/grafana/navigating_grafana.html#launch_grafana_from_browser).

1. Em um navegador, ative o Grafana. 

    Insira a URL de serviço do {{site.data.keyword.monitoringshort}} para a região em que você criou o cluster. 
    
    Para obter as URLs por região, consulte [URLs para o serviço de monitoramento](/docs/services/cloud-monitoring/monitoring_ov.html#region).

    Por exemplo, para a região Sul dos EUA, ative: [https://metrics.ng.bluemix.net/](https://metrics.ng.bluemix.net/).

2. Configure o domínio do {{site.data.keyword.monitoringshort} no qual é possível visualizar as métricas do cluster.

    No Grafana, selecione seu ID. Em seguida, verifique se você está na conta correta e escolha um domínio.

    Os clusters que possuem um espaço do CF associado encaminham as métricas para o domínio de métricas de espaço. Selecione `Domain = space` e a organização e o espaço que estão associados ao seu cluster.

    Os clusters que são criados no nível de conta encaminham métricas para o domínio de métricas de contas. Selecione `Domain = account`

## Etapa 7: Monitorar o cluster no Grafana
{: #step7}

O {{site.data.keyword.containershort}} fornece um painel do Grafana que pode ser usado para monitorar suas métricas do cluster. 

Conclua as etapas a seguir para abrir o painel de amostra:

1. Selecione a alternância de barra de menus lateral ![Barra de menus lateral do Grafana](images/grafana_settings.gif "Barra de menus lateral do Grafana").
2. Selecione **Painéis**.
3. Clique em **Abrir**.
4. Selecione **ClusterMonitoringDashboard_v1**.

O painel de amostra é aberto. 

![Painel de amostra do Grafana](images/cluster_grafana_sample_dashboard.png "Painel de amostra do Grafana")



## Etapas Seguintes
{: #next_steps}

Defina um alerta para uma métrica. Para obter mais informações, consulte [Configurando alertas](/docs/services/cloud-monitoring/config_alerts_ov.html#config_alerts_ov).
