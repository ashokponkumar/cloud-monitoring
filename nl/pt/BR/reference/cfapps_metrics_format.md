---

copyright:
  years: 2017, 2018

lastupdated: "2018-02-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Apps CF
{: #cfapps_metrics_format}

As consultas que você define no Grafana para monitorar um aplicativo Cloud Foundry que é executado no
{{site.data.keyword.Bluemix}} Public devem estar em conformidade com o formato a seguir:
{:shortdesc}

```
{Source}.{Cloud Type}.{Service Name}.{Region}.{CFapp Name}.{CFapp Index}.{Metric series}.[Functions]
```
{: codeblock}

em que [service fields] representa os campos específicos.

<table>
  <caption>Campos comuns em uma consulta</caption>
  <tr>
    <th>Campo</th>
	<th>Descrição</th>
	<th>Valor</th>
  </tr>
  <tr>
    <td>Fonte</td>
	<td>Este é um nome reservado. As métricas sob esta hierarquia são provenientes de aplicativos e serviços do
{{site.data.keyword.Bluemix_notm}}.</td>
	<td>*ibmcloud*</td>
  </tr>
  <tr>
    <td>Tipo de nuvem</td>
	<td>Indica o tipo do Cloud. </td>
	<td>Para a nuvem pública do {{site.data.keyword.Bluemix_notm}}, o valor é: *public*</td>
  </tr>
  <tr>
    <td>Nome do serviço</td>
	<td>Define o nome do serviço no {{site.data.keyword.Bluemix_notm}}.</td>
	<td>Para apps CF, o valor é: *cloud-foundry*</td>
  </tr>
  <tr>
    <td>Região</td>
	<td>Define a região em que a instância do app CF está em execução.</td>
	<td>Para a região Sul dos EUA, o valor é: *us-south* <br>Para a região do Reino Unido, o valor é: *eu-gb*  <br>Para Alemanha, o valor é: *eu-de* <br>Para Sydney, o valor é: *au-syd* </td>
  </tr>
  <tr>
    <td>Nome do CFapp</td>
	<td>O nome do aplicativo CF.</td>
	<td></td>
  </tr>
  <tr>
    <td>Índice do CFapp</td>
	  <td>O índice da instância do aplicativo CF.</td>
	  <td>Por exemplo: *0* </br>Se você tiver um app CF com uma instância, haverá apenas o índice 0.
Se você escalar o app CF, por exemplo, para 10 instâncias, você terá 0 a 9 como valores de índice.</td>
  </tr>
  <tr>
    <td>Índice do CFapp</td>
	<td>Valor fixo.</td>
	<td>*container*</td>
  </tr>
  <tr>
    <td>Série de métrica</td>
	<td>O tipo de métrica. Disco, memória e CPU são séries de métricas coletadas automaticamente.</td>
	<td>Por exemplo: *cpu-utilization* </td>
  </tr>
  <tr>
    <td>Funções</td>
    <td>Funções de consulta que podem ser selecionadas para visualizar uma métrica de contêiner no painel. </td>
    <td>[Funções
![(Ícone de link externo)](../../../icons/launch-glyph.svg "Ícone de link externo")](http://graphite.readthedocs.io/en/latest/functions.html){: new_window}</td>
   </tr>
</table>




