# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="1340d-101">Como executar o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="1340d-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1340d-102">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="1340d-102">Prerequisites</span></span>

<span data-ttu-id="1340d-103">Para executar o projeto concluído nessa pasta, você precisará do seguinte:</span><span class="sxs-lookup"><span data-stu-id="1340d-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="1340d-104">O [SDK do .NET Core](https://dotnet.microsoft.com/download) instalado em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="1340d-104">The [.NET Core SDK](https://dotnet.microsoft.com/download) installed on your development machine.</span></span> <span data-ttu-id="1340d-105">(**Observação:** este tutorial foi escrito com o .NET Core SDK versão 2.2.401.</span><span class="sxs-lookup"><span data-stu-id="1340d-105">(**Note:** This tutorial was written with .NET Core SDK version 2.2.401.</span></span> <span data-ttu-id="1340d-106">As etapas deste guia podem funcionar com outras versões, mas que não foram testadas.</span><span class="sxs-lookup"><span data-stu-id="1340d-106">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="1340d-107">Uma conta corporativa ou de estudante da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1340d-107">A Microsoft work or school account.</span></span>

<span data-ttu-id="1340d-108">Se você não tiver uma conta da Microsoft, poderá [se inscrever no programa para desenvolvedores do office 365](https://developer.microsoft.com/office/dev-program) para obter uma assinatura gratuita do Office 365.</span><span class="sxs-lookup"><span data-stu-id="1340d-108">If you don't have a Microsoft account, you can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="1340d-109">Registrar um aplicativo Web com o centro de administração do Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1340d-109">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="1340d-110">Abra um navegador, navegue até o [centro de administração do Azure Active Directory](https://aad.portal.azure.com) e faça logon usando uma **conta pessoal** (também conhecida como conta da Microsoft) ou **Conta Corporativa ou de Estudante**.</span><span class="sxs-lookup"><span data-stu-id="1340d-110">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="1340d-111">Selecione **Azure Active Directory** na navegação à esquerda e, em seguida, selecione **registros de aplicativo** em **gerenciar**.</span><span class="sxs-lookup"><span data-stu-id="1340d-111">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="1340d-112">Uma captura de tela dos registros de aplicativo</span><span class="sxs-lookup"><span data-stu-id="1340d-112">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="1340d-113">Selecione **Novo registro**.</span><span class="sxs-lookup"><span data-stu-id="1340d-113">Select **New registration**.</span></span> <span data-ttu-id="1340d-114">Na página **Registrar um aplicativo**, defina os valores da seguinte forma.</span><span class="sxs-lookup"><span data-stu-id="1340d-114">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="1340d-115">Defina **Nome** para `.NET Core Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="1340d-115">Set **Name** to `.NET Core Graph Tutorial`.</span></span>
    - <span data-ttu-id="1340d-116">Defina os **tipos de conta com suporte** para **contas em qualquer diretório organizacional**.</span><span class="sxs-lookup"><span data-stu-id="1340d-116">Set **Supported account types** to **Accounts in any organizational directory**.</span></span>
    - <span data-ttu-id="1340d-117">Deixe o **URI de Redirecionamento** vazio.</span><span class="sxs-lookup"><span data-stu-id="1340d-117">Leave **Redirect URI** empty.</span></span>

    ![Uma captura de tela da página registrar um aplicativo](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="1340d-119">Selecione **registrar**.</span><span class="sxs-lookup"><span data-stu-id="1340d-119">Select **Register**.</span></span> <span data-ttu-id="1340d-120">Na página do **tutorial do gráfico do .NET Core** , copie o valor da **ID do aplicativo (cliente)** e salve-o, você precisará dele na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="1340d-120">On the **.NET Core Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Uma captura de tela da ID do aplicativo do novo registro de aplicativo](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="1340d-122">Selecione o link **Adicionar um URI de redirecionamento** .</span><span class="sxs-lookup"><span data-stu-id="1340d-122">Select the **Add a Redirect URI** link.</span></span> <span data-ttu-id="1340d-123">Na página **redirecionar URIs** , localize a seção **redirecionar URIs sugeridos para clientes públicos (móvel, área de trabalho)** .</span><span class="sxs-lookup"><span data-stu-id="1340d-123">On the **Redirect URIs** page, locate the **Suggested Redirect URIs for public clients (mobile, desktop)** section.</span></span> <span data-ttu-id="1340d-124">Selecione o `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span><span class="sxs-lookup"><span data-stu-id="1340d-124">Select the `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span></span>

    ![Captura de tela da página URIs de redirecionamento](/tutorial/images/aad-redirect-uris.png)

1. <span data-ttu-id="1340d-126">Localize a seção **tipo de cliente padrão** e altere o **aplicativo tratar como um cliente público** alternar para **Sim**e, em seguida, escolha **salvar**.</span><span class="sxs-lookup"><span data-stu-id="1340d-126">Locate the **Default client type** section and change the **Treat application as a public client** toggle to **Yes**, then choose **Save**.</span></span>

    ![Uma captura de tela da seção tipo de cliente padrão](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a><span data-ttu-id="1340d-128">Configurar o exemplo</span><span class="sxs-lookup"><span data-stu-id="1340d-128">Configure the sample</span></span>

1. <span data-ttu-id="1340d-129">Renomear `appsettings.json.example` o `appsettings.json`arquivo para.</span><span class="sxs-lookup"><span data-stu-id="1340d-129">Rename the `appsettings.json.example` file to `appsettings.json`.</span></span>
1. <span data-ttu-id="1340d-130">Edite `appsettings.json` o arquivo e faça as seguintes alterações.</span><span class="sxs-lookup"><span data-stu-id="1340d-130">Edit the `appsettings.json` file and make the following changes.</span></span>
    1. <span data-ttu-id="1340d-131">Substitua `YOUR_APP_ID_HERE` pela **ID do aplicativo** obtida do portal de registro do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1340d-131">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>

## <a name="build-and-run-the-sample"></a><span data-ttu-id="1340d-132">Criar e executar o exemplo</span><span class="sxs-lookup"><span data-stu-id="1340d-132">Build and run the sample</span></span>

<span data-ttu-id="1340d-133">Na sua interface de linha de comando (CLI), navegue até o diretório do projeto e execute os seguintes comandos.</span><span class="sxs-lookup"><span data-stu-id="1340d-133">In your command-line interface (CLI), navigate to the project directory and run the following commands.</span></span>

```Shell
dotnet restore
dotnet build
dotnet run
```
