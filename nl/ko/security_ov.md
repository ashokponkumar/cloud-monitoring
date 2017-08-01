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


# 보안
{: #security_ov}

다양한 인증 방법을 사용하여 {{site.data.keyword.monitoringshort}} 서비스에 메트릭을 전송할 수 있습니다. 메트릭 액세스, 수정 및 삭제 권한은 역할을 사용하여 제어됩니다.
{:shortdesc}

   
## 인증 모델
{: #auth}

{{site.data.keyword.Bluemix_notm}} 영역에 대해 {{site.data.keyword.monitoringshort}} 서비스에 저장된 메트릭에 액세스하려면 인증 토큰 또는 API 키가 필요합니다.  

{{site.data.keyword.monitoringshort}} 서비스는 다음 인증 모델을 지원합니다. 

* [{{site.data.keyword.Bluemix_notm}} UAA 인증](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_uaa)
* [{{site.data.keyword.Bluemix_notm}} IAM 인증](/docs/services/cloud-monitoring/security/auth_iam.html#auth_iam)

UAA 토큰 및 IAM 토큰은 일정 기간이 경과하면 만료됩니다. API 키는 만료되지 않습니다.
 

API 키가 보안 위험에 노출된 경우에는 이를 삭제하여 취소할 수 있습니다. 그 후에는 새 키를 다시 작성할 수 있습니다. 자세한 정보는 [Bluemix UI를 사용하여 API 키 취소](/docs/services/cloud-monitoring/security/auth_iam.html#revoke_ui)를 참조하십시오.  

IAM 인증 모델은 UI, CLI 또는 API 관리 기능을 제공합니다. UAA 토큰을 관리하는 데는 CLI만 사용할 수 있습니다. 

다음 표에는 각 도메인 유형에 대해 지원되는 인증 모델이 나열되어 있습니다. 

<table>
  <caption>표 1. 각 도메인에 대해 지원되는 인증 모델</caption>
  <tr>
    <th></th>
	<th align="right">계정</th>
    <th align="right">조직</th>
    <th align="right">영역</th>	
  </tr>
  <tr>
    <th align="left">UAA</th>
	<td align="center">아니오</td>
	<td align="center">예</td>
	<td align="center">예</td>
  </tr>
  <tr>
    <th align="left">IAM</th>
	<td align="center">예</td>
	<td align="center">예</td>
	<td align="center">예</td>
  </tr>
</table>

{{site.data.keyword.monitoringshort}} 서비스는 IAM 액세스 제어 기능을 사용하여 사용자가 도메인에 대해 수행할 수 있는 조치 또는 오퍼레이션을 제어합니다. 예를 들면, 사용자가 도메인에 대해 대시보드를 작성하고 전송하는 것은 허용하지만, 도메인으로부터 메트릭을 검색하는 것은 허용하지 않는 IAM 정책을 정의할 수 있습니다. 



## Bluemix UAA 역할
{: #bmx_roles}

다음 표에는 {{site.data.keyword.monitoringshort}} 서비스에 대해 작업하는 데 필요한 각 {{site.data.keyword.Bluemix_notm}} 역할의 권한이 나열되어 있습니다. 

<table>
  <caption>표 2. {{site.data.keyword.monitoringshort}} 서비스에 대해 작업하는 데 필요한 {{site.data.keyword.Bluemix_notm}} 역할 및 권한</caption>
  <tr>
    <th>역할</th>
	<th>도메인</th>
	<th>액세스 권한</th>
  </tr>
  <tr>
    <td>관리자</td>
	<td>조직 <br>영역</td>
	<td>모든 RESTful API</td>
  </tr>
  <tr>
    <td>개발자</td>
	<td>영역</td>
	<td>모든 RESTful API</td>
  </tr>
  <tr>
    <td>감사자</td>
	<td>조직 <br>영역</td>
	<td>메트릭 조회와 같은 읽기 전용 오퍼레이션을 수행하는 RESTful API 한정</td>
  </tr>
</table>


## Bluemix IAM 역할
{: #iam_roles}

다음 표에는 API, 서비스 조치 및 {{site.data.keyword.monitoringshort}} 서비스에서 사용하는 IAM 역할 간의 관계가 나열되어 있습니다. 

<table>
  <caption>표 3. API, 서비스 조치 및 IAM 역할 간의 관계</caption>
  <tr>
    <th>API</th>
	<th>조치</th>
	<th>IAM 역할</th>
	<th>설명</th>
  </tr>
  <tr>
    <td>POST /v1/metrics</td>
    <td>domain.write</td>
	<td>관리자, 편집자, 운영자</td>
	<td>도메인에 메트릭 전송</td>
  </tr>
  <tr>
    <td>GET /v1/metrics</td>
    <td>domain.render</td>
	<td>관리자, 편집자, 뷰어</td>
	<td>메트릭 검색/조회</td>
  </tr>
  <tr>
    <td>GET /v1/metrics/list</td>
    <td>domain.find</td>
	<td>관리자, 편집자</td>
	<td>도메인에서 메트릭 검색</td>
  </tr>
</table>

## 보안 토큰 또는 API 키 가져오기
{: #get_token}

{{site.data.keyword.Bluemix_notm}} UAA 모델을 사용하여 {{site.data.keyword.Bluemix_notm}} 내의 영역에 대한, {{site.data.keyword.monitoringshort}} 서비스에 저장된 데이터에 액세스하는 데 사용할 수 있는 인증 토큰을 가져오십시오. 인증 토큰은 {{site.data.keyword.Bluemix_notm}} CLI 또는 `Login` REST API를 사용하여 얻을 수 있습니다.
자세한 정보는 [{{site.data.keyword.Bluemix_notm}} CLI를 사용하여 UAA 토큰 가져오기](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_cli) 및 [API를 사용하여 UAA 토큰 가져오기](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_api)를 참조하십시오. 

{{site.data.keyword.Bluemix_notm}} IAM 모델을 사용하여 {{site.data.keyword.monitoringshort}} 서비스에 저장된 데이터에 액세스하는 데 사용할 수 있는 인증 토큰을 가져오거나 API 키를 가져오십시오. 토큰에는 만기 시간이 있습니다. API 키는 만료되지 않습니다.
자세한 정보는 [{{site.data.keyword.Bluemix_notm}} CLI를 사용하여 IAM 토큰 가져오기](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli), [{{site.data.keyword.Bluemix_notm}} CLI를 사용하여 IAM API 키 생성](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli) 또는 [{{site.data.keyword.Bluemix_notm}} UI를 사용하여 IAM API 키 생성](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_ui)을 참조하십시오. 



