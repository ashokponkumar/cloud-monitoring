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



# 규칙의 히스토리 검색
{: #retrieve_history}


경보의 히스토리를 검색하려면 다음 단계를 완료하십시오. 

1. {{site.data.keyword.Bluemix_notm}} 지역, 조직 및 영역에 로그인하십시오. 다음 명령을 실행하십시오. 

    예를 들어, 미국 남부 지역에 로그인하려면 다음 명령을 실행하십시오. 
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    지시사항을 따르십시오. {{site.data.keyword.Bluemix_notm}} 신임 정보를 입력하고 조직 및 영역을 선택하십시오. 

2. 인증 토큰 또는 API 키를 가져오십시오. 

    * IAM 인증의 경우에는 [Bluemix CLI를 사용하여 IAM 토큰 가져오기](/docs/services/cloud-monitoring/security/auth_iam.html#iam_token_cli) 또는 [Bluemix CLI를 사용하여 IAM API 키 생성](/docs/services/cloud-monitoring/security/auth_iam.html#iam_apikey_cli)을 참조하십시오. 
	                                          
	* UAA 인증의 경우에는 [Bluemix CLI를 사용하여 UAA 토큰 가져오기](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_cli) 또는 [REST API를 사용하여 UAA 토큰 가져오기](/docs/services/cloud-monitoring/security/auth_uaa.html#uaa_api)를 참조하십시오. 

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
	
	그 후 변수 *Token*을 내보내십시오:
	
	```
	export Token="djn.._l_HWtlNTbxslBXs0qjBI9GqCnuQ"
	```
	{: screen}
	
	**참고:** 이 토큰은 *Bearer*를 제외합니다.
	
	
3. 영역 GUID를 가져오십시오. 이 GUID에는 영역임을 식별하기 위해 접두부 *s-*가 지정되어야 합니다.

    다음 명령을 실행하십시오.
	
	```
	bx iam space SpaceName --guid
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
	
	그 후 변수 *Space*를 내보내십시오:
	
	```
	export Space="s-667fadfc-jhtg-1234-9f0e-cf4123451095"
	```
	{: screen}
	
4. 경보의 히스토리를 가져오려면 다음 cURL 명령을 실행하십시오.

    ```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/history?rule=Rule_Name
	```
	{: codeblock}
	
	여기서,
	
	* *X-Auth-User-Token*은 {{site.data.keyword.Bluemix_notm}} UAA 토큰, IAM 토큰 또는 API 키를 전달하는 매개변수입니다.

 * *Auth_Type*은 토큰 또는 API 키의 유형을 정의하는 접두부입니다. 다음 목록에는 올바른 값이 간략하게 설명되어 있습니다: *apikey*, *iam*, *uaa*. 여기서,

        * *apikey*는 토큰이 API 키임을 식별합니다.
		* *iam*은 지정된 토큰이 IAM 생성 토큰임을 식별합니다.
		* *uaa*는 지정된 토큰이 UAA 생성 토큰임을 식별합니다.
	
	* *X-Auth-Scope-Id*는 영역 GUID를 전달하는 매개변수입니다. 이 GUID에는 영역임을 식별하기 위해 접두부 *s-*가 지정되어야 합니다. 이 헤더는 UAA 토큰 인증을 사용하는 경우 필요합니다. IAM 인증 토큰을 사용하는 경우 이 헤더는 선택사항이며, 토큰에 바인드된 도메인 정보가 사용됩니다.
	
	* Token은 UAA 또는 IAM 토큰이거나 API 키입니다.
	
	* Space는 영역의 GUID입니다. 이는 UAA 토큰을 사용하는 경우에만 필요합니다.
	
 * Rule_Name은 이 경보를 트리거하는 데 사용되는 규칙의 이름입니다. 이 값은 필드 *name*에 지정된 값입니다.
	

예를 들어, 규칙 `highNginxCPU`의 히스토리는 다음과 같습니다.

```
[
 {
 "rule": "highNginxCPU",
 "timestamp": "2017-04-17T22.23.21.000",
 "value": 99.5,
 "from_level": "OK",
 "to_level": "ERROR"
 },
 {
 "rule": "highNginxCPU",
 "timestamp": "2017-04-17T10.11.12.123",
 "value": 98.5,
 "from_level" : "OK",
 "to_level": "WARN"
 },
 {
 "rule": "highNginxCPU",
 "timestamp": "2017-04-17T10.12.14.000",
 "value": 97.0,
 "from_level" : "WARN",
 "to_level": "OK"
 }
]
```
{: screen}


