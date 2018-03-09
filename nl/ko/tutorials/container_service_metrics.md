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


# Kubernetes 클러스터에 배치된 앱에 대한 Grafana에서 메트릭 분석
{: #container_service_metrics}

이 튜토리얼을 사용하여 {{site.data.keyword.monitoringlong}} 서비스를 사용하여 컨테이너의 성능을 모니터링하는 방법을 배울 수 있습니다.
{:shortdesc}


## 목표
{: #objectives}

Kubernetes 클러스터에 배치된 앱에 대한 컨테이너 메트릭을 검색하고 분석하는 방법을 학습하십시오.

1. 클러스터에서 수집된 메트릭이 {{site.data.keyword.monitoringshort}} 서비스로 전달되는 위치를 식별하십시오. 
2. Grafana를 실행하고 클러스터 메트릭을 볼 수 있는 {{site.data.keyword.monitoringshort} 도메인을 설정하십시오.
3. {{site.data.keyword.Bluemix_notm}}에서 Kubernetes 클러스터에 배치된 앱에 대한 컨테이너 메트릭을 검색하고 분석하십시오.

이 튜토리얼은 {{site.data.keyword.Bluemix_notm}}에서 작동하고 클러스터를 프로비저닝하고 클러스터가 {{site.data.keyword.Bluemix_notm}}의 {{site.data.keyword.monitoringshort}} 서비스에 메트릭을 전송하는 위치를 식별하고 클러스터에서 앱을 배치하고 Grafana를 사용하여 해당 클러스터에 대한 클러스터 메트릭을 보고 필터링하는 다음과 같은 엔드 투 엔드 시나리오를 가져오는 데 필요한 단계를 수행합니다.
**참고:** 이 튜토리얼을 완료하려면 전제조건 및 다른 단계에서 링크된 튜토리얼을 완료해야 합니다.


## 전제조건
{: #prereqs}

1. Kubernetes 표준 클러스터를 작성하고 클러스터에 앱을 배치하고 Grafana에서 모니터링하기 위해 {{site.data.keyword.Bluemix_notm}}에서 메트릭을 조회할 수 있는 권한이 있는 {{site.data.keyword.Bluemix_notm}} 계정의 구성원 또는 소유자여야 합니다.

    {{site.data.keyword.Bluemix_notm}}에 대한 사용자 ID에는 다음과 같은 정책이 지정되어 있어야 합니다.
    
    * {{site.data.keyword.containershort}}에 대한 IAM 정책(*운영자* 또는 *관리자* 권한의 경우)
    * {{site.data.keyword.monitoringshort}} 서비스가 프로비저닝된 영역에 대한 CF 역할(*개발자* 권한의 경우)
    
    자세한 정보는 [IBM Cloud UI를 통해 사용자에게 IAM 정책 지정](/docs/services/cloud-monitoring/security/grant_permissions.html#grant_permissions_ui_account) 및 [IBM Cloud UI를 사용하여 사용자에게 CF 역할 부여](/docs/services/cloud-monitoring/security/grant_permissions.html#grant_permissions_ui_space)를 참조하십시오.

2. Kubernetes 클러스터를 관리하고 명령행에서 앱을 배치할 수 있는 터미널 세션이 있어야 합니다. 이 튜토리얼의 경우, Ubuntu Linux 시스템에 대한 예입니다.

3. Ubuntu 시스템에서 {{site.data.keyword.containershort}}에 대해 작업할 수 있도록 CLI를 설치하십시오.

    * {{site.data.keyword.Bluemix_notm}} CLI를 설치하십시오. 자세한 정보는 [{{site.data.keyword.Bluemix_notm}} CLI 설치](/docs/cli/reference/bluemix_cli/download_cli.html#download_install)를 참조하십시오.
    
    * {{site.data.keyword.containershort}} CLI를 설치하여 {{site.data.keyword.containershort}}에서 Kubernetes 클러스터를 작성 및 관리하고 컨테이너화된 앱을 클러스터에 배치하십시오. 자세한 정보는 [CS 플러그인 설치](/docs/containers/cs_cli_install.html#cs_cli_install_steps)를 참조하십시오.
    

    
 

## 1단계; Kubernetes 클러스터 프로비저닝
{: #step1}

다음 단계를 완료하십시오.

1. 표준 Kubernetes 클러스터를 작성하십시오.

   * [UI를 통해 Kubernetes 표준 클러스터를 작성하십시오.](/docs/containers/cs_cluster.html#cs_cluster_ui).
   * [CLI를 사용하여 Kubernetes 표준 클러스터를 작성하십시오.](/docs/containers/cs_cluster.html#cs_cluster_cli).

2. 터미널에서 클러스터 컨텍스트를 설정하십시오. 컨텍스트가 설정된 후에는 Kubernetes 클러스터를 관리하고 Kubernetes 클러스터에 애플리케이션을 배치할 수 있습니다.

    작성한 클러스터와 연관된 {{site.data.keyword.Bluemix_notm}} 지역, 조직 및 영역에 로그인하십시오. 자세한 정보는 [{{site.data.keyword.Bluemix_notm}}에 로그인하는 방법](/docs/services/CloudLogAnalysis/qa/cli_qa.html#login)을 참조하십시오.

	{{site.data.keyword.containershort}} 서비스 플러그인을 초기화하십시오.

	```
	bx cs init
	```
	{: codeblock}

    클러스터에 터미널 컨텍스트를 설정하십시오.
    
	```
	bx cs cluster-config MyCluster
	```
	{: codeblock}

    이 명령 실행의 출력은 구성 파일에 대한 경로를 설정하기 위해 터미널에서 실행해야 하는 명령을 제공합니다. 예: 

	```
	export KUBECONFIG=/Users/ibm/.bluemix/plugins/container-service/clusters/MyCluster/kube-config-hou02-MyCluster.yml
	```
	{: codeblock}

    명령을 복사하고 붙여넣는 방법으로 터미널에서 환경 변수를 설정한 다음 **Enter**를 누르십시오.



## 2단계: 클러스터가 메트릭을 {{site.data.keyword.monitoringshort}} 서비스에 전달하는 위치 식별
{: #step2}

클러스터는 계정 레벨 리소스입니다. {{site.data.keyword.containershort}}에서 클러스터를 프로비저닝하는 경우, 클러스터는 계정 레벨에서 작성되거나 연관된 CF(Cloud Foundry)를 사용하여 작성될 수 있습니다. 클러스터가 프로비저닝되고 준비되는 즉시 메트릭이 자동으로 수집되고 {{site.data.keyword.monitoringshort}} 서비스로 전달됩니다.

* 연관된 CF 영역이 있는 클러스터가 메트릭을 영역 메트릭 도메인으로 전달합니다.
* 계정 레벨에서 작성된 클러스터가 메트릭을 계정 메트릭 도메인으로 전달합니다.

클러스터가 메트릭을 전달하는 위치를 식별하려면 다음 명령을 실행하십시오.

```
$ bx cs cluster-get ClusterName --json
```
{: codeblock}

여기서, *ClusterName*은 클러스터의 이름입니다.

출력에서 다음 필드가 메트릭이 전달되는 위치에 대한 정보를 표시합니다.

* **logOrg**는 CF 조직의 ID를 정의합니다.
* **logOrgName**은 CF 조직의 이름을 정의합니다.
* **logSpace**는 CF 조직의 ID를 정의합니다.
* **logSpaceName**는 CF 영역의 이름을 정의합니다.

필드가 비어 있으면 메트릭이 계정 도메인으로 전달됩니다.
필드에 CF 조직 및 CF 영역 세트가 있는 경우, 메트릭이 이 영역과 연관된 영역 도메인으로 전달됩니다.

예를 들어, 메트릭을 계정 도메인에 전달하는 클러스터의 출력은 다음과 유사합니다.

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





## 3단계: 사용자에게 메트릭 도메인에서 메트릭을 볼 수 있는 권한 부여
{: #step3}

사용자에게 영역 도메인에서 메트릭을 볼 수 있는 권한을 부여하려면 해당 사용자가 영역에서 {{site.data.keyword.monitoringshort}} 서비스를 사용하여 수행할 수 있는 조치를 설명하는 CF 역할을 사용자에게 지정해야 합니다.

사용자에게 계정 도메인에서 메트릭을 볼 수 있는 권한을 부여하려면 해당 사용자가 {{site.data.keyword.monitoringshort}} 서비스를 사용하여 수행할 수 있는 조치를 설명하는 IAM 정책을 사용자에게 지정해야 합니다.

### 사용자에게 영역 도메인에서 메트릭을 볼 수 있는 권한 부여
{: #space}

사용자에게 {{site.data.keyword.monitoringshort}} 서비스에 대해 작업할 수 있는 권한을 부여하려면 다음 단계를 완료하십시오.

1. {{site.data.keyword.Bluemix_notm}} 콘솔에 로그인하십시오.

    웹 브라우저를 열고 {{site.data.keyword.Bluemix_notm}} 대시보드: [http://bluemix.net ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://bluemix.net){:new_window}를 실행하십시오.
	
	사용자 ID 및 비밀번호를 사용하여 로그인하면 {{site.data.keyword.Bluemix_notm}} UI가 열립니다.

2. 메뉴 표시줄에서 **관리 > 계정 > 사용자**를 클릭하십시오.

    *사용자* 창에 사용자의 목록이 현재 선택된 계정에 대한 해당 이메일 주소와 함께 표시됩니다.
	
3. 사용자가 계정의 구성원이면 목록에서 사용자 이름을 선택하거나 *조치* 메뉴에서 **사용자 관리**를 클릭하십시오.

    사용자가 계정의 구성원이 아닌 경우 [사용자 초대](/docs/iam/iamuserinv.html#iamuserinv)를 참조하십시오.

4. **Cloud Foundry 액세스**를 선택한 다음 **조직 지정**을 선택하십시오.

5. 다음 값을 입력하십시오.

    <table>
      <caption></caption>
      <tr>
        <th>필드</th>
        <th>값</th>
      </tr>
      <tr>
        <td>조직</td>
        <td>MyOrg</td>
      </tr>
      <tr>
        <td>조직 역할</td>
        <td>조직 역할 없음</td>
      </tr>
      <tr>
        <td>지역</td>
        <td>미국 남부</td>
      </tr>
      <tr>
        <td>영역</td>
        <td>dev</td>
      </tr>
      <tr>
        <td>영역 역할</td>
        <td>감사자</td>
      </tr>
	
6. **역할 저장**을 클릭하십시오.

### 사용자에게 계정 도메인에서 메트릭을 볼 수 있는 권한 부여
{: #acc}

사용자에게 {{site.data.keyword.monitoringshort}} 서비스에 대해 작업할 수 있는 권한을 부여하려면 다음 단계를 완료하십시오.

1. {{site.data.keyword.Bluemix_notm}} 콘솔에 로그인하십시오.

    웹 브라우저를 열고 {{site.data.keyword.Bluemix_notm}} 대시보드: [http://bluemix.net ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://bluemix.net){:new_window}를 실행하십시오.
	
	사용자 ID 및 비밀번호를 사용하여 로그인하면 {{site.data.keyword.Bluemix_notm}} UI가 열립니다.

2. 메뉴 표시줄에서 **관리 > 계정 > 사용자**를 클릭하십시오.

    *사용자* 창에 사용자의 목록이 현재 선택된 계정에 대한 해당 이메일 주소와 함께 표시됩니다.
	
3. 사용자가 계정의 구성원이면 목록에서 사용자 이름을 선택하거나 *조치* 메뉴에서 **사용자 관리**를 클릭하십시오.

    사용자가 계정의 구성원이 아닌 경우 [사용자 초대](/docs/iam/iamuserinv.html#iamuserinv)를 참조하십시오.

4. **정책 액세스 > 액세스 지정 > 리소스에 대한 액세스 지정**을 선택하십시오.

5. **{{site.data.keyword.monitoringlong}}** 서비스를 선택하고 클러스터가 사용 가능한 지역을 선택하십시오. 이 튜토리얼의 경우, **미국 남부**입니다. 그런 다음 **뷰어** 역할을 선택하십시오.



## 4단계: {{site.data.keyword.containershort_notm}} 키 소유자 권한 부여
{: #step4}

클러스터가 메트릭을 영역 도메인에 전달할 때 조직 및 영역 내의 {{site.data.keyword.containershort}} 키 소유자에게 CF(Cloud Foundry) 권한도 부여해야 합니다. 키 소유자는 조직에 대한 *orgManager* 역할, 영역에 대한 *SpaceManager* 및 *Developer* 역할이 필요합니다.

클러스터가 메트릭을 계정 도메인에 전달할 때 {{site.data.keyword.containershort}} 키 소유자가 {{site.data.keyword.monitoringshort}} 서비스에 대한 관리자 권한 있는 IAM 정책을 보유하고 있어야 합니다.

### 영역 도메인에서 메트릭을 볼 수 있는 권한 부여
{: #space_1}

{{site.data.keyword.containershort}} 키 소유자 사용자 ID에 다음 권한 부여: 조직에 대한 *orgManager* 역할 및 영역에 대한 *SpaceManager* 및 *Developer*. 다음 단계를 완료하십시오.
    
1. {{site.data.keyword.Bluemix_notm}} 콘솔에 로그인하십시오.

    웹 브라우저를 열고 {{site.data.keyword.Bluemix_notm}} 대시보드: [http://bluemix.net ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://bluemix.net){:new_window}를 실행하십시오.
	
	사용자 ID 및 비밀번호를 사용하여 로그인하면 {{site.data.keyword.Bluemix_notm}} UI가 열립니다.

2. 메뉴 표시줄에서 **관리 > 계정 > 사용자**를 클릭하십시오.

    *사용자* 창에 사용자의 목록이 현재 선택된 계정에 대한 해당 이메일 주소와 함께 표시됩니다.
	
3. {{site.data.keyword.containershort}} 키 소유자 사용자 ID를 찾으십시오.

    `bx cs api-key-info ClusterName` 명령을 실행하여 {{site.data.keyword.containershort}} 키 소유자 사용자 ID를 가져오십시오.

4. **Cloud Foundry 액세스**를 선택한 다음 **조직 지정**을 선택하십시오.

5. 다음 값을 입력하십시오. 

    <table>
      <caption></caption>
      <tr>
        <th>필드</th>
        <th>값</th>
      </tr>
      <tr>
        <td>조직</td>
        <td>MyOrg</td>
      </tr>
      <tr>
        <td>조직 역할</td>
        <td>관리자</td>
      </tr>
      <tr>
        <td>Region</td>
        <td>미국 남부</td>
      </tr>
      <tr>
        <td>영역</td>
        <td>dev</td>
      </tr>
      <tr>
        <td>영역 역할</td>
        <td>개발자</td>
      </tr>
	
6. **역할 저장**을 클릭하십시오.


### 계정 도메인에서 메트릭을 볼 수 있는 권한 부여
{: #acc_1}

다음 단계를 완료하십시오.

1. {{site.data.keyword.Bluemix_notm}} 콘솔에 로그인하십시오.

    웹 브라우저를 열고 {{site.data.keyword.Bluemix_notm}} 대시보드: [http://bluemix.net ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://bluemix.net){:new_window}를 실행하십시오.
	
	사용자 ID 및 비밀번호를 사용하여 로그인하면 {{site.data.keyword.Bluemix_notm}} UI가 열립니다.

2. 메뉴 표시줄에서 **관리 > 계정 > 사용자**를 클릭하십시오. 

    *사용자* 창에 사용자의 목록이 현재 선택된 계정에 대한 해당 이메일 주소와 함께 표시됩니다.
	
3. {{site.data.keyword.containershort}} 키 소유자 사용자 ID를 찾으십시오.

    `bx cs api-key-info ClusterName` 명령을 실행하여 {{site.data.keyword.containershort}} 키 소유자 사용자 ID를 가져오십시오.

4. **정책 액세스 > 액세스 지정 > 리소스에 대한 액세스 지정**을 선택하십시오.

5. **{{site.data.keyword.monitoringlong}}** 서비스를 선택하고 클러스터가 사용 가능한 지역을 선택하십시오. 이 튜토리얼의 경우 **미국 남부**입니다. 그런 다음 **관리자** 역할을 선택하십시오.	

## 5단계: Kubernetes 클러스터에 샘플 앱 배치
{: #step5}

Kubernetes 클러스터에 샘플 앱을 배치하고 실행하십시오. 다음 튜토리얼의 단계를 완료하여 샘플 앱을 배치하십시오. [학습 1: Kubernetes 클러스터에 단일 인스턴스 앱 배치](/docs/containers/cs_tutorials_apps.html#cs_apps_tutorial_lesson1).

이 앱은 Hello World Node.js 앱입니다.

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

이 샘플 앱의 경우, 사용자가 브라우저에서 앱을 테스트할 때 앱이 stdout에 다음 메지시를 씁니다. `Sample app is listening on port 8080.`


## 6단계: Grafana 실행 및 메트릭 도메인 설정
{: #step6}

브라우저에서 Grafana를 실행하고 클러스터 메트릭을 볼 수 있는 {{site.data.keyword.monitoringshort}} 도메인을 설정하십시오.

클러스터의 메트릭을 분석하려면 클러스터가 작성된 클라우드 퍼블릭 지역에서 Grafana에 액세스해야 합니다. 자세한 정보는 [웹 브라우저에서 Grafana 대시보드로 이동](/docs/services/cloud-monitoring/grafana/navigating_grafana.html#launch_grafana_from_browser)을 참조하십시오.

1. 브라우저에서 Grafana를 실행하십시오. 

    클러스터를 작성한 지역에 대한 {{site.data.keyword.monitoringshort}} 서비스 URL을 입력하십시오. 
    
    지역별 URL을 가져오려면 [모니터링 서비스에 대한 URL](/docs/services/cloud-monitoring/monitoring_ov.html#region)을 참조하십시오.

    예를 들어, 미국 남부 지역의 경우 [https://metrics.ng.bluemix.net/](https://metrics.ng.bluemix.net/)을 실행하십시오.

2. 클러스터 메트릭을 볼 수 있는 {{site.data.keyword.monitoringshort} 도메인을 설정하십시오.

    Grafana에서 사용자 ID를 선택하십시오. 그런 다음, 올바른 계정인지 확인하고 도메인을 선택하십시오.

    연관된 CF 영역이 있는 클러스터가 메트릭을 영역 메트릭 도메인으로 전달합니다. `Domain = space`, 조직 및 클러스터와 연관된 영역을 선택하십시오.

    계정 레벨에서 작성된 클러스터가 메트릭을 계정 메트릭 도메인으로 전달합니다. `Domain = account`을 선택하십시오.

## 7단계: Grafana에서 클러스터 모니터링
{: #step7}

{{site.data.keyword.containershort}}에서는 클러스터 메트릭을 모니터링하는 데 사용할 수 있는 Grafana 대시보드를 제공합니다. 

샘플 대시보드를 열려면 다음 단계를 완료하십시오.

1. 측면 메뉴 표시줄 토글 ![Grafana 측면 메뉴 표시줄](images/grafana_settings.gif "Grafana 측면 메뉴 표시줄")을 선택하십시오.
2. **대시보드**를 선택하십시오.
3. **열기**를 클릭하십시오.
4. **ClusterMonitoringDashboard_v1**을 선택하십시오.

샘플 대시보드가 열립니다. 

![Grafana 샘플 대시보드](images/cluster_grafana_sample_dashboard.png "Grafana 샘플 대시보드")



## 다음 단계
{: #next_steps}

메트릭에 대한 경보를 정의하십시오. 자세한 정보는 [경보 구성](/docs/services/cloud-monitoring/config_alerts_ov.html#config_alerts_ov)을 참조하십시오.
