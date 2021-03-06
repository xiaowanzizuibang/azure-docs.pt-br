---
title: Monitorar o desempenho do aplicativo Web Java no Linux - Azure | Microsoft Docs
description: Monitoramento estendido do desempenho de aplicativo do seu site Java com o plug-in CollectD para Application Insights.
ms.topic: conceptual
ms.date: 03/14/2019
ms.openlocfilehash: 62a723dad7e9f6c2bfdabde159968d507d2d5d41
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81537518"
---
# <a name="collectd-linux-performance-metrics-in-application-insights"></a>collectd: métricas de desempenho do Linux no Application Insights


Para explorar as métricas de desempenho do sistema Linux no [Application Insights](../../azure-monitor/app/app-insights-overview.md), instale [collectd](https://collectd.org/) com seu plug-in do Application Insights. Essa solução de software livre reúne várias estatísticas de sistema e de rede.

Normalmente, você usará o collectd se já tiver [instrumentado seu serviço Web Java com o Application Insights][java]. Isso oferece a você mais dados para ajudá-lo a aprimorar o desempenho do aplicativo ou para diagnosticar problemas. 

## <a name="get-your-instrumentation-key"></a>Obter a chave de instrumentação
No [Portal do Microsoft Azure](https://portal.azure.com), abra o recurso [Application Insights](../../azure-monitor/app/app-insights-overview.md) onde você quer que os dados sejam exibidos. (Ou [crie um novo recurso](../../azure-monitor/app/create-new-resource.md ).)

Faça uma cópia da chave de instrumentação que identifica o recurso.

![Procure tudo, abra seu recurso e, no menu suspenso Essentials, selecione e copie a Chave de Instrumentação](./media/java-collectd/instrumentation-key-001.png)

## <a name="install-collectd-and-the-plug-in"></a>Instalar o plug-in e collectd
Em seus computadores com o servidor Linux:

1. Instale [collectd](https://collectd.org/) versão 5.4.0 ou posterior.
2. Baixe o [plug-in do gravador collectd do Application Insights](https://github.com/microsoft/ApplicationInsights-Java/tree/master/collectd/src/main/java/com/microsoft/applicationinsights/collectd/internal). Observe o número de versão.
3. Copie o plug-in JAR em `/usr/share/collectd/java`.
4. Edite `/etc/collectd/collectd.conf`:
   * Verifique se [o plug-in do Java](https://collectd.org/wiki/index.php/Plugin:Java) está habilitado.
   * Atualize o JVMArg para java.class.path de modo a incluir o JAR a seguir. Atualize o número de versão para corresponder àquela que você baixou:
   * `/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar`
   * Adicione esse snippet usando a chave de instrumentação do seu recurso:

```XML

     LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"
     <Plugin ApplicationInsightsWriter>
        InstrumentationKey "Your key"
     </Plugin>
```

Veja o exemplo de parte de um arquivo de configuração:

```XML

    ...
    # collectd plugins
    LoadPlugin cpu
    LoadPlugin disk
    LoadPlugin load
    ...

    # Enable Java Plugin
    LoadPlugin "java"

    # Configure Java Plugin
    <Plugin "java">
      JVMArg "-verbose:jni"
      JVMArg "-Djava.class.path=/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar:/usr/share/collectd/java/collectd-api.jar"

      # Enabling Application Insights plugin
      LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"

      # Configuring Application Insights plugin
      <Plugin ApplicationInsightsWriter>
        InstrumentationKey "12345678-1234-1234-1234-123456781234"
      </Plugin>

      # Other plugin configurations ...
      ...
    </Plugin>
    ...
```

Configure outros [plug-ins collectd](https://collectd.org/wiki/index.php/Table_of_Plugins), que possam coletar vários dados de diferentes fontes.

Reinicie o collectd, de acordo com seu [manual](https://collectd.org/wiki/index.php/First_steps).

## <a name="view-the-data-in-application-insights"></a>Exibir os dados no Application Insights
No recurso Application Insights, abra [métricas e adicione gráficos][metrics], selecionando as métricas que você deseja ver na categoria personalizada.

Por padrão, as métricas são agregadas em todos os computadores host dos quais as métricas foram coletadas. Para exibir as métricas por host, na folha Detalhes do gráfico, ative Agrupamento e escolha Agrupar por CollectD-Host.

## <a name="to-exclude-upload-of-specific-statistics"></a>Excluir o carregamento de estatísticas específicas
Por padrão, o plug-in do Application Insights envia todos os dados coletados por todos os plug-ins collect ‘read’ habilitados. 

Para excluir dados de plug-ins ou fontes de dados específicos:

* Edite o arquivo de configuração. 
* Em `<Plugin ApplicationInsightsWriter>`, adicione linhas diretivas como esta:

| Diretiva | Efeito |
| --- | --- |
| `Exclude disk` |Excluir todos os dados coletados pelo plug-in `disk` |
| `Exclude disk:read,write` |Exclua as fontes denominadas `read` e `write` do plug-in `disk`. |

Diretivas separadas por uma nova linha.

## <a name="problems"></a>Problemas?
*Não vejo dados no portal*

* Abra [Pesquisar][diagnostic] para ver se os eventos brutos aparecem. Às vezes, eles levam mais tempo para aparecer no Metrics Explorer.
* Talvez você precise [definir exceções de firewall para dados de saída](../../azure-monitor/app/ip-addresses.md)
* Habilite o rastreamento no plug-in do Application Insights. Adicione esta linha em `<Plugin ApplicationInsightsWriter>`:
  * `SDKLogger true`
* Abra um terminal e inicie collectd no modo detalhado para ver todos os problemas que ele está reportando:
  * `sudo collectd -f`

## <a name="known-issue"></a>Problema conhecido

O plug-in de Gravação do Application Insights é incompatível com determinados plugins de Leitura. Alguns plugins às vezes enviam "NaN" onde o plug-in do Application Insights espera um número de ponto flutuante.

Sintoma: o log coletado mostra erros que incluem "ia:... SyntaxError: token N inesperado ".

Solução alternativa: exclua dados coletados pelo problema de plugins de Gravação. 

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[apiexceptions]: ../../azure-monitor/app/api-custom-events-metrics.md#track-exception
[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: java-get-started.md
[javalogs]: java-trace-logs.md
[metrics]: ../../azure-monitor/platform/metrics-charts.md


