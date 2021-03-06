---
title: Monitor com testes na Web de várias etapas-insights de Aplicativo Azure
description: Configurar testes na Web de várias etapas para monitorar seus aplicativos Web com o Aplicativo Azure insights
ms.topic: conceptual
ms.date: 10/23/2019
ms.reviewer: sdash
ms.openlocfilehash: 3b8baad127b16a1bd9d071d0c3d4df68da8c3304
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77655933"
---
# <a name="multi-step-web-tests"></a>Testes na Web com diversas etapas

Você pode monitorar uma sequência registrada de URLs e interações com um site por meio de testes na Web de várias etapas. Este artigo explicará o processo de criação de um teste na Web de várias etapas com Visual Studio Enterprise.

> [!NOTE]
> Testes na Web de várias etapas dependem de arquivos do Visual Studio WebTest. Foi [anunciado](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/) que o Visual Studio 2019 será a última versão com a funcionalidade WebTest. É importante entender que, embora nenhum novo recurso seja adicionado, a funcionalidade do WebTest no Visual Studio 2019 ainda é suportada e continuará a ter suporte durante o ciclo de vida do suporte do produto. A equipe de produto Azure Monitor resolveu perguntas sobre o futuro de testes de disponibilidade de várias etapas [aqui](https://github.com/MicrosoftDocs/azure-docs/issues/26050#issuecomment-468814101).  

## <a name="pre-requisites"></a>Pré-requisitos

* Visual Studio 2017 Enterprise ou superior.
* Ferramentas de teste de carga e desempenho na Web do Visual Studio.

Para localizar o pré-requisito das ferramentas de teste. Inicie o **instalador do Visual Studio** > **componentes** > individuais de**depuração e teste** > de**desempenho da Web e ferramentas de teste de carga**.

![Captura de tela da interface do usuário do instalador do Visual Studio com componentes individuais selecionados com uma caixa de seleção ao lado do item para ferramentas de teste de carga e desempenho na Web](./media/availability-multistep/web-performance-load-testing.png)

> [!NOTE]
> Testes na Web de várias etapas têm custos adicionais associados a eles. Para saber mais, consulte o [Guia de preços oficial](https://azure.microsoft.com/pricing/details/application-insights/).

## <a name="record-a-multi-step-web-test"></a>Registrar um teste na Web de várias etapas 

> [!WARNING]
> Não é mais recomendável usar o gravador de várias etapas. O gravador foi desenvolvido para páginas HTML estáticas com interações básicas e não fornece uma experiência funcional para páginas da Web modernas.

Para obter orientação sobre como criar testes na Web do Visual Studio, consulte a [documentação oficial do visual studio 2019](https://docs.microsoft.com/visualstudio/test/how-to-create-a-web-service-test?view=vs-2019).

## <a name="upload-the-web-test"></a>Carregar o teste na Web

1. No portal do Application insights no painel disponibilidade, selecione **criar** > **Test type** > teste teste**da Web de várias etapas**.

2. Defina os locais de teste, a frequência e os parâmetros de alerta.

### <a name="frequency--location"></a>Local de & de frequência

|Setting| Explicação
|----|----|----|
|**Frequência de teste**| define a frequência com que o teste é executado em cada localização de teste. Com uma frequência padrão de cinco minutos e cinco locais de teste, seu site é testado em média a cada minuto.|
|**Locais de teste**| São os locais de onde nossos servidores enviam solicitações da Web para sua URL. **Nosso número mínimo de locais de teste recomendados é cinco** para garantir que você possa distinguir problemas em seu site por meio de problemas de rede. Você pode selecionar até 16 locais.

### <a name="success-criteria"></a>Critérios de sucesso

|Setting| Explicação
|----|----|----|
| **Tempo limite do teste** |diminua esse valor para ser alertado sobre respostas lentas. O teste é considerado uma falha se as respostas de seu site não são recebidas dentro desse período. Se você tiver selecionado **Analisar solicitações dependentes**, todas as imagens, arquivos de estilo, scripts e outros recursos dependentes devem ter sido recebidos dentro desse período.|
| **Resposta HTTP** | o código de status retornado que é contado como êxito. 200 é o código que indica que uma página da Web normal foi retornada.|
| **Correspondência de conteúdo** | Uma cadeia de caracteres, como "bem-vindo!" Faremos o teste que uma correspondência exata de maiúsculas e minúsculas ocorre em todas as respostas. É necessário que seja uma cadeia de caracteres simples, sem curingas. Lembre-se de que se o conteúdo de sua página for alterado, talvez seja necessário atualizá-lo. **Somente caracteres em inglês têm suporte com correspondência de conteúdo** |

### <a name="alerts"></a>Alertas

|Setting| Explicação
|----|----|----|
|**Quase em tempo real (visualização)** | É recomendável usar alertas quase em tempo real. A configuração desse tipo de alerta é feita após a criação do teste de disponibilidade.  |
|**Clássico** | Não recomendamos o uso de alertas clássicos para novos testes de disponibilidade.|
|**Limite de local de alerta**|é recomendável um mínimo de 3/5 locais. A relação ideal entre o limite de local de alerta e o número de locais de teste é o número **limite** = **de local de alerta de locais de teste-2, com um mínimo de cinco locais de teste.**|

## <a name="configuration"></a>Configuração

### <a name="plugging-time-and-random-numbers-into-your-test"></a>Tempo de conexão e números aleatórios em seu teste

Suponha que você está testando uma ferramenta que obtém dados dependentes de tempo, como estoques de um feed externo. Quando grava seu teste na Web você deve usar horários específicos, definindo-os, todavia, como parâmetros do teste, StartTime e EndTime.

![Captura de tela de meu aplicativo de ações incríveis](./media/availability-multistep/app-insights-72webtest-parameters.png)

Quando você executa o teste, o ideal é que EndTime seja sempre a hora atual e StartTime seja o horário de 15 minutos atrás.

O plug-in de data e hora do teste na Web fornece a maneira de lidar com os tempos de parametrização.

1. Adicione um plug-in de teste na Web para cada valor de parâmetro variável desejado. Na barra de ferramentas de teste da Web, escolha **Adicionar Plug-in de Teste na Web**.
    
    ![Adicionar plug-in de teste na Web](./media/availability-multistep/app-insights-72webtest-plugin-name.png)
    
    Neste exemplo, usamos duas instâncias do plug-in de Data e Hora. É uma instância de "15 minutos atrás" e outra de "agora".

2. Abra as propriedades de cada plug-in. Atribua um nome e configure-o para usar a hora atual. Para um deles, defina Add Minutes = -15.

    ![Parâmetros de contexto](./media/availability-multistep/app-insights-72webtest-plugin-parameters.png)

3. Nos parâmetros de teste na Web, use {{nome do plug-in}} para fazer referência a um nome de plug-in.

    ![StartTime](./media/availability-multistep/app-insights-72webtest-plugins.png)

Agora, carregue seu teste no portal. Ele usará os valores dinâmicos em todas as execuções do teste.

### <a name="dealing-with-sign-in"></a>Lidando com a entrada

Se os usuários entrarem em seu aplicativo, você terá várias opções para simular entradas para poder testar as páginas por trás da entrada. A abordagem usada dependerá do tipo de segurança fornecida pelo aplicativo.

Em todos os casos, você deve criar uma conta no aplicativo apenas para fins de teste. Se possível, restrinja as permissões da conta de teste para que não haja possibilidade de que os testes na Web afetem usuários reais.

**Nome de usuário e senha simples** Registre um teste na Web da maneira usual. Exclua os cookies primeiro.

**Autenticação SAML**

|Nome da propriedade| Descrição|
|----|-----|
| URI do público | O URI do público-alvo do token SAML.  Esse é o URI para o serviço de controle de acesso (ACS) – incluindo o nome do host e o namespace do ACS. |
| Senha do certificado | A senha para o certificado de cliente que concederá acesso à chave privada inserida. |
| Certificado do cliente  | O valor do certificado de cliente com a chave privada no formato codificado em base64. |
| Identificador de nome | O identificador de nome para o token |
| Não depois | O TimeSpan para o qual o token será válido.  O padrão é 5 minutos. |
| Não Antes De | O TimeSpan para o qual um token criado no passado será válido (para endereçar distorções de tempo).  O padrão é (negativo) 5 minutos. |
| Nome do parâmetro de contexto de destino | O parâmetro de contexto que receberá a declaração gerada. |


**Segredo do cliente** Se seu aplicativo tiver uma rota de entrada que envolva um segredo do cliente, use essa rota. O AAD (Azure Active Directory) é um exemplo de um serviço que fornece uma entrada de segredo do cliente. No AAD, o segredo do cliente é a Chave do Aplicativo.

Aqui está um teste na Web de exemplo de um aplicativo Web que usa uma chave de aplicativo:

![Captura de tela de exemplo](./media/availability-multistep/client-secret.png)

Obtenha o token do AAD usando o segredo do cliente (AppKey).
Extraia o token de portador da resposta.
Chame a API usando o token de portador no cabeçalho de autorização.
Verifique se o teste na Web é um cliente real, ou seja, se ele tem seu próprio aplicativo no AAD, e use sua chave de aplicativo clientId +. O serviço que está sendo testado também tem seu próprio aplicativo no AAD: o URI appID desse aplicativo é refletido no teste na Web no campo "recurso".

### <a name="open-authentication"></a>Autenticação Aberta
Um exemplo de autenticação aberta é entrar com sua conta da Microsoft ou do Google. Muitos aplicativos que usam OAuth fornecem a alternativa de segredo do cliente. Portanto, sua primeira tática deve ser investigar essa possibilidade.

Se o teste tiver de entrar usando OAuth, a abordagem geral será:

Use uma ferramenta como o Fiddler para examinar o tráfego entre o navegador da web, o site de autenticação e seu aplicativo.
Executar duas ou mais entradas usando computadores ou navegadores diferentes ou em longos intervalos (para permitir que os tokens expirem).
Ao comparar sessões diferentes, identifique o token passado de volta ao site de autenticação, que será então passado para o servidor de aplicativos após a entrada.
Registre um teste na Web usando o Visual Studio.
Parametrize os tokens, definindo o parâmetro quando o token for retornado do autenticador e usando-o na consulta ao site. (O Visual Studio tenta parametrizar o teste, mas não parametriza os tokens corretamente.)

## <a name="troubleshooting"></a>Solução de problemas

[Artigo de solução de problemas](troubleshoot-availability.md)dedicado.

## <a name="next-steps"></a>Próximas etapas

* [Alertas de disponibilidade](availability-alerts.md)
* [Testes da Web de ping de URL](monitor-web-app-availability.md)
