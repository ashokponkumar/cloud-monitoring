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


# Bluemix UAA 모델을 사용하여 인증
{: #auth_uaa}

{{site.data.keyword.Bluemix}} UAA 모델을 사용하여 {{site.data.keyword.Bluemix_notm}} 내의 영역에 대한, {{site.data.keyword.monitoringshort}} 서비스에 저장된 메트릭에 액세스하는 데 사용할 수 있는 인증 토큰을 가져오십시오. 인증 토큰은 {{site.data.keyword.Bluemix_notm}} CLI 또는 `Login` REST API를 사용하여 얻을 수 있습니다.
{:shortdesc}

UAA 인증 토큰을 사용하여 메트릭에 액세스하려면 다음 정보가 필요합니다. 

* RESTful API를 사용하여 {{site.data.keyword.monitoringshort}} 서비스에 액세스하는 데 필요한 UAA 토큰. 
* 영역의 GUID. 

		
## Bluemix CLI를 사용하여 UAA 토큰 가져오기
{: #uaa_cli}


인증 토큰을 가져오려면 다음 단계를 완료하십시오. 

1. {{site.data.keyword.Bluemix_notm}} CLI를 설치하십시오. 

   자세한 정보는 [{{site.data.keyword.Bluemix_notm}} CLI 설치](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa)를 참조하십시오. 
   
   CLI가 설치되어 있는 경우에는 다음 단계를 진행하십시오. 
    
2. {{site.data.keyword.Bluemix_notm}} 지역, 조직 및 영역에 로그인하십시오. 다음 명령을 실행하십시오. 

    ```
    cf login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    지시사항을 따르십시오. {{site.data.keyword.Bluemix_notm}} 신임 정보를 입력하고 조직 및 영역을 선택하십시오. 
	
3. `cf oauth-token` 명령을 실행하여 {{site.data.keyword.Bluemix_notm}} UAA 토큰을 가져오십시오. 

    ```
	cf oauth-token
	```
	{: codeblock}
	
	출력은 해당 영역 및 조직에 사용자 ID를 인증하는 데 사용해야 하는 UAA 토큰을 리턴합니다.

4. 인증 토큰을 얻은 조직의 영역에 대한 GUID를 가져오십시오.

   자세한 정보는 [영역의 GUID를 가져오는 방법](/docs/services/cloud-monitoring/qa/cli_qa.html#space_guid)을 참조하십시오.
	
5. 변수 TOKEN 및 SPACE를 내보내십시오.

    * *TOKEN*은 이전 단계에서 가져온, Bearer를 제외한 OAuth 토큰입니다.
	
	* *SPACE*은 이전 단계에서 가져온 영역의 GUID입니다.
	
	예를 들면 다음과 같습니다.
	
	```
	export TOKEN="eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8"
	export SPACE="667fb895-abcd-defg-aaaa-cf4587341095"
	```
	{: screen}
	
6. 다음 명령을 실행하여 {{site.data.keyword.monitoringshort}} 서비스에 대해 작업하는 데 필요한 UAA 토큰을 가져오십시오.

    ```
	curl -k -X GET  --header "X-Auth-Token: ${TOKEN}"  --header "X-Auth-Project-Id: ${SPACE}"  METRICS_ENDPOINT
    ```
    {: codeblock}	
	
	여기서,
	* SPACE는 서비스가 실행 중인 영역의 GUID입니다.
	* TOKEN은 bearer 접두부가 없는, 이전 단계에서 가져온 {{site.data.keyword.Bluemix_notm}} UAA 토큰입니다.
	* METRICS_ENDPOINT는 조직 및 영역이 사용 가능한 {{site.data.keyword.Bluemix_notm}} 지역의 메트릭 엔드포인트(https://metrics.ng.bluemix.net/token)입니다.

	
## API를 사용하여 UAA 토큰 가져오기
{: #uaa_api}

인증 토큰을 가져오려면 다음 cURL 명령을 실행하십시오.

```
curl -XPOST -d 'user=USERNAME&passwd=PASSWORD&space=SPACE_NAME&organization=ORG_NAME' METRICS_ENDPOINT
```
{: codeblock}

여기서,

* USERNAME은 {{site.data.keyword.monitoringshort}} 서비스에 대해 작업하는 데 필요한 인증 토큰을 가져올 {{site.data.keyword.Bluemix_notm}} ID입니다.
* PASSWORD는 {{site.data.keyword.Bluemix_notm}}에 로그인하는 데 필요한 사용자 ID의 비밀번호입니다.
* SPACE_NAME은 메트릭이 수집되는 영역의 이름입니다.
* ORG_NAME은 영역이 호스팅되고 있는 {{site.data.keyword.Bluemix_notm}} 내의 조직 이름입니다.
* METRICS_ENDPOINT는 조직 및 영역이 사용 가능한 {{site.data.keyword.Bluemix_notm}} 지역의 메트릭 엔드포인트(https://metrics.ng.bluemix.net/login)입니다.
 	
출력은 UAA 토큰을 포함하는 JSON 문서입니다. 필드 **access-token**에서 토큰 값을 가져오십시오.

예를 들면, 샘플 JSON 문서의 내용은 다음과 같습니다.

```
{
    "access_token": "eyJhbGc...",
    "logging_token": "xxxxxxxxxxxx",
    "org_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "space_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```
{: screen}

*logging_token*의 값이 {{site.data.keyword.monitoringshort}} 서비스에 대해 작업하는 데 필요한 UAA 토큰입니다.

**참고:**

* cURL을 사용하고 있지 않은 경우에는 헤더 **Content-Type: application/x-www-form-urlencoded**를 설정해야 합니다.
* 오류 코드 *BXNMS0122E: User credentials are invalid*가 표시되는 경우에는 올바른 {{site.data.keyword.IBM_notm}} ID를 사용하고 있는지 확인하십시오.


