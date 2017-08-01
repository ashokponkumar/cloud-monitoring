---

copyright:
  years: 2017
lastupdated: "2017-07-10"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 플랜 변경
{: #change_plan}

서비스 대시보드에서, 또는 `cf update-service` 명령을 실행하여 {{site.data.keyword.Bluemix}}의 {{site.data.keyword.monitoringshort}} 서비스 플랜을 변경할 수 있습니다. 플랜은 언제든지 업그레이드하거나 다운그레이드할 수 있습니다.
{:shortdesc}

## UI를 통한 서비스 플랜 변경
{: #change_plan_ui}

서비스 대시보드에서 {{site.data.keyword.Bluemix_notm}}의 서비스 플랜을 변경하려면 다음 단계를 완료하십시오. 

1. {{site.data.keyword.Bluemix_notm}}에 로그인한 후 {{site.data.keyword.Bluemix_notm}} 대시보드에서 {{site.data.keyword.monitoringshort}} 서비스를 클릭하십시오.  
    
2. {{site.data.keyword.Bluemix_notm}} UI에서 **플랜** 탭을 선택하십시오. 

    현재 플랜에 대한 정보가 표시됩니다. 
	
3. 플랜을 업그레이드하거나 다운그레이드할 새 플랜을 선택하십시오.  

4. **저장**을 클릭하십시오. 



## CLI를 통한 서비스 플랜 변경
{: #change_plan_cli}

CLI를 통해 {{site.data.keyword.Bluemix_notm}}의 서비스 플랜을 변경하려면 다음 단계를 완료하십시오. 

1. 1. {{site.data.keyword.Bluemix_notm}} 지역, 조직 및 영역에 로그인하십시오. 다음 명령을 실행하십시오. 

    ```
    cf login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}
	
2. `cf services` 명령을 사용하여 현재 플랜을 확인하고, 서비스 목록으로부터 영역에서 사용 가능한 {{site.data.keyword.loganalysisshort}} 서비스 이름을 가져오십시오.  

    필드 **name**의 값이 플랜을 변경하기 위해 사용해야 하는 값입니다.  

    예를 들면 다음과 같습니다. 
	
	```
	$ cf services
	Getting services in org MyOrg / space dev as xxx@yyy.com...
	OK
	name            service      plan   bound apps   last operation
	Monitoring-0c   Monitoring   premium             create succeeded
    ```
	{: screen}
    
3. 플랜을 업그레이드하거나 다운그레이드하십시오. `cf update-service` 명령을 실행하십시오. 
    
	```
	cf update-service service_name [-p new_plan]
	```
	{: codeblock}
	
	여기서,  
	
	* *service_name*은 서비스의 이름입니다. 해당 값을 가져오기 위해 `cf services` 명령을 실행할 수 있습니다. 
	* *new_plan*은 플랜의 이름입니다. 
	
	다음 표에는 다양한 플랜과 해당 지원되는 값이 나열되어 있습니다. 
	
	<table>
	  <caption>표 1. 플랜 목록</caption>
	  <tr>
	    <th>플랜</th>
		<th>기능</th>
	    <th>이름</th>
	  </tr>
	  <tr>
	    <td>라이트</td>
	    <td>무료 모니터링 기능입니다. </td>
		<td>lite</td>
	  </tr>
	  <tr>
	    <td>프리미엄</td>
	    <td>프리미엄 모니터링 기능입니다. </td>
		<td>premium</td>
	  </tr>
	</table>
	
	예를 들어, 플랜을 *라이트* 플랜으로 다운그레이드하려면 다음 명령을 실행하십시오. 
	
	```
	cf update-service "Monitoring-0c" -p lite
    Updating service instance Monitoring-0c as xxx@yyy.com...
    OK
	```
	{: screen}

4. 새 플랜으로 변경되었는지 확인하십시오. `cf services` 명령을 실행하십시오. 

    ```
	cf services
    Getting services in org MyOrg / space dev as xxx@yyy.com...
    OK

    name              service       plan   bound apps   last operation
    Monitoring-0c     Monitoring    lite                create succeeded
	```
	{: screen}






