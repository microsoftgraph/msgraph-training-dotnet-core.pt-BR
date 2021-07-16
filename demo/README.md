# <a name="how-to-run-the-completed-project"></a>Como executar o projeto concluído

## <a name="prerequisites"></a>Pré-requisitos

Para executar o projeto concluído nesta pasta, você precisa do seguinte:

- O [SDK do .NET Core](https://dotnet.microsoft.com/download) instalado em sua máquina de desenvolvimento. (**Observação:** este tutorial foi escrito com o .NET Core SDK versão 3.1.402. As etapas neste guia podem funcionar com outras versões, mas que não foram testadas.)
- Uma conta de trabalho ou de estudante da Microsoft.

Se você não tiver uma conta [](https://developer.microsoft.com/office/dev-program) da Microsoft, poderá inscrever-se no programa de desenvolvedor Office 365 para obter uma assinatura Office 365 gratuita.

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a>Registrar um aplicativo Web com o Azure Active Directory de administração

1. Abra um navegador, navegue até o [centro de administração do Azure Active Directory](https://aad.portal.azure.com) e faça logon usando uma **conta pessoal** (também conhecida como conta da Microsoft) ou **Conta Corporativa ou de Estudante**.

1. Selecione **Azure Active Directory** na navegação esquerda e selecione **Registros de aplicativos** em **Gerenciar**.

    ![Uma captura de tela dos registros do aplicativo ](/tutorial/images/aad-portal-app-registrations.png)

1. Selecione **Novo registro**. Na página **Registrar um aplicativo**, defina os valores da seguinte forma.

    - Defina **Nome** para `.NET Core Graph Tutorial`.
    - Definir **tipos de conta com suporte** como Contas em qualquer diretório **organizacional**.
    - Deixe o **URI de Redirecionamento** vazio.

    ![Captura de tela da página Registrar um aplicativo](/tutorial/images/aad-register-an-app.png)

1. Selecione **Registrar**. Na página **Tutorial do .NET Core Graph,** copie o valor da ID do Aplicativo **(cliente)** e salve-a, você precisará dele na próxima etapa.

    ![Captura de tela da ID do aplicativo do novo registro do aplicativo](/tutorial/images/aad-application-id.png)

1. Selecione o **link Adicionar um URI de** redirecionamento. Na página **URIs de redirecionamento,** localize a seção URIs de redirecionamento sugeridos para clientes **públicos (móveis, desktop).** Selecione o `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.

    ![Uma captura de tela da página URIs de redirecionamento](/tutorial/images/aad-redirect-uris.png)

1. Localize **a seção Configurações Avançadas** e **altere a opção** Permitir fluxos de cliente público para **Sim** e, em seguida, **escolha Salvar**.

    ![Uma captura de tela da seção Tipo de cliente padrão](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a>Configurar o exemplo

1. Inicialize o armazenamento secreto de desenvolvimento do [.NET](https://docs.microsoft.com/aspnet/core/security/app-secrets) abrindo sua CLI no diretório que contém **GraphTutorial.csproj** e executando o seguinte comando.

    ```Shell
    dotnet user-secrets init
    ```

1. Adicione a ID do aplicativo e uma lista de escopos necessários ao armazenamento secreto usando os comandos a seguir. Substitua `YOUR_APP_ID_HERE` pela ID do aplicativo que você criou no portal do Azure.

    ```Shell
    dotnet user-secrets set appId "YOUR_APP_ID_HERE"
    dotnet user-secrets set scopes "User.Read;Calendars.Read"
    ```

## <a name="build-and-run-the-sample"></a>Criar e executar o exemplo

Em sua interface de linha de comando (CLI), navegue até o diretório do projeto e execute os comandos a seguir.

```Shell
dotnet restore
dotnet build
dotnet run
```
