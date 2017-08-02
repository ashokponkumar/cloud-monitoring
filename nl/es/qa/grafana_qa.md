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


# Preguntas frecuentes y respuestas sobre el uso de Grafana
{: #grafana_qa}

Aquí encontrará las respuestas a preguntas comunes sobre el uso de Grafana con el servicio {{site.data.keyword.monitoringshort}}. 
{:shortdesc}

* [Recibo un error 404 cuando inicio una sesión en la interfaz de usuario web del servicio de supervisión](/docs/services/cloud-monitoring/qa/grafana_qa.html#404)
* [Acabo de cargar datos json para un panel de control de Grafana; ¿por qué he perdido la barra de desplazamiento?](/docs/services/cloud-monitoring/qa/grafana_qa.html#2)


## Recibo un error 404 cuando inicio una sesión en la interfaz de usuario web del servicio de supervisión utilizando el modelo de autorización UUA
{: #404}

Si recibe un error 404 al intentar iniciar una sesión en la interfaz de usuario web (Grafana) del servicio {{site.data.keyword.monitoringshort}}, compruebe que el espacio exista y que tenga acceso al mismo.

Si utiliza el [modelo de autenticación UAA](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_uaa) para acceder al servicio {{site.data.keyword.monitoringshort}}, debe utilizar el mismo ID de usuario y contraseña que ha utilizado para iniciar la sesión en la consola de {{site.data.keyword.Bluemix_notm}}. 

Para comprobar que tiene acceso a la cuenta, organización y espacio donde desea iniciar la sesión, inicie la sesión en la consola de {{site.data.keyword.Bluemix_notm}} y vaya al espacio. 

También puede utilizar la línea de mandatos para comprobar que tiene acceso a dicho espacio. Ejecute el siguiente mandato para iniciar una sesión en una región, organización y espacio de {{site.data.keyword.Bluemix_notm}}:

```
bx login -a https://api.ng.bluemix.net
```
{: codeblock}

Siga las instrucciones. Escriba sus credenciales de {{site.data.keyword.Bluemix_notm}}, seleccione una organización y un espacio.


## Acabo de cargar datos JSON para un panel de control de Grafana; ¿por qué he perdido la barra de desplazamiento?
{: #2}

Existe un error en Grafana que oculta la barra de desplazamiento después de importar un panel de control. 

Para ver la barra de desplazamiento, guardar el panel de control después de importarlo. 








