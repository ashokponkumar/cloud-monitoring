---

copyright:
  years: 2017

lastupdated: "2017-08-01"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

# Navigating to the Grafana dashboard
{:#navigating_grafana}

In {{site.data.keyword.Bluemix}}, you can use Grafana, an open source analytics and visualization platform, to monitor, search, analyze, and visualize your metrics in a variety of graphs, for example charts and tables. Use Grafana to perform advanced analytical tasks.
{:shortdesc}

You can launch Grafana in any of the following ways:

* From {{site.data.keyword.Bluemix_notm}}

    You can launch to your specific Docker container metrics in Grafana, in context to that specific container. This feature applies only to containers that are deployed in the {{site.data.keyword.Bluemix_notm}}-managed Cloud infrastructure. 
    
    For more information, see [Navigating to the Grafana dashboard from the {{site.data.keyword.Bluemix_notm}} 
    dashboard](navigating_grafana.html#launch_grafana_from_bluemix).

* From a direct browser link

    You can launch Grafana so the data that you see aggregates logs from services within a provided {{site.data.keyword.Bluemix_notm}} space.
    
    For more information, see [Navigating to the Grafana dashboard from a web browser](navigating_grafana.html#launch_grafana_from_browser).
    
For more information about Grafana, see the [Grafana User Guide ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://docs.grafana.org/guides/getting_started/){: new_window}.


##  Navigating to the Grafana dashboard from the Bluemix dashboard
{: #launch_grafana_from_bluemix}

**Note:** This feature applies only to containers that are deployed in the {{site.data.keyword.Bluemix_notm}}-managed Cloud infrastructure. 

The query that is used to filter the data that is displayed in Grafana retrieves data for the {{site.data.keyword.Bluemix_notm}} container from where you launch Kibana. 

To see the metrics of a Docker container in Grafana, complete the following steps:

1. Log in to {{site.data.keyword.Bluemix_notm}}, and then click the container from the {{site.data.keyword.Bluemix_notm}} dashboard. 
    
2. In the navigation bar, click **Monitoring and logs**. The monitoring tab opens. 
    
3. Click **Advanced view**. The **Grafana** dashboard opens.


##  Navigating to the Grafana dashboard from a web browser
{: #launch_grafana_from_browser}

The query that is used to filter the data that is displayed in Grafana retrieves data for a space in the {{site.data.keyword.Bluemix_notm}} organization. The metrics information that Grafana displays includes records for all resources that are deployed within the space of the {{site.data.keyword.Bluemix_notm}} organization that you are logged in.

Complete the following steps to launch Grafana from a browser:

1. Open a web browser. 
2. Enter the URL for the region where you want to monitor metrics. 

    The following table lists the URLs per region:
	<table>
      <caption>URLs to launch Grafana in different regions</caption>
      <tr>
        <th>Region</th>
	    <th>Endpoint</th>
      </tr>
      <tr>
        <td>US South</td>
    	<td>[https://metrics.ng.bluemix.net](https://metrics.ng.bluemix.net)</td>
      </tr>
      <tr>
        <td>United Kingdom</td>
	    <td>[https://metrics.eu-gb.bluemix.net](https://metrics.eu-gb.bluemix.net)</td>
      </tr>
    </table>
	
2. Select **Grafana**.
     

