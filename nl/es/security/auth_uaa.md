---

copyright:
  years: 2017, 2018

lastupdated: "2018-02-01"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Obtención de una señal de UAA
{: #auth_uaa}

Utilice el modelo de UAA de {{site.data.keyword.Bluemix}} para obtener una señal de autenticación que puede utilizar para trabajar con el servicio de {{site.data.keyword.monitoringshort}}. Puede obtener la señal de autenticación mediante la CLI de {{site.data.keyword.Bluemix_notm}}, o utilizando la API REST `Login`.
{:shortdesc}

La señal tiene un tiempo de caducidad. 
		
## Obtención de la señal de UAA mediante la CLI de IBM Cloud
{: #uaa_cli}


Para obtener una señal de UAA, siga estos pasos:

1. (Requisito previo) Instalar la CLI de {{site.data.keyword.Bluemix_notm}}.

   Para obtener más información, consulte [Instalación de la CLI de {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa).
   
   Si la CLI está instalada, continúe en el paso siguiente.
    
2. Inicie la sesión en una región, organización y espacio en {{site.data.keyword.Bluemix_notm}}. 

    Para obtener más información, consulte [Cómo iniciar la sesión en {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/qa/cli_qa.html#login).
	
3. Ejecute el mandato `bx iam oauth-tokens` para obtener la señal de UAA.

    ```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	La salida devuelve la señal de UAA.

**Nota:** Cuando utilice la señal, elimine *Bearer* del valor de la señal que pasa en las llamadas de API.
	


	
