---

copyright:
  years: 2017

lastupdated: "2017-05-25"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

# Grafana 대시보드로 이동
{:#navigating_grafana}

{{site.data.keyword.Bluemix}}에서는 오픈 소스 분석 및 시각화 플랫폼인 Grafana를 사용하여 메트릭을 모니터하고, 검색하고, 분석하고, 다양한 그래프(예: 차트 및 표)로 시각화할 수 있습니다. Grafana를 사용하여 고급 분석 태스크를 수행하십시오.
{:shortdesc}

Grafana는 다음 방법 중 하나로 실행할 수 있습니다. 

* {{site.data.keyword.Bluemix_notm}}에서

    Grafana의 특정 Docker 컨테이너 메트릭은 해당 컨테이너의 컨텍스트에서 시작할 수 있습니다. 이 기능은 {{site.data.keyword.Bluemix_notm}} 관리 클라우드 인프라에 배치된 컨테이너에만 적용됩니다.  
    
    자세한 정보는 [{{site.data.keyword.Bluemix_notm}} 대시보드에서 Grafana 대시보드로 이동](navigating_grafana.html#launch_grafana_from_bluemix)을 참조하십시오. 

* 직접 브라우저 링크에서

    보고 있는 데이터가 제공된 {{site.data.keyword.Bluemix_notm}} 영역 내의 서비스로부터 로그를 집계하도록 Grafana를 실행할 수 있습니다. 
    
    자세한 정보는 [웹 브라우저에서 Grafana 대시보드로 이동](navigating_grafana.html#launch_grafana_from_browser)을 참조하십시오. 
    
Grafana에 대한 자세한 정보는 [Grafana 사용자 안내서 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://docs.grafana.org/guides/getting_started/){: new_window}을 참조하십시오. 


##  Bluemix 대시보드에서 Grafana 대시보드로 이동
{: #launch_grafana_from_bluemix}

**참고:** 이 기능은 {{site.data.keyword.Bluemix_notm}} 관리 클라우드 인프라에 배치된 컨테이너에만 적용됩니다.  

Grafana에 표시되는 데이터를 필터링하는 데 사용되는 조회는 Kibana를 실행하는 {{site.data.keyword.Bluemix_notm}} 컨테이너의 데이터를 검색합니다.  

Grafana에서 Docker 컨테이너의 메트릭을 보려면 다음 단계를 완료하십시오. 

1. {{site.data.keyword.Bluemix_notm}}에 로그인한 다음 {{site.data.keyword.Bluemix_notm}} 대시보드에서 컨테이너를 클릭하십시오.  
    
2. 탐색줄에서 **모니터링 및 로그**를 클릭하십시오. 모니터링 탭이 열립니다.  
    
3. **고급 보기**를 클릭하십시오. **Grafana** 대시보드가 열립니다. 


##  웹 브라우저에서 Grafana 대시보드로 이동
{: #launch_grafana_from_browser}

Grafana에 표시되는 데이터를 필터링하는 데 사용되는 조회는 {{site.data.keyword.Bluemix_notm}} 조직에 있는 영역의 데이터를 검색합니다. Grafana가 표시하는 메트릭 정보는 사용자가 로그인한 {{site.data.keyword.Bluemix_notm}} 조직의 영역 내에 배치된 모든 리소스에 대한 레코드를 포함합니다. 

브라우저에서 Grafana를 실행하려면 다음 단계를 완료하십시오. 

1. [https://metrics.ng.bluemix.net](https://metrics.ng.bluemix.net)을 열어 Grafana 사용자 인터페이스에 로그인하십시오. 
2. **Grafana**를 선택하십시오. 
     

