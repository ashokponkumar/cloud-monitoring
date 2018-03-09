---

copyright:
  years: 2017, 2018

lastupdated: "2018-02-07"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Metriken in Grafana für eine App analysieren, die in einem Kubernetes-Cluster bereitgestellt wurde
{: #container_service_metrics}

In diesem Lernprogramm erfahren Sie, wie Sie den {{site.data.keyword.monitoringlong}}-Service zum Überwachen der Leistung Ihres Containers verwenden.
{:shortdesc}


## Lernziele
{: #objectives}

Sie erfahren, wie Sie nach Containermetriken für eine App suchen, die in einem Kubernetes-Cluster implementiert ist, und wie Sie diese Metriken analysieren:

1. Ermitteln Sie, wo Metriken, die in einem Cluster erfasst werden, an den {{site.data.keyword.monitoringshort}}-Service weitergeleitet werden. 
2. Starten Sie Grafana und legen Sie die {{site.data.keyword.monitoringshort}-Domäne fest, in der Sie die Clustermetriken anzeigen können.
3. Suchen und analysieren Sie Containermetriken für eine App, die in einem Kubernetes-Cluster in {{site.data.keyword.Bluemix_notm}} bereitgestellt wird.

Dieses Lernprogramm führt Sie durch die erforderlichen Schritte zum Erzielen des folgenden End-to-End-Szenarios bei der Arbeit in {{site.data.keyword.Bluemix_notm}}: Bereitstellen eines Clusters, ermitteln, wo der Cluster Metriken an den {{site.data.keyword.monitoringshort}}-Service in {{site.data.keyword.Bluemix_notm}} sendet, Bereitstellen einer App im Cluster und Verwenden von Grafana zum Anzeigen und Filtern von Containermetriken für diesen Cluster.


**Hinweis:** Zum Absolvieren dieses Lernprogramms müssen Sie die Voraussetzungen erfüllen und die Lernprogramme durchführen, die aus verschiedenen Schritten verlinkt sind.


## Voraussetzungen
{: #prereqs}

1. Sie müssen ein Mitglied oder ein Eigner eines {{site.data.keyword.Bluemix_notm}}-Kontos mit der Berechtigung zum Erstellen von Kubernetes-Standardclustern, Bereitstellen von Apps in Clustern und zum Abfragen der Metriken in {{site.data.keyword.Bluemix_notm}} für die Überwachung in Grafana sein.

    Ihrer Benutzer-ID in {{site.data.keyword.Bluemix_notm}} müssen die folgenden Richtlinien zugewiesen sein:
    
    * Eine IAM-Richtlinie für den {{site.data.keyword.containershort}} mit der Berechtigung *Operator* oder *Administrator*.
    * Eine CF-Rolle für den Bereich, in dem der {{site.data.keyword.monitoringshort}}-Service bereitgestellt wird, mit der Berechtigung *Entwickler*.
    
    Weitere Informationen finden Sie unter [Einem Benutzer eine IAM-Richtlinie über die IBM Cloud-Benutzerschnittstelle zuweisen](/docs/services/cloud-monitoring/security/grant_permissions.html#grant_permissions_ui_account) und [Einem Benutzer eine CF-Rolle über die IBM Cloud-Benutzerschnittstelle zuweisen](/docs/services/cloud-monitoring/security/grant_permissions.html#grant_permissions_ui_space).

2. Öffnen Sie eine Terminalsitzung, von der aus Sie die Kubernetes-Cluster verwalten und die Apps über die Befehlszeile bereitstellen können. Die Beispiele in diesem Lernprogramm gelten für ein Ubuntu Linux-System.

3. Installieren Sie die CLIs für die Arbeit mit dem {{site.data.keyword.containershort}} in Ihrem Ubuntu-System.

    * Installieren Sie die {{site.data.keyword.Bluemix_notm}}-CLI. Weitere Informationen finden Sie unter [{{site.data.keyword.Bluemix_notm}}-CLI installieren](/docs/cli/reference/bluemix_cli/download_cli.html#download_install).
    
    * Installieren Sie die {{site.data.keyword.containershort}}-CLI zum Erstellen und Verwalten Ihrer Kubernetes-Cluster in {{site.data.keyword.containershort}} und zum Bereitstellen containerisierten Apps im Cluster. Weitere Informationen finden Sie unter [CS-Plug-in installieren](/docs/containers/cs_cli_install.html#cs_cli_install_steps).
    

    
 

## Schritt 1: Kubernetes-Cluster bereitstellen
{: #step1}

Führen Sie die folgenden Schritte aus:

1. Erstellen Sie einen Kubernetes-Standardcluster.

   * [Kubernetes-Standardcluster über die Benutzerschnittstelle erstellen](/docs/containers/cs_cluster.html#cs_cluster_ui).
   * [Kubernetes-Standardcluster über die CLI erstellen](/docs/containers/cs_cluster.html#cs_cluster_cli).

2. Richten Sie den Clusterkontext in einem Terminal ein. Nach der Konfiguration des Kontexts können Sie den Kubernetes-Cluster verwalten und die Anwendung im Kubernetes-Cluster bereitstellen.

    Melden Sie sich bei der Region, der Organisation und dem Bereich in {{site.data.keyword.Bluemix_notm}} an, die/der dem von Ihnen erstellten Cluster zugeordnet ist. Weitere Informationen finden Sie unter [Wie melde ich mich bei {{site.data.keyword.Bluemix_notm}} an?](/docs/services/CloudLogAnalysis/qa/cli_qa.html#login).

	Initialisieren Sie das {{site.data.keyword.containershort}}-Service-Plug-in.

	```
	bx cs init
	```
	{: codeblock}

    Legen Sie den Terminalkontext für den Cluster fest.
    
	```
	bx cs cluster-config MyCluster
	```
	{: codeblock}

    Die Ausgabe dieses Befehls stellt den Befehl bereit, den Sie in Ihrem Terminal ausführen müssen, um den Pfad zur Konfigurationsdatei anzugeben. Beispiel:

	```
	export KUBECONFIG=/Users/ibm/.bluemix/plugins/container-service/clusters/MyCluster/kube-config-hou02-MyCluster.yml
	```
	{: codeblock}

    Kopieren Sie den Befehl und fügen Sie ihn ein, um die Umgebungsvariable in Ihrem Terminal festzulegen, und drücken Sie dann die **Eingabetaste**.



## Schritt 2: Ermitteln, wo der Cluster Metriken an den {{site.data.keyword.monitoringshort}}-Service weiterleitet
{: #step2}

Ein Cluster ist eine Ressource auf Kontoebene. Wenn Sie einen Cluster für den {{site.data.keyword.containershort}} bereitstellen, können Cluster auf Kontoebene oder mit einem zugehörigen CF-Bereich (CF, Cloud Foundry) erstellt werden. Sobald der Cluster bereitgestellt wurde und bereit ist, werden Metriken automatisch erfasst und an den {{site.data.keyword.monitoringshort}}-Service weitergeleitet.

* Cluster, denen ein CF-Bereich zugeordnet ist, leiten Metriken an die Bereichsmetrikdomäne weiter.
* Cluster, die auf Kontoebene erstellt werden, leiten Metriken an die Kontometrikdomäne weiter.

Um herauszufinden, wohin der Cluster Metriken weiterleitet, führen Sie den folgenden Befehl aus:

```
$ bx cs cluster-get ClusterName --json
```
{: codeblock}

Dabei ist *ClusterName* der Name des Clusters.

In der Ausgabe enthalten die folgenden Felder die Informationen darüber, wohin Metriken weitergeleitet werden:

* **logOrg** definiert die ID einer CF-Organisation.
* **logOrgName** definiert den Namen einer CF-Organisation.
* **logSpace** definiert die ID eines CF-Bereichs.
* **logSpaceName** definiert den Namen eines CF-Bereichs.

Wenn die Felder leer sind, werden Metriken an die Kontodomäne weitergeleitet.
Wenn in den Feldern eine CF-Organisation und ein CF-Bereich angegeben ist, werden Metriken an die Bereichsdomäne weitergeleitet, die diesem Bereich zugeordnet ist.

Die Ausgabe für einen Cluster, der Metriken an die Kontodomäne weiterleitet, sieht beispielsweise folgendermaßen aus:

```
$ bx cs cluster-get MyDemoCluster --json
{
    "id": "f9adabcjhefg745746hgfjbnkdnfsks",
    "name": "MyDemoCluster",
    "region": "eu-gb",
    "dataCenter": "lon02",
    "location": "eu-gb-lon02",
    "serverURL": "https://xxx.xxx.xxx.x:xxxxx",
    "state": "normal",
    "createdDate": "2018-01-30T17:41:14+0000",
    "modifiedDate": "2018-01-30T17:41:14+0000",
    "workerCount": 2,
    "isPaid": true,
    "masterKubeVersion": "1.8.6_1505",
    "targetVersion": "1.8.6_1505",
    "ingressHostname": "mydemocluster.uk-south.containers.mybluemix.net",
    "ingressSecretName": "mydemocluster",
    "ownerEmail": "xxxx@uibm.com",
    "logOrg": "",
    "logOrgName": "",
    "logSpace": "",
    "logSpaceName": "",
    "monitoringURL": "https://metrics.eu-gb.bluemix.net/app/#/grafana4/dashboard/db/a-siuhfieuhf7346586hfrhf_ClusterMonitoringDashboard_v1?scopeId=a-siuhfieuhf7346586hfrhf\u0026?var-Account_ID=a_siuhfieuhf7346586hfrhf\u0026var-Cluster=MyDemoCluster\u0026var-Namespace=default\u0026var-Pod_ID=All",
    "addons": [
        {
            "name": "customer-storage-pod",
            "enabled": true
        },
        {
            "name": "basic-ingress-v2",
            "enabled": true
        },
        {
            "name": "storage-watcher-pod",
            "enabled": true
        }
    ],
    "vlans": null
}
```
{: screen}





## Schritt 3: Dem Benutzer Berechtigungen zum Anzeigen von Metriken in der Metrikdomäne erteilen
{: #step3}

Um einem Benutzer Berechtigungen zum Anzeigen von Metriken in einer Bereichsdomäne zu erteilen, müssen Sie diesem Benutzer eine CF-Rolle zuweisen, die die Aktionen beschreibt, die der Benutzer mithilfe des {{site.data.keyword.monitoringshort}}-Service in dem Bereich durchführen kann.

Um einem Benutzer Berechtigungen zum Anzeigen von Metriken in einer Kontodomäne zu erteilen, müssen Sie diesem Benutzer eine IAM-Richtlinie zuweisen, die die Aktionen beschreibt, die der Benutzer mithilfe des {{site.data.keyword.monitoringshort}}-Service durchführen kann.

### Dem Benutzer Berechtigungen zum Anzeigen von Metriken in einer Bereichsdomäne erteilen
{: #space}

Führen Sie die folgenden Schritte aus, um einem Benutzer die Berechtigung zum Arbeiten mit dem {{site.data.keyword.monitoringshort}}-Service zu erteilen:

1. Melden Sie sich bei der {{site.data.keyword.Bluemix_notm}}-Konsole an.

    Öffnen Sie einen Web-Browser und starten Sie das {{site.data.keyword.Bluemix_notm}}-Dashboard: [http://bluemix.net ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](http://bluemix.net){:new_window}
	
	Nach der Anmeldung mit Ihrer Benutzer-ID und Ihrem Kennwort wird die {{site.data.keyword.Bluemix_notm}}-Benutzerschnittstelle geöffnet.

2. Klicken Sie in der Menüleiste auf **Verwalten > Konto > Benutzer**.

    Im Fenster *Benutzer* wird eine Liste der Benutzer mit ihren E-Mail-Adressen für das aktuell ausgewählte Konto angezeigt.
	
3. Wenn der Benutzer Mitglied des Kontos ist, wählen Sie den Benutzernamen in der Liste aus oder klicken Sie im Menü *Aktionen* auf **Benutzer verwalten**.

    Wenn der Benutzer kein Mitglied des Kontos ist, finden Sie weitere Informationen unter [Benutzer einladen](/docs/iam/iamuserinv.html#iamuserinv).

4. Wählen Sie **Cloud Foundry-Zugriff** und anschließend **Organisation zuweisen** aus.

5. Geben Sie die folgenden Werte ein:

    <table>
      <caption></caption>
      <tr>
        <th>Feld</th>
        <th>Wert</th>
      </tr>
      <tr>
        <td>Organisation</td>
        <td>MyOrg</td>
      </tr>       e
      <tr>
        <td>Organisationsrolle</td>
        <td>Keine Organisationsrolle</td>
      </tr>
      <tr>
        <td>Region</td>
        <td>USA (Süden)</td>
      </tr>
      <tr>
        <td>Bereich</td>
        <td>dev</td>
      </tr>
      <tr>
        <td>Bereichsrolle</td>
        <td>Prüfer</td>
      </tr>
	
6. Klicken Sie auf **Rolle speichern**.

### Dem Benutzer Berechtigungen zum Anzeigen von Metriken in der Kontodomäne erteilen
{: #acc}

Führen Sie die folgenden Schritte aus, um einem Benutzer die Berechtigung zum Arbeiten mit dem {{site.data.keyword.monitoringshort}}-Service zu erteilen:

1. Melden Sie sich bei der {{site.data.keyword.Bluemix_notm}}-Konsole an.

    Öffnen Sie einen Web-Browser und starten Sie das {{site.data.keyword.Bluemix_notm}}-Dashboard: [http://bluemix.net ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](http://bluemix.net){:new_window}
	
	Nach der Anmeldung mit Ihrer Benutzer-ID und Ihrem Kennwort wird die {{site.data.keyword.Bluemix_notm}}-Benutzerschnittstelle geöffnet.

2. Klicken Sie in der Menüleiste auf **Verwalten > Konto > Benutzer**.

    Im Fenster *Benutzer* wird eine Liste der Benutzer mit ihren E-Mail-Adressen für das aktuell ausgewählte Konto angezeigt.
	
3. Wenn der Benutzer Mitglied des Kontos ist, wählen Sie den Benutzernamen in der Liste aus oder klicken Sie im Menü *Aktionen* auf **Benutzer verwalten**.

    Wenn der Benutzer kein Mitglied des Kontos ist, finden Sie weitere Informationen unter [Benutzer einladen](/docs/iam/iamuserinv.html#iamuserinv).

4. Wählen Sie **Zugriffsrichtlinien > Zugriff zuweisen > Zugriff auf Ressourcen zuweisen**.

5. Wählen Sie den **{{site.data.keyword.monitoringlong}}**-Service, die Region, in der der Cluster verfügbar ist (**USA (Süden)** für dieses Lernprogramm), und eine Rolle (**Anzeigeberechtigter**) aus.



## Schritt 4: Berechtigungen für den Eigner des {{site.data.keyword.containershort_notm}} erteilen
{: #step4}

Wenn der Cluster Metriken an eine Bereichsdomäne weiterleitet, müssen Sie auch CF-Berechtigungen (CF, Cloud Foundry) für den Eigner des {{site.data.keyword.containershort}} in der Organisation und im Bereich erteilen. Dieser Eigner benötigt die Rolle *OrgManager* für diese Organisation und die Rolle *SpaceManger* und *Entwickler* für den Bereich.

Wenn der Cluster Metriken an die Kontodomäne weiterleitet, muss dem Eigner des {{site.data.keyword.containershort}} eine IAM-Richtlinie mit Administratorberechtigungen für den {{site.data.keyword.monitoringshort}}-Service zugewiesen sein.

### Berechtigungen zum Anzeigen in einer Bereichsdomäne erteilen
{: #space_1}

Erteilen Sie der Benutzer-ID des Eigners des {{site.data.keyword.containershort}} die folgenden Berechtigungen: die Rolle *OrgManager* für die Organisation und die Rolle *SpaceManager* und *Entwickler* für den Bereich. Führen Sie die folgenden Schritte aus:
    
1. Melden Sie sich bei der {{site.data.keyword.Bluemix_notm}}-Konsole an.

    Öffnen Sie einen Web-Browser und starten Sie das {{site.data.keyword.Bluemix_notm}}-Dashboard: [http://bluemix.net ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](http://bluemix.net){:new_window}
	
	Nach der Anmeldung mit Ihrer Benutzer-ID und Ihrem Kennwort wird die {{site.data.keyword.Bluemix_notm}}-Benutzerschnittstelle geöffnet.

2. Klicken Sie in der Menüleiste auf **Verwalten > Konto > Benutzer**.

    Im Fenster *Benutzer* wird eine Liste der Benutzer mit ihren E-Mail-Adressen für das aktuell ausgewählte Konto angezeigt.
	
3. Suchen Sie nach der Benutzer-ID des Eigners des {{site.data.keyword.containershort}}.

    Führen Sie den Befehl `bx cs api-key-info ClusterName` aus, um die Benutzer-ID des Eigners des {{site.data.keyword.containershort}} abzurufen.

4. Wählen Sie **Cloud Foundry-Zugriff** und anschließend **Organisation zuweisen** aus.

5. Geben Sie die folgenden Werte ein: 

    <table>
      <caption></caption>
      <tr>
        <th>Feld</th>
        <th>Wert</th>
      </tr>
      <tr>
        <td>Organisation</td>
        <td>MyOrg</td>
      </tr>
      <tr>
        <td>Organisationsrolle</td>
        <td>Manager</td>
      </tr>
      <tr>
        <td>Region</td>
        <td>USA (Süden)</td>
      </tr>
      <tr>
        <td>Bereich</td>
        <td>dev</td>
      </tr>
      <tr>
        <td>Bereichsrolle</td>
        <td>Entwickler</td>
      </tr>
	
6. Klicken Sie auf **Rolle speichern**.


### Berechtigungen zum Anzeigen von Metriken in der Kontodomäne erteilen
{: #acc_1}

Führen Sie die folgenden Schritte aus:

1. Melden Sie sich bei der {{site.data.keyword.Bluemix_notm}}-Konsole an.

    Öffnen Sie einen Web-Browser und starten Sie das {{site.data.keyword.Bluemix_notm}}-Dashboard: [http://bluemix.net ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](http://bluemix.net){:new_window}
	
	Nach der Anmeldung mit Ihrer Benutzer-ID und Ihrem Kennwort wird die {{site.data.keyword.Bluemix_notm}}-Benutzerschnittstelle geöffnet.

2. Klicken Sie in der Menüleiste auf **Verwalten>Konto>Benutzer**. 

    Im Fenster *Benutzer* wird eine Liste der Benutzer mit ihren E-Mail-Adressen für das aktuell ausgewählte Konto angezeigt.
	
3. Suchen Sie nach der Benutzer-ID des Eigners des {{site.data.keyword.containershort}}.

    Führen Sie den Befehl `bx cs api-key-info ClusterName` aus, um die Benutzer-ID des Eigners des {{site.data.keyword.containershort}} abzurufen.

4. Wählen Sie **Zugriffsrichtlinien > Zugriff zuweisen > Zugriff auf Ressourcen zuweisen** aus.

5. Wählen Sie den Service **{{site.data.keyword.monitoringlong}}**, die Region, in der der Cluster verfügbar ist (**USA (Süden)** für dieses Lernprogramm), und eine Rolle (**Administrator**) aus.	

## Schritt 5: Beispielapp im Kubernetes-Cluster bereitstellen
{: #step5}

Implementieren Sie die Beispielapp im Kubernetes-Cluster und führen Sie sie aus. Führen Sie die Schritte im folgenden Lernprogramm aus, um die Beispielapp bereitzustellen: [Lerneinheit 1: Einzelne Instanzapps in Kubernetes-Clustern bereitstellen](/docs/containers/cs_tutorials_apps.html#cs_apps_tutorial_lesson1).

Die App ist eine 'Hello World'-Node.js-App:

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
{: screen}

Wenn Sie in dieser Beispiel-App die App in einem Browser testen, schreibt die App die folgende Nachricht an 'stdout': `Sample app is listening on port 8080.`


## Schritt 6: Grafana starten und die Metrikdomäne festlegen
{: #step6}

Starten Sie Grafana über einen Browser und legen Sie die {{site.data.keyword.monitoringshort}}-Domäne fest, in der Sie die Clustermetriken anzeigen können.

Zum Analysieren von Metriken für einen Cluster müssen Sie auf Grafana in der öffentlichen Cloud-Region zugreifen, in der der Cluster erstellt wird. Weitere Informationen finden Sie unter [Von einem Web-Browser zum Grafana-Dashboard navigieren](/docs/services/cloud-monitoring/grafana/navigating_grafana.html#launch_grafana_from_browser).

1. Starten Sie Grafana über einen Browser. 

    Geben Sie die {{site.data.keyword.monitoringshort}}-Service-URL für die Region ein, in der Sie den Cluster erstellt haben. 
    
    Informationen zum Abrufen der URLs nach Region finden Sie unter [URLs für den Überwachungsservice](/docs/services/cloud-monitoring/monitoring_ov.html#region).

    Starten Sie zum Beispiel für die Region 'USA (Süden)': [https://metrics.ng.bluemix.net/](https://metrics.ng.bluemix.net/).

2. Legen Sie die {{site.data.keyword.monitoringshort}-Domäne fest, in der Sie die Clustermetriken anzeigen können.

    Wählen Sie in Grafana Ihre ID aus. Stellen Sie dann sicher, dass Sie sich im richtigen Konto befinden und wählen Sie eine Domäne aus.

    Cluster, denen ein CF-Bereich zugeordnet ist, leiten Metriken an die Bereichsmetrikdomäne weiter. Wählen Sie `Domäne = Bereich` sowie die Organisation und den Bereich aus, die/der dem Cluster zugeordnet ist.

    Cluster, die auf Kontoebene erstellt werden, leiten Metriken an die Kontometrikdomäne weiter. Wählen Sie `Domäne = Konto` aus.

## Schritt 7: Cluster in Grafana überwachen
{: #step7}

Der {{site.data.keyword.containershort}} stellt ein Grafana-Dashboard bereit, über das Sie Ihre Clustermetriken überwachen können. 

Führen Sie die folgenden Schritte aus, um das Beispieldashboard zu öffnen:

1. Wählen Sie das Steuerelement zum Hin- und Herschalten ![Grafana-Seitenmenüleiste](images/grafana_settings.gif "Grafana-Seitenmenüleiste") in der seitlichen Menüleiste aus.
2. Wählen Sie **Dashboards** aus.
3. Klicken Sie auf **Öffnen**.
4. Wählen Sie **ClusterMonitoringDashboard_v1** aus.

Das Beispieldashboard wird geöffnet. 

![Grafana-Beispieldashboard](images/cluster_grafana_sample_dashboard.png "Grafana-Beispieldashboard")



## Nächste Schritte
{: #next_steps}

Definieren Sie einen Alert für eine Metrik. Weitere Informationen finden Sie unter [Alerts konfigurieren ](/docs/services/cloud-monitoring/config_alerts_ov.html#config_alerts_ov).
