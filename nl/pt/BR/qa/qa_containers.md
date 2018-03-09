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



# FAQ para monitoramento de clusters do Kubernetes
{: #qa_containers}

Aqui estão as respostas para perguntas comuns sobre o serviço
do {{site.data.keyword.monitoringshort}} e o serviço do {{site.data.keyword.containershort_notm}}.
{:shortdesc}

* [A consulta do
Grafana para meus contêineres não está mostrando dados ou está com erro](/docs/services/cloud-monitoring/qa/qa_containers.html#metric_format_change)
* [Como configurar um ambiente em
cluster na minha sessão de terminal?](/docs/services/cloud-monitoring/qa/qa_containers.html#qa1)
* [Como localizar o ID
e o nome de um espaço que está associado a um cluster?](/docs/services/cloud-monitoring/qa/qa_containers.html#qa2)
* [Onde posso obter o nome do
cluster e o ID do cluster?](/docs/services/cloud-monitoring/qa/qa_containers.html#qa3)
* [Como obter a lista de
namespaces?](/docs/services/cloud-monitoring/qa/qa_containers.html#qa7)
* [Como obter a lista de pods em
um namespace em um cluster do Kubernetes?](/docs/services/cloud-monitoring/qa/qa_containers.html#qa8)
* [Como obter todos os pods em um
cluster por namespace?](/docs/services/cloud-monitoring/qa/qa_containers.html#qa9)

## A consulta do Grafana para meus contêineres não está mostrando dados ou está com erro
{: #metric_format_change}

O formato da consulta que pode ser definido para um contêiner mudou.

O formato a seguir foi descontinuado: 

```
{Prefix}.{Version}.{Provider}.{Type}.{ServiceName}.{Region}.{Account}.{Cluster}.{Metric}.{Container in a pod}.{Functions}
```
{: screen}

Os novos formatos que são válidos são os seguintes:

* [Formato de
consulta de métrica de CPU para um contêiner](/docs/services/cloud-monitoring/reference/metrics_format.html#cpu_containers)
* [Formato de
consulta de métrica de carregamento para um trabalhador](/docs/services/cloud-monitoring/reference/metrics_format.html#load_workers)
* [Formato
de consulta de métrica de memória para um contêiner](/docs/services/cloud-monitoring/reference/metrics_format.html#mem_containers)

Migre suas consultas antigas para o novo formato para visualizar seus dados no Grafana.

Por exemplo, se sua consulta inicia como
`crn.v1.bluemix.public.containers-kubernetes.us-south.`, deve-se migrar sua consulta para o
novo formato, ou seja, para `ibmcloud.public.containers-kubernetes.us-south`.

A tabela a seguir lista os campos no formato que foi descontinuado:

 <table>
      <caption>Tabela 1. Campos de consulta do Grafana para contêineres</caption>
      <tr>
        <th align="center">Campo</th>
        <th align="center">Descrição</th>
        <th align="center">Valores válidos</th>
      </tr>
      <tr>
        <td>Prefixo</td>
        <td>O prefixo que é usado para indicar que é um recurso do {{site.data.keyword.IBM_notm}} Cloud.</td>
        <td>`crn`</td>
      </tr>
      <tr>
        <td>Versão</td>
        <td>A versão que indica o formato e os campos dos dados de métrica coletados. </td>
        <td>`v1`</td>
      </tr>
      <tr>
        <td>Provedor</td>
        <td>Provedor em nuvem no qual os dados são coletados. Este campo é um identificador alfanumérico.</td>
        <td>Para o {{site.data.keyword.IBM_notm}} Cloud público, o valor é: `bluemix`</td>
      </tr>
      <tr>
        <td>Tipo</td>
        <td>Ambiente de nuvem no qual os dados são coletados.</td>
        <td>Para o {{site.data.keyword.IBM_notm}} Cloud público, o valor é: `public`</td>
      </tr>
      <tr>
        <td>Nome do serviço</td>
        <td>Infraestrutura do Cloud onde métricas são coletadas.</td>
        <td>Para o serviço {{site.data.keyword.monitoringshort}}, o valor é: `containers-kubernetes`</td>
      </tr>
      <tr>
        <td>Região</td>
        <td>Região Cloud onde métricas são coletadas.</td>
        <td>Para a região Sul dos EUA, o valor é: `ng` <br>Para a região do Reino Unido, o valor é: `eu-gb` <br>Para Alemanha, o valor é: `eu-de` <br>Para Sydney, o valor é: `au-syd` </td>
      </tr>
      <tr>
        <td>Conta</td>
        <td>GUID da conta em que as métricas são coletadas. <br>O formato deste campo é o seguinte: `a_ID` em que ID é o GUID da conta. <br>Para obter o GUID da conta, consulte [Como obter o GUID de uma conta](/docs/services/cloud-monitoring/qa/cli_qa.html#account_guid).</td>
        <td>a_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx</td>
      </tr>
      <tr>
        <td>Grupo</td>
        <td>GUID do cluster em que métricas são coletadas.</td>
        <td>xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx</td>
      </tr>
      <tr>
        <td>de Métrica</td>
        <td>A métrica que é coletada automaticamente para um contêiner.</td>
        <td>Para obter uma lista de métricas da CPU, consulte
[Métricas da CPU para contêineres](/docs/services/cloud-monitoring/containers/monitoring_containers_ov.html#cpu_metrics_containers)<br>Para obter uma lista de métricas da memória, consulte
[Métricas da memória](/docs/services/cloud-monitoring/containers/monitoring_containers_ov.html#memory_metrics) </td>
      </tr>
      <tr>
        <td>Contêiner em uma vagem</td>
        <td>A combinação de nomes e GUIDs de recursos do Kubernetes que são necessários para identificar exclusivamente um contêiner que é executado em um pod. <br>
**Nota:** ao observar a lista de opções disponíveis para essa entrada na
consulta, observe que há também uma entrada com o formato a seguir: {namespace}{pod_name}{deploymentname_name}_POD{container_ID}.
Essas entradas correspondem aos IDs de contêineres internos que são criados pelo Kubernetes.</td>
		<td>{namespace}_{pod_name}_{deployment_name}_{container_id}</td>
      </tr>
      <tr>
        <td>Funções</td>
        <td>Funções de consulta que podem ser selecionadas para visualizar uma métrica de contêiner no painel. <br>Para obter mais informações, consulte [Funções ![(Ícone de link externo)](../../icons/launch-glyph.svg "Ícone de link externo")](http://graphite.readthedocs.io/en/latest/functions.html){: new_window}</td>
        <td></td>
      </tr>
    </table>


## Como configurar um ambiente em cluster na minha sessão de terminal?
{: #qa1}

Deve-se configurar o contexto de um cluster do Kubernetes para gerenciá-lo usando comandos.

**Nota:** para gerenciar um cluster, é necessária uma política do IAM para o
serviço do {{site.data.keyword.containershort_notm}} designado para o seu usuário com permissões
para concluir a tarefa.

Conclua as etapas a seguir para configurar o contexto de um cluster:

1. Efetue login na região, na organização e no espaço no {{site.data.keyword.Bluemix_notm}} que está associado ao cluster criado. Para obter mais informações, veja [Como efetuar login no {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#login).

2. Inicialize o plug-in do serviço {{site.data.keyword.containershort_notm}}. Execute o seguinte comando:

	```
	bx cs init
	```
	{: codeblock}

3. Configure o contexto do cluster na sessão de terminal. Execute os seguintes comandos:
    
	```
	bx cs cluster-config MyCluster
	```
	{: codeblock}

    A saída de execução desse comando fornece o comando que deve ser executado no terminal para configurar o caminho para o arquivo de configuração. Por exemplo:

	```
	export KUBECONFIG=/Users/ibm/.bluemix/plugins/container-service/clusters/MyCluster/kube-config-hou02-MyCluster.yml
	```
	{: codeblock}

    Copie e cole o comando para configurar a variável de ambiente em seu terminal e, em seguida, pressione **Enter**.

 
 
## Como posso localizar o ID e o nome de um espaço que está associado a um cluster?
{: #qa2}

Quando um cluster é criado em uma conta do {{site.data.keyword.Bluemix_notm}}, as métricas são
associadas a um espaço dentro dessa conta. Ao criar consultas para visualizar métricas do cluster, um ID do
espaço é necessário.
{:shortdesc}

**Nota:** para gerenciar um cluster, é necessária uma política do IAM para o serviço
do {{site.data.keyword.containershort_notm}} designado para o seu usuário com permissões para concluir
a tarefa.

Para localizar o ID do espaço para um cluster, conclua as etapas a seguir:

1. Efetue login na região, na organização e no espaço no {{site.data.keyword.Bluemix_notm}} que está associado ao cluster criado. Para obter mais informações, veja [Como efetuar login no {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#login).

2. Inicialize o plug-in do serviço {{site.data.keyword.containershort_notm}}. Execute o seguinte comando:

	```
	bx cs init
	```
	{: codeblock}

3. Obtenha os detalhes do cluster. Execute o seguinte comando:

    ```
    bx cs cluster-get cluster-name
    ```
    {: codeblock}

    em que **cluster-name** é o nome do cluster.

    O ID do espaço é o valor indicado para o campo **Espaço de log**.

    Por exemplo, a saída da execução do comando é a seguinte:

    ```
    Name:			    cluster-name
    ID:			        c213f81296db4c68b84e212c19135a99
    State:			    normal
    Created:		    2017-08-22T18:18:59+0000
    Datacenter:		    dal10
    Master URL:		    https://169.46.7.242:21210
    Ingress subdomain:  cluster-name.us-south.containers.mybluemix.net
    Ingress secret:     cluster-name
    Workers:		    5
    Log Space:		    fa277ff8-8a73-324b-9b75-0f11d54a3ae2
    ```
    {: screen}

4. Obtenha o nome do espaço.

    Listar todos os nomes de espaço:
	
    ```
	bx account spaces
	```
    {: codeblock}
	
	Run the following command for each space until you find the name of the matching ID:
	 	```
	bx iam space Spacename --guid
	```
	{: codeblock}
	
	


## Where can I get the cluster name and the cluster ID?
{: #qa3}

Complete the following steps to get the cluster name and ID through the UI:

1. Log in to your {{site.data.keyword.Bluemix_notm}} account.

    The {{site.data.keyword.Bluemix_notm}} dashboard can be found at: [http://bluemix.net ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://bluemix.net){:new_window}.

	Depois de efetuar login com seu ID de usuário e senha, a UI do {{site.data.keyword.Bluemix_notm}} é aberta.

2. From the {{site.data.keyword.Bluemix_notm}} *Dashboard*, select **Menu > Containers**.

    The list of clusters that are available in the account are displayed.

3. To get the cluster ID, select a cluster entry.

    The overview page opens. In this page, you can get the cluster ID.



Complete the following steps to get the cluster name and ID thorugh the command line:

1. Log in to the region, organization, and space in the {{site.data.keyword.Bluemix_notm}} that is associated with the cluster that you created. Para obter mais informações, consulte [Como efetuar login no {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#login).
2. List the clusters that are available in the account. Execute o comando a seguir:

    ```
	bx cs clusters
	``` 
	{: codeblock}
	
	The output of this command lists all the clusters in the account with their corresponding IDs.
	
3. Para obter o ID do cluster, execute o comando a seguir:

    ```
	bx cs cluster-get my_cluster
	```
    {: screen}	



## How to get the list of namespaces?
{: #qa7}

To get a list of all the namespaces in your cluster, complete the following steps:

Conclua as etapas a seguir:

1. Set up the cluster context. For more information, see [How do I set up a cluster environment in my terminal session?](/docs/services/cloud-monitoring/qa/qa_containers.html#qa1).

2. List all the namespaces. Run the following kubectl command:

    ```
    kubectl get namespaces
	```
	{: codeblock}

## How to get the list of pods per namespace in a Kubernetes cluster?
{: #qa8}
		
To get the list of pods in a namespace, complete the following steps:

1. Set up the cluster context. For more information, see [How do I set up a cluster environment in my terminal session?](/docs/services/cloud-monitoring/qa/qa_containers.html#qa1).

2. To get the list of pods per namespace in a Kubernetes cluster, run the following command:

    ```
	kubectl --namespace=NamespaceName get pods 
	```
	{: codeblock}
	
	where Namespacename is the name of the namespace, for example, *default*.

## How to get all the pods in a cluster by namespace?
{: #qa9}
		
Conclua as etapas a seguir:
1. Set up the cluster context. For more information, see [How do I set up a cluster environment in my terminal session?](/docs/services/cloud-monitoring/qa/qa_containers.html#qa1).
	
2. To get all the pods in a cluster by namespace, run the following command:

	```
	kubectl get pods --all-namespaces
	```
	{: codeblock}
		


