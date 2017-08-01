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


# Perguntas e respostas frequentes ao usar o Grafana
{: #grafana_qa}

Veja as respostas para as perguntas comuns sobre como usar o Grafana com o serviço {{site.data.keyword.monitoringshort}}. 
{:shortdesc}

* [Aparece um erro 404 quando efetuo login na UI da web do serviço Monitoring](/docs/services/cloud-monitoring/qa/grafana_qa.html#404)
* [Acabei de transferir dados do json por upload para um painel do Grafana. Por que perdi minha barra de rolagem?](/docs/services/cloud-monitoring/qa/grafana_qa.html#2)


## Aparece um erro 404 quando efetuo login na UI da web do serviço Monitoring usando o modelo de autenticação do UUA
{: #404}

Se aparecer um erro 404 ao tentar efetuar login na UI da web do serviço {{site.data.keyword.monitoringshort}} (Grafana), verifique se existe espaço e se você tem acesso a ela.

Quando você usa o [modelo de autenticação do UAA](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_uaa) para acessar o serviço {{site.data.keyword.monitoringshort}}, deve-se usar o mesmo ID do usuário e senha que você usa para efetuar login no console do
{{site.data.keyword.Bluemix_notm}}. 

Para verificar se você tem acesso à conta, à organização e ao espaço no qual você deseja efetuar login, efetue login no console do {{site.data.keyword.Bluemix_notm}} e alterne para o espaço. 

Também é possível usar a linha de comandos para verificar se você tem acesso a esse espaço. Execute o comando a seguir para efetuar login em uma região, organização e espaço do {{site.data.keyword.Bluemix_notm}}:

```
bx login -a https://api.ng.bluemix.net
```
{: codeblock}

Siga as instruções. Insira suas credenciais do {{site.data.keyword.Bluemix_notm}}, selecione uma organização e um espaço.


## Acabei de transferir dados JSON por upload para um painel do Grafana; por que perdi a minha barra de rolagem?
{: #2}

Há um erro no Grafana que oculta a barra de rolagem depois de importar um painel. 

Para ver a barra de rolagem, salve o painel depois de importá-lo. 








