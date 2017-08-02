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


# Envío de datos al servicio de supervisión
{: #send_data_api}

Puede enviar métricas al servicio {{site.data.keyword.monitoringshort}} mediante la API de métricas. Puede enviar métricas a un espacio de {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}


Para contenedores {{site.data.keyword.Bluemix_notm}} Docker, se recopilan automáticamente métricas básicas del sistema. Para aplicaciones Cloud Foundry y apps que se ejecutan en una máquina virtual (VM), las métricas se deben enviar directamente desde la app mediante [la API de métricas](https://console.bluemix.net/apidocs/927-ibm-cloud-monitoring-rest-api?&language=node#introduction){: new_window}. 



## Envío de métricas en un espacio mediante UAA
{: #uaa}

Para enviar métricas a un espacio de {{site.data.keyword.Bluemix_notm}}, siga los pasos siguientes:

1. Inicie la sesión en una región, organización y espacio de {{site.data.keyword.Bluemix_notm}}. Ejecute el mandato:

    Por ejemplo, para iniciar sesión en la región EE. UU. sur, ejecute el siguiente mandato:
	
	```
    cf login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Siga las instrucciones. Escriba sus credenciales de {{site.data.keyword.Bluemix_notm}}, seleccione una organización y un espacio.

2. Obtenga la señal de autenticación de UAA.

    Para obtener más información sobre la autenticación de UAA, consulte [Obtención de la señal de UAA mediante la CLI de Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) o bien [Obtención de la señal de UAA mediante la API REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

	Por ejemplo, para utilizar la señal de UAA, ejecute el siguiente mandato:

    ```
	cf oauth-token
	```
	{: codeblock}
	
	El resultado de este mandato es el siguiente:
	
	```
	bearer eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8
	```
	{: screen}
	
	A continuación, exporte la variable *Token*. Por ejemplo:
	
	```
	export Token="eyJhbGciOiJI....cGlxryWS8"
	```
	{: screen}
	
	**Nota:** La señal excluye *Bearer*.
		
3. Obtenga el GUID del espacio. El GUID debe tener el prefijo *s-* para identificar un espacio.

    Ejecute el mandato siguiente:
	
	```
	cf space SpaceName --guid
	```
	{: codeblock}
	
	Donde *SpaceName* es el nombre del espacio donde va a definir una notificación.
	
	Por ejemplo, para obtener el GUID de un espacio con el nombre *dev*, ejecute el siguiente mandato:
	
	```
	cf space dev --guid
	```
	{: screen}
	
	El resultado de este mandato es el siguiente:
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	A continuación, exporte la variable *Space*. Por ejemplo:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. Ejecute el siguiente mandato cURL para enviar métricas:

    ```
	curl -XPOST -d @Metric_File --header "X-Auth-User-Token:uaa ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics
	```
	{: codeblock}
	
	donde
	
	* Metrics_File representa el archivo JSON que incluye la definición de las métricas que se envían al servicio {{site.data.keyword.monitoringshort}}.
	
	* El valor *X-Auth-User-Token* es un parámetro que pasa la señal UAA de {{site.data.keyword.Bluemix_notm}}.
	
	* El valor *Auth_Type* es el prefijo que define el tipo de señal. *uaa* identifica que la señal especificada es una señal generada por UAA.
	
	* *X-Auth-Scope-Id* es un parámetro que pasa el GUID de espacio. El GUID debe tener el prefijo *s-* para identificar un espacio.
	
	* Token representa la señal de UAA.
	
	* Space representa el GUID del espacio. 
	
	Por ejemplo, puede ejecutar el siguiente mandato para enviar 2 métricas, `myhost.cpu.idle` y `myapp.login.attempts`, al servicio {{site.data.keyword.monitoringshort}}:
	
	```
	curl -XPOST @metric.json --header "X-Auth-User-Token:uaa ${Token}" https://metrics.ng.bluemix.net/v1/metrics
	```
	{: screen}
	
	El archivo de ejemplo *metric.json* se define así:

    ```
    [
      {
        "name" : "myhost.cpu.idle",
        "value" : 22.37
      },
      {
        "name": "myapp.login.retries",
        "value": 4
      }
    ]
	```
	{: screen}

 
## Envío de métricas a un espacio mediante IAM o una clave de API
{: #iam}

Para enviar métricas a un espacio de {{site.data.keyword.Bluemix_notm}}, siga estos pasos:

1. Inicie la sesión en una región, organización y espacio de {{site.data.keyword.Bluemix_notm}}. Ejecute el mandato:

    Por ejemplo, para iniciar sesión en la región EE. UU. sur, ejecute el siguiente mandato:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Siga las instrucciones. Escriba sus credenciales de {{site.data.keyword.Bluemix_notm}}, seleccione una organización y un espacio.

2. Obtenga la señal de autenticación o la clave de API.

    Para obtener información sobre la autenticación de IAM, consulte [Obtención de la señal de IAM mediante la CLI de Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_cli) o [Generación de una clave de API de IAM mediante la CLI de Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).

	Por ejemplo, para utilizar la señal de IAM, ejecute el siguiente mandato:

    ```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	El resultado de este mandato es el siguiente:
	
	```
	Señal de IAM:  Portador djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ
    Señal de UAA:  Portador eyJhbGciOiJIUz..Ky6vagp3k_QcIcKJ-td83qXhO5Uze43KcplG6PzcGs8
	```
	{: screen}
	
	A continuación, exporte la variable *Token*. Por ejemplo:
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Nota:** La señal excluye *Bearer*.
		
3. Obtenga el GUID del espacio.

    Para obtener el GUID del espacio, consulte [¿Cómo se obtiene el GUID de un espacio?](/docs/service/cloud-monitoring/qa/cli_qa.html#space_guid). El GUID debe tener el prefijo *s-* para identificar un espacio.
    
    Por ejemplo, ejecute el siguiente mandato:
	
	```
	bx iam space NAME --guid
	```
	{: codeblock}
	
	Donde *SpaceName* es el nombre del espacio donde va a definir una notificación.
	
	Por ejemplo, para obtener el GUID de un espacio con el nombre *dev*, ejecute el siguiente mandato:
	
	```
	bx iam space dev --guid
	```
	{: screen}
	
	El resultado de este mandato es el siguiente:
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	A continuación, exporte la variable *Space*. Por ejemplo:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. Ejecute el mandato cURL para enviar métricas.

    ```
	curl -XPOST -d @Metric_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics
	```
	{: codeblock}
	
	donde
	
	* Metrics_File representa el archivo JSON que incluye la definición de las métricas que se envían al servicio {{site.data.keyword.monitoringshort}}.
	
	* El valor *X-Auth-User-Token* es un parámetro que pasa la señal de IAM de {{site.data.keyword.Bluemix_notm}} o la clave de API.
	
	* El valor *Auth_Type* es el prefijo que define el tipo de señal o de clave de API. En la lista siguiente se muestran los valores válidos: *apikey*, *iam* o *uaa*, donde

  * *apikey* identifica que la señal es una clave de API.
		* *iam* identifica que la señal especificada es una señal generada por IAM.
	
	* *X-Auth-Scope-Id* es un parámetro que pasa el GUID de espacio. El GUID debe tener el prefijo *s-* para identificar un espacio. 
	
	* Token representa la señal de IAM o clave de API.
	
	* Space representa el GUID del espacio. 

	
Por ejemplo, puede ejecutar el siguiente mandato para enviar dos métricas, `myhost.cpu.idle` y `myapp.login.retries`, a un espacio del servicio {{site.data.keyword.monitoringshort}}:
	
```
curl -XPOST @metric.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/metrics
```
{: screen}
	
El archivo de ejemplo *metric.json* se define así:

```
[
  {
    "name" : "myhost.cpu.idle",
    "value" : 22.37
  },
  {
    "name": "myapp.login.retries",
    "value": 4
  }
]
```
{: screen}


















 
