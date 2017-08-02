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



# Recuperación del historial de una regla
{: #retrieve_history}


Para recuperar el historial de una alerta, siga estos pasos:

1. Inicie la sesión en una región, organización y espacio de {{site.data.keyword.Bluemix_notm}}. Ejecute el mandato:

    Por ejemplo, para iniciar sesión en la región EE. UU. sur, ejecute el siguiente mandato:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Siga las instrucciones. Escriba sus credenciales de {{site.data.keyword.Bluemix_notm}}, seleccione una organización y un espacio.

2. Obtenga la señal de autenticación o la clave de API.

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
	
4. Ejecute el siguiente mandato cURL para obtener el historial de una alerta:

    ```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/history?rule=Rule_Name
	```
	{: codeblock}
	
	donde
	
	* El valor *X-Auth-User-Token* es un parámetro que pasa la señal UAA {{site.data.keyword.Bluemix_notm}}, señal IAM o clave de API.
	
	* El valor *Auth_Type* es el prefijo que define el tipo de señal o de clave de API. En la lista siguiente se muestran los valores válidos: *apikey*, *iam* o *uaa*, donde

  * *apikey* identifica que la señal es una clave de API.
		* *iam* identifica que la señal especificada es una señal generada por IAM.
		* *uaa* identifica que la señal especificada es una señal generada por UAA.
	
	* *X-Auth-Scope-Id* es un parámetro que pasa el GUID de espacio. El GUID debe tener el prefijo *s-* para identificar un espacio. Esta cabecera es necesaria si utiliza una autenticación de señal de UAA. Si utiliza una señal de autenticación de IAM, esta cabecera es opcional y se utiliza la información de dominio enlazada con la señal.
	
	* La señal es la señal de autenticación de UAA o IAM, o la clave de API.
	
	* Space es el GUID del espacio. Solo es necesario cuando se utiliza una señal de UAA.
	
	* Rule_Name es el nombre de la regla utilizada para desencadenar la alerta. El valor es el especificado en el campo *name*.
	
    
Por ejemplo, el historial de la regla `highNginxCPU` es el siguiente:

```
[
 {
 "rule": "highNginxCPU",
 "timestamp": "2017-04-17T22.23.21.000",
 "value": 99.5,
 "from_level": "OK",
 "to_level": "ERROR"
 },
 {
 "rule": "highNginxCPU",
 "timestamp": "2017-04-17T10.11.12.123",
 "value": 98.5,
 "from_level" : "OK",
 "to_level": "WARN"
 },
 {
 "rule": "highNginxCPU",
 "timestamp": "2017-04-17T10.12.14.000",
 "value": 97.0,
 "from_level" : "WARN",
 "to_level": "OK"
 }
]
```
{: screen}


