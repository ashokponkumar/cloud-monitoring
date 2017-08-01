---

copyright:
  years: 2017

lastupdated: "2017-07-18"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Alerts API를 사용한 규칙 관련 작업
{: #rules}

Alerts API를 사용하여 규칙을 작성하거나, 삭제하거나, 업데이트하거나, 규칙의 세부사항을 표시하거나, {{site.data.keyword.Bluemix_notm}} 영역에 정의된 규칙을 나열하십시오.
{:shortdesc}


## 규칙 작성
{: #create}

규칙을 작성하려면 다음 단계를 완료하십시오. 

1. 올바른 JSON을 포함하는 규칙 파일을 작성하십시오. 이 파일을 저장하십시오.  

    예를 들어, 파일을 저장하려면 {{site.data.keyword.Bluemix_notm}} 영역에서 실행되는 조회에 대해 정의하는 규칙에 *s-*와 같은 접두부를 사용하십시오. 
	
	**팁:** 
	
	* 다양한 알림 방법을 사용하여 오류 또는 경고에 대해 알리는 조회에 대해 다양한 규칙을 정의하십시오. 규칙당 1개의 알림 방법만 설정할 수 있습니다.  
	* {{site.data.keyword.monitoringshort_notm}} 서비스의 리소스를 그룹화하려면 디렉토리 *~/cloud-monitoring/rules/*에서 규칙 파일을 작성하십시오.  

    규칙 파일에 다음 정보를 입력하십시오. 
	
	* *name*: 고유 이름을 입력하십시오. 이 필드의 값은 나중에 규칙의 세부사항을 업데이트하거나, 삭제하거나 표시하는 데 사용됩니다. 
	* *description*: 설명을 입력하십시오. 
	* *expression*: 모니터할 조회를 입력하십시오. 
	* *enabled*: 규칙을 사용으로 설정하려면 *true*로 설정하십시오. 
	* *from*: 임계값을 기반으로 데이터를 분석하는 데 사용되는 초기 시점입니다. 
	* *until*: 임계값을 기반으로 데이터를 분석하는 데 사용되는 최종 시점입니다. 
	* *comparison*: *below*와 같이 어떤 유형의 확인을 수행할지 식별하는 데 사용되는 비교 오퍼레이션입니다. 
	* *error_level*: 오류 경보를 트리거하기 위해 설정하는 임계값을 정의합니다. 
	* *warning_level*: 경고 경보를 트리거하기 위해 설정하는 임계값을 정의합니다. 
	* *frequency*: 데이터 확인 빈도를 정의합니다. 
	* *dashboard_url*: Grafana에서 조회에 대한 대시보드를 표시하는 URL을 정의합니다. 
	* *notifications*: 이 규칙에 설명된 경보가 트리거된 경우의 알림 방법입니다. 
	
	예를 들면 다음과 같습니다.  
	
	```
	{
    "name": "checkbytesin",
    "description": "MH check Bytes In per second",
    "expression": "movingAverage(messagehub.65qser11-8034-1234-5678-c82fb42wdfgh.1.kafka-java-console-sample-topic.BytesInPerSec.15MinuteRate,\"5min\")",
    "enabled": true,
    "from": "-5min",
    "until": "now",
    "comparison": "above",
    "comparison_scope": "last",
    "error_level" : 27,
    "warning_level" : 25,
    "frequency": "1min",
    "dashboard_url": "https://metrics.ng.bluemix.net",
    "notifications": [
      "emailXXX"
    ]
    }
    ```
	{: screen}
	
2. {{site.data.keyword.Bluemix_notm}} 지역, 조직 및 영역에 로그인하십시오. 다음 명령을 실행하십시오. 

    예를 들어, 미국 남부 지역에 로그인하려면 다음 명령을 실행하십시오. 
	
	```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    지시사항을 따르십시오. {{site.data.keyword.Bluemix_notm}} 신임 정보를 입력하고 조직 및 영역을 선택하십시오. 

3. 인증 토큰 또는 API 키를 가져오십시오. 

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
		
4. 영역 GUID를 가져오십시오. 이 GUID에는 영역임을 식별하기 위해 접두부 *s-*가 지정되어야 합니다.

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
	
5. 규칙을 작성하려면 다음 cURL 명령을 실행하십시오.

    ```
	curl -XPOST -d @Rule_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: codeblock}
	
	여기서,
	
	* Rule_File은 경보 규칙을 정의하는 JSON 파일입니다.
	
	* *X-Auth-User-Token*은 {{site.data.keyword.Bluemix_notm}} UAA 토큰, IAM 토큰 또는 API 키를 전달하는 매개변수입니다.

 * *Auth_Type*은 토큰 또는 API 키의 유형을 정의하는 접두부입니다. 다음 목록에는 올바른 값이 간략하게 설명되어 있습니다: *apikey*, *iam*, *uaa*. 여기서,

        * *apikey*는 토큰이 API 키임을 식별합니다.
		* *iam*은 지정된 토큰이 IAM 생성 토큰임을 식별합니다.
		* *uaa*는 지정된 토큰이 UAA 생성 토큰임을 식별합니다.
	
	* *X-Auth-Scope-Id*는 영역 GUID를 전달하는 매개변수입니다. 이 GUID에는 영역임을 식별하기 위해 접두부 *s-*가 지정되어야 합니다. 이 헤더는 UAA 토큰 인증을 사용하는 경우 필요합니다. IAM 인증 토큰을 사용하는 경우 이 헤더는 선택사항이며, 토큰에 바인드된 도메인 정보가 사용됩니다.
	
	* Token은 UAA 또는 IAM 토큰이거나 API 키입니다.
	
	* Space는 영역의 GUID입니다. 이는 UAA 토큰을 사용하는 경우에만 필요합니다.
	
    예를 들면 다음과 같습니다.
	
	```
	curl -XPOST -d @s-rule-1.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: screen}

## 규칙 삭제
{: #delete}

규칙을 삭제하려면 다음 단계를 완료하십시오.

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
	
4. 규칙을 삭제하려면 다음 cURL 명령을 실행하십시오.

    ```
	curl -XDELETE --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/rule/Rule_Name
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
	
 * Rule_Name은 필드 *name*에 지정된 규칙의 이름입니다.
	

	
## 모든 규칙 나열
{: #list}

모든 규칙을 나열하려면 다음 단계를 완료하십시오.

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
	
4. 영역 내의 모든 규칙을 나열하려면 다음 cURL 명령을 실행하십시오.

    ```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/rules
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
	

	
	

## 규칙의 세부사항 표시
{: show}

규칙의 세부사항을 표시하려면 다음 단계를 완료하십시오.

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
	
4. 규칙의 세부사항을 표시하려면 다음 cURL 명령을 실행하십시오.

    ```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/rule/Rule_Name
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
	
 * Rule_Name은 필드 *name*에 지정된 규칙의 이름입니다. 	

	
## 규칙 업데이트
{: #update}

규칙을 업데이트하려면 다음 단계를 완료하십시오.

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
	
4. 규칙을 업데이트하려면 다음 cURL 명령을 실행하십시오.

    ```
	curl -XPUT `-d @Rule_File` --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/rule
	```
	{: codeblock}
	
	여기서,
	
	* Rule_File은 경보 규칙을 정의하는 JSON 파일입니다.
	
	* *X-Auth-User-Token*은 {{site.data.keyword.Bluemix_notm}} UAA 토큰, IAM 토큰 또는 API 키를 전달하는 매개변수입니다.

 * *Auth_Type*은 토큰 또는 API 키의 유형을 정의하는 접두부입니다. 다음 목록에는 올바른 값이 간략하게 설명되어 있습니다: *apikey*, *iam*, *uaa*. 여기서,

        * *apikey*는 토큰이 API 키임을 식별합니다.
		* *iam*은 지정된 토큰이 IAM 생성 토큰임을 식별합니다.
		* *uaa*는 지정된 토큰이 UAA 생성 토큰임을 식별합니다.
	
	* *X-Auth-Scope-Id*는 영역 GUID를 전달하는 매개변수입니다. 이 GUID에는 영역임을 식별하기 위해 접두부 *s-*가 지정되어야 합니다. 이 헤더는 UAA 토큰 인증을 사용하는 경우 필요합니다. IAM 인증 토큰을 사용하는 경우 이 헤더는 선택사항이며, 토큰에 바인드된 도메인 정보가 사용됩니다.
	
	* Token은 UAA 또는 IAM 토큰이거나 API 키입니다.
	
	* Space는 영역의 GUID입니다. 이는 UAA 토큰을 사용하는 경우에만 필요합니다.

	
	
