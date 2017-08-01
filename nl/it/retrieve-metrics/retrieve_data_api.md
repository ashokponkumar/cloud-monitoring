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


# Recupero dei dati dal servizio di monitoraggio
{: #retrieve_data_api}

Puoi recuperare le metriche dal servizio {{site.data.keyword.monitoringshort}} utilizzando [l'API Metriche](https://console.bluemix.net/apidocs/927-ibm-cloud-monitoring-rest-api?&language=node#introduction){: new_window}. Puoi recuperare le metriche da uno spazio {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}


Per definire la serie di dati che intendi recuperare, considera le seguenti informazioni: 

* Devi impostare lo spazio {{site.data.keyword.Bluemix_notm}} da dove vuoi recuperare i dati.

* Devi fornire un token o una chiave API per lavorare con il servizio {{site.data.keyword.monitoringshort}}. 

* Devi specificare un percorso a 1 o più metriche. Per ulteriori informazioni, vedi [Definizione delle metriche](#metrics).

* Facoltativamente, puoi specificare un periodo di tempo personalizzato. Per impostazione predefinita, se non specifichi un periodo di tempo, i dati che recuperi sono quelli corrispondenti alle ultime 24 ore. Per ulteriori informazioni, vedi [Configurazione un periodo di tempo](#time).

**Nota:** puoi recuperare un massimo di 5 destinazioni per richiesta.


## Definizione delle metriche
{: #metrics}

Per recuperare i dati per una metrica, devi specificare il percorso della metrica dalla root della struttura ad albero delle metriche fino alla metrica stessa e devi separare ciascun passo con un punto (.).

Il percorso è specificato nel parametro **Destinazione**. Può essere specificato un massimo di 5 destinazioni per richiesta.  

Puoi, facoltativamente, applicare delle funzioni alla metrica. Per ulteriori informazioni sulle funzioni supportate, vedi il documento relativo alle [funzioni di Graphite 0.9.15![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](http://graphite.readthedocs.io/en/latest/functions.html "Icona link esterno").

Puoi utilizzare dei caratteri jolly per definire un percorso. Utilizzando i caratteri jolly, puoi identificare più di una metrica in un singolo percorso. 

* **Asterisco**: puoi utilizzare un asterisco (*) per una corrispondenza a zero o più caratteri. 

    Puoi avere più di un asterisco in un singolo elemento percorso, ad esempio: `servers.sp*aeg*r.cpu.total.*` Questo esempio restituisce i dati relativi alla CPU totale per tutti i server che corrispondono allo schema.

* **Elenco o intervallo di caratteri**: puoi definire i caratteri tra parentesi ([...]) per specificare una singola posizione di carattere nella stringa percorso. 

    Puoi impostare un intervallo inclusivo di caratteri specificando due caratteri separati da un trattino (-). La corrispondenza sarà con tutti i caratteri compresi tra questi due caratteri. 
	
	Puoi definire uno o più intervalli di caratteri tra parentesi. 
	
	Quando i caratteri all'interno di un set di parentesi non possono essere letti come un intervallo, i caratteri vengono trattati come un elenco di valori. 
	
	Puoi includere un trattino (-) come un carattere in un elenco di valori inserendolo all'inizio a alla fine all'interno delle parentesi.

* **Elenco valori**: puoi impostare dei valori separati da virgole all'interno di parentesi graffe per definire degli elenchi di valori. 

    Ad esempio, per scaricare le metriche per i seguenti percorsi: `servers.myserver.cpu.total.user` e `servers.myserver.cpu.total.system}`, puoi impostare un singolo percorso per entrambi i tipi: `servers.myserver.cpu.total.{user,system}`



## Impostazione di un periodo di tempo
{: #time}

Puoi, facoltativamente, specificare un periodo di tempo utilizzando un tempo relativo o un tempo assoluto. Devi definire i parametri di query **From** e **Until**.  

* Quando l'ora di inizio del periodo di tempo viene omessa, il valore predefinito è 24 ore fa. 

* Quando l'ora di fine del periodo di tempo viene omessa, il valore predefinito è l'ora corrente *now*.

Il tempo relativo (**Relative time**) è sempre preceduto da un segno meno (-) e seguito da un'unità di tempo. 

La seguente tabella elenca le diverse abbreviazioni di tempo relativo che puoi usare:


| Abbreviazione | Descrizione |
|--------------|-------------|
| s            | Secondi      |
| min          | Minuti       |
| h            | Ore          |
| d            | Giorni       |
| w            | Settimane    |
| mon          | Mesi         |
| now          | Ora corrente |


Puoi utilizzare i seguenti formati per definire un tempo assoluto (**Absolute time**): `HH:MM_YYMMDD`, `YYYYMMDD`, `MM/DD/YY`

| Abbreviazione | Descrizione |
|--------------|-------------|
| YYYY         | Anno a quattro cifre |
| MM           | Mese a due cifre (01=Gen ecc.) |
| DD           | Giorno del mese a due cifre (da 01 a 31) |
| hh           | Due cifre dell'ora (da 00 a 23) |
| mm           | Due cifre del minuto (da 00 a 59) |
| ss           | Due cifre del secondo (da 00 a 59) |

**Esempio**

La seguente tabella mostra diversi esempi di come impostare diversi periodi di tempo impostando i parametri *from* e *until*:

<table>
  <caption>Tabella 1. Diversi esempi che mostrano come impostare diversi periodi di tempo impostando i parametri *from* e *until*</caption>
  <tr>
    <th>Esempio</th>
	<th>Descrizione</th>
  </tr>
  <tr>
    <td>&from=monday</td>
	<td>Periodo di tempo che include i dati da lunedì scorso fino a ora.</td>
  </tr>
  <tr>
    <td>&from=20171101&until=20171130</td>
	<td>Periodo di tempo impostato su un mese, ad esempio novembre</td>
  </tr>
  <tr>
    <td>&from=02:00_20170701&until=14:00_20170701</td>
	<td>Per visualizzare i dati in un ciclo di 24 ore, ad esempio, dalle 02.00 alle 14.00 del 1° luglio 2017</td>
  </tr>
  <tr>
    <td>&from=-8d&until=-7d</td>
	<td>Per visualizzare lo stesso giorno della settimana, ad esempio, la settimana scorsa</td>
  </tr>
  
</table>


## Impostazione dello spazio
{: #domain}

Quando usi il modello di autenticazione UAA {{site.data.keyword.Bluemix_notm}} o il modello di autenticazione IAM {{site.data.keyword.Bluemix_notm}} per eseguire l'autenticazione presso il servizio {{site.data.keyword.monitoringshort}}, il campo di intestazione **X-Auth-Scope-Id** è obbligatorio e
deve essere impostato sul GUID dello spazio. Il GUID deve essere come prefisso *s-* per identificare uno spazio.



## Impostazione della chiave API o del token di autorizzazione
{: #security}
  
Devi impostare il campo **X-Auth-User-Token** per definire il token UAA, il token IAM o la chiave API di cui si fa uso per l'autenticazione. 

Una chiave API o un token devono avere come prefisso uno dei seguenti valori: `apikey`, `iam` o `uaa` 

dove 

* *apikey* identifica che il token è una chiave API.
* *iam* identifica che il token specificato è un token generato da IAM.
* *uaa* identifica che il token specificato è un token generato da UAA.

Ad esempio, puoi impostare la seguente intestazione per una chiave API: `X-Auth-User-Token: apikey SomeIAMGeneratedKey`


## Recupero di metriche da uno spazio utilizzando UAA
{: #uaa}

Per recuperare le metriche da uno spazio {{site.data.keyword.Bluemix_notm}}, completa la seguente procedura:

1. Accedi a una regione, un'organizzazione e uno spazio {{site.data.keyword.Bluemix_notm}}. Esegui il comando:

    Ad esempio, per accedere alla regione Stati Uniti Sud, esegui il seguente comando:
	
	```
    cf login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Segui le istruzioni. Immetti le tue credenziali {{site.data.keyword.Bluemix_notm}}, seleziona un'organizzazione e uno spazio.

2. Ottieni il token di autenticazione UAA.

    Per ulteriori informazioni sull'autenticazione UAA, vedi [Ottenimento del token UAA utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) o [Ottenimento del token UAA utilizzando l'API REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

	Ad esempio, per utilizzare il token UAA, esegui questo comando:

    ```
	cf oauth-token
	```
	{: codeblock}
	
	Il risultato di questo comando è il seguente:
	
	```
	bearer eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8
	```
	{: screen}
	
	Quindi, esporta la variabile *Token*. Ad esempio:
	
	```
	export Token="eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8"
	```
	{: screen}
	
	**Nota:** il token esclude *Bearer*.
		
3. Ottieni il GUID dello spazio. Il GUID deve essere preceduto da *s-* per identificare uno spazio.

    Immetti il seguente comando:
	
	```
	cf space SpaceName --guid
	```
	{: codeblock}
	
	Dove *SpaceName* è il nome dello spazio che ha come prefisso *s-* in cui intendi definire una notifica.
	
	Ad esempio, per ottenere il GUID di uno spazio con nome *dev*, immetti il seguente comando:
	
	```
	cf space dev --guid
	```
	{: screen}
	
	Il risultato di questo comando è il seguente:
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	Quindi, esporta la variabile *Space*. Ad esempio:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. Immetti il seguente comando cURL per inviare le metriche:

    ```
	curl -XGET --header "X-Auth-User-Token:uaa ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics?from=Start_Time&until=End_Time&target=string
	```
	{: codeblock}

	dove
	
	* *X-Auth-User-Token* è un parametro che passa il token UAA {{site.data.keyword.Bluemix_notm}}.
	
	* *uaa* è il prefisso che indica che il token specificato è un token generato da UAA.
	
	* *X-Auth-Scope-Id* è un parametro che passa il GUID dello spazio. Il GUID deve essere preceduto da *s-* per identificare uno spazio. Questa intestazione è obbligatoria solo se usi l'autenticazione UAA. Se utilizzi un token di autenticazione IAM, questa intestazione è facoltativa e vengono utilizzate le informazioni di dominio associate al token.
	
	* *Token* rappresenta il token UAA.
	
	* *Space* rappresenta il GUID dello spazio. È obbligatorio solo se utilizzi un token UAA.
	
	* *Start_Time* definisce il periodo di tempo relativo o assoluto che imposta l'inizio della richiesta. Il valore predefinito è 24h fa: `-24h`. *End_Time* definisce il periodo di tempo relativo o assoluto per impostare la fine della richiesta. Il valore predefinito è l'ora corrente now: `now`. Per ulteriori informazioni, vedi [Impostazione di un periodo di tempo](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#time). 	

	* *Path*  identifica una o diverse metriche. Puoi, facoltativamente, applicare delle funzioni a ciascuna metrica. I percorsi supportano anche i caratteri jolly, che ti consentono di identificare più di una metrica in un singolo percorso. Per ulteriori informazioni, vedi [Definizione delle metriche](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#metrics).




## Recupero delle metriche da uno spazio utilizzando IAM o una chiave API
{: #iam}

Per recuperare le metriche da uno spazio {{site.data.keyword.Bluemix_notm}} utilizzando IAM o una chiave API, completa la seguente procedura:

1. Accedi a una regione, un'organizzazione e uno spazio {{site.data.keyword.Bluemix_notm}}. Esegui il comando:

    Ad esempio, per accedere alla regione Stati Uniti Sud, esegui il seguente comando:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Segui le istruzioni. Immetti le tue credenziali {{site.data.keyword.Bluemix_notm}}, seleziona un'organizzazione e uno spazio.

2. Ottieni il token di autenticazione o la chiave API.

    Per ulteriori informazioni sull'autenticazione IAM, vedi [Ottenimento del token IAM utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_cli) o [Generazione di una chiave API IAM utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).

	Ad esempio, per utilizzare il token IAM, esegui questo comando:

    ```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	Il risultato di questo comando è il seguente:
	
	```
	IAM token:  Bearer djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ
    UAA token:  Bearer eyJhbGciOiJIUz..Ky6vagp3k_QcIcKJ-td83qXhO5Uze43KcplG6PzcGs8
	```
	{: screen}
	
	Quindi, esporta la variabile *Token*. Ad esempio:
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Nota:** il token esclude *Bearer*.
		
3. Ottieni il GUID dello spazio.

    Per ottenere il GUID dello spazio, vedi [Come ottengo il GUID di uno spazio](/docs/service/cloud-monitoring/qa/cli_qa.html#space_guid). Il GUID deve essere preceduto da *s-* per identificare uno spazio.
    
    Ad esempio, immetti il seguente comando:
	
	```
	bx iam space NAME --guid
	```
	{: codeblock}
	
	Dove *SpaceName* è il nome dello spazio che ha come prefisso *s-* in cui intendi definire una notifica.
	
	Ad esempio, per ottenere il GUID di uno spazio con nome *dev*, immetti il seguente comando:
	
	```
	bx iam space dev --guid
	```
	{: screen}
	
	Il risultato di questo comando è il seguente:
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	Quindi, esporta la variabile *Space*. Ad esempio:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. Esegui un comando cURL per recuperare le metriche.

   	```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics?from=Start_Time&until=End_Time&target=string
	```
	{: codeblock}

	dove
	
	* *X-Auth-User-Token* è un parametro che passa la chiave API o il token AIM {{site.data.keyword.Bluemix_notm}}.
	
	* *Auth_Type* è il prefisso che definisce il tipo di token o chiave API. 

        * *apikey* identifica che il token è una chiave API.
		* *iam* identifica che il token specificato è un token generato da IAM.
	
	* *X-Auth-Scope-Id* è un parametro che passa il GUID dello spazio. Il GUID deve essere preceduto da *s-* per identificare uno spazio. Questa intestazione è obbligatoria solo se usi l'autenticazione UAA. Se utilizzi un token di autenticazione IAM, questa intestazione è facoltativa e vengono utilizzate le informazioni di dominio associate al token.
	
	* *Token* rappresenta il token UAA.
	
	* *Space* rappresenta il GUID dello spazio. È obbligatorio solo se utilizzi un token UAA.
	
	* *Start_Time* definisce il periodo di tempo relativo o assoluto che imposta l'inizio della richiesta. Il valore predefinito è 24h fa: `-24h`. *End_Time* definisce il periodo di tempo relativo o assoluto per impostare la fine della richiesta. Il valore predefinito è l'ora corrente now: `now`. Per ulteriori informazioni, vedi [Impostazione di un periodo di tempo](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#time). 	
	* *Path*  identifica una o diverse metriche. Puoi, facoltativamente, applicare delle funzioni a ciascuna metrica. I percorsi supportano anche i caratteri jolly, che ti consentono di identificare più di una metrica in un singolo percorso. Per ulteriori informazioni, vedi [Definizione delle metriche](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#metrics).






