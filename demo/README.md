# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="c30b1-101">Como executar o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="c30b1-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c30b1-102">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="c30b1-102">Prerequisites</span></span>

<span data-ttu-id="c30b1-103">Para executar o projeto concluído nesta pasta, você precisa do seguinte:</span><span class="sxs-lookup"><span data-stu-id="c30b1-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="c30b1-104">O [SDK do .NET Core](https://dotnet.microsoft.com/download) instalado em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="c30b1-104">The [.NET Core SDK](https://dotnet.microsoft.com/download) installed on your development machine.</span></span> <span data-ttu-id="c30b1-105">(**Observação:** este tutorial foi escrito com o .NET Core SDK versão 3.1.402.</span><span class="sxs-lookup"><span data-stu-id="c30b1-105">(**Note:** This tutorial was written with .NET Core SDK version 3.1.402.</span></span> <span data-ttu-id="c30b1-106">As etapas neste guia podem funcionar com outras versões, mas que não foram testadas.)</span><span class="sxs-lookup"><span data-stu-id="c30b1-106">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="c30b1-107">Uma conta de trabalho ou de estudante da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="c30b1-107">A Microsoft work or school account.</span></span>

<span data-ttu-id="c30b1-108">Se você não tiver uma conta [](https://developer.microsoft.com/office/dev-program) da Microsoft, poderá inscrever-se no programa de desenvolvedor Office 365 para obter uma assinatura Office 365 gratuita.</span><span class="sxs-lookup"><span data-stu-id="c30b1-108">If you don't have a Microsoft account, you can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="c30b1-109">Registrar um aplicativo Web com o Azure Active Directory de administração</span><span class="sxs-lookup"><span data-stu-id="c30b1-109">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="c30b1-110">Abra um navegador, navegue até o [centro de administração do Azure Active Directory](https://aad.portal.azure.com) e faça logon usando uma **conta pessoal** (também conhecida como conta da Microsoft) ou **Conta Corporativa ou de Estudante**.</span><span class="sxs-lookup"><span data-stu-id="c30b1-110">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="c30b1-111">Selecione **Azure Active Directory** na navegação esquerda e selecione **Registros de aplicativos** em **Gerenciar**.</span><span class="sxs-lookup"><span data-stu-id="c30b1-111">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="c30b1-112">Uma captura de tela dos registros do aplicativo</span><span class="sxs-lookup"><span data-stu-id="c30b1-112">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="c30b1-113">Selecione **Novo registro**.</span><span class="sxs-lookup"><span data-stu-id="c30b1-113">Select **New registration**.</span></span> <span data-ttu-id="c30b1-114">Na página **Registrar um aplicativo**, defina os valores da seguinte forma.</span><span class="sxs-lookup"><span data-stu-id="c30b1-114">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="c30b1-115">Defina **Nome** para `.NET Core Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="c30b1-115">Set **Name** to `.NET Core Graph Tutorial`.</span></span>
    - <span data-ttu-id="c30b1-116">Definir **tipos de conta com suporte** como Contas em qualquer diretório **organizacional**.</span><span class="sxs-lookup"><span data-stu-id="c30b1-116">Set **Supported account types** to **Accounts in any organizational directory**.</span></span>
    - <span data-ttu-id="c30b1-117">Deixe o **URI de Redirecionamento** vazio.</span><span class="sxs-lookup"><span data-stu-id="c30b1-117">Leave **Redirect URI** empty.</span></span>

    ![Captura de tela da página Registrar um aplicativo](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="c30b1-119">Selecione **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="c30b1-119">Select **Register**.</span></span> <span data-ttu-id="c30b1-120">Na página **Tutorial do .NET Core Graph,** copie o valor da ID do Aplicativo **(cliente)** e salve-a, você precisará dele na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="c30b1-120">On the **.NET Core Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Captura de tela da ID do aplicativo do novo registro do aplicativo](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="c30b1-122">Selecione o **link Adicionar um URI de** redirecionamento.</span><span class="sxs-lookup"><span data-stu-id="c30b1-122">Select the **Add a Redirect URI** link.</span></span> <span data-ttu-id="c30b1-123">Na página **URIs de redirecionamento,** localize a seção URIs de redirecionamento sugeridos para clientes **públicos (móveis, desktop).**</span><span class="sxs-lookup"><span data-stu-id="c30b1-123">On the **Redirect URIs** page, locate the **Suggested Redirect URIs for public clients (mobile, desktop)** section.</span></span> <span data-ttu-id="c30b1-124">Selecione o `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span><span class="sxs-lookup"><span data-stu-id="c30b1-124">Select the `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span></span>

    ![Uma captura de tela da página URIs de redirecionamento](/tutorial/images/aad-redirect-uris.png)

1. <span data-ttu-id="c30b1-126">Localize **a seção Configurações Avançadas** e **altere a opção** Permitir fluxos de cliente público para **Sim** e, em seguida, **escolha Salvar**.</span><span class="sxs-lookup"><span data-stu-id="c30b1-126">Locate the **Advanced settings** section and change the **Allow public client flows** toggle to **Yes**, then choose **Save**.</span></span>

    ![Uma captura de tela da seção Tipo de cliente padrão](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a><span data-ttu-id="c30b1-128">Configurar o exemplo</span><span class="sxs-lookup"><span data-stu-id="c30b1-128">Configure the sample</span></span>

1. <span data-ttu-id="c30b1-129">Inicialize o armazenamento secreto de desenvolvimento do [.NET](https://docs.microsoft.com/aspnet/core/security/app-secrets) abrindo sua CLI no diretório que contém **GraphTutorial.csproj** e executando o seguinte comando.</span><span class="sxs-lookup"><span data-stu-id="c30b1-129">Initialize the [.NET development secret store](https://docs.microsoft.com/aspnet/core/security/app-secrets) by opening your CLI in the directory that contains **GraphTutorial.csproj** and running the following command.</span></span>

    ```Shell
    dotnet user-secrets init
    ```

1. <span data-ttu-id="c30b1-130">Adicione a ID do aplicativo e uma lista de escopos necessários ao armazenamento secreto usando os comandos a seguir.</span><span class="sxs-lookup"><span data-stu-id="c30b1-130">Add your application ID and a list of required scopes to the secret store using the following commands.</span></span> <span data-ttu-id="c30b1-131">Substitua `YOUR_APP_ID_HERE` pela ID do aplicativo que você criou no portal do Azure.</span><span class="sxs-lookup"><span data-stu-id="c30b1-131">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    ```Shell
    dotnet user-secrets set appId "YOUR_APP_ID_HERE"
    dotnet user-secrets set scopes "User.Read;Calendars.Read"
    ```

## <a name="build-and-run-the-sample"></a><span data-ttu-id="c30b1-132">Criar e executar o exemplo</span><span class="sxs-lookup"><span data-stu-id="c30b1-132">Build and run the sample</span></span>

<span data-ttu-id="c30b1-133">Em sua interface de linha de comando (CLI), navegue até o diretório do projeto e execute os comandos a seguir.</span><span class="sxs-lookup"><span data-stu-id="c30b1-133">In your command-line interface (CLI), navigate to the project directory and run the following commands.</span></span>

```Shell
dotnet restore
dotnet build
dotnet run
```
