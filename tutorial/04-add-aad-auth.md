<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="d3679-101">Neste exercício, você estenderá o aplicativo do exercício anterior para oferecer suporte à autenticação com o Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d3679-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="d3679-102">Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="d3679-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="d3679-103">Nesta etapa, você integrará a [biblioteca de autenticação da Microsoft (MSAL) para .net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d3679-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) into the application.</span></span>

1. <span data-ttu-id="d3679-104">Inicialize o [repositório de segredo de desenvolvimento do .net](/aspnet/core/security/app-secrets) abrindo sua CLI no diretório que contém **GraphTutorial. csproj** e executando o comando a seguir.</span><span class="sxs-lookup"><span data-stu-id="d3679-104">Initialize the [.NET development secret store](/aspnet/core/security/app-secrets) by opening your CLI in the directory that contains **GraphTutorial.csproj** and running the following command.</span></span>

    ```Shell
    dotnet user-secrets init
    ```

1. <span data-ttu-id="d3679-105">Adicione a ID do aplicativo e uma lista de escopos necessários para o repositório secreto usando os seguintes comandos.</span><span class="sxs-lookup"><span data-stu-id="d3679-105">Add your application ID and a list of required scopes to the secret store using the following commands.</span></span> <span data-ttu-id="d3679-106">Substitua `YOUR_APP_ID_HERE` pela ID do aplicativo que você criou no portal do Azure.</span><span class="sxs-lookup"><span data-stu-id="d3679-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    ```Shell
    dotnet user-secrets set appId "YOUR_APP_ID_HERE"
    dotnet user-secrets set scopes "User.Read;MailboxSettings.Read;Calendars.ReadWrite"
    ```

    <span data-ttu-id="d3679-107">Vamos examinar os escopos de permissão que você acabou de definir.</span><span class="sxs-lookup"><span data-stu-id="d3679-107">Let's look at the permission scopes you just set.</span></span>

    - <span data-ttu-id="d3679-108">**User. Read** permitirá que o aplicativo Leia o perfil do usuário conectado para obter informações como nome para exibição e endereço de email.</span><span class="sxs-lookup"><span data-stu-id="d3679-108">**User.Read** will allow the app to read the signed-in user's profile to get information such as display name and email address.</span></span>
    - <span data-ttu-id="d3679-109">**MailboxSettings. Read** permitirá que o aplicativo Leia o fuso horário, o formato de data e o formato de hora preferencial do usuário.</span><span class="sxs-lookup"><span data-stu-id="d3679-109">**MailboxSettings.Read** will allow the app to read the user's preferred time zone, date format, and time format.</span></span>
    - <span data-ttu-id="d3679-110">**Calendars. ReadWrite** permitirá que o aplicativo Leia os eventos existentes no calendário do usuário e adicione novos eventos.</span><span class="sxs-lookup"><span data-stu-id="d3679-110">**Calendars.ReadWrite** will allow the app to read the existing events on the user's calendar and add new events.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="d3679-111">Implementar logon</span><span class="sxs-lookup"><span data-stu-id="d3679-111">Implement sign-in</span></span>

<span data-ttu-id="d3679-112">Nesta seção, você criará um provedor de autenticação que pode ser usado com o SDK do Graph e também pode ser usado para solicitar explicitamente um token de acesso usando o [fluxo do código de dispositivo](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code).</span><span class="sxs-lookup"><span data-stu-id="d3679-112">In this section you will create an authentication provider that can be used with the Graph SDK and can also be used to explicitly request an access token by using the [device code flow](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code).</span></span>

### <a name="create-an-authentication-provider"></a><span data-ttu-id="d3679-113">Criar um provedor de autenticação</span><span class="sxs-lookup"><span data-stu-id="d3679-113">Create an authentication provider</span></span>

1. <span data-ttu-id="d3679-114">Crie um novo diretório no diretório **GraphTutorial** chamado **autenticação**.</span><span class="sxs-lookup"><span data-stu-id="d3679-114">Create a new directory in the **GraphTutorial** directory named **Authentication**.</span></span>
1. <span data-ttu-id="d3679-115">Crie um novo arquivo no diretório de **autenticação** chamado **DeviceCodeAuthProvider.cs** e adicione o código a seguir ao arquivo.</span><span class="sxs-lookup"><span data-stu-id="d3679-115">Create a new file in the **Authentication** directory named **DeviceCodeAuthProvider.cs** and add the following code to that file.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Authentication/DeviceCodeAuthProvider.cs" id="AuthProviderSnippet":::

<span data-ttu-id="d3679-116">Considere o que esse código faz.</span><span class="sxs-lookup"><span data-stu-id="d3679-116">Consider what this code does.</span></span>

- <span data-ttu-id="d3679-117">Ele usa a `IPublicClientApplication` implementação MSAL para solicitar e gerenciar tokens.</span><span class="sxs-lookup"><span data-stu-id="d3679-117">It uses the MSAL `IPublicClientApplication` implementation to request and manage tokens.</span></span>
- <span data-ttu-id="d3679-118">A `GetAccessToken` função:</span><span class="sxs-lookup"><span data-stu-id="d3679-118">The `GetAccessToken` function:</span></span>
  - <span data-ttu-id="d3679-119">Entra no usuário se ainda não estiverem conectados usando o fluxo de código de dispositivo.</span><span class="sxs-lookup"><span data-stu-id="d3679-119">Signs in the user if they are not already signed in using the device code flow.</span></span>
  - <span data-ttu-id="d3679-120">Garante que o token retornado seja sempre atualizado usando a `AcquireTokenSilent` função, que retorna o token em cache se ele não tiver expirado e atualiza o token se ele tiver expirado.</span><span class="sxs-lookup"><span data-stu-id="d3679-120">Ensures that the token returned is always fresh by using the `AcquireTokenSilent` function, which returns the cached token if it not expired, and refreshes the token if it is expired.</span></span>
- <span data-ttu-id="d3679-121">Ele implementa a `IAuthenticationProvider` interface para que o SDK do Graph possa usar a classe para autenticar chamadas de gráfico.</span><span class="sxs-lookup"><span data-stu-id="d3679-121">It implements the `IAuthenticationProvider` interface so that the Graph SDK can use the class to authenticate Graph calls.</span></span>

## <a name="sign-in-and-display-the-access-token"></a><span data-ttu-id="d3679-122">Entrar e exibir o token de acesso</span><span class="sxs-lookup"><span data-stu-id="d3679-122">Sign in and display the access token</span></span>

<span data-ttu-id="d3679-123">Nesta seção, você atualizará o aplicativo para chamar a `GetAccessToken` função, que entrará no usuário.</span><span class="sxs-lookup"><span data-stu-id="d3679-123">In this section you will update the application to call the `GetAccessToken` function, which will sign in the user.</span></span> <span data-ttu-id="d3679-124">Você também adicionará código para exibir o token.</span><span class="sxs-lookup"><span data-stu-id="d3679-124">You will also add code to display the token.</span></span>

1. <span data-ttu-id="d3679-125">Adicione a função a seguir à classe `Program`.</span><span class="sxs-lookup"><span data-stu-id="d3679-125">Add the following function to the `Program` class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="LoadAppSettingsSnippet":::

1. <span data-ttu-id="d3679-126">Adicione o seguinte código à `Main` função imediatamente após a `Console.WriteLine(".NET Core Graph Tutorial\n");` linha.</span><span class="sxs-lookup"><span data-stu-id="d3679-126">Add the following code to the `Main` function immediately after the `Console.WriteLine(".NET Core Graph Tutorial\n");` line.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="InitializationSnippet":::

1. <span data-ttu-id="d3679-127">Adicione o seguinte código à `Main` função imediatamente após a `// Display access token` linha.</span><span class="sxs-lookup"><span data-stu-id="d3679-127">Add the following code to the `Main` function immediately after the `// Display access token` line.</span></span>

    ```csharp
    Console.WriteLine($"Access token: {accessToken}\n");
    ```

1. <span data-ttu-id="d3679-128">Criar e executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d3679-128">Build and run the app.</span></span> <span data-ttu-id="d3679-129">O aplicativo exibe uma URL e um código de dispositivo.</span><span class="sxs-lookup"><span data-stu-id="d3679-129">The application displays a URL and device code.</span></span>

    ```Shell
    PS C:\Source\GraphTutorial> dotnet run
    .NET Core Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

    > [!TIP]
    > <span data-ttu-id="d3679-130">Se você encontrar erros, Compare seu **Program.cs** com o [exemplo no GitHub](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demo/GraphTutorial/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="d3679-130">If you encounter errors, compare your **Program.cs** with the [example on GitHub](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demo/GraphTutorial/Program.cs).</span></span>

1. <span data-ttu-id="d3679-131">Abra um navegador e navegue até a URL exibida.</span><span class="sxs-lookup"><span data-stu-id="d3679-131">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="d3679-132">Insira o código fornecido e entre.</span><span class="sxs-lookup"><span data-stu-id="d3679-132">Enter the provided code and sign in.</span></span> <span data-ttu-id="d3679-133">Depois de concluído, volte para o aplicativo e escolha o **1. Exibir** opção de token de acesso para exibir o token de acesso.</span><span class="sxs-lookup"><span data-stu-id="d3679-133">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>
