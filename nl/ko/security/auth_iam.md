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


# Bluemix IAM 모델을 사용하여 인증
{: #auth_iam}

{{site.data.keyword.Bluemix}} IAM 모델을 사용하여 {{site.data.keyword.monitoringshort}} 서비스에 저장된 메트릭에 액세스하는 데 사용할 수 있는 인증 토큰을 가져오거나 API 키를 가져오십시오. 토큰에는 만기 시간이 있습니다. API 키는 만료되지 않습니다.
{:shortdesc}


## Bluemix CLI를 사용하여 IAM 토큰 가져오기 
{: #iam_token_cli}

{{site.data.keyword.Bluemix_notm}} CLI를 사용하여 인증 토큰을 가져오려면 다음 단계를 완료하십시오. 

1. {{site.data.keyword.Bluemix_notm}} CLI를 설치하십시오. 

   자세한 정보는 [{{site.data.keyword.Bluemix_notm}} CLI 설치](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa)를 참조하십시오. 
   
   CLI가 설치되어 있는 경우에는 다음 단계를 진행하십시오. 
    
2. {{site.data.keyword.Bluemix_notm}} 지역, 조직 및 영역에 로그인하십시오. 다음 명령을 실행하십시오. 

    예를 들어, 미국 남부 지역의 경우에는 다음 명령을 실행하십시오. 
	
    ```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    지시사항을 따르십시오. {{site.data.keyword.Bluemix_notm}} 신임 정보를 입력하고 조직 및 영역을 선택하십시오. 
	
3. `bx iam oauth-tokens` 명령을 실행하여 IAM 토큰을 가져오십시오. 

    ```
	bx iam oauth-tokens
	```
	{: codeblock}
	
	출력은 해당 영역 및 조직에 사용자 ID를 인증하는 데 사용해야 하는 IAM 토큰을 리턴합니다.

**참고:** 토큰을 사용할 때는 API 호출에 전달하는 토큰의 값에서 *Bearer*를 제거하십시오.
		
		
## Bluemix CLI를 사용하여 IAM API 키 생성
{: #iam_apikey_cli}

{{site.data.keyword.Bluemix_notm}} CLI를 사용하여 IAM API 키를 생성하려면 다음 단계를 완료하십시오.

1. {{site.data.keyword.Bluemix_notm}} CLI를 설치하십시오.

   자세한 정보는 [{{site.data.keyword.Bluemix_notm}} CLI 설치](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa)를 참조하십시오.

   CLI가 설치되어 있는 경우에는 다음 단계를 진행하십시오.
	
2. `bx login` {{site.data.keyword.Bluemix_notm}} CLI 명령을 사용하여 {{site.data.keyword.Bluemix_notm}}에 로그인하십시오.

    예를 들어, 미국 남부 지역의 경우에는 다음 명령을 실행하십시오.
	
    ```
    bx login -a https://api.ng.bluemix.net
    ```
    {: codeblock}

    지시사항을 따르십시오. {{site.data.keyword.Bluemix_notm}} 신임 정보를 입력하고 조직 및 영역을 선택하십시오.

3. `bx iam api-key-create` 명령을 실행하여 IAM API 키를 작성하십시오.

    ```
    bx iam api-key-create NAME [-d DESCRIPTION][-f, --file FILE]
	```
	{: codeblock} 
	
	여기서,
	
	* NAME은 작성되는 API 키의 이름입니다.
	* DESCRIPTION은 API 키를 설명하는 데 사용되는 텍스트입니다.
	* FILE은 키가 저장되는 파일의 이름입니다.
	
    예를 들어, *MyKey*라는 API 키를 작성하려는 경우에는 다음 명령을 실행하십시오.
	
	```
	bx iam api-key-create MyKey -d "This is my API key for service X" 
	```
	{: screen}
	
## Bluemix UI를 사용하여 IAM API 키 생성
{: #iam_apikey_ui}

{{site.data.keyword.Bluemix_notm}} UI를 통해 IAM API 키를 생성하려면 다음 단계를 완료하십시오.

1. {{site.data.keyword.Bluemix_notm}} 콘솔에 로그인하십시오.

2. 콘솔 메뉴 표시줄에서 **관리 > 보안 > Bluemix API 키**를 클릭하십시오.

3. **API 키 작성**을 클릭하십시오.

4. API 키의 이름 및 설명을 입력하십시오. 그 후 **API 키 작성**을 클릭하십시오.

5. API 키를 저장하십시오. **API 키 다운로드**를 클릭하십시오.

    API 키를 표시하려면 **표시**를 클릭하십시오.

**참고:** API 키는 작성 시에만 표시하거나 다운로드할 수 있습니다. API 키를 유실한 경우에는 새 API 키를 작성해야 합니다.


	

	
## Bluemix UI를 사용하여 API 키 취소
{: #revoke_ui}
	
{{site.data.keyword.Bluemix_notm}} UI를 통해 IAM API 키를 취소하려면 다음 단계를 완료하십시오.

1. {{site.data.keyword.Bluemix_notm}} 콘솔에 로그인하십시오.

2. 콘솔 메뉴 표시줄에서 **관리 > 보안 > Bluemix API 키**를 클릭하십시오.

3. **조치**를 클릭한 후 **키 삭제**를 클릭하십시오.





	

	
	
	
	
	
	
