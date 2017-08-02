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


# Recuperación de datos desde el servicio de supervisión
{: #retrieve_data_api}

Puede recuperar métricas desde el servicio {{site.data.keyword.monitoringshort}} mediante la [API de métricas](https://console.bluemix.net/apidocs/927-ibm-cloud-monitoring-rest-api?&language=node#introduction){: new_window}. Puede recuperar métricas desde un espacio de {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}


Para definir el conjunto de datos que piensa recuperar, tenga en cuenta la información siguiente: 

* Debe establecer el espacio de {{site.data.keyword.Bluemix_notm}} desde donde desea recuperar los datos.

* Debe proporcionar una señal o una clave de API para trabajar con el servicio {{site.data.keyword.monitoringshort}}. 

* Debe especificar una vía de acceso a 1 o más métricas. Para obtener más información, consulte [Definición de las métricas](#metrics).

* Opcionalmente, puede especificar un periodo de tiempo personalizado. De forma predeterminada, si no especifica un periodo de tiempo, los datos que recupera son los datos que corresponden a las últimas 24 horas. Para obtener más información, consulte [Configuración de un periodo de tiempo](#time).

**Nota:** puede recuperar un máximo de 5 destinos por solicitud.


## Definición de las métricas
{: #metrics}

Para recuperar datos para una métrica, debe especificar la vía de acceso de la métrica desde la raíz del árbol de métricas hasta la métrica, y separar cada paso con un punto (.).

La vía de acceso se especifica en el parámetro **Target**. Se pueden especificar un máximo de 5 destinos por solicitud.  

Opcionalmente, puede aplicar funciones a la métrica. Para obtener más información sobre las funciones admitidas, consulte [Funciones de Graphite 0.9.15 ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](http://graphite.readthedocs.io/en/latest/functions.html "Icono de enlace externo").

Puede utilizar comodines para definir una vía de acceso. Al utilizar comodines, puede identificar más de una métrica en una sola vía de acceso. 

* **Asterisco**: puede utilizar un asterisco (*) para que coincida con cero o más caracteres. 

    Puede incluir más de un asterisco dentro de un único elemento de vía de acceso, por ejemplo: `servers.sp*aeg*r.cpu.total.*`. Este ejemplo devuelve datos sobre la CPU total para todos los servidores que coincidan con el patrón.

* **Rango o lista de caracteres**: puede definir caracteres entre corchetes ([...]) para especificar una única posición en la serie de la vía de acceso. 

    Puede definir un rango inclusivo de caracteres especificando dos caracteres separados por un guión (-). Coincidirá con cualquier carácter entre estos dos caracteres. 
	
	Puede definir una o varios rangos de caracteres entre corchetes. 
	
	Cuando los caracteres incluidos dentro de un conjunto de corchetes no se puedan leer como un rango, se tratan como una lista de valores. 
	
	Puede incluir un guión (-) como carácter en una lista de valores poniéndolo al principio o al final dentro de los corchetes.

* **Lista de valores**: puede establecer valores separados por comas entre llaves para definir listas de valores. 

    Por ejemplo, para descargar métricas para las vías de acceso `servers.myserver.cpu.total.user` y `servers.myserver.cpu.total.system}`, puede establecer una sola vía de acceso para ambos tipos: `servers.myserver.cpu.total.{user,system}`



## Definición de un periodo de tiempo
{: #time}

Opcionalmente, puede especificar un periodo de tiempo utilizando un tiempo relativo o un tiempo absoluto. Debe definir los parámetros de consulta **From** y **Until**.  

* Cuando la hora de inicio del periodo de tiempo se omite, el valor predeterminado es hace 24 horas. 

* Cuando la hora de finalización del periodo de tiempo se omite, el valor predeterminado es la hora actual, *now*.

El **tiempo relativo** siempre va precedido por un signo menos (-), y seguido de una unidad de tiempo. 

En la tabla siguiente se muestran las diferentes abreviaturas de tiempo relativo que puede utilizar:


| Abreviatura | Descripción |
|--------------|-------------|
| s            | Segundos     |
| min          | Minutos     |
| h            | Horas       |
| d            | Días        |
| w            | Semanas       |
| mon          | Meses      |
| now          | Hora actual |


Puede utilizar cualquiera de los formatos siguientes para definir un **tiempo absoluto**: `HH:MM_AAMMDD`, `AAAAMMDD`, `MM/DD/AA`

| Abreviatura | Descripción |
|--------------|-------------|
| YYYY         | Año con cuatro dígitos |
| MM           | Mes con dos dígitos (01=Ene, etc.) |
| DD           | Día del mes con dos dígitos (de 01 hasta 31) |
| hh           | Dos dígitos de hora (de 00 a 23) |
| mm           | Dos dígitos de minuto (de 00 a 59) |
| ss           | Dos dígitos de segundo (de 00 a 59) |

**Ejemplo**

En la tabla siguiente se muestran diferentes ejemplos sobre cómo establecer periodos de tiempo distintos definiendo los parámetros *from* y *until*:

<table>
  <caption>Tabla 1. Diferentes ejemplos que muestran cómo establecer periodos de tiempo distintos definiendo los parámetros *from* y *until*</caption>
  <tr>
    <th>Ejemplo</th>
	<th>Descripción</th>
  </tr>
  <tr>
    <td>&from=monday</td>
	<td>Periodo de tiempo que incluye datos desde el último lunes hasta ahora.</td>
  </tr>
  <tr>
    <td>&from=20171101&until=20171130</td>
	<td>Periodo de tiempo establecido en un mes, por ejemplo, noviembre </td>
  </tr>
  <tr>
    <td>&from=02:00_20170701&until=14:00_20170701</td>
	<td>Para mostrar datos dentro de un ciclo de 24 horas, por ejemplo, desde las 02:00 hasta las 14:00 del 1 de julio de 2017</td>
  </tr>
  <tr>
    <td>&from=-8d&until=-7d</td>
	<td>Para mostrar el mismo día de la semana, por ejemplo, la última semana</td>
  </tr>
  
</table>


## Definición del espacio
{: #domain}

Cuando se utiliza el modelo de autenticación UAA de {{site.data.keyword.Bluemix_notm}} o el modelo de autenticación IAM de {{site.data.keyword.Bluemix_notm}} para autenticarse con el servicio {{site.data.keyword.monitoringshort}}, es necesario el campo de cabecera **X-Auth-Scope-Id** y se debe definir en el GUID del espacio. El GUID debe tener el prefijo *s-* para identificar un espacio.



## Establecimiento de la señal de autorización o de la clave de API
{: #security}
  
Debe establecer el campo **X-Auth-User-Token** para definir la señal UAA, la señal IAM o la clave de API que se utiliza para la autenticación. 

Una señal o una clave de API deben llevar uno de los siguientes prefijos: `apikey`, `iam` o `uaa` 

donde 

* *apikey* identifica que la señal es una clave de API.
* *iam*  identifica que la señal especificada es una señal generada por IAM.
* *uaa* identifica que la señal especificada es una señal generada por UAA.

Por ejemplo, puede establecer la cabecera siguiente para una clave de API: `X-Auth-User-Token: apikey SomeIAMGeneratedKey`


## Recuperación de métricas desde un espacio mediante UAA
{: #uaa}

Para recuperar métricas desde un espacio de {{site.data.keyword.Bluemix_notm}}, siga los pasos siguientes:

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
	export Token="eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8"
	```
	{: screen}
	
	**Nota:** La señal excluye *Bearer*.
		
3. Obtenga el GUID del espacio. El GUID debe tener el prefijo *s-* para identificar un espacio.

    Ejecute el mandato siguiente:
	
	```
	cf space SpaceName --guid
	```
	{: codeblock}
	
	Donde *SpaceName* es el nombre del espacio con el prefijo *s-* donde va a definir una notificación.
	
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
	curl -XGET --header "X-Auth-User-Token:uaa ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics?from=Start_Time&until=End_Time&target=string
	```
	{: codeblock}

	donde
	
	* El valor *X-Auth-User-Token* es un parámetro que pasa la señal UAA de {{site.data.keyword.Bluemix_notm}}.
	
	* *uaa* es el prefijo que indica que la señal especificada es una señal generada por UAA.
	
	* *X-Auth-Scope-Id* es un parámetro que pasa el GUID de espacio. El GUID debe tener el prefijo *s-* para identificar un espacio. Esta cabecera solo es necesaria si utiliza la autenticación de UAA. Si utiliza una señal de autenticación de IAM, esta cabecera es opcional y se utiliza la información de dominio enlazada con la señal.
	
	* *Token* representa la señal de UAA.
	
	* *Space representa el GUID del espacio. Solo es necesario cuando se utiliza una señal de UAA.
	
	* *Start_Time* define el periodo de tiempo relativo o absoluto que establece el inicio de la solicitud. De forma predeterminada, es hace 24 horas: `-24h`. *End_Time* define el periodo de tiempo relativo o absoluto para establecer el final de la solicitud. De forma predeterminada, es el momento actual: `now`. Para obtener más información, consulte [Definición de un periodo de tiempo](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#time).
	
	* *Path*  identifica una o varias métricas. Opcionalmente, puede aplicar funciones a cada métrica. En las vías de acceso también se admiten los comodines, que permiten identificar más de una métrica en una sola vía de acceso. Para obtener más información, consulte [Definición de las métricas](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#metrics).
	
	
	
## Recuperación de métricas desde un espacio mediante IAM o una clave de API
{: #iam}

Para recuperar métricas desde un espacio de {{site.data.keyword.Bluemix_notm}} mediante IAM o una clave de API, siga estos pasos:

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
	
	Donde *SpaceName* es el nombre del espacio con el prefijo *s-* donde va a definir una notificación.
	
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
	
5. Ejecute el mandato cURL para recuperar métricas.

   	```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics?from=Start_Time&until=End_Time&target=string
	```
	{: codeblock}

	donde
	
	* El valor *X-Auth-User-Token* es un parámetro que pasa la señal de AIM de {{site.data.keyword.Bluemix_notm}} o la clave de API.
	
	* El valor *Auth_Type* es el prefijo que define el tipo de señal o de clave de API.

        * *apikey* identifica que la señal es una clave de API.
		* *iam* identifica que la señal especificada es una señal generada por IAM.
	
	* *X-Auth-Scope-Id* es un parámetro que pasa el GUID de espacio. El GUID debe tener el prefijo *s-* para identificar un espacio. Esta cabecera solo es necesaria si utiliza la autenticación de UAA. Si utiliza una señal de autenticación de IAM, esta cabecera es opcional y se utiliza la información de dominio enlazada con la señal.
	
	* *Token* representa la señal de UAA.
	
	* *Space representa el GUID del espacio. Solo es necesario cuando se utiliza una señal de UAA.
	
	* *Start_Time* define el periodo de tiempo relativo o absoluto que establece el inicio de la solicitud. De forma predeterminada, es hace 24 horas: `-24h`. *End_Time* define el periodo de tiempo relativo o absoluto para establecer el final de la solicitud. De forma predeterminada, es el momento actual: `now`. Para obtener más información, consulte [Definición de un periodo de tiempo](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#time).
	
	* *Path*  identifica una o varias métricas. Opcionalmente, puede aplicar funciones a cada métrica. En las vías de acceso también se admiten los comodines, que permiten identificar más de una métrica en una sola vía de acceso. Para obtener más información, consulte [Definición de las métricas](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#metrics).






