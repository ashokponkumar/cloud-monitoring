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

# Monitorando apps em execução no Cloud Foundry
 {:#monitoring_bluemix_apps}

Para enviar métricas no serviço {{site.data.keyword.monitoringshort}}, um aplicativo Cloud Foundry (CF) pode enviar as métricas diretamente do app usando a API de REST do {{site.data.keyword.monitoringshort}}.
{:shortdesc}

Ao usar a infraestrutura do Cloud Foundry para executar seus apps no {{site.data.keyword.Bluemix_notm}}, talvez seja desejado monitorar o status de funcionamento do app, o uso de recursos e as métricas de tráfego. Com essas informações de desempenho, será possível, então, tomar decisões ou executar ações adequadamente.


Um app em execução no cloud foundry não pode executar o collectd. Em vez disso, os dados devem ser enviados do próprio app, usando um cliente lenhador ou a API RESTful. 

##Maneiras adicionais para enviar dados de um app CF [@lindj criou um buildpack collectd e statsd] (https://github.com/jimlindeman/python-buildpack-collectd)

Esse é um clone da liberação do buildpack do Python que tem o collectd instalado para atender em localhost: 8125 com plug-in de entrada statsd e o plug-in de saída do Graphite de diversos locatários do Bluemix 

É possível usar um buildpack para implementar um app CF no Bluemix. O buildpack fornece a estrutura e o suporte de tempo de execução para o app. É possível instalar e configurar o collectd para atender no localhost: 8125 com plug-in de entrada statsd e o plug-in de saída do Graphite de diversos locatários do Bluemix 
