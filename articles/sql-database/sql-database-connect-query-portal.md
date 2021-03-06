---
title: Consultar um Banco de Dados SQL usando o editor de consultas no portal do Azure
description: Saiba como usar o Editor de Consultas para executar consultas T-SQL (Transact-SQL) em um Banco de Dados SQL do Azure.
keywords: conectar-se ao banco de dados sql, consultar o banco de dados sql, portal do azure, portal, editor de consultas
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: Ninarn
ms.author: ninarn
ms.reviewer: carlrab
ms.date: 03/12/2020
ms.openlocfilehash: c072161db7a477b7973571a18d4f686b1b9c6202
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80985670"
---
# <a name="quickstart-use-the-azure-portals-query-editor-to-query-a-sql-database"></a>Início Rápido: Usar o editor de consultas do portal do Azure para consultar um banco de dados SQL

O editor de consultas é uma ferramenta no portal do Azure para executar consultas SQL em seu banco de dados SQL do Azure ou no SQL Data Warehouse do Azure. 

Neste início rápido, você usará o editor de consultas para executar consultas T-SQL (Transact-SQL) em um banco de dados SQL do Azure.


## <a name="prerequisites"></a>Pré-requisitos

A conclusão deste início rápido requer o banco de dados de exemplo AdventureWorksLT. Se você não tiver uma cópia funcional do banco de dados SQL AdventureWorksLT, o início rápido a seguir criará rapidamente uma:

- [Início Rápido: Criar um banco de dados SQL do Azure individual usando o portal do Azure, o PowerShell ou a CLI do Azure](sql-database-single-database-get-started.md) 

### <a name="configure-network-settings"></a>Definir as configurações de rede

Se você receber um dos seguintes erros no editor de consultas: *As configurações de rede local poderão estar impedindo que o Editor de Consultas emita consultas. Clique aqui para obter instruções sobre como definir as configurações de rede* ou *Não foi possível estabelecer uma conexão com o servidor. Isso pode indicar um problema com a configuração de firewall local ou as configurações de proxy de rede*, as seguintes informações importantes devem ajudar a resolver:

> [!IMPORTANT]
> O editor de consultas usa as portas 443 e 1443 para se comunicar. Verifique se você habilitou o tráfego HTTPS de saída nessas portas. Você também precisa [adicionar o endereço IP de saída para regras de firewall permitido do servidor](sql-database-server-level-firewall-rule.md) para acessar seus bancos de dados e data warehouses.


## <a name="open-the-sql-database-query-editor"></a>Abrir o Editor de Consultas do Banco de Dados SQL

1. Entre no [portal do Azure](https://portal.azure.com/) e selecione o banco de dados SQL que você deseja consultar.

2. No menu **Banco de dados SQL**, selecione **Editor de consultas (versão prévia)** .

    ![localizar editor de consultas](./media/sql-database-connect-query-portal/find-query-editor.PNG)


## <a name="establish-a-connection-to-the-database"></a>Estabelecer uma conexão com o banco de dados

Embora você tenha entrado no portal, você ainda precisará fornecer credenciais para acessar o banco de dados SQL. Você pode se conectar usando a autenticação do SQL ou o Azure Active Directory para se conectar ao banco de dados.

### <a name="connect-using-sql-authentication"></a>Conectar-se usando Autenticação SQL

1. Na página **Logon**, em **Autenticação do SQL Server**, insira um **Logon** e uma **Senha** para um usuário que tenha acesso ao banco de dados. Se você não tiver certeza, use o logon e a senha do administrador do servidor do banco de dados.

    ![entrar](./media/sql-database-connect-query-portal/login-menu.png)

2. Selecione **OK**.


### <a name="connect-using-azure-active-directory"></a>Conectar-se usando o Azure Active Directory

Configurar um administrador do Azure AD (Azure Active Directory) permite que você use uma única identidade para entrar no portal do Azure e no seu banco de dados SQL. Para se conectar ao seu banco de dados usando o Azure AD, siga as etapas abaixo para configurar um administrador do Azure AD para o servidor SQL.

> [!NOTE]
> * Ainda não há suporte para contas de email (por exemplo, outlook.com, gmail.com, yahoo.com e assim por diante) serem usadas como administradores do Azure AD. Escolha um usuário criado nativamente no Azure AD ou federado no Azure AD.
> * A conexão do administrador do Azure AD não funciona com contas que têm a autenticação de dois fatores habilitada.

#### <a name="set-an-active-directory-admin-for-the-database-server"></a>Definir um administrador do Active Directory para o servidor de banco de dados

1. No portal do Azure, selecione seu servidor SQL.

2. No menu **Servidor SQL**, selecione **Administrador do Active Directory**.

3. Na barra de ferramentas da página do **Administrador do Active Directory** do servidor SQL, selecione **Definir administrador** e escolha o usuário ou o grupo como o administrador do Azure AD.

    ![selecionar active directory](./media/sql-database-connect-query-portal/select-active-directory.png)

4. Na página **Adicionar administrador**, na caixa de pesquisa, insira um usuário ou grupo a ser localizado, selecione-o como administrador e, em seguida, escolha o botão **Selecionar**.

5. De volta à barra de ferramentas da página de **administrador do Active Directory** do SQL Server, selecione **salvar**.

### <a name="connect-to-the-database"></a>Conectar-se ao banco de dados

6. No menu **SQL Server**, selecione **bancos de dados SQL** e, em seguida, selecione o seu banco de dados SQL.

7. No menu **Banco de dados SQL**, selecione **Editor de consultas (versão prévia)** . Na página **Logon**, no rótulo **autenticação do Active Directory**, uma mensagem será exibida informando que você foi conectado se você for um administrador do Azure AD. Em seguida, selecione o botão **Continuar como** *\<sua ID de usuário ou grupo>* . Se a página indicar que você não fez logon com êxito, talvez seja necessário atualizá-la.

## <a name="query-a-sql-database"></a>Consultar um banco de dados SQL

As consultas de exemplo a seguir devem ser executadas com êxito no banco de dados de exemplo AdventureWorksLT.

### <a name="run-a-select-query"></a>Executar uma consulta SELECT

1. Cole a consulta a seguir no editor de consultas:

   ```sql
    SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
    FROM SalesLT.ProductCategory pc
    JOIN SalesLT.Product p
    ON pc.productcategoryid = p.productcategoryid;
   ```

2. Selecione **Executar** e, em seguida, examine a saída no painel **Resultados**.

   ![resultados do editor de consultas](./media/sql-database-connect-query-portal/query-editor-results.png)

3. Opcionalmente, você pode salvar a consulta como um arquivo .sql ou exportar os dados retornados como um arquivo .json, .csv ou .xml.

### <a name="run-an-insert-query"></a>Executar uma consulta INSERT

Execute a instrução T-SQL [INSERT](/sql/t-sql/statements/insert-transact-sql/) a seguir para adicionar um novo produto à tabela `SalesLT.Product`.

1. Substitua a consulta anterior por esta.

    ```sql
    INSERT INTO [SalesLT].[Product]
           ( [Name]
           , [ProductNumber]
           , [Color]
           , [ProductCategoryID]
           , [StandardCost]
           , [ListPrice]
           , [SellStartDate]
           )
    VALUES
           ('myNewProduct'
           ,123456789
           ,'NewColor'
           ,1
           ,100
           ,100
           ,GETDATE() );
   ```


2. Selecione **Executar** para inserir uma nova linha na tabela `Product`. O painel **Mensagens** exibe **Êxito na consulta: Linhas afetadas: 1**.


### <a name="run-an-update-query"></a>Executar uma consulta UPDATE

Execute a instrução T-SQL [UPDATE](/sql/t-sql/queries/update-transact-sql/) a seguir para modificar o novo produto.

1. Substitua a consulta anterior por esta.

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. Selecione **Executar** para atualizar a linha especificada na tabela `Product`. O painel **Mensagens** exibe **Êxito na consulta: Linhas afetadas: 1**.

### <a name="run-a-delete-query"></a>Executar uma consulta DELETE

Execute a instrução T-SQL [DELETE](/sql/t-sql/statements/delete-transact-sql/) a seguir para remover o novo produto.

1. Substitua a consulta anterior por esta:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. Selecione **Executar** para excluir a linha especificada da tabela `Product`. O painel **Mensagens** exibe **Êxito na consulta: Linhas afetadas: 1**.


## <a name="query-editor-considerations"></a>Considerações sobre o editor de consultas

Há algumas coisas que se deve saber ao trabalhar com o editor de consultas.

* O editor de consultas usa as portas 443 e 1443 para se comunicar. Verifique se você habilitou o tráfego HTTPS de saída nessas portas. Você também precisará adicionar o endereço IP de saída para regras de firewall permitido do servidor para acessar seus bancos de dados e data warehouses.

* Se você tiver uma conexão do Link Privado, o Editor de Consultas funcionará sem precisar adicionar o endereço IP do cliente ao firewall do Banco de Dados SQL

* Pressionar **F5** atualiza a página do editor de consultas e qualquer consulta em que se esteja trabalhando é perdida.

* O editor de consultas não dá suporte à conexão com o banco de dados `master`.

* Há um tempo limite de 5 minutos para a execução da consulta.

* O editor de consultas só dá suporte à projeção cilíndrica para tipos de dados geográficos.

* Não há suporte para IntelliSense para tabelas e exibições de banco de dados, mas o editor dá suporte ao preenchimento automático em nomes que já foram digitados.




## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre o T-SQL (Transact-SQL) com suporte em bancos de dados SQL do Azure, confira [Resolver diferenças do Transact-SQL durante a migração para o Banco de Dados SQL](sql-database-transact-sql-information.md).
