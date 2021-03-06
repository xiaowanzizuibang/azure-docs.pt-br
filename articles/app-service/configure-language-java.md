---
title: Configurar aplicativos Java do Windows
description: Saiba como configurar aplicativos Java para serem executados em instâncias de VM do Windows no serviço Azure App. Este artigo mostra as tarefas de configuração mais comuns.
keywords: serviço de aplicativo do Azure, aplicativo Web, Windows, OSS, Java
author: jasonfreeberg
ms.devlang: java
ms.topic: article
ms.date: 04/12/2019
ms.author: jafreebe
ms.reviewer: cephalin
ms.custom: seodec18
ms.openlocfilehash: cebe5564767ee345c1aea96b6ac54b9398c3e9a3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81733025"
---
# <a name="configure-a-windows-java-app-for-azure-app-service"></a>Configurar um aplicativo Java do Windows para o serviço Azure App

Azure App serviço permite que os desenvolvedores de Java compilem, implantem e dimensionem rapidamente seus aplicativos Web Tomcat em um serviço totalmente gerenciado baseado no Windows. Implante aplicativos com o plug-ins do Maven na linha de comando ou em editores, como IntelliJ, Eclipse ou Visual Studio Code.

Este guia fornece os principais conceitos e instruções para desenvolvedores Java usando o no serviço de aplicativo. Se você nunca usou Azure App serviço, leia primeiro o [início rápido do Java](app-service-web-get-started-java.md) . As perguntas gerais sobre como usar o serviço de aplicativo que não são específicas do desenvolvimento do Java são respondidas nas [perguntas frequentes do Windows do serviço de aplicativo](faq-configuration-and-management.md).

## <a name="deploying-your-app"></a>Implantação do aplicativo

Você pode usar o [plug-in do aplicativo Web do Azure para Maven](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) para implantar seus arquivos. War. A implantação com IDEs populares também tem suporte com [Azure Toolkit for IntelliJ](/java/azure/intellij/azure-toolkit-for-intellij) ou [Azure Toolkit for Eclipse](/java/azure/eclipse/azure-toolkit-for-eclipse).

Caso contrário, o método de implantação dependerá do tipo de arquivo morto:

- Para implantar arquivos .war para Tomcat, use o ponto de extremidade `/api/wardeploy/` para executar POST de seu arquivo morto. Para obter mais informações sobre essa API, confira [essa documentação](https://docs.microsoft.com/azure/app-service/deploy-zip#deploy-war-file).
- Para implantar arquivos. jar em Java SE, use o `/api/zipdeploy/` ponto de extremidade do site kudu. Para obter mais informações sobre essa API, confira [essa documentação](https://docs.microsoft.com/azure/app-service/deploy-zip#rest).

Não implante seu. War usando FTP. A ferramenta FTP foi projetada para carregar os scripts de inicialização, dependências ou outros arquivos de runtime. Não é a opção ideal para a implantação de aplicativos Web.

## <a name="logging-and-debugging-apps"></a>Aplicativos de registro em log e depuração

Relatórios de desempenho, visualizações de tráfego e verificações de integridade estão disponíveis para cada aplicativo por meio do portal do Azure. Para obter mais informações, consulte [visão geral do diagnóstico de serviço Azure app](overview-diagnostics.md).

### <a name="use-flight-recorder"></a>Usar o Flight Recorder

Todos os tempos de execução do Java no serviço de aplicativo usando o azul JVMs vêm com o gravador de vôo Zulu. Você pode usar isso para registrar eventos de nível JVM, System e Java para monitorar o comportamento e solucionar problemas em seus aplicativos Java.

Para fazer uma gravação cronometrada, você precisará do PID (ID do processo) do aplicativo Java. Para localizar o PID, abra um navegador para o site do SCM do seu aplicativo Web em https://<your-site-Name>. scm.azurewebsites.net/ProcessExplorer/. Esta página mostra os processos em execução em seu aplicativo Web. Localize o processo chamado "Java" na tabela e copie o PID correspondente (ID do processo).

Em seguida, abra o **console de depuração** na barra de ferramentas superior do site do SCM e execute o comando a seguir. Substitua `<pid>` pela ID do processo que você copiou anteriormente. Esse comando iniciará uma gravação do criador de perfil de 30 segundos de seu aplicativo Java e `timed_recording_example.jfr` gerará `D:\home` um arquivo chamado no diretório.

```
jcmd <pid> JFR.start name=TimedRecording settings=profile duration=30s filename="D:\home\timed_recording_example.JFR"
```

Para obter mais informações, consulte a [referência do comando Jcmd](https://docs.oracle.com/javacomponents/jmc-5-5/jfr-runtime-guide/comline.htm#JFRRT190).

#### <a name="analyze-jfr-files"></a>Analisar `.jfr` arquivos

Use [FTPS](deploy-ftp.md) para baixar o arquivo JFR para o computador local. Para analisar o arquivo JFR, baixe e instale o [controle de missão Zulu](https://www.azul.com/products/zulu-mission-control/). Para obter instruções sobre o controle de missão Zulu, consulte a [documentação azul](https://docs.azul.com/zmc/) e as [instruções de instalação](https://docs.microsoft.com/java/azure/jdk/java-jdk-flight-recorder-and-mission-control).

### <a name="stream-diagnostic-logs"></a>Logs de diagnóstico de fluxo

[!INCLUDE [Access diagnostic logs](../../includes/app-service-web-logs-access-no-h.md)]

Para obter mais informações, consulte [Stream logs in Cloud Shell](troubleshoot-diagnostic-logs.md#in-cloud-shell).

### <a name="app-logging"></a>Registro em log do aplicativo

Habilite o [registro em log de aplicativos](troubleshoot-diagnostic-logs.md#enable-application-logging-windows) por meio do portal do Azure ou da [CLI do Azure](/cli/azure/webapp/log#az-webapp-log-config) para configurar o Serviço de Aplicativo do Azure para gravar a saída do console padrão do aplicativo e os fluxos de erro do console padrão no sistema de arquivos local ou no Armazenamento de Blobs do Azure. O registro em log na instância do sistema de arquivos do Serviço de Aplicativo local será desabilitado 12 horas após ser configurado. Se você precisar de uma retenção mais longa, configure o aplicativo para gravar a saída em um contêiner do armazenamento de blobs. Seus logs de aplicativo Java e Tomcat podem ser encontrados no diretório */LogFiles/Application/*

Se o aplicativo usar [Logback](https://logback.qos.ch/) ou [Log4j](https://logging.apache.org/log4j) para rastreamento, será possível encaminhá-los para revisão no Azure Application Insights usando as instruções de configuração de estrutura de registros em [Explorar os logs de rastreamento de Java no Application Insights](/azure/application-insights/app-insights-java-trace-logs).


## <a name="customization-and-tuning"></a>Personalização e ajuste

O serviço de Azure App dá suporte ao ajuste e à personalização prontos para uso por meio do portal do Azure e da CLI. Examine os seguintes artigos para configuração de aplicativo Web não específica para Java:

- [Definir configurações de aplicativo](configure-common.md#configure-app-settings)
- [Configurar um domínio personalizado](app-service-web-tutorial-custom-domain.md)
- [Configurar associações TLS](configure-ssl-bindings.md)
- [Adicionar uma CDN](../cdn/cdn-add-to-web-app.md)
- [Configurar o site kudu](https://github.com/projectkudu/kudu/wiki/Configurable-settings)

### <a name="set-java-runtime-options"></a>Definir opções de runtime do Java

Para definir a memória alocada ou outras opções de tempo de execução da JVM `JAVA_OPTS` , crie uma configuração de [aplicativo](configure-common.md#configure-app-settings) chamada com as opções. O serviço de aplicativo passa essa configuração como uma variável de ambiente para o tempo de execução Java quando ele é iniciado.

No portal do Azure, em **Configurações do aplicativo** para o aplicativo Web, crie uma configuração de aplicativo denominada `JAVA_OPTS` que inclui as configurações adicionais, como `-Xms512m -Xmx1204m`.

Para definir a configuração do aplicativo do plug-in do Maven, adicione marcas de configuração/valor na seção plug-in do Azure. O exemplo a seguir define um tamanho de heap de Java mínimo e máximo específico:

```xml
<appSettings>
    <property>
        <name>JAVA_OPTS</name>
        <value>-Xms512m -Xmx1204m</value>
    </property>
</appSettings>
```

Os desenvolvedores que executam um único aplicativo com um slot de implantação no Plano do Serviço de Aplicativo podem usar as seguintes opções:

- Instâncias B1 e S1:`-Xms1024m -Xmx1024m`
- Instâncias B2 e S2:`-Xms3072m -Xmx3072m`
- Instâncias B3 e S3:`-Xms6144m -Xmx6144m`

Ao ajustar as configurações de heap do aplicativo, examine os detalhes do Plano do Serviço de Aplicativo e considere os vários aplicativos e slots de implantação necessários para encontrar a alocação de memória ideal.

### <a name="turn-on-web-sockets"></a>Ativar os soquetes da Web

Ative o suporte para soquetes da Web no portal do Azure, nas **Configurações de aplicativo**. Será necessário reiniciar o aplicativo para a configuração entrar em vigor.

Ative o suporte para soquete da Web usando a CLI do Azure com o seguinte comando:

```azurecli-interactive
az webapp config set --name <app-name> --resource-group <resource-group-name> --web-sockets-enabled true
```

Depois, reinicie o aplicativo:

```azurecli-interactive
az webapp stop --name <app-name> --resource-group <resource-group-name>
az webapp start --name <app-name> --resource-group <resource-group-name>
```

### <a name="set-default-character-encoding"></a>Definir a codificação de caractere padrão

No portal do Azure, em **Configurações do aplicativo** para o aplicativo Web, crie uma configuração de aplicativo denominada `JAVA_OPTS` com o valor `-Dfile.encoding=UTF-8`.

Como alternativa, é possível definir a configuração do aplicativo usando o plug-in do Maven do Serviço de Aplicativo. Adicione as marcas de nome e valor de configuração à configuração do plug-in:

```xml
<appSettings>
    <property>
        <name>JAVA_OPTS</name>
        <value>-Dfile.encoding=UTF-8</value>
    </property>
</appSettings>
```

### <a name="pre-compile-jsp-files"></a>Pré-compilar arquivos JSP

Para melhorar o desempenho de aplicativos Tomcat, você pode compilar seus arquivos JSP antes de implantar no serviço de aplicativo. Você pode usar o [plug-in do Maven](https://sling.apache.org/components/jspc-maven-plugin/plugin-info.html) fornecido pelo Apache sling ou usando esse [arquivo de compilação Ant](https://tomcat.apache.org/tomcat-9.0-doc/jasper-howto.html#Web_Application_Compilation).

## <a name="secure-applications"></a>Aplicativos seguros

Aplicativos Java em execução no serviço de aplicativo têm o mesmo conjunto de [práticas recomendadas de segurança](/azure/security/security-paas-applications-using-app-services) que outros aplicativos.

### <a name="authenticate-users-easy-auth"></a>Autenticar usuários (fácil autenticação)

Configure a autenticação de aplicativo no portal do Azure com a opção **autenticação e autorização** . Em seguida, será possível habilitar a autenticação usando o Azure Active Directory ou os logons de redes sociais, como Facebook, Google ou GitHub. A configuração do portal do Azure só funciona ao configurar um único provedor de autenticação. Para saber mais, confira [Configurar o aplicativo do Serviço de Aplicativo para usar o logon do Azure Active Directory](configure-authentication-provider-aad.md) e os artigos relacionados para outros provedores de identidade. Se você precisar habilitar vários provedores de entrada, siga as instruções no artigo [Personalizar a autenticação do Serviço de Aplicativo](app-service-authentication-how-to.md).

#### <a name="tomcat"></a>Tomcat

Seu aplicativo Tomcat pode acessar as declarações do usuário diretamente do servlet, convertendo o objeto principal em um objeto Map. O objeto MAP mapeará cada tipo de declaração para uma coleção de declarações para esse tipo. No código a seguir, `request` é uma instância do `HttpServletRequest`.

```java
Map<String, Collection<String>> map = (Map<String, Collection<String>>) request.getUserPrincipal();
```

Agora você pode inspecionar `Map` o objeto para qualquer declaração específica. Por exemplo, o trecho de código a seguir itera em todos os tipos de declaração e imprime o conteúdo de cada coleção.

```java
for (Object key : map.keySet()) {
        Object value = map.get(key);
        if (value != null && value instanceof Collection {
            Collection claims = (Collection) value;
            for (Object claim : claims) {
                System.out.println(claims);
            }
        }
    }
```

Para desconectar os usuários, use `/.auth/ext/logout` o caminho. Para executar outras ações, consulte a documentação sobre o [uso de autenticação e autorização do serviço de aplicativo](https://docs.microsoft.com/azure/app-service/app-service-authentication-how-to). Também há documentação oficial na [interface HttpServletRequest](https://tomcat.apache.org/tomcat-5.5-doc/servletapi/javax/servlet/http/HttpServletRequest.html) do Tomcat e seus métodos. Os seguintes métodos servlets também são alimentados com base na configuração do serviço de aplicativo:

```java
public boolean isSecure()
public String getRemoteAddr()
public String getRemoteHost()
public String getScheme()
public int getServerPort()
```

Para desabilitar esse recurso, crie uma configuração de aplicativo `WEBSITE_AUTH_SKIP_PRINCIPAL` chamada com um valor `1`de. Para desabilitar todos os filtros de servlet adicionados pelo serviço de aplicativo, crie `WEBSITE_SKIP_FILTERS` uma configuração chamada com `1`um valor de.

### <a name="configure-tlsssl"></a>Configurar TLS/SSL

Siga as instruções em [proteger um nome DNS personalizado com uma associação TLS no serviço Azure app](configure-ssl-bindings.md) para carregar um certificado TLS/SSL existente e associá-lo ao nome de domínio do seu aplicativo. Por padrão, o aplicativo ainda permitirá conexões HTTP – siga as etapas específicas no tutorial para impor o SSL e o TLS.

### <a name="use-keyvault-references"></a>Usar referências do keyvault

O [Azure keyvault](../key-vault/general/overview.md) fornece gerenciamento de segredos centralizado com políticas de acesso e histórico de auditoria. Você pode armazenar segredos (como senhas ou cadeias de conexão) no keyvault e acessar esses segredos em seu aplicativo por meio de variáveis de ambiente.

Primeiro, siga as instruções para [conceder acesso ao aplicativo para Key Vault](app-service-key-vault-references.md#granting-your-app-access-to-key-vault) e [fazer uma referência de keyvault para seu segredo em uma configuração de aplicativo](app-service-key-vault-references.md#reference-syntax). Você pode validar que a referência seja resolvida para o segredo imprimindo a variável de ambiente ao acessar remotamente o terminal do serviço de aplicativo.

Para injetar esses segredos em seu arquivo de configuração Spring ou Tomcat, use a sintaxe de`${MY_ENV_VAR}`injeção de variável de ambiente (). Para arquivos de configuração do Spring, consulte esta documentação sobre [configurações externas](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html).


## <a name="configure-apm-platforms"></a>Configurar plataformas APM

Esta seção mostra como conectar aplicativos Java implantados no serviço de Azure App no Linux com as plataformas de monitoramento de desempenho de aplicativos NewRelic e AppDynamics (APM).

### <a name="configure-new-relic"></a>Configurar o New Relic

1. Criar uma nova conta do Relic em [NewRelic.com](https://newrelic.com/signup)
2. Baixe o agente Java de NewRelic, ele terá um nome de arquivo semelhante a *newrelic-Java-x. x. x. zip*.
3. Copie sua chave de licença, você precisará dela para configurar o agente mais tarde.
4. Use o [console do kudu](https://github.com/projectkudu/kudu/wiki/Kudu-console) para criar um novo diretório */Home/site/wwwroot/APM*.
5. Carregue os arquivos do novo agente Java Relic desempacotados em um diretório em */Home/site/wwwroot/APM*. Os arquivos do seu agente devem estar no */Home/site/wwwroot/APM/newrelic*.
6. Modifique o arquivo YAML em */Home/site/wwwroot/APM/newrelic/newrelic.yml* e substitua o valor da licença de espaço reservado por sua própria chave de licença.
7. No portal do Azure, navegue até seu aplicativo no serviço de aplicativo e criar uma nova configuração de aplicativo.
    - Se seu aplicativo estiver usando **Java SE**, crie uma variável de ambiente denominada `JAVA_OPTS` com o valor `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`.
    - Se você estiver usando **Tomcat**, crie uma variável de ambiente denominada `CATALINA_OPTS` com o valor `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`.

### <a name="configure-appdynamics"></a>Configurar o AppDynamics

1. Criar uma conta do AppDynamics em [AppDynamics.com](https://www.appdynamics.com/community/register/)
2. Baixe o agente Java do site AppDynamics, o nome do arquivo será semelhante a *AppServerAgent-x. x. x. xxxxx. zip*
3. Use o [console do kudu](https://github.com/projectkudu/kudu/wiki/Kudu-console) para criar um novo diretório */Home/site/wwwroot/APM*.
4. Carregue os arquivos do agente Java em um diretório em */Home/site/wwwroot/APM*. Os arquivos do seu agente devem estar no */Home/site/wwwroot/APM/AppDynamics*.
5. No portal do Azure, navegue até seu aplicativo no serviço de aplicativo e criar uma nova configuração de aplicativo.
    - Se seu aplicativo estiver usando **Java SE**, crie uma variável de ambiente denominada `JAVA_OPTS` com o valor `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>` quando `<app-name>` for o nome do seu Serviço de Aplicativo.
    - Se você estiver usando **Java SE**, crie uma variável de ambiente denominada `CATALINA_OPTS` com o valor `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>` quando `<app-name>` for o nome do seu Serviço de Aplicativo.

>  Se você já tiver uma variável de ambiente para `JAVA_OPTS` ou `CATALINA_OPTS`, acrescente o `-javaagent:/...` opção ao final do valor atual.

## <a name="data-sources"></a>Fontes de dados

### <a name="tomcat"></a>Tomcat

Essas instruções se aplicam a todas as conexões de banco de dados. Você precisará preencher os espaços reservados com nome de classe do driver do banco de dados escolhido e o arquivo JAR. É fornecida uma tabela com nomes de classe e downloads de driver para bancos de dados comuns.

| Banco de dados   | Nome de Classe do Driver                             | Driver JDBC                                                                      |
|------------|-----------------------------------------------|------------------------------------------------------------------------------------------|
| PostgreSQL | `org.postgresql.Driver`                        | [Download](https://jdbc.postgresql.org/download.html)                                    |
| MySQL      | `com.mysql.jdbc.Driver`                        | [Baixar](https://dev.mysql.com/downloads/connector/j/) (Selecione "Independente de Plataforma") |
| SQL Server | `com.microsoft.sqlserver.jdbc.SQLServerDriver` | [Download](https://docs.microsoft.com/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server?view=sql-server-2017#download)                                                           |

Para configurar o Tomcat para usar Java Database Connectivity (JDBC) ou a API de persistência Java (JPA), primeiro `CATALINA_OPTS` Personalize a variável de ambiente que é lida pelo Tomcat na inicialização. Defina esses valores por meio de uma configuração de aplicativo no [plug-in Maven do Serviço de Aplicativo](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md):

```xml
<appSettings>
    <property>
        <name>CATALINA_OPTS</name>
        <value>"$CATALINA_OPTS -Ddbuser=${DBUSER} -Ddbpassword=${DBPASSWORD} -DconnURL=${CONNURL}"</value>
    </property>
</appSettings>
```

Ou defina as variáveis de ambiente na página**configurações do aplicativo** de **configuração** > no portal do Azure.

Em seguida, determine se a fonte de dados deve estar disponível para um aplicativo ou para todos os aplicativos em execução no servlet do Tomcat.

#### <a name="application-level-data-sources"></a>Fontes de dados em nível de aplicativo

1. Crie um arquivo *Context. xml* no diretório *meta-inf/* do seu projeto. Crie o diretório *meta-inf/* se ele não existir.

2. Em *Context. xml*, adicione um `Context` elemento para vincular a fonte de dados a um endereço JNDI. Substitua o espaço reservado `driverClassName` pelo nome de classe do seu driver da tabela acima.

    ```xml
    <Context>
        <Resource
            name="jdbc/dbconnection"
            type="javax.sql.DataSource"
            url="${dbuser}"
            driverClassName="<insert your driver class name>"
            username="${dbpassword}"
            password="${connURL}"
        />
    </Context>
    ```

3. Atualize o *Web. xml* do seu aplicativo para usar a fonte de dados em seu aplicativo.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/dbconnection</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

#### <a name="finalize-configuration"></a>Finalizar a configuração

Por fim, colocaremos os JARs do driver no classpath do Tomcat e reiniciaremos o serviço de aplicativo. Certifique-se de que os arquivos do driver JDBC estejam disponíveis para o carregador de diretório do Tomcat colocando-os no */Home/Tomcat/lib* Directory. (Crie esse diretório se ele ainda não existir.) Para carregar esses arquivos em sua instância do serviço de aplicativo, execute as seguintes etapas:

1. No [Cloud Shell](https://shell.azure.com), instale a extensão webapp:

    ```azurecli-interactive
    az extension add -–name webapp
    ```

2. Execute o seguinte comando da CLI para criar um túnel SSH do seu sistema local para o serviço de aplicativo:

    ```azurecli-interactive
    az webapp remote-connection create --resource-group <resource-group-name> --name <app-name> --port <port-on-local-machine>
    ```

3. Conecte-se à porta de túnel local com seu cliente SFTP e carregue os arquivos na pasta */Home/Tomcat/lib*

Como alternativa, você pode usar um cliente de FTP para carregar o driver JDBC. Siga estas [instruções para obter suas credenciais FTP](deploy-configure-credentials.md).

## <a name="configuring-tomcat"></a>Configurando o Tomcat

Para editar os arquivos `server.xml` de configuração ou do Tomcat, primeiro anote a sua versão principal do Tomcat no Portal.

1. Localize o diretório base do Tomcat para sua versão executando o `env` comando. Procure a variável de ambiente que começa com `AZURE_TOMCAT`e corresponde à sua versão principal. Por exemplo, `AZURE_TOMCAT85_HOME` aponta para o diretório Tomcat para Tomcat 8,5.
1. Depois de identificar o diretório base do Tomcat para sua versão, copie o diretório de configuração `D:\home`para. Por exemplo, se `AZURE_TOMCAT85_HOME` tiver um valor de `D:\Program Files (x86)\apache-tomcat-8.5.37`, o novo caminho do diretório copiado seria `D:\home\apache-tomcat-8.5.37`.

Por fim, reinicie o Serviço de Aplicativo. Suas implantações devem ir `D:\home\site\wwwroot\webapps` para assim como antes.

## <a name="configure-java-se"></a>Configurar Java SE

Ao executar um. Aplicativo JAR em Java SE no Windows, `server.port` é passado como uma opção de linha de comando à medida que seu aplicativo é iniciado. Você pode resolver manualmente a porta HTTP da variável de ambiente, `HTTP_PLATFORM_PORT`. O valor dessa variável de ambiente será a porta HTTP em que seu aplicativo deve escutar. 

## <a name="java-runtime-statement-of-support"></a>Declaração de suporte do runtime do Java

### <a name="jdk-versions-and-maintenance"></a>Versões e manutenção do JDK

O JDK (Java Development Kit) com suporte do Azure é o [Zulu](https://www.azul.com/downloads/azure-only/zulu/) fornecido pela [Azul Systems](https://www.azul.com/).

As atualizações da versão principal serão fornecidas por meio de novas opções de tempo de execução no serviço Azure App para Windows. Os clientes podem atualizar para essas versões mais recentes do Java configurando a implantação do Serviço de Aplicativo. Além disso, são responsáveis por testar e garantir que a atualização da versão principal atenda às suas necessidades.

Os JDKs com suporte são corrigidos automaticamente a cada trimestre em janeiro, abril, julho e outubro de cada ano. Para saber mais sobre Java no Azure, confira [este documento de suporte](https://docs.microsoft.com/azure/developer/java/fundamentals/java-jdk-long-term-support).

### <a name="security-updates"></a>Atualizações de segurança

Os patches e as correções para grandes vulnerabilidades de segurança serão liberados assim que forem disponibilizados pela Azul Systems. Uma "grande" vulnerabilidade é definida por uma pontuação básica de 9,0 ou mais na [versão 2 do NIST Common Vulnerability Scoring System](https://nvd.nist.gov/cvss.cfm).

O Tomcat 8,0 atingiu [o fim da vida útil (EOL) a partir de 30 de setembro de 2018](https://tomcat.apache.org/tomcat-80-eol.html). Embora o tempo de execução ainda esteja disponíveis no serviço Azure App, o Azure não aplicará atualizações de segurança ao Tomcat 8,0. Se possível, migre seus aplicativos para o Tomcat 8,5 ou 9,0. O Tomcat 8,5 e o 9,0 estão disponíveis no serviço Azure App. Consulte o [site oficial do Tomcat](https://tomcat.apache.org/whichversion.html) para obter mais informações. 

### <a name="deprecation-and-retirement"></a>Reprovação e desativação

Se um runtime do Java com suporte for desativado, os desenvolvedores do Azure que usam o runtime afetado receberão um aviso de reprovação pelo menos seis meses antes de o runtime ser desativado.

### <a name="local-development"></a>Desenvolvimento local

Os desenvolvedores podem baixar a Production Edition do Azul Zulu Enterprise JDK para desenvolvimento local do [site de download da Azul](https://www.azul.com/downloads/azure-only/zulu/).

### <a name="development-support"></a>Suporte ao desenvolvimento

O suporte ao produto para o [azul Zulu com suporte do Azure](https://www.azul.com/downloads/azure-only/zulu/) está disponível por meio da Microsoft ao desenvolver para o azure ou [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) com um [plano de suporte qualificado do Azure](https://azure.microsoft.com/support/plans/).

### <a name="runtime-support"></a>Suporte de runtime

Os desenvolvedores podem [abrir um problema](/azure/azure-portal/supportability/how-to-create-azure-support-request) com o Azul Zulu JDK por meio do Suporte do Azure se tiverem um [plano de suporte qualificado](https://azure.microsoft.com/support/plans/).

## <a name="next-steps"></a>Próximas etapas

Este tópico fornece a declaração de suporte do Java em tempo de execução para Azure App serviço no Windows.

- Para saber mais sobre como hospedar aplicativos Web com o serviço Azure App, consulte [visão geral do serviço de aplicativo](overview.md).
- Para obter informações sobre o Java no desenvolvimento do Azure, consulte [Azure para Java dev Center](https://docs.microsoft.com/java/azure/?view=azure-java-stable).
