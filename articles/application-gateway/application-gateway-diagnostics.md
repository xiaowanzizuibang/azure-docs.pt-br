---
title: Integridade de back-end e logs de diagnóstico
titleSuffix: Azure Application Gateway
description: Saiba como habilitar e gerenciar logs de acesso e de desempenho do Gateway de Aplicativo Azure
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 11/22/2019
ms.author: victorh
ms.openlocfilehash: b458537c7cf8a254cd188c565ab1925afa202369
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81312661"
---
# <a name="back-end-health-and-diagnostic-logs-for-application-gateway"></a>Integridade de back-end e logs de diagnóstico para o gateway de aplicativo

Você pode monitorar Aplicativo Azure recursos de gateway das seguintes maneiras:

* [Integridade do back-end](#back-end-health): o Gateway de Aplicativo fornece a capacidade de monitorar a integridade dos servidores nos pools de back-end por meio do portal do Azure e do PowerShell. Também é possível encontrar a integridade dos pools de back-end por meio dos logs de diagnóstico de desempenho.

* [Logs](#diagnostic-logging): os logs permitem que o desempenho, o acesso e outros dados sejam salvos ou consumidos de um recurso para fins de monitoramento.

* [Métricas](application-gateway-metrics.md): o gateway de aplicativo tem várias métricas que ajudam a verificar se o sistema está sendo executado conforme o esperado.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="back-end-health"></a>Integridade do back-end

O Gateway de Aplicativo fornece a capacidade de monitorar a integridade de membros individuais dos pools de back-end por meio do portal, do PowerShell e da CLI (interface de linha de comando). Também é possível encontrar um resumo de integridade agregado dos pools de back-end por meio dos logs de diagnóstico de desempenho.

O relatório de integridade do back-end reflete o resultado da investigação de integridade do Gateway de Aplicativo nas instâncias de back-end. Quando a investigação é bem-sucedida e o back-end pode receber tráfego, ele é considerado íntegro. Caso contrário, ele é considerado não íntegro.

> [!IMPORTANT]
> Se houver um NSG (grupo de segurança de rede) em uma sub-rede de gateway de aplicativo, abra os intervalos de porta 65503-65534 para SKUs v1 e 65200-65535 para SKUs v2 na sub-rede do gateway de aplicativo para o tráfego de entrada. Esse intervalo de porta é necessário para a comunicação da infraestrutura do Azure. Elas são protegidas (bloqueadas) por certificados do Azure. Sem os certificados apropriados, as entidades externas, incluindo os clientes desses gateways, não poderão iniciar nenhuma alteração nesses pontos de extremidade.


### <a name="view-back-end-health-through-the-portal"></a>Exibir a integridade do back-end por meio do portal

No portal, a integridade do back-end é fornecida automaticamente. Em um gateway de aplicativo existente, selecione **monitoramento** > **integridade de back-end**.

Cada membro no pool de back-end é listado nesta página (seja uma NIC, um IP ou um FQDN). São mostrados o nome do pool de back-end, a porta, as configurações de HTTP do back-end e o status de integridade. Os valores válidos para o status de integridade são **íntegros**, não **íntegros**e **desconhecidos**.

> [!NOTE]
> Se o status **Desconhecido** de integridade do back-end for exibido, verifique se o acesso ao back-end não está bloqueado por uma regra do NSG, uma UDR (rota definida pelo usuário) ou um DNS personalizado na rede virtual.

![Integridade do back-end][10]

### <a name="view-back-end-health-through-powershell"></a>Exibir a integridade do back-end por meio do PowerShell

O seguinte código do PowerShell mostra como exibir a integridade do back-end usando o cmdlet `Get-AzApplicationGatewayBackendHealth`:

```powershell
Get-AzApplicationGatewayBackendHealth -Name ApplicationGateway1 -ResourceGroupName Contoso
```

### <a name="view-back-end-health-through-azure-cli"></a>Exibir a integridade do back-end por meio da CLI do Azure

```azurecli
az network application-gateway show-backend-health --resource-group AdatumAppGatewayRG --name AdatumAppGateway
```

### <a name="results"></a>Resultados

O seguinte snippet mostra um exemplo da resposta:

```json
{
"BackendAddressPool": {
    "Id": "/subscriptions/00000000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/appGatewayBackendPool"
},
"BackendHttpSettingsCollection": [
    {
    "BackendHttpSettings": {
        "Id": "/00000000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings"
    },
    "Servers": [
        {
        "Address": "hostname.westus.cloudapp.azure.com",
        "Health": "Healthy"
        },
        {
        "Address": "hostname.westus.cloudapp.azure.com",
        "Health": "Healthy"
        }
    ]
    }
]
}
```

## <a name="diagnostic-logs"></a><a name="diagnostic-logging"></a>Logs de diagnóstico

Você pode usar tipos diferentes de logs no Azure para gerenciar e solucionar problemas de gateways de aplicativo. Você pode acessar alguns desses logs por meio do portal. Todos os logs podem ser extraídos de um Armazenamento de blobs do Azure e exibidos em diferentes ferramentas, como [logs do Azure Monitor](../azure-monitor/insights/azure-networking-analytics.md), Excel e Power BI. Saiba mais sobre os tipos diferentes de logs na lista a seguir:

* **Log de atividades**: você pode usar os [logs de atividades do Azure](../monitoring-and-diagnostics/insights-debugging-with-events.md) (anteriormente conhecidos como logs operacionais e logs de auditoria) para exibir todas as operações que estão sendo enviadas à sua assinatura do Azure, bem como seu status. As entradas do log de atividades são coletadas por padrão e podem ser exibidas no portal do Azure.
* **Log de acesso**: você pode usar esse log para exibir padrões de acesso do gateway de aplicativo e analisar informações importantes. Isso inclui o IP do chamador, a URL solicitada, a latência de resposta, o código de retorno e os bytes de entrada e saída. Um log de acesso é coletado a cada 60 segundos. Esse log contém um registro por instância do Gateway de Aplicativo. A instância do Gateway de Aplicativo é identificada pela propriedade instanceId.
* **Log de desempenho**: você pode usar esse log para exibir o desempenho das instâncias do Gateway de Aplicativo. Esse log captura informações de desempenho de cada instância, incluindo o total de solicitações atendidas, a vazão de dados em bytes, o total de solicitações atendidas, a contagem de solicitações com falha e a contagem de instâncias de back-end íntegras ou não íntegras. Um log de desempenho é coletado a cada 60 segundos. O log de desempenho está disponível apenas para a SKU v1. Para a SKU v2, use [métricas](application-gateway-metrics.md) para dados de desempenho.
* **Logs de firewall**: use esse log para exibir as solicitações registradas por meio do modo de detecção ou prevenção de um gateway de aplicativo configurado com o firewall do aplicativo Web. Os logs de firewall são coletados a cada 60 segundos. 

> [!NOTE]
> Os logs estão disponíveis apenas para os recursos implantados no modelo de implantação do Azure Resource Manager. Você não pode usar logs para recursos do modelo de implantação clássico. Para obter um melhor entendimento dos dois modelos, consulte o artigo [Noções básicas sobre a implantação do Resource Manager e a implantação clássica](../azure-resource-manager/management/deployment-models.md).

Você tem três opções para armazenar os logs:

* **Conta de armazenamento**: as contas de armazenamento são mais adequadas para os logs quando eles são armazenados por mais tempo e examinados quando necessário.
* **Hubs de eventos**: os hubs de eventos são uma ótima opção para integração com outras ferramentas de Siem (gerenciamento de eventos e informações de segurança) para obter alertas sobre seus recursos.
* **Logs de Azure monitor**: os logs de Azure monitor são mais bem usados para o monitoramento geral em tempo real de seu aplicativo ou a análise de tendências.

### <a name="enable-logging-through-powershell"></a>Habilitar o log por meio do PowerShell

O log de atividade é habilitado automaticamente para todos os recursos do Resource Manager. Você deve habilitar o log de acesso e de desempenho para começar a coletar os dados disponíveis por meio desses logs. Para habilitar o log, use as seguintes etapas:

1. Anote a ID do recurso da conta de armazenamento, na qual os dados de log são armazenados. Esse valor tem o formato /subscriptions/\<subscriptionId\>/resourceGroups/\<grupo de recursos name\>/providers/Microsoft.Storage/storageAccounts/\<nome da conta de armazenamento\>. Use qualquer conta de armazenamento em sua assinatura. Use o portal do Azure para encontrar essas informações.

    ![Portal: ID do recurso da conta de armazenamento](./media/application-gateway-diagnostics/diagnostics1.png)

2. Anote a ID do Recurso do gateway de aplicativo para o qual o log está habilitado. Esse valor tem o formato /subscriptions/\<subscriptionId\>/resourceGroups/\<grupo de recursos name\>/providers/Microsoft.Network/applicationGateways/\<nome do gateway de aplicativo\>. Use o portal para encontrar essas informações.

    ![Portal: ID do recurso do gateway de aplicativo](./media/application-gateway-diagnostics/diagnostics2.png)

3. Habilite o log de diagnóstico usando o seguinte cmdlet do PowerShell:

    ```powershell
    Set-AzDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true     
    ```

> [!TIP]
>Os logs de atividades não exigem uma conta de armazenamento separada. O uso do armazenamento para logs de acesso e de desempenho gera encargos de serviço.

### <a name="enable-logging-through-the-azure-portal"></a>Habilitar o log por meio do portal do Azure

1. No portal do Azure, localize o recurso e selecione **configurações de diagnóstico**.

   Para o Gateway de Aplicativo, três logs estão disponíveis:

   * Log de acesso
   * Log de desempenho
   * Log de firewall

2. Para começar a coletar dados, selecione **Ativar diagnóstico**.

   ![Ativando o diagnóstico][1]

3. A página **Configurações de diagnóstico** fornece as configurações dos logs de diagnóstico. Neste exemplo, o Log Analytics armazena os logs. Você também pode usar os hubs de eventos e uma conta de armazenamento para salvar os logs de diagnóstico.

   ![Iniciando o processo de configuração][2]

5. Digite um nome para as configurações, confirme as configurações e selecione **salvar**.

### <a name="activity-log"></a>Log de atividades

O Azure gera o log de atividades por padrão. Os logs são preservados por 90 dias no armazenamento de logs de eventos do Azure. Saiba mais sobre esses logs lendo o artigo [Exibir eventos e o log de atividades](../monitoring-and-diagnostics/insights-debugging-with-events.md).

### <a name="access-log"></a>Log de acesso

O log de acesso é gerado apenas se você o habilitou em cada instância do Gateway de Aplicativo, conforme detalhado nas etapas anteriores. Os dados são armazenados na conta de armazenamento especificada quando o log foi habilitado. Cada acesso do gateway de aplicativo é registrado no formato JSON, conforme mostrado no exemplo a seguir para V1:

|Valor  |Descrição  |
|---------|---------|
|instanceId     | Instância do Gateway de Aplicativo que atendeu à solicitação.        |
|clientIP     | IP de origem da solicitação.        |
|clientPort     | Porta de origem da solicitação.       |
|httpMethod     | Método HTTP usado pela solicitação.       |
|requestUri     | URI da solicitação recebida.        |
|RequestQuery     | **Server-Routed**: instância do pool de back-end que recebeu a solicitação.</br>**X-AzureApplicationGateway-LOG-ID**: ID de Correlação usada para a solicitação. Ela pode ser usada para solucionar problemas de tráfego nos servidores back-end. </br>**SERVER-STATUS**: código de resposta HTTP que o Gateway de Aplicativo recebeu do back-end.       |
|UserAgent     | Agente do usuário do cabeçalho da solicitação HTTP.        |
|httpStatus     | Código de status HTTP retornado ao cliente do Gateway de Aplicativo.       |
|httpVersion     | Versão HTTP da solicitação.        |
|receivedBytes     | Tamanho do pacote recebido, em bytes.        |
|sentBytes| Tamanho do pacote enviado, em bytes.|
|timeTaken| Duração (em milissegundos) necessária para que uma solicitação seja processada e sua resposta seja enviada. Isso é calculado como o intervalo a partir da hora em que o Gateway de Aplicativo recebe o primeiro byte de uma solicitação HTTP até a hora em que a operação de envio de resposta é concluída. É importante observar que o campo Time-Taken geralmente inclui a hora em que os pacotes de solicitação e resposta são transmitidos pela rede. |
|sslEnabled| Se a comunicação com os pools de back-end usavam TLS/SSL. Os valores válidos são ativado e desativado.|
|host| O nome do host com o qual a solicitação foi enviada para o servidor de back-end. Se o nome de host de back-end estiver sendo substituído, este deverá refletir isso.|
|originalHost| O nome do host com o qual a solicitação foi recebida pelo gateway de aplicativo do cliente.|
```json
{
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/PEERINGTEST/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
    "operationName": "ApplicationGatewayAccess",
    "time": "2017-04-26T19:27:38Z",
    "category": "ApplicationGatewayAccessLog",
    "properties": {
        "instanceId": "ApplicationGatewayRole_IN_0",
        "clientIP": "191.96.249.97",
        "clientPort": 46886,
        "httpMethod": "GET",
        "requestUri": "/phpmyadmin/scripts/setup.php",
        "requestQuery": "X-AzureApplicationGateway-CACHE-HIT=0&SERVER-ROUTED=10.4.0.4&X-AzureApplicationGateway-LOG-ID=874f1f0f-6807-41c9-b7bc-f3cfa74aa0b1&SERVER-STATUS=404",
        "userAgent": "-",
        "httpStatus": 404,
        "httpVersion": "HTTP/1.0",
        "receivedBytes": 65,
        "sentBytes": 553,
        "timeTaken": 205,
        "sslEnabled": "off",
        "host": "www.contoso.com",
        "originalHost": "www.contoso.com"
    }
}
```
Para o gateway de aplicativo e o WAF v2, os logs mostram um pouco mais de informações:

|Valor  |Descrição  |
|---------|---------|
|instanceId     | Instância do Gateway de Aplicativo que atendeu à solicitação.        |
|clientIP     | IP de origem da solicitação.        |
|clientPort     | Porta de origem da solicitação.       |
|httpMethod     | Método HTTP usado pela solicitação.       |
|requestUri     | URI da solicitação recebida.        |
|UserAgent     | Agente do usuário do cabeçalho da solicitação HTTP.        |
|httpStatus     | Código de status HTTP retornado ao cliente do Gateway de Aplicativo.       |
|httpVersion     | Versão HTTP da solicitação.        |
|receivedBytes     | Tamanho do pacote recebido, em bytes.        |
|sentBytes| Tamanho do pacote enviado, em bytes.|
|timeTaken| Período de tempo (em **segundos**) necessário para que uma solicitação seja processada e sua resposta seja enviada. Isso é calculado como o intervalo a partir da hora em que o Gateway de Aplicativo recebe o primeiro byte de uma solicitação HTTP até a hora em que a operação de envio de resposta é concluída. É importante observar que o campo Time-Taken geralmente inclui a hora em que os pacotes de solicitação e resposta são transmitidos pela rede. |
|sslEnabled| Se a comunicação com os pools de back-end usava o TLS. Os valores válidos são ativado e desativado.|
|sslCipher| Conjunto de codificação que está sendo usado para comunicação TLS (se o TLS estiver habilitado).|
|sslProtocol| Protocolo SSL/TLS que está sendo usado (se o TLS estiver habilitado).|
|serverRouted| O servidor back-end para o qual o gateway de aplicativo roteia a solicitação.|
|serverStatus| Código de status HTTP do servidor de back-end.|
|serverResponseLatency| Latência da resposta do servidor de back-end.|
|host| Endereço listado no cabeçalho do host da solicitação.|
```json
{
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/PEERINGTEST/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
    "operationName": "ApplicationGatewayAccess",
    "time": "2017-04-26T19:27:38Z",
    "category": "ApplicationGatewayAccessLog",
    "properties": {
        "instanceId": "appgw_1",
        "clientIP": "191.96.249.97",
        "clientPort": 46886,
        "httpMethod": "GET",
        "requestUri": "/phpmyadmin/scripts/setup.php",
        "userAgent": "-",
        "httpStatus": 404,
        "httpVersion": "HTTP/1.0",
        "receivedBytes": 65,
        "sentBytes": 553,
        "timeTaken": 205,
        "sslEnabled": "off",
        "sslCipher": "",
        "sslProtocol": "",
        "serverRouted": "104.41.114.59:80",
        "serverStatus": "200",
        "serverResponseLatency": "0.023",
        "host": "www.contoso.com",
    }
}
```

### <a name="performance-log"></a>Log de desempenho

O log de desempenho é gerado apenas se você o habilitou em cada instância do Gateway de Aplicativo, conforme detalhado nas etapas anteriores. Os dados são armazenados na conta de armazenamento especificada quando o log foi habilitado. Os dados do log de desempenho são gerados em intervalos de 1 minuto. Ele está disponível apenas para a SKU v1. Para a SKU v2, use [métricas](application-gateway-metrics.md) para dados de desempenho. Os seguintes dados são registrados em log:


|Valor  |Descrição  |
|---------|---------|
|instanceId     |  Instância do Gateway de Aplicativo para a qual os dados de desempenho estão sendo gerados. Para um gateway de aplicativo de várias instâncias, há uma linha por instância.        |
|healthyHostCount     | Número de hosts íntegros no pool de back-end.        |
|unHealthyHostCount     | Número de hosts não íntegros no pool de back-end.        |
|requestCount     | Número de solicitações atendidas.        |
|latency | Latência média (em milissegundos) de solicitações da instância para o back-end que atende às solicitações. |
|failedRequestCount| Número de solicitações com falha.|
|throughput| Vazão de dados média desde o último log, medida em bytes por segundo.|

```json
{
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
    "operationName": "ApplicationGatewayPerformance",
    "time": "2016-04-09T00:00:00Z",
    "category": "ApplicationGatewayPerformanceLog",
    "properties":
    {
        "instanceId":"ApplicationGatewayRole_IN_1",
        "healthyHostCount":"4",
        "unHealthyHostCount":"0",
        "requestCount":"185",
        "latency":"0",
        "failedRequestCount":"0",
        "throughput":"119427"
    }
}
```

> [!NOTE]
> A latência é calculada a partir da hora em que o primeiro byte da solicitação HTTP é recebido até a hora em que o último byte da resposta HTTP é enviado. É a soma do tempo de processamento do Gateway de Aplicativo e do custo de rede para o back-end, mais o tempo que o back-end leva para processar a solicitação.

### <a name="firewall-log"></a>Log de firewall

O log de firewall é gerado apenas se você o habilitou em cada gateway de aplicativo, conforme detalhado nas etapas anteriores. Esse log também exige a configuração de um firewall de aplicativo Web em um gateway de aplicativo. Os dados são armazenados na conta de armazenamento especificada quando o log foi habilitado. Os seguintes dados são registrados em log:


|Valor  |Descrição  |
|---------|---------|
|instanceId     | Instância do Gateway de Aplicativo para a qual os dados de firewall estão sendo gerados. Para um gateway de aplicativo de várias instâncias, há uma linha por instância.         |
|clientIp     |   IP de origem da solicitação.      |
|clientPort     |  Porta de origem da solicitação.       |
|requestUri     | URL da solicitação recebida.       |
|ruleSetType     | Tipo de conjunto de regras. O valor disponível é OWASP.        |
|ruleSetVersion     | Versão utilizada do conjunto de regras. Os valores disponíveis são 2.2.9 e 3.0.     |
|ruleId     | ID da Regra do evento de gatilho.        |
|mensagem     | Mensagem amigável para o evento de gatilho. Mais detalhes são fornecidos na seção de detalhes.        |
|ação     |  Ação executada na solicitação. Os valores disponíveis são MATCHED e blocked.      |
|site     | Site para o qual o log foi gerado. No momento, somente Global é listado porque as regras são globais.|
|detalhes     | Detalhes do evento de gatilho.        |
|details.message     | Descrição da regra.        |
|details.data     | Dados específicos encontrados na solicitação que corresponderam à regra.         |
|details.file     | Arquivo de configuração que continha a regra.        |
|details.line     | Número de linha no arquivo de configuração que disparou o evento.       |
|hostname   | Nome do host ou endereço IP do gateway de aplicativo.    |
|transactionId  | ID exclusiva para uma determinada transação que ajuda a agrupar várias violações de regra que ocorreram na mesma solicitação.   |

```json
{
  "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
  "operationName": "ApplicationGatewayFirewall",
  "time": "2017-03-20T15:52:09.1494499Z",
  "category": "ApplicationGatewayFirewallLog",
  "properties": {
    "instanceId": "ApplicationGatewayRole_IN_0",
    "clientIp": "104.210.252.3",
    "clientPort": "4835",
    "requestUri": "/?a=%3Cscript%3Ealert(%22Hello%22);%3C/script%3E",
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "ruleId": "941320",
    "message": "Possible XSS Attack Detected - HTML Tag Handler",
    "action": "Blocked",
    "site": "Global",
    "details": {
      "message": "Warning. Pattern match \"<(a|abbr|acronym|address|applet|area|audioscope|b|base|basefront|bdo|bgsound|big|blackface|blink|blockquote|body|bq|br|button|caption|center|cite|code|col|colgroup|comment|dd|del|dfn|dir|div|dl|dt|em|embed|fieldset|fn|font|form|frame|frameset|h1|head|h ...\" at ARGS:a.",
      "data": "Matched Data: <script> found within ARGS:a: <script>alert(\\x22hello\\x22);</script>",
      "file": "rules/REQUEST-941-APPLICATION-ATTACK-XSS.conf",
      "line": "865"
    }
    "hostname": "40.90.218.100",
    "transactionId": "AYAcUqAcAcAcAcAcASAcAcAc"
  }
}

```

### <a name="view-and-analyze-the-activity-log"></a>Exibir e analisar o log de atividades

Você pode exibir e analisar os dados do log de atividades usando um dos seguintes métodos:

* **Ferramentas do Azure**: recupere informações do log de atividades por meio do Azure PowerShell, da CLI do Azure, da API REST do Azure ou do portal do Azure. As instruções passo a passo para cada método são detalhadas no artigo [Activity operations with Resource Manager](../azure-resource-manager/management/view-activity-logs.md) (Operações de atividade com o Resource Manager).
* **Power BI**: se ainda não tiver uma conta do [Power BI](https://powerbi.microsoft.com/pricing), experimente uma gratuitamente. Usando os [aplicativos de modelo de Power bi](https://docs.microsoft.com/power-bi/service-template-apps-overview), você pode analisar seus dados.

### <a name="view-and-analyze-the-access-performance-and-firewall-logs"></a>Exibir e analisar os logs de acesso, de desempenho e de firewall

Os [logs do Azure Monitor](../azure-monitor/insights/azure-networking-analytics.md) podem coletar os arquivos de log de contadores e eventos de sua conta de Armazenamento de blobs. Ele inclui visualizações e funcionalidades de pesquisa avançadas para analisar os logs.

Você também pode se conectar à sua conta de armazenamento e recuperar as entradas de log JSON para logs de desempenho e acesso. Depois de baixar os arquivos JSON, você pode convertê-los em CSV e exibi-los no Excel, no Power BI ou em qualquer outra ferramenta de visualização de dados.

> [!TIP]
> Se estiver familiarizado com o Visual Studio e os conceitos básicos de alteração de valores de constantes e variáveis em C#, você poderá usar as [ferramentas de conversor de log](https://github.com/Azure-Samples/networking-dotnet-log-converter) disponíveis no GitHub.
>
>

#### <a name="analyzing-access-logs-through-goaccess"></a>Analisar logs de acesso por meio do GoAccess

Publicamos um modelo do Resource Manager que instala e executa o popular analisador de logs [GoAccess](https://goaccess.io/) para logs de acesso do Gateway de Aplicativo. O GoAccess fornece valiosas estatísticas de tráfego HTTP, tais como visitantes exclusivos, arquivos solicitados, hosts, sistemas operacionais, navegadores, códigos de status HTTP e muito mais. Para obter mais detalhes, consulte o [arquivo Leiame na pasta de modelo do Resource Manager no GitHub](https://aka.ms/appgwgoaccessreadme).

## <a name="next-steps"></a>Próximas etapas

* Visualize o contador e os logs de eventos com os [logs do Azure Monitor](../azure-monitor/insights/azure-networking-analytics.md).
* [Visualize o log de atividades do Azure com](https://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) a postagem de blog Power bi.
* Postagem no blog [View and analyze Azure Activity Logs in Power BI and more](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) (Exibir e analisar os Logs de Atividades do Azure no Power BI e muito mais).

[1]: ./media/application-gateway-diagnostics/figure1.png
[2]: ./media/application-gateway-diagnostics/figure2.png
[3]: ./media/application-gateway-diagnostics/figure3.png
[4]: ./media/application-gateway-diagnostics/figure4.png
[5]: ./media/application-gateway-diagnostics/figure5.png
[6]: ./media/application-gateway-diagnostics/figure6.png
[7]: ./media/application-gateway-diagnostics/figure7.png
[8]: ./media/application-gateway-diagnostics/figure8.png
[9]: ./media/application-gateway-diagnostics/figure9.png
[10]: ./media/application-gateway-diagnostics/figure10.png
