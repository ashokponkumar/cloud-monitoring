---

copyright:
  years: 2017

lastupdated: "2017-07-14"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Perguntas e respostas frequentes
{: #qa}

Veja a seguir as repostas a perguntas comuns sobre o serviço {{site.data.keyword.monitoringshort}}. 
{:shortdesc}

* [Por que não posso ver os meus dados antigos para uma métrica para a qual não enviei valores de métricas recentemente?](#qa3)


## Por que não posso ver os meus dados antigos para uma métrica para a qual não enviei valores de métricas recentemente?
{: #qa3}

O {{site.data.keyword.monitoringshort}} exclui todos os dados para um caminho de métrica que parece ser de natureza transitória, identificando as métricas que não foram gravadas nos últimos sete dias. 

Por exemplo:

* Quando um contêiner for excluído, as métricas associadas a esse contêiner existirão durante 7 dias e, depois desse tempo, elas serão excluídas.
* Se você tiver um gráfico statsd chamado `<space_id>.test.statsd.gauge-hello` e você não grava nele durante uma semana; a métrica será identificada como temporária e essa métrica será excluída junto a todas as suas informações históricas. 

