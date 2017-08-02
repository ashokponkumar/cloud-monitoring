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


# Preguntas frecuentes y respuestas sobre el uso de la CLI de Bluemix
{: #cli_qa}

Aquí encontrará las respuestas a preguntas comunes sobre el uso de la CLI de {{site.data.keyword.Bluemix}} con el servicio {{site.data.keyword.monitoringshort}}. 
{:shortdesc}

* [¿Cómo se instala la CLI de {{site.data.keyword.Bluemix_notm}}?](#install_bmx_cli)
* [¿Cómo se obtiene el GUID de una cuenta?](#account_guid)
* [¿Cómo se obtiene el GUID de una organización?](#org_guid)
* [¿Cómo se obtiene el GUID de un espacio?](#space_guid)


## ¿Cómo se instala la CLI de Bluemix?
{: #install_bmx_cli}

Siga estos pasos para instalar la CLI de {{site.data.keyword.Bluemix_notm}}:

1. Descargue la CLI.

    Por ejemplo, para instalar el paquete de la CLI de {{site.data.keyword.Bluemix_notm}} en un sistema Ubuntu, descargue el paquete de la CLI de [{{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](http://clis.ng.bluemix.net/ui/home.html "Icono de enlace externo"){: new_window}. 

2. Ejecute el siguiente mandato para extraer el paquete de la CLI de {{site.data.keyword.Bluemix_notm}}:
    
    ```
    tar -xvf Bluemix_CLI_0.5.4_amd64.tar.gz
    ```
    {: codeblock}
    
3. Vaya al directorio *Bluemix_CLI* y ejecute el mandato `./install_bluemix_cli` con el permiso root para instalar el plugin de CF. Puede ejecutar el mandato como usuario root o puede utilizar el mandato sudo para obtener el permiso root. Por ejemplo:
    
    ```
    cd Bluemix_CLI
    ```
    {: codeblock}
    
    ```
    sudo ./install_bluemix_cli
    ```
    {: codeblock}
    
4. Verifique la instalación del plugin de CF. Por ejemplo, compruebe la versión del plugin de CF. Ejecute el mandato siguiente:
    
    ```
    cf -v
    ```
    {: codeblock}
    
    La salida tiene este aspecto:
    
    ```
    cf version 6.25.0+787326d.2017-02-28
    ```
    {: screen}
    
5. Verifique la instalación de la CLI de {{site.data.keyword.Bluemix_notm}}. Por ejemplo, compruebe la versión del plugin. Ejecute el mandato siguiente:
    
    ```
    bx -version
    ```
    {: codeblock}
    
    La salida tiene este aspecto:
    
    ```
    bx version 0.5.4+ae22935-2017-05-18T03:55:55+00:00
    ```
    {: screen}
	
## ¿Cómo se obtiene el GUID de una cuenta?
{: #account_guid}
	
Siga estos pasos para obtener el GUID de una cuenta:
	
1. Inicie la sesión en una región, organización y espacio de {{site.data.keyword.Bluemix_notm}}. Ejecute el mandato:

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    Siga las instrucciones. Escriba sus credenciales de {{site.data.keyword.Bluemix_notm}}, seleccione una organización y un espacio.
	
2. Ejecute el mandato `bx iam accounts` para obtener el GUID de una cuenta.

    ```
	bx iam accounts
	```
	{: codeblock} 
	
	Por ejemplo, para recuperar todas las cuentas con sus correspondientes GUID para el usuario xxx@yyy.com, ejecute el mandato:
	
	```
	bx iam accounts
	Retrieving all accounts of xxx@yyy.com...
    OK
    Account GUID                       Name                               Type    State    Owner User ID
    12345123451234512345123451234512   A Account                          TRIAL   ACTIVE   xxx@yyy.com
    23456234562345622456234561234561   B Account                          TRIAL   ACTIVE   zzz@yyy.com   
	```
	{: screen}

	
## ¿Cómo se obtiene el GUID de una organización?
{: #org_guid}

Siga los pasos siguientes para obtener el GUID de una organización:
	
1. Inicie la sesión en una región, organización y espacio de {{site.data.keyword.Bluemix_notm}}. Ejecute el mandato:

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    Siga las instrucciones. Escriba sus credenciales de {{site.data.keyword.Bluemix_notm}}, seleccione una organización y un espacio.

2. Ejecute el mandato `bx iam org` para obtener el GUID de la organización. 

    ```
    bx iam org NAME --guid
    ```
    {: codeblock}
	
    donde NAME es el nombre de la organización de {{site.data.keyword.Bluemix_notm}}.
		
## ¿Cómo se obtiene el GUID de un espacio?
{: #space_guid}
	
Siga estos pasos para obtener el GUID de un espacio:
	
1. Inicie la sesión en una región, organización y espacio de {{site.data.keyword.Bluemix_notm}}. Ejecute el mandato:

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    Siga las instrucciones. Escriba sus credenciales de {{site.data.keyword.Bluemix_notm}}, seleccione una organización y un espacio.
	
2. Ejecute el mandato `bx iam space` para obtener el GUID del espacio. 

    ```
    bx iam space NAME --guid
    ```
    {: codeblock}
	
    donde NAME es el nombre de un espacio de {{site.data.keyword.Bluemix_notm}}. 
	
    Por ejemplo, para obtener el GUID del espacio *dev*, ejecute el siguiente mandato:
	
    ```
    bx iam space dev --guid
    e03afff1-3456-4af6-1234-59treg1b0abc
    ```
    {: screen}




		
		
