---

copyright:
  years: 2017

Lastupdated: "2017-05-25"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

# Navegando para o painel Grafana
{:#navigating_grafana}

No {{site.data.keyword.Bluemix}}, é possível usar o Grafana, uma plataforma de análise de dados e de visualização de software livre para monitorar, procurar, analisar e visualizar suas métricas em uma variedade de gráficos, por exemplo, diagramas e tabelas. Use o Grafana para executar tarefas analíticas avançadas.
{:shortdesc}

É possível ativar o Grafana de qualquer uma das maneiras a seguir:

* No {{site.data.keyword.Bluemix_notm}}

    É possível ativar suas métricas de contêiner do Docker específicas no Grafana dentro do contexto para esse contêiner específico. Esse recurso se aplica somente aos contêineres que são implementados na infraestrutura em Nuvem gerenciada pelo {{site.data.keyword.Bluemix_notm}}. 
    
    Para obter mais informações, veja [Navegando para o painel do Grafana por meio do painel do {{site.data.keyword.Bluemix_notm}}](navigating_grafana.html#launch_grafana_from_bluemix).

* Em um link direto do navegador

    É possível ativar o Grafana para que os dados que são exibidos agreguem logs de serviços em um espaço fornecido do {{site.data.keyword.Bluemix_notm}}.
    
    Para obter mais informações, veja [Navegando para o painel do Grafana por meio de um navegador da web](navigating_grafana.html#launch_grafana_from_browser).
    
Para obter mais informações sobre Grafana, consulte o [Guia do Usuário do Grafana ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](http://docs.grafana.org/guides/getting_started/ "Ícone de link externo"){: new_window}.


##  Navegando para o painel do Grafana por meio do painel do Bluemix
{: #launch_grafana_from_bluemix}

**Nota:** esse recurso se aplica somente aos contêineres que são implementados na infraestrutura em Nuvem gerenciada pelo {{site.data.keyword.Bluemix_notm}}. 

A consulta que é usada para filtrar os dados que são exibidos no Grafana recupera dados para o contêiner do {{site.data.keyword.Bluemix_notm}} no local em que o Kibana é ativado. 

Para ver as métricas de um contêiner do Docker no Grafana, conclua as etapas a seguir:

1. Efetue login no {{site.data.keyword.Bluemix_notm}} e, em seguida, clique no contêiner por meio do painel do {{site.data.keyword.Bluemix_notm}}. 
    
2. Na barra de navegação, clique em **Monitoramento e logs**. A guia de monitoramento é aberta. 
    
3. Clique em **Visualização avançada**. O painel do **Grafana** é aberto.


##  Navegando para o painel do Grafana por meio de um navegador da web
{: #launch_grafana_from_browser}

A consulta que é usada para filtrar os dados que são exibidos no Grafana recupera dados para um espaço na organização do {{site.data.keyword.Bluemix_notm}}. As informações de métricas que o Grafana exibe incluem registros para todos os recursos que estiverem implementados no espaço da organização do {{site.data.keyword.Bluemix_notm}} na qual você estiver com login efetuado.

Conclua as etapas a seguir para ativar o Grafana em um navegador:

1. Abra [https://metrics.ng.bluemix.net](https://metrics.ng.bluemix.net) para efetuar login na interface com o usuário do Grafana.
2. Selecione **Grafana**.
     

