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


# Grafana 사용에 대해 자주 묻는 질문 및 응답
{: #grafana_qa}

여기에는 Grafana를 {{site.data.keyword.monitoringshort}} 서비스와 함께 사용하는 데 대한 일반적인 질문의 답이 제공되어 있습니다.
{:shortdesc}

* [Monitoring 서비스 웹 UI에 로그인할 때 404 오류가 발생함](/docs/services/cloud-monitoring/qa/grafana_qa.html#404)
* [방금 Grafana 대시보드를 위한 json 데이터를 업로드했는데 화면이동 막대가 사라진 이유가 무엇입니까?](/docs/services/cloud-monitoring/qa/grafana_qa.html#2)


## UUA 인증 모델을 사용해 Monitoring 서비스 웹 UI에 로그인할 때 404 오류가 발생함
{: #404}

{{site.data.keyword.monitoringshort}} 서비스 웹 UI(Grafana)에 로그인하는 중에 404 오류가 발생하는 경우에는 해당 영역이 존재하며 여기에 대한 액세스 권한이 있는지 확인하십시오. 

[UAA 인증 모델](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_uaa)을 사용하여 {{site.data.keyword.monitoringshort}} 서비스에 액세스하는 경우에는 {{site.data.keyword.Bluemix_notm}} 콘솔에 로그인할 때 사용하는 것과 같은 사용자 ID 및 비밀번호를 사용해야 합니다.  

로그인하려는 계정, 조직 및 영역에 대한 액세스 권한이 있는지 확인하려면 {{site.data.keyword.Bluemix_notm}} 콘솔에 로그인한 후 영역으로 전환하십시오.  

명령행을 사용하여 영역에 대한 액세스 권한이 있는지 확인할 수도 있습니다. 다음 명령을 실행하여 {{site.data.keyword.Bluemix_notm}} 지역, 조직 및 영역에 로그인하십시오. 

```
bx login -a https://api.ng.bluemix.net
```
{: codeblock}

지시사항을 따르십시오. {{site.data.keyword.Bluemix_notm}} 신임 정보를 입력하고 조직 및 영역을 선택하십시오. 


## 방금 Grafana 대시보드를 위한 JSON 데이터를 업로드했는데 화면이동 막대가 사라진 이유가 무엇입니까?
{: #2}

Grafana에는 대시보드를 가져온 후 화면이동 막대가 숨겨지는 버그가 있습니다.  

화면이동 막대를 표시하려면 대시보드를 가져온 후 저장하십시오.  








