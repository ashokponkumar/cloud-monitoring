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


# Autenticación mediante el modelo IAM de Bluemix
{: #auth_iam}

Utilice el modelo IAM de {{site.data.keyword.Bluemix}} para obtener una señal de autenticación que pueda utilizar para acceder a las métricas almacenadas en el servicio {{site.data.keyword.monitoringshort}} o para obtener una clave de API. La señal tiene un tiempo de caducidad. La clave de API no caduca.
{:shortdesc}


## Obtención de una señal IAM mediante la CLI de Bluemix 
{: #iam_token_cli}

Para obtener la señal de autorización mediante la CLI de {{site.data.keyword.Bluemix_notm}}, siga los pasos siguientes:

1. Instale la CLI de {{site.data.keyword.Bluemix_notm}}.

   Para obtener más información, consulte [Instalación de la CLI de {{site.data.keyword.Bluemix_notm}} ](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa).
   
   Si la CLI está instalada, continúe en el paso siguiente.
    
2. Inicie la sesión en una región, organización y espacio de {{site.data.keyword.Bluemix_notm}}. Ejecute el mandato:

    Por ejemplo, para la región EE. UU. sur, ejecute el siguiente mandato:
	
    ```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Siga las instrucciones. Escriba sus credenciales de {{site.data.keyword.Bluemix_notm}}, seleccione una organización y un espacio.
	
3. Ejecute el mandato `bx iam oauth-tokens` para obtener la señal IAM.

    ```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	El resultado devuelve la señal IAM que debe utilizar para autenticar su ID de usuario en dicho espacio y organización.

**Nota: ** Cuando utilice la señal, elimine *Bearer* del valor de la señal que pasa en una llamada de API.
		
		
## Generación de una clave de API IAM mediante la CLI de Bluemix
{: #iam_apikey_cli}

Siga estos pasos para generar una clave de API IAM mediante la CLI de {{site.data.keyword.Bluemix_notm}}:

1. Instale la CLI de {{site.data.keyword.Bluemix_notm}}.

   Para obtener más información, consulte [Instalación de la CLI de {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa).

   Si la CLI está instalada, continúe en el paso siguiente.
	
2. Inicie una sesión en {{site.data.keyword.Bluemix_notm}} mediante el mandato `bx login` de la CLI de {{site.data.keyword.Bluemix_notm}}:

    Por ejemplo, para la región EE. UU. sur, ejecute el siguiente mandato:
	
    ```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Siga las instrucciones. Escriba sus credenciales de {{site.data.keyword.Bluemix_notm}}, seleccione una organización y un espacio.
 
3. Ejecute el mandato `bx iam api-key-create` para crear una clave de API IAM.

    ```
    bx iam api-key-create NAME [-d DESCRIPTION][-f, --file FILE]
	```
	{: codeblock} 
	
	donde
	
	* NAME es el nombre de la clave de API que se va a crear.
	* DESCRIPTION es el texto que utilizará para describir la clave de API.
	* FILE es el nombre del archivo en el que se guarda la clave.
	
    Por ejemplo, para crear una clave de API *MyKey*, ejecute el siguiente mandato:
	
	```
	bx iam api-key-create MyKey -d "This is my API key for service X" 
	```
	{: screen}
	
	
	
	
## Generación de una clave de API IAM mediante la interfaz de usuario de Bluemix
{: #iam_apikey_ui}

Siga estos pasos para generar una clave de API IAM mediante la interfaz de usuario de {{site.data.keyword.Bluemix_notm}}:

1. Inicie la sesión en la consola de {{site.data.keyword.Bluemix_notm}}. 

2. En la barra de menús de la consola, pulse **Gestionar > Seguridad > Claves de API de Bluemix**.

3. Pulse **Crear clave de API**.

4. Especifique un nombre y una descripción para su clave de API. Luego pulse **Crear clave de API**.

5. Guarde la clave de API. Pulse **Descargar clave de API**.

    Pulse **Mostrar** para ver la clave de API.  

**Nota:** La clave de API únicamente está disponible para ser mostrada o descargada en el momento de su creación. Si se pierde la clave de API, deberá crear una nueva clave de API.  


	
## Revocación de una clave de API mediante la interfaz de usuario de Bluemix:
{: #revoke_ui}
	
Siga estos pasos para revocar una clave de API IAM mediante la interfaz de usuario de {{site.data.keyword.Bluemix_notm}}:

1. Inicie la sesión en la consola de {{site.data.keyword.Bluemix_notm}}.

2. En la barra de menús de la consola, pulse **Gestionar > Seguridad > Claves de API de Bluemix**.

3. Pulse **Acciones** y, a continuación, **Suprimir clave**.





	

	
	
	
	
	
	
