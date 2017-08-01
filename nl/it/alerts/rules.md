---

copyright:
  years: 2017

lastupdated: "2017-07-18"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Gestione delle regole utilizzando l'API Avvisi
{: #rules}

Utilizza l'API Avvisi per creare, eliminare e aggiornare una regola, per visualizzare i dettagli di una regola e per elencare le regole definite nel tuo spazio {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}


## Creazione di una regola
{: #create}

Per creare una regola, completa la seguente procedura:

1. Crea un file di regole che contiene un JSON valido. Salva il file. 

    Ad esempio, per salvare il file, utilizza un prefisso come *s-* per le regole che definisci per le query in esecuzione in uno spazio {{site.data.keyword.Bluemix_notm}}.
	
	**Suggerimenti:** 
	
	* Definisci diverse regole per una query per la segnalazione di errori o avvertenze utilizzando un metodo di notifica differente. Puoi impostare 1 solo metodo di notifica per ogni regola. 
	* Crea il tuo file di regole nella seguente directory: *~/cloud-monitoring/rules/* per raggruppare le risorse del servizio {{site.data.keyword.monitoringshort_notm}}. 

    Nel file delle regole, immetti le seguenti informazioni:
	
	* *name*: immetti un nome univoco. Il valore di questo campo viene utilizzato in seguito per aggiornare, eliminare e mostrare i dettagli della regola.
	* *description*: immetti una descrizione.
	* *expression*: immetti la query che desideri monitorare.
	* *enabled*: imposta su *true* per abilitare la regola.
	* *from*: punto temporale iniziale utilizzato per analizzare i dati in base ai valori di soglia.
	* *until*: punto temporale finale utilizzato per analizzare i dati in base ai valori di soglia.
	* *comparison*: operazione di confronto utilizzata per identificare il tipo di controllo da effettuare, ad esempio *below*.
	* *error_level*: definisce la soglia che imposti per attivare un avviso di errore.
	* *warning_level*: definisce la soglia che imposti per attivare un avviso di avvertenza.
	* *frequency*: definisce la frequenza di controllo dei dati.
	* *dashboard_url*: definisce un URL che mostra in Grafana un dashboard con la query.
	* *notifications*: il metodo di notifica nel caso in cui venga attivato l'avviso descritto con questa regola.
	
	Ad esempio, 
	
	```
	{
    "name": "checkbytesin",
    "description": "MH check Bytes In per second",
    "expression": "movingAverage(messagehub.65qser11-8034-1234-5678-c82fb42wdfgh.1.kafka-java-console-sample-topic.BytesInPerSec.15MinuteRate,\"5min\")",
    "enabled": true,
    "from": "-5min",
    "until": "now",
    "comparison": "above",
    "comparison_scope": "last",
    "error_level" : 27,
    "warning_level" : 25,
    "frequency": "1min",
    "dashboard_url": "https://metrics.ng.bluemix.net",
    "notifications": [
      "emailXXX"
    ]
    }
    ```
	{: screen}
	
2. Accedi a una regione, un'organizzazione e uno spazio {{site.data.keyword.Bluemix_notm}}. Esegui il comando:

    Ad esempio, per accedere alla regione Stati Uniti Sud, esegui il seguente comando:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Segui le istruzioni. Immetti le tue credenziali {{site.data.keyword.Bluemix_notm}}, seleziona un'organizzazione e uno spazio.

3. Ottieni il token di autenticazione o la chiave API.

    * Per l'autenticazione IAM, consulta [Ottenimento del token IAM utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) oppure [Generazione di una chiave API IAM utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Per l'autenticazione UAA, consulta [Ottenimento del token UAA utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) oppure [Ottenimento del token UAA utilizzando l'API REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

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
	
	Quindi, esporta la variabile *Token*:
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Nota:** il token esclude *Bearer*.
	
4. Ottieni il GUID dello spazio. Il GUID deve essere preceduto da *s-* per identificare uno spazio.

    Immetti il seguente comando:
	
	```
	bx iam space SpaceName --guid
	```
	{: codeblock}
	
	Dove *SpaceName* è il nome dello spazio in cui intendi definire una notifica.
	
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
	
	Quindi, esporta la variabile *Space*:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. Immetti il seguente comando cURL per creare una regola:

    ```
	curl -XPOST -d @Rule_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: codeblock}
	
	dove
	
	* Rule_File è il file JSON che definisce la tua regola di avviso.
	
	* *X-Auth-User-Token* è un parametro che passa il token UAA, il token IAM o la chiave API {{site.data.keyword.Bluemix_notm}}.
	
	* *Auth_Type* è il prefisso che definisce il tipo di token o chiave API. Il seguente elenco descrive i valori validi: *apikey*, *iam* o *uaa*, dove

        * *apikey* identifica che il token è una chiave API.
		* *iam* identifica che il token specificato è un token generato da IAM.
		* *uaa* identifica che il token specificato è un token generato da UAA.
	
	* *X-Auth-Scope-Id* è un parametro che passa il GUID dello spazio. Il GUID deve essere preceduto da *s-* per identificare uno spazio. Questa intestazione è obbligatoria se utilizzi un'autenticazione token UAA. Se utilizzi un token di autenticazione IAM, questa intestazione è facoltativa e vengono utilizzate le informazioni di dominio associate al token.
	
	* Token è il token di autenticazione UAA o IAM o la chiave API.
	
	* Space è il GUID dello spazio. È obbligatorio solo se utilizzi un token UAA.
	
    Ad esempio, 	
	
	```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}

## Eliminazione di una regola
{: #delete}

Per eliminare una regola, completa la seguente procedura:

1. Accedi a una regione, un'organizzazione e uno spazio {{site.data.keyword.Bluemix_notm}}. Esegui il comando:

    Ad esempio, per accedere alla regione Stati Uniti Sud, esegui il seguente comando:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Segui le istruzioni. Immetti le tue credenziali {{site.data.keyword.Bluemix_notm}}, seleziona un'organizzazione e uno spazio.

2. Ottieni il token di autenticazione o la chiave API.

    * Per l'autenticazione IAM, consulta [Ottenimento del token IAM utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) oppure [Generazione di una chiave API IAM utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Per l'autenticazione UAA, vedi [Ottenimento del token UAA utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) oppure [Ottenimento del token UAA utilizzando l'API REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

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
	
	Quindi, esporta la variabile *Token*:
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Nota:** il token esclude *Bearer*.
	
3. Ottieni il GUID dello spazio. Il GUID deve essere preceduto da *s-* per identificare uno spazio.

    Immetti il seguente comando:
	
	```
	bx iam space SpaceName --guid
	```
	{: codeblock}
	
	Dove *SpaceName* è il nome dello spazio in cui intendi definire una notifica.
	
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
	
	Quindi, esporta la variabile *Space*:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. Immetti il seguente comando cURL per eliminare una regola:

    ```
	curl -XDELETE --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/rule/Rule_Name
	```
	{: codeblock}
	
	dove
	
    * *X-Auth-User-Token* è un parametro che passa il token UAA, il token IAM o la chiave API {{site.data.keyword.Bluemix_notm}}.
	
	* *Auth_Type* è il prefisso che definisce il tipo di token o chiave API. Il seguente elenco descrive i valori validi: *apikey*, *iam* o *uaa*, dove

        * *apikey* identifica che il token è una chiave API.
		* *iam* identifica che il token specificato è un token generato da IAM.
		* *uaa* identifica che il token specificato è un token generato da UAA.
	
	* *X-Auth-Scope-Id* è un parametro che passa il GUID dello spazio. Il GUID deve essere preceduto da *s-* per identificare uno spazio. Questa intestazione è obbligatoria se utilizzi un'autenticazione token UAA. Se utilizzi un token di autenticazione IAM, questa intestazione è facoltativa e vengono utilizzate le informazioni di dominio associate al token.
	
	* Token è il token di autenticazione UAA o IAM o la chiave API.
	
	* Space è il GUID dello spazio. È obbligatorio solo se utilizzi un token UAA.
	
	* Rule_Name è il nome della regola come specificato nel campo *name*.
	
    
	
## Elenco di tutte le regole
{: #list}

Per elencare tutte le regole, completa la seguente procedura:

1. Accedi a una regione, un'organizzazione e uno spazio {{site.data.keyword.Bluemix_notm}}. Esegui il comando:

    Ad esempio, per accedere alla regione Stati Uniti Sud, esegui il seguente comando:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Segui le istruzioni. Immetti le tue credenziali {{site.data.keyword.Bluemix_notm}}, seleziona un'organizzazione e uno spazio.

2. Ottieni il token di autenticazione o la chiave API.

    * Per l'autenticazione IAM, consulta [Ottenimento del token IAM utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) oppure [Generazione di una chiave API IAM utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Per l'autenticazione UAA, vedi [Ottenimento del token UAA utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) oppure [Ottenimento del token UAA utilizzando l'API REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

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
	
	Quindi, esporta la variabile *Token*:
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Nota:** il token esclude *Bearer*.
	
3. Ottieni il GUID dello spazio. Il GUID deve essere preceduto da *s-* per identificare uno spazio.

    Immetti il seguente comando:
	
	```
	bx iam space SpaceName --guid
	```
	{: codeblock}
	
	Dove *SpaceName* è il nome dello spazio in cui intendi definire una notifica.
	
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
	
	Quindi, esporta la variabile *Space*:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. Immetti il seguente comando cURL per elencare tutte le regole in uno spazio:

    ```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/rules
	```
	{: codeblock}
	
	dove
	
	* *X-Auth-User-Token* è un parametro che passa il token UAA, il token IAM o la chiave API {{site.data.keyword.Bluemix_notm}}.
	
	* *Auth_Type* è il prefisso che definisce il tipo di token o chiave API. Il seguente elenco descrive i valori validi: *apikey*, *iam* o *uaa*, dove

        * *apikey* identifica che il token è una chiave API.
		* *iam* identifica che il token specificato è un token generato da IAM.
		* *uaa* identifica che il token specificato è un token generato da UAA.
	
	* *X-Auth-Scope-Id* è un parametro che passa il GUID dello spazio. Il GUID deve essere preceduto da *s-* per identificare uno spazio. Questa intestazione è obbligatoria se utilizzi un'autenticazione token UAA. Se utilizzi un token di autenticazione IAM, questa intestazione è facoltativa e vengono utilizzate le informazioni di dominio associate al token.
	
	* Token è il token di autenticazione UAA o IAM o la chiave API.
	
	* Space è il GUID dello spazio. È obbligatorio solo se utilizzi un token UAA.
	

	
	

## Visualizzazione dei dettagli di una regola
{: show}

Per visualizzare i dettagli di una regola, completa la seguente procedura:

1. Accedi a una regione, un'organizzazione e uno spazio {{site.data.keyword.Bluemix_notm}}. Esegui il comando:

    Ad esempio, per accedere alla regione Stati Uniti Sud, esegui il seguente comando:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Segui le istruzioni. Immetti le tue credenziali {{site.data.keyword.Bluemix_notm}}, seleziona un'organizzazione e uno spazio.

2. Ottieni il token di autenticazione o la chiave API.

    * Per l'autenticazione IAM, consulta [Ottenimento del token IAM utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) oppure [Generazione di una chiave API IAM utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Per l'autenticazione UAA, vedi [Ottenimento del token UAA utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) oppure [Ottenimento del token UAA utilizzando l'API REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

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
	
	Quindi, esporta la variabile *Token*:
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Nota:** il token esclude *Bearer*.
	
3. Ottieni il GUID dello spazio. Il GUID deve essere preceduto da *s-* per identificare uno spazio.

    Immetti il seguente comando:
	
	```
	bx iam space SpaceName --guid
	```
	{: codeblock}
	
	Dove *SpaceName* è il nome dello spazio in cui intendi definire una notifica.
	
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
	
	Quindi, esporta la variabile *Space*:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. Immetti il seguente comando cURL per visualizzare i dettagli di una regola:

    ```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/rule/Rule_Name
	```
	{: codeblock}
	
	dove
	
	* *X-Auth-User-Token* è un parametro che passa il token UAA, il token IAM o la chiave API {{site.data.keyword.Bluemix_notm}}.
	
	* *Auth_Type* è il prefisso che definisce il tipo di token o chiave API. Il seguente elenco descrive i valori validi: *apikey*, *iam* o *uaa*, dove

        * *apikey* identifica che il token è una chiave API.
		* *iam* identifica che il token specificato è un token generato da IAM.
		* *uaa* identifica che il token specificato è un token generato da UAA.
	
	* *X-Auth-Scope-Id* è un parametro che passa il GUID dello spazio. Il GUID deve essere preceduto da *s-* per identificare uno spazio. Questa intestazione è obbligatoria se utilizzi un'autenticazione token UAA. Se utilizzi un token di autenticazione IAM, questa intestazione è facoltativa e vengono utilizzate le informazioni di dominio associate al token.
	
	* Token è il token di autenticazione UAA o IAM o la chiave API.
	
	* Space è il GUID dello spazio. È obbligatorio solo se utilizzi un token UAA.
	
	* Rule_Name è il nome della regola come specificato nel campo *name*.
	

## Aggiornamento di una regola
{: #update}

Per aggiornare una regola, completa la seguente procedura:

1. Accedi a una regione, un'organizzazione e uno spazio {{site.data.keyword.Bluemix_notm}}. Esegui il comando:

    Ad esempio, per accedere alla regione Stati Uniti Sud, esegui il seguente comando:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Segui le istruzioni. Immetti le tue credenziali {{site.data.keyword.Bluemix_notm}}, seleziona un'organizzazione e uno spazio.

2. Ottieni il token di autenticazione o la chiave API.

    * Per l'autenticazione IAM, consulta [Ottenimento del token IAM utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) oppure [Generazione di una chiave API IAM utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli).
	
	* Per l'autenticazione UAA, vedi [Ottenimento del token UAA utilizzando la CLI Bluemix](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) oppure [Ottenimento del token UAA utilizzando l'API REST](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api).

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
	
	Quindi, esporta la variabile *Token*:
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**Nota:** il token esclude *Bearer*.
	
3. Ottieni il GUID dello spazio. Il GUID deve essere preceduto da *s-* per identificare uno spazio.

    Immetti il seguente comando:
	
	```
	bx iam space SpaceName --guid
	```
	{: codeblock}
	
	Dove *SpaceName* è il nome dello spazio in cui intendi definire una notifica.
	
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
	
	Quindi, esporta la variabile *Space*:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. Immetti il seguente comando cURL per aggiornare una regola:

    ```
	curl -XPUT `-d @Rule_File` --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: codeblock}
	
	dove
	
	* Rule_File è il file JSON che definisce la tua regola di avviso.
	
	* *X-Auth-User-Token* è un parametro che passa il token UAA, il token IAM o la chiave API {{site.data.keyword.Bluemix_notm}}.
	
	* *Auth_Type* è il prefisso che definisce il tipo di token o chiave API. Il seguente elenco descrive i valori validi: *apikey*, *iam* o *uaa*, dove

        * *apikey* identifica che il token è una chiave API.
		* *iam* identifica che il token specificato è un token generato da IAM.
		* *uaa* identifica che il token specificato è un token generato da UAA.
	
	* *X-Auth-Scope-Id* è un parametro che passa il GUID dello spazio. Il GUID deve essere preceduto da *s-* per identificare uno spazio. Questa intestazione è obbligatoria se utilizzi un'autenticazione token UAA. Se utilizzi un token di autenticazione IAM, questa intestazione è facoltativa e vengono utilizzate le informazioni di dominio associate al token.
	
	* Token è il token di autenticazione UAA o IAM o la chiave API.
	
	* Space è il GUID dello spazio. È obbligatorio solo se utilizzi un token UAA.

	
	
