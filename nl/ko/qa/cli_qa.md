---

copyright:
  years: 2017

lastupdated: "2017-06-19"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Bluemix CLI 사용에 대해 자주 묻는 질문 및 응답
{: #cli_qa}

여기에는 {{site.data.keyword.Bluemix}} CLI를 {{site.data.keyword.monitoringshort}} 서비스와 함께 사용하는 데 대한 일반적인 질문의 답이 제공되어 있습니다.
{:shortdesc}

* [{{site.data.keyword.Bluemix_notm}} CLI 설치 방법](#install_bmx_cli)
* [계정의 GUID를 가져오는 방법](#account_guid)
* [조직의 GUID를 가져오는 방법](#org_guid)
* [영역의 GUID를 가져오는 방법](#space_guid)


## Bluemix CLI 설치 방법
{: #install_bmx_cli}

{{site.data.keyword.Bluemix_notm}} CLI를 설치하려면 다음 단계를 완료하십시오. 

1. CLI를 다운로드하십시오. 

    예를 들어, Ubuntu 시스템에 {{site.data.keyword.Bluemix_notm}} CLI 패키지를 설치하려는 경우에는 [{{site.data.keyword.Bluemix_notm}} CLI 패키지 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://clis.ng.bluemix.net/ui/home.html){: new_window}을 다운로드하십시오.  

2. 다음 명령을 사용하여 {{site.data.keyword.Bluemix_notm}} CLI 패키지의 압축을 푸십시오. 
    
    ```
    tar -xvf Bluemix_CLI_0.5.4_amd64.tar.gz
    ```
    {: codeblock}
    
3. *Bluemix_CLI* 디렉토리로 이동한 후 루트 권한으로 `./install_bluemix_cli` 명령을 실행하여 CF 플러그인을 설치하십시오. 루트 사용자로 이 명령을 실행하거나, sudo 명령을 사용하여 루트 권한을 확보하십시오. 예를 들면 다음과 같습니다. 
    
    ```
    cd Bluemix_CLI
    ```
    {: codeblock}
    
    ```
    sudo ./install_bluemix_cli
    ```
    {: codeblock}
    
4. CF 플러그인의 설치를 확인하십시오. 예를 들면, CF 플러그인의 버전을 확인하십시오. 다음 명령을 실행하십시오. 
    
    ```
    cf -v
    ```
    {: codeblock}
    
    출력은 다음과 같습니다. 
    
    ```
    cf version 6.25.0+787326d.2017-02-28
    ```
    {: screen}
    
5. {{site.data.keyword.Bluemix_notm}} CLI의 설치를 확인하십시오. 예를 들면, 플러그인의 버전을 확인하십시오. 다음 명령을 실행하십시오. 
    
    ```
    bx -version
    ```
    {: codeblock}
    
    출력은 다음과 같습니다. 
    
    ```
    bx version 0.5.4+ae22935-2017-05-18T03:55:55+00:00
    ```
    {: screen}
	
## 계정의 GUID를 가져오는 방법
{: #account_guid}
	
계정의 GUID를 가져오려면 다음 단계를 완료하십시오. 
	
1. {{site.data.keyword.Bluemix_notm}} 지역, 조직 및 영역에 로그인하십시오. 다음 명령을 실행하십시오. 

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    지시사항을 따르십시오. {{site.data.keyword.Bluemix_notm}} 신임 정보를 입력하고 조직 및 영역을 선택하십시오. 
	
2. `bx iam accounts` 명령을 실행하여 계정의 GUID를 가져오십시오. 

    ```
	bx iam accounts
	```
	{: codeblock} 
	
	예를 들어, 사용자 xxx@yyy.com의 모든 계정을 해당 GUID와 함께 검색하려면 다음 명령을 실행하십시오.
	
	```
	bx iam accounts
	Retrieving all accounts of xxx@yyy.com...
    OK
    Account GUID                       Name                               Type    State    Owner User ID   
    12345123451234512345123451234512   A Account                          TRIAL   ACTIVE   xxx@yyy.com   
    23456234562345622456234561234561   B Account                          TRIAL   ACTIVE   zzz@yyy.com   
	```
	{: screen}

	
## 조직의 GUID를 가져오는 방법
{: #org_guid}

조직의 GUID를 가져오려면 다음 단계를 완료하십시오.
	
1. {{site.data.keyword.Bluemix_notm}} 지역, 조직 및 영역에 로그인하십시오. 다음 명령을 실행하십시오.

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    지시사항을 따르십시오. {{site.data.keyword.Bluemix_notm}} 신임 정보를 입력하고 조직 및 영역을 선택하십시오.

2. `bx iam org` 명령을 실행하여 조직 GUID를 가져오십시오.

    ```
    bx iam org NAME --guid
    ```
    {: codeblock}
	
    여기서 NAME은 {{site.data.keyword.Bluemix_notm}} 조직의 이름입니다.
		
## 영역의 GUID를 가져오는 방법
{: #space_guid}
	
영역의 GUID를 가져오려면 다음 단계를 완료하십시오.
	
1. {{site.data.keyword.Bluemix_notm}} 지역, 조직 및 영역에 로그인하십시오. 다음 명령을 실행하십시오.

    ```
    bx login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    지시사항을 따르십시오. {{site.data.keyword.Bluemix_notm}} 신임 정보를 입력하고 조직 및 영역을 선택하십시오.

2. `bx iam space` 명령을 실행하여 영역 GUID를 가져오십시오.

    ```
    bx iam space NAME --guid
    ```
    {: codeblock}
	
    여기서 NAME은 {{site.data.keyword.Bluemix_notm}} 영역의 이름입니다.
	
    예를 들어, 영역 *dev*의 GUID를 가져오려면 다음 명령을 실행하십시오.
	
    ```
    bx iam space dev --guid
    e03afff1-3456-4af6-1234-59treg1b0abc
    ```
    {: screen}

	
