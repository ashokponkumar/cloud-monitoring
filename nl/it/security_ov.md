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


# Sicurezza
{: #security_ov}

Per inviare le metriche al servizio {{site.data.keyword.monitoringshort}} puoi utilizzare diversi metodi di autenticazione. Le autorizzazioni per accedere, modificare ed eliminare le metriche vengono controllate utilizzando dei ruoli.
{:shortdesc}

   
## Modelli di autenticazione
{: #auth}

Per accedere alle metriche archiviate nel servizio {{site.data.keyword.monitoringshort}} per uno spazio {{site.data.keyword.Bluemix_notm}}, ti serve un token di autenticazione oppure la chiave API. 

Il servizio {{site.data.keyword.monitoringshort}} supporta i seguenti modelli di autenticazione:

* [Autenticazione UAA {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_uaa)
* [Autenticazione IAM {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/security/auth_iam.html#auth_iam)

Un token UAA e un token IAM scadono dopo un certo periodo di tempo. La chiave API non scade. 

Se la chiave API è compromessa, puoi revocarla eliminandola. Puoi quindi ricrearne una nuova. Per ulteriori informazioni, vedi [Revoca di una chiave API utilizzando l'IU Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#revoke_ui). 

Il modello di autenticazione IAM offre funzionalità di gestione IU, CLI o API. Puoi utilizzare la CLI solo per gestire i token UAA.

La seguente tabella elenca i modelli di autenticazione supportati per ogni tipo di dominio:

<table>
  <caption>Tabella 1. Modelli di autenticazione supportati per ogni dominio</caption>
  <tr>
    <th></th>
	<th align="right">Account</th>
    <th align="right">Organizzazione</th>
    <th align="right">Spazio</th>	
  </tr>
  <tr>
    <th align="left">UAA</th>
	<td align="center">No</td>
	<td align="center">Sì</td>
	<td align="center">Sì</td>
  </tr>
  <tr>
    <th align="left">IAM</th>
	<td align="center">Sì</td>
	<td align="center">Sì</td>
	<td align="center">Sì</td>
  </tr>
</table>

Il servizio {{site.data.keyword.monitoringshort}} utilizza la funzione di controllo dell'accesso IAM per regolare le azioni o le operazioni che possono essere eseguite da un utente o servizio nel dominio. Ad esempio, puoi definire una politica IAM per consentire a un utente di inviare e creare dashboard in un dominio, ma non di recuperare le metriche dal dominio.



## Ruoli UAA Bluemix
{: #bmx_roles}

La seguente tabella elenca i privilegi di ciascun ruolo {{site.data.keyword.Bluemix_notm}} per lavorare con il servizio {{site.data.keyword.monitoringshort}}:

<table>
  <caption>Tabella 2. Ruoli e privilegi {{site.data.keyword.Bluemix_notm}} per lavorare con il servizio {{site.data.keyword.monitoringshort}}.</caption>
  <tr>
    <th>Ruolo</th>
	<th>Dominio</th>
	<th>Privilegi di accesso</th>
  </tr>
  <tr>
    <td>Gestore</td>
	<td>Organizzazione <br>Spazio</td>
	<td>Tutte le API RESTful</td>
  </tr>
  <tr>
    <td>Sviluppatore</td>
	<td>Spazio</td>
	<td>Tutte le API RESTful</td>
  </tr>
  <tr>
    <td>Revisore</td>
	<td>Organizzazione <br>Spazio</td>
	<td>Solo le API RESTful che eseguono operazioni di sola lettura, come le metriche di query.</td>
  </tr>
</table>


## Ruoli IAM Bluemix
{: #iam_roles}

La seguente tabella elenca la relazione tra l'API, un'azione di servizio e un ruolo IAM utilizzato dal servizio {{site.data.keyword.monitoringshort}}.

<table>
  <caption>Tabella 3. Relazione tra l'API, un'azione di servizio e un ruolo IAM. </caption>
  <tr>
    <th>API</th>
	<th>Azione</th>
	<th>Ruolo IAM</th>
	<th>Descrizione</th>
  </tr>
  <tr>
    <td>POST /v1/metrics</td>
    <td>domain.write</td>
	<td>Amministratore, Editor, Operatore</td>
	<td>Inviare metriche al dominio</td>
  </tr>
  <tr>
    <td>GET /v1/metrics</td>
    <td>domain.render</td>
	<td>Amministratore, Editor, Visualizzatore</td>
	<td>Richiamare/eseguire query sulle metriche</td>
  </tr>
  <tr>
    <td>GET /v1/metrics/list</td>
    <td>domain.find</td>
	<td>Amministratore, Editor</td>
	<td>Ricercare le metriche nel dominio</td>
  </tr>
</table>

## Ottenimento del token di sicurezza o della chiave API
{: #get_token}

Utilizza il modello UAA {{site.data.keyword.Bluemix_notm}} per ottenere un token di autenticazione che puoi utilizzare per accedere ai dati archiviati nel servizio {{site.data.keyword.monitoringshort}} per uno spazio in {{site.data.keyword.Bluemix_notm}}. Puoi ottenere il token di autenticazione utilizzando la CLI {{site.data.keyword.Bluemix_notm}} o l'API REST `Login`. Per ulteriori informazioni, vedi [Ottenimento del token UAA utilizzando la CLI {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_cli) e [Ottenimento del token UAA utilizzando l'API](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_api).

Utilizza il modello IAM {{site.data.keyword.Bluemix_notm}} per ottenere un token di autenticazione che puoi utilizzare per accedere ai dati archiviati nel servizio {{site.data.keyword.monitoringshort}} o per ottenere una chiave API. Il token ha un tempo di scadenza, la chiave API non scade. Per ulteriori informazioni, vedi [Ottenimento del token IAM utilizzando la CLI {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli), [Generazione di una chiave API IAM utilizzando la CLI {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli) oppure [Generazione di una chiave API IAM utilizzando l'IU {{site.data.keyword.Bluemix_notm}}](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_ui).



