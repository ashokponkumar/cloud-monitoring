---

copyright:
  years: 2017

lastupdated: "2017-07-10"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Suministro del servicio de supervisión
{: #provision}

Puede suministrar el servicio {{site.data.keyword.monitoringshort}} desde la interfaz de usuario de {{site.data.keyword.Bluemix}} o desde la línea de mandatos.
{:shortdesc}


## Suministro desde la interfaz de usuario
{: #ui}

Complete los pasos siguientes para suministrar una instancia del servicio {{site.data.keyword.monitoringshort}} en {{site.data.keyword.Bluemix_notm}}:

1. Inicie sesión en su cuenta de {{site.data.keyword.Bluemix_notm}}.

    El panel de control de {{site.data.keyword.Bluemix_notm}} se puede encontrar en: [http://bluemix.net ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](http://bluemix.net "Icono de enlace externo"){:new_window}.
    
	Después de iniciar sesión con su ID de usuario y su contraseña, se abre la interfaz de usuario de {{site.data.keyword.Bluemix_notm}}.

2. Pulse **Catálogo**. Se abre la lista de los servicios que están disponibles en {{site.data.keyword.Bluemix_notm}}.

3. Seleccione la categoría **DevOps** para filtrar la lista de servicios que se muestra.

4. Pulse el mosaico **Supervisión**.

5. Seleccione un plan de servicio. Para recopilar y acceder a las métricas durante un máximo de 45 días, y para definir reglas de alertas, incluidas reglas con caracteres comodín, seleccione el plan **Premium**. De forma predeterminada, se establece el plan **Lite**, que le autoriza para recopilar métricas de plataforma en el espacio donde suministra el servicio y permite un periodo de retención de 15 días para dichas métricas. 

    Para obtener más información acerca de los planes de servicio, consulte [Planes de servicio](/docs/services/cloud-monitoring/monitoring_ov.html#plans).
	
6. Pulse **Crear** para suministrar el servicio {{site.data.keyword.monitoringshort}} en el espacio de {{site.data.keyword.Bluemix_notm}} en el que ha iniciado la sesión.
  
 

## Suministro desde la CLI
{: #cli}

Complete los pasos siguientes para suministrar una instancia del servicio {{site.data.keyword.monitoringshort}} en {{site.data.keyword.Bluemix_notm}} mediante la línea de mandatos:

1. [Requisito previo] Instale la CLI de {{site.data.keyword.Bluemix_notm}}.

   Para obtener más información, consulte [Instalación de la CLI de {{site.data.keyword.Bluemix_notm}} ](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa).
   
   Si la CLI está instalada, continúe en el paso siguiente.
    
2. Inicie la sesión en una región, organización y espacio de {{site.data.keyword.Bluemix_notm}}. Ejecute el mandato:

    ```
    cf login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    Siga las instrucciones. Escriba sus credenciales de {{site.data.keyword.Bluemix_notm}}, seleccione una organización y un espacio.
	
3. Ejecute el mandato `cf create-service` para suministrar una instancia.

    ```
	cf create-service service_name service_plan service_instance_name
	```
	{: codeblock}
	
	Donde
	
	* service_name es el nombre del servicio, que es **Monitoring**.
	* service_plan es el nombre del plan de servicio. Para recopilar y acceder a las métricas durante un máximo de 45 días, y para definir reglas de alertas, incluidas reglas con caracteres comodín, seleccione el plan **Premium**. De forma predeterminada, se establece el plan **Lite**, que le autoriza para recopilar métricas de plataforma en el espacio donde suministra el servicio y permite un periodo de retención de 15 días para dichas métricas. Para obtener una lista de planes, consulte [Planes de servicio de {{site.data.keyword.monitoringshort}}](/docs/services/cloud-monitoring/monitoring_ov.html#plan).
	* service_instance_name es el nombre que desea utilizar para la nueva instancia del servicio que crea.
	
	Para obtener más información sobre el mandato cf, consulte [cf create-service](/docs/cli/reference/cfcommands/index.html#cf_create-service).

	Por ejemplo, para crear una instancia del servicio {{site.data.keyword.monitoringshort}} con un plan premium, ejecute el mandato siguiente: 	
	```
	cf create-service Monitoring premium my_monitoring_svc
	```
	{: codeblock}
	
4. Verifique que el servicio se ha creado satisfactoriamente. Ejecute el mandato siguiente:

    ```	
	cf services
	```
	{: codeblock}
	
	La salida de la ejecución del mandato tiene el siguiente aspecto:
	
	```
    Getting services in org MyOrg / space MySpace as xxx@yyy.com...
    OK
    
    name                           service                  plan                   bound apps              last operation
    my_monitoring_svc              Monitoring               premium                                        create succeeded
	```
	{: screen}

	



