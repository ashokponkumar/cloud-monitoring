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


# Monitoring 서비스에서 데이터 검색
{: #retrieve_data_api}

[Metrics API](https://console.bluemix.net/apidocs/927-ibm-cloud-monitoring-rest-api?&language=node#introduction){: new_window}를 사용하여 {{site.data.keyword.monitoringshort}} 서비스에서 메트릭을 검색할 수 있습니다. 사용자는 {{site.data.keyword.Bluemix_notm}} 영역에서 메트릭을 검색할 수 있습니다.
{:shortdesc}


검색할 데이터 세트를 정의하려는 경우에는 다음 정보를 고려하십시오.  

* 데이터를 검색할 {{site.data.keyword.Bluemix_notm}} 영역을 설정해야 합니다. 

* {{site.data.keyword.monitoringshort}} 서비스를 사용한 작업에 필요한 토큰 또는 API 키를 제공해야 합니다.  

* 하나 이상의 메트릭에 대한 경로를 지정해야 합니다. 자세한 정보는 [메트릭 정의](#metrics)를 참조하십시오. 

* 선택적으로 사용자 정의 기간을 지정할 수 있습니다. 기본적으로, 기간을 지정하지 않는 경우 검색되는 데이터는 최근 24시간에 해당하는 데이터입니다. 자세한 정보는 [기간 구성](#time)을 참조하십시오. 

**참고:** 요청당 최대 5개의 대상을 검색할 수 있습니다. 


## 메트릭 정의
{: #metrics}

메트릭에 대한 데이터를 검색하려면 메트릭 트리의 루트에서 메트릭으로 메트릭의 경로를 지정해야 하며, 각 단계를 마침표(.)로 구분해야 합니다.

이 경로는 **Target** 매개변수에 지정됩니다. 요청당 최대 5개의 대상을 지정할 수 있습니다.   

선택적으로 메트릭에 함수를 적용할 수 있습니다. 지원되는 함수에 대한 자세한 정보는 [Graphite 0.9.15 함수 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://graphite.readthedocs.io/en/latest/functions.html)을 참조하십시오. 

경로를 정의하는 데 와일드카드를 사용할 수 있습니다. 와일드카드를 사용하면 하나의 경로에서 둘 이상의 메트릭을 식별할 수 있습니다.  

* **별표**: 별표(*)를 사용하여 0개 이상의 문자를 비교할 수 있습니다.  

    하나의 경로 요소에 둘 이상의 별표를 포함시킬 수 있으며, 예를 들면 `servers.sp*aeg*r.cpu.total.*`와 같습니다. 이 예는 패턴과 일치하는 모든 서버의 총 CPU에 대한 데이터를 리턴합니다. 

* **문자 목록 또는 범위**: 문자를 대괄호([...]) 안에 정의하여 경로 문자열 내에 하나의 문자 위치를 지정할 수 있습니다.  

    하이픈(-)으로 구분된 두 문자를 지정하여 경계 문자를 포함하는 범위를 설정할 수 있습니다. 이러한 두 문자 사이의 범위에 포함되는 모든 문자는 일치 항목으로 간주됩니다.  
	
	대괄호로 묶인 하나 이상의 문자 범위를 정의할 수 있습니다.  
	
	대괄호 세트 내의 문자를 하나의 범위로 읽을 수 없는 경우 해당 문자는 값 목록으로 취급됩니다.  
	
	대괄호 내의 시작 또는 끝 부분에 하이픈(-)을 삽입하여 이를 값 목록 내의 문자로 포함시킬 수 있습니다. 

* **값 목록**: 중괄호 내에 쉼표로 구분된 값을 설정하여 값 목록을 정의할 수 있습니다.  

    예를 들어, 경로 `servers.myserver.cpu.total.user` 및 `servers.myserver.cpu.total.system}`에서 메트릭을 다운로드하려는 경우에는 두 유형 모두에 대해 다음과 같은 하나의 경로를 설정할 수 있습니다: `servers.myserver.cpu.total.{user,system}`



## 기간 설정
{: #time}

선택적으로 상대 시간 또는 절대 시간을 사용하여 기간을 지정할 수 있습니다. 조회 매개변수 **From** 및 **Until**을 정의해야 합니다.   

* 기간의 시작 시간이 생략되는 경우의 기본값은 24시간 전입니다.  

* 기간의 종료 시간이 생략되는 경우의 기본값은 현재 시간인 *now*입니다. 

**상대 시간** 앞에는 항상 빼기 기호(-)가 오며 그 뒤에는 시간 단위가 옵니다.  

다음 표에는 사용할 수 있는 다양한 상대 시간 약어가 나열되어 있습니다. 


| 약어         | 설명|
|--------------|-------------|
| s            | 초          |
| min          | 분          |
| h            | 시간        |
| d            | 일          |
| w            | 주          |
| mon          | 월          |
| now          | 현재 시간   |


**절대 시간**을 정의하는 데는 `HH:MM_YYMMDD`, `YYYYMMDD`, `MM/DD/YY` 중 어느 형식이든 사용할 수 있습니다. 

| 약어         | 설명|
|--------------|-------------|
| YYYY         | 4자리 연도  |
| MM           | 2자리 월(01 = 1월 등) |
| DD           | 2자리 일(01 - 31) |
| hh           | 2자리 시간(00 - 23) |
| mm           | 2자리 분(00 - 59) |
| ss           | 2자리 초(00 - 59) |

**예**

다음 표는 *from* 및 *until* 매개변수를 설정하여 다양한 기간을 설정하는 방법에 대한 여러 예를 보여줍니다. 

<table>
  <caption>표 1. *from* 및 *until* 매개변수를 설정하여 다양한 기간을 설정하는 방법을 보여주는 여러 예</caption>
  <tr>
    <th>예</th>
	<th>설명</th>
  </tr>
  <tr>
    <td>&from=monday</td>
	<td>마지막 월요일부터 지금까지의 데이터를 포함하는 기간</td>
  </tr>
  <tr>
    <td>&from=20171101&until=20171130</td>
	<td>월(예: 11월)로 설정된 기간</td>
  </tr>
  <tr>
    <td>&from=02:00_20170701&until=14:00_20170701</td>
	<td>2017월 7월 1일의 02:00 - 14:00와 같이, 24시간 주기 내의 데이터를 표시합니다. </td>
  </tr>
  <tr>
    <td>&from=-8d&until=-7d</td>
	<td>주(예: 지난 주)의 동일한 요일을 표시합니다. </td>
  </tr>
  
</table>


## 영역 설정
{: #domain}

{{site.data.keyword.monitoringshort}} 서비스에 인증하는 데 {{site.data.keyword.Bluemix_notm}} UAA 인증 모델 또는 {{site.data.keyword.Bluemix_notm}} IAM 인증 모델을 사용하는 경우 헤더 필드 **X-Auth-Scope-Id**는 필수이며 영역 GUID로 설정되어야 합니다. 이 GUID에는 영역임을 식별하기 위해 접두부 *s-*가 지정되어야 합니다. 



## 인증 토큰 또는 API 키 설정
{: #security}
  
필드 **X-Auth-User-Token**을 설정하여 인증에 사용되는 UAA 토큰, IAM 토큰 또는 API 키를 정의해야 합니다.  

토큰 또는 API 키에는 다음 값 중 하나로 접두부가 지정되어 있어야 합니다: `apikey`, `iam`, `uaa` 

여기서,  

* *apikey*는 토큰이 API 키임을 식별합니다. 
* *iam*은 지정된 토큰이 IAM 생성 토큰임을 식별합니다. 
* *uaa*는 지정된 토큰이 UAA 생성 토큰임을 식별합니다. 

예를 들면, API 키에 대해 `X-Auth-User-Token: apikey SomeIAMGeneratedKey`와 같은 헤더를 설정할 수 있습니다. 


## UAA를 사용하여 영역에서 메트릭 검색
{: #uaa}

{{site.data.keyword.Bluemix_notm}} 영역에서 메트릭을 검색하려면 다음 단계를 완료하십시오. 

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
	export Token="eyJhbGciOiJI....cGFzc3dvcmQiLCJjZiIsInVhYSIsIm9wZW5pZCJdfQ.JaoaVudG4jqjeXz6q3JQL_SJJfoIFvY8m-rGlxryWS8"
	```
	{: screen}
	
	**참고:** 이 토큰은 *Bearer*를 제외합니다.
		
3. 영역 GUID를 가져오십시오. 이 GUID에는 영역임을 식별하기 위해 접두부 *s-*가 지정되어야 합니다.

    다음 명령을 실행하십시오.
	
	```
	cf space SpaceName --guid
	```
	{: codeblock}
	
	여기서 *SpaceName*은 알림을 정의하는, *s-*로 접두부 지정된 영역의 이름입니다.
	
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
	curl -XGET --header "X-Auth-User-Token:uaa ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics?from=Start_Time&until=End_Time&target=string
	```
	{: codeblock}

	여기서,
	
	* *X-Auth-User-Token*은 {{site.data.keyword.Bluemix_notm}} UAA 토큰을 전달하는 매개변수입니다.
	
	* *uaa*는 지정된 토큰이 UAA 생성 토큰임을 표시하는 접두부입니다.
	
	* *X-Auth-Scope-Id*는 영역 GUID를 전달하는 매개변수입니다. 이 GUID에는 영역임을 식별하기 위해 접두부 *s-*가 지정되어야 합니다. 이 헤더는 UAA 인증을 사용하는 경우에만 필요합니다. IAM 인증 토큰을 사용하는 경우 이 헤더는 선택사항이며, 토큰에 바인드된 도메인 정보가 사용됩니다.
	
	* *Token*은 UAA 토큰을 나타냅니다.
	
	* *Space*는 영역의 GUID를 나타냅니다. 이는 UAA 토큰을 사용하는 경우에만 필요합니다.
	
 * *Start_Time*은 요청의 시작을 설정하는 상대 또는 절대 기간을 정의합니다. 기본적으로 24시간 전(`-24h`)으로 설정됩니다. *End_Time*은 요청의 종료를 설정하기 위한 상대 또는 절대 기간을 정의합니다. 기본적으로 현재 시간(`now`)으로 설정됩니다. 자세한 정보는 [기간 설정](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#time)을 참조하십시오.
	
	* *Path*는 하나 또는 몇 가지 메트릭을 식별합니다. 선택적으로 각 메트릭에 함수를 적용할 수 있습니다. 경로는 하나의 경로에서 둘 이상의 메트릭을 식별할 수 있게 해 주는 와일드카드 또한 지원합니다. 자세한 정보는 [메트릭 정의](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#metrics)를 참조하십시오.
	
	
	
## IAM 또는 API 키를 사용하여 영역에서 메트릭 검색
{: #iam}

IAM 또는 API 키를 사용하여 {{site.data.keyword.Bluemix_notm}} 영역에서 메트릭을 검색하려면 다음 단계를 완료하십시오.

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
	
	여기서 *SpaceName*은 알림을 정의하는, *s-*로 접두부 지정된 영역의 이름입니다.
	
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
	
5. cURL 명령을 실행하여 메트릭을 검색하십시오.

   	```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/metrics?from=Start_Time&until=End_Time&target=string
	```
	{: codeblock}

	여기서,
	
	* *X-Auth-User-Token*은 {{site.data.keyword.Bluemix_notm}} IAM 토큰 또는 API 키를 전달하는 매개변수입니다.
	
	* *Auth_Type*은 토큰 또는 API 키의 유형을 정의하는 접두부입니다.
	 * *apikey*는 토큰이 API 키임을 식별합니다.
		* *iam*은 지정된 토큰이 IAM 생성 토큰임을 식별합니다.
		* *X-Auth-Scope-Id*는 영역 GUID를 전달하는 매개변수입니다. 이 GUID에는 영역임을 식별하기 위해 접두부 *s-*가 지정되어야 합니다. 이 헤더는 UAA 인증을 사용하는 경우에만 필요합니다. IAM 인증 토큰을 사용하는 경우 이 헤더는 선택사항이며, 토큰에 바인드된 도메인 정보가 사용됩니다.
	
	* *Token*은 UAA 토큰을 나타냅니다.
	
	* *Space*는 영역의 GUID를 나타냅니다. 이는 UAA 토큰을 사용하는 경우에만 필요합니다.
	
 * *Start_Time*은 요청의 시작을 설정하는 상대 또는 절대 기간을 정의합니다. 기본적으로 24시간 전(`-24h`)으로 설정됩니다. *End_Time*은 요청의 종료를 설정하기 위한 상대 또는 절대 기간을 정의합니다. 기본적으로 현재 시간(`now`)으로 설정됩니다. 자세한 정보는 [기간 설정](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#time)을 참조하십시오.
	
	* *Path*는 하나 또는 몇 가지 메트릭을 식별합니다. 선택적으로 각 메트릭에 함수를 적용할 수 있습니다. 경로는 하나의 경로에서 둘 이상의 메트릭을 식별할 수 있게 해 주는 와일드카드 또한 지원합니다. 자세한 정보는 [메트릭 정의](/docs/services/cloud-monitoring/retrieve-metrics/retrieve_data_api.html#metrics)를 참조하십시오.






