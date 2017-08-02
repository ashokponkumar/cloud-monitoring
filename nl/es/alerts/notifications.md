---

copyright:
  years: 2017

lastupdated: "2017-07-18"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Cómo trabajar con notificaciones mediante la API de alertas
{: #notifications}

Utilice la API de alertas para crear, suprimir y actualizar una notificación, para mostrar los detalles de una notificación y para obtener una lista de las notificaciones definidas en su espacio de {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

## Creación de una plantilla de notificación
{: #template}

Una notificación es un archivo JSON. 

Puede crear tantas plantillas de notificación como desee y luego reutilizarlas para crear notificaciones de ese tipo en su organización. 

Puede definir cualquiera de los siguientes tipos de notificaciones:

* Email: Defina una notificación de tipo *Email* para enviar un correo electrónico a una dirección de correo electrónico válida. 
* Webhook: Defina una notificación de tipo *Webhook* solo para puntos finales https. Añada un parámetro al punto final para ayudar a reducir la posibilidad de que alguien más intente invocar su punto final.
* Pagerduty: Defina una notificación de tipo *PagerDuty* para enviar los datos de alerta correspondientes a una métrica al sistema de gestión de incidentes PagerDuty. 

Por ejemplo, en la tabla siguiente encontrará ejemplos de plantillas de notificación:

<table>
  <caption></caption>
  <tr>
    <th>Tipo</th>
	<th>Plantilla</th>
	<th>Ejemplo</th>
  </tr>
  <tr>
    <td>Correo electrónico</td>
	<td>
	```
	{
	"name": "Template_Name",
	"type": "Email",
	"description" : "Description",
	"detail": "EmailAddress"
	}
	```
	{: codeblock}
	</td>
	<td>
	```
	{
	"name": "my-email",
	"type": "Email",
	"description" : "Send email notification when there is an infrastructure problem.",
    "detail": "xxx@yyy.com"
    }
	```
	{: screen}
	</td>
  </tr>
  <tr>
    <td>Webhook</td>
	<td>
	```
	{
	"name": "Template_Name",
	"type": "Webhook",
	"description" : "Description",
	"detail": "Endpoint"
	}
	```
	{: codeblock}
	</td>
	<td>
	```
	{
	"name": "my-webhook",
	"type": "Webhook",
	"description" : "Fire a webhook when there is an infrastructure problem..",
    "detail": "https://myendpoint.bluemix.net?key=abcd1234"
    }
	```
	{: screen}
	</td>
  </tr>
  <tr>
    <td>Pagerduty</td>
	<td>
	```
	{
	"name": "Template_Name",
	"type": "PagerDuty",
	"description" : "Description",
	"detail": "Pagerduty_APIkey"
	}
	```
	{: codeblock}
	</td>
	<td>
	```
	{
	"name": "my-pagerduty",
	"type": "PagerDuty",
	"description" : "Fire a PagerDuty alert when there is an infrastructure problem..",
    "detail": "abcd1234"
    }
	```
	{: screen}
	</td>
  </tr>
</table>

Donde

* *Template_Name* define el nombre de la plantilla de notificación.
* *Description* explica cuándo se utiliza este tipo de notificación.
* *EmailAddress* define la dirección de correo electrónico del destinatario de la notificación.
* *Endpoint* define el URL en el que se debe realizar la acción POST. Es el URL que puede admitir la solicitud POST. 
* *Pagerduty_APIkey* define una clave de API exclusiva. Esta clave de API la genera el administrador o el propietario de la cuenta de PagerDuty.


Siga los pasos siguientes para crear una plantilla de notificación:

1. Cree un directorio para almacenar los recursos del servicio {{site.data.keyword.monitoringshort}}, por ejemplo *cloud-monitoring*.

    Por ejemplo, en un sistema Ubuntu, ejecute el siguiente mandato:
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
2. Cree un directorio para almacenar sus plantillas de notificación, por ejemplo *notification-templates*.

    Por ejemplo, en un sistema Ubuntu, ejecute el siguiente mandato:
	
	```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	Cambie a este directorio:
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
3. Crear una plantilla de notificación en un directorio local del sistema.

    Por ejemplo, cree el archivo **email-template.json** para una plantilla de notificación por correo electrónico, utilizando, por ejemplo, el editor vi: 
	
	```
	{
    "name": "email_template",
    "type": "Email",
    "description" : "Send email to manager of department X when ....",
    "detail": "xxx@yyy"
    }
	```
	{: codeblock}
	


## Creación de una notificación
{: #create}

Para
crear una notificación, siga los pasos siguientes:

1. Cree un archivo de notificación.

    1. Utilice una plantilla de notificación para crear el archivo. Para obtener más información, consulte [Creación de una plantilla de notificación](#template).
	
	2. Actualice el archivo:
	
	    * Escriba un nombre exclusivo en el campo *name*. El valor de este campo se utiliza más adelante para actualizar, suprimir y mostrar los detalles de la notificación.
		* Entre una descripción.
		* Escriba una dirección de correo electrónico, punto final de URL o clave de PagerDuty válidos.
		
	3. Guarde el archivo. Por ejemplo, utilice un prefijo como *s-* para las notificaciones que defina para las consultas que se ejecutan en un espacio de {{site.data.keyword.Bluemix_notm}}.
	
	    **Consejo:** Cree su archivo de notificaciones en el siguiente directorio: *~/cloud-monitoring/notifications/* para agrupar los recursos del servicio {{site.data.keyword.monitoringshort_notm}}. 
	
2. Inicie la sesión en una región, organización y espacio de {{site.data.keyword.Bluemix_notm}}. Ejecute el mandato:

    Por ejemplo, para iniciar sesión en la región EE. UU. sur, ejecute el siguiente mandato:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Siga las instrucciones. Escriba sus credenciales de {{site.data.keyword.Bluemix_notm}}, seleccione una organización y un espacio.

3. Obtenga la señal de autenticación o la clave de API.

    * Para la autenticación de IAM, consulte [Obtención de la señal de IAM utilizando la CLI de Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) u [Obtención de la clave de API de IAM utilizando la CLI de Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Para la autenticación de UAA, consulte [Obtención de la señal de UAA mediante la CLI de Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) o bien [Obtención de la señal de UAA mediante la API REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

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
	
	A continuación, exporte la variable *Token*:
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Nota:** La señal excluye *Bearer*.
		
4. Obtenga el GUID del espacio. El GUID debe tener el prefijo *s-* para identificar un espacio.

    Ejecute el mandato siguiente:
	
	```
	bx iam space SpaceName --guid
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
	
	A continuación, exporte la variable *Space*:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. Ejecute el siguiente mandato cURL para crear una notificación:

    ```
	curl -XPOST -d @Notification_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: codeblock}
	
	donde
	
	* Notification_File es un archivo JSON que define la notificación.
	
	* El valor *X-Auth-User-Token* es un parámetro que pasa la señal UAA de {{site.data.keyword.Bluemix_notm}}, la señal IAM o la clave de API.
	
	* El valor *Auth_Type* es el prefijo que define el tipo de señal o de clave de API. En la lista siguiente se muestran los valores válidos: *apikey*, *iam* o *uaa*, donde

  * *apikey* identifica que la señal es una clave de API.
		* *iam* identifica que la señal especificada es una señal generada por IAM.
		* *uaa* identifica que la señal especificada es una señal generada por UAA.
	
	* *X-Auth-Scope-Id* es un parámetro que pasa el GUID de espacio. El GUID debe tener el prefijo *s-* para identificar un espacio. Esta cabecera es necesaria si utiliza una autenticación de señal de UAA. Si utiliza una señal de autenticación de IAM, esta cabecera es opcional y se utiliza la información de dominio enlazada con la señal.
	
	* La señal es la señal de UAA, la señal de IAM o la clave de API.
	
	* Space es el GUID del espacio. Solo es necesario cuando se utiliza una señal de UAA.
	
    Por ejemplo, 	
	
	```
	curl -XPOST -d @s-email-dep-A.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.{DomainName}/v1/alert/notification
	```
	{: screen}

## Supresión de una notificación
{:#delete}

Para suprimir una notificación, siga los pasos siguientes:

1. Inicie la sesión en una región, organización y espacio de {{site.data.keyword.Bluemix_notm}}. Ejecute el mandato:

    Por ejemplo, para iniciar sesión en la región EE. UU. sur, ejecute el siguiente mandato:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Siga las instrucciones. Escriba sus credenciales de {{site.data.keyword.Bluemix_notm}}, seleccione una organización y un espacio.

2. Obtenga la señal de autenticación o la clave de API.

    * Para la autenticación de IAM, consulte [Obtención de la señal de IAM mediante la CLI de Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) o [Generación de una clave de API de IAM mediante la CLI de Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Para la autenticación de UAA, consulte [Obtención de la señal de UAA utilizando la CLI de Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) u [Obtención de la señal de UAA utilizando la API REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

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
	
	A continuación, exporte la variable *Token*:
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Nota:** La señal excluye *Bearer*.
	
3. Obtenga el GUID del espacio. El GUID debe tener el prefijo *s-* para identificar un espacio.

    Ejecute el mandato siguiente:
	
	```
	bx iam space SpaceName --guid
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
	
	A continuación, exporte la variable *Space*:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. Ejecute el siguiente mandato cURL para suprimir una notificación:

    ```
	curl -XDELETE --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification/Notification_Name
	```
	{: codeblock}
	
	donde
	
	* El valor *Auth_Type* es el prefijo que define el tipo de señal o de clave de API. En la lista siguiente se muestran los valores válidos: *apikey*, *iam* o *uaa*, donde

  * *apikey* identifica que la señal es una clave de API.
		* *iam* identifica que la señal especificada es una señal generada por IAM.
		* *uaa* identifica que la señal especificada es una señal generada por UAA.
	
	* El valor *X-Auth-User-Token* es un parámetro que pasa la señal UAA de {{site.data.keyword.Bluemix_notm}}, la señal IAM o la clave de API. La señal o la clave de API deben llevar uno de los siguientes prefijos: *apikey*, *iam* o *uaa*, donde

  * *apikey* identifica que la señal es una clave de API.
		* *iam* identifica que la señal especificada es una señal generada por IAM.
		* *uaa* identifica que la señal especificada es una señal generada por UAA.
		
	* La señal es la señal de UAA, la señal de IAM o la clave de API.
	
	* Space es el GUID del espacio. Solo es necesario cuando se utiliza una señal de UAA.
	
	* Notification_Name es el nombre de la notificación, es decir, el valor del campo *name* cuando genera una lista de las propiedades de una notificación.
	


## Listado de todas las notificaciones
{: #list}

Para obtener una lista de todas las notificaciones, siga estos pasos:

1. Inicie la sesión en una región, organización y espacio de {{site.data.keyword.Bluemix_notm}}. Ejecute el mandato:

    Por ejemplo, para iniciar sesión en la región EE. UU. sur, ejecute el siguiente mandato:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Siga las instrucciones. Escriba sus credenciales de {{site.data.keyword.Bluemix_notm}}, seleccione una organización y un espacio.

2. Obtenga la señal de autenticación o la clave de API.

    * Para la autenticación de IAM, consulte [Obtención de la señal de IAM mediante la CLI de Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) o [Generación de una clave de API de IAM mediante la CLI de Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Para la autenticación de UAA, consulte [Obtención de la señal de UAA utilizando la CLI de Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) u [Obtención de la señal de UAA utilizando la API REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

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
	
	A continuación, exporte la variable *Token*:
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Nota:** La señal excluye *Bearer*.
	
3. Obtenga el GUID del espacio. El GUID debe tener el prefijo *s-* para identificar un espacio.

    Ejecute el mandato siguiente:
	
	```
	bx iam space SpaceName --guid
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
	
	A continuación, exporte la variable *Space*:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. Ejecute el siguiente mandato cURL para obtener una lista de todas las notificaciones:

    ```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notifications
	```
	{: codeblock}
	
	donde
	
	* El valor *Auth_Type* es el prefijo que define el tipo de señal o de clave de API. En la lista siguiente se muestran los valores válidos: *apikey*, *iam* o *uaa*, donde

  * *apikey* identifica que la señal es una clave de API.
		* *iam* identifica que la señal especificada es una señal generada por IAM.
		* *uaa* identifica que la señal especificada es una señal generada por UAA.
		
	* El valor *X-Auth-User-Token* es un parámetro que pasa la señal UAA de {{site.data.keyword.Bluemix_notm}}, la señal IAM o la clave de API. La señal o la clave de API deben llevar uno de los siguientes prefijos: *apikey*, *iam* o *uaa*, donde

  * *apikey* identifica que la señal es una clave de API.
		* *iam* identifica que la señal especificada es una señal generada por IAM.
		* *uaa* identifica que la señal especificada es una señal generada por UAA.
		
	* La señal es la señal de UAA, la señal de IAM o la clave de API.
	
	* Space es el GUID del espacio. Solo es necesario cuando se utiliza una señal de UAA.


			

## Muestra de los detalles de una notificación
{: #show}


Para mostrar la información sobre una notificación, siga los siguientes pasos:

1. Inicie la sesión en una región, organización y espacio de {{site.data.keyword.Bluemix_notm}}. Ejecute el mandato:

    Por ejemplo, para iniciar sesión en la región EE. UU. sur, ejecute el siguiente mandato:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Siga las instrucciones. Escriba sus credenciales de {{site.data.keyword.Bluemix_notm}}, seleccione una organización y un espacio.

2. Obtenga la señal de autenticación o la clave de API.

    * Para la autenticación de IAM, consulte [Obtención de la señal de IAM mediante la CLI de Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) o [Generación de una clave de API de IAM mediante la CLI de Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Para la autenticación de UAA, consulte [Obtención de la señal de UAA utilizando la CLI de Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) u [Obtención de la señal de UAA utilizando la API REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

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
	
	A continuación, exporte la variable *Token*:
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Nota:** La señal excluye *Bearer*.
	
3. Obtenga el GUID del espacio. El GUID debe tener el prefijo *s-* para identificar un espacio.

    Ejecute el mandato siguiente:
	
	```
	bx iam space SpaceName --guid
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
	
	A continuación, exporte la variable *Space*:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. Ejecute el siguiente mandato cURL para mostrar los detalles de una notificación:

    ```
	curl -XGET --header "X-Auth-User-Token:uaa ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification/Notification_Name
	```
	{: codeblock}
	
	donde
	
	* El valor *X-Auth-User-Token* es un parámetro que pasa la señal UAA de {{site.data.keyword.Bluemix_notm}}, la señal IAM o la clave de API. La señal o la clave de API deben llevar uno de los siguientes prefijos: *apikey*, *iam* o *uaa*, donde

  * *apikey* identifica que la señal es una clave de API.
		* *iam* identifica que la señal especificada es una señal generada por IAM.
		* *uaa* identifica que la señal especificada es una señal generada por UAA.
		
	* La señal es la señal de UAA, la señal de IAM o la clave de API.
	
	* Space es el GUID del espacio. Solo es necesario cuando se utiliza una señal de UAA.
	
	* Notification_Name es el nombre de la notificación, es decir, el valor del campo *name* cuando genera una lista de las propiedades de una notificación.
	
     
		

			
## Prueba de una notificación
{: #test}	
			
Para probar una notificación, siga los pasos siguientes:

1. Inicie la sesión en una región, organización y espacio de {{site.data.keyword.Bluemix_notm}}. Ejecute el mandato:

    Por ejemplo, para iniciar sesión en la región EE. UU. sur, ejecute el siguiente mandato:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Siga las instrucciones. Escriba sus credenciales de {{site.data.keyword.Bluemix_notm}}, seleccione una organización y un espacio.

2. Obtenga la señal de autenticación o la clave de API.

    * Para la autenticación de IAM, consulte [Obtención de la señal de IAM mediante la CLI de Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) o [Generación de una clave de API de IAM mediante la CLI de Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Para la autenticación de UAA, consulte [Obtención de la señal de UAA utilizando la CLI de Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) u [Obtención de la señal de UAA utilizando la API REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

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
	
	A continuación, exporte la variable *Token*:
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Nota:** La señal excluye *Bearer*.

3. Obtenga el GUID del espacio. El GUID debe tener el prefijo *s-* para identificar un espacio.

    Ejecute el mandato siguiente:
	
	```
	bx iam space SpaceName --guid
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
	
	A continuación, exporte la variable *Space*:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. Ejecute el siguiente mandato cURL para probar una notificación:

    ```
	curl -XPOST --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification/test/Notification_Name
	```
	{: codeblock}
	
	donde
	
	* El valor *Auth_Type* es el prefijo que define el tipo de señal o de clave de API. En la lista siguiente se muestran los valores válidos: *apikey*, *iam* o *uaa*, donde

  * *apikey* identifica que la señal es una clave de API.
		* *iam* identifica que la señal especificada es una señal generada por IAM.
		* *uaa* identifica que la señal especificada es una señal generada por UAA.
	
	* El valor *X-Auth-User-Token* es un parámetro que pasa la señal UAA de {{site.data.keyword.Bluemix_notm}}, la señal IAM o la clave de API. La señal o la clave de API deben llevar uno de los siguientes prefijos: *apikey*, *iam* o *uaa*, donde

  * *apikey* identifica que la señal es una clave de API.
		* *iam* identifica que la señal especificada es una señal generada por IAM.
		* *uaa* identifica que la señal especificada es una señal generada por UAA.
		
	* La señal es la señal de UAA, la señal de IAM o la clave de API.
	
	* Space es el GUID del espacio. Solo es necesario cuando se utiliza una señal de UAA.
	
	* Notification_Name es el nombre de la notificación, es decir, el valor del campo *name* cuando genera una lista de las propiedades de una notificación.
	


## Actualización de una notificación
{: #update}

Para actualizar una notificación, siga los pasos siguientes:

1. Inicie la sesión en una región, organización y espacio de {{site.data.keyword.Bluemix_notm}}. Ejecute el mandato:

    Por ejemplo, para iniciar sesión en la región EE. UU. sur, ejecute el siguiente mandato:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Siga las instrucciones. Escriba sus credenciales de {{site.data.keyword.Bluemix_notm}}, seleccione una organización y un espacio.

2. Obtenga la señal de autenticación o la clave de API.

    * Para la autenticación de IAM, consulte [Obtención de la señal de IAM mediante la CLI de Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) o [Generación de una clave de API de IAM mediante la CLI de Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Para la autenticación de UAA, consulte [Obtención de la señal de UAA utilizando la CLI de Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) u [Obtención de la señal de UAA utilizando la API REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

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
	
	A continuación, exporte la variable *Token*:
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Nota:** La señal excluye *Bearer*.
	
3. Obtenga el GUID del espacio. El GUID debe tener el prefijo *s-* para identificar un espacio.

    Ejecute el mandato siguiente:
	
	```
	bx iam space SpaceName --guid
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
	
	A continuación, exporte la variable *Space*:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. Ejecute el siguiente mandato cURL para actualizar una notificación:

    ```
	curl -XPUT -d @Notification_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: codeblock}
	
	donde
	
	* Notification_File es un archivo JSON que define la notificación.
	
	* El valor *Auth_Type* es el prefijo que define el tipo de señal o de clave de API. En la lista siguiente se muestran los valores válidos: *apikey*, *iam* o *uaa*, donde

  * *apikey* identifica que la señal es una clave de API.
		* *iam* identifica que la señal especificada es una señal generada por IAM.
		* *uaa* identifica que la señal especificada es una señal generada por UAA.
	
	* El valor *X-Auth-User-Token* es un parámetro que pasa la señal de UAA, la señal de IAM o la clave de API de bluemix. La señal o la clave de API deben llevar uno de los siguientes prefijos: *apikey*, *iam* o *uaa*, donde

  * *apikey* identifica que la señal es una clave de API.
		* *iam* identifica que la señal especificada es una señal generada por IAM.
		* *uaa* identifica que la señal especificada es una señal generada por UAA.
		
	* La señal es la señal de UAA, la señal de IAM o la clave de API.
	
	* Space es el GUID del espacio. Solo es necesario cuando se utiliza una señal de UAA.

	
        

