---

copyright:
  years: 2017
lastupdated: "2017-06-26"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Introduzione a IBM Cloud Monitoring in Bluemix
{: #getting-started-with-ibm-cloud-monitoring}

In questa esercitazione introduttiva, ti guideremo attraverso i passi necessari per analizzare un contenitore utilizzando il servizio {{site.data.keyword.monitoringlong}}. Impara a cercare e analizzare le metriche del contenitore per un'applicazione distribuita in un cluster Kubernetes.
{:shortdesc}

## Prima di iniziare
{: #prereqs}

Crea un [account Bluemix](https://console.bluemix.net/registration/). Il tuo ID utente deve essere un membro o un proprietario di un account Bluemix con le autorizzazioni per creare cluster Kubernetes, distribuire applicazioni nei cluster ed eseguire query nei log di Bluemix per l'analisi avanzata in Kibana.

Apri una sessione del terminale dalla quale puoi gestire il cluster Kubernetes e distribuire le applicazioni dalla riga di comando. In questa esercitazione, gli esempi vengono forniti per un sistema Ubuntu Linux.

[Installa i plug-in della CLI](/docs/containers/cs_cli_install.html#cs_cli_install_steps) nel tuo ambiente locale per gestire il servizio IBM Bluemix Container dalla riga di comando. 



## Passo 1: Distribuzione di un'applicazione in un contenitore
{: #step1}

Completa la seguente procedura per distribuire un contenitore in un cluster Kubernetes:

1. [Crea un cluster Kubernetes](/docs/containers/cs_cluster.html#cs_cluster_ui).

2. [Configura il contesto del cluster](/docs/containers/cs_cli_install.html#cs_cli_configure) in un terminale Linux. Una volta impostato il contesto, puoi gestire il cluster Kubernetes e distribuire l'applicazione nel cluster.

3. Distribuisci ed esegui un'applicazione di esempio nel cluster Kubernetes. [Completa i passi per la lezione 1](/docs/containers/cs_tutorials.html#cs_apps_tutorial).

    l'applicazione è un'applicazione Node.js Hello World:

    ```
    var express = require('express')
    var app = express()

    app.get('/', function(req, res) {
      res.send('Hello world! Your app is up and running in a cluster!\n')
    })
    app.listen(8080, function() {
      console.log('Sample app is listening on port 8080.')
    })
    ```

    Quando l'applicazione viene distribuita, la raccolta delle metriche viene abilitata automaticamente.



## Passo 2: Passaggio al dashboard Grafana
{: #step2}

Avvia Grafana da un browser. 

Per analizzare le metriche per un cluster, devi accedere a Grafana nella regione pubblica del cloud in cui viene creato il cluster. 
    
Quindi, da un browser, avvia il seguente URL per aprire Grafana: `https://metrics.ng.bluemix.net/`
    
    
## Passo 3: Analisi delle metriche in Grafana
{: #step3}

Completa la seguente procedura per creare un dashboard Grafana:
    
1. Crea un nuovo dashboard.

    * Seleziona il commutatore della barra dei menu laterale ![Barra di menu laterale Grafana](images/grafana_settings.gif "Barra dei menu laterale Grafana"). 
    * Seleziona **Dashboard**. 
    * Fai clic su **Nuovo**
    
    Si aprirà un dashboard. Il dashboard include una riga vuota che è pronta per la configurazione. 
    
    ![Dashboard Grafana](images/grafana4_f1.gif "Dashboard Grafana")
    
     In Grafana, aggiungi le righe per dividere il dashboard in sezioni. Una riga raggruppa 1 o più pannelli. In una riga, un pannello è l'unità di visualizzazione più piccola che puoi configurare per visualizzare i dati per una metrica, ad esempio puoi scegliere un pannello grafico o un pannello tabella. Puoi trascinare e rilasciare i pannelli per riorganizzarli in un dashboard. I dati visualizzati da un pannello vengono configurati tramite query. In un pannello, puoi definire una o più query. Ogni query rappresenta una serie di dati diversa. Puoi anche impostare l'intervallo di tempo per un pannello. Normalmente, l'intervallo di tempo è impostato dal selezionatore di tempo del *Dashboard*.
    
2. Aggiungi un pannello *Grafico* per monitorare i nanosecondi del tempo cpu in tutti i core per un contenitore.
    
    1. Seleziona **Grafico**.
    
    2. Fai clic sul titolo del grafico e quindi seleziona **modifica**.
    
        Si apre la scheda *Metriche*. Qui puoi vedere l'origine dati predefinita.
    
        ![Pannello grafico che include le schede di configurazione](images/grafana4_f2.gif "Pannello grafico che include le schede di configurazione")
    
3. Definisci la query che filtra i dati da visualizzare nel grafico. 

    La seguente tabella elenca i diversi campi necessari per configurare una query che filtra i dati per una metrica del contenitore:

    <table>
      <caption>Tabella 1. Campi di query Grafana per i contenitori</caption>
      <tr>
        <th align="center">Campo</th>
        <th align="center">Descrizione</th>
        <th align="center">Valori validi</th>
      </tr>
      <tr>
        <td>Prefisso</td>
        <td>Prefisso per le metriche dei contenitori. <br><br>Questo prefisso si applica ai dati raccolti per i contenitori distribuiti in un cluster Kubernetes.</td>
        <td>`crn`</td>
      </tr>
      <tr>
        <td>Versione</td>
        <td>Versione dei dati di metrica raccolti.</td>
        <td>`v1`</td>
      </tr>
      <tr>
        <td>Provider</td>
        <td>Provider cloud in cui vengono raccolti i dati.</td>
        <td>`bluemix`</td>
      </tr>
      <tr>
        <td>Tipo</td>
        <td>Ambiente cloud in cui vengono raccolti i dati.</td>
        <td>`public`</td>
      </tr>
      <tr>
        <td>Origine</td>
        <td>Infrastruttura cloud in cui vengono raccolte le metriche.</td>
        <td>`containers-kubernetes`</td>
      </tr>
      <tr>
        <td>Regione</td>
        <td>Regione cloud in cui vengono raccolte le metriche.</td>
        <td>* `ng` <br>* `eu-gb` <br>* `eu-de` </td>
      </tr>
      <tr>
        <td>Account</td>
        <td>GUID dell'account in cui vengono raccolte le metriche. <br>Il formato di questo campo è il seguente: `a_*ID*` dove ID è il GUID dell'account.</td>
        <td></td>
      </tr>
      <tr>
        <td>Cluster</td>
        <td>GUID del cluster in cui vengono raccolte le metriche.</td>
        <td></td>
      </tr>
      <tr>
        <td>Metrica del contenitore</td>
        <td>Metriche raccolte per un contenitore.</td>
        <td>* `memory_current` <br>* `memory_limit` <br>* `cpu_usage` <br>* `cpu_usage_pct` <br>* `cpu_num_cores`</td>
      </tr>
      <tr>
        <td>Contenitore in un pod</td>
        <td>Combinazione di nomi di risorse e di GUID Kubernetes richiesti per identificare in modo univoco un contenitore eseguito in un pod. <br> Il formato di questo campo è il seguente: *{namespace}_#{pod_name}_#{container_name}_#{container_id}* <br><br>**Nota:** quando esamini l'elenco delle opzioni disponibili per questa voce nella query, nota che esiste anche una voce con il seguente formato: *{namespace}_#{pod_name}_#{container_name}_POD_#{container_id}*. Queste voci corrispondono agli ID contenitore interni creati da Kubernetes.</td>
        <td></td>
      </tr>
      <tr>
        <td>Funzioni</td>
        <td>Funzioni della query che puoi selezionare per visualizzare una metrica del contenitore nel pannello. <br>Per ulteriori informazioni, vedi [Funzioni![(Icona link esterno)](../icons/launch-glyph.svg "Icona link esterno")](http://graphite.readthedocs.io/en/latest/functions.html "Icona link esterno"){: new_window}</td>
        <td></td>
      </tr>
    </table>
    
    La seguente immagine mostra come viene visualizzata la query in Grafana quando la configuri:
    
    ![Query di esempio](images/grafana4_query_f3.gif "Query di esempio")
    
    Completa la seguente procedura per definire la query:
    
    Nella scheda *Metrics*, seleziona **Add query**. <br>Viene aggiunta una voce di query. Ogni query viene etichettata con una lettera.
    
    ![Nuova voce di query](images/grafana4_query_f1.gif "Nuova voce di query")
        
    1. Fai clic su **Select metric** e scegli `crn`.
    
    2. Fai clic su **Select metric** e scegli `v1`.
    
    3. Fai clic su **Select metric** e scegli `bluemix`.
    
    4. Fai clic su **Select metric** e scegli `public`.
    
    5. Fai clic su **Select metric** e scegli `containers-kubernetes`.
    
    6. Fai clic su **Select metric** e scegli la regione da cui stai lavorando, ad esempio `us-south`.
    
    7. Fai clic su **Select metric** e scegli l'ID account per cui vuoi visualizzare i dati, ad esempio `a_91d1d1exxxxxxx4df920bbd06461b066`
    
    8. Fai clic su **Select metric** e scegli l'ID cluster.
    
    9. Fai clic su **Select metric** e scegli una metrica del contenitore. Per monitorare l'*utilizzo della CPU* di un contenitore, scegli `container-metric-cpu_usage`.
    
    10. Fai clic su **Select metric** e scegli l'ID che corrisponde al contenitore per il quale vuoi monitorare l'utilizzo della CPU, ad esempio `default_hello-world-deployment-3355293961-0fwkg_hello-world-deployment_ad5eb446a493db31f1d9eb158f5de915fc063d6c136823488b694e63bb00aa57`.
    
    11. Fai clic sull'immagine del segno più ![Icone Aggiungi](images/grafana_plus_image.gif "Immagine del segno più") e scegli una funzione. Puoi aggiungere una funzione per trasformare, combinare ed eseguire calcoli sui dati disponibili per una metrica.
        
        Ad esempio, puoi aggiungere la funzione **alias(newName)** per aggiungere un alias per una metrica. Questo alias viene utilizzato per stampare una stringa anziché il nome della metrica nella legenda visualizzata nel grafico.
        
        Per aggiungere un alias per la tua metrica, completa la seguente procedura:
        
        1. Fai clic sul simbolo più.
        2. Seleziona **Special**. 
        3 Seleziona **alias**.
        4. Immetti una stringa, ad esempio `My sample metric`.
        
4. Salva il dashboard per un successivo riutilizzo. 

    1. Fai clic sull'immagine di salvataggio del dashboard ![Immagine di salvataggio del dashboard](images/grafana_save_image.gif "Immagine di salvataggio del dashboard"). 
    
        ![Salva dashboard](images/grafana_save_dashboard.gif "Salva dashboard")
    
    2. Immetti il nome del dashboard.
    3. Fai clic su **Salva**.


## Passi successivi
{: #next_steps}

Definisci un avviso per una metrica. Per ulteriori informazioni, vedi [Configurazione degli avvisi](/docs/services/cloud-monitoring/config_alerts_ov.html#config_alerts_ov).



