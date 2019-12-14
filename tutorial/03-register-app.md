<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você criará um novo aplicativo do Azure AD usando o centro de administração do Azure Active Directory.

1. Abra um navegador, navegue até o [centro de administração do Azure Active Directory](https://aad.portal.azure.com) e faça logon usando uma **conta pessoal** (também conhecida como conta da Microsoft) ou **Conta Corporativa ou de Estudante**.

1. Selecione **Azure Active Directory** na navegação esquerda e selecione **Registros de aplicativos** em **Gerenciar**.

    ![Uma captura de tela dos registros de aplicativo ](./images/aad-portal-app-registrations.png)

1. Selecione **Novo registro**. Na página **Registrar um aplicativo**, defina os valores da seguinte forma.

    - Defina **Nome** para `.NET Core Graph Tutorial`.
    - Defina **Tipos de conta com suporte** para **Contas em qualquer diretório organizacional e contas pessoais da Microsoft**.
    - Deixe o **URI de Redirecionamento** vazio.

    ![Uma captura de tela da página registrar um aplicativo](./images/aad-register-an-app.png)

1. Selecione **registrar**. Na página do **tutorial do gráfico do .NET Core** , copie o valor da **ID do aplicativo (cliente)** e salve-o, você precisará dele na próxima etapa.

    ![Uma captura de tela da ID do aplicativo do novo registro de aplicativo](./images/aad-application-id.png)

1. Selecione o link **Adicionar um URI de redirecionamento** . Na página **redirecionar URIs** , localize a seção **redirecionar URIs sugeridos para clientes públicos (móvel, área de trabalho)** . Selecione o `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.

    ![Captura de tela da página URIs de redirecionamento](./images/aad-redirect-uris.png)

1. Localize a seção **tipo de cliente padrão** e altere o **aplicativo tratar como um cliente público** alternar para **Sim**e, em seguida, escolha **salvar**.

    ![Uma captura de tela da seção tipo de cliente padrão](./images/aad-default-client-type.png)
