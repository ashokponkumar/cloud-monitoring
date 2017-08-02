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

# Apps de supervisión en ejecución en Cloud Foundry
 {:#monitoring_bluemix_apps}

Para enviar métricas al servicio {{site.data.keyword.monitoringshort}}, una aplicación Cloud Foundry (CF) puede enviar las métricas directamente desde la app mediante la API REST de {{site.data.keyword.monitoringshort}}.
{:shortdesc}

Cuando utilice la infraestructura Cloud Foundry para ejecutar sus apps en {{site.data.keyword.Bluemix_notm}}, es posible que desee supervisar el estado de salud de la app, el uso de recursos y métricas sobre tráfico. Con esta información de rendimiento puede tomar decisiones o llevar a cabo acciones según convenga.


Una app que se ejecute en cloud foundry no puede ejecutar collectd. Los datos se deben enviar desde la propia app, mediante un cliente lumberjack o la API RESTful. 

##Formas adicionales de enviar datos desde una app CF [@lindj ha creado un paquete de compilación collectd y statsd] (https://github.com/jimlindeman/python-buildpack-collectd)

Es un clon del release python-buildpack que tiene collectd instalado para escuchar en localhost:8125 con el plugin de entrada statsd y el plugin de salida de graphite multiarrendatario de Bluemix 

Puede utilizar un paquete de compilación para desplegar una app CF en Bluemix. El paquete de compilación proporciona la infraestructura y el soporte de entorno de ejecución para la app. Puede instalar y configurar collectd de modo que escuche en localhost:8125 con el plugin de entrada statsd y el plugin de salida de graphite multiarrendatario de Bluemix 
