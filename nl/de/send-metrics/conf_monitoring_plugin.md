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

# Daten mithilfe des Überwachungs-Plug-ins senden (collectd)
{: #conf_monitoring_plugin}

Sie können 'collectd' so konfigurieren, dass Metriken in einer Umgebung erfasst werden. Verwenden Sie das {{site.data.keyword.monitoringshort}}-Plug-in, um diese Metriken aus Ihrer Umgebung an eine Bereichsdomäne zu senden. 
{: shortdesc}

Die folgende Abbildung zeigt eine Übersicht über die Verwendung des {{site.data.keyword.monitoringshort}}-Plug-ins zum Senden von Metriken an den {{site.data.keyword.monitoringshort}}-Service:

![Übersicht über die Verwendung des {{site.data.keyword.monitoringshort}}-Plug-ins](images/monitoring_plugin_ov.png "Übersicht über die Verwendung des {{site.data.keyword.monitoringshort}}-Plug-ins")



## Überwachungs-Plug-in installieren
{: #install}

Führen Sie von einem Terminal aus die folgenden Schritte durch: 

1. (Pre-req) Installieren Sie die {{site.data.keyword.Bluemix_notm}}-CLI.

   Weitere Informationen finden Sie unter [{{site.data.keyword.Bluemix_notm}}-CLI installieren](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa).
   
   Wenn die CLI installiert ist, fahren Sie mit dem nächsten Schritt fort.
	
2. Melden Sie sich bei einer Region, einer Organisation und einem Bereich in {{site.data.keyword.Bluemix_notm}} an. 

    Weitere Informationen finden Sie unter [Wie melde ich mich bei {{site.data.keyword.Bluemix_notm}} an?](/docs/services/cloud-monitoring/qa/cli_qa.html#login).

    Führen Sie zum Beispiel den folgenden Befehl aus, um sich an der Region 'USA (Süden)' anzumelden:
	
	```
    bx login -a https://api.ng.bluemix.net
	bx target -o MyOrg -s MySpace
    ```
    {: codeblock}

    Befolgen Sie die Anweisungen. Geben Sie Ihre {{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise ein, wählen Sie eine Organisation und einen Bereich aus.

### Schritt 1: 'collectd' installieren
{: #collectd}

Führen Sie die folgenden Schritte als Administrator aus, um 'collectd' zu installieren:

1.	Melden Sie sich als Rootbenutzer an. 

    ```
    sudo -s
    ```
    {: codeblock}

2.	Installieren Sie das NTP-Paket (NTP, Network Time Protocol), um die Zeit der Protokolle zu synchronisieren. 

    Für ein Ubuntu-System prüfen Sie beispielsweise, ob `timedatectl status` Folgendes anzeigt: *Network time on: yes*. Ist dies der Fall, ist Ihr Ubuntu-System bereits für die Verwendung von NTP konfiguriert und Sie können diesen Schritt überspringen.
    
    ```
    # timedatectl status
    Local time: Mon 2017-06-12 03:01:22 PDT
    Universal time: Mon 2017-06-12 10:01:22 UTC
    RTC time: Mon 2017-06-12 10:01:22
    Time zone: America/Los_Angeles (PDT, -0700)
    Network time on: yes
    NTP synchronized: yes
    RTC in local TZ: no
    ```
    {: screen}
    
    Führen Sie die folgenden Schritte aus, um NTP auf einem Ubuntu-System zu installieren:

    1.	Führen Sie den folgenden Befehl aus, um die Pakete zu aktualisieren. 

        ```
        apt-get update
        ```
        {: codeblock}
        
    2.	Führen Sie den folgenden Befehl aus, um das Paket 'ntp' zu installieren. 

        ```
        apt-get install ntp
        ```
        {: codeblock}
        
    3.	Führen Sie den folgenden Befehl aus, um das Paket 'ntpdate' zu installieren. 
    
        ```
        apt-get install ntpdate
        ```
        {: codeblock}
        
    4.	Führen Sie den folgenden Befehl aus, um den Service zu stoppen. 
        
        ```
        service ntp stop
        ```
        {: codeblock}
        
    5.	Führen Sie den folgenden Befehl aus, um die Systemuhr zu synchronisieren. 
    
        ```
        ntpdate -u 0.ubuntu.pool.ntp.org
        ```
        {: codeblock}
        
        Das Ergebnis bestätigt, dass die Zeit angepasst wird, zum Beispiel:
        
        ```
        4 May 19:02:17 ntpdate[5732]: adjust time server 50.116.55.65 offset 0.000685 sec
        ```
        {: screen}
        
    6.	Führen Sie den folgenden Befehl aus, um 'ntpd' erneut zu starten. 
    
        ```
        service ntp start
        ```
        {: codeblock}
    
        Das Ergebnis bestätigt, dass der Service gestartet wird.

3. Installieren Sie 'collectd'. Führen Sie folgenden Befehl aus:

    ```
	apt-get install collectd 
	```
	{: codeblock}


### Schritt 2: Installieren Sie das Überwachungs-Plug-in
{: #plugin}

Führen Sie die folgenden Schritte aus, um das {{site.data.keyword.monitoringshort}}-Plug-in zu installieren:

1. 	Melden Sie sich als Rootbenutzer an. 

    ```
    sudo -s
    ```
    {: codeblock}
	
2. Fügen Sie das {{site.data.keyword.monitoringshort}}-Service-Repository hinzu. Führen Sie den folgenden Befehl aus:

    ```
    wget -O - https://downloads.opvis.bluemix.net/client/IBM_Logmet_repo_install.sh | bash
    ```
   {: codeblock}
   
3. Installieren Sie das Plug-in. Führen Sie den folgenden Befehl aus:

    ```
	apt-get install ibmcloud-monitoring
	```
	{: codeblock}


## Überwachungs-Plug-in konfigurieren
{: #configure}

Zum Konfigurieren des {{site.data.keyword.monitoringshort}}-Plug-ins führen Sie die folgenden Schritte aus:
	
### Schritt 1: Weisen Sie dem Benutzer eine IAM-Richtlinie zu.
{: #iam_policy}

Um Metriken in eine Domäne senden zu können, muss Ihrer Benutzer-ID eine IAM-Rolle erteilt worden sein, die die Berechtigungen besitzt, Metriken an den Überwachungsservice zu senden.
 
* Die Berechtigungen werden festgelegt, indem diesem Benutzer eine IAM-Richtlinie in der IBM Cloud zugewiesen wird. 
* Folgende IAM-Rollen ermöglichen dem Benutzer das Senden von Metriken: *Administrator*, *Editor*, *Operator*. 

Zum Zuweisen einer IAM-Richtlinie zu einem Benutzer wählen Sie eine der folgenden Methoden:

* Über die {{site.data.keyword.Bluemix_notm}}-Benutzerschnittstelle: Weitere Informationen finden Sie unter [Benutzern eine IAM-Richtlinie über die {{site.data.keyword.Bluemix_notm}}-Benutzerschnittstelle zuweisen](/docs/services/cloud-monitoring/security/assign_policy.html#assign_policy_ui).
* Über die Befehlszeile: Weitere Informationen finden Sie unter [Benutzern eine IAM-Richtlinie über die Befehlszeile zuweisen](/docs/services/cloud-monitoring/security/assign_policy.html#assign_policy_commandline).
 
### Schritt 2: Rufen Sie den API-Schlüssel ab.
{: #api_key}
 
Zum Senden von Metriken an einen Bereich müssen Sie einen API-Schlüssel abrufen, um sich beim {{site.data.keyword.monitoringshort}}-Service authentifizieren zu können.

Weitere Informationen zum Abrufen eines API-Schlüssels finden Sie unter [API-Schlüssel abrufen](/docs/services/cloud-monitoring/security/auth_api_key.html#auth_api_key).
	
Legen Sie auf demselben Terminal, von dem aus Sie sich bei {{site.data.keyword.Bluemix_notm}} angemeldet haben, die Variable APIKEY für das Token fest.

Beispiel:

```
export APIKEY="kjshdgf.....ldkdjdj"
```
{: screen}
	
### Schritt 3: Rufen Sie Informationen zum Endpunkt ab.
{: #endpoint}

Informationen zum Festlegen des {{site.data.keyword.monitoringshort}}-Endpunkts, an den Sie die Metriken senden wollen, finden Sie in der Liste der Endpunkte nach Region - siehe [Endpunkte](/docs/services/cloud-monitoring/send_retrieve_metrics_ov.html#endpoints). Ermitteln Sie dort den Endpunkt für die Region, an die Sie Metriken senden wollen.

Legen Sie auf demselben Terminal, von dem aus Sie sich bei {{site.data.keyword.Bluemix_notm}} angemeldet haben, die Variable **METRIC_ENDPOINT** fest. Beispiel:

```
export METRIC_ENDPOINT="metrics.ng.bluemix.net"
```
{: screen}



### Schritt 4: Rufen Sie Informationen zur Bereichsdomänen-ID ab.
{: #domain}

Informationen zum Abrufen der Domänen-ID für einen Bereich finden Sie unter [Bereichs-GUID abrufen](/docs/services/cloud-monitoring/qa/cli_qa.html#space_guid). Legen Sie anschließend die Domänen-ID wie folgt fest: 's-SpaceID'. Hierbei ist 'SpaceID' die GUID des Bereichs.

Legen Sie auf demselben Terminal, von dem aus Sie sich bei {{site.data.keyword.Bluemix_notm}} angemeldet haben, die Variable 'SpaceID' fest:

```
export SpaceID="kjshdgf.....ldkdjdj"
```
{: screen}



### Schritt 5: Führen Sie das Konfigurationsscript aus.
{: #script}

Führen Sie das folgende Script aus, um das {{site.data.keyword.monitoringshort}}-Plug-in für das Senden von Metriken an einen Bereich zu konfigurieren:

```
/opt/ibmcloud_monitoring/configure -e $METRIC_ENDPOINT -a $APIKEY -s s-$SpaceID
```
{: codeblock}


Das Script fügt automatisch die Zeilengruppe **Include** in die Hauptdatei 'collectd.conf' ein:

```
<LoadPlugin IBMCloudMonitoring>
   FlushInterval 60
</LoadPlugin>
<Plugin IBMCloudMonitoring>
  <Endpoint "ng">
     Host "metrics.ng.bluemix.net"
     Port 9095
     ApiKey "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
     SkipInternalPrefixForStatsd false
     RateCounter false
     ScopeId "s-cedc73c5-6d55-4193-a9de-378620d6fab5"
  </Endpoint>
</Plugin>
```
{: screen}


### Schritt 6: Starten Sie den 'collectd'-Dämon neu.
{: #restart}

Führen Sie die folgenden Schritte aus:

1. 	Melden Sie sich als Rootbenutzer an. 

    ```
    sudo -s
    ```
    {: codeblock}
	
2.  Starten Sie 'collectd' neu.

   Wenn der 'collectd'-Dämon als Service hinzugefügt wurde, führen Sie den folgenden Befehl aus:
	
	```
	service collectd restart
	```
	{: codeblock}

Die in 'collectd' aktivierten Metriken sind diejenigen, die für die Analyse in Grafana zur Verfügung stehen.


