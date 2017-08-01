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


# Alerts API를 사용한 알림 관련 작업
{: #notifications}

Alerts API를 사용하여 알림을 작성하거나, 삭제하거나, 업데이트하거나, 알림의 세부사항을 표시하거나, {{site.data.keyword.Bluemix_notm}} 영역에 정의된 알림을 나열하십시오.
{:shortdesc}

## 알림 템플리트 작성
{: #template}

알림은 JSON 파일입니다.  

사용자는 개수에 제한 없이 알림 템플리트를 작성할 수 있으며, 그 후 조직에서 이를 재사용하여 해당 유형의 알림을 작성할 수 있습니다.  

다음 유형의 알림을 정의할 수 있습니다. 

* 이메일: 유효한 이메일 주소로 이메일을 발송하려면 유형이 *이메일*인 알림을 정의하십시오.  
* 웹훅: https 엔드포인트에 대해서만 유형이 *웹훅*인 알림을 정의하십시오. 다른 사람이 사용자의 엔드포인트를 호출하려 시도할 가능성을 줄이기 위해 엔드포인트에 매개변수를 추가하십시오. 
* PagerDuty: 사용자의 PagerDuty 인시던트 관리 시스템으로 메트릭에 대한 경보 데이터를 전송하려면 유형이 *PagerDuty*인 알림을 정의하십시오.  

예를 들면, 다음 표에는 알림 템플리트의 예가 나열되어 있습니다. 

<table>
  <caption></caption>
  <tr>
    <th>유형</th>
	<th>템플리트</th>
	<th>샘플</th>
  </tr>
  <tr>
    <td>이메일</td>
	<td>
	```
	{
	"name": "Template_Name",
	"type": "Email",
	"description" : "Description",
	"detail": "EmailAddress"
	}
	```
	{: codeblock}
	</td>
	<td>
	```
	{
	"name": "my-email",
	"type": "Email",
	"description" : "Send email notification when there is an infrastructure problem.",
	"detail": "xxx@yyy.com"
	}
	```
	{: screen}
	</td>
  </tr>
  <tr>
    <td>웹훅</td>
	<td>
	```
	{
	"name": "Template_Name",
	"type": "Webhook",
	"description" : "Description",
	"detail": "Endpoint"
	}
	```
	{: codeblock}
	</td>
	<td>
	```
	{
	"name": "my-webhook",
	"type": "Webhook",
	"description" : "Fire a webhook when there is an infrastructure problem..",
	"detail": "https://myendpoint.bluemix.net?key=abcd1234"
	}
	```
	{: screen}
	</td>
  </tr>
  <tr>
    <td>PagerDuty</td>
	<td>
	```
	{
	"name": "Template_Name",
	"type": "PagerDuty",
	"description" : "Description",
	"detail": "Pagerduty_APIkey"
	}
	```
	{: codeblock}
	</td>
	<td>
	```
	{
	"name": "my-pagerduty",
	"type": "PagerDuty",
	"description" : "Fire a PagerDuty alert when there is an infrastructure problem..",
	"detail": "abcd1234"
	}
	```
	{: screen}
	</td>
  </tr>
</table>

여기서, 

* *Template_Name*은 알림 템플리트의 이름을 정의합니다. 
* *Description*은 이 유형의 알림을 사용하는 경우를 설명합니다. 
* *EmailAddress*는 알림 수신자의 이메일 주소를 정의합니다. 
* *Endpoint*는 POST를 수행해야 하는 URL을 정의합니다. 이 URL은 POST 요청을 수신할 수 있는 URL입니다.  
* *Pagerduty_APIkey*는 고유 API 키를 정의합니다. 이 API 키는 PagerDuty 계정 관리자 또는 소유자에 의해 생성됩니다. 


알림 템플리트를 작성하려면 다음 단계를 완료하십시오. 

1. {{site.data.keyword.monitoringshort}} 서비스 리소스를 저장할 디렉토리(예: *cloud-monitoring*)를 작성하십시오. 

    예를 들면, Ubuntu 시스템에서는 다음 명령을 실행하십시오. 
	
	```
	mkdir ~/cloud-monitoring
	```
	{: codeblock}
	
2. 알림 템플리트를 저장할 디렉토리(예: *notification-templates*)를 작성하십시오. 

    예를 들면, Ubuntu 시스템에서는 다음 명령을 실행하십시오. 
	
	```
	mkdir ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
	이 디렉토리로 이동하십시오. 
	
	```
	cd ~/cloud-monitoring/notification-templates
	```
	{: codeblock}
	
3. 시스템의 로컬 디렉토리에서 알림 템플리트를 작성하십시오. 

    예를 들면, 이메일 알림 템플리트의 경우에는 vi 편집기 등을 사용하여 **email-template.json**을 작성하십시오.  
	
	```
	{
    "name": "email_template",
    "type": "Email",
    "description" : "Send email to manager of department X when ....",
    "detail": "xxx@yyy"
    }
	```
	{: codeblock}
	


## 알림 작성
{: #create}

알림을 작성하려면 다음 단계를 완료하십시오. 

1. 알림 파일을 작성하십시오. 

    1. 알림 템플리트를 사용하여 파일을 작성하십시오. 자세한 정보는 [알림 템플리트 작성](#template)을 참조하십시오. 
	
	2. 파일을 업데이트하십시오. 
	
	    * 필드 *name*에 고유 이름을 입력하십시오. 이 필드의 값은 나중에 알림의 세부사항을 업데이트하거나, 삭제하거나 표시하는 데 사용됩니다. 
		* 설명을 입력하십시오. 
		* 유효한 이메일 주소, URL 엔드포인트 또는 PagerDuty 키를 입력하십시오. 
		
	3. 파일을 저장하십시오. 예를 들면, {{site.data.keyword.Bluemix_notm}} 영역에서 실행되는 조회에 대해 정의하는 알림에 *s-*와 같은 접두부를 사용하십시오. 
	
	    **팁:** {{site.data.keyword.monitoringshort_notm}} 서비스의 리소스를 그룹화하려면 디렉토리 *~/cloud-monitoring/notifications/*에서 알림 파일을 작성하십시오.  
	
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
	
5. 알림을 작성하려면 다음 cURL 명령을 실행하십시오.

    ```
	curl -XPOST -d @Notification_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: codeblock}
	
	여기서,
	
	* Notification_File은 알림을 정의하는 JSON 파일입니다.
	
	* *X-Auth-User-Token*은 {{site.data.keyword.Bluemix_notm}} UAA 토큰, IAM 토큰 또는 API 키를 전달하는 매개변수입니다.
	
	* *Auth_Type*은 토큰 또는 API 키의 유형을 정의하는 접두부입니다. 다음 목록에는 올바른 값이 간략하게 설명되어 있습니다: *apikey*, *iam*, *uaa*. 여기서,

        * *apikey*는 토큰이 API 키임을 식별합니다.
		* *iam*은 지정된 토큰이 IAM 생성 토큰임을 식별합니다.
		* *uaa*는 지정된 토큰이 UAA 생성 토큰임을 식별합니다.
	
	* *X-Auth-Scope-Id*는 영역 GUID를 전달하는 매개변수입니다. 이 GUID에는 영역임을 식별하기 위해 접두부 *s-*가 지정되어야 합니다. 이 헤더는 UAA 토큰 인증을 사용하는 경우 필요합니다. IAM 인증 토큰을 사용하는 경우 이 헤더는 선택사항이며, 토큰에 바인드된 도메인 정보가 사용됩니다.
	
	* Token은 UAA 토큰, IAM 토큰 또는 API 키입니다.
	
	* Space는 영역의 GUID입니다. 이는 UAA 토큰을 사용하는 경우에만 필요합니다.
	
    예를 들면 다음과 같습니다.
	
	```
	curl -XPOST -d @s-email-dep-A.json --header "X-Auth-User-Token:iam ${Token}" https://metrics.{DomainName}/v1/alert/notification
	```
	{: screen}

## 알림 삭제
{:#delete}

알림을 삭제하려면 다음 단계를 완료하십시오.

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
	
4. 알림을 삭제하려면 다음 cURL 명령을 실행하십시오.

    ```
	curl -XDELETE --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification/Notification_Name
	```
	{: codeblock}
	
	여기서,
	
	* *Auth_Type*은 토큰 또는 API 키의 유형을 정의하는 접두부입니다. 다음 목록에는 올바른 값이 간략하게 설명되어 있습니다: *apikey*, *iam*, *uaa*. 여기서,

        * *apikey*는 토큰이 API 키임을 식별합니다.
		* *iam*은 지정된 토큰이 IAM 생성 토큰임을 식별합니다.
		* *uaa*는 지정된 토큰이 UAA 생성 토큰임을 식별합니다.
	
	* *X-Auth-User-Token*은 {{site.data.keyword.Bluemix_notm}} UAA 토큰, IAM 토큰 또는 API 키를 전달하는 매개변수입니다. 토큰 또는 API 키에는 다음 값 중 하나로 접두부가 지정되어 있어야 합니다: *apikey*, *iam* or *uaa*. 여기서,

        * *apikey*는 토큰이 API 키임을 식별합니다.
		* *iam*은 지정된 토큰이 IAM 생성 토큰임을 식별합니다.
		* *uaa*는 지정된 토큰이 UAA 생성 토큰임을 식별합니다.
	
	* Token은 UAA 토큰, IAM 토큰 또는 API 키입니다.
	
	* Space는 영역의 GUID입니다. 이는 UAA 토큰을 사용하는 경우에만 필요합니다.
	
 * Notification_Name은 알림의 이름, 즉 알림의 특성을 나열했을 때 필드 *name*의 값입니다.
	


## 모든 알림 나열
{: #list}

모든 알림을 나열하려면 다음 단계를 완료하십시오.

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
	
4. 모든 알림을 나열하려면 다음 cURL 명령을 실행하십시오.

    ```
	curl -XGET --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notifications
	```
	{: codeblock}
	
	여기서,
	
	* *Auth_Type*은 토큰 또는 API 키의 유형을 정의하는 접두부입니다. 다음 목록에는 올바른 값이 간략하게 설명되어 있습니다: *apikey*, *iam*, *uaa*. 여기서,

        * *apikey*는 토큰이 API 키임을 식별합니다.
		* *iam*은 지정된 토큰이 IAM 생성 토큰임을 식별합니다.
		* *uaa*는 지정된 토큰이 UAA 생성 토큰임을 식별합니다.
	
	* *X-Auth-User-Token*은 {{site.data.keyword.Bluemix_notm}} UAA 토큰, IAM 토큰 또는 API 키를 전달하는 매개변수입니다. 토큰 또는 API 키에는 다음 값 중 하나로 접두부가 지정되어 있어야 합니다: *apikey*, *iam* or *uaa*. 여기서,

        * *apikey*는 토큰이 API 키임을 식별합니다.
		* *iam*은 지정된 토큰이 IAM 생성 토큰임을 식별합니다.
		* *uaa*는 지정된 토큰이 UAA 생성 토큰임을 식별합니다.
	
	* Token은 UAA 토큰, IAM 토큰 또는 API 키입니다.
	
	* Space는 영역의 GUID입니다. 이는 UAA 토큰을 사용하는 경우에만 필요합니다.


			

## 알림의 세부사항 표시
{: #show}


알림에 대한 정보를 표시하려면 다음 단계를 완료하십시오.

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
	
4. 알림의 세부사항을 표시하려면 다음 cURL 명령을 실행하십시오.

    ```
	curl -XGET --header "X-Auth-User-Token:uaa ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification/Notification_Name
	```
	{: codeblock}
	
	여기서,
	
	* *X-Auth-User-Token*은 {{site.data.keyword.Bluemix_notm}} UAA 토큰, IAM 토큰 또는 API 키를 전달하는 매개변수입니다. 토큰 또는 API 키에는 다음 값 중 하나로 접두부가 지정되어 있어야 합니다: *apikey*, *iam* or *uaa*. 여기서,

        * *apikey*는 토큰이 API 키임을 식별합니다.
		* *iam*은 지정된 토큰이 IAM 생성 토큰임을 식별합니다.
		* *uaa*는 지정된 토큰이 UAA 생성 토큰임을 식별합니다.
	
	* Token은 UAA 토큰, IAM 토큰 또는 API 키입니다.
	
	* Space는 영역의 GUID입니다. 이는 UAA 토큰을 사용하는 경우에만 필요합니다.
	
 * Notification_Name은 알림의 이름, 즉 알림의 특성을 나열했을 때 필드 *name*의 값입니다.
	

		

			
## 알림 테스트
{: #test}	
			
알림을 테스트하려면 다음 단계를 완료하십시오.

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
	
4. 알림을 테스트하려면 다음 cURL 명령을 실행하십시오.

    ```
	curl -XPOST --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification/test/Notification_Name
	```
	{: codeblock}
	
	여기서,
	
	* *Auth_Type*은 토큰 또는 API 키의 유형을 정의하는 접두부입니다. 다음 목록에는 올바른 값이 간략하게 설명되어 있습니다: *apikey*, *iam*, *uaa*. 여기서,

        * *apikey*는 토큰이 API 키임을 식별합니다.
		* *iam*은 지정된 토큰이 IAM 생성 토큰임을 식별합니다.
		* *uaa*는 지정된 토큰이 UAA 생성 토큰임을 식별합니다.
	
	* *X-Auth-User-Token*은 {{site.data.keyword.Bluemix_notm}} UAA 토큰, IAM 토큰 또는 API 키를 전달하는 매개변수입니다. 토큰 또는 API 키에는 다음 값 중 하나로 접두부가 지정되어 있어야 합니다: *apikey*, *iam* or *uaa*. 여기서,

        * *apikey*는 토큰이 API 키임을 식별합니다.
		* *iam*은 지정된 토큰이 IAM 생성 토큰임을 식별합니다.
		* *uaa*는 지정된 토큰이 UAA 생성 토큰임을 식별합니다.
	
	* Token은 UAA 토큰, IAM 토큰 또는 API 키입니다.
	
	* Space는 영역의 GUID입니다. 이는 UAA 토큰을 사용하는 경우에만 필요합니다.
	
 * Notification_Name은 알림의 이름, 즉 알림의 특성을 나열했을 때 필드 *name*의 값입니다.
	


## 알림 업데이트
{: #update}

알림을 업데이트하려면 다음 단계를 완료하십시오.

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
	
4. 알림을 업데이트하려면 다음 cURL 명령을 실행하십시오.

    ```
	curl -XPUT -d @Notification_File --header "X-Auth-User-Token:Auth_Type ${Token}" --header "X-Auth-Scope-Id: ${Space}" https://metrics.ng.bluemix.net/v1/alert/notification
	```
	{: codeblock}
	
	여기서,
	
	* Notification_File은 알림을 정의하는 JSON 파일입니다.
	
	* *Auth_Type*은 토큰 또는 API 키의 유형을 정의하는 접두부입니다. 다음 목록에는 올바른 값이 간략하게 설명되어 있습니다: *apikey*, *iam*, *uaa*. 여기서,

        * *apikey*는 토큰이 API 키임을 식별합니다.
		* *iam*은 지정된 토큰이 IAM 생성 토큰임을 식별합니다.
		* *uaa*는 지정된 토큰이 UAA 생성 토큰임을 식별합니다.
	
	* *X-Auth-User-Token*은 Bluemix UAA 토큰, IAM 토큰 또는 API 키를 전달하는 매개변수입니다. 토큰 또는 API 키에는 다음 값 중 하나로 접두부가 지정되어 있어야 합니다: *apikey*, *iam* or *uaa*. 여기서,

        * *apikey*는 토큰이 API 키임을 식별합니다.
		* *iam*은 지정된 토큰이 IAM 생성 토큰임을 식별합니다.
		* *uaa*는 지정된 토큰이 UAA 생성 토큰임을 식별합니다.
	
	* Token은 UAA 토큰, IAM 토큰 또는 API 키입니다.
	
	* Space는 영역의 GUID입니다. 이는 UAA 토큰을 사용하는 경우에만 필요합니다.


			

