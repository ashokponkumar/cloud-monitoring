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


# Seguridad
{: #security_ov}

Puede utilizar distintos métodos de autenticación para enviar métricas al servicio {{site.data.keyword.monitoringshort}}. Los permisos para acceder, modificar y suprimir métricas se controlan mediante roles.
{:shortdesc}

   
## Modelos de autenticación
{: #auth}

Para acceder a las métricas almacenadas en el servicio {{site.data.keyword.monitoringshort}} para un espacio de {{site.data.keyword.Bluemix_notm}}, necesita una señal de autenticación o una clave de API. 

El servicio {{site.data.keyword.monitoringshort}} da soporte a los siguientes modelos de autenticación:

* [Autenticación UAA de {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_uaa)
* [Autenticación IAM de {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/security/auth_iam.html#auth_iam)

Las señales UAA y las señales IAM caducan después de un periodo de tiempo. La clave de API no caduca. 

Si la clave de API está en peligro, puede revocarla suprimiéndola. A continuación, puede volver a crear una nueva. Para obtener más información, consulte [Revocación de una clave de API mediante la interfaz de usuario de Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#revoke_ui). 

El modelo de autenticación IAM ofrece funciones de gestión de API, CLI e interfaz de usuario. Sólo puede utilizar la CLI para gestionar señales UAA.

En la tabla siguiente se muestran los modelos de autenticación que reciben soporte para cada tipo de dominio:

<table>
  <caption>Tabla 1. Modelos de autorización admitidos para cada dominio</caption>
  <tr>
    <th></th>
	<th align="right">Cuenta</th>
    <th align="right">Organización</th>
    <th align="right">Espacio</th>	
  </tr>
  <tr>
    <th align="left">UAA</th>
	<td align="center">No</td>
	<td align="center">Sí</td>
	<td align="center">Sí</td>
  </tr>
  <tr>
    <th align="left">IAM</th>
	<td align="center">Sí</td>
	<td align="center">Sí</td>
	<td align="center">Sí</td>
  </tr>
</table>

El servicio {{site.data.keyword.monitoringshort}} utiliza la característica de control de accesos de IAM para controlar las acciones u operaciones que puede realizar un usuario o servicio sobre el dominio. Por ejemplo, puede definir una política de IAM para permitir que un usuario envíe y cree paneles de control sobre un dominio, pero no que recupere las métricas del dominio.



## Roles UAA de Bluemix
{: #bmx_roles}

En la tabla siguiente se muestran los privilegios de cada rol de {{site.data.keyword.Bluemix_notm}} para trabajar con el servicio {{site.data.keyword.monitoringshort}}:

<table>
  <caption>Tabla 2. Roles y privilegios de {{site.data.keyword.Bluemix_notm}} para trabajar con el servicio {{site.data.keyword.monitoringshort}}.</caption>
  <tr>
    <th>Rol</th>
	<th>Dominio</th>
	<th>Privilegios de acceso</th>
  </tr>
  <tr>
    <td>Gestor</td>
	<td>Organización <br>Espacio</td>
	<td>Todas las API RESTful</td>
  </tr>
  <tr>
    <td>Desarrollador</td>
	<td>Espacio</td>
	<td>Todas las API RESTful</td>
  </tr>
  <tr>
    <td>Auditor</td>
	<td>Organización <br>Espacio</td>
	<td>Solo las API RESTful que realizan operaciones de solo lectura, como por ejemplo consultar métricas.</td>
  </tr>
</table>


## Roles IAM de Bluemix
{: #iam_roles}

En la tabla siguiente se muestra la relación entre la API, una acción de servicio y un rol de IAM que utiliza el servicio {{site.data.keyword.monitoringshort}}.

<table>
  <caption>Tabla 3. Relación entre la API, una acción de servicio y un rol de IAM. </caption>
  <tr>
    <th>API</th>
	<th>Acción</th>
	<th>Rol de IAM</th>
	<th>Descripción</th>
  </tr>
  <tr>
    <td>POST /v1/metrics</td>
    <td>domain.write</td>
	<td>Administrador, Editor, Operador</td>
	<td>Enviar métricas al dominio</td>
  </tr>
  <tr>
    <td>GET /v1/metrics</td>
    <td>domain.render</td>
	<td>Administrador, Editor, Visor</td>
	<td>Recuperar/consultas métricas</td>
  </tr>
  <tr>
    <td>GET /v1/metrics/list</td>
    <td>domain.find</td>
	<td>Administrador, Editor</td>
	<td>Buscar métricas en el dominio</td>
  </tr>
</table>

## Obtención de la señal de seguridad o la clave de API
{: #get_token}

Utilice el modelo UAA de {{site.data.keyword.Bluemix_notm}} para obtener una señal de autenticación que pueda utilizar para acceder a los datos almacenados en el servicio {{site.data.keyword.monitoringshort}} para un espacio de {{site.data.keyword.Bluemix_notm}}. Para obtener la señal de autenticación, utilice la CLI de {{site.data.keyword.Bluemix_notm}} o la API REST `Login`. Para obtener más información, consulte [Obtención de la señal de UAA mediante la CLI de {{site.data.keyword.Bluemix_notm}} ](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_cli)y [Obtención de la señal de UAA mediante la API](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_api).

Utilice el modelo IAM de {{site.data.keyword.Bluemix_notm}} para obtener una señal de autenticación que pueda utilizar para acceder a los datos almacenados en el servicio {{site.data.keyword.monitoringshort}} o para obtener una clave de API. La señal tiene un tiempo de caducidad. La clave de API no caduca. Para obtener más información, consulte [Obtención de una señal IAM mediante la CLI de {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli), [Generación de una clave de API IAM mediante la CLI de {{site.data.keyword.Bluemix_notm}} ](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli) o [Generación de una clave de API IAM mediante la interfaz de usuario de {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_ui).



