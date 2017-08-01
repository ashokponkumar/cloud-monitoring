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



# Recupero della cronologia di una regola
{: #retrieve_history}


Per recuperare la cronologia di un avviso, completa la seguente procedura:

1. Accedi a una regione, un'organizzazione e uno spazio {{site.data.keyword.Bluemix_notm}}. Esegui il comando:

    Ad esempio, per accedere alla regione Stati Uniti Sud, esegui il seguente comando:
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    Segui le istruzioni. Immetti le tue credenziali {{site.data.keyword.Bluemix_notm}}, seleziona un'organizzazione e uno spazio.

2. Ottieni il token di autenticazione o la chiave API.

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
	
4. Immetti il seguente comando cURL per ottenere la cronologia di un avviso:

    ```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/history?rule=Rule_Name
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
	
	* Rule_Name è il nome della regola utilizzata per attivare l'avviso. Il valore è quello specificato nel campo *name*.
	
    
Ad esempio, la cronologia della regola `highNginxCPU` è la seguente:

```
[
 {
 "rule": "highNginxCPU",
 "timestamp": "2017-04-17T22.23.21.000",
 "value": 99.5,
 "from_level": "OK",
 "to_level": "ERROR"
 },
 {
 "rule": "highNginxCPU",
 "timestamp": "2017-04-17T10.11.12.123",
 "value": 98.5,
 "from_level" : "OK",
 "to_level": "WARN"
 },
 {
 "rule": "highNginxCPU",
 "timestamp": "2017-04-17T10.12.14.000",
 "value": 97.0,
 "from_level" : "WARN",
 "to_level": "OK"
 }
]
```
{: screen}


