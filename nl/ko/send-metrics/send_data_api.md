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


# Monitoring 서비스에 데이터 전송
{: #send_data_api}

Metrics API를 사용하여 메트릭을 {{site.data.keyword.monitoringshort}} 서비스에 전송할 수 있습니다. 사용자는 {{site.data.keyword.Bluemix_notm}} 영역에 메트릭을 전송할 수 있습니다.
{:shortdesc}


{{site.data.keyword.Bluemix_notm}} Docker 컨테이너의 경우, 기본 시스템 메트릭은 자동으로 수집됩니다. Cloud Foundry 애플리케이션, 그리고 가상 머신(VM)에서 실행 중인 앱의 경우에는 [Metrics API](https://console.bluemix.net/apidocs/927-ibm-cloud-monitoring-rest-api?&language=node#introduction){: new_window}를 사용하여 앱에서 바로 메트릭을 전송해야 합니다.  



## UAA를 사용하여 영역에 메트릭 전송
{: #uaa}

{{site.data.keyword.Bluemix_notm}} 영역에 메트릭을 전송하려면 다음 단계를 완료하십시오. 

1. {{site.data.keyword.Bluemix_notm}} 지역, 조직 및 영역에 로그인하십시오. 다음 명령을 실행하십시오. 

    예를 들어, 미국 남부 지역에 로그인하려면 다음 명령을 실행하십시오. 
	
	```
    cf login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    지시사항을 따르십시오. {{site.data.keyword.Bluemix_notm}} 신임 정보를 입력하고 조직 및 영역을 선택하십시오. 

2. UAA 인증 토큰을 가져오십시오. 

    UAA 인증에 대한 자세한 정보는 [Bluemix CLI를 사용하여 UAA 토큰 가져오기](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) 또는 [REST API를 사용하여 UAA 토큰 가져오기](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api)를 참조하십시오. 

	예를 들어, UAA 토큰을 사용하려면 다음 명령을 실행하십시오. 

    ```
	cf oauth-token
	```
	{: codeblock}
	
	이 명령의 결과는 다음과 같습니다.
	
	```
	bearer eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8
	```
	{: screen}
	
	그 후 변수 *Token*을 내보내십시오. 예를 들면 다음과 같습니다. 
	
	```
	export Token="eyJhbGciOiJI....cGlxryWS8"
	```
	{: screen}
	
	**참고:** 이 토큰은 *Bearer*를 제외합니다.
		
3. 영역 GUID를 가져오십시오. 이 GUID에는 영역임을 식별하기 위해 접두부 *s-*가 지정되어야 합니다.

    다음 명령을 실행하십시오.
	
	```
	cf space SpaceName --guid
	```
	{: codeblock}
	
	여기서 *SpaceName*은 알림을 정의하는 영역의 이름입니다.
	
	예를 들어, 이름이 *dev*인 영역의 GUID를 가져오려면 다음 명령을 실행하십시오.
	
	```
	cf space dev --guid
	```
	{: screen}
	
	이 명령의 결과는 다음과 같습니다.
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	그 후 변수 *Space*를 내보내십시오. 예를 들면 다음과 같습니다. 
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. 메트릭을 전송하려면 다음 cURL 명령을 실행하십시오.

    ```
	curl -XPOST -d @Metric_File --header "X-Auth-User-Token:uaa ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics
	```
	{: codeblock}
	
	여기서,
	
	* Metrics_File은 {{site.data.keyword.monitoringshort}} 서비스로 전송되는 메트릭의 정의를 포함하는 JSON 파일을 나타냅니다.
	
	* *X-Auth-User-Token*은 {{site.data.keyword.Bluemix_notm}} UAA 토큰을 전달하는 매개변수입니다.
	
	* *Auth_Type*은 토큰의 유형을 정의하는 접두부입니다. *uaa*는 지정된 토큰이 UAA 생성 토큰임을 식별합니다.
	
	* *X-Auth-Scope-Id*는 영역 GUID를 전달하는 매개변수입니다. 이 GUID에는 영역임을 식별하기 위해 접두부 *s-*가 지정되어야 합니다.

    * Token은 UAA 토큰을 나타냅니다.
	
	* Space는 영역의 GUID를 나타냅니다.
	
	예를 들어, 2개의 메트릭 `myhost.cpu.idle` 및 `myapp.login.attempts`를 {{site.data.keyword.monitoringshort}} 서비스에 전송하려는 경우에는 다음 명령을 실행하십시오.
	
	```
	curl -XPOST @metric.json --header "X-Auth-User-Token:uaa ${Token}" https://metrics.ng.bluemix.net/v1/metrics
	```
	{: screen}
	
	샘플 파일 *metric.json*은 다음과 같이 정의되어 있습니다.

    ```
    [
      {
        "name" : "myhost.cpu.idle",
        "value" : 22.37
      },
      {
        "name": "myapp.login.retries",
        "value": 4
      }
    ]
	```
	{: screen}

 
## IAM 또는 API 키를 사용하여 영역에 메트릭 전송
{: #iam}

{{site.data.keyword.Bluemix_notm}} 영역에 메트릭을 전송하려면 다음 단계를 완료하십시오.

1. {{site.data.keyword.Bluemix_notm}} 지역, 조직 및 영역에 로그인하십시오. 다음 명령을 실행하십시오.

    예를 들어, 미국 남부 지역에 로그인하려면 다음 명령을 실행하십시오.
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    지시사항을 따르십시오. {{site.data.keyword.Bluemix_notm}} 신임 정보를 입력하고 조직 및 영역을 선택하십시오.

2. 인증 토큰 또는 API 키를 가져오십시오.

    IAM 인증에 대한 자세한 정보는 [Bluemix CLI를 사용하여 IAM 토큰 가져오기](/docs/services/cloud-monitoring/security/auth_iam.html#iam_cli) 또는 [Bluemix CLI를 사용하여 IAM API 키 생성](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli)을 참조하십시오.

	예를 들어, IAM 토큰을 사용하려면 다음 명령을 실행하십시오.

    ```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	이 명령의 결과는 다음과 같습니다.
	
	```
	IAM 토큰:  Bearer djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ
    UAA 토큰:  Bearer eyJhbGciOiJIUz..Ky6vagp3k_QcIcKJ-td83qXhO5Uze43KcplG6PzcGs8
	```
	{: screen}
	
	그 후 변수 *Token*을 내보내십시오. 예를 들면 다음과 같습니다. 
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**참고:** 이 토큰은 *Bearer*를 제외합니다.
		
3. 영역 GUID를 가져오십시오.

    영역 GUID를 가져오려는 경우에는 [영역의 GUID를 가져오는 방법](/docs/service/cloud-monitoring/qa/cli_qa.html#space_guid)을 참조하십시오. 이 GUID에는 영역임을 식별하기 위해 접두부 *s-*가 지정되어야 합니다.

    예를 들면, 다음 명령을 실행하십시오.
	
	```
	bx iam space NAME --guid
	```
	{: codeblock}
	
	여기서 *SpaceName*은 알림을 정의하는 영역의 이름입니다.
	
	예를 들어, 이름이 *dev*인 영역의 GUID를 가져오려면 다음 명령을 실행하십시오.
	
	```
	bx iam space dev --guid
	```
	{: screen}
	
	이 명령의 결과는 다음과 같습니다.
	
	```
	667fadfc-jhtg-1234-9f0e-cf4123451095
	```
	{: screen}
	
	그 후 변수 *Space*를 내보내십시오. 예를 들면 다음과 같습니다. 
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
5. 메트릭을 전송하려면 cURL 명령을 실행하십시오.

    ```
	curl -XPOST -d @Metric_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics
	```
	{: codeblock}
	
	여기서,
	
	* Metrics_File은 {{site.data.keyword.monitoringshort}} 서비스로 전송되는 메트릭의 정의를 포함하는 JSON 파일을 나타냅니다.
	
	* *X-Auth-User-Token*은 {{site.data.keyword.Bluemix_notm}} IAM 토큰 또는 API 키를 전달하는 매개변수입니다.
	
	* *Auth_Type*은 토큰 또는 API 키의 유형을 정의하는 접두부입니다. 다음 목록에는 올바른 값이 간략하게 설명되어 있습니다: *apikey*, *iam*, *uaa*. 여기서,

        * *apikey*는 토큰이 API 키임을 식별합니다.
		* *iam*은 지정된 토큰이 IAM 생성 토큰임을 식별합니다.
		* *X-Auth-Scope-Id*는 영역 GUID를 전달하는 매개변수입니다. 이 GUID에는 영역임을 식별하기 위해 접두부 *s-*가 지정되어야 합니다.

    * Token은 IAM 토큰 또는 API 키를 나타냅니다.
	
	   * Space는 영역의 GUID를 나타냅니다.

	
예를 들면, 다음 명령을 실행하여 두 메트릭 `myhost.cpu.idle` 및 `myapp.login.retries`를 {{site.data.keyword.monitoringshort}} 서비스의 영역에 전송할 수 있습니다.
	
```
curl -XPOST @metric.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/metrics
```
{: screen}
	
샘플 파일 *metric.json*은 다음과 같이 정의되어 있습니다.

```
[
  {
    "name" : "myhost.cpu.idle",
    "value" : 22.37
  },
  {
    "name": "myapp.login.retries",
    "value": 4
  }
]
```
{: screen}















