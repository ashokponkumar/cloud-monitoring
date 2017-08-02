---

copyright:
  years: 2017

lastupdated: "2017-07-12"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Autenticación mediante el modelo UAA de Bluemix
{: #auth_uaa}

Utilice el modelo UAA de {{site.data.keyword.Bluemix}} para obtener una señal de autenticación que pueda utilizar para acceder a las métricas almacenadas en el servicio {{site.data.keyword.monitoringshort}} para un espacio de {{site.data.keyword.Bluemix_notm}}. Para obtener la señal de autenticación, utilice la CLI de {{site.data.keyword.Bluemix_notm}} o la API REST `Login`.
{:shortdesc}

Para acceder a las métricas utilizando una señal de autenticación UAA, necesita la siguiente información:

* Una señal UAA para acceder al servicio {{site.data.keyword.monitoringshort}} utilizado las API RESTful.
* El GUID del espacio.

		
## Obtención de la señal UAA mediante la CLI de Bluemix
{: #uaa_cli}


Para obtener la señal de autorización, siga estos pasos:

1. Instale la CLI de {{site.data.keyword.Bluemix_notm}}.

   Para obtener más información, consulte [Instalación de la CLI de {{site.data.keyword.Bluemix_notm}} ](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa).
   
   Si la CLI está instalada, continúe en el paso siguiente.
    
2. Inicie la sesión en una región, organización y espacio de {{site.data.keyword.Bluemix_notm}}. Ejecute el mandato:

    ```
    cf login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    Siga las instrucciones. Escriba sus credenciales de {{site.data.keyword.Bluemix_notm}}, seleccione una organización y un espacio.
	
3. Ejecute el mandato `cf oauth-token` para obtener la señal UAA de {{site.data.keyword.Bluemix_notm}}.

    ```
	cf oauth-token
	```
	{: codeblock}
	
	El resultado devuelve la señal UAA que debe utilizar para autenticar su ID de usuario en dicho espacio y organización.

4. Obtenga el GUID del espacio de la organización para el que ha obtenido una señal de autenticación.

   Para obtener más información, consulte [¿Cómo se obtiene el GUID de un espacio](/docs/services/cloud-monitoring/qa/cli_qa.html#space_guid).  	
5. Exporte las siguientes variables: TOKEN y SPACE.

    * *TOKEN* es la señal oauth que ha obtenido en el paso anterior sin incluir Bearer.
	
	* *SPACE* es el GUID del espacio que se ha obtenido en el paso anterior.
		
	Por ejemplo,
	
	```
	export TOKEN="eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8"
	export SPACE="667fb895-abcd-defg-aaaa-cf4587341095"
	```
	{: screen}
	
6. ejecute el mandato siguiente para obtener la señal UAA para trabajar con el servicio {{site.data.keyword.monitoringshort}}:
 
    ```
	curl -k -X GET  --header "X-Auth-Token: ${TOKEN}"  --header "X-Auth-Project-Id: ${SPACE}"  METRICS_ENDPOINT
    ```
    {: codeblock}	
	
	donde
	* SPACE es el GUID del espacio en el que se ejecuta el servicio.
	* TOKEN es la señal UAA de {{site.data.keyword.Bluemix_notm}} que ha obtenido en un paso anterior sin el prefijo bearer.
	* METRICS_ENDPOINT es el punto final de métricas (https://metrics.ng.bluemix.net/token) correspondiente a la región de {{site.data.keyword.Bluemix_notm}} en la que se encuentran la organización y el espacio.

	
## Obtención de la señal UAA mediante la API
{: #uaa_api}

Para obtener la señal de autorización, ejecute el siguiente mandato cURL:

```
curl -XPOST -d 'user=USERNAME&passwd=PASSWORD&space=SPACE_NAME&organization=ORG_NAME' METRICS_ENDPOINT
```
{: codeblock}

donde

* USERNAME es un ID de {{site.data.keyword.Bluemix_notm}} para el que desea obtener la señal de autenticación para trabajar con el servicio {{site.data.keyword.monitoringshort}}.
* PASSWORD es la contraseña del ID de usuario utilizado para iniciar sesión en {{site.data.keyword.Bluemix_notm}}.
* SPACE_NAME es el nombre del espacio en el que se recopilan las métricas.
* ORG_NAME es el nombre de la organización de {{site.data.keyword.Bluemix_notm}} que contiene el espacio.
* METRICS_ENDPOINT es el punto final de métricas (https://metrics.ng.bluemix.net/login) correspondiente a la región de {{site.data.keyword.Bluemix_notm}} en la que se encuentran la organización y el espacio.

El resultado es un documento JSON que contiene la señal UAA. Obtenga el valor de la señal del campo **access-token**.

Por ejemplo, un documento JSON de ejemplo tiene el siguiente aspecto:

```
{
    "access_token": "eyJhbGc...",
    "logging_token": "xxxxxxxxxxxx",
    "org_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "space_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```
{: screen}

El valor de *logging_token* corresponde a la señal UAA que necesita para trabajar con el servicio {{site.data.keyword.monitoringshort}}.
 
**Nota:**

* Si no utiliza cURL, debe definir la cabecera **Content-Type: application/x-www-form-urlencoded**.
* Si recibe el código de error *BXNMS0122E: Las credenciales de usuario no son válidas*, compruebe que está utilizando un ID de {{site.data.keyword.IBM_notm}} válido.


