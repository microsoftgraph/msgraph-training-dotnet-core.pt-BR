<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="a6f43-101">Neste exercício, você criará um novo aplicativo do Azure AD usando o Azure Active Directory de administração.</span><span class="sxs-lookup"><span data-stu-id="a6f43-101">In this exercise you will create a new Azure AD application using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="a6f43-102">Abra um navegador, navegue até o [centro de administração do Azure Active Directory](https://aad.portal.azure.com) e faça logon usando uma **conta pessoal** (também conhecida como conta da Microsoft) ou **Conta Corporativa ou de Estudante**.</span><span class="sxs-lookup"><span data-stu-id="a6f43-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="a6f43-103">Selecione **Azure Active Directory** na navegação esquerda e selecione **Registros de aplicativos** em **Gerenciar**.</span><span class="sxs-lookup"><span data-stu-id="a6f43-103">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="a6f43-104">Uma captura de tela dos registros do aplicativo</span><span class="sxs-lookup"><span data-stu-id="a6f43-104">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="a6f43-105">Selecione **Novo registro**.</span><span class="sxs-lookup"><span data-stu-id="a6f43-105">Select **New registration**.</span></span> <span data-ttu-id="a6f43-106">Na página **Registrar um aplicativo**, defina os valores da seguinte forma.</span><span class="sxs-lookup"><span data-stu-id="a6f43-106">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="a6f43-107">Defina **Nome** para `.NET Core Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="a6f43-107">Set **Name** to `.NET Core Graph Tutorial`.</span></span>
    - <span data-ttu-id="a6f43-108">Defina **Tipos de conta com suporte** para **Contas em qualquer diretório organizacional e contas pessoais da Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="a6f43-108">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="a6f43-109">Em **URI de** redirecionamento, altere o menu suspenso para Cliente Público (área de trabalho **móvel &)** e de definir o valor como `https://login.microsoftonline.com/common/oauth2/nativeclient` .</span><span class="sxs-lookup"><span data-stu-id="a6f43-109">Under **Redirect URI**, change the dropdown to **Public client (mobile & desktop)**, and set the value to `https://login.microsoftonline.com/common/oauth2/nativeclient`.</span></span>

    ![Captura de tela da página Registrar um aplicativo](./images/aad-register-an-app.png)

1. <span data-ttu-id="a6f43-111">Selecione **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="a6f43-111">Select **Register**.</span></span> <span data-ttu-id="a6f43-112">Na página **Tutorial do .NET Core Graph,** copie o valor da ID do Aplicativo **(cliente)** e salve-a, você precisará dele na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="a6f43-112">On the **.NET Core Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Captura de tela da ID do aplicativo do novo registro do aplicativo](./images/aad-application-id.png)

1. <span data-ttu-id="a6f43-114">Selecione **Autenticação** em **Gerenciar**.</span><span class="sxs-lookup"><span data-stu-id="a6f43-114">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="a6f43-115">Localize **a seção Configurações Avançadas** e **altere a opção** Permitir fluxos de cliente público para **Sim** e, em seguida, **escolha Salvar**.</span><span class="sxs-lookup"><span data-stu-id="a6f43-115">Locate the **Advanced settings** section and change the **Allow public client flows** toggle to **Yes**, then choose **Save**.</span></span>

    ![Uma captura de tela da alternância Permitir fluxos de cliente público](./images/aad-default-client-type.png)
