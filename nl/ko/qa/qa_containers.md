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



# Kubernetes 클러스터 모니터링에 대한 FAQ
{: #qa_containers}

여기에는 {{site.data.keyword.monitoringshort}} 서비스 및 {{site.data.keyword.containershort_notm}} 서비스에 대한 일반적인 질문의 답이 제공되어 있습니다.
{:shortdesc}

* [내 컨테이너에 대한 Grafana 조회가 데이터를 표시하지 않거나 오류가 있음](/docs/services/cloud-monitoring/qa/qa_containers.html#metric_format_change)
* [내 터미널 세션에서 클러스터 환경을 설정하는 방법](/docs/services/cloud-monitoring/qa/qa_containers.html#qa1)
* [클러스터와 연관된 영역의 영역 ID 및 영역 이름을 찾는 방법](/docs/services/cloud-monitoring/qa/qa_containers.html#qa2)
* [클러스터 이름 및 클러스터 ID를 가져오는 방법](/docs/services/cloud-monitoring/qa/qa_containers.html#qa3)
* [네임스페이스의 목록을 가져오는 방법](/docs/services/cloud-monitoring/qa/qa_containers.html#qa7)
* [Kubernetes 클러스터 내의 네임스페이스의 팟 목록을 가져오는 방법](/docs/services/cloud-monitoring/qa/qa_containers.html#qa8)
* [네임스페이스 기준으로 클러스터 내의 모든 팟을 가져오는 방법](/docs/services/cloud-monitoring/qa/qa_containers.html#qa9)

## 내 컨테이너에 대한 Grafana 조회가 데이터를 표시하지 않거나 오류가 있음
{: #metric_format_change}

컨테이너에 대해 정의할 수 있는 조회의 형식이 변경되었습니다.

다음 형식은 더 이상 사용되지 않습니다. 

```
{Prefix}.{Version}.{Provider}.{Type}.{ServiceName}.{Region}.{Account}.{Cluster}.{Metric}.{Container in a pod}.{Functions}
```
{: screen}

유효한 새 형식은 다음과 같습니다.

* [컨테이너에 대한 CPU 메트릭 조회 형식](/docs/services/cloud-monitoring/reference/metrics_format.html#cpu_containers)
* [작업자에 대한 로드 메트릭 조회 형식](/docs/services/cloud-monitoring/reference/metrics_format.html#load_workers)
* [컨테이너에 대한 메모리 메트릭 조회 형식](/docs/services/cloud-monitoring/reference/metrics_format.html#mem_containers)

이전 조회를 새 형식으로 마이그레이션하여 Grafana에서 데이터를 시각화하십시오.

예를 들어, 조회가 `crn.v1.bluemix.public.containers-kubernetes.us-south`로 시작하는 경우, 새 형식인 `ibmcloud.public.containers-kubernetes.us-south`로 조회를 마이그레이션해야 합니다.

다음 표에는 더 이상 사용되지 않는 형식의 필드가 나열되어 있습니다.

 <table>
      <caption>표 1. 컨테이너에 대한 Grafana 조회 필드</caption>
      <tr>
        <th align="center">필드</th>
        <th align="center">설명</th>
        <th align="center">올바른 값</th>
      </tr>
      <tr>
        <td>Prefix</td>
        <td>{{site.data.keyword.IBM_notm}} Cloud 리소스임을 표시하는 데 사용되는 접두부입니다.</td>
        <td>`crn`</td>
      </tr>
      <tr>
        <td>Version</td>
        <td>수집된 메트릭 데이터의 형식 및 필드를 표시하는 버전입니다. </td>
        <td>`v1`</td>
      </tr>
      <tr>
        <td>Provider</td>
        <td>데이터가 수집되는 클라우드 제공자입니다. 이 필드는 영숫자 ID입니다.</td>
        <td>퍼블릭 {{site.data.keyword.IBM_notm}} Cloud의 경우 값은 `bluemix`입니다.</td>
      </tr>
      <tr>
        <td>유형</td>
        <td>데이터가 수집되는 클라우드 환경입니다.</td>
        <td>퍼블릭 {{site.data.keyword.IBM_notm}} Cloud의 경우 값은 `public`입니다.</td>
      </tr>
      <tr>
        <td>Service name</td>
        <td>메트릭이 수집되는 클라우드 인프라입니다.</td>
        <td>{{site.data.keyword.monitoringshort}} 서비스의 경우 값은 `containers-kubernetes`입니다.</td>
      </tr>
      <tr>
        <td>Region</td>
        <td>메트릭이 수집되는 클라우드 지역입니다.</td>
        <td>미국 남부 지역의 경우 값은 `ng`입니다. <br>영국 지역의 경우 값은 `eu-gb`입니다. <br>독일의 경우 값은 `eu-de`입니다. <br>시드니의 경우 값은 `au-syd`입니다. </td>
      </tr>
      <tr>
        <td>Account</td>
        <td>메트릭이 수집되는 계정의 GUID입니다. <br>이 필드의 형식은 `a_ID`이며, 여기서 ID는 계정의 GUID입니다. <br>계정의 GUID를 가져오려면 [계정의 GUID를 가져오는 방법](/docs/services/cloud-monitoring/qa/cli_qa.html#account_guid)을 참조하십시오.</td>
        <td>a_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx</td>
      </tr>
      <tr>
        <td>Cluster</td>
        <td>메트릭이 수집되는 클러스터의 GUID입니다.</td>
        <td>xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx</td>
      </tr>
      <tr>
        <td>Metric</td>
        <td>컨테이너에 대해 자동으로 수집되는 메트릭입니다.</td>
        <td>CPU 메트릭 목록은 [컨테이너에 대한 CPU 메트릭](/docs/services/cloud-monitoring/containers/monitoring_containers_ov.html#cpu_metrics_containers)을 참조하십시오.<br>메모리 메트릭 목록은 [메모리 메트릭](/docs/services/cloud-monitoring/containers/monitoring_containers_ov.html#memory_metrics)을 참조하십시오.</td>
      </tr>
      <tr>
        <td>Container in a pod</td>
        <td>포드에서 실행되는 컨테이너를 고유하게 식별하는 데 필요한 Kubernetes 리소스 이름과 GUID의 조합입니다. <br>**참고:** 조회에서 이 항목에 대해 사용 가능한 옵션의 목록을 볼 때 {namespace}{pod_name}{deploymentname_name}_POD{container_ID} 형식의 항목이 있는지 알아보십시오. 이 항목은 Kubernetes에 의해 작성된 내부 컨테이너 ID에 해당합니다.</td>
		<td>{namespace}_{pod_name}_{deployment_name}_{container_id}</td>
      </tr>
      <tr>
        <td>Functions</td>
        <td>패널에서 컨테이너 메트릭을 시각화하기 위해 선택할 수 있는 조회 함수입니다. <br>자세한 정보는 [함수![(외부 링크 아이콘)](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://graphite.readthedocs.io/en/latest/functions.html){: new_window}를 참조하십시오.</td>
        <td></td>
      </tr>
    </table>


## 내 터미널 세션에서 클러스터 환경을 설정하는 방법
{: #qa1}

명령을 사용하여 Kubernetes 클러스터를 관리하려면 해당 컨텍스트를 설정해야 합니다.

**참고:** 클러스터를 관리하려면 태스크를 완료하는 데 필요한 권한과 함께 사용자에게 지정된 {{site.data.keyword.containershort_notm}} 서비스에 대한 IAM 정책이 필요합니다.

클러스터의 컨텍스트를 설정하려면 다음 단계를 완료하십시오.

1. 작성한 클러스터와 연관된 {{site.data.keyword.Bluemix_notm}} 지역, 조직 및 영역에 로그인하십시오. 자세한 정보는 [{{site.data.keyword.Bluemix_notm}}에 로그인하는 방법](/docs/services/cloud-monitoring/qa/cli_qa.html#login)을 참조하십시오.

2. {{site.data.keyword.containershort_notm}} 서비스 플러그인을 초기화하십시오. 다음 명령을 실행하십시오.

	```
	bx cs init
	```
	{: codeblock}

3. 터미널 세션에서 클러스터의 컨텍스트를 설정하십시오. 다음 명령을 실행하십시오. 
    
	```
	bx cs cluster-config MyCluster
	```
	{: codeblock}

    이 명령 실행의 출력은 구성 파일에 대한 경로를 설정하기 위해 터미널에서 실행해야 하는 명령을 제공합니다. 예를 들면 다음과 같습니다.

	```
	export KUBECONFIG=/Users/ibm/.bluemix/plugins/container-service/clusters/MyCluster/kube-config-hou02-MyCluster.yml
	```
	{: codeblock}

    명령을 복사하고 붙여넣는 방법으로 터미널에서 환경 변수를 설정한 다음 **Enter**를 누르십시오.

 
 
## 클러스터와 연관된 영역의 영역 ID 및 영역 이름을 찾는 방법
{: #qa2}

클러스터가 {{site.data.keyword.Bluemix_notm}} 계정에서 작성되면 메트릭이 계정 내의 영역과 연관됩니다. 클러스터 메트릭을 보기 위해 조회를 작성할 때 영역 ID가 필요합니다.
{:shortdesc}

**참고:** 클러스터를 관리하려면 태스크를 완료하는 데 필요한 권한과 함께 사용자에게 지정된 {{site.data.keyword.containershort_notm}} 서비스에 대한 IAM 정책이 필요합니다.

클러스터에 대한 영역 ID를 찾으려면 다음 단계를 완료하십시오.

1. 작성한 클러스터와 연관된 {{site.data.keyword.Bluemix_notm}} 지역, 조직 및 영역에 로그인하십시오. 자세한 정보는 [{{site.data.keyword.Bluemix_notm}}에 로그인하는 방법](/docs/services/cloud-monitoring/qa/cli_qa.html#login)을 참조하십시오.

2. {{site.data.keyword.containershort_notm}} 서비스 플러그인을 초기화하십시오. 다음 명령을 실행하십시오.

	```
	bx cs init
	```
	{: codeblock}

3. 클러스터 세부사항을 가져오십시오. 다음 명령을 실행하십시오.

    ```
    bx cs cluster-get cluster-name
    ```
    {: codeblock}

    여기서, **cluster-name**은 클러스터의 이름입니다.

    영역 ID는 **Log Space** 필드에 대해 표시되는 값입니다.

    예를 들어, 명령 실행의 출력은 다음과 같습니다.

    ```
    Name:			    cluster-name
    ID:			        c213f81296db4c68b84e212c19135a99
    State:			    normal
    Created:		    2017-08-22T18:18:59+0000
    Datacenter:		    dal10
    Master URL:		    https://169.46.7.242:21210
    Ingress subdomain:  cluster-name.us-south.containers.mybluemix.net
    Ingress secret:     cluster-name
    Workers:		    5
    Log Space:		    fa277ff8-8a73-324b-9b75-0f11d54a3ae2
    ```
    {: screen}

4. 영역 이름을 가져오십시오.

    모든 영역 이름 나열:
	
    ```
	bx account spaces
	```
    {: codeblock}
	
	일치하는 ID 이름을 찾을 때까지 각 영역에 대해 다음 명령 실행:
	
	```
	bx iam space Spacename --guid
	```
	{: codeblock}
	
	


## 클러스터 이름 및 클러스터 ID를 가져오는 방법
{: #qa3}

UI를 통해 클러스터 이름 및 ID를 가져오려면 다음 단계를 완료하십시오.

1. {{site.data.keyword.Bluemix_notm}} 계정에 로그인하십시오.

    {{site.data.keyword.Bluemix_notm}} 대시보드는 [http://bluemix.net ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://bluemix.net){:new_window}에서 찾을 수 있습니다.

	사용자 ID 및 비밀번호를 사용하여 로그인하면 {{site.data.keyword.Bluemix_notm}} UI가 열립니다.

2. {{site.data.keyword.Bluemix_notm}} *대시보드*에서 **메뉴 > 컨테이너**를 선택하십시오.

    계정에서 사용 가능한 클러스터의 목록이 표시됩니다.

3. 클러스터 ID를 가져오려면 클러스터 항목을 선택하십시오.

    개요 페이지가 열립니다. 이 페이지에서 클러스터 ID를 가져올 수 있습니다.



다음 단계를 완료하여 명령행을 통해 클러스터 이름 및 ID를 가져올 수 있습니다.

1. 작성한 클러스터와 연관된 {{site.data.keyword.Bluemix_notm}} 지역, 조직 및 영역에 로그인하십시오. 자세한 정보는 [{{site.data.keyword.Bluemix_notm}}에 로그인하는 방법](/docs/services/cloud-monitoring/qa/cli_qa.html#login)을 참조하십시오.

2. 계정에서 사용 가능한 클러스터를 나열하십시오. 다음 명령을 실행하십시오. 

    ```
	bx cs clusters
	``` 
	{: codeblock}
	
	이 명령의 출력에는 계정의 모든 클러스터가 해당 ID와 함께 나열됩니다.
	
3. 클러스터 ID를 가져오려면 다음 명령을 실행하십시오.

    ```
	bx cs cluster-get my_cluster
	```
    {: screen}	
 


## 네임스페이스의 목록을 가져오는 방법
{: #qa7}

클러스터에 있는 모든 네임스페이스의 목록을 가져오려면 다음 단계를 완료하십시오.

다음 단계를 완료하십시오.

1. 클러스터 컨텍스트를 설정하십시오. 자세한 정보는 [내 터미널 세션에서 클러스터 환경을 설정하는 방법](/docs/services/cloud-monitoring/qa/qa_containers.html#qa1)을 참조하십시오.

2. 모든 네임스페이스를 나열하십시오. 다음 kubectl 명령을 실행하십시오.

    ```
    kubectl get namespaces
	```
	        {: codeblock}

## Kubernetes 클러스터 내의 네임스페이스별 팟 목록을 가져오는 방법
{: #qa8}
		
네임스페이스의 팟 목록을 가져오려면 다음 단계를 완료하십시오.

1. 클러스터 컨텍스트를 설정하십시오. 자세한 정보는 [내 터미널 세션에서 클러스터 환경을 설정하는 방법](/docs/services/cloud-monitoring/qa/qa_containers.html#qa1)을 참조하십시오.

2. Kubernetes 클러스터 내의 네임스페이스별 팟 목록을 가져오려면 다음 명령을 실행하십시오.

    ```
	kubectl --namespace=NamespaceName get pods
	```
	{: codeblock}
	
	여기서, Namespacename은 네임스페이스의 이름이며 예를 들어, *default*입니다.

## 네임스페이스 기준으로 클러스터 내의 모든 팟을 가져오는 방법
{: #qa9}
		
다음 단계를 완료하십시오.

1. 클러스터 컨텍스트를 설정하십시오. 자세한 정보는 [내 터미널 세션에서 클러스터 환경을 설정하는 방법](/docs/services/cloud-monitoring/qa/qa_containers.html#qa1)을 참조하십시오.

2. 네임스페이스 기준으로 클러스터 내의 모든 팟을 가져오려면 다음 명령을 실행하십시오.

	```
	kubectl get pods --all-namespaces
	```
	{: codeblock}
		


