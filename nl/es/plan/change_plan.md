---

copyright:
  years: 2017
lastupdated: "2017-07-10"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Cambio del plan
{: #change_plan}

Puede cambiar su plan de servicio {{site.data.keyword.monitoringshort}} en {{site.data.keyword.Bluemix}} en el panel de control del servicio o mediante el mandato `cf update-service`. Puede actualizar o reducir el plan siempre que lo desee.
{:shortdesc}

## Cambio del plan de servicio mediante la interfaz de usuario
{: #change_plan_ui}

Para cambiar el plan de servicio en {{site.data.keyword.Bluemix_notm}} en el panel de control del servicio, siga estos pasos:

1. Inicie una sesión en {{site.data.keyword.Bluemix_notm}} y pulse el servicio {{site.data.keyword.monitoringshort}} en el panel de control de {{site.data.keyword.Bluemix_notm}}. 
    
2. Seleccione el separador **Plan** en la interfaz de usuario de {{site.data.keyword.Bluemix_notm}}.

    Aparecerá información sobre el plan actual.
	
3. Seleccione un nuevo plan para actualizar o reducir su plan. 

4. Pulse **Guardar**.



## Cambio del plan de servicio mediante la CLI
{: #change_plan_cli}

Para cambiar el plan de servicio en {{site.data.keyword.Bluemix_notm}} mediante la CLI, siga los pasos siguientes:

1. 1. Inicie la sesión en una región, organización y espacio de {{site.data.keyword.Bluemix_notm}}. Ejecute el mandato:

    ```
    cf login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}
	
2. Ejecute el mandato `cf services` para comprobar el plan actual y para obtener el nombre del servicio {{site.data.keyword.loganalysisshort}} de la lista de servicios disponibles en el espacio. 

    El valor del campo **name** es el que se debe utilizar para cambiar el plan. 

    Por ejemplo,
	
	```
	$ cf services
	Getting services in org MyOrg / space dev as xxx@yyy.com...
	OK
	name            service      plan   bound apps   last operation
	Monitoring-0c   Monitoring   premium             create succeeded
    ```
	{: screen}
    
3. Actualice o reduzca el servicio plan. Ejecute el mandato `cf update-service`.
    
	```
	cf update-service service_name [-p new_plan]
	```
	{: codeblock}
	
	donde 
	
	* *service_name* es el nombre del servicio. Puede ejecutar el mandato `cf services` para obtener el valor.
	* *new_plan* es el nombre del plan.
	
	En la tabla siguiente se muestran los distintos planes y los valores que admiten:
	
	<table>
	  <caption>Tabla 1. Lista de planes.</caption>
	  <tr>
	    <th>Plan</th>
		<th>Características</th>
	    <th>Nombre</th>
	  </tr>
	  <tr>
	    <td>Lite</td>
	    <td>Supervisión gratuita.</td>
		<td>lite</td>
	  </tr>
	  <tr>
	    <td>Premium</td>
	    <td>Supervisión Premium.</td>
		<td>premium</td>
	  </tr>
	</table>
	
	Por ejemplo, para reducir su plan al plan *Lite*, ejecute el siguiente mandato:
	
	```
	cf update-service "Monitoring-0c" -p lite
    Updating service instance Monitoring-0c as xxx@yyy.com...
    OK
	```
	{: screen}

4. Verifique que el plan se ha modificado. Ejecute el mandato `cf services`.

    ```
	cf services
    Getting services in org MyOrg / space dev as xxx@yyy.com...
    OK

    name              service       plan   bound apps   last operation
    Monitoring-0c     Monitoring    lite                create succeeded
	```
	{: screen}






