---

copyright:
  years: 2017

lastupdated: "2017-06-19"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Cloud Foundry에서 실행 중인 앱 모니터링
 {:#monitoring_bluemix_apps}

{{site.data.keyword.monitoringshort}} 서비스에 메트릭을 전송하려는 경우, Cloud Foundry(CF) 애플리케이션은 {{site.data.keyword.monitoringshort}} REST API를 사용하여 앱에서 메트릭을 바로 전송할 수 있습니다.
{:shortdesc}

사용자는 Cloud Foundry 인프라를 사용하여 앱을 {{site.data.keyword.Bluemix_notm}}에서 실행하면서 앱의 상태, 리소스 사용량 및 트래픽 메트릭을 모니터하려 할 수 있습니다. 이러한 성능 정보가 있으면 그에 맞게 의사 결정을 하거나 조치를 취할 수 있습니다. 


Cloud Foundry에서 실행되는 앱은 collectd를 실행할 수 없습니다. 대신, lumberjack 클라이언트 또는 RESTful API를 사용하여 데이터를 앱에서 직접 전송해야 합니다.  

##CF 앱에서 데이터를 전송하는 또 다른 방법 [@lindj가 collectd 및 statsd 빌드팩을 작성함] (https://github.com/jimlindeman/python-buildpack-collectd)

이는 statsd 입력 플러그인 및 Bluemix의 멀티 테넌트 graphite 출력 플러그인을 사용하여 localhost:8125를 청취하는 collectd가 설치된 python-buildpack 릴리스의 복제본입니다.  

빌드팩을 사용하여 Bluemix에 CF 앱을 배치할 수 있습니다. 빌드팩은 앱에서 필요로 하는 프레임워크 및 런타임 지원을 제공합니다. 사용자는 statsd 입력 플러그인 및 Bluemix의 멀티 테넌트 graphite 출력 플러그인을 사용하여 localhost:8125를 청취하는 collectd를 설치하고 구성할 수 있습니다.  
