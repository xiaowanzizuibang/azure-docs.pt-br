---
title: Hospedagem de site estático no Armazenamento do Microsoft Azure
description: O Armazenamento do Microsoft Azure agora oferece um site estático hospedand, fornecendo uma solução econômica e dimensionável para hospedar aplicativos web modernos.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.author: normesta
ms.reviewer: dineshm
ms.date: 05/29/2019
ms.subservice: blobs
ms.openlocfilehash: 848fd89444281d82d6d0d1bfc4df15d499c09ee0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79370484"
---
# <a name="static-website-hosting-in-azure-storage"></a>Hospedagem de site estático no Armazenamento do Microsoft Azure

Você pode fornecer conteúdo estático (HTML, CSS, JavaScript e arquivos de imagem) diretamente de um contêiner de armazenamento chamado *$Web*. Hospedar seu conteúdo no armazenamento do Azure permite que você use arquiteturas sem servidor que incluem [Azure Functions](/azure/azure-functions/functions-overview) e outros serviços de PaaS (plataforma como serviço).

[!INCLUDE [updated-for-az](../../../includes/storage-data-lake-gen2-support.md)]

> [!NOTE]
> Se o seu site depende do código do servidor, use [Azure app serviço](/azure/app-service/overview) em vez disso.

## <a name="setting-up-a-static-website"></a>Configurando um site estático

A hospedagem de site estática é um recurso que você precisa habilitar na conta de armazenamento.

Para habilitar a hospedagem de site estático, selecione o nome do arquivo padrão e, opcionalmente, forneça um caminho para uma página personalizada do 404. Se um contêiner de armazenamento de BLOBs chamado **$Web** ainda não existir na conta, um será criado para você. Adicione os arquivos do seu site a esse contêiner.

Para obter orientações passo a passo, consulte [hospedar um site estático no armazenamento do Azure](storage-blob-static-website-how-to.md).

![Métrica de métricas de sites estáticos de Armazenamento do Microsoft Azure](./media/storage-blob-static-website/storage-blob-static-website-blob-container.png)

Os arquivos no contêiner **$Web** diferenciam maiúsculas de minúsculas, são atendidos por meio de solicitações de acesso anônimo e estão disponíveis somente por meio de operações de leitura.

## <a name="uploading-content"></a>Carregando conteúdo

Você pode usar qualquer uma dessas ferramentas para carregar conteúdo no contêiner de **$Web** :

> [!div class="checklist"]
> * [CLI do Azure](storage-blob-static-website-how-to.md#cli)
> * [Módulo do Azure PowerShell](storage-blob-static-website-how-to.md#powershell)
> * [AzCopy](../common/storage-use-azcopy-v10.md)
> * [Gerenciador de Armazenamento do Azure](https://azure.microsoft.com/features/storage-explorer/)
> * [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/)
> * [Extensão Visual Studio Code](/azure/javascript/tutorial-vscode-static-website-node-01)

## <a name="viewing-content"></a>Exibindo conteúdo

Os usuários podem exibir o conteúdo do site de um navegador usando a URL pública do site. Você pode encontrar a URL usando o portal do Azure, o CLI do Azure ou o PowerShell. Use essa tabela como um guia.

|Ferramenta| Orientação |
|----|----|
|**Azure portal** | [Localize a URL do site usando o portal do Azure](storage-blob-static-website-how-to.md#portal-find-url) |
|**CLI do Azure** | [Localize a URL do site usando o CLI do Azure](storage-blob-static-website-how-to.md#cli-find-url) |
|**Módulo do Azure PowerShell** | [Localizar a URL do site usando o PowerShell](storage-blob-static-website-how-to.md#powershell-find-url) |

A URL do seu site contém um código regional. Por exemplo, a `https://contosoblobaccount.z22.web.core.windows.net/` URL contém código `z22`regional.

Embora esse código deva permanecer na URL, ele é apenas para uso interno, e você não precisará usar esse código de nenhuma outra maneira.

O documento de índice especificado quando você habilita a hospedagem de site estático é exibido quando os usuários abrem o site e não especificam um arquivo específico `https://contosoblobaccount.z22.web.core.windows.net`(por exemplo:).  

Se o servidor retornar um erro 404 e você não tiver especificado um documento de erro ao habilitar o site, uma página 404 padrão será retornada ao usuário.

> [!NOTE]
> Não há suporte para [CORS](https://docs.microsoft.com/rest/api/storageservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services) com site estático.

## <a name="impact-of-the-setting-the-public-access-level-of-the-web-container"></a>Impacto da configuração do nível de acesso público do contêiner da Web

Você pode modificar o nível de acesso público do contêiner de **$Web** , mas isso não tem impacto sobre o ponto de extremidade de site estático primário porque esses arquivos são servidos por meio de solicitações de acesso anônimo. Isso significa acesso público (somente leitura) a todos os arquivos.

A captura de tela a seguir mostra a configuração de nível de acesso público no portal do Azure:

![Captura de tela mostrando como definir o nível de acesso público no portal](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

Embora o ponto de extremidade de site estático primário não seja afetado, uma alteração no nível de acesso público afeta o ponto de extremidade do serviço blob primário.

Por exemplo, se você alterar o nível de acesso público do contêiner de **$Web** de **privado (sem acesso anônimo)** para **BLOB (acesso de leitura anônimo para BLOBs somente)**, o nível de acesso público ao ponto de extremidade `https://contosoblobaccount.z22.web.core.windows.net/index.html` de site estático primário não será alterado.

No entanto, o acesso público ao ponto de extremidade `https://contosoblobaccount.blob.core.windows.net/$web/index.html` do serviço blob primário muda de particular para público. Agora, os usuários podem abrir esse arquivo usando qualquer um desses dois pontos de extremidade.

## <a name="mapping-a-custom-domain-to-a-static-website-url"></a>Mapeando um domínio personalizado para uma URL de site estático

Você pode tornar seu site estático disponível por meio de um domínio personalizado. 

É mais fácil habilitar o acesso HTTP para seu domínio personalizado, pois o armazenamento do Azure dá suporte nativo a ele. Para habilitar o HTTPS, você precisará usar a CDN do Azure porque o armazenamento do Azure ainda não oferece suporte nativo a HTTPS com domínios personalizados. consulte [mapear um domínio personalizado para um ponto de extremidade do armazenamento de BLOBs do Azure](storage-custom-domain-name.md) para obter orientações passo a passo.

Se a conta de armazenamento estiver configurada para [exigir transferência segura](../common/storage-require-secure-transfer.md) por HTTPS, os usuários deverão usar o ponto de extremidade HTTPS. 

> [!TIP]
> Considere hospedar seu domínio no Azure. Para obter mais informações, consulte [hospedar seu domínio no DNS do Azure](../../dns/dns-delegate-domain-azure-dns.md).

## <a name="adding-http-headers"></a>Adicionando cabeçalhos HTTP

Não há como configurar cabeçalhos como parte do recurso de site estático. No entanto, você pode usar a CDN do Azure para adicionar cabeçalhos e acrescentar (ou substituir) valores de cabeçalho. Consulte [referência do mecanismo de regras padrão para a CDN do Azure](https://docs.microsoft.com/azure/cdn/cdn-standard-rules-engine-reference).

Se você quiser usar cabeçalhos para controlar o cache, consulte [controlar o comportamento de cache da CDN do Azure com regras de cache](https://docs.microsoft.com/azure/cdn/cdn-caching-rules).

## <a name="pricing"></a>Preços

Você pode habilitar a hospedagem de sites estáticos gratuitamente. Você é cobrado apenas pelo armazenamento de BLOBs que seu site utiliza e pelos custos de operações. Para obter mais detalhes sobre preços para Armazenamento de Blob do Azure, confira [Página de Preços do Armazenamento de Blob do Azure](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="metrics"></a>Métricas

Você pode habilitar métricas em páginas de site estáticos. Depois de habilitar as métricas, as estatísticas de tráfego nos arquivos no contêiner de **$Web** são relatadas no painel de métricas.

Para habilitar as métricas em suas páginas de site estáticos, consulte [habilitar métricas em páginas de site estático](storage-blob-static-website-how-to.md#metrics).

## <a name="next-steps"></a>Próximas etapas

* [Hospedar um site estático no armazenamento do Azure](storage-blob-static-website-how-to.md)
* [Mapear um domínio personalizado para um ponto de extremidade do Armazenamento de Blobs do Azure](storage-custom-domain-name.md)
* [Azure Functions](/azure/azure-functions/functions-overview)
* [Serviço de Aplicativo do Azure](/azure/app-service/overview)
* [Compile seu primeiro aplicativo web sem servidor](https://docs.microsoft.com/azure/functions/tutorial-static-website-serverless-api-with-database)
* [Tutorial: Hospedar seu domínio no DNS do Azure](../../dns/dns-delegate-domain-azure-dns.md)
