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


# Domande e risposte frequenti sull'utilizzo di Grafana
{: #grafana_qa}

Queste sono le risposte alle domande più frequenti sull'utilizzo di Grafana con il servizio {{site.data.keyword.monitoringshort}}. 
{:shortdesc}

* [Quando accedo all'IU web del servizio di monitoraggio ricevo un errore 404](/docs/services/cloud-monitoring/qa/grafana_qa.html#404)
* [Ho appena caricato i dati json per un dashboard Grafana, perché non vedo più la barra di scorrimento?](/docs/services/cloud-monitoring/qa/grafana_qa.html#2)


## Quando accedo all'IU web del servizio di monitoraggio utilizzando il modello di autenticazione UUA ricevo un errore 404
{: #404}

Se ricevi un errore 404 quando provi ad accedere all'IU web del servizio {{site.data.keyword.monitoringshort}} (Grafana), verifica che lo spazio esista e che tu abbia accesso ad esso.

Se utilizzi il [modello di autenticazione UAA](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_uaa) per accedere al servizio {{site.data.keyword.monitoringshort}}, devi utilizzare lo stesso ID utente e password utilizzati per accedere alla console {{site.data.keyword.Bluemix_notm}}. 

Per verificare di disporre dell'accesso all'account, all'organizzazione e allo spazio a cui vuoi connetterti, accedi alla console {{site.data.keyword.Bluemix_notm}} e passa allo spazio. 

Per verificare di avere accesso a tale spazio, puoi utilizzare anche la riga di comando. Immetti il seguente comando per accedere a una regione, un'organizzazione e uno spazio {{site.data.keyword.Bluemix_notm}}:

```
bx login -a https://api.ng.bluemix.net
```
{: codeblock}

Segui le istruzioni. Immetti le tue credenziali {{site.data.keyword.Bluemix_notm}}, seleziona un'organizzazione e uno spazio.


## Ho appena caricato i dati JSON per un dashboard Grafana, perché non vedo più la barra di scorrimento?
{: #2}

Esiste un bug in Grafana che nasconde la barra di scorrimento dopo aver importato un dashboard. 

Per visualizzare la barra di scorrimento, salva il dashboard dopo averlo importato. 








