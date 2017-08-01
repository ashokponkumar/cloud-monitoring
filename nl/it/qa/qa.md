---

copyright:
  years: 2017

lastupdated: "2017-07-14"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Domande e risposte frequenti
{: #qa}

Queste sono le risposte alle domande più frequenti sul servizio {{site.data.keyword.monitoringshort}}. 
{:shortdesc}

* [Perché non riesco a visualizzare i miei vecchi dati per una metrica per la quale non ho inviato recentemente alcun valore?](#qa3)


## Perché non riesco a visualizzare i miei vecchi dati per una metrica per la quale non ho inviato recentemente alcun valore?
{: #qa3}

{{site.data.keyword.monitoringshort}} elimina tutti i dati per un percorso di metrica che sembra essere di natura transitoria identificando le metriche che non sono state scritte negli ultimi 7 giorni. 

Ad esempio:

* Quando un contenitore viene eliminato, le metriche associate a tale contenitore rimangono per 7 giorni, dopo di che vengono eliminate.
* Se hai un misuratore statsd denominato `<space_id>.test.statsd.gauge-hello` e non scrivi in esso per una settimana, la metrica verrà identificata come transitoria e quindi verrà eliminata insieme a tutte le relative informazioni sulla cronologia. 

