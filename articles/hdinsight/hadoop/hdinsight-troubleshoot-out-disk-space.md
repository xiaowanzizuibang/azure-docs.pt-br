---
title: O nó do cluster fica sem espaço em disco no Azure HDInsight
description: Solução de problemas de Apache Hadoop de espaço em disco do nó de cluster no Azure HDInsight.
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/05/2019
ms.openlocfilehash: fbfd82473b68f5032d19834ac809191d498a5a67
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75894121"
---
# <a name="scenario-cluster-node-runs-out-of-disk-space-in-azure-hdinsight"></a>Cenário: o nó de cluster é executado sem espaço em disco no Azure HDInsight

Este artigo descreve as etapas de solução de problemas e as possíveis resoluções para problemas ao interagir com clusters do Azure HDInsight.

## <a name="issue"></a>Problema

Um trabalho pode falhar com mensagem de erro semelhante a:`/usr/hdp/2.6.3.2-14/hadoop/libexec/hadoop-config.sh: fork: No space left on device.`

Ou você pode receber um alerta do Apache Ambari semelhante `local-dirs usable space is below configured utilization percentage`a:.

## <a name="cause"></a>Causa

O cache de aplicativos Apache yarn pode ter consumido todo o espaço em disco disponível. O aplicativo Spark provavelmente está sendo executado de forma ineficiente.

## <a name="resolution"></a>Resolução

1. Use a interface do usuário do Ambari para determinar qual nó está ficando sem espaço em disco.

1. Determine qual pasta no nó preocupantes contribui para a maior parte do espaço em disco. Use SSH para o nó primeiro e, `df` em seguida, execute para listar o uso do disco para todas as montagens. Normalmente, é `/mnt` um disco temporário usado pelo OSS. Você pode inserir em uma pasta e, em `sudo du -hs` seguida, digitar para mostrar os tamanhos de arquivo resumidos em uma pasta. Se você vir uma pasta semelhante a `/mnt/resource/hadoop/yarn/local/usercache/livy/appcache/application_1537280705629_0007`, isso significará que o aplicativo ainda está em execução. Isso pode ser devido à persistência de RDD ou a arquivos aleatórios intermediários.

1. Para atenuar o problema, encerre o aplicativo, que liberará o espaço em disco usado por esse aplicativo.

1. Para resolver o problema por fim, Otimize seu aplicativo.

## <a name="next-steps"></a>Próximas etapas

Se você não encontrou seu problema ou não conseguiu resolver seu problema, visite um dos seguintes canais para obter mais suporte:

* Obtenha respostas de especialistas do Azure por meio do [suporte da Comunidade do Azure](https://azure.microsoft.com/support/community/).

* Conecte- [@AzureSupport](https://twitter.com/azuresupport) se com a conta de Microsoft Azure oficial para melhorar a experiência do cliente conectando a Comunidade do Azure aos recursos certos: respostas, suporte e especialistas.

* Se precisar de mais ajuda, você poderá enviar uma solicitação de suporte do [portal do Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Selecione **suporte** na barra de menus ou abra o Hub **ajuda + suporte** . Para obter informações mais detalhadas, consulte [como criar uma solicitação de suporte do Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). O acesso ao gerenciamento de assinaturas e ao suporte de cobrança está incluído na sua assinatura do Microsoft Azure, e o suporte técnico é fornecido por meio de um dos [planos de suporte do Azure](https://azure.microsoft.com/support/plans/).
