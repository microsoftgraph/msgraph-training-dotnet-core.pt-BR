<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você criará um novo aplicativo do Azure AD usando o Azure Active Directory de administração.

1. Abra um navegador, navegue até o [centro de administração do Azure Active Directory](https://aad.portal.azure.com) e faça logon usando uma **conta pessoal** (também conhecida como conta da Microsoft) ou **Conta Corporativa ou de Estudante**.

1. Selecione **Azure Active Directory** na navegação esquerda e selecione **Registros de aplicativos** em **Gerenciar**.

    ![Uma captura de tela dos registros do aplicativo ](./images/aad-portal-app-registrations.png)

1. Selecione **Novo registro**. Na página **Registrar um aplicativo**, defina os valores da seguinte forma.

    - Defina **Nome** para `.NET Core Graph Tutorial`.
    - Defina **Tipos de conta com suporte** para **Contas em qualquer diretório organizacional e contas pessoais da Microsoft**.
    - Em **URI de** redirecionamento, altere o menu suspenso para Cliente Público (área de trabalho **móvel &)** e de definir o valor como `https://login.microsoftonline.com/common/oauth2/nativeclient` .

    ![Captura de tela da página Registrar um aplicativo](./images/aad-register-an-app.png)

1. Selecione **Registrar**. Na página **Tutorial do .NET Core Graph,** copie o valor da ID do Aplicativo **(cliente)** e salve-a, você precisará dele na próxima etapa.

    ![Captura de tela da ID do aplicativo do novo registro do aplicativo](./images/aad-application-id.png)

1. Selecione **Autenticação** em **Gerenciar**. Localize **a seção Configurações Avançadas** e **altere a opção** Permitir fluxos de cliente público para **Sim** e, em seguida, **escolha Salvar**.

    ![Uma captura de tela da alternância Permitir fluxos de cliente público](./images/aad-default-client-type.png)
