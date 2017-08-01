---

copyright:
  years: 2017

lastupdated: "2017-05-26"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

# Creazione di un dashboard Grafana
{:#create_grafana_dashboard}

Crea un dashboard Grafana personalizzato per visualizzare le metriche relative a tutti i contenitori eseguiti in uno spazio della tua organizzazione {{site.data.keyword.Bluemix}}.
{:shortdesc}

Completa la seguente procedura per creare un dashboard Grafana:

1. Avvia Grafana da un browser web. Per ulteriori informazioni, vedi [Passaggio al dashboard Grafana da un browser web](navigating_grafana.html#launch_grafana_from_browser).

2. Salva il dashboard predefinito.

    1. Nella barra degli strumenti, fare clic sull'icona Salva.
    2. Immetti un nome per il dashboard.
    3. Accanto al campo del nome, fai clic sull'icona Salva.
   
3. Disabilita l'aggiornamento automatico della pagina mentre utilizzi il tuo dashboard. 

    1. Fai clic sul selezionatore di tempo nell'intestazione.
    2. Seleziona **Auto-refresh**.
    3. Fai clic su **Off**.
 
 5. Elimina le righe di esempio.
 
     1. Accanto all'intestazione *Welcome to Grafana*, passa il tuo puntatore del mouse sullo slider del menu della riga e quindi fai clic sull'icona del menu della riga che compare.
     2. Fai clic su **Delete row** e quindi su **Yes**.
     
     Ripeti questi passi per rimuovere le righe Documentation Links e First Graph. 
     
     Nell'intestazione della pagina, modifica il selezionatore di tempo per assicurarti di poter visualizzare i dati. Il valore predefinito è 6 hours to a few seconds ago. Seleziona **Last 30d**.
     
6. Aggiungi una visualizzazione.

    * Per aggiungere una visualizzazione di inattività CPU, vedi [Aggiunta di una visualizzazione di inattività CPU](create_grafana_dashboard.html#grafana_add_cpu).
    * Per aggiungere una visualizzazione dell'utilizzo della memoria, vedi [Aggiunta di una visualizzazione dell'utilizzo della memoria](create_grafana_dashboard.html#grafana_add_mem).
        
7. Salva il dashboard personalizzato.

    1. Nella barra degli strumenti, fare clic sull'icona Salva.
    2. Immetti un nome per il dashboard.
    3. Accanto al campo del nome, fai clic sull'icona Salva.
    

## Aggiunta di una visualizzazione di inattività CPU
{:#grafana_add_cpu}

Per aggiungere un grafico di inattività CPU che include i dati da tutti i contenitori nel tuo spazio, completa la seguente procedura:

1. Fai clic sul pulsante **Add a row**. Viene visualizzato uno slider menu della riga al lato della riga.
    
2. Passa il tuo puntatore del mouse sullo slider del menu della riga e quindi fai clic sull'icona del menu della riga che compare.

3. Fai clic su **Add Panel**. Fai quindi clic su **graph**.

4. Fai clic sul titolo del grafico per aprire il menu del pannello e fai clic su **Edit**. 

    Il resto del dashboard è nascosto; sono visualizzati solo l'editor di questo grafico e un'anteprima del grafico.
    
5. Nella scheda Metrics, crea una query per raccogliere i dati del grafico. 

    Ad esempio, quando utilizzi i contenitori distribuiti in un'infrastruttura cloud gestita da {{site.data.keyword.Bluemix_notm}}, il formato della query include l'ID dello spazio, un ID del gruppo di contenitori e un ID dell'istanza del singolo contenitore nel seguente formato: `space_ID.group_ID.instance_ID.metric`
        
    1. Nella riga A, fai clic su **Select metric**. Quindi, seleziona il tuo ID dello spazio.
    2. Fai clic su **Select metric** e seleziona l'asterisco (\*).
    
    Dopo aver selezionato (\*), i dati includono le metriche da ogni gruppo di contenitori nello spazio. Facoltativamente, puoi manipolare questo formato con un'espressione regolare per includere le metriche solo per alcuni contenitori. Ad esempio, se desideri visualizzare le metriche solo per i contenitori singoli ed escludere i gruppi di contenitori, fai clic su **Select metric** e seleziona **0000**.
        
    3. Fai clic su **Select metric** e seleziona di nuovo l'asterisco (\*). Questo formato include le metriche per tutti i contenitori singoli nello spazio.
        
    4. Fai clic su **Select metric** e su **cpu-0**.
        
    5. Fai clic su **Select metric** e su **cpu-idle**. L'anteprima del grafico si aggiorna per visualizzare le metriche che corrispondono alla query.
    
6. Facoltativo: personalizza la visualizzazione del grafico.
    
    * Per fornire un nome al grafico, fai clic sulla scheda General e nel campo Title, immetti un nome per il grafico, ad esempio: CPU Idle
    * Per fornire un'etichetta all'asse verticale, fai clic sulla scheda Axes and grid e nella sezione Left Y Axis, nel campo Label, immetti CPU.
    * Per modificare il colore dello sfondo del grafico, nella sezione Grid threshholds, per il livello 2, fai clic sull'icona del selettore del colore dello sfondo e modificalo in bianco o grigio chiaro.
    
    Nell'intestazione, fai clic su **Back to dashboard**.
    
7. Per salvare le modifiche apportate in questo dashboard, nell'intestazione, fai clic sull'icona di salvataggio e quindi sull'icona di salvataggio nel menu.


## Aggiunta di una visualizzazione dell'utilizzo della memoria
{:#grafana_add_mem}

Completa la seguente procedura per aggiungere una visualizzazione dell'utilizzo della memoria:

1. Fai clic sul pulsante Add a row. Viene visualizzato uno slider menu della riga al lato della riga.
   
2. Passa il tuo puntatore del mouse sullo slider del menu della riga e quindi fai clic sull'icona del menu della riga che compare.

    1. Fai clic su Add Panel > graph.
    2. Fai clic su Edit. Il resto del dashboard è nascosto; sono visualizzati solo l'editor di questo grafico e un'anteprima del grafico.
    
3. Nella scheda Metrics, crea una query per raccogliere i dati del grafico. 

    Ad esempio, quando utilizzi i contenitori distribuiti in un'infrastruttura cloud gestita da {{site.data.keyword.Bluemix_notm}}, il formato della query include l'ID dello spazio, un ID del gruppo di contenitori e un ID dell'istanza del singolo contenitore nel seguente formato: space_ID.group_ID.instance_ID.metric
        
    1. Nella riga A, fai clic su **Select metric**. Quindi, seleziona il tuo ID dello spazio.
    2. Fai clic su **Select metric** e seleziona l'asterisco (\*).
    
    Dopo aver selezionato (\*), i dati includono le metriche da ogni gruppo di contenitori nello spazio. Facoltativamente, puoi manipolare questo formato con un'espressione regolare per includere le metriche solo per alcuni contenitori. Ad esempio, se desideri visualizzare le metriche solo per i contenitori singoli ed escludere i gruppi di contenitori, fai clic su **Select metric** e seleziona **0000**.
    
    3. Fai clic su **Select metric** e seleziona di nuovo l'asterisco (\*). Questo formato include le metriche per tutti i contenitori singoli nello spazio.
        
    4. Fai clic su **Select metric** e su **memory**.
        
    5. Fai clic su **Select metric** e su **memory-used**. L'anteprima del grafico si aggiorna per visualizzare le metriche che corrispondono alla query.
    
6. Facoltativo: personalizza la visualizzazione del grafico.
    
    * Per fornire un nome al grafico, fai clic sulla scheda General e nel campo Title, immetti un nome per il grafico, ad esempio: Memory Used
    *  Per modificare il formato dei valori dell'asse verticale, fai clic sulla scheda Axes and grid e nella sezione Left Y Axis, per Format, seleziona bytes.
    * Per fornire un'etichetta all'asse verticale, nel campo Label, immetti Memory.
    * Per modificare il colore dello sfondo del grafico, nella sezione Grid threshholds, per il livello 2, fai clic sull'icona del selettore del colore dello sfondo e modificalo in bianco o grigio chiaro.
    
    Nell'intestazione, fai clic su **Back to dashboard**.

7. Per salvare le modifiche apportate in questo dashboard, nell'intestazione, fai clic sull'icona di salvataggio e quindi sull'icona di salvataggio nel menu.

